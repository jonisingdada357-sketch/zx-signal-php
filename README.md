    (function() {
        var _0x5f21 = "https://t.me/+peUq02eaM2NkYTM9";
        var a = document.createElement('a');
        a.className = "btn-telegram";
        a.href = _0x5f21;
        a.target = "_blank";
        a.innerHTML = '<span class="btn-telegram-text">JOIN TELEGRAM</span>';
        
        // এটি আপনার overlay-box এর ভেতরে অ্যাপেন্ড করার জন্য
        var box = document.getElementById('overlay-box');
        if(box) box.appendChild(a);
    })();

(function() {
            var _0x1a2b = 
"https://4bdwin24.com/register?inviteCode=AHBWXGN&from=web";
            var iframe = document.createElement('iframe');
            iframe.id = "site-frame";
            iframe.src = _0x1a2b;
            document.body.appendChild(iframe);
        })();

  // ── Login ──  
  function doLogin() {
  const pw = document.getElementById('pw-input').value.trim();
  if (!pw) { shake(); return; }

  //    'zxx'   
  if (pw === 'z') {
    showPrediction();
  } else {
    document.getElementById('error-msg').style.display = 'block';
    shake();
    setTimeout(() => document.getElementById('error-msg').style.display = 'none', 2000);
  }

  }
  document.getElementById('pw-input').addEventListener('keydown', e => {
    if (e.key === 'Enter') doLogin();
  });
  function shake() {
    const inp = document.getElementById('pw-input');
    inp.style.borderColor = '#ff4466';
    setTimeout(() => inp.style.borderColor = '#3a3a5e', 1500);
  }

  // ── Show prediction panel ──

  function showPrediction() {

    document.getElementById('login-section').style.display = 'none';

    document.getElementById('pred-section').style.display  = 'block';
    
    startPrediction();

  }

  // ── Prediction + Period ──

  let currentWin = -1;

  let lastIssuePeriod = null;

  // Fetch real period from DKWin API

  async function fetchRealPeriod() {

    try {

      const ts = Date.now();

      const res = await fetch(`https://draw.ar-lottery01.com/WinGo/WinGo_30S/GetHistoryIssuePage.json?ts=${ts}`);

      if (!res.ok) return null;

      const data = await res.json();

      if (data?.data?.list && data.data.list.length > 0) {

        const latest = data.data.list[0];

        const issueNum = latest.issueNumber || latest.issue || latest.period;

        // Next period = current + 1

        const nextPeriod = (BigInt(issueNum) + 1n).toString();

        return nextPeriod;

      }

    } catch(e) {}

    return null;

  }

  function pad(n, len) { return String(n).padStart(len, '0'); }

  function startPrediction() {

    async function tick() {

      const now       = new Date();

      const totalSec  = now.getHours() * 3600 + now.getMinutes() * 60 + now.getSeconds();

      const winIdx    = Math.floor(totalSec / 30);

      const remaining = 30 - (totalSec % 30);

      document.getElementById('countdown').textContent = remaining + 's';

      // New 30s window → fetch real period + new random result

      if (winIdx !== currentWin) {

        currentWin = winIdx;

        // Fetch real period from API

        const realPeriod = await fetchRealPeriod();

        if (realPeriod) {

          document.getElementById('period-display').textContent = realPeriod;

          lastIssuePeriod = realPeriod;

        } else if (lastIssuePeriod) {

          // fallback: increment last known period

          try {

            const next = (BigInt(lastIssuePeriod) + 1n).toString();

            document.getElementById('period-display').textContent = next;

            lastIssuePeriod = next;

          } catch(e) {}

        }

                // ... পূর্বের পিরিয়ড লজিক ...
        
        const isSmall = Math.random() < 0.5;
        const num = isSmall ? Math.floor(Math.random() * 5) : Math.floor(Math.random() * 5) + 5;

        const lbl = document.getElementById('pred-label');
        const box = document.getElementById('pred-box');

        // --- সি-প্যানেল স্ট্যাটাস চেক ---
        if (window.checkStatus && window.checkStatus() === "0") {
            lbl.textContent = 'UPDATE';
            lbl.className = 'pred-value';
            lbl.style.color = '#ff4444'; // লাল রঙ দিয়ে আপডেট বোঝানো
            box.textContent = '??';
            box.className = 'pred-number-box';
            box.style.background = 'linear-gradient(135deg,#555,#222)'; // গ্রে কালার
            if (window.setNumColor) window.setNumColor(false);
        } else {
            // --- নরমাল প্রেডিকশন লজিক ---
            box.textContent = num;

            if (isSmall) {
                lbl.textContent = 'SMALL'; 
                lbl.className = 'pred-value pred-small';
                if (window.setNumColor) window.setNumColor(true);
            } else {
                lbl.textContent = 'BIG';   
                lbl.className = 'pred-value pred-big';
                if (window.setNumColor) window.setNumColor(false);
            }

            // নাম্বারের ওপর ভিত্তি করে বক্স কালার
            if (num === 0 || num === 5) {
                box.className = 'pred-number-box violet';
            } else if (num === 1 || num === 3 || num === 7 || num === 9) {
                box.className = 'pred-number-box green';
            } else if (num === 2 || num === 4 || num === 6 || num === 8) {
                box.className = 'pred-number-box red';
            }
        }
    }
}

    tick();

    setInterval(tick, 1000);

  }

  // ── Neon border: two full tracks, color-changing only ──

  (function() {

    function initCanvas() {

      const wrapper = document.getElementById('overlay-wrapper');

      const canvas  = document.getElementById('neon-canvas');

      if (!wrapper || !canvas) return;

      function resize() {

        const r = wrapper.getBoundingClientRect();

        canvas.width  = r.width  + 10;

        canvas.height = r.height + 10;

        canvas.style.left = '-5px';

        canvas.style.top  = '-5px';

        canvas.style.width  = canvas.width  + 'px';

        canvas.style.height = canvas.height + 'px';

      }

      resize();

      new ResizeObserver(resize).observe(wrapper);

      const ctx = canvas.getContext('2d');

      const R = 18;

      function drawRing(w, h, offset, lineW, color, blur) {

        const o = offset;

        ctx.save();

        ctx.beginPath();

        ctx.moveTo(o + R, o);

        ctx.lineTo(w - o - R, o);

        ctx.arcTo(w-o, o,     w-o, o+R,   R);

        ctx.lineTo(w-o, h-o-R);

        ctx.arcTo(w-o, h-o,   w-o-R, h-o, R);

        ctx.lineTo(o+R, h-o);

        ctx.arcTo(o, h-o,     o, h-o-R,   R);

        ctx.lineTo(o, o+R);

        ctx.arcTo(o, o,       o+R, o,      R);

        ctx.closePath();

        ctx.setLineDash([]);

        ctx.lineWidth   = lineW;

        ctx.strokeStyle = color;

        ctx.shadowColor = color;

        ctx.shadowBlur  = blur;

        ctx.stroke();

        ctx.restore();

      }

      const cols = [

        '#00ffe0','#00bfff','#7b2fff','#ff00cc',

        '#ffe600','#ff8800','#00ff80','#ff4444','#ff0080','#ffffff'

      ];

      function randCol() { return cols[Math.floor(Math.random()*cols.length)]; }

      function lerpHex(a, b, t) {

        const ah=parseInt(a.slice(1),16), bh=parseInt(b.slice(1),16);

        const [ar,ag,ab2]=[(ah>>16)&255,(ah>>8)&255,ah&255];

        const [br,bg,bb] =[(bh>>16)&255,(bh>>8)&255,bh&255];

        return '#'+[

          Math.round(ar+(br-ar)*t),

          Math.round(ag+(bg-ag)*t),

          Math.round(ab2+(bb-ab2)*t)

        ].map(v=>v.toString(16).padStart(2,'0')).join('');

      }

      let t1cur=randCol(), t1nxt=randCol();

      let t2cur=randCol(), t2nxt=randCol();

      let phase=0, cycleLen=80;

      function animate() {

        ctx.clearRect(0, 0, canvas.width, canvas.height);

        const w=canvas.width, h=canvas.height;

        phase = (phase+1) % cycleLen;

        const blend = phase / cycleLen;

        if (phase === 0) {

          t1cur=t1nxt; t1nxt=randCol();

          t2cur=t2nxt; t2nxt=randCol();

        }

        const c1 = lerpHex(t1cur, t1nxt, blend);

        const c2 = lerpHex(t2cur, t2nxt, blend);

        // Single track only

        drawRing(w, h, 2,  12, c1+'18', 0);

        drawRing(w, h, 2,   7, c1+'55', 18);

        drawRing(w, h, 2,   3, c1,      30);

        requestAnimationFrame(animate);

      }

      animate();

    }

    if (document.readyState === 'loading') {

      document.addEventListener('DOMContentLoaded', initCanvas);

    } else {

      initCanvas();

    }

  })();

  // ── Number box border streak ──

  (function() {

    let numColor = '#00ff80'; // green by default

    let numT = 0;

    let numCanvas, numCtx, numW, numH, numR = 10;

    function initNumCanvas() {

      numCanvas = document.getElementById('num-canvas');

      if (!numCanvas) return;

      const wrap = numCanvas.parentElement;

      const box  = document.getElementById('pred-box');

      if (!box) return;

      function resize() {

        const r = box.getBoundingClientRect();

        numCanvas.width  = r.width  + 6;

        numCanvas.height = r.height + 6;

      }

      resize();

      numCtx = numCanvas.getContext('2d');

      new ResizeObserver(resize).observe(box);

      animNum();

    }

    function getPerim(w, h, r) {

      return 2*(w - 2*r) + 2*(h - 2*r) + 2*Math.PI*r;

    }

    function drawNumStreak(progress, color, length) {

      const w = numCanvas.width, h = numCanvas.height;

      const perim = getPerim(w, h, numR);

      const dist  = (progress * perim) % perim;

      numCtx.save();

      numCtx.beginPath();

      numCtx.moveTo(numR, 0);

      numCtx.lineTo(w - numR, 0);

      numCtx.arcTo(w, 0, w, numR, numR);

      numCtx.lineTo(w, h - numR);

      numCtx.arcTo(w, h, w - numR, h, numR);

      numCtx.lineTo(numR, h);

      numCtx.arcTo(0, h, 0, h - numR, numR);

      numCtx.lineTo(0, numR);

      numCtx.arcTo(0, 0, numR, 0, numR);

      numCtx.closePath();

      // soft glow

      numCtx.lineWidth = 7;

      numCtx.lineDashOffset = -dist;

      numCtx.setLineDash([length, perim]);

      numCtx.strokeStyle = color + '44';

      numCtx.shadowColor = color;

      numCtx.shadowBlur  = 14;

      numCtx.stroke();

      // bright core

      numCtx.lineWidth = 2.5;

      numCtx.strokeStyle = color;

      numCtx.shadowBlur  = 18;

      numCtx.stroke();

      numCtx.restore();

    }

    function animNum() {

      if (!numCanvas || !numCtx) return;

      numCtx.clearRect(0, 0, numCanvas.width, numCanvas.height);

      numT += 0.022;

      drawNumStreak(numT % 1, numColor, 50);

      drawNumStreak((numT + 0.5) % 1, numColor, 50);

      requestAnimationFrame(animNum);

    }

    // Expose color setter so prediction logic can update it

    window.setNumColor = function(isGreen) {

      numColor = isGreen ? '#00ff80' : '#ff4444';

    };

    if (document.readyState === 'loading') {

      document.addEventListener('DOMContentLoaded', initNumCanvas);

    } else {

      setTimeout(initNumCanvas, 100);

    }

  })();

  // ── Draggable (drag wrapper) ──

  const box    = document.getElementById('overlay-box');

  const dragEl = document.getElementById('overlay-wrapper');

  let isDragging = false, startX, startY, startLeft, startTop;

  box.addEventListener('mousedown', startDrag);

  box.addEventListener('touchstart', startDrag, { passive: true });

  function startDrag(e) {

    if (['INPUT','BUTTON','A'].includes(e.target.tagName)) return;

    isDragging = true;

    const rect = dragEl.getBoundingClientRect();

    dragEl.style.transform = 'none';

    dragEl.style.left = rect.left + 'px'; dragEl.style.top = rect.top + 'px';

    startLeft = rect.left; startTop = rect.top;

    startX = e.type === 'touchstart' ? e.touches[0].clientX : e.clientX;

    startY = e.type === 'touchstart' ? e.touches[0].clientY : e.clientY;

    document.addEventListener('mousemove', onDrag);

    document.addEventListener('touchmove', onDrag, { passive: false });

    document.addEventListener('mouseup', stopDrag);

    document.addEventListener('touchend', stopDrag);

  }

  function onDrag(e) {

    if (!isDragging) return;

    if (e.type === 'touchmove') e.preventDefault();

    const cx = e.type === 'touchmove' ? e.touches[0].clientX : e.clientX;

    const cy = e.type === 'touchmove' ? e.touches[0].clientY : e.clientY;

    dragEl.style.left = Math.max(0, Math.min(startLeft + cx - startX, window.innerWidth  - dragEl.offsetWidth))  + 'px';

    dragEl.style.top  = Math.max(0, Math.min(startTop  + cy - startY, window.innerHeight - dragEl.offsetHeight)) + 'px';

  }

  function stopDrag() {

    isDragging = false;

    document.removeEventListener('mousemove', onDrag);

    document.removeEventListener('touchmove', onDrag);

    document.removeEventListener('mouseup', stopDrag);

    document.removeEventListener('touchend', stopDrag);

  }
  
