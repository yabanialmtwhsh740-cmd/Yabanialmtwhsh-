<!DOCTYPE html>
<html lang="ar">
<head>
<meta charset="UTF-8">
<title>✨ᎯᏞᏚᎯᏞᎯᎷ.ᏁᎬᎿ✨</title>
<meta name="viewport" content="width=device-width, initial-scale=1">
<script src="https://www.gstatic.com/firebasejs/9.23.0/firebase-app-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/9.23.0/firebase-auth-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/9.23.0/firebase-database-compat.js"></script>
<script src="https://cdn.jsdelivr.net/npm/qrcodejs@1.0.0/qrcode.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/html5-qrcode@2.3.7/html5-qrcode.min.js"></script>
<style>
body{margin:0;font-family:Arial;direction:rtl;background:#000;color:#fff;overflow-x:hidden;transition:all 0.5s;}
header{text-align:center;padding:14px;font-size:22px;text-shadow:0 0 15px #00ffcc;transition:all 0.5s;}
.card{background:rgba(0,0,0,.65);backdrop-filter:blur(10px);border:1px solid rgba(0,255,204,.3);box-shadow:0 0 25px rgba(0,255,204,.2);border-radius:12px;padding:14px;margin:10px;display:flex;flex-direction:column;align-items:center;}
input,select{width:90%;padding:10px;margin:6px 0;border-radius:6px;border:none;transition:all 0.5s;}
button{padding:10px 14px;margin:6px 0;border-radius:50%;background:linear-gradient(135deg,#00ffcc,#0066ff);color:#000;font-weight:bold;cursor:pointer;transition:all 0.3s;}
button:hover{transform:scale(1.1) rotateY(10deg);}
.hidden{display:none}
#cards{display:flex;flex-wrap:wrap;justify-content:center;}
.card-bubble{width:120px;height:120px;border-radius:15px;background:rgba(0,255,204,.2);margin:5px;text-align:center;line-height:120px;cursor:pointer;display:flex;flex-direction:column;align-items:center;justify-content:center;transition:all 0.3s;}
.card-bubble:hover{transform:scale(1.1) rotateY(15deg);box-shadow:0 0 30px #00ffcc;}
#settingsCircle{position:fixed;bottom:20px;right:20px;width:60px;height:60px;border-radius:50%;background:linear-gradient(45deg,#00ffcc,#0066ff);display:flex;justify-content:center;align-items:center;cursor:pointer;box-shadow:0 0 15px #00ffcc;z-index:999;transition:all 0.3s;}
#settingsMenu{position:fixed;bottom:90px;right:20px;width:200px;background:rgba(0,0,0,0.85);backdrop-filter:blur(10px);border-radius:12px;padding:10px;display:flex;flex-direction:column;box-shadow:0 0 20px #00ffcc;z-index:998;opacity:0;transform:translateY(20px);transition:all 0.3s ease-in-out;}
#settingsMenu.show{opacity:1;transform:translateY(0);}
#profileCircle{position:fixed;bottom:20px;left:20px;width:60px;height:60px;border-radius:50%;background:linear-gradient(45deg,#ffcc00,#ff6600);display:flex;justify-content:center;align-items:center;cursor:pointer;box-shadow:0 0 15px #ffcc00;z-index:999;transition:all 0.3s;}
#profileCard{position:fixed;top:50%;left:50%;transform:translate(-50%,-50%);width:250px;background:rgba(0,0,0,.85);backdrop-filter:blur(10px);border-radius:12px;padding:14px;display:none;flex-direction:column;align-items:center;box-shadow:0 0 20px #00ffcc;z-index:1000;}
#profileCard img{width:100px;height:100px;border-radius:50%;}
#profileCard button{margin-top:10px;border-radius:12px;}
#adminCircle{position:fixed;top:14px;left:50%;transform:translateX(-50%);width:50px;height:50px;border-radius:50%;background:linear-gradient(45deg,#00ffcc,#0066ff);display:flex;justify-content:center;align-items:center;cursor:pointer;box-shadow:0 0 15px #00ffcc;z-index:999;transition:all 0.3s;}
#splashScreen{position:fixed;top:0;left:0;width:100%;height:100%;background:#000;display:flex;justify-content:center;align-items:center;z-index:10000;color:#00ffcc;font-size:28px;animation:fadeSplash 3s forwards;}
@keyframes fadeSplash{0%{opacity:1;}90%{opacity:1;}100%{opacity:0;display:none;}}
</style>
</head>
<body>
<div id="splashScreen">✨ᎯᏞᏚᎯᏞᎯᎷ.ᏁᎬᎿ✨</div>
<header>✨ᎯᏞᏚᎯᏞᎯᎷ.ᏁᎬᎿ✨</header>

<div id="auth" class="card">
<input id="email" placeholder="الإيميل">
<input id="pass" type="password" placeholder="كلمة المرور">
<button onclick="login()">دخول / إنشاء حساب</button>
<small>أول مرة؟ سيتم إنشاء الحساب تلقائيًا</small>
</div>

<div id="app" class="hidden">
<div id="adminCircle" class="hidden" onclick="goToAdmin()">👑</div>
<div id="profileCircle">👤</div>
<div id="profileCard">
<img id="avatar" src="https://via.placeholder.com/100">
<p id="uemail"></p>
<p>الرصيد: <b><span id="balance">0</span></b> ريال</p>
<p>ID: <span id="uid">---</span></p>
<button onclick="closeProfile()">رجوع</button>
</div>

<div class="card">
<h3>إنشاء QR للتحويل</h3>
<input id="qrAmount" type="number" placeholder="المبلغ">
<button onclick="makeQR()">QR</button>
<div id="qrcode"></div>
</div>

<div class="card">
<h3>مسح QR بالكاميرا</h3>
<div id="reader"></div>
<button onclick="startScan()">تشغيل الكاميرا</button>
<button onclick="stopScan()">إيقاف</button>
</div>

<div class="card">
<h3>شراء كرت</h3>
<div id="cards"></div>
</div>

<div class="card">
<h3>سجل العمليات</h3>
<div id="logs"></div>
</div>

<div class="card hidden" id="admin">
<h3>لوحة المطوّر</h3>
<button onclick="addBalance()">➕ إضافة رصيد</button>
<button onclick="deleteUser()">❌ حذف مستخدم</button>
<button onclick="addCard()">➕ إضافة كرت جديد</button>
<button onclick="sendToDeveloper(100)">💸 إرسال رصيد للمطور</button>
<div id="manageCards"></div>
</div>

<div id="settingsCircle">⚙️</div>
<div id="settingsMenu">
<button onclick="logout()">تسجيل الخروج</button>
<label>لون التطبيق:
<select id="themeSelect" onchange="changeTheme(this.value)">
<option value="#000">أسود</option>
<option value="#111111">رمادي داكن</option>
<option value="#222222">رمادي فاتح</option>
<option value="#003366">أزرق داكن</option>
<option value="#330033">بنفسجي</option>
</select>
</label>
<button onclick="startScan()">مسح QR</button>
<button onclick="contactDeveloper()">تواصل مع المطور</button>
</div>

<script>
// Firebase Config
const firebaseConfig={apiKey:"AIzaSyBwbxWIr0KLe6s_U39vbG5Mm-NE_J7y9ag",authDomain:"yabanialmtwhsh-c01cb.firebaseapp.com",projectId:"yabanialmtwhsh-c01cb",storageBucket:"yabanialmtwhsh-c01cb.firebasestorage.app",messagingSenderId:"348232184030",appId:"1:348232184030:web:cc8a583db0374d88f3124f",measurementId:"G-PVMJN9069C"};
firebase.initializeApp(firebaseConfig);
const ADMIN_EMAIL="yabanialmtwhsh740@email.com";
let user,cardList=[{price:200,MB:500,amount:100,icon:"https://via.placeholder.com/120?text=200MB",symbols:[]},{price:300,MB:800,amount:100,icon:"https://via.placeholder.com/120?text=800MB",symbols:[]}]; // يمكنك إضافة باقي الكروت
const authForm=document.getElementById("auth");
const emailInput=document.getElementById("email");
const passInput=document.getElementById("pass");
const appDiv=document.getElementById("app");
const uemail=document.getElementById("uemail");
const balance=document.getElementById("balance");
const uidSpan=document.getElementById("uid");
const profileCard=document.getElementById("profileCard");

firebase.auth().onAuthStateChanged(u=>{
  if(u){
    user=u;
    authForm.classList.add("hidden");
    appDiv.classList.remove("hidden");
    uemail.innerText=u.email;
    if(u.email===ADMIN_EMAIL) document.getElementById("adminCircle").classList.remove("hidden");
    assignID();
    loadUser();
    displayCards();
  } else {
    authForm.classList.remove("hidden");
    appDiv.classList.add("hidden");
  }
});

function login(){
  const email=emailInput.value.trim();
  const pass=passInput.value.trim();
  if(!email || !pass) return alert("أدخل إيميل وكلمة مرور صحيحة");
  firebase.auth().signInWithEmailAndPassword(email,pass)
    .catch(()=>firebase.auth().createUserWithEmailAndPassword(email,pass)
    .catch(e=>alert("حدث خطأ: "+e.message)));
}

// باقي الوظائف (عرض الكروت، شراء، سجل العمليات، لوحة المطور، QR، إشعارات 3D) كما في النسخة السابقة
</script>
</body>
</html>
