<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>ToneGen 1.2.1</title>

<style>

body{
    font-family:Arial,sans-serif;
    max-width:1200px;
    margin:auto;
    padding:20px;
}

button{
    padding:10px;
    margin:4px;
    cursor:pointer;
}

.tone{
    border:2px solid #000;
    padding:16px;
    margin:16px 0;
}

.row{
    display:flex;
    flex-wrap:wrap;
    gap:12px;
}

.field{
    flex:1;
    min-width:180px;
}

input[type=number],
select{
    width:100%;
    padding:8px;
}

input[type=range]{
    width:100%;
}

</style>
</head>
<body>

<h1>ToneGen 1.2.1</h1>

<button onclick="addTone()">Add Tone</button>
<button onclick="stopAll()">Stop All</button>
<button onclick="savePreset()">Save Preset</button>
<input type="file" id="loadPresetFile">

<div id="tones"></div>

<script>

let audioContext;
let activeTones=[];

function getAudioContext(){

    if(!audioContext){

        audioContext =
        new (
            window.AudioContext ||
            window.webkitAudioContext
        )();
    }

    return audioContext;
}

function parabolaCurve(size=65536){

    const curve =
    new Float32Array(size);

    for(let i=0;i<size;i++){

        const x =
        (i/(size-1))*2-1;

        curve[i] =
        (2*x*x)-1;
    }

    return curve;
}

class Tone{

    constructor(
        frequency,
        waveform,
        volume
    ){

        this.frequency =
        frequency;

        this.waveform =
        waveform;

        this.volume =
        volume;
    }

    start(){

        const ctx =
        getAudioContext();

        if(this.waveform==="noise"){

            const buffer =
            ctx.createBuffer(
                1,
                ctx.sampleRate*2,
                ctx.sampleRate
            );

            const data =
            buffer.getChannelData(0);

            for(let i=0;i<data.length;i++){

                data[i]=
                Math.random()*2-1;
            }

            this.source =
            ctx.createBufferSource();

            this.source.buffer =
            buffer;

            this.source.loop =
            true;

            this.gain =
            ctx.createGain();

            this.gain.gain.value =
            this.volume;

            this.source.connect(
                this.gain
            );

            this.gain.connect(
                ctx.destination
            );

            this.source.start();

            return;
        }

        if(this.waveform==="bell"){

            const partials =
            [1,2.71,4.07,6.12];

            this.bellOscs=[];

            partials.forEach(
                (mult,index)=>{

                    const osc =
                    ctx.createOscillator();

                    const gain =
                    ctx.createGain();

                    osc.type =
                    "sine";

                    osc.frequency.value =
                    this.frequency *
                    mult;

                    gain.gain.setValueAtTime(
                        this.volume/
                        (index+1),
                        ctx.currentTime
                    );

                    gain.gain.exponentialRampToValueAtTime(
                        0.0001,
                        ctx.currentTime+3
                    );

                    osc.connect(gain);
                    gain.connect(
                        ctx.destination
                    );

                    osc.start();

                    osc.stop(
                        ctx.currentTime+3
                    );

                    this.bellOscs.push({
                        osc,
                        gain
                    });

                }
            );

            return;
        }

        this.osc =
        ctx.createOscillator();

        this.gain =
        ctx.createGain();

        this.osc.frequency.value =
        this.frequency;

        this.gain.gain.value =
        this.volume;

        switch(this.waveform){

            case "parabola":

                this.osc.type =
                "triangle";

                this.shaper =
                ctx.createWaveShaper();

                this.shaper.curve =
                parabolaCurve();

                this.osc.connect(
                    this.shaper
                );

                this.shaper.connect(
                    this.gain
                );

                break;

            case "pulse":

                this.osc.type =
                "square";

                this.osc.connect(
                    this.gain
                );

                break;

            case "organ":

                const organWave =
                ctx.createPeriodicWave(
                    new Float32Array([
                        0,
                        1,
                        0.8,
                        0.6,
                        0.4,
                        0.3,
                        0.2
                    ]),
                    new Float32Array(7)
                );

                this.osc.setPeriodicWave(
                    organWave
                );

                this.osc.connect(
                    this.gain
                );

                break;

            default:

                this.osc.type =
                this.waveform;

                this.osc.connect(
                    this.gain
                );

        }

        this.gain.connect(
            ctx.destination
        );

        this.osc.start();
    }

