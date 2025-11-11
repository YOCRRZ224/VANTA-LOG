<h1 align="center">⚡ VANTA LOG</h1>

<p align="center">
  <b>Absorb Every Error.</b><br>
  A one-line drop-in web logging console built especially for Android & web developers.<br>
  Instantly visualize console output, network requests, fetch/XHR timings, and JS errors — right inside your webpage.
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Status-Under%20Construction-orange?style=for-the-badge&logo=tools" alt="Status">
  <img src="https://img.shields.io/badge/Platform-Android%20%7C%20Web-brightgreen?style=for-the-badge&logo=android" alt="Platform">
  <img src="https://img.shields.io/badge/Version-Stable%20Build%20Coming-blueviolet?style=for-the-badge&logo=github" alt="Version">
  <img src="https://img.shields.io/badge/License-MIT-green?style=for-the-badge&logo=open-source-initiative" alt="License">
  <img src="https://img.shields.io/badge/Made%20for-Developers-00ffaa?style=for-the-badge&logo=codeforces" alt="Made for Developers">
</p>

---

## 🧠 What is VANTA LOG?

VANTA LOG is a **lightweight in-browser debug console overlay** that lets you see all JavaScript logs, network requests, errors, and warnings in one sleek floating UI.  
It’s designed primarily for **Android developers testing locally** using Termux, Pydroid, or mobile browsers — where DevTools are limited or unavailable.

You can literally **paste one script** at the bottom of your HTML file and get a real-time log monitor.  
No build tools. No setup. No dependencies.  
Just pure `<script>` magic.

---

## 🚀 Quick Start (Plug & Play)

Paste this snippet **right before the closing `</body>` tag** of your project’s `index.html`:

