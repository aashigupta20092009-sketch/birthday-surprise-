<!DOCTYPE html><html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Birthday Surprise 🎂</title><style>
* {
    box-sizing: border-box;
}

body {
    margin: 0;
    min-height: 100vh;
    font-family: Arial, sans-serif;
    text-align: center;
    color: white;
    background: linear-gradient(135deg, #17002b, #67005e, #ff4f9a);
    overflow-x: hidden;
}

.container {
    padding: 25px 15px 50px;
    position: relative;
    z-index: 2;
}

h1 {
    font-size: clamp(30px, 8vw, 48px);
    color: #ffe082;
    text-shadow: 0 0 20px #ffb6e6;
    animation: glow 2s infinite alternate;
}

@keyframes glow {
    from { text-shadow: 0 0 8px pink; }
    to { text-shadow: 0 0 28px #fff, 0 0 40px hotpink; }
}

.subtitle {
    font-size: 18px;
}

.cake {
    margin: 35px auto 10px;
    width: 190px;
    height: 145px;
    position: relative;
    animation: bounce 2s infinite ease-in-out;
}

@keyframes bounce {
    0%, 100% { transform: translateY(0); }
    50% { transform: translateY(-10px); }
}

.plate {
    position: absolute;
    bottom: 0;
    left: 0;
    width: 190px;
    height: 15px;
    background: #e0e0e0;
    border-radius: 50%;
}

.cake-bottom {
    position: absolute;
    bottom: 12px;
    left: 15px;
    width: 160px;
    height: 65px;
    background: linear-gradient(#ff85b3, #e92e76);
    border-radius: 8px 8px 15px 15px;
}

.cake-top {
    position: absolute;
    bottom: 65px;
    left: 15px;
    width: 160px;
    height: 25px;
    background: #fff0f5;
    border-radius: 50%;
}

.candle {
    position: absolute;
    bottom: 88px;
    left: 91px;
    width: 10px;
    height: 38px;
    background: repeating-linear-gradient(
        45deg, #fff 0 5px, #ff4d8d 5px 10px
    );
    border-radius: 3px;
}

.flame {
    position: absolute;
    bottom: 125px;
    left: 90px;
    width: 13px;
    height: 20px;
    background: orange;
    border-radius: 50% 50% 50% 0;
    transform: rotate(-45deg);
    box-shadow: 0 0 18px yellow;
    animation: flicker .4s infinite alternate;
}

@keyframes flicker {
    from { transform: rotate(-45deg) scale(1); }
    to { transform: rotate(-40deg) scale(.8); }
}

button {
    border: none;
    padding: 13px 22px;
    margin: 8px;
    border-radius: 30px;
    background: linear-gradient(45deg, #ffca6a, #ff70b5);
    color: #43002c;
    font-size: 16px;
    font-weight: bold;
    cursor: pointer;
    box-shadow: 0 0 15px #ff8fce;
}

button:active {
    transform: scale(.95);
}

.gift {
    font-size: 85px;
    margin: 12px;
    cursor: pointer;
    display: inline-block;
    animation: wiggle 1.2s infinite;
}

@keyframes wiggle {
    0%, 100% { transform: rotate(0); }
    25% { transform: rotate(-8deg); }
    75% { transform: rotate(8deg); }
}

#message {
    display: none;
    max-width: 500px;
    margin: 20px auto;
    padding: 20px;
    background: rgba(255,255,255,.13);
    border: 1px solid rgba(255,255,255,.4);
    border-radius: 20px;
    line-height: 1.8;
    animation: appear 1s ease;
}

@keyframes appear {
    from { opacity: 0; transform: scale(.6); }
    to { opacity: 1; transform: scale(1); }
}

.balloon {
    position: fixed;
    bottom: -100px;
    font-size: 38px;
    z-index: 1;
    pointer-events: none;
    animation: floatUp linear forwards;
}

@keyframes floatUp {
    to {
        transform: translateY(-120vh) rotate(25deg);
        opacity: .2;
    }
}

#confetti {
    position: fixed;
    inset: 0;
    width: 100%;
    height: 100%;
    pointer-events: none;
    z-index: 5;
}

footer {
    margin-top: 30px;
    font-size: 13px;
    opacity: .8;
}
</style></head><body><canvas id="confetti"></canvas>

<div class="container"><h1>🎉 Happy Birthday! 🎉</h1>

<p class="subtitle">
    Someone special deserves a magical surprise! 💖
</p>

<div class="cake">
    <div class="plate"></div>
    <div class="cake-bottom"></div>
    <div class="cake-top"></div>
    <div class="candle"></div>
    <div class="flame" id="flame"></div>
</div>

<h2>🎂 Make a Wish! 🎂</h2>

<button onclick="blowCandle()">🕯️ Blow Candle</button>

<p>👇 Tap the gift to open your surprise 👇</p>

<div class="gift" onclick="openGift()">🎁</div>

<div>
    <button onclick="celebrate()">🎊 Celebrate!</button>
    <button onclick="toggleMusic()" id="musicButton">
        🎵 Play Music
    </button>
</div>

<div id="message">
    <h2>💖 A Special Birthday Wish 💖</h2>

    <p>
        May your life always be filled with happiness,
        love, laughter and beautiful memories! ✨
    </p>

    <p>
        May every dream come true and every new day
        bring you a reason to smile. 🌸
    </p>

    <h2>🎂 Happy Birthday! 🎂</h2>
    <p>You are truly special! ❤️</p>
</div>

<footer>
    Made with ❤️ and lots of birthday magic ✨
</footer>

</div><script>
let giftOpened = false;
let musicOn = false;
let audioContext;
let musicTimer;

function openGift() {
    document.getElementById("message").style.display = "block";

    if (!giftOpened) {
        giftOpened = true;
        celebrate();
        document.querySelector(".gift").textContent = "🎉";
    }
}

function blowCandle() {
    document.getElementById("flame").style.display = "none";

    alert("✨ Make a wish! May it come true! 💖");
}

function createBalloon() {
    const balloon = document.createElement("div");
    balloon.className = "balloon";

    const balloons = ["🎈", "🎈", "💖", "🎈", "✨"];
    balloon.textContent =
        balloons[Math.floor(Math.random() * balloons.length)];

    balloon.style.left = Math.random() * 95 + "vw";
    balloon.style.animationDuration = (4 + Math.random() * 4) + "s";

    document.body.appendChild(balloon);

    setTimeout(() => balloon.remove(), 8500);
}

function celebrate() {
    for (let i = 0; i < 30; i++) {
        setTimeout(createBalloon, i * 100);
    }

    startConfetti();
}

/* Confetti animation */
const canvas = document.getElementById("confetti");
const ctx = canvas.getContext("2d");
let pieces = [];
let confettiRunning = false;

function resizeCanvas() {
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;
}

window.addEventListener("resize", resizeCanvas);
resizeCanvas();

function startConfetti() {
    for (let i = 0; i < 140; i++) {
        pieces.push({
            x: Math.random() * canvas.width,
            y: -Math.random() * canvas.height,
            size: 3 + Math.random() * 6,
            speed: 2 + Math.random() * 4,
            color: `hsl(${Math.random() * 360},100%,65%)`
        });
    }

    if (!confettiRunning) {
        confettiRunning = true;
        requestAnimationFrame(drawConfetti);
    }
}

function drawConfetti() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);

    pieces.forEach(p => {
        ctx.fillStyle = p.color;
        ctx.fillRect(p.x, p.y, p.size, p.size * 1.5);

        p.y += p.speed;
        p.x += Math.sin(p.y / 20);

        if (p.y > canvas.height) {
            p.y = -10;
            p.x = Math.random() * canvas.width;
        }
    });

    if (pieces.length > 0) {
        requestAnimationFrame(drawConfetti);
    } else {
        confettiRunning = false;
    }
}

