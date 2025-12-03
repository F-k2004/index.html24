<!DOCTYPE html>
<html lang="fa">
<head>
<meta charset="UTF-8">
<title>⚡ Neon Electric Field</title>
<meta name="viewport" content="width=device-width, initial-scale=1">
<style>
  body {
    margin: 0;
    overflow: hidden;
    background: #000014;
    font-family: sans-serif;
  }
  canvas {
    display: block;
    filter: blur(1.5px) brightness(1.4);
  }
</style>
</head>

<body>
<canvas id="c"></canvas>

<script>
const canvas = document.getElementById("c");
const ctx = canvas.getContext("2d");

let w, h;
function resize() {
  w = canvas.width = window.innerWidth;
  h = canvas.height = window.innerHeight;
}
window.addEventListener("resize", resize);
resize();

const particles = [];
const maxParticles = 230;
let mouse = { x: w / 2, y: h / 2 };

// تولید ذرات الکتریکی
for (let i = 0; i < maxParticles; i++) {
  particles.push({
    x: Math.random() * w,
    y: Math.random() * h,
    vx: (Math.random() - 0.5) * 2,
    vy: (Math.random() - 0.5) * 2,
    size: 1 + Math.random() * 3,
  });
}

// حرکت موس = جذب الکتریکی
canvas.addEventListener("mousemove", (e) => {
  mouse.x = e.clientX;
  mouse.y = e.clientY;
});

// رسم ذرات و خطوط
function draw() {
  ctx.clearRect(0, 0, w, h);

  for (let p of particles) {
    // کشش ذرات به سمت موس
    let dx = mouse.x - p.x;
    let dy = mouse.y - p.y;
    let dist = Math.hypot(dx, dy);

    if (dist < 200) {
      p.vx += dx / 2000;
      p.vy += dy / 2000;
    }

    // حرکت طبیعی
    p.x += p.vx;
    p.y += p.vy;

    // برگشت از دیوارها
    if (p.x < 0 || p.x > w) p.vx *= -1;
    if (p.y < 0 || p.y > h) p.vy *= -1;

    // نور نئونی ذره
    ctx.beginPath();
    ctx.fillStyle = `hsla(${(p.x + p.y) % 360}, 100%, 70%, 0.9)`;
    ctx.arc(p.x, p.y, p.size, 0, Math.PI * 2);
    ctx.fill();
  }

  // خطوط الکتریکی بین ذرات
  for (let i = 0; i < particles.length; i++) {
    for (let j = i + 1; j < particles.length; j++) {
      let p1 = particles[i];
      let p2 = particles[j];

      let dx = p1.x - p2.x;
      let dy = p1.y - p2.y;
      let dist = Math.hypot(dx, dy);

      if (dist < 150) {
        let alpha = 1 - dist / 150;

        ctx.strokeStyle = `hsla(${(p1.x + p2.y) % 360}, 100%, 60%, ${alpha})`;
        ctx.lineWidth = 1;

        ctx.beginPath();
        ctx.moveTo(p1.x, p1.y);
        ctx.lineTo(p2.x, p2.y);
        ctx.stroke();
      }
    }
  }
}

function animate() {
  draw();
  requestAnimationFrame(animate);
}

animate();
</script>
</body>
</html>
