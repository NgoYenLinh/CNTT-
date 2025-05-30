
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>hong biết nữa</title>
  <style>
    html, body {
      margin: 0;
      padding: 0;
      overflow: hidden;
      background: rgb(7, 2, 0);
    }
    canvas {
      display: block;
    }
  </style>
</head>
<body>
  <canvas id="galaxy"></canvas>
  <script>
    const canvas = document.getElementById('galaxy');
    const ctx = canvas.getContext('2d');

    function resizeCanvas() {
      canvas.width = window.innerWidth;
      canvas.height = window.innerHeight;
    }

    window.addEventListener('resize', resizeCanvas);
    resizeCanvas();

    const particles = [];
    const NUM_PARTICLES = 5000;
    const getCenter = () => ({
      x: canvas.width / 2,
      y: canvas.height / 2
    });

    for (let i = 0; i < NUM_PARTICLES; i++) {
      const angle = Math.random() * 2 * Math.PI;
      const radius = Math.pow(Math.random(), 0.5) * 450;
      const speed = 0.001 + Math.random() * 0.002;
      particles.push({
        angle,
        radius,
        speed,
        color: `hsl(${angle * 180 / Math.PI}, 100%, 85%)`
      });
    }

    function drawCenterPlanet() {
      const { x, y } = getCenter();
      const gradient = ctx.createRadialGradient(x, y, 0, x, y, 120);
      gradient.addColorStop(0, 'rgba(255, 255, 200, 1)');
      gradient.addColorStop(0.4, 'rgba(200, 150, 255, 0.6)');
      gradient.addColorStop(1, 'rgba(0, 0, 0, 0)');
      ctx.beginPath();
      ctx.fillStyle = gradient;
      ctx.arc(x, y, 120, 0, 2 * Math.PI);
      ctx.fill();
    }

    function animate() {
      ctx.fillStyle = 'rgba(0, 0, 0, 0.1)';
      ctx.fillRect(0, 0, canvas.width, canvas.height);

      drawCenterPlanet();

      const { x: cx, y: cy } = getCenter();

      particles.forEach(p => {
        p.angle += p.speed;
        const x = cx + Math.cos(p.angle) * p.radius;
        const y = cy + Math.sin(p.angle) * p.radius * 0.3;

        ctx.beginPath();
        ctx.fillStyle = p.color;
        ctx.arc(x, y, 1.1, 0, 2 * Math.PI);
        ctx.fill();
      });

      requestAnimationFrame (animate);
      }
      animate();
  </script>
</body>
</html>
