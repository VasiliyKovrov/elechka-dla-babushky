# elechka-dla-babushky
 ЭТО ЕЛКА ДЛЯ ТВОЕЙ БАБУШКИ ОТ ЧЕЛОВЕКА С ИМЕНЕМ ВАСЯ
index.HTML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Новогодняя Елка</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <header>
        <p>С новым 2026 годом</p>
    </header>
    <main>
        <div id="tree-conteiner"></div>
    </main>
    <footer>
        <p>От Васи</p>
        <p>Бабушке</p>
    </footer>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r79/three.min.js"></script>
    <script src="tree.js"></script>
</body>
</html>

tree.js
const container = document.getElementById('tree-conteiner');
const WIDTH = container.clientWidth;
const HEIGHT = container.clientHeight;

const scene = new THREE.Scene();

const camera = new THREE.PerspectiveCamera(
  45,              
  WIDTH / HEIGHT,  
  0.1,             
  1000,            
);
camera.position.set(0, 3, 8);
camera.lookAt(new THREE.Vector3(0, 2, 0));

const renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true });
renderer.setSize(WIDTH, HEIGHT);
renderer.setPixelRatio(window.devicePixelRatio || 1);
container.appendChild(renderer.domElement);

const ambient = new THREE.AmbientLight(0xffffff, 0.4);
scene.add(ambient);
const dirLight = new THREE.DirectionalLight(0xffffff, 0.8);
dirLight.position.set(5, 10, 7);
scene.add(dirLight);

const floorGeometry = new THREE.CircleGeometry(4, 32);
const floorMaterial = new THREE.MeshPhongMaterial({ color: 0x222222 });
const floor = new THREE.Mesh(floorGeometry, floorMaterial);
floor.rotation.x = -Math.PI / 2;
floor.position.y = 0;
scene.add(floor);

const tree = new THREE.Group();

const coneMaterial = new THREE.MeshPhongMaterial({ color: 0x0d8f2d, shininess: 40 });

const coneLevels = [
  { radius: 2.2, height: 2.4, y: 1.0 },
  { radius: 1.7, height: 2.0, y: 2.5 },
  { radius: 1.2, height: 1.6, y: 3.7 }
];

coneLevels.forEach(level => {
  const geo = new THREE.ConeGeometry(level.radius, level.height, 32);
  const mesh = new THREE.Mesh(geo, coneMaterial);
  mesh.position.y = level.y;
  tree.add(mesh);
});

const starGeo = new THREE.OctahedronGeometry(0.3);
const starMat = new THREE.MeshPhongMaterial({ color: 0xffee55, emissive: 0xffdd33 });
const star = new THREE.Mesh(starGeo, starMat);
star.position.y = coneLevels[coneLevels.length - 1].y + 1.1;
tree.add(star);

tree.position.y = 0;
scene.add(tree);

let lastTime = 0;
function animate(time) {
  requestAnimationFrame(animate);

  const delta = (time - lastTime) / 1000;
  lastTime = time;

  tree.rotation.y += delta * 0.4;
  star.rotation.y -= delta * 0.8;

  renderer.render(scene, camera);
}
requestAnimationFrame(animate);

window.addEventListener('resize', () => {
  const newWidth = container.clientWidth;
  const newHeight = container.clientHeight;
  if (newWidth === 0 || newHeight === 0) return;

  renderer.setSize(newWidth, newHeight);
  camera.aspect = newWidth / newHeight;
  camera.updateProjectionMatrix();
});

style.css
body {
    margin: 0;
    font-family: system-ui, sans-serif;
    background: radial-gradient(circle at top, #0b1a3a, #020412);
    color: #fff;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: flex-start;
    min-height: 100vh;
    flex-basis: auto;
    justify-content: center
}

header {
    margin-top: 30px;
    font-size: 32px;
    font-weight: 700;
    text-align: center;
}

main{
    margin: 30px auto;
    width: 600px;
    max-width: 90vw;
    aspect-ratio: 4 / 3;
    border: 4px solid #ffd700;
    border-radius: 16px;
    box-shadow: 0 0 25px rgba(255, 215, 0, 0.6);
    overflow: hidden;
    position: relative;
    background: #000;
}

footer {
    margin-top: 10px;
    font-size: 20px;
    text-align: center;
    opacity: 0.9;
}

#tree-conteiner {
    width: 100%;
    height: 100%;
}

@media (max-width: 600px) {
    header{
        font-size: 24px;
    }
    footer{
        font-size: 16px;
    }
}