```html
<!-- ⚡ VANTA LOG: Absorb Every Error -->
<script>
(function () {
  const MAX_LINES = 1000;

  // --- Overlay ---
  const dbg = document.createElement('div');
  dbg.id = 'vantalog';
  dbg.style.cssText = [
    'position:fixed',
    'left:8px',
    'right:8px',
    'bottom:8px',
    'max-height:40vh',
    'background:rgba(10,10,10,0.9)',
    'color:#bfffdc',
    'font-family:monospace',
    'font-size:12px',
    'line-height:1.25',
    'padding:8px',
    'border-radius:8px',
    'box-shadow:0 8px 30px rgba(0,0,0,0.7)',
    'z-index:2147483647',
    'overflow:auto',
    'backdrop-filter:blur(4px)'
  ].join(';');

  // --- Header ---
  const header = document.createElement('div');
  header.style.cssText = 'display:flex;gap:8px;align-items:center;margin-bottom:6px;';
  header.innerHTML = `
    <strong style="color:#00ffaa;margin-right:6px">VANTA LOG</strong>
    <button id="dbgClear" style="background:#111;border:1px solid #222;color:#fff;padding:4px 8px;border-radius:6px;cursor:pointer"><i class="ph ph-eraser"></i></button>
    <button id="dbgDownload" style="background:#111;border:1px solid #222;color:#fff;padding:4px 8px;border-radius:6px;cursor:pointer"><i class="ph ph-download"></i></button>
    <div id="filterContainer" style="position:relative;">
      <button id="filterBtn" style="background:#111;border:1px solid #222;color:#00ffaa;padding:4px 8px;border-radius:6px;cursor:pointer;display:flex;align-items:center;gap:4px;">
        <i class="ph ph-funnel-simple"></i>
      </button>
      <div id="filterMenu" style="display:none;position:absolute;top:32px;left:0;background:#111;border:1px solid #222;border-radius:6px;overflow:hidden;box-shadow:0 4px 12px rgba(0,0,0,0.5);">
        <div class="filterOption" data-type="all" style="padding:6px 12px;cursor:pointer;color:#fff;">All</div>
        <div class="filterOption" data-type="warn" style="padding:6px 12px;cursor:pointer;color:#ffcc00;"><i class="ph ph-seal-warning"></i></div>
        <div class="filterOption" data-type="error" style="padding:6px 12px;cursor:pointer;color:#ff5050;"><i class="ph ph-link-break"></i></div>
        <div class="filterOption" data-type="info" style="padding:6px 12px;cursor:pointer;color:#00aaff;"><i class="ph ph-info"></i></div>
        <div class="filterOption" data-type="debug" style="padding:6px 12px;cursor:pointer;color:#ff66ff;"><i class="ph ph-bug"></i></div>
        <div class="filterOption" data-type="resources" style="padding:6px 12px;cursor:pointer;color:#00ffaa;"><i class="ph ph-globe"></i></div>
      </div>
    </div>
    <button id="vantalogClose" style="background:#111;border:1px solid #222;color:#f66;padding:4px 8px;border-radius:6px;cursor:pointer;margin-left:auto"><i class="ph ph-x"></i></button>
    <span id="dbgCount" style="margin-left:6px;color:#9beec6;font-size:11px"></span>
  `;
  dbg.appendChild(header);

  const list = document.createElement('div');
  list.id = 'debugList';
  list.style.cssText = 'max-height:calc(40vh - 36px);overflow:auto;padding-right:6px;';
  dbg.appendChild(list);
  document.body.appendChild(dbg);

  // --- Floating toggle ---
  const floatBtn = document.createElement('button');
  floatBtn.id = 'vantalog-toggle';
  floatBtn.innerHTML = '<i class="ph ph-caret-up"></i>';
  floatBtn.style.cssText = `
    position: fixed;
    bottom: 16px;
    right: 16px;
    width: 46px;
    height: 46px;
    border-radius: 50%;
    background: rgba(0, 255, 170, 0.12);
    border: 1px solid #00ffaa55;
    color: #00ffaa;
    font-size: 24px;
    display: none;
    align-items: center;
    justify-content: center;
    cursor: pointer;
    z-index: 2147483648;
    backdrop-filter: blur(6px);
    transition: all 0.25s ease;
    box-shadow: 0 0 10px rgba(0,255,170,0.2);
  `;
  floatBtn.onmouseenter = () => (floatBtn.style.transform = 'scale(1.1)');
  floatBtn.onmouseleave = () => (floatBtn.style.transform = 'scale(1)');
  document.body.appendChild(floatBtn);

  // --- Logs + colors ---
  const logs = [];
  const colors = { log:'#66ffd2', info:'#00aaff', warn:'#ffcc00', error:'#ff5050', debug:'#ff66ff' };
  const glows = { log:'0 0 6px #66ffd2', info:'0 0 6px #00aaff', warn:'0 0 8px #ffcc00', error:'0 0 8px #ff5050', debug:'0 0 6px #ff66ff' };

  function trim() { while(logs.length>MAX_LINES) logs.shift(); }
  function updateCount(){ document.getElementById('dbgCount').textContent = `${logs.length} lines`; }
  function formatTime(d=new Date()){ return d.toISOString().replace('T',' ').replace('Z',''); }
  function escapeHtml(s){ return String(s).replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;'); }
  function getCircularReplacer(){ const seen=new WeakSet(); return (k,v)=>{ if(typeof v==='object'&&v!==null){ if(seen.has(v)) return '[Circular]'; seen.add(v);} return v; }; }

  function addLine(type,args){
    const time = formatTime();
    const text = args.map(a=>{ try{return typeof a==='string'?a:JSON.stringify(a,getCircularReplacer(),2);}catch{return String(a);}}).join(' ');
    const entry={time,type,text};
    logs.push(entry); trim(); updateCount();

    const row=document.createElement('div');
    row.style.cssText=`padding:6px 8px;border-radius:6px;margin-bottom:6px;background:rgba(0,0,0,0.2);color:${colors[type]};text-shadow:${glows[type]}`;
    row.dataset.type=type; row.dataset.text=text.toLowerCase();
    row.innerHTML=`<span style="color:${colors[type]}">[${time}]</span>
      <span style="color:${colors[type]};margin-left:8px;font-weight:600">${type}</span>
      <pre style="margin:6px 0 0 0;color:#dfffe9;white-space:pre-wrap">${escapeHtml(text)}</pre>`;
    list.appendChild(row);
    list.scrollTop=list.scrollHeight;
  }

  // --- Filter menu logic ---
  const filterBtn = document.getElementById('filterBtn');
  const filterMenu = document.getElementById('filterMenu');
  let activeFilter = 'all';

  filterBtn.onclick = () => {
    filterMenu.style.display = filterMenu.style.display==='none'?'block':'none';
  };

  document.querySelectorAll('.filterOption').forEach(opt => {
    opt.onclick = () => {
      activeFilter = opt.dataset.type;
      filterMenu.style.display='none';

      if (activeFilter === 'resources') {
        const res = performance.getEntriesByType('resource');
        list.innerHTML = `
          <div style="color:#00ffaa;font-weight:600;margin-bottom:6px;">Loaded Resources (${res.length})</div>
          ${res.map(r => `
            <div style="background:rgba(0,0,0,0.3);margin-bottom:6px;padding:6px 8px;border-radius:6px;color:#9beec6;font-size:11px;">
              <div><span style="color:#00ffaa;">[${r.initiatorType?.toUpperCase() || 'GEN'}]</span> ${r.name}</div>
              <small style="color:#aaa;">${(r.transferSize/1024).toFixed(1)} KB • ${Math.round(r.duration)} ms</small>
            </div>
          `).join('')}
        `;
        return;
      }

      Array.from(list.children).forEach(row => {
        row.style.display = (activeFilter==='all' || row.dataset.type===activeFilter) ? 'block':'none';
      });
    };
  });

  // --- Override console ---
  const original = {
    log:console.log.bind(console),
    info:console.info.bind(console),
    warn:console.warn.bind(console),
    error:console.error.bind(console),
    debug:console.debug?console.debug.bind(console):console.log.bind(console)
  };
  ['log','info','warn','error','debug'].forEach(level=>{
    console[level] = (...args)=>{ try{ addLine(level,args); }catch(e){ original.error('dbg addLine failed',e);} original[level](...args); };
  });

  // --- Capture fetch/XHR/errors ---
  window.addEventListener('error',e=>console.error('UncaughtError',e.error||e.message||e));
  window.addEventListener('unhandledrejection',e=>console.error('UnhandledRejection',e.reason||e));
  const _fetch=window.fetch;
  if(_fetch) window.fetch=async(...a)=>{
    const t0=performance.now(); console.log('fetch →',a[0],a[1]||{});
    try{ const r=await _fetch(...a); console.log(`fetch ← ${r.status} ${a[0]} (${Math.round(performance.now()-t0)}ms)`); return r; }
    catch(e){ console.error('fetch error',a[0],e); throw e; }
  };
  (function(){
    const XHR=window.XMLHttpRequest;
    if(!XHR) return;
    const o=XHR.prototype.open,s=XHR.prototype.send;
    XHR.prototype.open=function(m,u){ this._m=m; this._u=u; return o.apply(this,arguments); };
    XHR.prototype.send=function(b){
      const t=performance.now();
      this.addEventListener('loadend',()=>console.log(`XHR ${this._m} ${this._u} → ${this.status} (${Math.round(performance.now()-t)}ms)`));
      this.addEventListener('error',()=>console.error(`XHR ERROR ${this._m} ${this._u}`));
      return s.apply(this,arguments);
    };
  })();

  // --- Controls ---
  document.getElementById('dbgClear').onclick = ()=>{ logs.length=0; list.innerHTML=''; updateCount(); console.log('VantaLog cleared'); };
  document.getElementById('dbgDownload').onclick = ()=>{
    const blob=new Blob([JSON.stringify(logs,null,2)],{type:'application/json'});
    const url=URL.createObjectURL(blob);
    const a=document.createElement('a');
    a.href=url; a.download=`vantalog_${new Date().toISOString().replace(/[:.]/g,'_')}.json`;
    document.body.appendChild(a); a.click(); a.remove(); URL.revokeObjectURL(url);
  };
  document.getElementById('vantalogClose').onclick = ()=>{ dbg.style.display='none'; floatBtn.style.display='flex'; };
  floatBtn.onclick = ()=>{ dbg.style.display='block'; floatBtn.style.display='none'; };
  window.addEventListener('keydown',e=>{
    if(e.ctrlKey && e.shiftKey && e.code==='KeyD'){
      const hidden=dbg.style.display==='none';
      dbg.style.display=hidden?'block':'none';
      floatBtn.style.display=hidden?'none':'flex';
      console.log('VantaLog toggled',dbg.style.display);
    }
  });

  updateCount();
  console.log('VANTA LOG - Absorb Every Error');
})();
</script>
```
Once added, refresh your site — you’ll see the VANTA LOG panel pop up automatically.


