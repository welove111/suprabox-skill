# 🎨 SUPRABOX UI SKILL
**Premium SaaS Interface Builder — Pure HTML, Netlify Ready**

> Build stunning interfaces that look like they're built by a $100k design team.  
> Inspired by: Spline · Linear · Vercel · Stripe · Raycast

---

## 📁 Structure

```
suprabox-skill/
├── SKILL.md                    ← Claude reads this first (brain of the skill)
├── README.md                   ← This file
│
├── templates/
│   ├── dark-cyber.html         ← Template A: Dark + Teal + Gold + Constellation
│   ├── spline-3d-hero.html     ← Template B: Spline 3D background + Glass
│   └── glassmorphism.html      ← Template D: Blob + Frosted glass + Purple/Cyan
│
├── components/                 ← Copy-paste components
│   └── (see SKILL.md for all components)
│
└── examples/
    └── suprabox-demo.html      ← Live example (SUPRABOX portal)
```

---

## 🚀 How to Use with Claude

### Tell Claude:
```
"Read /mnt/skills/user/suprabox-skill/SKILL.md then build me a [page type]"
```

### Examples:
```
"Read the skill then build me a dark cyber hero page for a crypto dashboard"

"Read the skill then use Template B (Spline 3D) for a landing page about logistics"

"Read the skill then improve this existing page without breaking any functions"
```

---

## 🎨 Templates

| Template | Style | Best For |
|----------|-------|----------|
| `dark-cyber.html` | Dark + Teal + Gold + Particles | Dashboards, Portals, Admin |
| `spline-3d-hero.html` | 3D background + Clean | Landing pages, SaaS |
| `glassmorphism.html` | Blobs + Frosted glass | Modern apps, Portfolios |

---

## ⚡ Deploy

All templates are **single HTML files** — deploy instantly:

1. Download the HTML file
2. Go to [app.netlify.com](https://app.netlify.com)
3. Drag & drop the file
4. Done — live in 30 seconds

---

## 🔧 Spline Integration

Get free 3D scenes from [spline.design](https://spline.design):
1. Open a public scene
2. Click **Share** → **Embed**
3. Copy the `.splinecode` URL
4. Paste in `spline-3d-hero.html` where it says `SPLINE_URL`

**Free public scenes:**
- Blob fluid: `https://prod.spline.design/6Wq1Q7YGyM-iab9i/scene.splinecode`
- Space orb: `https://prod.spline.design/uWMfni-v6K-HVSA8/scene.splinecode`
- Geometric: `https://prod.spline.design/kZDDjO5HuC9GJUM7/scene.splinecode`
- Crystal: `https://prod.spline.design/7PfgwuSC2-HBHQ3x/scene.splinecode`

---

## 📐 Design Tokens

```
Colors:    Dark (#020509) · Teal (#00e5ff) · Gold (#f5a623) · Green (#00e5a0)
Fonts:     Inter (body) · Space Mono (data/mono)
Radius:    8px · 14px · 20px · 28px · 9999px
Spacing:   8 · 16 · 24 · 40 · 64 · 96px
Easing:    cubic-bezier(.16, 1, .3, 1)
```

---

## 📋 AI Rules (for Claude)

```
✅ ALWAYS: Inter font, custom cursor, dark default, smooth easing
✅ ALWAYS: mobile responsive, loader animation, constellation/blobs
❌ NEVER: React/Vue, Framer Motion, rebuild existing pages
❌ NEVER: Flashing animations, change Supabase keys, remove JS logic
```

---

*Built by Houssam Zina · Tassila Messageries · Agadir, Maroc*
