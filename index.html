<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Firebase Messenger Pro</title>
<style>
body{margin:0;background:#0a0a0a;color:#0f0;font-family:monospace;display:flex;justify-content:center}
#app{width:100%;max-width:700px;padding:10px}
input,button{width:100%;padding:10px;margin:5px 0;background:#000;color:#0f0;border:1px solid #0f0}
.chat{height:420px;overflow:auto;border:1px solid #0f0;padding:10px}
.msg{padding:6px;border-bottom:1px solid #0f0;position:relative}
.me{color:#0ff}
.small{font-size:11px;opacity:0.7}
.del{position:absolute;right:5px;top:5px;color:red;cursor:pointer;font-size:10px}
.row{display:flex;gap:5px}
.row input{flex:1}
</style>
</head>
<body>

<div id="app">

<h2>🔥 FIREBASE MESSENGER PRO</h2>

<div id="login">
  <input id="room" placeholder="Room name">
  <input id="name" placeholder="Your name">

  <button onclick="createRoom()">Create room</button>
  <button onclick="joinRoom()">Join room</button>
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
import { getDatabase, ref, set, push, onValue, remove } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-database.js";
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

let room,name;

function enc(t){return btoa(unescape(encodeURIComponent(t)))}
function dec(t){try{return decodeURIComponent(escape(atob(t)))}catch{return t}}

window.createRoom=function(){
  room=document.getElementById("room").value.trim();
  name=document.getElementById("name").value.trim();
  if(!room||!name) return alert("fill");

  set(ref(db,"rooms/"+room),{
    created:true,
    createdAt:Date.now()
  });

  enter();
}

window.joinRoom=function(){
  room=document.getElementById("room").value.trim();
  name=document.getElementById("name").value.trim();
  if(!room||!name) return alert("fill");

  enter();
}

function enter(){
  document.getElementById("login").style.display="none";
  document.getElementById("chatBox").style.display="block";
  document.getElementById("info").innerText="Room: "+room;

  set(ref(db,"rooms/"+room+"/users/"+name),{online:true});

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
        <b>${m.user}</b><br>
        ${content}
        <div class="del" onclick="del('${id}')">x</div>
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

  document.getElementById("msg").value="";
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

window.del=function(id){
  remove(ref(db,"rooms/"+room+"/msgs/"+id));
}

function typing(){
  onValue(ref(db,"rooms/"+room+"/typing"),snap=>{
    const d=snap.val()||{};
    const arr=Object.keys(d).filter(u=>d[u]&&u!==name);
    document.getElementById("typing").innerText=
      arr.length?arr.join(", ")+" typing...":"";
  });
}

document.getElementById("msg").oninput=()=>{
  set(ref(db,"rooms/"+room+"/typing/"+name),true);
  setTimeout(()=>set(ref(db,"rooms/"+room+"/typing/"+name),false),800);
}

window.leave=function(){
  remove(ref(db,"rooms/"+room+"/users/"+name));
  location.reload();
}
</script>

</body>
</html>
