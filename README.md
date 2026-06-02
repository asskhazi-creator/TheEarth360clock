<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>Earth360 Clock</title>

<link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@700&display=swap" rel="stylesheet">

<style>

*{
margin:0;
padding:0;
box-sizing:border-box;
}

body{
background:#000;
color:#fff;
font-family:Arial,sans-serif;
width:100vw;
height:100vh;
overflow:hidden;
display:flex;
justify-content:center;
align-items:center;
text-align:center;
}

.container{
width:100%;
padding:15px;
}

h1{
font-size:30px;
letter-spacing:2px;
margin-bottom:10px;
}

.tagline{
font-size:13px;
opacity:.7;
margin-bottom:25px;
}

.earth360{
font-family:'Orbitron',sans-serif;
font-size:64px;
font-weight:bold;
letter-spacing:3px;
margin-bottom:10px;
}

.degree{
font-family:'Orbitron',sans-serif;
font-size:42px;
margin-bottom:8px;
}

.label{
font-size:13px;
opacity:.7;
margin-bottom:20px;
}

.clock24{
font-family:'Orbitron',sans-serif;
font-size:38px;
margin-bottom:5px;
}

.clock12{
font-family:'Orbitron',sans-serif;
font-size:38px;
margin-top:20px;
margin-bottom:5px;
}

.day{
font-size:30px;
font-weight:bold;
margin-top:25px;
}

.date{
font-size:22px;
margin-top:10px;
}

.footer{
margin-top:35px;
font-size:14px;
opacity:.7;
}

</style>
</head>

<body>

<div class="container">

<h1>EARTH360 CLOCK</h1>

<div class="tagline">
ONE EARTH • ONE TIME
</div>

<div class="earth360" id="earth360">
00:00:00:00
</div>

<div class="label">
EARTH360 TIME
</div>

<div class="degree" id="degree">
0.00°
</div>

<div class="label">
EARTH DEGREE
</div>

<div class="clock24" id="clock24">
00:00:00
</div>

<div class="label">
24-HOUR TIME
</div>

<div class="clock12" id="clock12">
00:00:00 AM
</div>

<div class="label">
12-HOUR TIME
</div>

<div class="day" id="day">
MONDAY
</div>

<div class="date" id="date">
01 JANUARY 2026
</div>

<div class="footer">
Created by Aleem
</div>

</div>

<script>

// Screen Wake Lock
let wakeLock = null;

async function keepScreenOn() {
try{
wakeLock = await navigator.wakeLock.request('screen');
}catch(err){}
}

keepScreenOn();

document.addEventListener("visibilitychange",()=>{
if(document.visibilityState==="visible"){
keepScreenOn();
}
});

function updateClock(){

const now = new Date();

const h24 = String(now.getHours()).padStart(2,'0');
const m24 = String(now.getMinutes()).padStart(2,'0');
const s24 = String(now.getSeconds()).padStart(2,'0');

document.getElementById("clock24").innerHTML =
`${h24}:${m24}:${s24}`;

let h12 = now.getHours()%12;
h12 = h12 ? h12 : 12;

const ampm =
now.getHours() >= 12 ? "PM" : "AM";

document.getElementById("clock12").innerHTML =
`${String(h12).padStart(2,'0')}:${m24}:${s24} ${ampm}`;

const days = [
"SUNDAY",
"MONDAY",
"TUESDAY",
"WEDNESDAY",
"THURSDAY",
"FRIDAY",
"SATURDAY"
];

document.getElementById("day").innerHTML =
days[now.getDay()];

const months = [
"JANUARY","FEBRUARY","MARCH",
"APRIL","MAY","JUNE",
"JULY","AUGUST","SEPTEMBER",
"OCTOBER","NOVEMBER","DECEMBER"
];

document.getElementById("date").innerHTML =
`${String(now.getDate()).padStart(2,'0')} ${months[now.getMonth()]} ${now.getFullYear()}`;

const midnight =
new Date(
now.getFullYear(),
now.getMonth(),
now.getDate()
);

const elapsed = now - midnight;

const fraction = elapsed / 86400000;

let total =
fraction * (36*36*36*36);

let eh = Math.floor(total/(36*36*36));

total %= (36*36*36);

let em = Math.floor(total/(36*36));

total %= (36*36);

let es = Math.floor(total/36);

let ems = Math.floor(total%36);

document.getElementById("earth360").innerHTML =
`${String(eh).padStart(2,'0')}:`+
`${String(em).padStart(2,'0')}:`+
`${String(es).padStart(2,'0')}:`+
`${String(ems).padStart(2,'0')}`;

const degree = fraction * 360;

document.getElementById("degree").innerHTML =
degree.toFixed(2) + "°";

}

setInterval(updateClock,100);
updateClock();

</script>

</body>
</html>
