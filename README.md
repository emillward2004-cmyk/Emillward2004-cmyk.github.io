# UBER EATS PROFIT AND COST CALCULATOR
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Delivery Profit Tracker</title>

<style>

body{
font-family:-apple-system,BlinkMacSystemFont,"Segoe UI",Roboto;
background:#0f172a;
color:white;
margin:0;
padding:20px;
}

h1{
text-align:center;
margin-bottom:20px;
}

.container{
max-width:420px;
margin:auto;
}

.card{
background:#1e293b;
padding:18px;
border-radius:14px;
margin-bottom:16px;
box-shadow:0 5px 20px rgba(0,0,0,0.4);
}

label{
font-size:14px;
opacity:.8;
}

input{
width:100%;
padding:12px;
margin:6px 0 14px 0;
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

button:active{
transform:scale(.98);
}

.result{
font-size:15px;
margin:6px 0;
}

.history{
max-height:220px;
overflow-y:auto;
}

.shift{
background:#0f172a;
padding:10px;
border-radius:10px;
margin-bottom:8px;
font-size:14px;
}

.total{
font-weight:bold;
font-size:16px;
margin-top:8px;
}

</style>
</head>

<body>

<div class="container">

<h1>🚗 Delivery Tracker</h1>

<div class="card">

<label>Miles Driven</label>
<input type="number" id="miles">

<label>Fuel Cost (£)</label>
<input type="number" id="fuel">

<label>Insurance Cost (£)</label>
<input type="number" id="insurance">

<label>Gross Income (£)</label>
<input type="number" id="income">

<button onclick="saveShift()">Save Shift</button>

</div>

<div class="card">

<div class="result">⛽ Fuel per mile: £<span id="fuelpm">0</span></div>
<div class="result">🛡 Insurance per mile: £<span id="inspm">0</span></div>
<div class="result">🚗 Cost per mile: £<span id="costpm">0</span></div>
<div class="result">💰 Profit: £<span id="profit">0</span></div>

</div>

<div class="card">

<h3>Shift History</h3>

<div class="history" id="history"></div>

<div class="total">Total Profit: £<span id="totalProfit">0</span></div>

</div>

</div>

<script>

let shifts = JSON.parse(localStorage.getItem("shifts")) || [];

function saveShift(){

let miles = parseFloat(document.getElementById("miles").value);
let fuel = parseFloat(document.getElementById("fuel").value);
let insurance = parseFloat(document.getElementById("insurance").value);
let income = parseFloat(document.getElementById("income").value);

if(!miles || !fuel || !insurance || !income) return;

let fuelpm = fuel/miles;
let inspm = insurance/miles;
let costpm = (fuel+insurance)/miles;
let profit = income - fuel - insurance;

document.getElementById("fuelpm").innerText = fuelpm.toFixed(2);
document.getElementById("inspm").innerText = inspm.toFixed(2);
document.getElementById("costpm").innerText = costpm.toFixed(2);
document.getElementById("profit").innerText = profit.toFixed(2);

let shift = {
date:new Date().toLocaleDateString(),
miles,
fuel,
insurance,
income,
profit
};

shifts.push(shift);

localStorage.setItem("shifts",JSON.stringify(shifts));

renderHistory();

}

function renderHistory(){

let history = document.getElementById("history");
history.innerHTML="";

let totalProfit = 0;

shifts.slice().reverse().forEach(s=>{

totalProfit += s.profit;

let div=document.createElement("div");
div.className="shift";

div.innerHTML=`
📅 ${s.date}<br>
Miles: ${s.miles} | Income: £${s.income}<br>
Profit: £${s.profit.toFixed(2)}
`;

history.appendChild(div);

});

document.getElementById("totalProfit").innerText = totalProfit.toFixed(2);

}

renderHistory();

</script>

</body>
</html>