---

## 📱 Why Android Devs Love It

Feature	Description

🧾 Live Log Overlay	Captures console.log, warn, error, debug, and info in real time
⚙️ Network Tracker	Monitors fetch() and XMLHttpRequest calls with response times
🚨 Error Catcher	Captures window.error and unhandledrejection automatically
🌈 Log Filters	Instantly switch between All / Warning / Error / Info / Debug / Resources
💾 Export Logs	Download all captured logs as .json
🧼 Clear Console	One-tap log clear with built-in counter
🎨 Neon Aesthetic UI	Matte glassmorphism overlay with phosphor icons
📱 Mobile-Friendly	Works perfectly in mobile browsers, Termux, and Pydroid
🪄 Hotkey Access	Ctrl + Shift + D toggles visibility
💎 Self-contained	No frameworks, no external dependencies


---

## 🧰 Keyboard Shortcuts

Shortcut	Action

Ctrl + Shift + D	Toggle VANTA LOG visibility
ESC	(Coming Soon) Quick close
⇧ Hover Button	Click to reopen when hidden



---

## 💾 Exported File Format

When you click the Download button, you’ll get a .json file named like:
```
vantalog_2025-11-11_12_45_02.json
```
Each entry looks like:
```
{
  "time": "2025-11-11 12:45:02",
  "type": "error",
  "text": "Uncaught ReferenceError: foo is not defined"
}
```
---

