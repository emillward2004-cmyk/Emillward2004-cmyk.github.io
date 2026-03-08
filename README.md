
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Delivery Tracker</title>

<style>

body{
font-family:-apple-system,BlinkMacSystemFont,Segoe UI,Roboto;
background:#0f172a;
color:white;
margin:0;
}

header{
text-align:center;
padding:15px;
font-size:20px;
font-weight:bold;
background:#020617;
}

.container{
padding:20px;
max-width:500px;
margin:auto;
}

.card{
background:#1e293b;
padding:18px;
border-radius:14px;
margin-bottom:16px;
}

input{
width:100%;
padding:12px;
margin:8px 0 14px 0;
border-radius:10px;
border:none;
font-size:16px;
}

button{
width:100%;
padding:14px;
border:none;
border-radius:12px;
background:#22c55e;
color:white;
font-size:16px;
font-weight:bold;
}

.tabbar{
position:fixed;
bottom:0;
left:0;
right:0;
background:#020617;
display:flex;
justify-content:space-around;
padding:10px 0;
}

.tab{
color:white;
font-size:13px;
text-align:center;
cursor:pointer;
opacity:.7;
}

.tab.active{
opacity:1;
}

.page{
display:none;
padding-bottom:80px;
}

.page.active{
display:block;
}

.shift{
background:#0f172a;
padding:10px;
border-radius:10px;
margin-bottom:8px;
font-size:14px;
}

.stat{
margin:8px 0;
font-size:15px;
}

</style>
</head>

<body>

<header>🚗 Delivery Tracker</header>

<div class="container">

<div id="dashboard" class="page active">

<div class="card">
<div class="stat">Total Shifts: <span id="totalShifts">0</span></div>
<div class="stat">Total Miles: <span id="totalMiles">0</span></div>
<div class="stat">Total Income: £<span id="totalIncome">0</span></div>
<div class="stat">Total Profit: £<span id="totalProfit">0</span></div>
</div>

</div>

<div id="addshift" class="page">

<div class="card">

<label>Miles</label>
<input id="miles" type="number">

<label>Fuel Cost (£)</label>
<input id="fuel" type="number">

<label>Insurance Cost (£)</label>
<input id="insurance" type="number">

<label>Income (£)</label>
<input id="income" type="number">

<button onclick="saveShift()">Save Shift</button>

</div>

</div>

<div id="history" class="page">

<div class="card">
<div id="shiftList"></div>
</div>

</div>

<div id="stats" class="page">

<div class="card">

<div class="stat">Fuel per mile: £<span id="fuelpm">0</span></div>
<div class="stat">Insurance per mile: £<span id="inspm">0</span></div>
<div class="stat">Total cost per mile: £<span id="costpm">0</span></div>
<div class="stat">Profit per mile: £<span id="profitpm">0</span></div>

</div>

</div>

</div>

<div class="tabbar">

<div class="tab active" onclick="switchTab('dashboard',this)">Dashboard</div>
<div class="tab" onclick="switchTab('addshift',this)">Add Shift</div>
<div class="tab" onclick="switchTab('history',this)">History</div>
<div class="tab" onclick="switchTab('stats',this)">Stats</div>

</div>

<script>

let shifts = JSON.parse(localStorage.getItem("shifts")) || [];

function switchTab(id,el){

document.querySelectorAll(".page").forEach(p=>p.classList.remove("active"));
document.querySelectorAll(".tab").forEach(t=>t.classList.remove("active"));

document.getElementById(id).classList.add("active");
el.classList.add("active");

}

function saveShift(){

let miles=parseFloat(milesInput.value);
let fuel=parseFloat(fuelInput.value);
let insurance=parseFloat(insuranceInput.value);
let income=parseFloat(incomeInput.value);

let profit = income - fuel - insurance;

let shift={
date:new Date().toLocaleDateString(),
miles,
fuel,
insurance,
income,
profit
};

shifts.push(shift);

localStorage.setItem("shifts",JSON.stringify(shifts));

update();

}

function update(){

let totalMiles=0;
let totalFuel=0;
let totalInsurance=0;
let totalIncome=0;
let totalProfit=0;

let list=document.getElementById("shiftList");
list.innerHTML="";

shifts.slice().reverse().forEach(s=>{

totalMiles+=s.miles;
totalFuel+=s.fuel;
totalInsurance+=s.insurance;
totalIncome+=s.income;
totalProfit+=s.profit;

let div=document.createElement("div");

div.className="shift";

div.innerHTML=`
📅 ${s.date}<br>
Miles: ${s.miles} | Income: £${s.income}<br>
Profit: £${s.profit.toFixed(2)}
`;

list.appendChild(div);

});

document.getElementById("totalShifts").innerText=shifts.length;
document.getElementById("totalMiles").innerText=totalMiles.toFixed(1);
document.getElementById("totalIncome").innerText=totalIncome.toFixed(2);
document.getElementById("totalProfit").innerText=totalProfit.toFixed(2);

if(totalMiles>0){

document.getElementById("fuelpm").innerText=(totalFuel/totalMiles).toFixed(2);
document.getElementById("inspm").innerText=(totalInsurance/totalMiles).toFixed(2);
document.getElementById("costpm").innerText=((totalFuel+totalInsurance)/totalMiles).toFixed(2);
document.getElementById("profitpm").innerText=(totalProfit/totalMiles).toFixed(2);

}

}

update();

</script>

</body>
</html>