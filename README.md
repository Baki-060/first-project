
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>SYSTEM</title>
<style>
*{margin:0;padding:0;box-sizing:border-box}
body{
 background:#000;
 color:#00ff41;
 font-family:monospace;
 overflow:hidden;
}
canvas{
 position:fixed;
 inset:0;
 width:100%;
 height:100%;
}
.terminal{
 position:absolute;
 top:50%;
 left:50%;
 transform:translate(-50%,-50%);
 width:90%;
 text-align:center;
 text-shadow:0 0 12px #00ff41;
}
#title{
 font-size:clamp(30px,8vw,65px);
 font-weight:bold;
 letter-spacing:5px;
}
#text{
 margin-top:20px;
 font-size:clamp(12px,3vw,18px);
 line-height:2;
 color:#00ff41;
}
.cursor{
 animation:blink .7s infinite;
}
@keyframes blink{
 50%{opacity:0}
}
</style>
</head>
<body>

<canvas id="matrix"></canvas>

<div class="terminal">
 <div id="title"></div>
 <div id="text"></div>
</div>

<script>
const canvas=document.getElementById("matrix");
const ctx=canvas.getContext("2d");

let columns,drops;

function resize(){
 canvas.width=innerWidth;
 canvas.height=innerHeight;
 columns=Math.floor(canvas.width/16);
 drops=Array(columns).fill(1);
}
resize();
window.addEventListener("resize",resize);

const chars="010101010101010101";

function matrix(){
 ctx.fillStyle="rgba(0,0,0,0.08)";
 ctx.fillRect(0,0,canvas.width,canvas.height);

 ctx.fillStyle="#00ff41";
 ctx.font="15px monospace";

 for(let i=0;i<drops.length;i++){
  let char=chars[Math.floor(Math.random()*chars.length)];
  ctx.fillText(char,i*16,drops[i]*16);

  if(drops[i]*16>canvas.height && Math.random()>.975){
   drops[i]=0;
  }
  drops[i]++;
 }
 requestAnimationFrame(matrix);
}
matrix();

const title="SYSTEM...";
const messages=[
 "> INITIALIZING SYSTEM",
 "> CONNECTING...",
 "> LOADING DATA",
 "> ACCESS GRANTED",
 "> SYSTEM READY"
];

let i=0;
let j=0;

function typeTitle(){
 if(i<title.length){
  document.getElementById("title").textContent+=title[i];
  i++;
  setTimeout(typeTitle,180);
 }else{
  typeMessages();
 }
}

function typeMessages(){
 if(j<messages.length){
  let line=document.createElement("div");
  document.getElementById("text").appendChild(line);

  let k=0;
  function write(){
   if(k<messages[j].length){
    line.textContent+=messages[j][k];
    k++;
    setTimeout(write,55);
   }else{
    j++;
    setTimeout(typeMessages,500);
   }
  }
  write();
 }
}

setTimeout(typeTitle,1500);
</script>
</body>
</html>