    stop(){

        try{
            this.osc?.stop();
        }catch{}

        try{
            this.source?.stop();
        }catch{}

        try{

            this.bellOscs?.forEach(
                p=>{

                    try{
                        p.osc.stop();
                    }catch{}

                    try{
                        p.osc.disconnect();
                    }catch{}

                    try{
                        p.gain.disconnect();
                    }catch{}
                }
            );

        }catch{}

        try{
            this.osc?.disconnect();
        }catch{}

        try{
            this.source?.disconnect();
        }catch{}

        try{
            this.gain?.disconnect();
        }catch{}

        try{
            this.shaper?.disconnect();
        }catch{}
    }
}

function createToneCardData(card){

    return {

        frequency:
        parseFloat(
            card.querySelector(".freq").value
        ),

        waveform:
        card.querySelector(".wave").value,

        volume:
        card.querySelector(".volume").value
    };
}

function addTone(data={}){

    const card =
    document.createElement("div");

    card.className =
    "tone";

    card.innerHTML = `

    <div class="row">

        <div class="field">
            <label>Frequency (Hz)</label>
            <input
            class="freq"
            type="number"
            min="20"
            max="20000"
            step="0.1"
            value="${data.frequency || 440}">
        </div>

        <div class="field">
            <label>Waveform</label>

            <select class="wave">
                <option value="sine">Sine</option>
                <option value="square">Square</option>
                <option value="triangle">Triangle</option>
                <option value="sawtooth">Sawtooth</option>
                <option value="parabola">Parabola</option>
                <option value="pulse">Pulse</option>
                <option value="noise">White Noise</option>
                <option value="organ">Organ</option>
                <option value="bell">Bell</option>
            </select>
        </div>

        <div class="field">
            <label>Volume</label>

            <input
            class="volume"
            type="range"
            min="0"
            max="100"
            value="${data.volume || 20}">
        </div>

    </div>

    <div class="row">

        <button class="start">
            Start
        </button>

        <button class="stop">
            Stop
        </button>

        <button class="remove">
            Remove
        </button>

    </div>
    `;

    card.querySelector(".wave").value =
    data.waveform || "sine";

    let tone=null;

    card.querySelector(".start")
    .onclick = async()=>{

        await getAudioContext().resume();

        if(tone)return;

        tone =
        new Tone(
            parseFloat(
                card.querySelector(".freq").value
            ),
            card.querySelector(".wave").value,
            card.querySelector(".volume").value/100
        );

        tone.start();

        activeTones.push(tone);
    };

    card.querySelector(".stop")
    .onclick = ()=>{

        if(!tone)return;

        tone.stop();

        activeTones =
        activeTones.filter(
            t=>t!==tone
        );

        tone=null;
    };

    card.querySelector(".remove")
    .onclick = ()=>{

        if(tone){

            tone.stop();

            activeTones =
            activeTones.filter(
                t=>t!==tone
            );
        }

        card.remove();
    };

    card.querySelector(".freq")
    .addEventListener("input",()=>{

        if(
            tone &&
            tone.osc
        ){

            tone.osc.frequency.value =
            parseFloat(
                card.querySelector(".freq").value
            );
        }
    });

    card.querySelector(".volume")
    .addEventListener("input",()=>{

        if(
            tone &&
            tone.gain
        ){

            tone.gain.gain.value =
            card.querySelector(".volume").value/100;
        }
    });

    document
    .getElementById("tones")
    .appendChild(card);
}

function stopAll(){

    activeTones.forEach(
        tone=>tone.stop()
    );

    activeTones=[];
}

function savePreset(){

    const cards =
    document.querySelectorAll(".tone");

    const preset =
    [];

    cards.forEach(card=>{

        preset.push(
            createToneCardData(card)
        );
    });

    const blob =
    new Blob(
        [
            JSON.stringify(
                preset,
                null,
                2
            )
        ],
        {
            type:
            "application/json"
        }
    );

    const a =
    document.createElement("a");

    a.href =
    URL.createObjectURL(blob);

    a.download =
    "ToneGenPreset.json";

    a.click();
}

document
.getElementById("loadPresetFile")
.addEventListener(
    "change",
    e=>{

        const file =
        e.target.files[0];

        if(!file)return;

        const reader =
        new FileReader();

        reader.onload =
        ()=>{

            document
            .getElementById("tones")
            .innerHTML="";

            const preset =
            JSON.parse(
                reader.result
            );

            preset.forEach(
                addTone
            );
        };

        reader.readAsText(file);
    }
);

addTone();

</script>

</body>
</html>
