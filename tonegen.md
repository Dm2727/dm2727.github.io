<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>ToneGen 1.2</title>

<style>
body{
    font-family:"Helvetica World",Helvetica,Arial,sans-serif;
    background:#fff;
    color:#000;
    max-width:1200px;
    margin:auto;
    padding:20px;
}

h1{
    font-weight:400;
}

button,input,select{
    font-family:inherit;
    font-size:18px;
}

button{
    border:1px solid #000;
    background:#fff;
    padding:14px;
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

label{
    display:block;
    margin-bottom:6px;
}

input[type=number],
select{
    width:100%;
    padding:10px;
}

input[type=range]{
    width:100%;
}

.big{
    min-height:60px;
}
</style>
</head>
<body>

<h1>ToneGen 1.2</h1>

<button class="big" onclick="addTone()">
Add Tone
</button>

<button class="big" onclick="stopAll()">
Stop All
</button>

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

        this.frequency=frequency;
        this.waveform=waveform;
        this.volume=volume;

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

                data[i] =
                Math.random()*2-1;

            }

            const source =
            ctx.createBufferSource();

            source.buffer =
            buffer;

            source.loop =
            true;

            this.gain =
            ctx.createGain();

            this.gain.gain.value =
            this.volume;

            source.connect(
                this.gain
            );

            this.gain.connect(
                ctx.destination
            );

            source.start();

            this.source =
            source;

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

                this.osc.type =
                "triangle";

                this.osc.connect(
                    this.gain
                );

                break;

            case "bell":

                this.osc.type =
                "sine";

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
    }

}

function addTone(){

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
            min="0.1"
            max="30000"
            step="0.1"
            value="440">
        </div>

        <div class="field">
            <label>Waveform</label>

            <select class="wave">

                <option value="sine">
                    Sine
                </option>

                <option value="square">
                    Square
                </option>

                <option value="triangle">
                    Triangle
                </option>

                <option value="sawtooth">
                    Sawtooth
                </option>

                <option value="parabola">
                    Parabola
                </option>

                <option value="pulse">
                    Pulse
                </option>

                <option value="noise">
                    White Noise
                </option>

                <option value="organ">
                    Organ
                </option>

                <option value="bell">
                    Bell
                </option>

            </select>
        </div>

        <div class="field">
            <label>Volume</label>

            <input
            class="volume"
            type="range"
            min="0"
            max="100"
            value="20">
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

    let tone;

    card.querySelector(".start")
    .onclick = ()=>{

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
        tone=null;

    };

    card.querySelector(".remove")
    .onclick = ()=>{

        if(tone){

            tone.stop();
        }

        card.remove();
    };

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

addTone();

</script>

</body>
</html>
