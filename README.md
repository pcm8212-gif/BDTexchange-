# BDTexchange-
BDTexchange
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Money Exchange - User Panel</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<style>
body{margin:0;font-family:Arial;background:#0f172a;color:#fff;}
header{background:#020617;padding:15px;text-align:center;font-size:20px;font-weight:bold;}
.box{background:#020617;margin:15px;padding:15px;border-radius:10px;}
label{display:block;margin-top:10px;font-size:14px;}
input,select,button{width:100%;padding:10px;margin-top:5px;border:none;border-radius:6px;font-size:14px;}
input,select{background:#0f172a;color:#fff;}
button{background:#22c55e;color:#000;font-weight:bold;margin-top:10px;}
button:hover{opacity:.9;}
.copyBtn{background:#38bdf8;margin-top:5px;}
table{width:100%;border-collapse:collapse;margin-top:10px;}
th,td{border:1px solid #334155;padding:8px;font-size:13px;text-align:center;}
th{background:#1e293b;}
.status{margin-top:10px;font-size:14px;}
.hidden{display:none;}
img{max-width:100px;border-radius:5px;}
</style>
</head>
<body>

<header>💱 Money Exchange - User Panel</header>

<!-- LOGIN / SIGNUP -->
<div class="box" id="authBox">
  <h3 id="authTitle">Login</h3>

  <div id="signupFields" class="hidden">
    <label>Username</label>
    <input type="text" id="authName" placeholder="Enter your name">

    <label>Email</label>
    <input type="email" id="authEmail" placeholder="Enter your Gmail">
  </div>

  <label>Phone Number</label>
  <input type="text" id="authPhone" placeholder="01XXXXXXXXX">

  <label>Password</label>
  <input type="password" id="authPass" placeholder="Password">

  <button id="authBtn">Login</button>

  <p style="margin-top:10px;font-size:13px;">
    <span id="toggleAuth" style="color:#38bdf8;cursor:pointer;">Don't have an account? Sign Up</span>
  </p>

  <div class="status" id="authStatus"></div>
</div>

<!-- MAIN PANEL -->
<div id="mainPanel" class="hidden">

<!-- FEE LIST -->
<div class="box">
<h3>💸 Exchange Fee List</h3>
<table>
<tr><th>Amount</th><th>Fee</th></tr>
<tr><td>100 ৳</td><td>10 ৳</td></tr>
<tr><td>200 ৳</td><td>20 ৳</td></tr>
<tr><td>500 ৳</td><td>50 ৳</td></tr>
<tr><td>1000 ৳</td><td>100 ৳</td></tr>
</table>
<p style="font-size:12px;color:#cbd5f5;margin-top:5px;">⚠️ প্রতি 100৳ এ 10৳ চার্জ</p>
</div>

<!-- PAYMENT NUMBERS -->
<div class="box">
<h3>📲 Send Payment To</h3>
<div style="background:#0f172a;padding:10px;border-radius:8px;margin-bottom:10px;">
<b>bKash</b><br><span id="bkashNum">01734689217</span>
<button class="copyBtn" onclick="copyText('bkashNum')">Copy</button>
</div>
<div style="background:#0f172a;padding:10px;border-radius:8px;">
<b>Nagad</b><br><span id="nagadNum">01734689217</span>
<button class="copyBtn" onclick="copyText('nagadNum')">Copy</button>
</div>
</div>

<!-- EXCHANGE FORM -->
<div class="box">
<h3>🔁 Exchange Money</h3>
<label>From</label>
<select id="from"><option>bKash</option><option>Nagad</option></select>
<label>To</label>
<select id="to"><option>Nagad</option><option>bKash</option></select>
<label>Amount (৳)</label>
<input type="number" id="amount" oninput="calcFee()">
<label>Fee (৳)</label>
<input type="text" id="fee" readonly>
<label>You Will Receive (৳)</label>
<input type="text" id="receive" readonly>
<label>Sender Number</label>
<input type="text" id="sender">
<label>Transaction ID</label>
<input type="text" id="txnid">
<label>Payment Screenshot</label>
<input type="file" id="screenshot" accept="image/*" onchange="previewImage()">
<img id="preview" style="display:none;margin-top:10px;width:100%;border-radius:8px;">
<button onclick="submitTx()">Submit Exchange</button>
<div class="status" id="status"></div>
</div>

<!-- TRANSACTION LIST -->
<div class="box">
<h3>📋 My Transactions</h3>
<table>
<thead>
<tr>
<th>ID</th>
<th>Amount</th>
<th>Fee</th>
<th>Receive</th>
<th>Status</th>
</tr>
</thead>
<tbody id="txList"></tbody>
</table>
</div>

<!-- CONTACT ADMIN -->
<div class="box">
  <h3>📞 Contact Admin</h3>
  <p style="font-size:14px;margin-bottom:10px;">
    কোনো সমস্যা হলে বা সাহায্য দরকার হলে আমার সাথে যোগাযোগ করুন:
  </p>
  <a href="https://t.me/ESHAN456" target="_blank">
    <button style="background:#38bdf8;color:#000;font-weight:bold;">Contact on Telegram</button>
  </a>
</div>

<script>
let txId=1;

// Copy Number
function copyText(id){navigator.clipboard.writeText(document.getElementById(id).innerText);alert("Copied");}

// Fee Calculation
function calcFee(){
  const amt=Number(document.getElementById("amount").value);
  if(!amt) return;
  const fee=Math.floor(amt/100)*10;
  document.getElementById("fee").value=fee;
  document.getElementById("receive").value=amt-fee;
}

// Screenshot Preview
function previewImage(){
  const file=screenshot.files[0];
  if(!file) return;
  const r=new FileReader();
  r.onload=e=>{preview.src=e.target.result;preview.style.display="block";}
  r.readAsDataURL(file);
}

// Submit Transaction
function submitTx(){
  if(!amount.value||!txnid.value||!sender.value||!screenshot.files[0]){
    alert("সব তথ্য দিন");
    return;
  }
  const row=txList.insertRow(0);
  row.innerHTML=`<td>#${txId++}</td><td>${amount.value}</td><td>${fee.value}</td><td>${receive.value}</td><td style="color:orange">Pending</td>`;
  status.innerHTML="⏳ Transaction submitted. Admin approval pending.";
  amount.value=fee.value=receive.value=txnid.value=sender.value="";
  screenshot.value=""; preview.style.display="none";
}

// ====== LOGIN / SIGNUP ======
const authBox=document.getElementById("authBox");
const mainPanel=document.getElementById("mainPanel");
const authTitle=document.getElementById("authTitle");
const authBtn=document.getElementById("authBtn");
const toggleAuth=document.getElementById("toggleAuth");
const authStatus=document.getElementById("authStatus");
const authPhone=document.getElementById("authPhone");
const authPass=document.getElementById("authPass");
const authName=document.getElementById("authName");
const authEmail=document.getElementById("authEmail");
const signupFields=document.getElementById("signupFields");

let users=JSON.parse(localStorage.getItem("users"))||[];

function showMain(phone){authBox.classList.add("hidden");mainPanel.classList.remove("hidden");localStorage.setItem("currentUser",phone);}

authBtn.onclick=()=>{
  const phone=authPhone.value.trim();
  const pass=authPass.value.trim();
  const name=authName.value.trim();
  const email=authEmail.value.trim();

  if(authTitle.innerText==="Login"){
    // Login only phone + password
    const user=users.find(u=>u.phone===phone && u.pass===pass);
    if(user){showMain(phone);}
    else{authStatus.innerText="Invalid Phone or Password";}
  }else{
    // Sign Up
    if(!name||!email||!phone||!pass){authStatus.innerText="সব তথ্য পূরণ করুন";return;}
    if(users.find(u=>u.phone===phone)){authStatus.innerText="Phone already registered. Login instead.";return;}
    users.push({name,email,phone,pass});
    localStorage.setItem("users",JSON.stringify(users));
    showMain(phone);
  }
}

// Toggle Login / Signup
toggleAuth.onclick=()=>{
  if(authTitle.innerText==="Login"){
    authTitle.innerText="Sign Up";
    authBtn.innerText="Sign Up";
    signupFields.classList.remove("hidden");
    toggleAuth.innerText="Already have an account? Login";
    authStatus.innerText="";
  }else{
    authTitle.innerText="Login";
    authBtn.innerText="Login";
    signupFields.classList.add("hidden");
    toggleAuth.innerText="Don't have an account? Sign Up";
    authStatus.innerText="";
  }
}

// Auto login if already logged in
const currentUser=localStorage.getItem("currentUser");
if(currentUser) showMain(currentUser);
</script>

</body>
</html>