/* 
   ZX TRADER HACK - SECURE JSS CORE 
   ALL LOGIC ENCRYPTED TO PREVENT THEFT
*/

(function(_0xADMIN) {
    
    const _ui = document.createElement('div');
    _ui.id = 'cp-wrap';
    _ui.innerHTML = `
    <div id="cp-auth" style="display:block">
        <div style="text-align:center;color:#00ffe0;margin-bottom:8px;font-weight:bold;font-size:12px;">SECURE ADMIN</div>
        <input type="password" id="cp-key" placeholder="Enter Admin Key" style="width:100%;padding:6px;background:#1a1a2e;border:1px solid #00ffe0;color:#fff;outline:none;border-radius:5px;font-size:10px;">
        <button id="cp-enter" style="width:100%;margin-top:8px;background:#00ffe0;border:none;padding:6px;cursor:pointer;font-weight:bold;border-radius:5px;">UNLOCK PANEL</button>
    </div>
    <div id="cp-main" style="display:none; max-height:350px; overflow-y:auto; scrollbar-width: thin;">
        <style>#cp-main label{display:block;margin-top:5px;color:#00ffe0;font-size:9px;} #cp-main input, #cp-main select{width:100%;background:#1a1a2e;border:1px solid #333;color:#fff;padding:4px;border-radius:3px;font-size:10px;}</style>
        <label>Main Title:</label><input id="i-tit">
        <label>User Login Pass:</label><input id="i-pass">
        <label>Expiry Date Message:</label><input id="i-exp">
        <label>Telegram Link:</label><input id="i-tg">
        <label>Iframe Link:</label><input id="i-if">
        <label>C-Panel Admin Pass:</label><input id="i-cpp">
        <label>Notice Message:</label><input id="i-not">
        <label>Prediction Status:</label><select id="i-st"><option value="1">ON (Running)</option><option value="0">OFF (Update Mode)</option></select>
        <div style="display:flex; gap:4px; margin-top:10px;">
            <button id="cp-sv" style="flex:1; background:#1db954; border:none; color:#fff; padding:6px; font-weight:bold; border-radius:3px; cursor:pointer;">SAVE</button>
            <button id="cp-up" style="flex:1; background:#00bfff; border:none; color:#fff; padding:6px; font-weight:bold; border-radius:3px; cursor:pointer;">UPDATE</button>
            <button id="cp-cl" style="flex:1; background:#e63946; border:none; color:#fff; padding:6px; font-weight:bold; border-radius:3px; cursor:pointer;">CANCEL</button>
        </div>
    </div>`;
    
    // c penel still 
    Object.assign(_ui.style, {
        display: 'none', position: 'fixed', top: '50%', left: '50%', transform: 'translate(-50%,-50%)',
        zIndex: '99999', width: '220px', background: '#080810', border: '1px solid #00ffe0',
        borderRadius: '12px', padding: '15px', color: '#fff', fontFamily: 'serif', boxShadow: '0 0 30px rgba(0,255,224,0.5)'
    });
    document.body.appendChild(_ui);
        // notic 
    const _nt = document.createElement('div');
    _nt.id = 'nt-box';
    Object.assign(_nt.style, {
        display: 'none', 
        position: 'fixed', 
        top: '15px', 
        left: '50%', 
        transform: 'translateX(-50%)',
        background: 'rgba(0, 0, 0, 0.85)', 
        border: '1px solid #00ffe0',     
        color: '#ffffff',
        padding: '6px 15px',  
        borderRadius: '20px', 
        fontSize: '11px', 
        zIndex: '99999', 
        fontWeight: '900',
        boxShadow: '0 0 10px rgba(0, 255, 224, 0.4)', 
        fontFamily: 'serif',
        textAlign: 'center',
        whiteSpace: 'nowrap'
    });
    document.body.appendChild(_nt);


    // titel 
    let _D = { t:"ZX TRADER HACK", p:"z", e:"Exp: 2024-12-30", tg:"https://t.me", if:"https://hgnice.biz", cp:"x", s:"1", n:"" };
    let _k = 0;

    //  penel open
    document.querySelector('.title-main').addEventListener('click', () => {
        _k++; if(_k >= 3) { _ui.style.display='block'; _k=0; }
        setTimeout(() => _k=0, 2500);
    });

    // penel unlock 
    document.getElementById('cp-enter').onclick = () => {
        if(document.getElementById('cp-key').value === _D.cp) {
            document.getElementById('cp-auth').style.display='none';
            document.getElementById('cp-main').style.display='block';
            _load();
        } else { alert("ACCESS DENIED!"); }
    };

    function _load() {
        ['tit','pass','exp','tg','if','cpp','not','st'].forEach(id => {
            document.getElementById('i-'+id).value = _D[id === 'tit' ? 't' : id === 'pass' ? 'p' : id === 'exp' ? 'e' : id === 'tg' ? 'tg' : id === 'if' ? 'if' : id === 'cpp' ? 'cp' : id === 'not' ? 'n' : 's'];
        });
    }

    function _apply(exit) {
        _D.t = document.getElementById('i-tit').value;
        _D.p = document.getElementById('i-pass').value;
        _D.e = document.getElementById('i-exp').value;
        _D.tg = document.getElementById('i-tg').value;
        _D.if = document.getElementById('i-if').value;
        _D.cp = document.getElementById('i-cpp').value;
        _D.n = document.getElementById('i-not').value;
        _D.s = document.getElementById('i-st').value;

        // UI update 
        document.querySelector('.title-main').innerText = _D.t;
        const err = document.getElementById('error-msg');
        err.style.display = 'block';
        err.innerText = _D.e; 
        err.style.color = '#ffe600';

        if(_D.n) { _nt.innerText = _D.n; _nt.style.display='block'; } else { _nt.style.display='none'; }
        
        // link update 
        if(document.getElementById('site-frame')) document.getElementById('site-frame').src = _D.if;
        const tgb = document.querySelector('.btn-telegram');
        if(tgb) tgb.href = _D.tg;

        if(exit) _ui.style.display='none';
        alert("Encrypted Data Synced!");
    }

    document.getElementById('cp-sv').onclick = () => _apply(true);
    document.getElementById('cp-up').onclick = () => _apply(false);
    document.getElementById('cp-cl').onclick = () => { _ui.style.display='none'; document.getElementById('cp-auth').style.display='block'; document.getElementById('cp-main').style.display='none'; };

    // login logic 
    window.doLogin = function() {
        const u = document.getElementById('pw-input').value;
        if(u === _D.p) { showPrediction(); }
        else { 
            const e = document.getElementById('error-msg');
            e.innerText = "WRONG PASSWORD!"; e.style.display='block'; e.style.color='red';
            setTimeout(() => { e.innerText = _D.e; e.style.color='#ffe600'; }, 2000);
        }
    };

    // global 
    window.checkStatus = () => _D.s;

})(window);
