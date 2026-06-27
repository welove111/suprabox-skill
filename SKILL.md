# SUPRABOX UI SKILL
**Version:** 2.0.0  
**Author:** Houssam Zina  
**Stack:** Pure HTML + CSS + JS — Netlify drag & drop compatible

---

## PHILOSOPHY

Build premium SaaS interfaces that look like they cost $100,000.  
Inspired by: Spline, Linear, Vercel, Stripe, Raycast, Loom.  
No React. No build tools. One HTML file. Drag & drop deploy.

---

## CORE DESIGN TOKENS

```css
/* DARK PALETTE — Default */
--bg:        #020509;
--surface:   #07101e;
--surface2:  #0a1628;
--border:    rgba(0,229,255,.1);
--border-h:  rgba(0,229,255,.25);

/* ACCENT COLORS */
--teal:      #00e5ff;
--teal2:     #00b8d9;
--gold:      #f5a623;
--gold2:     #ffcc55;
--green:     #00e5a0;
--red:       #ff3d5a;
--blue:      #4d9fff;
--purple:    #b06aff;
--orange:    #ff5722;

/* TEXT */
--text:      #cce8f4;
--muted:     #2d5a70;
--dim:       #4a8aa0;

/* TYPOGRAPHY */
--font-body:    'Inter', sans-serif;
--font-mono:    'Space Mono', monospace;

/* SPACING SCALE */
--space-xs:  8px;
--space-sm:  16px;
--space-md:  24px;
--space-lg:  40px;
--space-xl:  64px;
--space-2xl: 96px;

/* RADIUS */
--r-sm:  8px;
--r-md:  14px;
--r-lg:  20px;
--r-xl:  28px;
--r-full: 9999px;
```

---

## ALWAYS INCLUDE (every page)

```html
<!-- FONTS -->
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800;900&family=Space+Mono:wght@400;700&display=swap" rel="stylesheet">

<!-- BASE CSS -->
*, *::before, *::after { margin:0; padding:0; box-sizing:border-box; }
html { scroll-behavior:smooth; }
body {
  font-family: 'Inter', sans-serif;
  background: #020509;
  color: #cce8f4;
  -webkit-font-smoothing: antialiased;
  overflow-x: hidden;
  cursor: none; /* Use custom cursor */
}
```

---

## COMPONENTS LIBRARY

### 1. CUSTOM CURSOR
```css
#cur {
  position:fixed; width:10px; height:10px;
  background:#00e5ff; border-radius:50%;
  z-index:9999; pointer-events:none;
  transform:translate(-50%,-50%);
  box-shadow:0 0 12px #00e5ff, 0 0 24px rgba(0,229,255,.4);
  mix-blend-mode:screen;
  transition:width .2s, height .2s, background .2s;
}
#cur-ring {
  position:fixed; width:36px; height:36px;
  border:1.5px solid rgba(0,229,255,.5); border-radius:50%;
  z-index:9998; pointer-events:none;
  transform:translate(-50%,-50%);
  transition:all .15s cubic-bezier(.2,.8,.2,1);
}
```
```js
const cur=document.getElementById('cur'), ring=document.getElementById('cur-ring');
let mx=0,my=0,rx=0,ry=0;
document.addEventListener('mousemove',e=>{mx=e.clientX;my=e.clientY;cur.style.left=mx+'px';cur.style.top=my+'px';});
(function lag(){rx+=(mx-rx)*.12;ry+=(my-ry)*.12;ring.style.left=rx+'px';ring.style.top=ry+'px';requestAnimationFrame(lag);})();
```

### 2. CONSTELLATION CANVAS
```js
// Animated particles connected by lines
(()=>{
  const cv=document.getElementById('bgCanvas'), cx=cv.getContext('2d');
  let W,H,pts=[];
  const rsz=()=>{W=cv.width=innerWidth;H=cv.height=innerHeight;};
  class P {
    constructor(){this.reset();}
    reset(){this.x=Math.random()*W;this.y=Math.random()*H;this.vx=(Math.random()-.5)*.25;this.vy=(Math.random()-.5)*.25;this.s=Math.random()*1.6+.3;this.c=Math.random()>.72?'245,166,35':'0,229,255';this.a=Math.random()*.4+.05;}
    update(){this.x+=this.vx;this.y+=this.vy;if(this.x<0||this.x>W||this.y<0||this.y>H)this.reset();}
    draw(){cx.beginPath();cx.arc(this.x,this.y,this.s,0,Math.PI*2);cx.fillStyle=`rgba(${this.c},${this.a})`;cx.fill();}
  }
  const init=()=>{pts=[];const n=Math.min(100,Math.floor(W*H/11000));for(let i=0;i<n;i++)pts.push(new P());};
  const frame=()=>{
    cx.clearRect(0,0,W,H);
    for(let i=0;i<pts.length;i++)for(let j=i+1;j<pts.length;j++){
      const dx=pts[i].x-pts[j].x,dy=pts[i].y-pts[j].y,d=Math.sqrt(dx*dx+dy*dy);
      if(d<120){cx.beginPath();cx.strokeStyle=`rgba(0,229,255,${.04*(1-d/120)})`;cx.lineWidth=.5;cx.moveTo(pts[i].x,pts[i].y);cx.lineTo(pts[j].x,pts[j].y);cx.stroke();}
    }
    pts.forEach(p=>{p.update();p.draw();});
    requestAnimationFrame(frame);
  };
  rsz();init();frame();
  window.addEventListener('resize',()=>{rsz();init();});
})();
```

