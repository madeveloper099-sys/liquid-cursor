# liquid-cursor
Interactive liquid cursor animation built with HTML, CSS and JavaScript.

<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Liquid Cursor | Creative Experience</title>

<style>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    cursor: none;
}

body {
    min-height: 100vh;
    background: #080808;
    color: white;
    font-family: Arial, sans-serif;
    overflow: hidden;
}

body::before {
    content: "";
    position: fixed;
    width: 450px;
    height: 450px;
    background: #ff6a1510;
    filter: blur(100px);
    top: 20%;
    left: 35%;
    pointer-events: none;
}

.container {
    min-height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    text-align: center;
    padding: 25px;
}

.content {
    position: relative;
    z-index: 2;
}

.tag {
    color: #ff8a36;
    letter-spacing: 5px;
    font-size: 11px;
    margin-bottom: 25px;
}

h1 {
    font-size: clamp(45px, 9vw, 100px);
    font-weight: 900;
    letter-spacing: -5px;
    background: linear-gradient(
        120deg, #fff, #ff7a18, #a855f7
    );
    background-clip: text;
    -webkit-background-clip: text;
    color: transparent;
}

.description {
    color: #888;
    font-size: 14px;
    letter-spacing: 2px;
    margin-top: 20px;
    line-height: 1.8;
}

.hint {
    margin-top: 45px;
    color: #ff8a36;
    font-size: 11px;
    letter-spacing: 3px;
    animation: pulse 2s infinite;
}

.cursor {
    position: fixed;
    top: 0;
    left: 0;
    width: 18px;
    height: 18px;
    background: #ff7a18;
    border-radius: 50%;
    pointer-events: none;
    z-index: 1000;
    transform: translate(-50%, -50%);
    box-shadow: 0 0 20px #ff7a18,
                0 0 45px #ff7a18;
}

.trail {
    position: fixed;
    width: 9px;
    height: 9px;
    border-radius: 50%;
    background: #b45cff;
    pointer-events: none;
    z-index: 999;
    box-shadow: 0 0 15px #b45cff;
    transform: translate(-50%, -50%);
}

.particle {
    position: fixed;
    width: 7px;
    height: 7px;
    border-radius: 50%;
    pointer-events: none;
    z-index: 998;
    animation: explode 700ms ease-out forwards;
}

@keyframes explode {
    to {
        transform: translate(var(--x), var(--y)) scale(0);
        opacity: 0;
    }
}

@keyframes pulse {
    50% { opacity: .4; }
}

@media (max-width: 600px) {
    h1 {
        letter-spacing: -2px;
    }

    .description {
        font-size: 11px;
    }
}

@media (hover: none) {
    * {
        cursor: auto;
    }

    .cursor, .trail {
        display: none;
    }
}
</style>
</head>

<body>

<div class="cursor" id="cursor"></div>

<main class="container">
    <div class="content">
        <div class="tag">INTERACTIVE EXPERIENCE / 001</div>

        <h1>LIQUID<br>MOTION.</h1>

        <p class="description">
            A DIFFERENT WAY TO EXPERIENCE<br>
            DIGITAL INTERACTION.
        </p>

        <div class="hint">
            MOVE YOUR MOUSE • CLICK ANYWHERE
        </div>
    </div>
</main>

<script>
const cursor = document.getElementById("cursor");

let mouseX = window.innerWidth / 2;
let mouseY = window.innerHeight / 2;

let cursorX = mouseX;
let cursorY = mouseY;

const trails = [];

for (let i = 0; i < 12; i++) {
    const dot = document.createElement("div");
    dot.className = "trail";
    document.body.appendChild(dot);

    trails.push({
        element: dot,
        x: mouseX,
        y: mouseY
    });
}

window.addEventListener("mousemove", (e) => {
    mouseX = e.clientX;
    mouseY = e.clientY;
});

function animate() {
    cursorX += (mouseX - cursorX) * 0.25;
    cursorY += (mouseY - cursorY) * 0.25;

    cursor.style.left = cursorX + "px";
    cursor.style.top = cursorY + "px";

    let previousX = cursorX;
    let previousY = cursorY;

    trails.forEach((trail, index) => {
        trail.x += (previousX - trail.x) * 0.3;
        trail.y += (previousY - trail.y) * 0.3;

        trail.element.style.left = trail.x + "px";
        trail.element.style.top = trail.y + "px";

        const size = 10 - index * 0.55;
        trail.element.style.width = size + "px";
        trail.element.style.height = size + "px";
        
        trail.element.style.opacity =
            1 - index / trails.length;

        previousX = trail.x;
        previousY = trail.y;
    });

    requestAnimationFrame(animate);
}

animate();

window.addEventListener("click", (e) => {
    for (let i = 0; i < 18; i++) {
        const particle = document.createElement("div");

        particle.className = "particle";

        const angle = Math.random() * Math.PI * 2;
        const distance = 40 + Math.random() * 100;

        const x = Math.cos(angle) * distance;
        const y = Math.sin(angle) * distance;

        particle.style.left = e.clientX + "px";
        particle.style.top = e.clientY + "px";

        particle.style.background =
            Math.random() > 0.5 ? "#ff7a18" : "#b45cff";

        particle.style.setProperty("--x", x + "px");
        particle.style.setProperty("--y", y + "px");

        document.body.appendChild(particle);

        particle.addEventListener("animationend", () => {
            particle.remove();
        });
    }
});
</script>

</body>
</html>
