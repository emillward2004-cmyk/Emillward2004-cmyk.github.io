# UBER EATS PROFIT AND COST CALCULATOR
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Uber Eats Profit Calculator</title>

<style>

body{
font-family:-apple-system,BlinkMacSystemFont,sans-serif;
background:linear-gradient(135deg,#1f2937,#111827);
color:white;
display:flex;
justify-content:center;
align-items:center;
height:100vh;
margin:0;
}

.container{
background:#1f2937;
padding:25px;
border-radius:18px;
width:90%;
max-width:420px;
box-shadow:0 10px 30px rgba(0,0,0,0.4);
}

h1{
text-align:center;
margin-bottom:20px;
}

label{
font-size:14px;
opacity:.8;
}

input{
width:100%;
padding:12px;
margin:6px 0 16px 0;
border-radius:10px;
border:none;
font-size:16px;
}

button{
width:100%;
padding:14px;
background:#22c55e;
border:none;
border-radius:12px;
font-size:16px;
font-weight:bold;
color:white;
cursor:pointer;
}

button:hover{
background:#16a34a;
}

.results{
margin-top:20px;
background:#111827;
padding:15px;
border-radius:12px;
}

.result{
margin:8px 0;
font-size:16px;
}

</style>
</head>

<body>

<div class="container">

<h1>🚗 Delivery Profit</h1>

<label>Miles Driven</label>
<input type="number" id="miles">

<label>Fuel Cost (£)</label>
<input type="number" id="fuel">

<label>Insurance Cost (£)</label>
<input type="number" id="insurance">

<label>Gross Income (£)</label>
<input type="number" id="income">

<button onclick="calculate()">Calculate</button>

<div class="results" id="results" style="display:none">

<div class="result">⛽ Fuel per mile: £<span id="fuelpm"></span></div>
<div class="result">🛡 Insurance per mile: £<span id="inspm"></span></div>
<div class="result">🚗 Total cost per mile: £<span id="costpm"></span></div>
<div class="result">💰 Net Profit: £<span id="profit"></span></div>

</div>

</div>

<script>

function calculate(){

let miles = parseFloat(document.getElementById("miles").value);
let fuel = parseFloat(document.getElementById("fuel").value);
let insurance = parseFloat(document.getElementById("insurance").value);
let income = parseFloat(document.getElementById("income").value);

let fuelpm = fuel / miles;
let inspm = insurance / miles;
let costpm = (fuel + insurance) / miles;
let profit = income - fuel - insurance;

document.getElementById("fuelpm").innerText = fuelpm.toFixed(2);
document.getElementById("inspm").innerText = inspm.toFixed(2);
document.getElementById("costpm").innerText = costpm.toFixed(2);
document.getElementById("profit").innerText = profit.toFixed(2);

document.getElementById("results").style.display="block";

}

</script>

</body>
</html>