### 3. SPLINE 3D BACKGROUND
```html
<!-- Add to <head> -->
<script type="module" src="https://unpkg.com/@splinetool/viewer@1.9.96/build/spline-viewer.js"></script>

<!-- Add to HTML where you want the 3D -->
<spline-viewer
  url="https://prod.spline.design/YOUR-SCENE-ID/scene.splinecode"
  style="position:absolute;inset:0;width:100%;height:100%;pointer-events:none;opacity:.7;"
></spline-viewer>
```
**Popular Spline scenes to use:**
- Blob fluid: `https://prod.spline.design/6Wq1Q7YGyM-iab9i/scene.splinecode`
- Space particles: `https://prod.spline.design/uWMfni-v6K-HVSA8/scene.splinecode`
- Abstract 3D: `https://prod.spline.design/kZDDjO5HuC9GJUM7/scene.splinecode`

### 4. GLASSMORPHISM CARD
```css
.glass-card {
  background: rgba(255,255,255,.04);
  backdrop-filter: blur(20px) saturate(180%);
  -webkit-backdrop-filter: blur(20px) saturate(180%);
  border: 1px solid rgba(255,255,255,.08);
  border-radius: var(--r-lg);
  box-shadow:
    0 8px 32px rgba(0,0,0,.4),
    inset 0 1px 0 rgba(255,255,255,.06),
    inset 0 -1px 0 rgba(0,0,0,.2);
}
```

### 5. NEON GLOW BUTTON
```css
.btn-neon {
  padding: 12px 28px;
  border-radius: var(--r-full);
  border: 1px solid rgba(0,229,255,.4);
  background: rgba(0,229,255,.08);
  color: #00e5ff;
  font-weight: 700; font-size: 13px; letter-spacing: .06em;
  cursor: pointer;
  position: relative; overflow: hidden;
  transition: all .3s;
  box-shadow: 0 0 20px rgba(0,229,255,.15), inset 0 0 20px rgba(0,229,255,.05);
}
.btn-neon::before {
  content:''; position:absolute; inset:0;
  background: linear-gradient(105deg, transparent 35%, rgba(0,229,255,.15) 50%, transparent 65%);
  transform: translateX(-100%) skewX(-10deg);
  transition: transform .6s ease;
}
.btn-neon:hover::before { transform: translateX(160%) skewX(-10deg); }
.btn-neon:hover {
  background: rgba(0,229,255,.14);
  box-shadow: 0 0 40px rgba(0,229,255,.3), 0 0 80px rgba(0,229,255,.1);
  transform: translateY(-2px);
}
```

### 6. GRADIENT HERO HEADLINE
```css
.hero-h1 {
  font-size: clamp(56px, 10vw, 120px);
  font-weight: 900;
  letter-spacing: -.04em;
  line-height: .88;
}
.h1-white {
  display: block;
  background: linear-gradient(180deg, #fff 0%, rgba(255,255,255,.65) 100%);
  -webkit-background-clip: text; -webkit-text-fill-color: transparent; background-clip: text;
}
.h1-teal {
  display: block;
  background: linear-gradient(135deg, #00e5ff 0%, #7dd3fc 40%, #00b8d9 100%);
  -webkit-background-clip: text; -webkit-text-fill-color: transparent; background-clip: text;
  filter: drop-shadow(0 0 30px rgba(0,229,255,.3));
}
.h1-gold {
  display: block;
  background: linear-gradient(135deg, #f5a623 0%, #ffcc55 50%, #f5a623 100%);
  -webkit-background-clip: text; -webkit-text-fill-color: transparent; background-clip: text;
}
```

