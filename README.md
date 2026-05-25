<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Diana Alejandra ❤️</title>

<style>
body{
    margin:0;
    overflow:hidden;
    background: radial-gradient(circle,#0a0205,#000);
    font-family:Arial;
    color:white;
}

/* INTRO */
#intro{
    position:absolute;
    width:100%;
    height:100%;
    display:flex;
    flex-direction:column;
    justify-content:center;
    align-items:center;
    background:black;
    z-index:10;
}

#intro h1{
    font-size:45px;
    color:#ff9bb3;
    letter-spacing:4px;
}

#intro p{
    margin:20px;
    font-size:14px;
    color:#ccc;
    text-align:center;
    max-width:300px;
}

#intro button{
    padding:12px 30px;
    border:1px solid pink;
    background:none;
    color:white;
    cursor:pointer;
    border-radius:20px;
}

/* TARJETA */
#card{
    position:absolute;
    top:50%;
    left:50%;
    transform:translate(-50%,-50%);
    background:rgba(0,0,0,0.85);
    padding:30px;
    border-radius:20px;
    text-align:center;
    display:none;
    max-width:320px;
    border:1px solid #ff9bb3;
}

#card h2{
    color:#ff9bb3;
}

#card button{
    margin-top:15px;
    padding:8px 20px;
    background:none;
    border:1px solid white;
    color:white;
    cursor:pointer;
}

/* CANVAS */
canvas{
    display:block;
}
</style>

<script type="importmap">
{
 "imports": {
  "three": "https://unpkg.com/three@0.160.0/build/three.module.js"
 }
}
</script>
</head>

<body>

<div id="intro">
<h1>DIANA ALEJANDRA</h1>
<p>Para la más hermosa, la más preciosa… Diana Alejandra.</p>
<button id="btnStart">Entrar a nuestro universo</button>
</div>

<div id="card">
<h2 id="title"></h2>
<p id="text"></p>
<button id="btnClose">Cerrar</button>
</div>

<script type="module">
import * as THREE from 'three';

const frases = [
"Tus ojos verdes que me pierden",
"Tu sonrisa que me desarma",
"Tu mirada cuando me ves",
"Lo hermosa que eres",
"Tu forma de abrazarme",
"Tu voz",
"Tu esencia",
"Tus besos",
"Tu presencia",
"Todo lo que eres",
"Que seas tú… Diana Alejandra"
];

let scene, camera, renderer, objects=[];

// BOTONES (SOLUCIONADO)
document.getElementById("btnStart").addEventListener("click", start);
document.getElementById("btnClose").addEventListener("click", closeCard);

function start(){
    document.getElementById("intro").style.display="none";

    scene = new THREE.Scene();

    camera = new THREE.PerspectiveCamera(75,innerWidth/innerHeight,0.1,1000);
    camera.position.z=60;

    renderer = new THREE.WebGLRenderer({antialias:true});
    renderer.setSize(innerWidth,innerHeight);
    document.body.appendChild(renderer.domElement);

    const geometry = new THREE.SphereGeometry(1.2,32,32);

    frases.forEach((f)=>{
        const material = new THREE.MeshBasicMaterial({color:0xff6f91});
        const sphere = new THREE.Mesh(geometry,material);

        sphere.position.x=(Math.random()-0.5)*120;
        sphere.position.y=(Math.random()-0.5)*80;
        sphere.position.z=(Math.random()-0.5)*120;

        sphere.userData.text=f;

        scene.add(sphere);
        objects.push(sphere);
    });

    animate();
}

const raycaster=new THREE.Raycaster();
const mouse=new THREE.Vector2();

window.addEventListener("click",(e)=>{
    mouse.x=(e.clientX/window.innerWidth)*2-1;
    mouse.y=-(e.clientY/window.innerHeight)*2+1;

    if(!camera) return;

    raycaster.setFromCamera(mouse,camera);
    const intersects=raycaster.intersectObjects(objects);

    if(intersects.length>0){
        const obj=intersects[0].object;

        document.getElementById("title").innerText=obj.userData.text;

        document.getElementById("text").innerText=
        "Diana Alejandra… la más hermosa, la más preciosa. No eres solo alguien especial, eres quien le dio sentido a todo en mi vida.";

        document.getElementById("card").style.display="block";
    }
});

function closeCard(){
    document.getElementById("card").style.display="none";
}

function animate(){
    requestAnimationFrame(animate);

    objects.forEach(o=>{
        o.rotation.y+=0.008;
        o.rotation.x+=0.004;
    });

    renderer.render(scene,camera);
}

window.addEventListener("resize",()=>{
    if(!camera) return;
    camera.aspect=innerWidth/innerHeight;
    camera.updateProjectionMatrix();
    renderer.setSize(innerWidth,innerHeight);
});
</script>

</body>
</html>
