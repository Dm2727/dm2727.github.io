<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Multi-Tone Generator</title>

<style>
* {
    box-sizing: border-box;
}

body {
    font-family:
        "Helvetica World",
        Helvetica,
        Arial,
        sans-serif;
    background: white;
    color: black;
    margin: 20px;
}

h1 {
    margin-bottom: 20px;
    font-weight: normal;
}

button,
input,
select {
    font-family: inherit;
}

#addTone {
    padding: 10px 16px;
    border: 1px solid black;
    background: white;
    cursor: pointer;
}

.tone {
    border: 1px solid black;
    padding: 15px;
    margin-top: 15px;
}

.row {
    display: flex;
    gap: 12px;
    flex-wrap: wrap;
    margin-bottom: 10px;
}

.field {
    flex: 1;
    min-width: 180px;
}

label {
    display: block;
    margin-bottom: 4px;
}

input,
select {
    width: 100%;
    padding: 6px;
    border: 1px solid black;
    background: white;
    color: black;
}

.actions {
    display: flex;
    gap: 8px;
    margin-top: 10px;
}

.actions button {
    padding: 8px 14px;
    border: 1px solid black;
    background: white;
    cursor: pointer;
}

.readout {
    margin-top: 4px;
    font-size: 13px;
}
</style>
</head>
<body>

<h1>Multi-Tone Generator</h1>

<button id="addTone">Add Tone</button>

<div id="tones"></div>

<script>
let audioContext;
let toneId = 0;

function getAudioContext() {
    if (!audioContext) {
        audioContext = new (window.AudioContext || window.webkitAudioContext)();
    }
    return audioContext;
}

function createParabolaCurve(size = 65536) {
    const curve = new Float32Array(size);

    for (let i = 0; i < size; i++) {
        const x = (i / (size - 1)) * 2 - 1;
        curve[i] = 2 * (x * x) - 1;
    }

    return curve;
}

function createToneCard() {
    toneId++;

    const div = document.createElement("div");
    div.className = "tone";

    div.innerHTML = `
        <div class="row">

            <div class="field">
                <label>Frequency (Hz)</label>
                <input
                    type="number"
                    min="0.1"
                    max="30000"
                    step="0.1"
                    value="440"
                    class="freq">
                <div class="readout"></div>
            </div>

            <div class="field">
                <label>Waveform</label>
                <select class="waveform">
                    <option value="sine">Sine</option>
                    <option value="square">Square</option>
                    <option value="triangle">Triangle</option>
                    <option value="sawtooth">Saw</option>
                    <option value="parabola">Parabola</option>
                </select>
            </div>

            <div class="field">
                <label>Volume (%)</label>
                <input
                    type="range"
                    min="0"
                    max="100"
                    value="20"
                    class="volume">
            </div>

        </div>

        <div class="actions">
            <button class="start">Start</button>
            <button class="stop">Stop</button>
            <button class="delete">Delete</button>
        </div>
    `;

    const freqInput = div.querySelector(".freq");
    const waveformInput = div.querySelector(".waveform");
    const volumeInput = div.querySelector(".volume");
    const readout = div.querySelector(".readout");

    const startBtn = div.querySelector(".start");
    const stopBtn = div.querySelector(".stop");
    const deleteBtn = div.querySelector(".delete");

    let oscillator = null;
    let gainNode = null;
    let waveShaper = null;

    function updateReadout() {
        const f = parseFloat(freqInput.value);

        if (f >= 1000) {
            readout.textContent =
                (f / 1000).toFixed(3) + " kHz";
        } else {
            readout.textContent =
                f.toFixed(2) + " Hz";
        }
    }

    updateReadout();

    freqInput.addEventListener("input", () => {
        updateReadout();

        if (oscillator) {
            oscillator.frequency.setValueAtTime(
                parseFloat(freqInput.value),
                audioContext.currentTime
            );
        }
    });

    volumeInput.addEventListener("input", () => {
        if (gainNode) {
            gainNode.gain.setValueAtTime(
                volumeInput.value / 100,
                audioContext.currentTime
            );
        }
    });

    waveformInput.addEventListener("change", () => {
        if (!oscillator) return;

        const mode = waveformInput.value;

        if (mode === "parabola") {
            oscillator.type = "triangle";
        } else {
            oscillator.type = mode;
        }
    });

    startBtn.addEventListener("click", () => {

        if (oscillator) return;

        const ctx = getAudioContext();

        oscillator = ctx.createOscillator();
        gainNode = ctx.createGain();

        oscillator.frequency.value =
            parseFloat(freqInput.value);

        gainNode.gain.value =
            volumeInput.value / 100;

        if (waveformInput.value === "parabola") {

            oscillator.type = "triangle";

            waveShaper = ctx.createWaveShaper();
            waveShaper.curve = createParabolaCurve();

            oscillator.connect(waveShaper);
            waveShaper.connect(gainNode);

        } else {

            oscillator.type =
                waveformInput.value;

            oscillator.connect(gainNode);
        }

        gainNode.connect(ctx.destination);

        oscillator.start();
    });

    stopBtn.addEventListener("click", () => {

        if (!oscillator) return;

        oscillator.stop();

        oscillator.disconnect();

        if (waveShaper) {
            waveShaper.disconnect();
        }

        gainNode.disconnect();

        oscillator = null;
        gainNode = null;
        waveShaper = null;
    });

    deleteBtn.addEventListener("click", () => {

        if (oscillator) {
            oscillator.stop();
        }

        div.remove();
    });

    document.getElementById("tones")
        .appendChild(div);
}

document
    .getElementById("addTone")
    .addEventListener("click", createToneCard);

createToneCard();
</script>

</body>
</html>
