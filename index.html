<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Fix Chat</title>
</head>
<body style="background:#000;color:#0f0;font-family:monospace;padding:20px">

<h2>CHAT FIX TEST</h2>

<input id="room" placeholder="room">
<input id="name" placeholder="name">

<button id="createBtn">CREATE ROOM</button>
<button id="joinBtn">JOIN ROOM</button>

<pre id="log"></pre>

<script type="module">
import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
import { getDatabase, ref, set } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-database.js";
import { getAuth, signInAnonymously } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-auth.js";

const firebaseConfig = {
  apiKey: "YOUR_KEY",
  authDomain: "YOUR_DOMAIN",
  databaseURL: "YOUR_DB",
  projectId: "YOUR_ID"
};

const app = initializeApp(firebaseConfig);
const db = getDatabase(app);
const auth = getAuth(app);

await signInAnonymously(auth);

const log = (t)=>document.getElementById("log").innerText += "\n" + t;

document.getElementById("createBtn").onclick = async () => {
  try {
    const room = document.getElementById("room").value;
    const name = document.getElementById("name").value;

    log("CLICKED CREATE");

    if (!room || !name) {
      log("EMPTY INPUT");
      return;
    }

    log("CREATING ROOM...");

    await set(ref(db, "rooms/" + room), {
      created: true,
      host: name,
      time: Date.now()
    });

    log("ROOM CREATED ✅");
  } catch (e) {
    log("ERROR ❌ " + e.message);
    console.error(e);
  }
};

document.getElementById("joinBtn").onclick = () => {
  log("JOIN CLICKED");
};
</script>

</body>
</html>
