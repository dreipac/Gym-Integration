// init supabase
const supabase = window.supabase.createClient(window.__SUPABASE_URL, window.__SUPABASE_ANON);

// Views/Refs
const formLogin   = document.getElementById("form-login");
const formRegister= document.getElementById("form-register");
const linkToReg   = document.getElementById("link-to-register");
const linkToLogin = document.getElementById("link-to-login");
const loginMsg    = document.getElementById("login-msg");
const regMsg      = document.getElementById("reg-msg");

function show(view){
  const isLogin = view === "login";
  formLogin.hidden = !isLogin;
  formRegister.hidden = isLogin;
  (isLogin ? loginMsg : regMsg).textContent = "";
  (isLogin ? loginMsg : regMsg).classList.remove("err");
}

// Links: Login <-> Register (SPA)
if (linkToReg)   linkToReg.addEventListener("click", (e)=>{ e.preventDefault(); show("register"); });
if (linkToLogin) linkToLogin.addEventListener("click", (e)=>{ e.preventDefault(); show("login"); });

// Default: immer Login zeigen
show("login");

// already logged in? -> go to app
(async () => {
  const { data: { session } } = await supabase.auth.getSession();
  if (session) location.replace("index.html");
})();

// login
formLogin.addEventListener("submit", async (e) => {
  e.preventDefault();
  loginMsg.textContent = "Anmelden…";
  loginMsg.classList.remove("err");

  const email = document.getElementById("login-email").value.trim();
  const password = document.getElementById("login-password").value;

  const { error } = await supabase.auth.signInWithPassword({ email, password });
  if (error) {
    loginMsg.textContent = "Fehler: " + error.message;
    loginMsg.classList.add("err");
  } else {
    location.replace("index.html"); 
  }
});

// password reset
const linkForgot = document.getElementById("link-forgot");
if (linkForgot) {
  linkForgot.addEventListener("click", async (e) => {
    e.preventDefault();
    const email = document.getElementById("login-email").value.trim();
    if (!email) {
      loginMsg.textContent = "Bitte gib zuerst deine E-Mail ein.";
      loginMsg.classList.add("err");
      return;
    }
    loginMsg.textContent = "Sende E-Mail zum Zurücksetzen…";
    loginMsg.classList.remove("err");

    const { error } = await supabase.auth.resetPasswordForEmail(email, {
      redirectTo: "https://dreipac.github.io/Gym-Integration/index.html"
      // lokal: redirectTo: "http://localhost:5500/index.html"
    });

    if (error) {
      loginMsg.textContent = "Fehler: " + error.message;
      loginMsg.classList.add("err");
    } else {
      loginMsg.textContent = "Wenn die E-Mail existiert, haben wir dir einen Link geschickt.";
    }
  });
}


// register
formRegister.addEventListener("submit", async (e) => {
  e.preventDefault();
  regMsg.textContent = "Konto wird erstellt…";
  regMsg.classList.remove("err");

  const email = document.getElementById("reg-email").value.trim();
  const password = document.getElementById("reg-password").value;

  const { data, error } = await supabase.auth.signUp({
    email,
    password,
    options: {
      // Nach E-Mail-Bestätigung hierhin zurück (URL ggf. anpassen)
      emailRedirectTo: "https://dreipac.github.io/Gym-Integration/index.html"
      // Für lokalen Test:
      // emailRedirectTo: "http://localhost:5500/index.html"
    }
  });

  if (error) {
    regMsg.textContent = "Fehler: " + error.message;
    regMsg.classList.add("err");
    return;
  }

  regMsg.textContent = data.user?.email_confirmed_at
    ? "Konto erstellt – weiterleiten…"
    : "Konto erstellt. Prüfe dein Postfach zur Bestätigung.";

  const { data: { session } } = await supabase.auth.getSession();
  if (session) location.href = "index.html";
});
