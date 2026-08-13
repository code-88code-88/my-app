<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>تطبيقي</title>

<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
  font-family: Arial, sans-serif;
}

body {
  min-height: 100vh;
  background: #0b0b0f;
  color: white;
}

/* شاشة الدخول */
#loginScreen {
  position: fixed;
  inset: 0;
  background: linear-gradient(145deg, #08080c, #171722);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 9999;
}

.login-box {
  width: min(90%, 360px);
  padding: 30px 25px;
  border-radius: 25px;
  background: rgba(255,255,255,.07);
  border: 1px solid rgba(255,255,255,.12);
  box-shadow: 0 20px 60px rgba(0,0,0,.5);
  text-align: center;
  backdrop-filter: blur(15px);
}

.logo {
  width: 70px;
  height: 70px;
  margin: auto auto 18px;
  border-radius: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
  background: #ffffff;
  color: #111;
  font-size: 28px;
  font-weight: bold;
}

.login-box h1 {
  font-size: 24px;
  margin-bottom: 8px;
}

.login-box p {
  color: #aaa;
  font-size: 14px;
  margin-bottom: 25px;
}

/* خانات الرقم */
.pin {
  display: flex;
  justify-content: center;
  direction: ltr;
  gap: 10px;
  margin-bottom: 22px;
}

.pin span {
  width: 48px;
  height: 58px;
  border-radius: 14px;
  border: 1px solid #444;
  background: #111118;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 25px;
  font-weight: bold;
}

.pin span.active {
  border-color: white;
}

/* لوحة الأرقام */
.keypad {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 10px;
}

.keypad button {
  height: 55px;
  border: 0;
  border-radius: 15px;
  background: #20202a;
  color: white;
  font-size: 20px;
  cursor: pointer;
  transition: .15s;
}

.keypad button:active {
  transform: scale(.93);
  background: #33333f;
}

.error {
  height: 20px;
  color: #ff5c5c;
  font-size: 13px;
  margin-bottom: 8px;
}

/* التطبيق */
#app {
  display: none;
  min-height: 100vh;
  padding: 25px;
}

.app-header {
  padding: 20px;
  border-radius: 22px;
  background: #15151d;
  margin-bottom: 20px;
}

.app-header h2 {
  margin-bottom: 7px;
}

.card {
  background: #15151d;
  border-radius: 22px;
  padding: 25px;
  text-align: center;
}

.logout {
  margin-top: 20px;
  padding: 13px 25px;
  border: 0;
  border-radius: 14px;
  background: #292934;
  color: white;
  cursor: pointer;
}
</style>
</head>

<body>

<!-- شاشة الدخول -->
<div id="loginScreen">

  <div class="login-box">

    <div class="logo">🔐</div>

    <h1>تطبيقي</h1>
    <p>أدخل رمز الدخول للمتابعة</p>

    <div class="pin">
      <span id="p1"></span>
      <span id="p2"></span>
      <span id="p3"></span>
      <span id="p4"></span>
    </div>

    <div class="error" id="error"></div>

    <div class="keypad">
      <button onclick="press(1)">1</button>
      <button onclick="press(2)">2</button>
      <button onclick="press(3)">3</button>

      <button onclick="press(4)">4</button>
      <button onclick="press(5)">5</button>
      <button onclick="press(6)">6</button>

      <button onclick="press(7)">7</button>
      <button onclick="press(8)">8</button>
      <button onclick="press(9)">9</button>

      <button onclick="clearPin()">⌫</button>
      <button onclick="press(0)">0</button>
      <button onclick="login()">✓</button>
    </div>

  </div>

</div>


<!-- واجهة التطبيق الرئيسية -->
<div id="app">

  <div class="app-header">
    <h2>مرحباً بك 👋</h2>
    <p>تم فتح تطبيقك بنجاح.</p>
  </div>

  <div class="card">
    <h2>واجهة تطبيقك</h2>
    <p style="margin-top:10px;color:#aaa;">
      ضع هنا محتوى تطبيقك الحقيقي.
    </p>

    <button class="logout" onclick="logout()">
      قفل التطبيق
    </button>
  </div>

</div>


<script>

/* الرقم السري */
const PASSWORD = "4321";

let enteredPin = "";


/* إدخال رقم */
function press(number) {

  if (enteredPin.length >= 4) return;

  enteredPin += number;

  updatePin();

  if (enteredPin.length === 4) {
    setTimeout(login, 200);
  }
}


/* تحديث الخانات */
function updatePin() {

  for (let i = 1; i <= 4; i++) {

    const box = document.getElementById("p" + i);

    if (enteredPin[i - 1]) {
      box.textContent = "●";
      box.classList.add("active");
    } else {
      box.textContent = "";
      box.classList.remove("active");
    }
  }
}


/* مسح */
function clearPin() {

  enteredPin = enteredPin.slice(0, -1);

  document.getElementById("error").textContent = "";

  updatePin();
}


/* تسجيل الدخول */
function login() {

  if (enteredPin.length !== 4) return;

  if (enteredPin === PASSWORD) {

    document.getElementById("loginScreen").style.display = "none";
    document.getElementById("app").style.display = "block";

    enteredPin = "";
    updatePin();

  } else {

    document.getElementById("error").textContent =
      "رمز الدخول غير صحيح";

    enteredPin = "";

    updatePin();
  }
}


/* قفل التطبيق */
function logout() {

  document.getElementById("app").style.display = "none";
  document.getElementById("loginScreen").style.display = "flex";

  enteredPin = "";
  updatePin();
}

</script>

</body>
</html>