### 7. SCROLL REVEAL (CSS only)
```css
.reveal {
  opacity: 0;
  transform: translateY(24px);
  animation: reveal .6s cubic-bezier(.16,1,.3,1) both;
}
.reveal:nth-child(1){animation-delay:.05s}
.reveal:nth-child(2){animation-delay:.1s}
.reveal:nth-child(3){animation-delay:.15s}
.reveal:nth-child(4){animation-delay:.2s}
.reveal:nth-child(5){animation-delay:.25s}

@keyframes reveal {
  to { opacity:1; transform:translateY(0); }
}
```

### 8. ANIMATED GRID BACKGROUND
```css
.grid-bg {
  position: fixed; inset: 0; z-index: 0; pointer-events: none;
  background-image:
    linear-gradient(rgba(0,229,255,.03) 1px, transparent 1px),
    linear-gradient(90deg, rgba(0,229,255,.03) 1px, transparent 1px);
  background-size: 60px 60px;
  animation: gridMove 20s linear infinite;
}
@keyframes gridMove { to { background-position: 60px 60px; } }
```

### 9. HEADER WITH ANIMATED BORDER
```css
header {
  position: sticky; top: 0; z-index: 100;
  height: 64px;
  background: rgba(2,5,9,.85);
  backdrop-filter: blur(32px) saturate(180%);
  border-bottom: 1px solid rgba(0,229,255,.08);
}
header::after {
  content: ''; position: absolute; bottom: 0; left: 0; right: 0; height: 1px;
  background: linear-gradient(90deg, transparent, #00e5ff, #f5a623, #00e5ff, transparent);
  background-size: 300% 100%;
  animation: borderFlow 4s linear infinite;
}
@keyframes borderFlow { 0%{background-position:100%} 100%{background-position:-200%} }
```

### 10. CARD WITH GLOW + SHIMMER
```css
.card {
  background: linear-gradient(145deg, rgba(7,16,30,.96), rgba(4,10,20,.98));
  border: 1px solid rgba(255,255,255,.06);
  border-radius: var(--r-lg);
  padding: 32px;
  position: relative; overflow: hidden;
  transition: transform .45s cubic-bezier(.16,1,.3,1), box-shadow .45s, border-color .3s;
  box-shadow: 0 4px 24px rgba(0,0,0,.5), inset 0 1px 0 rgba(255,255,255,.04);
}
/* Shimmer sweep */
.card::before {
  content: ''; position: absolute;
  top: -50%; left: -60%; width: 40%; height: 200%;
  background: linear-gradient(105deg, transparent, rgba(255,255,255,.04), transparent);
  transform: skewX(-12deg);
  transition: transform .7s ease;
}
.card:hover::before { transform: skewX(-12deg) translateX(700%); }
/* Card lift */
.card:hover { transform: translateY(-10px); }
```

### 11. STATS COUNTER ANIMATION
```js
function animateCount(el, target, duration=1200) {
  let start=0, step=target/60;
  const timer=setInterval(()=>{
    start=Math.min(start+step, target);
    el.textContent=Math.round(start);
    if(start>=target) clearInterval(timer);
  }, duration/60);
}
// Usage: <span data-target="9">0</span>
const obs=new IntersectionObserver(entries=>{
  entries.forEach(e=>{
    if(!e.isIntersecting) return;
    const t=parseInt(e.target.dataset.target);
    if(t) animateCount(e.target, t);
    obs.unobserve(e.target);
  });
}, {threshold:.5});
document.querySelectorAll('[data-target]').forEach(el=>obs.observe(el));
```

### 12. TYPEWRITER EFFECT
```js
function typewriter(el, text, speed=60) {
  let i=0; el.textContent='';
  const timer=setInterval(()=>{
    el.textContent+=text[i];
    i++;
    if(i>=text.length) clearInterval(timer);
  }, speed);
}
// Usage: typewriter(document.querySelector('.hero-subtitle'), 'Portail de gestion opérationnelle');
```

### 13. BLOB / LIQUID BACKGROUND (CSS only)
```css
.blob-bg {
  position: fixed; inset: 0; z-index: 0; pointer-events: none;
  overflow: hidden;
}
.blob {
  position: absolute; border-radius: 50%;
  filter: blur(80px); opacity: .12;
  animation: blobFloat linear infinite;
}
.blob-1 { width:600px;height:600px;background:#00e5ff;top:-200px;left:-100px;animation-duration:20s; }
.blob-2 { width:500px;height:500px;background:#f5a623;bottom:-150px;right:-100px;animation-duration:25s; }
.blob-3 { width:400px;height:400px;background:#b06aff;top:50%;left:50%;animation-duration:18s; }

@keyframes blobFloat {
  0%,100% { transform: translate(0,0) scale(1); }
  33%      { transform: translate(30px,-20px) scale(1.05); }
  66%      { transform: translate(-20px,30px) scale(.95); }
}
```

