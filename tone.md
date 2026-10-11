<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Dm2727's tone generator and keypad 1.1</title>

<style>
body{
    font-family:"Helvetica World",Helvetica,Arial,sans-serif;
    background:#fff;
    color:#000;
    margin:20px auto;
    max-width:900px;
    padding:20px;
}

h1,h2{
    font-weight:400;
}

button,input,select{
    font-family:inherit;
}

button{
    background:#fff;
    color:#000;
    border:1px solid #000;
    padding:10px;
    cursor:pointer;
}

input,select{
    border:1px solid #000;
    background:#fff;
    color:#000;
    padding:8px;
}

.tone{
    border:1px solid #000;
    padding:12px;
    margin-bottom:12px;
}

.row{
    display:flex;
    flex-wrap:wrap;
    gap:10px;
}

.row>*{
    flex:1;
}

#display{
    width:100%;
    margin-bottom:10px;
    text-align:center;
    font-size:24px;
}

.keypad{
    display:grid;
    grid-template-columns:repeat(3,1fr);
    gap:8px;
    max-width:340px;
}

.keypad button{
    height:70px;
    font-size:24px;
}

.controls{
    display:grid;
    grid-template-columns:1fr 1fr;
    gap:8px;
    margin-top:8px;
    max-width:340px;
}

.call{
    background:#000;
    color:#fff;
}

.status{
    margin-top:15px;
}
</style>
</head>
<body>

<h1>Multi-Tone Generator</h1>

<button onclick="addTone()">Add Tone</button>

<div id="tones"></div>

<hr>

<h2>Fake Telephone Dialler</h2>

<input
id="display"
readonly
placeholder="Enter number">

<div class="keypad">
<button onclick="pressKey('1')">1</button>
<button onclick="pressKey('2')">2</button>
<button onclick="pressKey('3')">3</button>

<button onclick="pressKey('4')">4</button>
<button onclick="pressKey('5')">5</button>
<button onclick="pressKey('6')">6</button>

<button onclick="pressKey('7')">7</button>
<button onclick="pressKey('8')">8</button>
<button onclick="pressKey('9')">9</button>

<button onclick="pressKey('+')">+</button>
<button onclick="pressKey('0')">0</button>
<button onclick="pressKey('#')">#</button>
</div>

<div class="controls">
<button class="call" onclick="fakeCall()">
Call
</button>

<button onclick="hangUp()">
End
</button>

<button onclick="backspace()">
Backspace
</button>

<button onclick="clearDisplay()">
Clear
</button>
</div>

<div class="status" id="status">
Ready
</div>

<script>

let audioContext;
let activeTones = [];
let callActive = false;

function getContext() {
    if (!audioContext) {
        audioContext = new (
            window.AudioContext ||
            window.webkitAudioContext
        )();
    }
    return audioContext;
}

function sleep(ms) {
    return new Promise(resolve =>
        setTimeout(resolve, ms)
    );
}

function createParabolaCurve(size = 65536) {

    const curve = new Float32Array(size);

    for (let i = 0; i < size; i++) {

        const x =
            (i / (size - 1)) * 2 - 1;

        curve[i] =
            (2 * x * x) - 1;
    }

    return curve;
}

class Tone {

    constructor(
        frequency,
        waveform,
        volume
    ) {

        this.frequency = frequency;
        this.waveform = waveform;
        this.volume = volume;

        this.osc = null;
        this.gain = null;
        this.shaper = null;
    }

    start() {

        if (this.osc) return;

        const ctx = getContext();

        this.osc =
            ctx.createOscillator();

        this.gain =
            ctx.createGain();

        this.osc.frequency.value =
            this.frequency;

        this.gain.gain.value =
            this.volume;

        if (
            this.waveform ===
            "parabola"
        ) {

            this.osc.type =
                "triangle";

            this.shaper =
                ctx.createWaveShaper();

            this.shaper.curve =
                createParabolaCurve();

            this.osc.connect(
                this.shaper
            );

            this.shaper.connect(
                this.gain
            );

        } else {

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

    stop() {

        if (!this.osc) return;

        try {
            this.osc.stop();
        } catch {}

        this.osc.disconnect();

        if (this.shaper) {
            this.shaper.disconnect();
        }

        this.gain.disconnect();

        this.osc = null;
    }
}

function createTone(
    frequency,
    waveform = "sine",
    volume = 0.1
) {

    const tone =
        new Tone(
            frequency,
            waveform,
            volume
        );

    activeTones.push(tone);

    return tone;
}

function stopAllAudio() {

    activeTones.forEach(t => {

        try {
            t.stop();
        } catch {}

    });

    activeTones = [];
}

function addTone() {

    const card