/* Simple original birthday tune using the phone speaker */
function toggleMusic() {
    const button = document.getElementById("musicButton");

    if (musicOn) {
        musicOn = false;
        clearTimeout(musicTimer);

        if (audioContext) {
            audioContext.close();
            audioContext = null;
        }

        button.textContent = "🎵 Play Music";
        return;
    }

    const AudioCtx = window.AudioContext || window.webkitAudioContext;

    if (!AudioCtx) {
        alert("Audio is not supported in this browser.");
        return;
    }

    audioContext = new AudioCtx();
    musicOn = true;
    button.textContent = "⏹️ Stop Music";

    const notes = [523, 523, 587, 523, 698, 659, 523, 523, 587, 523, 784, 698];
    let i = 0;

    function playNote() {
        if (!musicOn || !audioContext) return;

        const oscillator = audioContext.createOscillator();
        const gain = audioContext.createGain();

        oscillator.type = "sine";
        oscillator.frequency.value = notes[i % notes.length];

        gain.gain.setValueAtTime(0.08, audioContext.currentTime);
        gain.gain.exponentialRampToValueAtTime(
            0.001, audioContext.currentTime + 0.3
        );

        oscillator.connect(gain);
        gain.connect(audioContext.destination);

        oscillator.start();
        oscillator.stop(audioContext.currentTime + 0.3);

        i++;
        musicTimer = setTimeout(playNote, 380);
    }

    playNote();
}
</script></body>
</html># birthday-surprise-