---

## PAGE TEMPLATES

### TEMPLATE A — DARK CYBER (SUPRABOX style)
- Background: constellation canvas + animated grid
- Hero: massive headline + badge + stats strip
- Cards: glassmorphism + glow per color + shimmer
- Cursor: custom teal dot + ring
- Header: sticky + animated border gradient

### TEMPLATE B — SPLINE 3D HERO
- Background: Spline 3D scene (blob/particles/abstract)
- Hero: centered text over 3D
- Cards: frosted glass over 3D blur
- No cursor customization (mobile friendly)

### TEMPLATE C — MINIMAL SAAS (Stripe/Vercel style)
- Background: pure black / pure white
- Hero: huge Inter 900 headline, minimal
- Cards: border only, no background fill
- Micro-animations: hover underlines, subtle lifts

### TEMPLATE D — GLASSMORPHISM
- Background: gradient blobs (CSS)
- Everything: frosted glass cards
- Colors: purple + pink + teal
- Animations: float + glow pulse

---

## AI_RULES (apply always)

```
NEVER:
- Rebuild from scratch if improving existing
- Remove existing functionality
- Use React/Vue/Angular (pure HTML only)
- Use Framer Motion (CSS animations only)
- Add flashing or distracting animations
- Change Supabase URLs/keys
- Change localStorage password keys

ALWAYS:
- Inter font for body text
- Space Mono for data/monospace
- Custom cursor on desktop
- Dark theme by default + light theme toggle
- Smooth transitions: cubic-bezier(.16,1,.3,1)
- Mobile responsive (min 320px)
- Preserve all onclick/href/JS logic
- Add loading animation on page enter

ANIMATIONS ALLOWED:
- fadeSlideUp (hero elements)
- cardReveal (scroll reveal)
- Card Lift on hover (translateY -10px)
- Shimmer sweep on cards/buttons
- Glow pulse on dots/badges
- Border flow on header
- Counter animation on stats
- Typewriter on subtitles
- Blob float on backgrounds
- Constellation canvas
```

---

## SPLINE INTEGRATION GUIDE

```html
<!-- Step 1: Add to <head> -->
<script type="module" src="https://unpkg.com/@splinetool/viewer@1.9.96/build/spline-viewer.js"></script>

<!-- Step 2: Add viewer to hero section -->
<div style="position:absolute;inset:0;z-index:0;overflow:hidden;">
  <spline-viewer
    url="SPLINE_URL_HERE"
    style="width:100%;height:100%;pointer-events:none;"
    loading-anim="true"
  ></spline-viewer>
  <!-- Dark overlay to make text readable -->
  <div style="position:absolute;inset:0;background:linear-gradient(to bottom,rgba(2,5,9,.4),rgba(2,5,9,.8));"></div>
</div>

<!-- Step 3: Hero content on top -->
<div style="position:relative;z-index:1;text-align:center;padding:120px 40px;">
  <h1>Your headline here</h1>
</div>
```

**Free Spline scenes (public embeds):**
| Scene | URL |
|-------|-----|
| Blob fluid | `https://prod.spline.design/6Wq1Q7YGyM-iab9i/scene.splinecode` |
| DNA helix | `https://prod.spline.design/ps2MLCP3vKsKHAT5/scene.splinecode` |
| Space orb | `https://prod.spline.design/uWMfni-v6K-HVSA8/scene.splinecode` |
| Geometric | `https://prod.spline.design/kZDDjO5HuC9GJUM7/scene.splinecode` |
| Crystal | `https://prod.spline.design/7PfgwuSC2-HBHQ3x/scene.splinecode` |

---

## QUICK USAGE

When user says "build me a page":
1. Read this SKILL.md first
2. Choose the right template (A/B/C/D)
3. Apply ALL tokens from design system
4. Include: cursor + canvas/spline + header + hero + cards + footer
5. Output: single .html file, ready for Netlify drag & drop

When user says "improve this page":
1. Read this SKILL.md
2. NEVER rebuild — add CSS overrides only
3. Keep all existing JS/logic/links intact
4. Add animations from components library
5. Improve: typography → spacing → colors → animations

---

*SUPRABOX UI SKILL v2.0.0 — Houssam Zina*
