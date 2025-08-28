<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Monthsary — Scrapbook</title>
  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600&family=Indie+Flower&display=swap" rel="stylesheet">
  <style>
    :root{
      --accent:#8b5cf6;
      --accent-2:#f472b6;
      --paper:#fbf6ef;
      --muted:#6b7280;
    }
    *{box-sizing:border-box}
    body{
      margin:0;
      font-family: 'Poppins', system-ui, -apple-system, 'Segoe UI', Roboto, Arial;
      background: linear-gradient(180deg,#efe7dd 0%, #f9f6f2 100%);
      min-height:100vh;
      display:flex;
      align-items:center;
      justify-content:center;
      padding:40px;
    }

    /* scrapbook paper background */
    .scene{
      width:100%;
      max-width:820px;
      background: repeating-linear-gradient(0deg, rgba(0,0,0,0.02) 0px, rgba(0,0,0,0.02) 1px, transparent 1px, transparent 30px),
                  linear-gradient(180deg,#fffdf9 0%, #fff8f3 100%);
      border-radius:14px;
      padding:34px;
      box-shadow: 0 12px 40px rgba(17,24,39,0.12);
      border:1px solid rgba(0,0,0,0.04);
      position:relative;
      overflow:hidden;
    }

    header{display:flex;align-items:center;gap:12px}
    .logo{width:54px;height:54px;border-radius:10px;background:linear-gradient(135deg,var(--accent),var(--accent-2));display:flex;align-items:center;justify-content:center;color:#fff;font-weight:700;box-shadow:0 8px 26px rgba(139,92,246,0.16)}
    h1{margin:0;font-size:20px;color:#4c1d95}
    p.lead{margin:6px 0 0;color:var(--muted);font-size:13px}

    /* Lock */
    .lock{display:flex;gap:12px;align-items:center;margin-top:18px}
    input[type=date]{padding:10px 12px;border-radius:8px;border:1px dashed rgba(0,0,0,0.06);background:linear-gradient(180deg,#fff,#fff);outline:none}
    .btn{padding:10px 14px;border-radius:10px;background:linear-gradient(90deg,var(--accent),var(--accent-2));color:white;border:none;cursor:pointer;box-shadow:0 10px 26px rgba(139,92,246,0.12);font-weight:600}
    .btn.ghost{background:transparent;color:var(--accent);border:1px solid rgba(139,92,246,0.12);box-shadow:none}

    /* floating music button (sticker) */
    .music-toggle{position:fixed;right:20px;top:20px;z-index:999; width:56px;height:56px;border-radius:12px;border:none;display:flex;align-items:center;justify-content:center;cursor:pointer;background:linear-gradient(135deg,var(--accent),var(--accent-2));box-shadow:0 12px 30px rgba(0,0,0,0.12);color:#fff}

    /* scrapbook area */
    .paper-area{display:flex;flex-direction:column;gap:18px;margin-top:20px}

    /* polaroid style message card */
    .polaroid{
      width:100%;max-width:680px;margin:0 auto;background:var(--paper);border-radius:8px;padding:18px 18px 26px;border:1px solid rgba(0,0,0,0.04);box-shadow: 0 16px 40px rgba(17,24,39,0.08);position:relative;transform:rotate(-1deg);
    }
    .tape{position:absolute;left:32px;top:-12px;width:100px;height:24px;background:linear-gradient(90deg,#ffe9f0,#fff3c8);transform:rotate(-8deg);border-radius:4px;box-shadow:0 4px 10px rgba(0,0,0,0.08)}
    .tape.right{right:36px;left:auto;top:-10px;transform:rotate(6deg)}

    .polaroid .title{font-family:'Indie Flower', cursive;font-size:18px;color:#4c1d95;margin-bottom:8px}
    .message{white-space:pre-wrap;color:#222;line-height:1.7;font-size:15px;padding:8px 6px}

    /* small caption / date tag */
    .tag{position:absolute;right:14px;bottom:12px;font-size:12px;color:var(--muted)}

    /* scratch polaroid (placed after message, centered) */
    .scratch-polaroid{width:320px;padding:14px;background:#fff;border-radius:10px;border:8px solid #fff;box-shadow:0 12px 34px rgba(17,24,39,0.12);margin:18px auto 0;position:relative;display:flex;flex-direction:column;align-items:center}
    .scratch-polaroid .caption{font-family:'Indie Flower', cursive;color:#4c1d95;margin-bottom:8px}
    .scratch-container{position:relative;width:260px;height:260px;border-radius:8px}
    #hiddenImage{width:100%;height:100%;object-fit:cover;border-radius:6px}
    #scratchCanvas{position:absolute;inset:0;border-radius:6px;cursor:crosshair}

    /* decorative stickers */
    .sticker{position:absolute;width:64px;height:64px;background:linear-gradient(135deg,#fff,#ffe);border-radius:10px;padding:6px;display:flex;align-items:center;justify-content:center;box-shadow:0 8px 20px rgba(0,0,0,0.12)}
    .sticker.heart{left:18px;bottom:20px}
    .sticker.star{right:18px;bottom:30px}

    @media (max-width:720px){.scene{padding:20px}.polaroid{transform:none}}

  </style>
</head>
<body>

  <button class="music-toggle" id="musicToggle" aria-pressed="false" title="Play / Pause" onclick="toggleMusic()">
    <svg id="musicIcon" width="22" height="22" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path d="M5 3v18l15-9L5 3z" fill="white"/></svg>
  </button>

  <div class="scene" role="main">
    <header>
      <div class="logo">M</div>
      <div>
        <h1>Our Scrapbook</h1>
        <p class="lead">Unlock the page with our date — then scratch the little polaroid for the surprise 🎶</p>
      </div>
    </header>

    <!-- lock -->
    <div class="lock" id="page1">
      <input type="date" id="dateInput" aria-label="Enter monthsary date" />
      <button class="btn" onclick="unlock()">Unlock</button>
      <button class="btn ghost" onclick="fillSample()">Sample date</button>
      <p id="error" style="color:#ef4444;display:none;margin-left:8px">Wrong date — try again 💔</p>
    </div>

    <!-- scrapbook paper area -->
    <div id="page2" style="display:none" class="paper-area">

      <div class="polaroid">
        <div class="tape"></div>
        <div class="tape right"></div>
        <div class="title">elooo, el^</div>
        <div class="message" id="messagePreview">
            Happy monthsary sa atin. Alam ko I’ve made mistakes, alam ko I’ve hurt you, and I’ll never stop being sorry for that. Pero ngayon, gusto ko lang magsulat nang totoo, nang galing talaga sa puso ko, kasi monthsary natin ito at gusto kong maramdaman mo gaano kita kamahal at gaano ka kaimportante sa akin.
            
            Swerte ako kasi ikaw ang pumili sa’kin. Kung hindi mo ako kinausap nung umpisa, baka hindi nangyari lahat ng ito. Yung simpleng hi mo noon, naging simula ng pinaka espesyal na bagay sa buhay ko. Lagi kong naaalala na ikaw ang nagbigay ng “yes,” at dahil dun lagi akong thankful.
            
            Mahal kita hindi lang dahil girlfriend kita, kundi dahil sa mismong pagkatao mo. Yung sweetness mo na kahit simpleng message lang, ramdam na ramdam ko yung care. Yung innocence mo na rare na makita ngayon, at kahit overthinker ka, mahal ko rin yun kasi ibig sabihin nakikita mo yung mga bagay na hindi napapansin ng iba. Kahit yung tampo mo, may meaning, kasi pinapakita nun na may value yung feelings mo. Kahit nagiging cold ka minsan, naiintindihan ko, at mas lalo pa kitang gusto kapag bumabalik yung lambing mo.
            
            Yung “elooo, el^” mo, hindi lang greeting para sa akin. Parang maliit na mundo natin yun na walang ibang makakaintindi kundi tayo lang. Yung mga simpleng bagay na gusto mo tulad ng calamares, baked mac, pasta, ice cream, pati baking at traveling, lahat ng yun parang pieces na bumubuo kung sino ka. At dahil mahal kita, lahat ng yun naging mahalaga din para sa akin. Ang sarap isipin na kahit sa ganitong maliliit na bagay, nakikita ko kung gaano ka ka-unique at ka-special.
            
            Pinaka tumatak sa akin ay kung paano mo ako tiningnan bilang tao. Ikaw yung nagsabi ng mga bagay na hindi ko pa narinig sa iba, na rare ako, mabait, may halaga. Dahil sayo natutunan kong makakita ng worth sa sarili ko. Isa yun sa mga bagay na hinding hindi ko makakalimutan.
            
            Ngayong monthsary, hindi ako magbibigay ng empty promises. Pero sigurado ako dito: araw-araw kitang pipiliin. Hindi lang sa grand gestures, kundi sa simpleng effort na consistent. Hindi ko hahayaang maramdaman mo na parang napapabayaan ka lang. Nandito ako para alagaan ka, intindihin ka, at ipakita sa’yo na mahalaga ka. Kapag napapagod ka o nagdududa ka, gusto ko lang na bigyan mo ako ng konting chance para patunayan, hindi lang sa salita kundi sa gawa, na worth it pa rin tayong dalawa.
            
            Happy Monthsary, el^. Hindi lang dahil date ito pero dahil reminder siya na isang buwan na naman yung nalagpasan natin nang magkasama. Salamat kasi andiyan ka, salamat kasi ikaw yung nagbigay ng dahilan para maging masaya ako at para gusto kong maging mas mabuting tao.
            
            Mahal na mahal kita, sobra sobra. Lagi akong para sa’yo.</div>
        <div class="tag">– your baby, your happiest boy 🩵</div>
      </div>

      <!-- scratch polaroid placed after the message -->
      <div class="scratch-polaroid" role="region" aria-label="Scratch to reveal">
        <div class="caption">scratch to reveal the playlist</div>
        <div class="scratch-container">
          <img id="hiddenImage" src="frame.png" alt="Spotify QR" />
          <canvas id="scratchCanvas" width="260" height="260"></canvas>
        </div>
      </div>

      <!-- small stickers -->
      <div class="sticker heart">💕</div>
      <div class="sticker star">⭐</div>

    </div>
  </div>

  <!-- audio -->
  <audio id="bgMusic" loop preload="none"><source src="The 1975 - About You (Official).mp3" type="audio/mpeg"></audio>

  <script>
    const correctDate = "2025-07-28"; // put your monthsary date here
    const page1 = document.getElementById('page1');
    const page2 = document.getElementById('page2');

    function fillSample(){ document.getElementById('dateInput').value = correctDate; }

    function unlock(){
      const input = document.getElementById('dateInput').value;
      const error = document.getElementById('error');
      if(input === correctDate){
        page1.style.display = 'none';
        page2.style.display = 'flex';
        error.style.display = 'none';
        // attempt to play the background music (this is a user gesture so most browsers will allow it)
        const bg = document.getElementById('bgMusic');
        if(bg){
          const playPromise = bg.play();
          if(playPromise !== undefined){
            playPromise.catch(() => {
              // If play is blocked (rare since unlock is a click), do nothing — user can use the music toggle.
              // We could show a subtle hint or animate the toggle to invite a tap.
              musicToggleBtn.animate([{transform:'scale(1)'},{transform:'scale(1.06)'},{transform:'scale(1)'}],{duration:300});
            });
          }
        }
        setTimeout(initScratch, 100);
      } else { error.style.display = 'block'; }
    }

    // music toggle
    const music = document.getElementById('bgMusic');
    const musicIcon = document.getElementById('musicIcon');
    const musicToggleBtn = document.getElementById('musicToggle');
    function setIconPlaying(playing){
      if(playing){ musicIcon.innerHTML = '<rect x="6" y="5" width="4" height="14" rx="1" fill="white"></rect><rect x="14" y="5" width="4" height="14" rx="1" fill="white"></rect>'; musicToggleBtn.setAttribute('aria-pressed','true'); }
      else { musicIcon.innerHTML = '<path d="M5 3v18l15-9L5 3z" fill="white" />'; musicToggleBtn.setAttribute('aria-pressed','false'); }
    }
    music.addEventListener('play', ()=> setIconPlaying(true));
    music.addEventListener('pause', ()=> setIconPlaying(false));
    function toggleMusic(){ if(music.paused){ music.play().catch(()=> musicToggleBtn.animate([{transform:'scale(1)'},{transform:'scale(1.06)'},{transform:'scale(1)'}],{duration:300})); } else { music.pause(); } }
    setIconPlaying(false);

    // scratch logic
    let scratchInitialized = false;
    function initScratch(){
      if(scratchInitialized) return; scratchInitialized = true;
      const canvas = document.getElementById('scratchCanvas');
      const ctx = canvas.getContext('2d');
      // size for hi-dpi
      const width = canvas.clientWidth; const height = canvas.clientHeight; canvas.width = width; canvas.height = height;
      // overlay
      const g = ctx.createLinearGradient(0,0,0,canvas.height); g.addColorStop(0,'#eaeaea'); g.addColorStop(1,'#cfcfcf'); ctx.fillStyle = g; ctx.fillRect(0,0,canvas.width,canvas.height);
      ctx.globalCompositeOperation = 'destination-out';
      let scratching = false; const radius = Math.max(16, Math.round((canvas.width+canvas.height)/60));
      function getPos(e){ const r = canvas.getBoundingClientRect(); if(e.touches && e.touches[0]) return {x:e.touches[0].clientX-r.left,y:e.touches[0].clientY-r.top}; return {x:e.clientX-r.left,y:e.clientY-r.top}; }
      function drawDot(x,y){ ctx.beginPath(); ctx.arc(x,y,radius,0,Math.PI*2); ctx.fill(); }
      canvas.addEventListener('mousedown', ()=> scratching=true); canvas.addEventListener('mouseup', ()=> scratching=false); canvas.addEventListener('mouseleave', ()=> scratching=false);
      canvas.addEventListener('mousemove', (e)=>{ if(!scratching) return; const p=getPos(e); drawDot(p.x,p.y); });
      canvas.addEventListener('touchstart', (e)=>{ scratching=true; const p=getPos(e); drawDot(p.x,p.y); e.preventDefault(); },{passive:false});
      canvas.addEventListener('touchmove', (e)=>{ if(!scratching) return; const p=getPos(e); drawDot(p.x,p.y); e.preventDefault(); },{passive:false});
      canvas.addEventListener('touchend', ()=> scratching=false);
      // reveal auto-clear
      function revealAmount(){ try{ const img = ctx.getImageData(0,0,canvas.width,canvas.height); const pixels = img.data; let trans=0; for(let i=3;i<pixels.length;i+=4) if(pixels[i]===0) trans++; return (trans/(canvas.width*canvas.height))*100;}catch(e){return 0;} }
      const checker = setInterval(()=>{ const p=revealAmount(); if(p>55){ ctx.clearRect(0,0,canvas.width,canvas.height); clearInterval(checker);} },700);
    }

  </script>
</body>
</html>