## 🧠 Developer Notes

Works offline, no dependencies.

Designed with Phosphor Icons for clean, scalable visuals.

Compatible with any frontend framework (React, Vue, Svelte, Vanilla JS).

Built for debugging on-the-go in mobile environments.



---

## 🛠️ Upcoming Features (Stable Version)

[ ] Persistent logs using localStorage

[ ] Search bar & live keyword filtering

[ ] Scroll-lock toggle

[ ] Pause / Resume logging

[ ] Collapsible log groups

[ ] Compact/verbose mode

[ ] Multi-theme support

[ ] Remote log streaming (dev dashboards)



---

## ⚠️ Project Status

🚧 UNDER CONSTRUCTION — Stable release coming soon.
The current build is Beta / Experimental, but already functional and stable enough for daily debugging.

> 💡 Want to contribute? Fork it, enhance the UI or performance, and send a PR.




---

## 💚 License

Licensed under the MIT License — you’re free to use, modify, and distribute it.


---

<p align="center">
  <img src="https://img.shields.io/badge/Built%20With-JavaScript-00ffaa?style=for-the-badge&logo=javascript" alt="JS">
  <img src="https://img.shields.io/badge/Frontend-Neon%20Glassmorphism-0a0c12?style=for-the-badge" alt="Theme">
  <img src="https://img.shields.io/badge/Made%20By-YOCRRZ%20%F0%9F%94%A5-00ffaa?style=for-the-badge" alt="Made by YOCRRZ">
</p><p align="center">
  <sub>✨ “Plug it in, light it up, and debug like a legend.” ✨</sub>
</p>
```
---
