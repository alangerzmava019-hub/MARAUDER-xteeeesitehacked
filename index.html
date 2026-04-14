<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Telegram Full Pro</title>
<style>
body{margin:0;background:#0a0a0a;color:#0f0;font-family:monospace;display:flex;justify-content:center}
#app{width:100%;max-width:700px;padding:10px}
input,button{width:100%;padding:10px;margin:5px 0;background:#000;color:#0f0;border:1px solid #0f0}
.chat{height:420px;overflow:auto;border:1px solid #0f0;padding:10px}
.msg{padding:6px;border-bottom:1px solid #0f0;position:relative}
.me{color:#00ffff}
.small{font-size:11px;opacity:0.7}
.row{display:flex;gap:5px}
.row input{flex:1}
.badge{font-size:10px;color:#0f0;opacity:0.7}
.del{position:absolute;right:5px;top:5px;color:#f55;cursor:pointer;font-size:10px}
</style>
</head>
<body>

<div id="app">
<h2>🔐 TELEGRAM FULL PRO</h2>

<div id="login">
  <input id="room" placeholder="Room name">
  <input id="name" placeholder="Your name">
  <button onclick="join()">Join room</button>
</div>

<div id="chatBox" style="display:none;">
  <div class="small" id="info"></div>

  <div class="chat" id="chat"></div>
  <div class="small" id="typing"></div>

  <div class="row">
    <input id="msg" placeholder="Message">
    <button onclick="send()">➤</button>
  </div>

  <input type="file" id="file">
  <button onclick="sendFile()">Send file</button>

  <button onclick="leave()">Leave</button>
</div>

</div>

<script type="module">
import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
import { getAuth, signInAnonymously } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-auth.js";
import { getDatabase, ref, push, onValue, set, remove, update } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-database.js";
import { getStorage, ref as sRef, uploadBytes, getDownloadURL } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-storage.js";

const firebaseConfig = {
  apiKey:"YOUR_KEY",
  authDomain:"YOUR_DOMAIN",
  databaseURL:"YOUR_DB",
  projectId:"YOUR_ID",
  storageBucket:"YOUR_BUCKET",
  appId:"YOUR_APP_ID"
};

const app=initializeApp(firebaseConfig);
const auth=getAuth(app);
const db=getDatabase(app);
const storage=getStorage(app);

await signInAnonymously(auth);

let room,name,key="secret";

function enc(t){return btoa(unescape(encodeURIComponent(t)))}
function dec(t){try{return decodeURIComponent(escape(atob(t)))}catch{return t}}

window.join=function(){
  room=document.getElementById("room").value.trim();
  name=document.getElementById("name").value.trim();
  if(!room||!name) return alert("fill");

  document.getElementById("login").style.display="none";
  document.getElementById("chatBox").style.display="block";
  document.getElementById("info").innerText="Room: "+room;

  set(ref(db,"rooms/"+room+"/users/"+name),{online:true,time:Date.now()});

  listen();
  typing();
}

function listen(){
  onValue(ref(db,"rooms/"+room+"/msgs"),snap=>{
    const chat=document.getElementById("chat");
    chat.innerHTML="";

    snap.forEach(c=>{
      const m=c.val();
      const id=c.key;

      const div=document.createElement("div");
      div.className="msg"+(m.user===name?" me":"");

      let content="";

      if(m.type==="file"){
        content=`📎 <a href="${m.url}" target="_blank">file</a>`;
      }else{
        content=dec(m.text);
      }

      div.innerHTML=`
        <span class="badge">${m.user}</span><br>
        ${content}
        <div class="del" onclick="delMsg('${id}')">x</div>
      `;

      chat.appendChild(div);
    });

    chat.scrollTop=chat.scrollHeight;
  });
}

window.send=function(){
  const t=document.getElementById("msg").value;
  if(!t) return;

  push(ref(db,"rooms/"+room+"/msgs"),{
    user:name,
    text:enc(t),
    type:"text",
    time:Date.now()
  });

  set(ref(db,"rooms/"+room+"/typing/"+name),false);
  document.getElementById("msg").value="";
}

document.getElementById("msg").oninput=()=>{
  set(ref(db,"rooms/"+room+"/typing/"+name),true);
  setTimeout(()=>set(ref(db,"rooms/"+room+"/typing/"+name),false),800);
}

function typing(){
  onValue(ref(db,"rooms/"+room+"/typing"),snap=>{
    const d=snap.val()||{};
    const arr=Object.keys(d).filter(u=>d[u]&&u!==name);
    document.getElementById("typing").innerText=
      arr.length?arr.join(", ")+" typing...":"";
  });
}

window.sendFile=async function(){
  const f=document.getElementById("file").files[0];
  if(!f) return;

  const s=sRef(storage,"files/"+Date.now()+"_"+f.name);
  await uploadBytes(s,f);
  const url=await getDownloadURL(s);

  push(ref(db,"rooms/"+room+"/msgs"),{
    user:name,
    type:"file",
    url,
    time:Date.now()
  });
}

window.delMsg=function(id){
  remove(ref(db,"rooms/"+room+"/msgs/"+id));
}

window.leave=function(){
  remove(ref(db,"rooms/"+room+"/users/"+name));
  location.reload();
}
</script>

</body>
</html>
