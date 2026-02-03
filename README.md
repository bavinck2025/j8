<img width="416" height="791" alt="image" src="https://github.com/user-attachments/assets/667a985b-17ff-4b3c-beac-755c25b7c209" />






<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Dino Game</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<style>
body{
margin:0;
background:#f7f7f7;
display:flex;
justify-content:center;
align-items:center;
height:100vh;
}

canvas{
border:2px solid #333;
background:white;
touch-action:none;
}
</style>
</head>

<body>

<canvas id="game"></canvas>

<script>
const c = document.getElementById("game");
const ctx = c.getContext("2d");

function resize(){
c.width = Math.min(innerWidth-20,600);
c.height = 200;
}
resize();
addEventListener("resize",resize);

let dino,cactus,score,jumping,gameOver;

function reset(){
dino={x:50,y:140,w:30,h:40,vy:0};
cactus=[];
score=0;
jumping=false;
gameOver=false;
}

reset();

let gravity=0.7;
let speed=4;

document.addEventListener("keydown",e=>{
if(e.code==="Space") jump();
});

c.addEventListener("touchstart",e=>{
e.preventDefault();
jump();
});

function jump(){
if(!jumping){
dino.vy=-13;
jumping=true;
}
}

function spawn(){
if(!gameOver) cactus.push({x:c.width,y:150,w:20,h:30});
}

setInterval(spawn,1400);

function collide(a,b){
return(
a.x<b.x+b.w &&
a.x+a.w>b.x &&
a.y<b.y+b.h &&
a.y+a.h>b.y
);
}

function loop(){
ctx.clearRect(0,0,c.width,c.height);

dino.vy+=gravity;
dino.y+=dino.vy;

if(dino.y>=140){
dino.y=140;
jumping=false;
}

ctx.fillStyle="red";
ctx.fillRect(dino.x,dino.y,dino.w,dino.h);

ctx.fillStyle="black";
cactus.forEach((ca,i)=>{
ca.x-=speed;
ctx.fillRect(ca.x,ca.y,ca.w,ca.h);

if(collide(dino,ca)){
reset();
}
if(ca.x<0){
cactus.splice(i,1);
score++;
}
});

ctx.fillText("Score: "+score,10,20);

requestAnimationFrame(loop);
}

loop();
</script>

</body>
</html>
