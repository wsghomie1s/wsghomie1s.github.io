<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover">
<title>XorwelOS</title>
<meta name="description" content="Xorwel — advanced all-in-one entertainment hub: games, cloud gaming, movies & anime, music, AI, chat, weather, and privacy proxy.">
<meta name="theme-color" content="#dc143c">
<meta name="color-scheme" content="dark">
<meta name="mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
<meta name="apple-mobile-web-app-title" content="Xorwel">
<meta property="og:title" content="Xorwel">
<meta property="og:description" content="Games, movies, music, AI and proxy — one sleek hub.">
<meta property="og:type" content="website">
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link rel="preconnect" href="https://api.themoviedb.org">
<link rel="preconnect" href="https://api.open-meteo.com">
<link rel="dns-prefetch" href="https://image.tmdb.org">

<!-- Ultraviolet -->
<script src="https://cdn.jsdelivr.net/npm/@titaniumnetwork-dev/ultraviolet@3.2.7/dist/uv.bundle.js"></script>
<script src="https://cdn.jsdelivr.net/npm/@titaniumnetwork-dev/ultraviolet@3.2.7/dist/uv.config.js"></script>

<!-- Scramjet -->
<script src="https://cdn.jsdelivr.net/npm/@mercuryworkshop/scramjet@1.0.1/dist/scramjet.all.js"></script>

<!-- BareMux (shared transport) -->
<script src="https://cdn.jsdelivr.net/npm/@mercuryworkshop/bare-mux@1.0.6/dist/index.js"></script>
<script src="https://cdn.jsdelivr.net/npm/@mercuryworkshop/epoxy-tls@1.0.6/dist/epoxy.js"></script>

<!-- Local AI -->
<!-- Transformers.js loaded dynamically by initAI() -->

<style>
@import url('https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@400;500;600;700&display=swap');

:root {
  --bg: #000000;
  --bg-soft: #0a0005;
  --surface: rgba(220,20,60,0.06);
  --surface-hover: rgba(220,20,60,0.14);
  --surface-strong: rgba(220,20,60,0.22);
  --surface-solid: #14000a;
  --surface-solid-2: #1a0010;
  --text: #ff4d6d;
  --text-bright: #ff8fa3;
  --text-dim: #b03a52;
  --text-muted: #7a2840;
  --accent: #dc143c;
  --accent-hover: #ff1a4a;
  --accent-bright: #ff2d55;
  --accent-glow: 0 0 28px rgba(220,20,60,0.65), 0 0 60px rgba(220,20,60,0.25);
  --accent-glow-sm: 0 0 14px rgba(220,20,60,0.5);
  --success: #ff2d55;
  --warning: #ff6b35;
  --danger: #b00020;
  --border: rgba(220,20,60,0.25);
  --border-strong: rgba(220,20,60,0.45);
  --border-faint: rgba(220,20,60,0.12);
  --radius-sm: 10px;
  --radius: 16px;
  --radius-lg: 24px;
  --radius-xl: 32px;
  --shadow-sm: 0 2px 10px rgba(0,0,0,0.55);
  --shadow: 0 10px 30px rgba(0,0,0,0.65);
  --shadow-lg: 0 20px 60px rgba(0,0,0,0.75);
  --sidebar-w: 270px;
  --transition: 0.28s cubic-bezier(0.4, 0, 0.2, 1);
  --transition-bounce: 0.55s cubic-bezier(0.34, 1.56, 0.64, 1);
  --accent-rgb: 220,20,60;
  --glass: rgba(20,0,10,0.55);
  --glass-border: rgba(255,255,255,0.06);
}

* { margin:0; padding:0; box-sizing:border-box; font-family:'Space Grotesk', sans-serif; -webkit-font-smoothing: antialiased; }

html, body { background: var(--bg); color: var(--text); min-height: 100vh; overflow-x: hidden; scroll-behavior: smooth; }

body {
  background:
    radial-gradient(ellipse 80% 60% at 10% 10%, rgba(220,20,60,0.14) 0%, transparent 55%),
    radial-gradient(ellipse 70% 50% at 90% 90%, rgba(180,10,50,0.10) 0%, transparent 55%),
    radial-gradient(circle at 50% 50%, rgba(40,0,15,0.4) 0%, transparent 70%),
    linear-gradient(180deg, #000000 0%, #0a0005 100%);
  background-attachment: fixed;
}

body::before {
  content: '';
  position: fixed;
  inset: 0;
  background: 
    repeating-linear-gradient(0deg, transparent 0px, transparent 2px, rgba(220,20,60,0.015) 2px, rgba(220,20,60,0.015) 3px),
    radial-gradient(circle at 30% 20%, rgba(255,45,85,0.04) 0%, transparent 40%),
    radial-gradient(circle at 70% 80%, rgba(220,20,60,0.05) 0%, transparent 45%);
  pointer-events: none;
  z-index: 1;
  opacity: 0.7;
  animation: auroraShift 18s ease-in-out infinite alternate;
}

@keyframes auroraShift {
  0% { opacity: 0.55; filter: hue-rotate(0deg); }
  50% { opacity: 0.8; filter: hue-rotate(8deg); }
  100% { opacity: 0.6; filter: hue-rotate(-5deg); }
}

body::after {
  content: '';
  position: fixed;
  inset: 0;
  background-image: 
    radial-gradient(1px 1px at 20% 30%, rgba(255,80,120,0.35), transparent),
    radial-gradient(1px 1px at 40% 70%, rgba(255,100,140,0.25), transparent),
    radial-gradient(1px 1px at 60% 20%, rgba(255,60,100,0.3), transparent),
    radial-gradient(1px 1px at 80% 50%, rgba(255,90,130,0.2), transparent),
    radial-gradient(1.5px 1.5px at 15% 80%, rgba(255,120,150,0.2), transparent),
    radial-gradient(1px 1px at 90% 15%, rgba(255,70,110,0.25), transparent);
  background-size: 100% 100%;
  pointer-events: none;
  z-index: 1;
  opacity: 0.4;
  animation: twinkle 8s ease-in-out infinite alternate;
}

@keyframes twinkle {
  0% { opacity: 0.25; }
  100% { opacity: 0.55; }
}

::-webkit-scrollbar { width: 12px; height: 12px; }
::-webkit-scrollbar-track { background: #000; border-left: 1px solid var(--border-faint); }
::-webkit-scrollbar-thumb { background: linear-gradient(180deg, var(--accent), var(--accent-bright)); border-radius: 6px; border: 3px solid #000; box-shadow: var(--accent-glow-sm); }
::-webkit-scrollbar-thumb:hover { background: var(--accent-bright); box-shadow: var(--accent-glow); }
::selection { background: var(--accent); color: #fff; }
*:focus-visible { outline: 2px solid var(--accent); outline-offset: 2px; border-radius: 4px; }

/* SIDEBAR */
.sidebar {
  position: fixed; left: 0; top: 0;
  width: var(--sidebar-w); height: 100vh;
  background: linear-gradient(185deg, #0f0008 0%, #050003 40%, #000000 100%);
  border-right: 1px solid var(--border);
  display: flex; flex-direction: column;
  padding: 22px 14px; z-index: 100;
  transition: transform 0.4s cubic-bezier(0.2, 0.9, 0.4, 1);
  overflow: hidden;
  box-shadow: 8px 0 40px rgba(0,0,0,0.5);
}
.sidebar::before { content:''; position:absolute; top:0; left:0; right:0; height:280px; background: radial-gradient(ellipse 120% 80% at top, rgba(220,20,60,0.28) 0%, transparent 70%); pointer-events:none; }
.sidebar::after { content:''; position:absolute; top:0; left:0; width:2px; height:100%; background: linear-gradient(180deg, transparent 5%, var(--accent) 30%, var(--accent-bright) 50%, var(--accent) 70%, transparent 95%); animation: sidebarPulse 3.5s ease-in-out infinite; box-shadow: 0 0 12px var(--accent); }
@keyframes sidebarPulse { 0%,100%{opacity:0.35; filter:brightness(0.9);} 50%{opacity:1; filter:brightness(1.3);} }
.sidebar-logo {
  font-size: 1.55rem; font-weight: 700; letter-spacing: 5px;
  text-transform: lowercase; text-align: center; padding: 18px 0 10px;
  background: linear-gradient(135deg, #ff8095 0%, #ff4d6d 30%, #dc143c 60%, #b00020 100%);
  -webkit-background-clip: text; -webkit-text-fill-color: transparent; background-clip: text;
  position: relative; z-index: 1; filter: drop-shadow(0 0 12px rgba(220,20,60,0.85));
  animation: logoGlow 4s ease-in-out infinite alternate;
}
@keyframes logoGlow {
  0% { filter: drop-shadow(0 0 8px rgba(220,20,60,0.6)); }
  100% { filter: drop-shadow(0 0 18px rgba(255,45,85,0.95)); }
}
.sidebar-nav { flex:1; display:flex; flex-direction:column; gap:4px; margin-top: 18px; overflow-y:auto; padding-right:4px; }
.nav-item {
  display:flex; align-items:center; gap:14px;
  padding: 13px 16px; border-radius: 14px;
  cursor:pointer; color: var(--text-dim);
  font-weight: 500; font-size: 0.93rem;
  transition: all var(--transition); position:relative; overflow:hidden;
  user-select:none; border:1px solid transparent;
}
.nav-item svg { width:20px; height:20px; flex-shrink:0; transition: transform 0.35s cubic-bezier(0.34,1.56,0.64,1); stroke: currentColor; fill: none; stroke-width: 2; }
.nav-item:hover { background: var(--surface-hover); color: var(--text-bright); transform: translateX(5px); border-color: var(--border); box-shadow: 0 4px 16px rgba(0,0,0,0.3); }
.nav-item:hover svg { transform: scale(1.18) rotate(-6deg); filter: drop-shadow(0 0 8px var(--accent)); }
.nav-item.active { background: linear-gradient(135deg, var(--accent) 0%, #c4123a 50%, #8b0018 100%); color:#fff; box-shadow: 0 8px 28px rgba(220,20,60,0.55), inset 0 1px 0 rgba(255,255,255,0.15); border-color: var(--accent-bright); }
.nav-item.active::before { content:''; position:absolute; left:0; top:10px; bottom:10px; width:3px; background:#fff; border-radius:0 4px 4px 0; box-shadow:0 0 12px #fff, 0 0 4px #fff; }
.nav-item.active svg { filter: drop-shadow(0 0 4px rgba(255,255,255,0.5)); }
.account-btn {
  margin-top:auto; padding: 12px 16px; border-radius: 12px;
  background: var(--surface); border: 1px solid var(--border);
  color: var(--text); font-weight: 600; cursor:pointer;
  display:flex; align-items:center; gap:12px;
  transition: all var(--transition); font-size: 0.9rem;
}
.account-btn:hover { background: var(--surface-hover); transform: translateY(-2px); box-shadow: 0 12px 32px rgba(0,0,0,0.5), var(--accent-glow-sm); border-color: var(--accent); }
.account-btn svg { width:18px; height:18px; }
.account-btn .avatar-mini { width:32px; height:32px; border-radius:50%; background: linear-gradient(135deg, var(--accent), var(--accent-bright)); display:flex; align-items:center; justify-content:center; font-weight:700; font-size:0.85rem; color:#fff; box-shadow: var(--accent-glow-sm), inset 0 2px 4px rgba(255,255,255,0.2); }

/* MAIN */
.main { margin-left: var(--sidebar-w); padding: 22px 28px; min-height: 100vh; transition: margin-left 0.3s ease; position:relative; z-index:2; }
.top-bar { display:flex; justify-content:space-between; align-items:center; margin-bottom: 28px; gap:16px; flex-wrap:wrap; animation: fadeInDown 0.5s ease; }
@keyframes fadeInDown { from{opacity:0; transform:translateY(-20px);} to{opacity:1; transform:translateY(0);} }

.top-left-group { display:flex; align-items:center; gap:14px; }

.back-btn {
  width: 46px; height: 46px; border-radius: 14px;
  background: var(--surface); border: 1px solid var(--border);
  color: var(--text); cursor:pointer;
  display:flex; align-items:center; justify-content:center;
  transition: all var(--transition);
  box-shadow: 0 2px 10px rgba(0,0,0,0.3);
}
.back-btn:hover { background: var(--surface-hover); transform: translateX(-4px); border-color: var(--accent-bright); box-shadow: var(--accent-glow-sm); color: var(--text-bright); }
.back-btn:active { transform: translateX(-1px) scale(0.96); }
.back-btn svg { width: 20px; height: 20px; stroke: currentColor; fill: none; stroke-width: 2.5; stroke-linecap: round; stroke-linejoin: round; transition: transform 0.25s; }
.back-btn:hover svg { transform: translateX(-2px); }

.clock-display {
  font-size: 2.4rem; font-weight: 700; letter-spacing: 4px;
  font-variant-numeric: tabular-nums; color: var(--text-bright);
  text-shadow: 0 0 20px rgba(255,45,85,0.8), 0 0 40px rgba(220,20,60,0.5), 0 0 80px rgba(220,20,60,0.25);
  animation: clockPulse 3s ease-in-out infinite alternate;
}
@keyframes clockPulse {
  0% { text-shadow: 0 0 18px rgba(255,45,85,0.7), 0 0 36px rgba(220,20,60,0.4); }
  100% { text-shadow: 0 0 28px rgba(255,80,120,0.95), 0 0 55px rgba(220,20,60,0.6), 0 0 90px rgba(220,20,60,0.3); }
}

.search-bar {
  display:flex; gap:0.5rem; width:100%; max-width:640px;
  background: rgba(20,0,10,0.7); backdrop-filter: blur(28px) saturate(1.3);
  -webkit-backdrop-filter: blur(28px) saturate(1.3);
  border: 1px solid var(--border); border-radius: 50px;
  padding: 0.4rem 0.4rem 0.4rem 1.5rem;
  box-shadow: var(--shadow), inset 0 1px 0 rgba(255,255,255,0.06), 0 0 0 1px rgba(0,0,0,0.3);
  transition: all var(--transition);
}
.search-bar:focus-within { 
  border-color: var(--accent-bright); 
  box-shadow: 0 0 0 4px rgba(220,20,60,0.2), var(--shadow-lg), var(--accent-glow); 
  transform: translateY(-2px); 
  background: rgba(30,0,12,0.8);
}
.search-bar input { flex:1; background:transparent; border:none; color:var(--text-bright); font-size: 1rem; outline:none; font-weight:500; letter-spacing: 0.2px; }
.search-bar input::placeholder { color: var(--text-muted); }
.search-bar button { padding: 0.75rem 1.6rem; border-radius: 40px; border: none; background: linear-gradient(135deg, var(--accent) 0%, #e01240 50%, #b00020 100%); color:#fff; font-weight: 700; cursor:pointer; display:flex; align-items:center; gap:0.45rem; transition: all var(--transition); box-shadow: var(--accent-glow-sm), inset 0 1px 0 rgba(255,255,255,0.15); }
.search-bar button:hover { transform: scale(1.05); box-shadow: var(--accent-glow); filter: brightness(1.08); }
.search-bar button:active { transform: scale(0.98); }

.hamburger { display:none; width:46px; height:46px; border-radius: 12px; background: var(--surface); border: 1px solid var(--border); color:var(--text); cursor:pointer; align-items:center; justify-content:center; transition: all var(--transition); }
.hamburger:hover { background: var(--surface-hover); transform: scale(1.05); border-color: var(--accent); }

/* CONTENT */
.content-section { display:none; }
.content-section.active { display:block; animation: sectionIn 0.4s cubic-bezier(0.2, 0.9, 0.4, 1); }
@keyframes sectionIn { from{opacity:0; transform:translateY(16px);} to{opacity:1; transform:translateY(0);} }
.section-title { font-size: 1.7rem; font-weight: 700; color: var(--text-bright); margin-bottom: 1.35rem; text-shadow: 0 0 24px rgba(220,20,60,0.55), 0 0 48px rgba(220,20,60,0.2); display:flex; align-items:center; gap:0.7rem; letter-spacing: -0.4px; }
.section-title svg { width:26px; height:26px; stroke: var(--accent-bright); filter: drop-shadow(0 0 8px var(--accent)); fill: none; stroke-width: 2; transition: transform 0.3s; }
.section-title:hover svg { transform: scale(1.1) rotate(-3deg); }

.grid { display:grid; grid-template-columns: repeat(auto-fill, minmax(160px, 1fr)); gap: 1.2rem; }

.card {
  background: linear-gradient(165deg, rgba(220,20,60,0.08) 0%, rgba(20,0,10,0.6) 40%, rgba(10,0,5,0.85) 100%);
  backdrop-filter: blur(24px) saturate(1.4);
  -webkit-backdrop-filter: blur(24px) saturate(1.4);
  border: 1px solid var(--border); border-radius: var(--radius);
  padding: 1rem; text-align:center; cursor:pointer;
  transition: transform var(--transition), box-shadow var(--transition), background var(--transition), border-color var(--transition), filter 0.3s;
  display:flex; flex-direction:column; gap:0.7rem; overflow:hidden; position:relative;
  animation: cardIn 0.45s cubic-bezier(0.2, 0.9, 0.4, 1) backwards;
  will-change: transform;
  box-shadow: 0 4px 20px rgba(0,0,0,0.4), inset 0 1px 0 rgba(255,255,255,0.04);
}
.card::before {
  content: '';
  position: absolute; inset: 0; border-radius: inherit;
  background: linear-gradient(135deg, rgba(255,255,255,0.06) 0%, transparent 40%, transparent 60%, rgba(220,20,60,0.05) 100%);
  opacity: 0; transition: opacity 0.35s; pointer-events: none; z-index: 0;
}
.card > * { position: relative; z-index: 1; }
@keyframes cardIn { from{opacity:0; transform:translateY(20px) scale(0.94);} to{opacity:1; transform:translateY(0) scale(1);} }
.card:hover { 
  transform: translateY(-10px) scale(1.03); 
  box-shadow: var(--shadow-lg), 0 0 0 1px var(--accent-bright), var(--accent-glow); 
  background: linear-gradient(165deg, rgba(220,20,60,0.16) 0%, rgba(40,5,15,0.75) 50%, rgba(15,0,8,0.9) 100%); 
  border-color: var(--accent-bright); 
  filter: brightness(1.05);
}
.card:hover::before { opacity: 1; }
.poster { width:100%; aspect-ratio: 2/3; border-radius: 10px; overflow:hidden; background: linear-gradient(135deg, #1a0010 0%, #0a0005 100%); position:relative; }
.poster img { width:100%; height:100%; object-fit:cover; transition: transform 0.6s cubic-bezier(0.2, 0.9, 0.4, 1), opacity 0.4s; opacity:0; }
.poster img.loaded { opacity:1; animation: imageFadeIn 0.55s ease; }
@keyframes imageFadeIn { from{opacity:0; transform:scale(1.08);} to{opacity:1; transform:scale(1);} }
.card:hover .poster img { transform: scale(1.12); }
.poster { box-shadow: 0 4px 16px rgba(0,0,0,0.4); }
.card:hover .poster { box-shadow: 0 8px 28px rgba(0,0,0,0.55); }
.poster .fallback-icon { position:absolute; inset:0; display:flex; align-items:center; justify-content:center; color:var(--text-muted); }
.poster .fallback-icon svg { width:44px; height:44px; opacity:0.5; stroke: var(--accent); fill: none; stroke-width: 2; }
.poster .skeleton { position:absolute; inset:0; background: linear-gradient(90deg, rgba(220,20,60,0.03) 0%, rgba(220,20,60,0.12) 50%, rgba(220,20,60,0.03) 100%); background-size: 200% 100%; animation: shimmer 1.4s infinite; }
@keyframes shimmer { from{background-position: 200% 0;} to{background-position: -200% 0;} }
.card h3 { font-size: 0.9rem; font-weight: 600; white-space: nowrap; overflow:hidden; text-overflow:ellipsis; color: var(--text-bright); }
.card:hover h3 { color: var(--accent-bright); }
.card .meta { font-size: 0.72rem; color: var(--text-dim); font-weight: 500; }
.card button.play-btn { background: linear-gradient(145deg, var(--accent-bright), var(--accent) 40%, #b00020); border:none; cursor:pointer; padding: 0.5rem; border-radius: 50%; width: 42px; height: 42px; display:flex; align-items:center; justify-content:center; margin: 0 auto; transition: all var(--transition-bounce); box-shadow: 0 4px 18px rgba(220,20,60,0.45), inset 0 1px 0 rgba(255,255,255,0.2); }
.card button.play-btn:hover { transform: scale(1.2) rotate(12deg); box-shadow: 0 8px 28px rgba(220,20,60,0.7), var(--accent-glow); filter: brightness(1.1); }
.card button.play-btn:active { transform: scale(1.05) rotate(5deg); }
.card button.play-btn svg { width:17px; height:17px; fill:#fff; filter: drop-shadow(0 1px 2px rgba(0,0,0,0.3)); }

/* PROXY APP LAUNCHER */
.proxy-wrapper { max-width: 1200px; margin: 0 auto; padding: 20px 0; }
.proxy-search-hero { display:flex; gap:0.5rem; width:100%; max-width: 900px; margin: 0 auto 2.5rem; background: rgba(20,0,10,0.85); backdrop-filter: blur(24px); border: 1px solid var(--border); border-radius: 16px; padding: 0.5rem 0.5rem 0.5rem 1.6rem; box-shadow: var(--shadow-lg), inset 0 1px 0 rgba(255,255,255,0.04); transition: all var(--transition); }
.proxy-search-hero:focus-within { border-color: var(--accent); box-shadow: 0 0 0 4px rgba(220,20,60,0.15), var(--shadow-lg), var(--accent-glow-sm); }
.proxy-search-hero svg.search-icon { width:20px; height:20px; stroke: var(--text-dim); align-self:center; flex-shrink:0; }
.proxy-search-hero input { flex:1; background:transparent; border:none; color:var(--text); font-size:1rem; outline:none; font-weight:500; padding: 0.6rem 0; }
.proxy-search-hero input::placeholder { color: var(--text-muted); }
.proxy-search-hero button { padding: 0.75rem 1.8rem; border-radius: 12px; border:none; background: linear-gradient(135deg, var(--accent), #b00020); color:#fff; font-weight: 700; cursor:pointer; transition: all var(--transition); box-shadow: var(--accent-glow-sm); }
.proxy-search-hero button:hover { transform: translateY(-1px); box-shadow: var(--accent-glow); }

.proxy-apps-grid { display:grid; grid-template-columns: repeat(auto-fill, minmax(150px, 1fr)); gap: 18px; max-width: 1100px; margin: 0 auto; }
.proxy-app {
  background: linear-gradient(165deg, rgba(40,8,18,0.95) 0%, rgba(18,2,10,0.98) 100%);
  border: 1px solid var(--border); border-radius: 20px;
  padding: 24px 12px 18px; display:flex; flex-direction:column; align-items:center; gap:14px;
  cursor:pointer; transition: all var(--transition-bounce); position:relative; overflow:hidden;
  animation: cardIn 0.45s cubic-bezier(0.2, 0.9, 0.4, 1) backwards;
  box-shadow: 0 6px 24px rgba(0,0,0,0.45), inset 0 1px 0 rgba(255,255,255,0.04);
}
.proxy-app::after {
  content: ''; position: absolute; inset: 0; border-radius: inherit;
  background: radial-gradient(circle at 50% 0%, rgba(255,255,255,0.08), transparent 60%);
  opacity: 0; transition: opacity 0.3s; pointer-events: none;
}
.proxy-app:hover { transform: translateY(-10px) scale(1.04); border-color: var(--accent-bright); box-shadow: var(--shadow-lg), 0 0 0 1px var(--accent-bright), var(--accent-glow); background: linear-gradient(165deg, rgba(55,10,25,0.95) 0%, rgba(30,4,15,0.98) 100%); }
.proxy-app:hover::after { opacity: 1; }
.proxy-app:active { transform: translateY(-3px) scale(0.97); }
.proxy-app .app-icon-wrap { width: 72px; height: 72px; border-radius: 18px; display:flex; align-items:center; justify-content:center; position:relative; transition: transform 0.4s cubic-bezier(0.34,1.56,0.64,1); box-shadow: 0 8px 24px rgba(0,0,0,0.55), inset 0 1px 0 rgba(255,255,255,0.12); overflow:hidden; }
.proxy-app:hover .app-icon-wrap { transform: scale(1.12) rotate(-4deg); box-shadow: 0 12px 32px rgba(0,0,0,0.6); }
.proxy-app .app-icon-wrap img, .proxy-app .app-icon-wrap svg { width: 100%; height: 100%; object-fit: cover; }
.proxy-app .app-label { font-size: 0.84rem; font-weight: 600; color: var(--text-bright); text-align:center; letter-spacing: 0.3px; transition: color 0.25s; }
.proxy-app:hover .app-label { color: var(--accent-bright); text-shadow: 0 0 12px rgba(255,45,85,0.4); }

/* App brand colors */
.proxy-app.youtube .app-icon-wrap { background:#ff0000; }
.proxy-app.tiktok .app-icon-wrap { background:#000; }
.proxy-app.roblox .app-icon-wrap { background:#00a2ff; }
.proxy-app.geometrydash .app-icon-wrap { background:#ff8f00; }
.proxy-app.kick .app-icon-wrap { background:#53fc18; }
.proxy-app.twitch .app-icon-wrap { background:#9146ff; }
.proxy-app.snapchat .app-icon-wrap { background:#fffc00; }
.proxy-app.instagram .app-icon-wrap { background: linear-gradient(45deg, #f09433, #e6683c, #dc2743, #cc2366, #bc1888); }
.proxy-app.discord .app-icon-wrap { background:#5865f2; }
.proxy-app.netflix .app-icon-wrap { background:#e50914; }
.proxy-app.spotify .app-icon-wrap { background:#1db954; }
.proxy-app.reddit .app-icon-wrap { background:#ff4500; }
.proxy-app.twitter .app-icon-wrap { background:#000; }
.proxy-app.github .app-icon-wrap { background:#24292e; }
.proxy-app.wikipedia .app-icon-wrap { background:#fff; }
.proxy-app.hackernews .app-icon-wrap { background:#ff6600; }
.proxy-app.tiktok .app-icon-wrap svg { width: 48px; height: 48px; }

/* PROXY TOOLBAR */
.proxy-toolbar { display:flex; gap:0.5rem; align-items:center; flex-wrap:wrap; margin-bottom: 1rem; padding: 0.6rem; background: var(--surface); backdrop-filter: blur(20px); border: 1px solid var(--border); border-radius: 14px; }
.proxy-toolbar button { padding: 0.55rem 0.9rem; border-radius: 10px; border: 1px solid var(--border); background: var(--surface); color: var(--text); font-weight: 600; cursor:pointer; display:inline-flex; align-items:center; justify-content:center; gap:0.4rem; transition: all var(--transition); font-size: 0.85rem; min-width: 40px; }
.proxy-toolbar button:hover { background: var(--surface-hover); border-color: var(--accent); transform: translateY(-1px); }
.proxy-toolbar button svg { width:16px; height:16px; stroke: currentColor; fill: none; stroke-width: 2; stroke-linecap: round; }
.proxy-toolbar .engine-switch { display:flex; gap:0.3rem; margin-left:auto; padding: 0.3rem; background: var(--surface-solid); border-radius: 10px; border: 1px solid var(--border); }
.proxy-toolbar .engine-switch button { padding: 0.4rem 0.8rem; font-size: 0.75rem; border-radius: 8px; }
.proxy-toolbar .engine-switch button.active { background: linear-gradient(135deg, var(--accent), #b00020); color:#fff; border-color: var(--accent-bright); }

.proxy-status { display:flex; align-items:center; gap:0.5rem; font-size: 0.78rem; color: var(--text-dim); margin-bottom: 0.8rem; padding: 0.5rem 0.9rem; background: rgba(20,0,10,0.7); border: 1px solid var(--border); border-radius: 10px; font-weight: 500; }
.proxy-status .dot { width:9px; height:9px; border-radius:50%; background: var(--warning); animation: pulse 1.2s infinite; box-shadow: 0 0 8px currentColor; }
.proxy-status .dot.ok { background: var(--success); animation: none; box-shadow: 0 0 12px var(--success); }
.proxy-status .dot.err { background: var(--danger); animation: none; }
@keyframes pulse { 0%,100%{opacity:1;} 50%{opacity:0.4;} }

.proxy-frame-container { display:none; margin-top: 1rem; border-radius: 14px; overflow:hidden; border: 1px solid var(--border); box-shadow: var(--shadow-lg); background: #000; }
.proxy-frame-container.show { display:block; }
.proxy-frame-container iframe { width:100%; height: 75vh; border:none; background:#000; display:block; }

/* MUSIC */
.music-container { display:flex; flex-direction:column; min-height: calc(100vh - 220px); position:relative; }
.music-topbar { display:flex; justify-content:space-between; align-items:center; gap: 1rem; margin-bottom: 1.4rem; flex-wrap:wrap; }
.music-search { display:flex; gap:0.5rem; flex:1; min-width: 300px; max-width: 520px; background: var(--surface); border: 1px solid var(--border); border-radius: 40px; padding: 0.35rem 0.35rem 0.35rem 1.3rem; backdrop-filter: blur(20px); transition: all var(--transition); }
.music-search:focus-within { border-color: var(--accent); box-shadow: 0 0 0 4px rgba(220,20,60,0.15), var(--accent-glow-sm); }
.music-search input { flex:1; background:transparent; border:none; color:var(--text); font-size: 0.95rem; outline:none; font-weight:500; }
.music-search input::placeholder { color: var(--text-muted); }
.music-search button { padding: 0.6rem 1.2rem; border-radius: 30px; border:none; background: linear-gradient(135deg, var(--accent), #b00020); color:#fff; font-weight: 700; cursor:pointer; display:flex; align-items:center; gap:0.4rem; font-size: 0.85rem; transition: all var(--transition); }
.music-search button:hover { box-shadow: var(--accent-glow-sm); }
.music-tabs { display:flex; gap:0.4rem; flex-wrap:wrap; }
.music-tab { padding: 0.55rem 1.1rem; border-radius: 30px; border: 1px solid var(--border); background: var(--surface); color: var(--text); font-weight: 600; font-size: 0.82rem; cursor:pointer; transition: all var(--transition); }
.music-tab:hover { background: var(--surface-hover); border-color: var(--accent); transform: translateY(-1px); }
.music-tab.active { background: linear-gradient(135deg, var(--accent), #b00020); color:#fff; border-color: var(--accent-bright); box-shadow: var(--accent-glow-sm); }
.music-main { flex:1; padding-bottom: 130px; }
.music-section-title { font-size: 1.15rem; font-weight: 700; color: var(--text-bright); margin: 1.5rem 0 0.8rem; }
.music-section-title:first-child { margin-top: 0; }

.track-list { display:flex; flex-direction:column; gap:3px; }
.track-row { display:grid; grid-template-columns: 40px 52px 1fr 1fr auto auto; align-items:center; gap: 0.9rem; padding: 0.6rem 0.9rem; border-radius: 10px; cursor:pointer; transition: all 0.15s; animation: cardIn 0.3s ease backwards; border: 1px solid transparent; }
.track-row:hover { background: var(--surface-hover); transform: translateX(3px); border-color: var(--border); }
.track-row.playing { background: rgba(220,20,60,0.15); border: 1px solid var(--accent); box-shadow: var(--accent-glow-sm); }
.track-row .track-num { font-size: 0.82rem; color: var(--text-dim); text-align:center; font-variant-numeric: tabular-nums; }
.track-row .track-art { width: 52px; height: 52px; border-radius: 8px; overflow:hidden; background: var(--surface-solid); flex-shrink:0; border: 1px solid var(--border); }
.track-row .track-art img { width:100%; height:100%; object-fit:cover; }
.track-row .track-info { min-width:0; }
.track-row .track-title { font-size: 0.9rem; font-weight: 600; white-space: nowrap; overflow:hidden; text-overflow:ellipsis; color: var(--text-bright); margin-bottom: 2px; }
.track-row .track-artist { font-size: 0.75rem; color: var(--text-dim); white-space:nowrap; overflow:hidden; text-overflow:ellipsis; }
.track-row .track-album { font-size: 0.78rem; color: var(--text-dim); white-space:nowrap; overflow:hidden; text-overflow:ellipsis; }
.track-row .track-dur { font-size: 0.75rem; color: var(--text-dim); }
.track-row .track-actions { display:flex; gap:0.3rem; opacity:0; transition: opacity 0.2s; }
.track-row:hover .track-actions { opacity:1; }
.track-row .track-actions button { width: 32px; height: 32px; border-radius: 50%; border:none; background:transparent; color: var(--text-dim); cursor:pointer; display:flex; align-items:center; justify-content:center; transition: all 0.15s; }
.track-row .track-actions button:hover { background: var(--surface-hover); color: var(--accent-bright); transform: scale(1.15); }
.track-row .track-actions button.fav-on { color: var(--accent-bright); }

.genre-grid { display:grid; grid-template-columns: repeat(auto-fill, minmax(180px, 1fr)); gap:1rem; margin-top: 0.5rem; }
.genre-card { padding: 1.6rem 1.2rem; border-radius: 14px; cursor:pointer; color:#fff; font-weight: 700; font-size: 1rem; text-shadow: 0 2px 8px rgba(0,0,0,0.5); transition: all var(--transition-bounce); min-height: 90px; display:flex; align-items:center; animation: cardIn 0.4s ease backwards; border: 1px solid rgba(255,255,255,0.1); }
.genre-card:hover { transform: translateY(-6px) scale(1.03); box-shadow: var(--shadow-lg); }

.playlist-grid { display:grid; grid-template-columns: repeat(auto-fill, minmax(180px, 1fr)); gap: 1.2rem; }
.playlist-card { background: var(--surface); border: 1px solid var(--border); border-radius: 14px; padding: 0.9rem; cursor:pointer; transition: all var(--transition); display:flex; flex-direction:column; gap:0.6rem; }
.playlist-card:hover { transform: translateY(-6px); box-shadow: var(--shadow-lg); border-color: var(--accent); }
.playlist-card .pl-art { width:100%; aspect-ratio:1; border-radius: 10px; overflow:hidden; background: var(--surface-solid); border: 1px solid var(--border); }
.playlist-card .pl-art img { width:100%; height:100%; object-fit:cover; }
.playlist-card .pl-title { font-size: 0.9rem; font-weight: 700; white-space:nowrap; overflow:hidden; text-overflow:ellipsis; color: var(--text-bright); }
.playlist-card .pl-artist { font-size: 0.75rem; color: var(--text-dim); white-space:nowrap; overflow:hidden; text-overflow:ellipsis; }

.music-player { position: fixed; bottom:0; left: var(--sidebar-w); right:0; background: linear-gradient(180deg, rgba(25,0,12,0.97), rgba(8,0,4,0.99)); backdrop-filter: blur(32px) saturate(1.4); -webkit-backdrop-filter: blur(32px) saturate(1.4); border-top: 1px solid var(--border-strong); padding: 0.95rem 1.5rem; display:grid; grid-template-columns: 1fr 2fr 1fr; align-items:center; gap:1rem; z-index: 500; box-shadow: 0 -12px 50px rgba(0,0,0,0.7), 0 -1px 0 rgba(255,255,255,0.04); transform: translateY(100%); transition: transform 0.45s cubic-bezier(0.2, 0.9, 0.4, 1); }
.music-player.visible { transform: translateY(0); }
.music-player::before {
  content: ''; position: absolute; top: 0; left: 10%; right: 10%; height: 1px;
  background: linear-gradient(90deg, transparent, var(--accent), transparent);
  opacity: 0.6;
}
.mp-left { display:flex; align-items:center; gap: 0.85rem; min-width:0; }
.mp-art { width: 56px; height: 56px; border-radius: 10px; overflow:hidden; background: var(--surface-solid); border: 1px solid var(--border); flex-shrink:0; display:flex; align-items:center; justify-content:center; color: var(--text-dim); }
.mp-art img { width:100%; height:100%; object-fit:cover; }
.mp-art svg { width: 24px; height: 24px; }
.mp-meta { min-width:0; }
.mp-title { font-size: 0.9rem; font-weight: 700; white-space:nowrap; overflow:hidden; text-overflow:ellipsis; color: var(--text-bright); }
.mp-artist { font-size: 0.75rem; color: var(--text-dim); white-space:nowrap; overflow:hidden; text-overflow:ellipsis; }
.mp-center { display:flex; flex-direction:column; gap: 0.5rem; align-items:center; }
.mp-controls { display:flex; align-items:center; gap: 0.4rem; }
.mp-btn { width: 40px; height: 40px; border-radius: 50%; border:none; background:transparent; color: var(--text); cursor:pointer; display:flex; align-items:center; justify-content:center; transition: all 0.2s; }
.mp-btn:hover { background: var(--surface-hover); transform: scale(1.12); color: var(--accent-bright); }
.mp-btn svg { width: 18px; height: 18px; }
.mp-btn.main { width: 48px; height: 48px; background: linear-gradient(135deg, var(--accent), #b00020); color:#fff; box-shadow: 0 4px 20px rgba(220,20,60,0.5); }
.mp-btn.main:hover { transform: scale(1.08); box-shadow: 0 6px 28px rgba(220,20,60,0.7); }
.mp-btn.main svg { width: 22px; height: 22px; }
.mp-btn.active { color: var(--accent-bright); }
.mp-progress-row { display:flex; align-items:center; gap: 0.6rem; width:100%; max-width: 520px; }
.mp-time { font-size: 0.7rem; color: var(--text-dim); min-width: 36px; text-align:center; font-variant-numeric: tabular-nums; font-weight: 600; }
.mp-progress { flex:1; height: 6px; background: rgba(220,20,60,0.15); border-radius: 20px; overflow:hidden; cursor:pointer; transition: height 0.15s; }
.mp-progress:hover { height: 8px; }
.mp-progress-fill { height:100%; background: linear-gradient(90deg, var(--accent), var(--accent-bright)); width:0%; border-radius:20px; }
.mp-right { display:flex; align-items:center; gap:0.6rem; justify-content:flex-end; }
.mp-volume { display:flex; align-items:center; gap:0.4rem; color: var(--text-dim); }
.mp-volume svg { width: 16px; height: 16px; }
.mp-volume input[type="range"] { width: 90px; -webkit-appearance:none; appearance:none; height: 4px; background: rgba(220,20,60,0.2); border-radius: 10px; outline:none; cursor:pointer; }
.mp-volume input[type="range"]::-webkit-slider-thumb { -webkit-appearance:none; appearance:none; width:12px; height:12px; border-radius:50%; background: var(--accent-bright); cursor:pointer; box-shadow: 0 0 8px var(--accent); }
.mp-volume input[type="range"]::-moz-range-thumb { width:12px; height:12px; border-radius:50%; background: var(--accent-bright); border:none; cursor:pointer; }

.music-viz { position: fixed; bottom: 96px; left: 50%; transform: translateX(-50%); width: 280px; height: 40px; opacity: 0.7; pointer-events:none; z-index: 501; }

/* MODAL / PLAYER */
.modal { display:none; position: fixed; inset:0; background: rgba(0,0,0,0.9); z-index: 2000; justify-content:center; align-items:center; backdrop-filter: blur(12px); animation: modalIn 0.3s ease; }
.modal.show { display:flex; }
@keyframes modalIn { from{opacity:0;} to{opacity:1;} }
#playerModal { display:none; position: fixed; inset:0; background: rgba(0,0,0,0.98); z-index: 2000; justify-content:center; align-items:center; flex-direction:column; gap:1rem; padding: 20px; }
#playerModal.show { display:flex; }
#playerModal iframe { width: 95%; height: 82%; border:none; border-radius: 18px; box-shadow: 0 30px 90px rgba(0,0,0,0.9), 0 0 0 1px var(--border); background: #000; }
.close-btn { position: absolute; top: 22px; right: 30px; width: 48px; height: 48px; border-radius: 50%; background: rgba(220,20,60,0.15); border: 1px solid var(--border); color: #fff; cursor:pointer; display:flex; align-items:center; justify-content:center; transition: all 0.25s; z-index: 2001; }
.close-btn:hover { background: var(--accent); transform: rotate(90deg) scale(1.1); }
.close-btn svg { width: 22px; height: 22px; stroke: currentColor; fill: none; stroke-width: 2.5; }

#playerControls { display:flex; gap: 0.8rem; padding: 0.9rem 1.2rem; background: var(--surface-solid); border-radius: 16px; border: 1px solid var(--border); flex-wrap:wrap; align-items:center; justify-content:center; }
#playerControls select, #playerControls button { padding: 0.55rem 1rem; border-radius: 10px; border: 1px solid var(--border); background: var(--surface); color: var(--text); font-size: 0.85rem; cursor:pointer; display:flex; align-items:center; gap:0.4rem; transition: all var(--transition); font-weight: 600; }
#playerControls select:hover, #playerControls button:hover { background: var(--surface-hover); border-color: var(--accent); }
#playerControls button.active { background: linear-gradient(135deg, var(--accent), #b00020); color:#fff; }

/* SOURCE HUD */
#sourceTestHUD { display:none; position: fixed; inset:0; background: rgba(0,0,0,0.96); z-index: 2100; flex-direction:column; align-items:center; justify-content:center; gap: 2rem; padding: 2rem; backdrop-filter: blur(16px); }
#sourceTestHUD.show { display:flex; }
#sourceTestHUD .hud-title { font-size: 1.9rem; font-weight: 700; color: var(--text-bright); }
#sourceTestHUD .hud-sub { color: var(--text-dim); font-size: 1rem; text-align:center; max-width: 540px; }
#sourceTestHUD .hud-progress { width:100%; max-width: 640px; height: 10px; background: rgba(220,20,60,0.15); border-radius: 20px; overflow:hidden; border: 1px solid var(--border); }
#sourceTestHUD .hud-progress-fill { height:100%; background: linear-gradient(90deg, var(--accent), var(--accent-bright)); width:0%; transition: width 0.4s; }
#sourceTestHUD .hud-source-list { display:flex; flex-wrap:wrap; gap: 0.5rem; max-width: 760px; justify-content:center; }
#sourceTestHUD .hud-source-chip { padding: 0.45rem 1rem; border-radius: 20px; background: var(--surface); border: 1px solid var(--border); font-size: 0.75rem; color: var(--text-dim); font-weight: 600; }
#sourceTestHUD .hud-source-chip.testing { background: var(--warning); color:#000; }
#sourceTestHUD .hud-source-chip.failed { background: rgba(220,20,60,0.2); border-color: var(--danger); color: var(--danger); text-decoration: line-through; }
#sourceTestHUD .hud-source-chip.success { background: var(--accent); color:#fff; border-color: var(--accent-bright); }
#sourceTestHUD .hud-actions { display:flex; gap:1rem; }
#sourceTestHUD button { padding: 0.85rem 1.7rem; border-radius: 30px; border:none; background: linear-gradient(135deg, var(--accent), #b00020); color:#fff; font-weight: 700; cursor:pointer; font-size: 0.9rem; transition: all var(--transition); }
#sourceTestHUD button:hover { transform: translateY(-3px); box-shadow: var(--accent-glow); }
#sourceTestHUD button.skip { background: var(--surface); border: 1px solid var(--border); color: var(--text); }
#sourceTestHUD button.cancel { background: var(--danger); }

/* SETTINGS */
.settings-grid { display:grid; grid-template-columns: repeat(auto-fit, minmax(300px, 1fr)); gap: 1.2rem; }
.setting-card { background: linear-gradient(180deg, var(--surface), rgba(220,20,60,0.03)); backdrop-filter: blur(20px); border: 1px solid var(--border); border-radius: var(--radius-lg); padding: 1.5rem; animation: cardIn 0.4s ease backwards; transition: all var(--transition); }
.setting-card:hover { border-color: var(--accent); transform: translateY(-2px); }
.setting-card h3 { margin-bottom: 1rem; font-size: 1rem; display:flex; align-items:center; gap:0.5rem; font-weight: 700; color: var(--text-bright); }
.setting-card h3 svg { width:18px; height:18px; stroke: var(--accent-bright); fill:none; stroke-width:2; }
.setting-card select, .setting-card input { width:100%; padding: 0.75rem 1rem; border-radius: 12px; border: 1px solid var(--border); background: rgba(220,20,60,0.06); color: var(--text); margin-bottom: 0.8rem; outline:none; font-size: 0.9rem; font-weight: 500; font-family: inherit; }
.setting-card select:focus, .setting-card input:focus { border-color: var(--accent); box-shadow: 0 0 0 3px rgba(220,20,60,0.18); }
.setting-card button { padding: 0.75rem 1.3rem; border-radius: 12px; border:none; background: linear-gradient(135deg, var(--accent), #b00020); color:#fff; cursor:pointer; font-weight: 700; display:inline-flex; align-items:center; gap:0.4rem; transition: all var(--transition); font-size: 0.85rem; }
.setting-card button:hover { transform: translateY(-2px); box-shadow: var(--accent-glow-sm); }
.theme-category { margin-top: 1rem; margin-bottom: 0.6rem; font-size: 0.72rem; text-transform: uppercase; letter-spacing: 2px; color: var(--text-dim); font-weight: 700; }
.theme-category:first-child { margin-top:0; }
.theme-grid { display:grid; grid-template-columns: repeat(auto-fill, minmax(105px, 1fr)); gap: 0.55rem; }
.theme-chip { padding: 0.75rem 0.6rem; border-radius: 12px; cursor:pointer; border: 2px solid var(--border); background: var(--surface); color: var(--text); font-size: 0.72rem; font-weight: 700; text-align:center; transition: all var(--transition); display:flex; flex-direction:column; align-items:center; gap: 0.4rem; user-select:none; }
.theme-chip:hover { transform: translateY(-4px); border-color: var(--accent); }
.theme-chip.active { border-color: var(--accent-bright); background: linear-gradient(135deg, var(--accent), #b00020); color:#fff; }
.theme-chip .swatch { width: 30px; height: 30px; border-radius: 50%; border: 2px solid rgba(255,255,255,0.15); }

.bookmark-item { display:flex; align-items:center; gap:0.6rem; padding: 0.8rem 1.1rem; background: var(--surface); border: 1px solid var(--border); border-radius: 12px; margin-bottom: 0.5rem; transition: all var(--transition); }
.bookmark-item:hover { background: var(--surface-hover); transform: translateX(4px); border-color: var(--accent); }
.bookmark-item .b-title { flex:1; font-weight: 600; font-size: 0.92rem; color: var(--text-bright); }
.bookmark-item .b-url { font-size: 0.72rem; color: var(--text-dim); flex:2; overflow:hidden; text-overflow:ellipsis; white-space:nowrap; }
.bookmark-item button { padding: 0.35rem 0.75rem; font-size: 0.75rem; border-radius: 8px; border:none; background: var(--surface-hover); color: var(--text); cursor:pointer; font-weight: 700; }
.bookmark-item button.open { background: linear-gradient(135deg, var(--accent), #b00020); color:#fff; }
.bookmark-item button.del { background: var(--danger); color:#fff; }

#aiChat { height: 440px; overflow-y:auto; background: rgba(220,20,60,0.04); border: 1px solid var(--border); border-radius: 12px; padding: 1rem; margin-bottom: 1rem; }
.ai-msg { margin-bottom: 0.8rem; line-height: 1.55; padding: 0.75rem 1rem; border-radius: 14px; animation: msgIn 0.35s; max-width: 85%; word-wrap:break-word; font-size: 0.9rem; }
@keyframes msgIn { from{opacity:0; transform:translateY(10px);} to{opacity:1; transform:translateY(0);} }
.ai-msg.user { background: rgba(220,20,60,0.15); margin-left: auto; border-bottom-right-radius: 4px; border: 1px solid rgba(220,20,60,0.3); color: var(--text-bright); }
.ai-msg.bot { background: var(--surface); margin-right: auto; border-bottom-left-radius: 4px; border: 1px solid var(--border); color: var(--text); }
.ai-msg strong { display:block; font-size: 0.72rem; margin-bottom: 0.35rem; text-transform: uppercase; letter-spacing: 1.2px; font-weight: 700; }

.account-header { display:flex; align-items:center; gap: 1.5rem; padding: 2rem; background: linear-gradient(135deg, rgba(220,20,60,0.12), rgba(220,20,60,0.03)); border: 1px solid var(--border); border-radius: var(--radius-lg); margin-bottom: 1.5rem; }
.account-avatar { width: 100px; height: 100px; border-radius: 50%; background: linear-gradient(135deg, var(--accent), var(--accent-bright)); display:flex; align-items:center; justify-content:center; font-size: 2.4rem; font-weight: 700; color:#fff; flex-shrink:0; box-shadow: 0 12px 48px rgba(220,20,60,0.5); }
.account-info h2 { font-size: 1.8rem; margin-bottom: 0.3rem; font-weight: 700; color: var(--text-bright); }
.account-info p { color: var(--text-dim); font-size: 0.9rem; }
.stats-row { display:grid; grid-template-columns: repeat(auto-fit, minmax(150px, 1fr)); gap: 1rem; margin-bottom: 1.5rem; }
.stat-box { padding: 1.3rem; background: var(--surface); border: 1px solid var(--border); border-radius: var(--radius); text-align:center; transition: all var(--transition); }
.stat-box:hover { transform: translateY(-4px); border-color: var(--accent); }
.stat-box .num { font-size: 2rem; font-weight: 700; background: linear-gradient(135deg, var(--accent-bright), var(--accent)); -webkit-background-clip: text; -webkit-text-fill-color: transparent; background-clip: text; }
.stat-box .lbl { font-size: 0.72rem; color: var(--text-dim); text-transform: uppercase; letter-spacing: 1.5px; margin-top: 0.4rem; font-weight: 700; }
.form-group { margin-bottom: 1rem; }
.form-group label { display:block; font-size: 0.8rem; color: var(--text-dim); margin-bottom: 0.4rem; font-weight: 700; text-transform: uppercase; }
.form-error { color: var(--accent-bright); font-size: 0.85rem; margin-top: 0.5rem; min-height: 1.2em; text-align:center; font-weight: 600; }
.btn-primary { width:100%; padding: 0.9rem; border-radius: 12px; border:none; background: linear-gradient(135deg, var(--accent), #b00020); color:#fff; font-weight: 700; font-size: 0.95rem; cursor:pointer; transition: all var(--transition); margin-top: 0.5rem; box-shadow: var(--accent-glow-sm); font-family: inherit; }
.btn-primary:hover { transform: translateY(-2px); box-shadow: var(--accent-glow); }
.btn-link { background:none; border:none; color: var(--accent-bright); cursor:pointer; font-size: 0.85rem; font-weight: 700; padding: 0.4rem; }
.btn-link:hover { text-decoration: underline; }
.form-switch { text-align:center; margin-top: 1rem; font-size: 0.85rem; color: var(--text-dim); }

.loading { text-align:center; padding: 2.5rem; color: var(--text-dim); display:flex; flex-direction:column; align-items:center; gap: 1rem; font-size: 0.9rem; font-weight: 500; }
.spinner { 
  width: 48px; height: 48px; 
  border: 3px solid rgba(220,20,60,0.12); 
  border-top-color: var(--accent-bright); 
  border-right-color: var(--accent);
  border-radius: 50%; 
  animation: spin 0.85s cubic-bezier(0.5,0.1,0.5,0.9) infinite;
  box-shadow: 0 0 20px rgba(220,20,60,0.25);
}
@keyframes spin { to{transform:rotate(360deg);} }
.loading { text-align:center; padding: 3rem 1.5rem; color: var(--text-dim); display:flex; flex-direction:column; align-items:center; gap: 1.1rem; font-size: 0.92rem; font-weight: 500; letter-spacing: 0.3px; }

#toastContainer { position: fixed; bottom: 24px; right: 24px; display:flex; flex-direction:column-reverse; gap: 10px; z-index: 3000; pointer-events:none; max-width: 380px; }
.toast { 
  background: linear-gradient(165deg, rgba(30,5,15,0.98), rgba(10,0,5,0.99)); 
  border: 1px solid var(--border); border-left: 4px solid var(--accent); 
  border-radius: 16px; padding: 14px 18px; font-size: 0.88rem; 
  box-shadow: var(--shadow-lg), 0 0 30px rgba(0,0,0,0.4); 
  transform: translateX(140%) scale(0.95); transition: transform 0.45s cubic-bezier(0.2,0.9,0.4,1), opacity 0.3s; 
  pointer-events:auto; display:flex; align-items:center; gap: 12px; 
  font-weight: 500; color: var(--text-bright);
  backdrop-filter: blur(16px);
}
.toast.show { transform: translateX(0) scale(1); }
.toast.success { border-left-color: var(--accent-bright); }
.toast.error { border-left-color: var(--danger); }
.toast.warning { border-left-color: var(--warning); }
.toast svg { width: 20px; height: 20px; flex-shrink:0; stroke: currentColor; }

@media (max-width: 900px) {
  .sidebar { transform: translateX(-100%); box-shadow: 12px 0 60px rgba(0,0,0,0.8); }
  .sidebar.open { transform: translateX(0); }
  .main { margin-left: 0; padding: 16px; }
  .hamburger { display: flex; }
  .clock-display { font-size: 1.7rem; }
  .grid { grid-template-columns: repeat(auto-fill, minmax(130px, 1fr)); gap: 0.8rem; }
  .proxy-apps-grid { grid-template-columns: repeat(auto-fill, minmax(120px, 1fr)); gap: 12px; }
  #toastContainer { left: 16px; right: 16px; max-width: none; }
  .account-header { flex-direction: column; text-align: center; }
  .music-player { left: 0; grid-template-columns: 1fr auto; grid-template-areas: "meta center" "meta right"; padding: 0.7rem 0.9rem; }
  .mp-left { grid-area: meta; }
  .mp-center { grid-area: center; }
  .mp-right { grid-area: right; }
  .mp-volume { display: none; }
  .music-viz { display: none; }
  .track-row { grid-template-columns: 30px 44px 1fr auto; gap: 0.6rem; }
  .track-row .track-album, .track-row .track-dur { display: none; }
  .track-row .track-art { width: 44px; height: 44px; }
}

/* —— Advanced TV / Movies —— */
.tv-toolbar { margin-bottom: 1.2rem; }
.tv-toolbar-row {
  display: flex; flex-wrap: wrap; gap: 0.45rem; justify-content: center;
  margin-bottom: 0.55rem; align-items: center;
}
.tv-select {
  padding: 0.5rem 0.9rem; border-radius: 20px; border: 1px solid var(--border);
  background: var(--surface); color: var(--text); cursor: pointer;
  font-weight: 600; font-family: inherit; font-size: 0.82rem;
}
.tv-search {
  padding: 0.5rem 1rem; border-radius: 20px; border: 1px solid var(--border);
  background: var(--surface); color: var(--text); font-family: inherit;
  width: min(220px, 100%); font-size: 0.85rem;
}
.tv-chip {
  padding: 0.4rem 0.85rem; border-radius: 999px; border: 1px solid var(--border);
  background: var(--surface); color: var(--text); cursor: pointer;
  font-weight: 600; font-family: inherit; font-size: 0.78rem;
  transition: border-color 0.2s, background 0.2s, transform 0.15s;
}
.tv-chip:hover { border-color: var(--accent); transform: translateY(-1px); }
.tv-chip.active {
  border-color: var(--accent); background: rgba(220,20,60,0.22);
  box-shadow: 0 0 12px rgba(220,20,60,0.2); color: var(--accent-bright);
}
.tv-hero {
  position: relative; border-radius: 18px; overflow: hidden;
  margin-bottom: 1.4rem; min-height: 220px;
  background: var(--surface); border: 1px solid var(--border);
}
.tv-hero-bg {
  position: absolute; inset: 0; background-size: cover; background-position: center top;
  filter: brightness(0.45); transform: scale(1.05);
}
.tv-hero-content {
  position: relative; z-index: 1; padding: 1.5rem 1.6rem;
  display: flex; gap: 1.2rem; align-items: flex-end; min-height: 220px;
}
.tv-hero-poster {
  width: 110px; border-radius: 10px; box-shadow: 0 8px 24px rgba(0,0,0,0.5);
  flex-shrink: 0;
}
.tv-hero-text { flex: 1; }
.tv-hero-text h2 { margin: 0 0 0.35rem; font-size: 1.45rem; text-shadow: 0 2px 8px rgba(0,0,0,0.6); }
.tv-hero-text p { margin: 0; font-size: 0.85rem; color: rgba(255,255,255,0.85); max-width: 520px;
  display: -webkit-box; -webkit-line-clamp: 3; -webkit-box-orient: vertical; overflow: hidden; line-height: 1.4; }
.tv-hero-actions { display: flex; gap: 0.5rem; margin-top: 0.75rem; flex-wrap: wrap; }
.tv-hero-actions button {
  padding: 0.45rem 0.95rem; border-radius: 10px; border: none; cursor: pointer;
  font-weight: 700; font-family: inherit; font-size: 0.82rem;
  background: var(--accent); color: #fff;
}
.tv-hero-actions button.secondary {
  background: rgba(255,255,255,0.12); color: var(--text); border: 1px solid var(--border);
}
.card .card-actions {
  display: flex; gap: 0.3rem; padding: 0 0.5rem 0.65rem; margin-top: auto;
}
.card .card-actions button {
  flex: 1; padding: 0.35rem; border-radius: 8px; border: 1px solid var(--border);
  background: rgba(220,20,60,0.15); color: var(--text); cursor: pointer;
  font-size: 0.75rem; font-weight: 600; font-family: inherit;
}
.card .card-actions button:hover { background: rgba(220,20,60,0.35); }
.card .wl-btn { flex: 0 0 auto; width: 36px; }
.card .wl-btn.on { color: #ffd166; border-color: rgba(255,209,102,0.4); }
.tv-detail-modal {
  position: fixed; inset: 0; z-index: 10000; background: rgba(0,0,0,0.72);
  backdrop-filter: blur(8px); display: flex; align-items: center; justify-content: center;
  padding: 1rem;
}
.tv-detail-panel {
  background: var(--surface); border: 1px solid var(--border); border-radius: 18px;
  max-width: 720px; width: 100%; max-height: 90vh; overflow-y: auto;
  position: relative; box-shadow: 0 20px 60px rgba(0,0,0,0.5);
}
.tv-detail-close {
  position: absolute; top: 10px; right: 12px; z-index: 2;
  width: 36px; height: 36px; border-radius: 50%; border: 1px solid var(--border);
  background: rgba(0,0,0,0.5); color: var(--text); font-size: 1.3rem; cursor: pointer;
}
.tv-detail-body { padding: 1.2rem 1.4rem 1.5rem; }
.tv-detail-header { display: flex; gap: 1rem; margin-bottom: 1rem; }
.tv-detail-header img { width: 120px; border-radius: 10px; flex-shrink: 0; }
.tv-ep-grid {
  display: grid; grid-template-columns: repeat(auto-fill, minmax(70px, 1fr));
  gap: 0.4rem; margin-top: 0.6rem;
}
.tv-ep-btn {
  padding: 0.5rem; border-radius: 8px; border: 1px solid var(--border);
  background: var(--bg); color: var(--text); cursor: pointer; font-weight: 600;
  font-family: inherit; font-size: 0.8rem;
}
.tv-ep-btn:hover { border-color: var(--accent); background: rgba(220,20,60,0.15); }
.tv-season-row { display: flex; gap: 0.4rem; flex-wrap: wrap; margin: 0.5rem 0; }
@media (max-width: 600px) {
  .tv-hero-content { flex-direction: column; align-items: flex-start; }
  .tv-detail-header { flex-direction: column; }
}


/* —— Chat rooms —— */
.chat-local-wrap {
  display: grid; grid-template-columns: 220px 1fr; gap: 1rem;
  max-width: 960px; margin: 0 auto; min-height: 420px;
}
.chat-local-sidebar {
  background: var(--surface); border: 1px solid var(--border); border-radius: 14px;
  padding: 1rem;
}
.chat-local-main {
  background: var(--surface); border: 1px solid var(--border); border-radius: 14px;
  display: flex; flex-direction: column; min-height: 420px; overflow: hidden;
}
.chat-messages {
  flex: 1; overflow-y: auto; padding: 1rem; display: flex; flex-direction: column; gap: 0.55rem;
}
.chat-bubble {
  max-width: 85%; padding: 0.5rem 0.75rem; border-radius: 12px; font-size: 0.88rem;
  line-height: 1.4; word-break: break-word;
}
.chat-bubble.me {
  align-self: flex-end; background: linear-gradient(135deg, var(--accent), #8b0018); color: #fff;
}
.chat-bubble.other {
  align-self: flex-start; background: rgba(255,255,255,0.06); border: 1px solid var(--border);
}
.chat-bubble .chat-meta {
  font-size: 0.68rem; opacity: 0.75; margin-bottom: 0.15rem;
}
.chat-bubble.system {
  align-self: center; background: transparent; color: var(--text-dim); font-size: 0.78rem;
  max-width: 100%; text-align: center;
}
.chat-compose {
  display: flex; gap: 0.5rem; padding: 0.75rem; border-top: 1px solid var(--border);
}
.chat-compose input {
  flex: 1; padding: 0.55rem 0.9rem; border-radius: 12px; border: 1px solid var(--border);
  background: var(--bg); color: var(--text); font-family: inherit;
}
@media (max-width: 700px) {
  .chat-local-wrap { grid-template-columns: 1fr; }
}


/* —— Dashboard —— */
.dash-stats {
  display: flex; flex-wrap: wrap; gap: 0.6rem; justify-content: center;
  margin-bottom: 1.2rem;
}
.dash-stat {
  padding: 0.45rem 0.9rem; border-radius: 999px; border: 1px solid var(--border);
  background: var(--surface); font-size: 0.78rem; color: var(--text-dim);
}
.dash-stat strong { color: var(--accent-bright); font-weight: 700; }
.dash-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 1rem;
  max-width: 1000px;
  margin: 0 auto;
}
.dash-card {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: 16px;
  padding: 1rem 1.1rem;
  backdrop-filter: blur(12px);
}
.dash-card-wide { grid-column: 1 / -1; }
.dash-card-head {
  display: flex; justify-content: space-between; align-items: center;
  margin-bottom: 0.75rem; gap: 0.5rem;
}
.dash-card-head strong { font-size: 0.95rem; }
.weather-body { min-height: 100px; }
.weather-main {
  display: flex; align-items: center; gap: 1rem;
}
.weather-temp { font-size: 2.4rem; font-weight: 800; line-height: 1; }
.weather-meta { font-size: 0.82rem; color: var(--text-dim); line-height: 1.45; }
.weather-search {
  display: flex; gap: 0.4rem; margin-top: 0.85rem;
}
.weather-search input {
  flex: 1; padding: 0.45rem 0.75rem; border-radius: 10px;
  border: 1px solid var(--border); background: var(--bg); color: var(--text);
  font-family: inherit; font-size: 0.85rem;
}
.notes-area {
  width: 100%; min-height: 140px; resize: vertical;
  padding: 0.75rem; border-radius: 12px;
  border: 1px solid var(--border); background: var(--bg); color: var(--text);
  font-family: inherit; font-size: 0.9rem; line-height: 1.45;
}
.continue-row {
  display: flex; gap: 0.75rem; overflow-x: auto; padding-bottom: 0.35rem;
  scrollbar-width: thin;
}
.continue-item {
  flex: 0 0 140px; cursor: pointer; border-radius: 12px; overflow: hidden;
  border: 1px solid var(--border); background: var(--bg);
  transition: transform 0.2s, border-color 0.2s;
}
.continue-item:hover { transform: translateY(-3px); border-color: var(--accent); }
.continue-item img {
  width: 100%; aspect-ratio: 2/3; object-fit: cover; display: block; background: #111;
}
.continue-item .ci-info { padding: 0.45rem 0.5rem; }
.continue-item .ci-title {
  font-size: 0.75rem; font-weight: 600; white-space: nowrap; overflow: hidden; text-overflow: ellipsis;
}
.continue-item .ci-sub { font-size: 0.68rem; color: var(--text-dim); margin-top: 0.15rem; }
.dash-quick {
  display: flex; flex-wrap: wrap; gap: 0.5rem;
}
.dash-quick-btn {
  display: inline-flex; align-items: center; gap: 0.4rem;
  padding: 0.5rem 0.9rem; border-radius: 12px;
  border: 1px solid var(--border); background: var(--bg); color: var(--text);
  cursor: pointer; font-family: inherit; font-weight: 600; font-size: 0.82rem;
  transition: border-color 0.2s, background 0.2s, transform 0.15s;
}
.dash-quick-btn:hover { border-color: var(--accent); background: rgba(220,20,60,0.12); transform: translateY(-1px); }
.dash-quick-btn svg { stroke: currentColor; fill: none; }
@media (max-width: 700px) {
  .dash-stats {
  display: flex; flex-wrap: wrap; gap: 0.6rem; justify-content: center;
  margin-bottom: 1.2rem;
}
.dash-stat {
  padding: 0.45rem 0.9rem; border-radius: 999px; border: 1px solid var(--border);
  background: var(--surface); font-size: 0.78rem; color: var(--text-dim);
}
.dash-stat strong { color: var(--accent-bright); font-weight: 700; }
.dash-grid { grid-template-columns: 1fr; }
  .dash-card-wide { grid-column: 1; }
}


/* —— Global advanced polish —— */
html { scroll-behavior: smooth; }
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
#scrollProgress {
  position: fixed; top: 0; left: 0; height: 3px; width: 0%;
  background: linear-gradient(90deg, var(--accent), #ff6b8a, var(--accent-bright));
  z-index: 9999; pointer-events: none;
  box-shadow: 0 0 12px rgba(220,20,60,0.6);
  transition: width 0.08s linear;
}
#netStatus {
  position: fixed; bottom: 16px; left: 16px; z-index: 2500;
  font-size: 0.72rem; padding: 0.35rem 0.7rem; border-radius: 999px;
  background: rgba(0,0,0,0.65); border: 1px solid var(--border);
  color: var(--text-dim); backdrop-filter: blur(8px);
  display: none; align-items: center; gap: 0.35rem;
}
#netStatus.offline { display: inline-flex; color: #ff8a8a; border-color: rgba(255,100,100,0.4); }
#netStatus.online-flash { display: inline-flex; color: #8affb0; }
#cmdPalette {
  display: none; position: fixed; inset: 0; z-index: 12000;
  background: rgba(0,0,0,0.72); backdrop-filter: blur(10px);
  align-items: flex-start; justify-content: center; padding-top: min(15vh, 120px);
}
#cmdPalette.show { display: flex; }
.cmd-box {
  width: min(560px, 94vw); background: var(--surface);
  border: 1px solid var(--border); border-radius: 16px;
  box-shadow: 0 24px 64px rgba(0,0,0,0.55); overflow: hidden;
}
.cmd-box input {
  width: 100%; padding: 1rem 1.2rem; border: none; border-bottom: 1px solid var(--border);
  background: transparent; color: var(--text); font-size: 1.05rem; font-family: inherit; outline: none;
}
.cmd-results { max-height: 360px; overflow-y: auto; }
.cmd-item {
  display: flex; align-items: center; gap: 0.75rem;
  padding: 0.7rem 1.2rem; cursor: pointer; font-size: 0.9rem;
  border: none; background: transparent; width: 100%; text-align: left;
  color: var(--text); font-family: inherit;
}
.cmd-item:hover, .cmd-item.active {
  background: rgba(220,20,60,0.18);
}
.cmd-item kbd {
  margin-left: auto; font-size: 0.68rem; color: var(--text-dim);
  border: 1px solid var(--border); padding: 0.15rem 0.4rem; border-radius: 6px;
}
#scrollTopBtn {
  position: fixed; bottom: 24px; right: 24px; z-index: 2000;
  width: 44px; height: 44px; border-radius: 50%;
  border: 1px solid var(--border); background: var(--surface);
  color: var(--text); cursor: pointer; display: none;
  align-items: center; justify-content: center;
  box-shadow: 0 8px 24px rgba(0,0,0,0.35);
  backdrop-filter: blur(10px); transition: transform 0.2s, opacity 0.2s;
}
#scrollTopBtn.show { display: flex; }
#scrollTopBtn:hover { transform: translateY(-3px); border-color: var(--accent); }
.main-content { position: relative; }
/* Skeleton shimmer upgrade */
@keyframes shimmer {
  0% { background-position: -200% 0; }
  100% { background-position: 200% 0; }
}
.skeleton {
  background: linear-gradient(90deg, rgba(255,255,255,0.04) 25%, rgba(255,255,255,0.1) 50%, rgba(255,255,255,0.04) 75%);
  background-size: 200% 100%;
  animation: shimmer 1.4s ease infinite;
}
/* Focus visible for a11y */
:focus-visible {
  outline: 2px solid var(--accent-bright);
  outline-offset: 2px;
}
button:focus:not(:focus-visible) { outline: none; }
/* Card enter via IO class */
.card.reveal {
  animation: cardIn 0.45s cubic-bezier(0.2, 0.9, 0.3, 1) both;
}
@keyframes cardIn {
  from { opacity: 0; transform: translateY(16px) scale(0.97); }
  to { opacity: 1; transform: none; }
}

/* ============================================================
   MEGA ENHANCEMENT PACK — v2
   ============================================================ */

/* Global search overlay */
#searchOverlay {
  display: none;
  position: fixed;
  inset: 0;
  z-index: 11000;
  background: rgba(0,0,0,0.85);
  backdrop-filter: blur(16px);
  -webkit-backdrop-filter: blur(16px);
  align-items: flex-start;
  justify-content: center;
  padding-top: min(10vh, 80px);
}
#searchOverlay.show { display: flex; }
.search-overlay-box {
  width: min(720px, 94vw);
  background: var(--surface-solid);
  border: 1px solid var(--border);
  border-radius: 20px;
  box-shadow: 0 30px 80px rgba(0,0,0,0.7), 0 0 0 1px rgba(220,20,60,0.15);
  overflow: hidden;
  max-height: 80vh;
  display: flex;
  flex-direction: column;
  animation: cmdIn 0.25s cubic-bezier(0.2, 0.9, 0.4, 1);
}
@keyframes cmdIn { from { opacity: 0; transform: translateY(-10px) scale(0.98); } to { opacity: 1; transform: none; } }
.search-overlay-input {
  padding: 1.1rem 1.3rem;
  border-bottom: 1px solid var(--border);
  display: flex;
  align-items: center;
  gap: 0.75rem;
}
.search-overlay-input svg { width: 22px; height: 22px; stroke: var(--accent-bright); flex-shrink: 0; }
.search-overlay-input input {
  flex: 1;
  background: transparent;
  border: none;
  color: var(--text-bright);
  font-size: 1.15rem;
  outline: none;
  font-family: inherit;
  font-weight: 500;
}
.search-overlay-input input::placeholder { color: var(--text-muted); }
.search-overlay-hint {
  font-size: 0.68rem;
  color: var(--text-dim);
  padding: 0.55rem 1.3rem;
  border-bottom: 1px solid var(--border-faint);
  display: flex;
  gap: 0.9rem;
  flex-wrap: wrap;
  font-weight: 600;
}
.search-overlay-hint kbd {
  background: var(--surface);
  border: 1px solid var(--border);
  padding: 0.1rem 0.4rem;
  border-radius: 5px;
  font-family: inherit;
  font-size: 0.68rem;
}
.search-overlay-results {
  overflow-y: auto;
  padding: 0.5rem 0;
  flex: 1;
  min-height: 80px;
}
.search-group-title {
  padding: 0.65rem 1.3rem 0.3rem;
  font-size: 0.68rem;
  text-transform: uppercase;
  letter-spacing: 1.8px;
  color: var(--text-dim);
  font-weight: 700;
}
.search-result-item {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  padding: 0.6rem 1.3rem;
  cursor: pointer;
  border: none;
  background: transparent;
  width: 100%;
  text-align: left;
  color: var(--text);
  font-family: inherit;
  transition: background 0.15s;
}
.search-result-item:hover, .search-result-item.active {
  background: rgba(220,20,60,0.18);
}
.search-result-item img {
  width: 36px;
  height: 36px;
  border-radius: 6px;
  object-fit: cover;
  background: var(--bg);
  flex-shrink: 0;
}
.search-result-item .sr-info { flex: 1; min-width: 0; }
.search-result-item .sr-title {
  font-weight: 600;
  font-size: 0.88rem;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  color: var(--text-bright);
}
.search-result-item .sr-sub {
  font-size: 0.72rem;
  color: var(--text-dim);
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
.search-result-item .sr-badge {
  font-size: 0.62rem;
  padding: 0.18rem 0.5rem;
  border-radius: 999px;
  background: rgba(220,20,60,0.2);
  color: var(--accent-bright);
  font-weight: 700;
  border: 1px solid var(--border);
  text-transform: uppercase;
  letter-spacing: 0.5px;
  flex-shrink: 0;
}
.search-empty {
  padding: 2rem 1.3rem;
  text-align: center;
  color: var(--text-dim);
  font-size: 0.88rem;
}

/* Shortcuts modal */
#shortcutsModal {
  display: none;
  position: fixed;
  inset: 0;
  z-index: 11500;
  background: rgba(0,0,0,0.85);
  backdrop-filter: blur(16px);
  align-items: center;
  justify-content: center;
  padding: 1rem;
}
#shortcutsModal.show { display: flex; }
.shortcuts-panel {
  width: min(680px, 94vw);
  max-height: 82vh;
  overflow-y: auto;
  background: var(--surface-solid);
  border: 1px solid var(--border);
  border-radius: 20px;
  padding: 1.5rem 1.6rem;
  box-shadow: 0 30px 80px rgba(0,0,0,0.7);
  animation: cmdIn 0.25s cubic-bezier(0.2, 0.9, 0.4, 1);
}
.shortcuts-panel h2 {
  font-size: 1.3rem;
  margin-bottom: 0.3rem;
  color: var(--text-bright);
  display: flex;
  align-items: center;
  gap: 0.6rem;
}
.shortcuts-panel h2 svg { width: 22px; height: 22px; stroke: var(--accent-bright); }
.shortcuts-panel .shortcuts-sub {
  font-size: 0.82rem;
  color: var(--text-dim);
  margin-bottom: 0.5rem;
}
.shortcuts-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 0.2rem 1.5rem;
  margin-top: 1rem;
}
.shortcut-category {
  grid-column: 1 / -1;
  font-size: 0.68rem;
  text-transform: uppercase;
  letter-spacing: 2px;
  color: var(--accent-bright);
  font-weight: 700;
  margin-top: 1rem;
  padding-bottom: 0.3rem;
  border-bottom: 1px solid var(--border-faint);
}
.shortcut-category:first-child { margin-top: 0; }
.shortcut-row {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0.5rem 0;
  border-bottom: 1px solid var(--border-faint);
  font-size: 0.85rem;
  color: var(--text);
}
.shortcut-keys {
  display: flex;
  gap: 0.3rem;
  flex-shrink: 0;
}
.shortcut-keys kbd {
  background: var(--surface);
  border: 1px solid var(--border);
  border-bottom-width: 2px;
  padding: 0.2rem 0.55rem;
  border-radius: 6px;
  font-size: 0.72rem;
  font-weight: 700;
  font-family: inherit;
  color: var(--text-bright);
  min-width: 22px;
  text-align: center;
}

/* Music queue panel */
#musicQueuePanel {
  position: fixed;
  top: 0;
  right: 0;
  bottom: 0;
  width: min(380px, 92vw);
  background: var(--surface-solid);
  border-left: 1px solid var(--border);
  z-index: 1500;
  transform: translateX(100%);
  transition: transform 0.35s cubic-bezier(0.2, 0.9, 0.4, 1);
  display: flex;
  flex-direction: column;
  box-shadow: -12px 0 40px rgba(0,0,0,0.6);
}
#musicQueuePanel.show { transform: translateX(0); }
.mq-header {
  padding: 1rem 1.2rem;
  border-bottom: 1px solid var(--border);
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.mq-header strong { font-size: 1rem; color: var(--text-bright); }
.mq-body { flex: 1; overflow-y: auto; padding: 0.5rem; }
.mq-section {
  font-size: 0.68rem;
  text-transform: uppercase;
  letter-spacing: 1.8px;
  color: var(--text-dim);
  font-weight: 700;
  padding: 0.6rem 0.7rem 0.3rem;
}
.mq-item {
  display: flex;
  align-items: center;
  gap: 0.7rem;
  padding: 0.55rem 0.7rem;
  border-radius: 10px;
  cursor: pointer;
  transition: background 0.15s;
  border: 1px solid transparent;
}
.mq-item:hover { background: var(--surface-hover); border-color: var(--border); }
.mq-item.current { background: rgba(220,20,60,0.15); border-color: var(--accent); }
.mq-item img {
  width: 40px;
  height: 40px;
  border-radius: 6px;
  object-fit: cover;
  flex-shrink: 0;
  background: var(--bg);
}
.mq-item .mq-num {
  width: 22px;
  text-align: center;
  font-size: 0.72rem;
  color: var(--text-dim);
  font-variant-numeric: tabular-nums;
  flex-shrink: 0;
}
.mq-item .mq-info { flex: 1; min-width: 0; }
.mq-item .mq-title {
  font-size: 0.85rem;
  font-weight: 600;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  color: var(--text-bright);
}
.mq-item .mq-artist {
  font-size: 0.72rem;
  color: var(--text-dim);
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
.mq-item .mq-remove {
  width: 28px;
  height: 28px;
  border-radius: 50%;
  border: none;
  background: transparent;
  color: var(--text-dim);
  cursor: pointer;
  flex-shrink: 0;
  font-size: 1rem;
  line-height: 1;
}
.mq-item .mq-remove:hover { background: rgba(220,20,60,0.25); color: var(--accent-bright); }
.mq-empty {
  padding: 2rem 1rem;
  text-align: center;
  color: var(--text-dim);
  font-size: 0.85rem;
}

/* Recently viewed bar */
#recentBar {
  position: fixed;
  bottom: 16px;
  left: 50%;
  transform: translateX(-50%) translateY(140%);
  z-index: 1400;
  display: flex;
  gap: 0.4rem;
  padding: 0.5rem 0.7rem;
  background: rgba(15,0,8,0.94);
  border: 1px solid var(--border);
  border-radius: 999px;
  backdrop-filter: blur(16px);
  -webkit-backdrop-filter: blur(16px);
  box-shadow: 0 12px 40px rgba(0,0,0,0.5), 0 0 0 1px rgba(220,20,60,0.08);
  transition: transform 0.4s cubic-bezier(0.2, 0.9, 0.4, 1);
  align-items: center;
  max-width: 92vw;
  overflow: hidden;
}
#recentBar.show { transform: translateX(-50%) translateY(0); }
#recentBar .rb-label {
  font-size: 0.68rem;
  color: var(--text-dim);
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 1.2px;
  padding: 0 0.4rem;
  flex-shrink: 0;
}
.rb-item {
  display: flex;
  align-items: center;
  gap: 0.4rem;
  padding: 0.3rem 0.7rem;
  border-radius: 999px;
  background: rgba(220,20,60,0.12);
  border: 1px solid var(--border-faint);
  cursor: pointer;
  font-size: 0.75rem;
  color: var(--text);
  transition: all 0.15s;
  max-width: 160px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  flex-shrink: 0;
  font-weight: 600;
}
.rb-item:hover { background: rgba(220,20,60,0.28); border-color: var(--accent); color: var(--text-bright); }
.rb-close {
  background: transparent;
  border: none;
  color: var(--text-dim);
  cursor: pointer;
  font-size: 1rem;
  padding: 0 0.2rem;
  flex-shrink: 0;
  line-height: 1;
}
.rb-close:hover { color: var(--accent-bright); }

/* Toast with actions */
.toast { flex-direction: column; align-items: stretch !important; padding: 0.85rem 1rem !important; }
.toast-row { display: flex; align-items: center; gap: 10px; width: 100%; }
.toast-actions {
  display: flex;
  gap: 0.4rem;
  margin-top: 0.55rem;
  width: 100%;
  flex-wrap: wrap;
}
.toast-action-btn {
  padding: 0.35rem 0.75rem;
  border-radius: 8px;
  border: 1px solid var(--border);
  background: rgba(220,20,60,0.15);
  color: var(--text-bright);
  cursor: pointer;
  font-family: inherit;
  font-size: 0.75rem;
  font-weight: 700;
  transition: all 0.15s;
}
.toast-action-btn:hover { background: rgba(220,20,60,0.35); border-color: var(--accent); }

/* Proxy tabs */
.proxy-tabs {
  display: flex;
  gap: 0.35rem;
  margin-bottom: 0.8rem;
  padding-bottom: 0.5rem;
  border-bottom: 1px solid var(--border);
  overflow-x: auto;
  scrollbar-width: thin;
  max-width: 900px;
  margin-left: auto;
  margin-right: auto;
  min-height: 30px;
  align-items: center;
}
.proxy-tabs::-webkit-scrollbar { height: 5px; }
.proxy-tab {
  display: flex;
  align-items: center;
  gap: 0.4rem;
  padding: 0.42rem 0.8rem;
  border-radius: 8px;
  border: 1px solid var(--border);
  background: var(--surface);
  color: var(--text);
  cursor: pointer;
  font-family: inherit;
  font-size: 0.78rem;
  font-weight: 600;
  white-space: nowrap;
  max-width: 180px;
  transition: background 0.15s, border-color 0.15s;
  flex-shrink: 0;
}
.proxy-tab.active { background: var(--accent); color: #fff; border-color: var(--accent-bright); box-shadow: 0 4px 14px rgba(220,20,60,0.45); }
.proxy-tab .pt-title { overflow: hidden; text-overflow: ellipsis; }
.proxy-tab .pt-close {
  background: transparent;
  border: none;
  color: inherit;
  cursor: pointer;
  padding: 0;
  margin-left: 0.2rem;
  opacity: 0.7;
  font-size: 0.95rem;
  line-height: 1;
}
.proxy-tab .pt-close:hover { opacity: 1; }
.proxy-tab-add {
  padding: 0.42rem 0.75rem;
  border-radius: 8px;
  border: 1px dashed var(--border);
  background: transparent;
  color: var(--text-dim);
  cursor: pointer;
  font-family: inherit;
  font-weight: 700;
  font-size: 0.85rem;
  flex-shrink: 0;
}
.proxy-tab-add:hover { color: var(--accent-bright); border-color: var(--accent); }

/* Better TV detail tabs */
.tv-detail-tabs {
  display: flex;
  gap: 0.2rem;
  border-bottom: 1px solid var(--border);
  margin-bottom: 1rem;
  overflow-x: auto;
  scrollbar-width: thin;
}
.tv-detail-tab {
  padding: 0.6rem 0.95rem;
  border: none;
  background: transparent;
  color: var(--text-dim);
  cursor: pointer;
  font-family: inherit;
  font-weight: 700;
  font-size: 0.82rem;
  border-bottom: 2px solid transparent;
  white-space: nowrap;
  transition: color 0.15s, border-color 0.15s;
}
.tv-detail-tab:hover { color: var(--text-bright); }
.tv-detail-tab.active {
  color: var(--accent-bright);
  border-bottom-color: var(--accent);
}
.tv-similar-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(90px, 1fr));
  gap: 0.6rem;
}
.tv-similar-item {
  cursor: pointer;
  border-radius: 8px;
  overflow: hidden;
  transition: transform 0.2s;
}
.tv-similar-item:hover { transform: scale(1.06); }
.tv-similar-item img {
  width: 100%;
  aspect-ratio: 2/3;
  object-fit: cover;
  border-radius: 8px;
  background: #111;
  display: block;
}
.tv-similar-item .tv-similar-title {
  font-size: 0.7rem;
  margin-top: 0.35rem;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  color: var(--text-bright);
  font-weight: 600;
}

/* Player extras */
#playerModal .pip-btn {
  padding: 0.55rem 1rem;
  border-radius: 10px;
  border: 1px solid var(--border);
  background: var(--surface);
  color: var(--text);
  font-size: 0.85rem;
  cursor: pointer;
  font-weight: 600;
  font-family: inherit;
}
#playerModal .pip-btn:hover { background: var(--surface-hover); border-color: var(--accent); }

/* FAB */
#fabMenu {
  position: fixed;
  bottom: 24px;
  right: 24px;
  z-index: 1900;
  display: flex;
  flex-direction: column-reverse;
  align-items: flex-end;
  gap: 0.5rem;
}
.fab-main {
  width: 56px;
  height: 56px;
  border-radius: 50%;
  border: none;
  background: linear-gradient(135deg, var(--accent-bright), var(--accent));
  color: #fff;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 12px 40px rgba(220,20,60,0.55), inset 0 1px 0 rgba(255,255,255,0.2);
  transition: transform 0.35s cubic-bezier(0.34, 1.56, 0.64, 1);
  font-size: 1.6rem;
  font-weight: 300;
  line-height: 1;
}
.fab-main:hover { transform: scale(1.08); }
#fabMenu.open .fab-main { transform: rotate(45deg); }
.fab-item {
  padding: 0.65rem 1rem;
  border-radius: 999px;
  border: 1px solid var(--border);
  background: var(--surface-solid);
  color: var(--text-bright);
  cursor: pointer;
  display: flex;
  align-items: center;
  gap: 0.5rem;
  font-family: inherit;
  font-size: 0.82rem;
  font-weight: 700;
  box-shadow: 0 8px 24px rgba(0,0,0,0.5);
  opacity: 0;
  transform: translateY(20px) scale(0.85);
  pointer-events: none;
  transition: all 0.28s cubic-bezier(0.2, 0.9, 0.4, 1);
  white-space: nowrap;
}
.fab-item svg { width: 16px; height: 16px; stroke: currentColor; fill: none; stroke-width: 2; }
#fabMenu.open .fab-item {
  opacity: 1;
  transform: translateY(0) scale(1);
  pointer-events: auto;
}
#fabMenu.open .fab-item:nth-of-type(1) { transition-delay: 0.04s; }
#fabMenu.open .fab-item:nth-of-type(2) { transition-delay: 0.08s; }
#fabMenu.open .fab-item:nth-of-type(3) { transition-delay: 0.12s; }
#fabMenu.open .fab-item:nth-of-type(4) { transition-delay: 0.16s; }
#fabMenu.open .fab-item:nth-of-type(5) { transition-delay: 0.20s; }
.fab-item:hover { border-color: var(--accent-bright); background: var(--surface-hover); }

/* Watch party bar */
#watchPartyBar {
  display: none;
  position: fixed;
  bottom: 16px;
  left: 50%;
  transform: translateX(-50%);
  background: rgba(15,0,8,0.96);
  border: 1px solid var(--accent);
  border-radius: 14px;
  padding: 0.6rem 1rem;
  z-index: 2500;
  align-items: center;
  gap: 0.75rem;
  backdrop-filter: blur(16px);
  box-shadow: 0 12px 40px rgba(0,0,0,0.6), 0 0 30px rgba(220,20,60,0.35);
  font-size: 0.82rem;
  color: var(--text-bright);
  font-weight: 600;
}
#watchPartyBar.show { display: flex; }
#watchPartyBar .wp-dot {
  width: 9px; height: 9px; border-radius: 50%;
  background: #22c55e;
  box-shadow: 0 0 10px #22c55e;
  animation: pulse 1.5s infinite;
}
#watchPartyBar button {
  padding: 0.35rem 0.75rem;
  border-radius: 8px;
  border: 1px solid var(--border);
  background: var(--surface);
  color: var(--text);
  cursor: pointer;
  font-family: inherit;
  font-weight: 700;
  font-size: 0.75rem;
}

/* Better skeleton block */
.skeleton-block {
  background: linear-gradient(90deg, rgba(220,20,60,0.04) 0%, rgba(220,20,60,0.14) 50%, rgba(220,20,60,0.04) 100%);
  background-size: 200% 100%;
  animation: shimmer 1.4s infinite;
  border-radius: 8px;
}

/* Content stat pills on dashboard */
.dash-stat.pulse {
  animation: statPulse 2.4s ease-in-out infinite;
}
@keyframes statPulse {
  0%, 100% { box-shadow: 0 0 0 0 rgba(220,20,60,0.2); }
  50% { box-shadow: 0 0 0 6px rgba(220,20,60,0); }
}

/* Improved checkbox/toggle style */
.switch {
  position: relative;
  display: inline-block;
  width: 42px;
  height: 24px;
  flex-shrink: 0;
}
.switch input { opacity: 0; width: 0; height: 0; }
.switch .slider {
  position: absolute;
  inset: 0;
  cursor: pointer;
  background: rgba(220,20,60,0.2);
  border: 1px solid var(--border);
  border-radius: 999px;
  transition: 0.3s;
}
.switch .slider::before {
  content: '';
  position: absolute;
  height: 16px; width: 16px;
  left: 3px; bottom: 3px;
  background: #fff;
  border-radius: 50%;
  transition: 0.3s;
}
.switch input:checked + .slider { background: var(--accent); border-color: var(--accent-bright); }
.switch input:checked + .slider::before { transform: translateX(18px); }

/* Responsive tweaks for enhancements */
@media (max-width: 600px) {
  .shortcuts-grid { grid-template-columns: 1fr; }
  #fabMenu { bottom: 90px; right: 16px; }
  .search-overlay-box { max-height: 88vh; border-radius: 16px; }
  .shortcuts-panel { padding: 1.2rem; border-radius: 16px; }
  #recentBar { bottom: 80px; }
  #watchPartyBar { bottom: 80px; font-size: 0.75rem; padding: 0.5rem 0.8rem; }
  #watchPartyBar span.wp-label { display: none; }
}

@media (prefers-reduced-motion: reduce) {
  #musicQueuePanel, #recentBar, #fabMenu .fab-item, #searchOverlay .search-overlay-box {
    transition: none !important;
    animation: none !important;
  }
}
</style>
</head>
<body>
<div id="scrollProgress" aria-hidden="true"></div>
<div id="netStatus" role="status"><span class="dot" style="width:8px;height:8px;border-radius:50%;background:currentColor;"></span><span id="netStatusText">Offline</span></div>
<div id="cmdPalette" role="dialog" aria-label="Command palette" onclick="if(event.target===this)closeCmdPalette()">
  <div class="cmd-box">
    <input type="text" id="cmdInput" placeholder="Type a command or search…" autocomplete="off" oninput="filterCmdPalette()" onkeydown="cmdKey(event)">
    <div class="cmd-results" id="cmdResults"></div>
  </div>
</div>

<!-- ENHANCEMENT: Global search overlay -->
<div id="searchOverlay" role="dialog" aria-label="Search" onclick="if(event.target===this)closeSearchOverlay()">
  <div class="search-overlay-box">
    <div class="search-overlay-input">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round"><circle cx="11" cy="11" r="8"/><line x1="21" y1="21" x2="16.65" y2="16.65"/></svg>
      <input type="text" id="searchOverlayInput" placeholder="Search games, movies, shows, anime, music, cloud…" autocomplete="off" oninput="doGlobalSearch(this.value)" onkeydown="searchOverlayKey(event)">
    </div>
    <div class="search-overlay-hint">
      <span><kbd>↑</kbd><kbd>↓</kbd> navigate</span>
      <span><kbd>Enter</kbd> open</span>
      <span><kbd>Esc</kbd> close</span>
    </div>
    <div class="search-overlay-results" id="searchOverlayResults"></div>
  </div>
</div>

<!-- ENHANCEMENT: Keyboard shortcuts modal -->
<div id="shortcutsModal" role="dialog" aria-label="Keyboard shortcuts" onclick="if(event.target===this)closeShortcutsModal()">
  <div class="shortcuts-panel">
    <h2><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round"><rect x="3" y="6" width="18" height="12" rx="2"/><line x1="7" y1="10" x2="7" y2="10"/><line x1="11" y1="10" x2="11" y2="10"/><line x1="15" y1="10" x2="15" y2="10"/><line x1="7" y1="14" x2="17" y2="14"/></svg> Keyboard shortcuts</h2>
    <p class="shortcuts-sub">Master Xorwel with these combos.</p>
    <div class="shortcuts-grid" id="shortcutsGrid"></div>
    <div style="margin-top:1.2rem; text-align:center;">
      <button class="tv-chip active" onclick="closeShortcutsModal()">Got it</button>
    </div>
  </div>
</div>

<button id="scrollTopBtn" title="Back to top" onclick="window.scrollTo({top:0,behavior:'smooth'})" aria-label="Scroll to top">↑</button>

<!-- ENHANCEMENT: FAB quick actions -->
<div id="fabMenu">
  <button class="fab-main" onclick="toggleFab()" aria-label="Quick actions">+</button>
  <button class="fab-item" onclick="openCmdPalette();closeFab()"><svg viewBox="0 0 24 24"><use href="#i-settings"/></svg> Commands</button>
  <button class="fab-item" onclick="openSearchOverlay();closeFab()"><svg viewBox="0 0 24 24"><use href="#i-search"/></svg> Search</button>
  <button class="fab-item" onclick="openShortcutsModal();closeFab()"><svg viewBox="0 0 24 24"><use href="#i-list"/></svg> Shortcuts</button>
  <button class="fab-item" onclick="switchTab('ai');closeFab()"><svg viewBox="0 0 24 24"><use href="#i-bot"/></svg> Ask AI</button>
  <button class="fab-item" onclick="switchTab('music');closeFab()"><svg viewBox="0 0 24 24"><use href="#i-music"/></svg> Music</button>
</div>

<!-- ENHANCEMENT: Recently viewed bar -->
<div id="recentBar" aria-hidden="true">
  <span class="rb-label">Recent</span>
  <div id="recentBarItems" style="display:flex;gap:0.4rem;overflow:hidden;"></div>
  <button class="rb-close" onclick="hideRecentBar()" title="Hide">×</button>
</div>

<!-- ENHANCEMENT: Watch party bar -->
<div id="watchPartyBar">
  <span class="wp-dot"></span>
  <span class="wp-label">Watch party active</span>
  <span id="wpMemberCount" style="color:var(--text-dim);font-weight:500;">1 member</span>
  <button onclick="endWatchParty()">End</button>
</div>

<!-- ENHANCEMENT: Music queue panel -->
<div id="musicQueuePanel" aria-label="Music queue">
  <div class="mq-header">
    <strong>Queue</strong>
    <button class="tv-chip" onclick="closeMusicQueue()">Close</button>
  </div>
  <div class="mq-body" id="mqBody"></div>
</div>

<!-- SVG ICON SPRITES -->
<svg style="display:none;" xmlns="http://www.w3.org/2000/svg">
  <symbol id="i-games" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><line x1="6" y1="12" x2="10" y2="12"/><line x1="8" y1="10" x2="8" y2="14"/><line x1="15" y1="13" x2="15.01" y2="13"/><line x1="18" y1="11" x2="18.01" y2="11"/><path d="M17.32 5H6.68a4 4 0 0 0-3.978 3.59c-.006.052-.01.101-.017.152C2.604 9.416 2 14.456 2 16a3 3 0 0 0 3 3c1 0 1.5-.5 2-1l1.414-1.414A2 2 0 0 1 9.828 16h4.344a2 2 0 0 1 1.414.586L17 18c.5.5 1 1 2 1a3 3 0 0 0 3-3c0-1.545-.604-6.584-.685-7.258-.007-.05-.011-.1-.017-.151A4 4 0 0 0 17.32 5z"/></symbol>
  <symbol id="i-cloud" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M17.5 19a4.5 4.5 0 0 0 0-9 6 6 0 0 0-11.6-1.6A4 4 0 0 0 6 19h11.5z"/></symbol>
  <symbol id="i-tv" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="2" y="7" width="20" height="15" rx="2" ry="2"/><polyline points="17 2 12 7 7 2"/></symbol>
  <symbol id="i-music" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M9 18V5l12-2v13"/><circle cx="6" cy="18" r="3"/><circle cx="18" cy="16" r="3"/></symbol>
  <symbol id="i-globe" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="10"/><line x1="2" y1="12" x2="22" y2="12"/><path d="M12 2a15.3 15.3 0 0 1 4 10 15.3 15.3 0 0 1-4 10 15.3 15.3 0 0 1-4-10 15.3 15.3 0 0 1 4-10z"/></symbol>
  <symbol id="i-bookmark" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M19 21l-7-5-7 5V5a2 2 0 0 1 2-2h10a2 2 0 0 1 2 2z"/></symbol>
  <symbol id="i-bot" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="4" y="8" width="16" height="12" rx="2"/><path d="M12 8V4"/><circle cx="12" cy="3" r="1"/><line x1="9" y1="13" x2="9.01" y2="13"/><line x1="15" y1="13" x2="15.01" y2="13"/><line x1="9" y1="16" x2="15" y2="16"/></symbol>
  <symbol id="i-settings" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="3"/><path d="M19.4 15a1.65 1.65 0 0 0 .33 1.82l.06.06a2 2 0 0 1-2.83 2.83l-.06-.06a1.65 1.65 0 0 0-1.82-.33 1.65 1.65 0 0 0-1 1.51V21a2 2 0 0 1-4 0v-.09A1.65 1.65 0 0 0 9 19.4a1.65 1.65 0 0 0-1.82.33l-.06.06a2 2 0 0 1-2.83-2.83l.06-.06a1.65 1.65 0 0 0 .33-1.82 1.65 1.65 0 0 0-1.51-1H3a2 2 0 0 1 0-4h.09A1.65 1.65 0 0 0 4.6 9a1.65 1.65 0 0 0-.33-1.82l-.06-.06a2 2 0 0 1 2.83-2.83l.06.06a1.65 1.65 0 0 0 1.82.33H9a1.65 1.65 0 0 0 1-1.51V3a2 2 0 0 1 4 0v.09a1.65 1.65 0 0 0 1 1.51 1.65 1.65 0 0 0 1.82-.33l.06-.06a2 2 0 0 1 2.83 2.83l-.06.06a1.65 1.65 0 0 0-.33 1.82V9a1.65 1.65 0 0 0 1.51 1H21a2 2 0 0 1 0 4h-.09a1.65 1.65 0 0 0-1.51 1z"/></symbol>
  <symbol id="i-user" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M20 21v-2a4 4 0 0 0-4-4H8a4 4 0 0 0-4 4v2"/><circle cx="12" cy="7" r="4"/></symbol>
  <symbol id="i-play" viewBox="0 0 24 24"><path d="M8 5v14l11-7z" fill="currentColor"/></symbol>
  <symbol id="i-close" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round"><line x1="18" y1="6" x2="6" y2="18"/><line x1="6" y1="6" x2="18" y2="18"/></symbol>
  <symbol id="i-menu" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round"><line x1="3" y1="12" x2="21" y2="12"/><line x1="3" y1="6" x2="21" y2="6"/><line x1="3" y1="18" x2="21" y2="18"/></symbol>
  <symbol id="i-arrow-left" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><polyline points="15 18 9 12 15 6"/></symbol>
  <symbol id="i-arrow-right" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><polyline points="9 18 15 12 9 6"/></symbol>
  <symbol id="i-refresh" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><polyline points="23 4 23 10 17 10"/><polyline points="1 20 1 14 7 14"/><path d="M3.51 9a9 9 0 0 1 14.85-3.36L23 10M1 14l4.64 4.36A9 9 0 0 0 20.49 15"/></symbol>
  <symbol id="i-home" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M3 9l9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z"/><polyline points="9 22 9 12 15 12 15 22"/></symbol>
  <symbol id="i-skip" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><polygon points="5 4 15 12 5 20 5 4" fill="currentColor"/><line x1="19" y1="5" x2="19" y2="19"/></symbol>
  <symbol id="i-captions" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round"><rect x="2" y="6" width="20" height="14" rx="2"/><line x1="7" y1="12" x2="10" y2="12"/><line x1="14" y1="12" x2="17" y2="12"/><line x1="7" y1="16" x2="17" y2="16"/></symbol>
  <symbol id="i-trash" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><polyline points="3 6 5 6 21 6"/><path d="M19 6v14a2 2 0 0 1-2 2H7a2 2 0 0 1-2-2V6"/><path d="M8 6V4a2 2 0 0 1 2-2h4a2 2 0 0 1 2 2v2"/></symbol>
  <symbol id="i-palette" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="13.5" cy="6.5" r=".5"/><circle cx="17.5" cy="10.5" r=".5"/><circle cx="8.5" cy="7.5" r=".5"/><circle cx="6.5" cy="12.5" r=".5"/><path d="M12 2C6.5 2 2 6.5 2 12s4.5 10 10 10c.926 0 1.648-.746 1.648-1.688 0-.437-.18-.835-.437-1.125-.29-.289-.438-.652-.438-1.125a1.64 1.64 0 0 1 1.668-1.668h1.996c3.051 0 5.555-2.503 5.555-5.554C21.965 6.012 17.461 2 12 2z"/></symbol>
  <symbol id="i-incognito" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="8" cy="15" r="4"/><circle cx="16" cy="15" r="4"/><path d="M4 15l1-8 5-2 4 1 5-1 1 10"/></symbol>
  <symbol id="i-check" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="3" stroke-linecap="round" stroke-linejoin="round"><polyline points="20 6 9 17 4 12"/></symbol>
  <symbol id="i-info" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="10"/><line x1="12" y1="16" x2="12" y2="12"/><line x1="12" y1="8" x2="12.01" y2="8"/></symbol>
  <symbol id="i-alert" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M10.29 3.86L1.82 18a2 2 0 0 0 1.71 3h16.94a2 2 0 0 0 1.71-3L13.71 3.86a2 2 0 0 0-3.42 0z"/><line x1="12" y1="9" x2="12" y2="13"/><line x1="12" y1="17" x2="12.01" y2="17"/></symbol>
  <symbol id="i-logout" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M9 21H5a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h4"/><polyline points="16 17 21 12 16 7"/><line x1="21" y1="12" x2="9" y2="12"/></symbol>
  <symbol id="i-search" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round"><circle cx="11" cy="11" r="8"/><line x1="21" y1="21" x2="16.65" y2="16.65"/></symbol>
  <symbol id="i-heart" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M20.84 4.61a5.5 5.5 0 0 0-7.78 0L12 5.67l-1.06-1.06a5.5 5.5 0 0 0-7.78 7.78l1.06 1.06L12 21.23l7.78-7.78 1.06-1.06a5.5 5.5 0 0 0 0-7.78z"/></symbol>
    <symbol id="i-chat" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M21 15a2 2 0 0 1-2 2H7l-4 4V5a2 2 0 0 1 2-2h14a2 2 0 0 1 2 2z"/></symbol>
  <symbol id="i-list" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round"><line x1="8" y1="6" x2="21" y2="6"/><line x1="8" y1="12" x2="21" y2="12"/><line x1="8" y1="18" x2="21" y2="18"/><line x1="3" y1="6" x2="3.01" y2="6"/><line x1="3" y1="12" x2="3.01" y2="12"/><line x1="3" y1="18" x2="3.01" y2="18"/></symbol>
  <symbol id="i-pip" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="2" y="4" width="20" height="16" rx="2"/><rect x="12" y="12" width="8" height="6" rx="1" fill="currentColor"/></symbol>
</svg>

<!-- SIDEBAR -->
<aside class="sidebar" id="sidebar">
  <div class="sidebar-logo">xorwel</div>
  <nav class="sidebar-nav">
    <div class="nav-item active" data-tab="home"><svg><use href="#i-home"/></svg> Home</div>
    <div class="nav-item" data-tab="games"><svg><use href="#i-games"/></svg> Games</div>
    <div class="nav-item" data-tab="cloud"><svg><use href="#i-cloud"/></svg> Cloud Gaming</div>
    <div class="nav-item" data-tab="tv"><svg><use href="#i-tv"/></svg> TV & Movies</div>
    <div class="nav-item" data-tab="music"><svg><use href="#i-music"/></svg> Music</div>
    <div class="nav-item" data-tab="proxy"><svg><use href="#i-globe"/></svg> Proxy</div>
    <div class="nav-item" data-tab="bookmarks"><svg><use href="#i-bookmark"/></svg> Bookmarks</div>
    <div class="nav-item" data-tab="chat"><svg><use href="#i-chat"/></svg> Chat</div>
    <div class="nav-item" data-tab="ai"><svg><use href="#i-bot"/></svg> AI</div>
    <div class="nav-item" data-tab="settings"><svg><use href="#i-settings"/></svg> Settings</div>
  </nav>
  <button class="account-btn" onclick="switchTab('account')" id="accountBtn">
    <div class="avatar-mini" id="avatarMini">?</div>
    <span id="accountBtnText">Sign In</span>
  </button>
</aside>

<!-- MAIN -->
<main class="main main-content">
  <div class="top-bar">
    <div class="top-left-group">
      <button class="back-btn" onclick="goHome()" title="Back to Games">
        <svg viewBox="0 0 24 24"><use href="#i-arrow-left"/></svg>
      </button>
      <button class="hamburger" onclick="toggleSidebar()"><svg width="20" height="20" viewBox="0 0 24 24"><use href="#i-menu"/></svg></button>
      <div class="clock-display" id="clockDisplay">00:00:00</div>
    </div>
    <div class="search-bar">
      <input type="text" id="searchInput" placeholder="Search games, TV, movies... (Ctrl+K)" onfocus="openSearchOverlay()" readonly>
      <button onclick="openSearchOverlay()"><svg width="16" height="16" viewBox="0 0 24 24"><use href="#i-search"/></svg> Search</button>
    </div>
  </div>

  <!-- HOME / DASHBOARD -->
  <section id="tab-home" class="content-section active">
    <div class="section-title"><svg><use href="#i-home"/></svg> Dashboard</div>
    <p style="text-align:center;color:var(--text-dim);margin:0 auto 1.2rem;font-size:0.9rem;">Welcome to Xorwel — weather, notes, and pick up where you left off.</p>
    <div id="dashStats" class="dash-stats"></div>
    <div class="dash-grid">
      <!-- Weather -->
      <div class="dash-card" id="weatherCard">
        <div class="dash-card-head">
          <strong>Weather</strong>
          <button class="tv-chip" onclick="refreshWeather()" title="Refresh">↻</button>
        </div>
        <div id="weatherBody" class="weather-body">
          <div class="loading"><div class="spinner"></div>Detecting location…</div>
        </div>
        <div class="weather-search">
          <input type="text" id="weatherCity" placeholder="City name…" onkeydown="if(event.key==='Enter')searchWeatherCity()">
          <button class="tv-chip active" onclick="searchWeatherCity()">Go</button>
        </div>
      </div>
      <!-- Notes -->
      <div class="dash-card" id="notesCard">
        <div class="dash-card-head">
          <strong>Quick notes</strong>
          <button class="tv-chip" onclick="clearNotes()" title="Clear">Clear</button>
        </div>
        <textarea id="notesArea" class="notes-area" placeholder="Jot something down… autosaves locally" oninput="saveNotes()"></textarea>
        <div style="font-size:0.7rem;color:var(--text-dim);margin-top:0.35rem;" id="notesStatus">Saved on this device</div>
      </div>
      <!-- Continue watching -->
      <div class="dash-card dash-card-wide" id="continueCard">
        <div class="dash-card-head">
          <strong>Continue watching</strong>
          <button class="tv-chip" onclick="switchTab('tv')">Browse all</button>
        </div>
        <div id="continueWatchRow" class="continue-row">
          <div class="loading" style="padding:1rem;">Play a movie or show — working sources are remembered here.</div>
        </div>
      </div>
      <!-- Quick links -->
      <div class="dash-card dash-card-wide">
        <div class="dash-card-head"><strong>Quick launch</strong></div>
        <div class="dash-quick">
          <button class="dash-quick-btn" onclick="switchTab('games')"><svg width="18" height="18"><use href="#i-games"/></svg> Games</button>
          <button class="dash-quick-btn" onclick="switchTab('tv')"><svg width="18" height="18"><use href="#i-tv"/></svg> Movies</button>
          <button class="dash-quick-btn" onclick="switchTab('music')"><svg width="18" height="18"><use href="#i-music"/></svg> Music</button>
          <button class="dash-quick-btn" onclick="switchTab('cloud')"><svg width="18" height="18"><use href="#i-cloud"/></svg> Cloud</button>
          <button class="dash-quick-btn" onclick="switchTab('chat')"><svg width="18" height="18"><use href="#i-chat"/></svg> Chat</button>
          <button class="dash-quick-btn" onclick="switchTab('ai')"><svg width="18" height="18"><use href="#i-bot"/></svg> AI</button>
          <button class="dash-quick-btn" onclick="switchTab('proxy')"><svg width="18" height="18"><use href="#i-globe"/></svg> Proxy</button>
          <button class="dash-quick-btn" onclick="switchTab('bookmarks')"><svg width="18" height="18"><use href="#i-bookmark"/></svg> Bookmarks</button>
        </div>
      </div>
    </div>
  </section>

<section id="tab-games" class="content-section">
    <div class="section-title"><svg><use href="#i-games"/></svg> Games <span id="gameCountLabel" style="font-size:0.9rem; font-weight:400; color:var(--text-dim); margin-left:0.5rem;"></span></div>
    <div class="grid" id="gameGrid"></div>
  </section>

  <!-- CLOUD -->
  <section id="tab-cloud" class="content-section">
    <div class="section-title"><svg><use href="#i-cloud"/></svg> Cloud Gaming</div>
    <p style="text-align:center;color:var(--text-dim);max-width:640px;margin:0 auto 1.2rem;font-size:0.9rem;">
      Stream AAA games from powerful remote PCs — no download, play in browser or app. Many services need an account; free tiers noted below.
    </p>
    <div style="display:flex;flex-wrap:wrap;gap:0.5rem;justify-content:center;margin-bottom:1rem;" id="cloudFilters">
      <button class="theme-chip active" data-cloud-filter="all" onclick="filterCloud('all')" style="flex-direction:row;">All</button>
      <button class="theme-chip" data-cloud-filter="aaa" onclick="filterCloud('aaa')" style="flex-direction:row;">AAA / Premium</button>
      <button class="theme-chip" data-cloud-filter="free" onclick="filterCloud('free')" style="flex-direction:row;">Free / Free tier</button>
      <button class="theme-chip" data-cloud-filter="browser" onclick="filterCloud('browser')" style="flex-direction:row;">Browser</button>
      <button class="theme-chip" data-cloud-filter="console" onclick="filterCloud('console')" style="flex-direction:row;">Console</button>
      <button class="theme-chip" data-cloud-filter="pc" onclick="filterCloud('pc')" style="flex-direction:row;">Full PC</button>
    </div>
    <div style="text-align:center;margin-bottom:1rem;">
      <input type="search" id="cloudSearch" placeholder="Filter cloud services…" oninput="filterCloud()"
        style="padding:0.55rem 1rem;border-radius:20px;border:1px solid var(--border);background:var(--surface);color:var(--text);width:min(320px,90%);font-family:inherit;">
    </div>
    <div id="cloudFeatured" style="display:grid;grid-template-columns:repeat(auto-fit,minmax(260px,1fr));gap:1rem;margin-bottom:1.5rem;"></div>
    <div class="section-title" style="font-size:1.1rem;margin-bottom:0.8rem;">All platforms</div>
    <div class="grid" id="cloudGrid"></div>
    <div style="margin-top:1.5rem;padding:1rem 1.2rem;border-radius:14px;background:rgba(220,20,60,0.06);border:1px solid var(--border);max-width:720px;margin-left:auto;margin-right:auto;">
      <strong style="color:var(--accent-bright);">Tips</strong>
      <ul style="margin:0.5rem 0 0 1.1rem;color:var(--text-dim);font-size:0.85rem;line-height:1.5;">
        <li>Cloud sites often block iframes — use <em>Open tab</em> if the player stays blank.</li>
        <li>Wired Ethernet + low latency region = best experience.</li>
        <li>GeForce NOW / Xbox / Luna need free or paid accounts.</li>
        <li>Browser services (e.g. CrazyGames cloud titles) work with no install.</li>
      </ul>
    </div>
  </section>

  <!-- TV -->
  <section id="tab-tv" class="content-section">
    <div class="section-title"><svg><use href="#i-tv"/></svg> TV & Movies</div>
    <p style="text-align:center;color:var(--text-dim);max-width:640px;margin:0 auto 1rem;font-size:0.88rem;">
      Powered by TMDB + MyAnimeList · multi-source streaming with automatic failover
    </p>
    <div class="tv-toolbar">
      <div class="tv-toolbar-row">
        <select id="tvFilter" onchange="filterTV()" class="tv-select">
          <option value="all">All types</option>
          <option value="movie">Movies</option>
          <option value="tv">Shows</option>
          <option value="anime">Anime</option>
        </select>
        <select id="tvSort" onchange="filterTV()" class="tv-select">
          <option value="rating">Top rated</option>
          <option value="popularity">Most popular</option>
          <option value="year_desc">Newest</option>
          <option value="year_asc">Oldest</option>
          <option value="title">A–Z</option>
        </select>
        <select id="tvGenre" onchange="onTvGenreChange()" class="tv-select">
          <option value="">All genres</option>
        </select>
        <input type="search" id="tvSearchLocal" class="tv-search" placeholder="Filter titles…" oninput="filterTV()">
        <button class="tv-chip" onclick="loadTVMore()" id="tvLoadMoreBtn" title="Fetch more pages">Load more</button>
        <button class="tv-chip" onclick="showWatchlist()" id="tvWatchlistBtn">★ Watchlist</button>
      </div>
      <div class="tv-toolbar-row" id="tvCategoryChips">
        <button class="tv-chip active" data-cat="home" onclick="tvCategory('home')">Home</button>
        <button class="tv-chip" data-cat="trending" onclick="tvCategory('trending')">Trending</button>
        <button class="tv-chip" data-cat="now_playing" onclick="tvCategory('now_playing')">In theaters</button>
        <button class="tv-chip" data-cat="upcoming" onclick="tvCategory('upcoming')">Upcoming</button>
        <button class="tv-chip" data-cat="top_movies" onclick="tvCategory('top_movies')">Top movies</button>
        <button class="tv-chip" data-cat="top_tv" onclick="tvCategory('top_tv')">Top TV</button>
        <button class="tv-chip" data-cat="anime" onclick="tvCategory('anime')">Anime</button>
      </div>
    </div>
    <div id="tvHero" class="tv-hero" style="display:none;"></div>
    <div id="tvCountBar" style="text-align:center;font-size:0.8rem;color:var(--text-dim);margin-bottom:0.8rem;"></div>
    <div class="grid" id="tvGrid"></div>
  </section>

  <!-- TV Detail / Episode modal (additive) -->
  <div id="tvDetailModal" class="tv-detail-modal" style="display:none;" onclick="if(event.target===this)closeTvDetail()">
    <div class="tv-detail-panel">
      <button class="tv-detail-close" onclick="closeTvDetail()">×</button>
      <div class="tv-detail-body" id="tvDetailBody"></div>
    </div>
  </div>

  <!-- MUSIC -->
  <section id="tab-music" class="content-section">
    <div class="section-title"><svg><use href="#i-music"/></svg> Music</div>
    <div class="music-container">
      <div class="music-topbar">
        <div class="music-search">
          <svg width="16" height="16" viewBox="0 0 24 24" style="stroke:var(--text-dim);align-self:center;flex-shrink:0;"><use href="#i-search"/></svg>
          <input type="text" id="musicSearchInput" placeholder="Search tracks, artists, albums..." onkeydown="if(event.key==='Enter')musicSearch()">
          <button onclick="musicSearch()">Search</button>
        </div>
        <div class="music-tabs">
          <button class="music-tab active" data-mtab="trending">Trending</button>
          <button class="music-tab" data-mtab="genres">Genres</button>
          <button class="music-tab" data-mtab="favorites">Favorites</button>
          <button class="music-tab" data-mtab="recent">Recent</button>
          <button class="music-tab" data-mtab="playlists">Playlists</button>
        </div>
      </div>
      <div class="music-main">
        <div id="musicContent"><div class="loading"><div class="spinner"></div>Loading music...</div></div>
      </div>
    </div>
  </section>

  <!-- PROXY -->
  <section id="tab-proxy" class="content-section">
    <div class="proxy-wrapper">
      <div class="section-title" style="justify-content:center;"><svg><use href="#i-globe"/></svg> Proxy Browser</div>

      <div class="proxy-search-hero">
        <svg class="search-icon" viewBox="0 0 24 24"><use href="#i-search"/></svg>
        <input type="text" id="proxyUrl" placeholder="Search or enter address" onkeydown="if(event.key==='Enter')proxyGo()">
        <button onclick="proxyGo()">Go</button>
      </div>

      <div class="proxy-status" style="max-width: 900px; margin: 0 auto 1.5rem;">
        <span class="dot" id="proxyDot"></span>
        <span id="proxyStatusText">Initializing proxy engines...</span>
      </div>

      <div class="proxy-toolbar" style="max-width: 900px; margin: 0 auto 1.5rem;">
        <button onclick="proxyBack()" title="Back"><svg viewBox="0 0 24 24"><use href="#i-arrow-left"/></svg></button>
        <button onclick="proxyForward()" title="Forward"><svg viewBox="0 0 24 24"><use href="#i-arrow-right"/></svg></button>
        <button onclick="proxyReload()" title="Reload"><svg viewBox="0 0 24 24"><use href="#i-refresh"/></svg></button>
        <button onclick="proxyHome()" title="Home"><svg viewBox="0 0 24 24"><use href="#i-home"/></svg></button>
        <button onclick="addBookmarkFromProxy()" title="Bookmark"><svg viewBox="0 0 24 24"><use href="#i-bookmark"/></svg></button>
        <button onclick="toggleIncognito()" id="incognitoBtn" title="Incognito"><svg viewBox="0 0 24 24"><use href="#i-incognito"/></svg></button>
        <div class="engine-switch">
          <button id="enginePublic" class="active" onclick="setEngine('public')">Public</button>
          <button id="engineUV" onclick="setEngine('uv')">UV</button>
          <button id="engineSJ" onclick="setEngine('sj')">SJ</button>
          <button onclick="cyclePublicGateway()" title="Switch public gateway" style="padding:0.4rem 0.7rem;font-size:0.7rem;">⇄ GW</button>
        </div>
      </div>

      <!-- ENHANCEMENT: Proxy tabs -->
      <div class="proxy-tabs" id="proxyTabs" style="max-width:900px;margin-left:auto;margin-right:auto;">
        <button class="proxy-tab active" data-ptab="0"><span class="pt-title">Home</span></button>
        <button class="proxy-tab-add" onclick="newProxyTab()" title="New tab">+</button>
      </div>

      <div class="proxy-apps-grid" id="proxyAppsGrid">
        <!-- YouTube -->
        <div class="proxy-app youtube" onclick="proxyGoTo('https://www.youtube.com')">
          <div class="app-icon-wrap">
            <svg viewBox="0 0 24 24" fill="#ffffff"><path d="M23.498 6.186a3.016 3.016 0 0 0-2.122-2.136C19.505 3.545 12 3.545 12 3.545s-7.505 0-9.377.505A3.017 3.017 0 0 0 .502 6.186C0 8.07 0 12 0 12s0 3.93.502 5.814a3.016 3.016 0 0 0 2.122 2.136c1.871.505 9.376.505 9.376.505s7.505 0 9.377-.505a3.015 3.015 0 0 0 2.122-2.136C24 15.93 24 12 24 12s0-3.93-.502-5.814zM9.545 15.568V8.432L15.818 12l-6.273 3.568z"/></svg>
          </div>
          <div class="app-label">YouTube</div>
        </div>

        <!-- TikTok -->
        <div class="proxy-app tiktok" onclick="proxyGoTo('https://www.tiktok.com')">
          <div class="app-icon-wrap">
            <svg viewBox="0 0 24 24" fill="#ffffff"><path d="M19.59 6.69a4.83 4.83 0 0 1-3.77-4.25V2h-3.45v13.67a2.89 2.89 0 0 1-5.2 1.74 2.89 2.89 0 0 1 2.31-4.64 2.93 2.93 0 0 1 .88.13V9.4a6.84 6.84 0 0 0-1-.05A6.33 6.33 0 0 0 5 20.1a6.34 6.34 0 0 0 10.86-4.43v-7a8.16 8.16 0 0 0 4.77 1.52v-3.4a4.85 4.85 0 0 1-1-.1z"/></svg>
          </div>
          <div class="app-label">TikTok</div>
        </div>

        <!-- Roblox -->
        <div class="proxy-app roblox" onclick="proxyGoTo('https://www.roblox.com')">
          <div class="app-icon-wrap">
            <svg viewBox="0 0 24 24" fill="#ffffff"><path d="M5.164 2.303a1.53 1.53 0 0 0-1.28 1.75l3.176 17.62a1.53 1.53 0 0 0 1.75 1.28l9.82-1.766a1.53 1.53 0 0 0 1.28-1.75l-3.176-17.62a1.53 1.53 0 0 0-1.75-1.28L5.164 2.303zm6.908 11.42a1.9 1.9 0 0 1-2.14-1.61 1.9 1.9 0 0 1 1.61-2.14 1.9 1.9 0 0 1 2.14 1.61 1.9 1.9 0 0 1-1.61 2.14z"/></svg>
          </div>
          <div class="app-label">Roblox</div>
        </div>

        <!-- Geometry Dash -->
        <div class="proxy-app geometrydash" onclick="proxyGoTo('https://geometrydash.com')">
          <div class="app-icon-wrap">
            <svg viewBox="0 0 24 24" fill="#000"><path d="M7 12L10 7L13 12L16 7L17 9L14 14L11 9L8 14Z" fill="#fff"/><rect x="5" y="17" width="4" height="3" fill="#fff"/><rect x="10" y="17" width="4" height="3" fill="#fff"/><rect x="15" y="17" width="4" height="3" fill="#fff"/></svg>
          </div>
          <div class="app-label">Geometry Dash</div>
        </div>

        <!-- Kick -->
        <div class="proxy-app kick" onclick="proxyGoTo('https://kick.com')">
          <div class="app-icon-wrap">
            <svg viewBox="0 0 24 24" fill="#000"><path d="M1.5 0v24h6.75v-4.5h3v-3h3v-3h3V9h-3V6h-3V3h-3V0H1.5zm5.25 3h3v3h3v3h3v3h-3v3h-3v3h-3V3z"/></svg>
          </div>
          <div class="app-label">Kick</div>
        </div>

        <!-- Twitch -->
        <div class="proxy-app twitch" onclick="proxyGoTo('https://www.twitch.tv')">
          <div class="app-icon-wrap">
            <svg viewBox="0 0 24 24" fill="#ffffff"><path d="M4.265 0L.5 4.5v15h5.5v4l4-4h6l7.5-7V0H4.265zm17.235 11.5l-3.5 3.5h-5.5l-3 3v-3H5.5V2h16v9.5zM15 5.5h2v5h-2zM10.5 5.5h2v5h-2z"/></svg>
          </div>
          <div class="app-label">Twitch</div>
        </div>

        <!-- Snapchat -->
        <div class="proxy-app snapchat" onclick="proxyGoTo('https://www.snapchat.com')">
          <div class="app-icon-wrap">
            <svg viewBox="0 0 24 24" fill="#000"><path d="M12.206 23.5c-2.299 0-4.187-1.248-5.716-2.244-.635-.414-1.32-.861-1.754-.861-.198 0-.421.046-.635.092-.276.06-.56.12-.785.12-.396 0-.685-.209-.717-.521-.03-.29.145-.542.375-.844.05-.07.11-.144.175-.227.53-.674 1.04-1.32 1.19-2.4.06-.42.02-.79-.02-1.11-.02-.15-.03-.3-.04-.44-.02-.4-.11-.65-.28-.79-.05-.04-.13-.09-.24-.13-.42-.15-.95-.32-1.29-.54-.42-.29-.57-.72-.41-1.13.15-.4.62-.68 1.25-.75.63-.08 1.19-.13 1.55-.62.29-.4.42-1.02.63-1.79.28-1.05.62-2.32 1.87-3.02.62-.35 1.4-.53 2.19-.53s1.57.18 2.19.53c1.25.7 1.59 1.97 1.87 3.02.21.77.34 1.39.63 1.79.36.49.92.54 1.55.62.63.07 1.1.35 1.25.75.16.41.01.84-.41 1.13-.34.22-.87.39-1.29.54-.11.04-.19.09-.24.13-.17.14-.26.39-.28.79-.01.14-.02.29-.04.44-.04.32-.08.69-.02 1.11.15 1.08.66 1.726 1.19 2.4.065.083.125.157.175.227.23.302.405.554.375.844-.032.312-.321.521-.717.521-.225 0-.509-.06-.785-.12-.214-.046-.437-.092-.635-.092-.434 0-1.119.447-1.754.861-1.529.996-3.417 2.244-5.716 2.244z"/></svg>
          </div>
          <div class="app-label">Snapchat</div>
        </div>

        <!-- Instagram -->
        <div class="proxy-app instagram" onclick="proxyGoTo('https://www.instagram.com')">
          <div class="app-icon-wrap">
            <svg viewBox="0 0 24 24" fill="#ffffff"><path d="M12 2.163c3.204 0 3.584.012 4.85.07 3.252.148 4.771 1.691 4.919 4.919.058 1.265.069 1.645.069 4.849 0 3.205-.012 3.584-.069 4.849-.149 3.225-1.664 4.771-4.919 4.919-1.266.058-1.644.07-4.85.07-3.204 0-3.584-.012-4.849-.07-3.26-.149-4.771-1.699-4.919-4.92-.058-1.265-.07-1.644-.07-4.849 0-3.204.013-3.583.07-4.849.149-3.227 1.664-4.771 4.919-4.919 1.266-.057 1.645-.069 4.849-.069zM12 0C8.741 0 8.333.014 7.053.072 2.695.272.273 2.69.073 7.052.014 8.333 0 8.741 0 12c0 3.259.014 3.668.072 4.948.2 4.358 2.618 6.78 6.98 6.98C8.333 23.986 8.741 24 12 24c3.259 0 3.668-.014 4.948-.072 4.354-.2 6.782-2.618 6.979-6.98.059-1.28.073-1.689.073-4.948 0-3.259-.014-3.667-.072-4.947-.196-4.354-2.617-6.78-6.979-6.98C15.668.014 15.259 0 12 0zm0 5.838a6.162 6.162 0 1 0 0 12.324 6.162 6.162 0 0 0 0-12.324zM12 16a4 4 0 1 1 0-8 4 4 0 0 1 0 8zm6.406-11.845a1.44 1.44 0 1 0 0 2.881 1.44 1.44 0 0 0 0-2.881z"/></svg>
          </div>
          <div class="app-label">Instagram</div>
        </div>

        <!-- Discord -->
        <div class="proxy-app discord" onclick="proxyGoTo('https://discord.com')">
          <div class="app-icon-wrap">
            <svg viewBox="0 0 24 24" fill="#ffffff"><path d="M20.317 4.37a19.79 19.79 0 0 0-4.885-1.515.074.074 0 0 0-.079.037c-.21.375-.444.864-.608 1.25a18.27 18.27 0 0 0-5.487 0 12.64 12.64 0 0 0-.617-1.25.077.077 0 0 0-.079-.037A19.736 19.736 0 0 0 3.677 4.37a.07.07 0 0 0-.032.027C.533 9.046-.32 13.58.099 18.057a.082.082 0 0 0 .031.057 19.9 19.9 0 0 0 5.993 3.03.078.078 0 0 0 .084-.028 14.09 14.09 0 0 0 1.226-1.994.076.076 0 0 0-.041-.106 13.107 13.107 0 0 1-1.872-.892.077.077 0 0 1-.008-.128 10.2 10.2 0 0 0 .372-.292.074.074 0 0 1 .077-.01c3.928 1.793 8.18 1.793 12.062 0a.074.074 0 0 1 .078.01c.12.098.246.198.373.292a.077.077 0 0 1-.006.127 12.299 12.299 0 0 1-1.873.892.077.077 0 0 0-.041.107c.36.698.772 1.362 1.225 1.993a.076.076 0 0 0 .084.028 19.839 19.839 0 0 0 6.002-3.03.077.077 0 0 0 .032-.054c.5-5.177-.838-9.674-3.549-13.66a.061.061 0 0 0-.031-.03zM8.02 15.33c-1.183 0-2.157-1.085-2.157-2.419 0-1.333.956-2.419 2.157-2.419 1.21 0 2.176 1.096 2.157 2.42 0 1.333-.956 2.418-2.157 2.418zm7.975 0c-1.183 0-2.157-1.085-2.157-2.419 0-1.333.955-2.419 2.157-2.419 1.21 0 2.176 1.096 2.157 2.42 0 1.333-.946 2.418-2.157 2.418z"/></svg>
          </div>
          <div class="app-label">Discord</div>
        </div>

        <!-- Netflix -->
        <div class="proxy-app netflix" onclick="proxyGoTo('https://www.netflix.com')">
          <div class="app-icon-wrap">
            <svg viewBox="0 0 24 24" fill="#ffffff"><path d="M5.398 0v.006c3.028 8.556 5.37 15.175 8.348 23.596 2.344.058 4.85.398 4.854.398-2.8-7.924-5.923-16.747-8.487-24h-4.715zm8.489 0v9.63L18.6 22.951c-.043-7.86-.004-15.913.002-22.95h-4.715zm6.715 22.79V0h-4.72v7.301L20.602 22.79z"/></svg>
          </div>
          <div class="app-label">Netflix</div>
        </div>

        <!-- Spotify -->
        <div class="proxy-app spotify" onclick="proxyGoTo('https://open.spotify.com')">
          <div class="app-icon-wrap">
            <svg viewBox="0 0 24 24" fill="#000"><path d="M12 0C5.4 0 0 5.4 0 12s5.4 12 12 12 12-5.4 12-12S18.66 0 12 0zm5.521 17.34c-.24.359-.66.48-1.021.24-2.82-1.74-6.36-2.101-10.561-1.141-.418.122-.779-.179-.899-.539-.12-.421.18-.78.54-.9 4.56-1.021 8.52-.6 11.64 1.32.42.18.479.659.301 1.02zm1.44-3.3c-.301.42-.841.6-1.262.3-3.239-1.98-8.159-2.58-11.939-1.38-.479.12-1.02-.12-1.14-.6-.12-.48.12-1.021.6-1.141C9.6 9.9 15 10.561 18.72 12.84c.361.181.54.78.241 1.2zm.12-3.36C15.24 8.4 8.82 8.16 5.16 9.301c-.6.179-1.2-.181-1.38-.721-.18-.601.18-1.2.72-1.381 4.26-1.26 11.28-1.02 15.721 1.621.539.3.719 1.02.419 1.56-.299.421-1.02.599-1.559.3z"/></svg>
          </div>
          <div class="app-label">Spotify</div>
        </div>

        <!-- Reddit -->
        <div class="proxy-app reddit" onclick="proxyGoTo('https://www.reddit.com')">
          <div class="app-icon-wrap">
            <svg viewBox="0 0 24 24" fill="#ffffff"><path d="M12 0A12 12 0 0 0 0 12a12 12 0 0 0 12 12 12 12 0 0 0 12-12A12 12 0 0 0 12 0zm5.01 4.744c.688 0 1.25.561 1.25 1.249a1.25 1.25 0 0 1-2.498.056l-2.597-.547-.8 3.747c1.824.07 3.48.632 4.674 1.488.308-.309.73-.491 1.207-.491.968 0 1.754.786 1.754 1.754 0 .716-.435 1.333-1.01 1.614a3.111 3.111 0 0 1 .042.52c0 2.694-3.13 4.87-7.004 4.87-3.874 0-7.004-2.176-7.004-4.87 0-.183.015-.366.043-.534A1.748 1.748 0 0 1 4.028 12c0-.968.786-1.754 1.754-1.754.463 0 .898.196 1.207.49 1.207-.883 2.878-1.43 4.744-1.487l.885-4.182a.342.342 0 0 1 .14-.197.35.35 0 0 1 .238-.042l2.906.617a1.214 1.214 0 0 1 1.108-.701zM9.25 12C8.561 12 8 12.562 8 13.25c0 .687.561 1.248 1.25 1.248.687 0 1.248-.561 1.248-1.249 0-.688-.561-1.249-1.249-1.249zm5.5 0c-.687 0-1.248.561-1.248 1.25 0 .687.561 1.248 1.249 1.248.688 0 1.249-.561 1.249-1.249 0-.688-.561-1.249-1.25-1.249zm-5.466 3.99a.327.327 0 0 0-.231.094.33.33 0 0 0 0 .463c.842.842 2.484.913 2.961.913.477 0 2.105-.056 2.961-.913a.361.361 0 0 0 .029-.463.33.33 0 0 0-.464 0c-.547.533-1.684.73-2.512.73-.828 0-1.979-.196-2.512-.73a.326.326 0 0 0-.232-.095z"/></svg>
          </div>
          <div class="app-label">Reddit</div>
        </div>

        <!-- Wikipedia -->
        <div class="proxy-app wikipedia" onclick="proxyGoTo('https://en.wikipedia.org')">
          <div class="app-icon-wrap">
            <svg viewBox="0 0 24 24" fill="#000"><path d="M12.09 13.119c-.936 1.932-2.217 4.548-2.853 5.728-.616 1.074-1.127.931-1.532.029-1.406-3.321-4.293-9.144-5.651-12.409-.251-.601-.441-.987-.619-1.139-.181-.15-.554-.24-1.122-.271v-.919h5.929v.919c-.859.052-1.313.164-1.372.336-.063.171.041.659.311 1.463.546 1.628 3.474 7.501 3.474 7.501l1.446-3.226s-1.058-2.281-1.888-4.011c-.609-1.266-.837-1.421-1.837-1.5v-.919h5.98v.919c-1.066.058-1.5.582-1.5.582l2.118 4.53 1.5-3.265c-.087-.414-.436-1.63-.879-2.616-.316-.702-.495-.751-1.261-.8v-.919h4.724v.919c-1.114.108-1.731.501-2.106 1.216-.327.622-1.464 3.335-1.464 3.335s1.734 3.868 2.352 5.295c.312.719.581.36 1.184-1.022.605-1.383 1.244-3.052 1.244-3.052s-.94-2.322-1.762-3.972c-.406-.819-.551-.905-1.301-.966v-.919h4.5v.919c-.78.045-1.153.273-1.549.906-.397.634-2.31 4.628-2.31 4.628s3.42 7.851 3.573 8.207c.151.354.482.281 1.043-1.05l3.023-7.212c.281-.667.353-1.216.054-1.281-.301-.064-.811-.111-1.415-.119v-.919h4.5v.919c-.945.088-1.612.42-2.122 1.5-2.05 4.511-4.348 9.5-4.348 9.5-.681 1.51-1.365 1.4-2.05-.15-.683-1.55-3.034-7.198-3.034-7.198l-1.521 3.34z"/></svg>
          </div>
          <div class="app-label">Wikipedia</div>
        </div>

        <!-- Hacker News -->
        <div class="proxy-app hackernews" onclick="proxyGoTo('https://news.ycombinator.com')">
          <div class="app-icon-wrap">
            <svg viewBox="0 0 24 24" fill="#fff"><rect x="0" y="0" width="24" height="24"/><path d="M12.378 17.205l-2.504-5.881-2.633 5.881H4.999v-.316l3.094-6.778L5.088 3.403v-.316h2.495c.198 0 .36.127.487.379l1.845 4.448 1.845-4.448c.084-.232.234-.379.45-.379h2.247v.316l-2.808 6.674 3.112 6.812v.316h-2.158c-.234 0-.415-.135-.487-.379l-2.121-5.03-2.086 4.95c-.108.232-.27.348-.487.348h-2.087z" fill="#000"/></svg>
          </div>
          <div class="app-label">Hacker News</div>
        </div>

        <!-- X -->
        <div class="proxy-app twitter" onclick="proxyGoTo('https://x.com')">
          <div class="app-icon-wrap">
            <svg viewBox="0 0 24 24" fill="#fff"><path d="M18.244 2.25h3.308l-7.227 8.26 8.502 11.24H16.17l-5.214-6.817L4.99 21.75H1.68l7.73-8.835L1.254 2.25H8.08l4.713 6.231zm-1.161 17.52h1.833L7.084 4.126H5.117z"/></svg>
          </div>
          <div class="app-label">X</div>
        </div>

        <!-- GitHub -->
        <div class="proxy-app github" onclick="proxyGoTo('https://github.com')">
          <div class="app-icon-wrap">
            <svg viewBox="0 0 24 24" fill="#fff"><path d="M12 .297c-6.63 0-12 5.373-12 12 0 5.303 3.438 9.8 8.205 11.385.6.113.82-.258.82-.577 0-.285-.01-1.04-.015-2.04-3.338.724-4.042-1.61-4.042-1.61C4.422 18.07 3.633 17.7 3.633 17.7c-1.087-.744.084-.729.084-.729 1.205.084 1.838 1.236 1.838 1.236 1.07 1.835 2.809 1.305 3.495.998.108-.776.417-1.305.76-1.605-2.665-.3-5.466-1.332-5.466-5.93 0-1.31.465-2.38 1.235-3.22-.135-.303-.54-1.523.105-3.176 0 0 1.005-.322 3.3 1.23.96-.267 1.98-.399 3-.405 1.02.006 2.04.138 3 .405 2.28-1.552 3.285-1.23 3.285-1.23.645 1.653.24 2.873.12 3.176.765.84 1.23 1.91 1.23 3.22 0 4.61-2.805 5.625-5.475 5.92.42.36.81 1.096.81 2.22 0 1.606-.015 2.896-.015 3.286 0 .315.21.69.825.57C20.565 22.092 24 17.592 24 12.297c0-6.627-5.373-12-12-12"/></svg>
          </div>
          <div class="app-label">GitHub</div>
        </div>
      </div>

      <div class="proxy-frame-container" id="proxyFrameContainer">
        <iframe id="proxyFrame" src="" sandbox="allow-scripts allow-same-origin allow-forms allow-popups allow-popups-to-escape-sandbox allow-presentation allow-downloads allow-modals" allow="fullscreen; encrypted-media; picture-in-picture" referrerpolicy="no-referrer"></iframe>
      </div>
    </div>
  </section>

  <!-- BOOKMARKS -->
  <section id="tab-bookmarks" class="content-section">
    <div class="section-title"><svg><use href="#i-bookmark"/></svg> Bookmarks</div>
    <div class="setting-card" style="max-width:900px; margin:0 auto;">
      <div style="display:flex; gap:0.5rem; margin-bottom:0.75rem; flex-wrap:wrap;">
        <input type="text" id="bmTitle" placeholder="Title" style="flex:1; min-width:120px;">
        <input type="text" id="bmUrl" placeholder="URL (https://...)" style="flex:2; min-width:200px;">
        <input type="text" id="bmTag" placeholder="Tag (optional)" style="flex:1; min-width:100px;">
        <button onclick="addBookmark()">Add</button>
      </div>
      <input type="search" id="bmSearch" placeholder="Filter bookmarks…" oninput="renderBookmarks()" style="width:100%;margin-bottom:1rem;padding:0.5rem 0.9rem;border-radius:12px;border:1px solid var(--border);background:var(--bg);color:var(--text);font-family:inherit;">
      <div id="bookmarkList"></div>
    </div>
  </section>

  <!-- CHAT -->
  <section id="tab-chat" class="content-section">
    <div class="section-title"><svg><use href="#i-chat"/></svg> Chat Rooms</div>
    <p style="text-align:center;color:var(--text-dim);max-width:640px;margin:0 auto 1rem;font-size:0.88rem;">
      Local multi-tab room (no server) plus public IRC embeds. Be respectful — public rooms are shared with the internet.
    </p>
    <div class="tv-toolbar-row" style="margin-bottom:1rem;">
      <button class="tv-chip active" data-chat-view="local" onclick="showChatView('local')">Local room</button>
      <button class="tv-chip" data-chat-view="rooms" onclick="showChatView('rooms')">Public rooms</button>
      <button class="tv-chip" data-chat-view="custom" onclick="showChatView('custom')">Custom embed</button>
    </div>

    <div id="chatViewLocal" class="chat-view">
      <div class="chat-local-wrap">
        <div class="chat-local-sidebar">
          <label style="font-size:0.78rem;color:var(--text-dim);">Display name</label>
          <input type="text" id="chatNick" maxlength="24" placeholder="Anonymous" style="width:100%;margin:0.3rem 0 0.8rem;padding:0.5rem 0.75rem;border-radius:10px;border:1px solid var(--border);background:var(--bg);color:var(--text);font-family:inherit;">
          <label style="font-size:0.78rem;color:var(--text-dim);">Room channel</label>
          <select id="chatChannel" onchange="switchChatChannel()" style="width:100%;margin:0.3rem 0 0.8rem;padding:0.5rem;border-radius:10px;border:1px solid var(--border);background:var(--bg);color:var(--text);font-family:inherit;">
            <option value="xorwel-general">#general</option>
            <option value="xorwel-games">#games</option>
            <option value="xorwel-movies">#movies</option>
            <option value="xorwel-music">#music</option>
            <option value="xorwel-random">#random</option>
          </select>
          <p style="font-size:0.72rem;color:var(--text-dim);line-height:1.4;">Works across tabs on this device/origin via BroadcastChannel. Open the site in two tabs to test.</p>
          <div id="chatPresence" style="margin-top:0.8rem;font-size:0.75rem;color:var(--accent-bright);"></div>
        </div>
        <div class="chat-local-main">
          <div id="chatMessages" class="chat-messages"></div>
          <div class="chat-compose">
            <input type="text" id="chatInput" placeholder="Type a message… (Enter to send)" maxlength="500" onkeydown="if(event.key==='Enter')sendChatMessage()">
            <button onclick="sendChatMessage()" class="tv-chip active">Send</button>
          </div>
        </div>
      </div>
    </div>

    <div id="chatViewRooms" class="chat-view" style="display:none;">
      <div class="grid" id="chatRoomsGrid"></div>
      <div id="chatEmbedWrap" style="display:none;margin-top:1rem;">
        <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:0.5rem;">
          <strong id="chatEmbedTitle">Room</strong>
          <button class="tv-chip" onclick="closeChatEmbed()">Close</button>
        </div>
        <iframe id="chatEmbedFrame" src="" style="width:100%;height:min(70vh,560px);border:1px solid var(--border);border-radius:14px;background:#0a0a0a;" allow="clipboard-write"></iframe>
      </div>
    </div>

    <div id="chatViewCustom" class="chat-view" style="display:none;">
      <div style="max-width:560px;margin:0 auto;text-align:center;">
        <p style="color:var(--text-dim);font-size:0.88rem;margin-bottom:0.8rem;">Paste any embeddable chat URL (Kiwi IRC, custom widget, etc.)</p>
        <input type="url" id="chatCustomUrl" placeholder="https://kiwiirc.com/nextclient/..." style="width:100%;padding:0.65rem 1rem;border-radius:12px;border:1px solid var(--border);background:var(--surface);color:var(--text);font-family:inherit;margin-bottom:0.6rem;">
        <button class="tv-chip active" onclick="openCustomChat()">Open in panel</button>
        <button class="tv-chip" onclick="openCustomChatTab()">Open tab</button>
      </div>
    </div>
  </section>

  <section id="tab-ai" class="content-section">
    <div class="section-title"><svg><use href="#i-bot"/></svg> Xorwel AI</div>
    <div class="setting-card" style="max-width:900px; margin:0 auto;">
      <h3>Chat <span id="aiModelStatus" style="font-size:0.7rem; font-weight:400; color:var(--text-dim); margin-left:0.5rem;">(loading...)</span></h3>
      <div style="margin-bottom:1rem; display:flex; flex-wrap:wrap; gap:0.4rem;">
        <button class="theme-chip" onclick="quickAI('Write a short essay on climate change')" style="flex-direction:row;">Essay</button>
        <button class="theme-chip" onclick="quickAI('Solve 2x + 5 = 15 step by step')" style="flex-direction:row;">Math</button>
        <button class="theme-chip" onclick="quickAI('Explain photosynthesis in simple terms')" style="flex-direction:row;">Science</button>
        <button class="theme-chip" onclick="quickAI('Write a resume summary for a software engineer')" style="flex-direction:row;">Resume</button>
        <button class="theme-chip" onclick="quickAI('Tell me a joke')" style="flex-direction:row;">Joke</button>
        <button class="theme-chip" onclick="quickAI('Write a JavaScript debounce function')" style="flex-direction:row;">Code</button>
        <button class="theme-chip" onclick="quickAI('Summarize the plot of Inception')" style="flex-direction:row;">Movie</button>
        <button class="theme-chip" onclick="clearAI()" style="flex-direction:row;">Clear</button>
      </div>
      <p style="font-size:0.75rem;color:var(--text-dim);margin-bottom:0.8rem;">Local model runs in your browser (privacy-first). Falls back to smart offline replies if the model is still loading.</p>
      <div id="aiChat"></div>
      <div style="display:flex; gap:0.5rem;">
        <input type="text" id="aiInput" placeholder="Ask anything..." style="flex:1;" onkeydown="if(event.key==='Enter')sendAI()">
        <button onclick="sendAI()" id="aiSendBtn">Send</button>
      </div>
    </div>
  </section>

  <!-- SETTINGS -->
  <section id="tab-settings" class="content-section">
    <div class="section-title"><svg><use href="#i-settings"/></svg> Settings</div>
    <div class="settings-grid">
      <div class="setting-card">
        <h3><svg viewBox="0 0 24 24"><use href="#i-palette"/></svg> Theme</h3>
        <div class="theme-category">Standard</div>
        <div class="theme-grid" id="themeGridStandard"></div>
        <div class="theme-category">Anime</div>
        <div class="theme-grid" id="themeGridAnime"></div>
        <div class="theme-category">Cartoon & Sports</div>
        <div class="theme-grid" id="themeGridCartoon"></div>
      </div>
      <div class="setting-card">
        <h3><svg viewBox="0 0 24 24"><use href="#i-tv"/></svg> TV Preferences</h3>
        <label style="font-size:0.85rem; display:block; margin-bottom:0.5rem; color:var(--text-dim); font-weight:700; text-transform:uppercase;">Preferred source:</label>
        <select id="preferredSourceSelect" onchange="setPreferredSource()"><option value="">Auto (random)</option></select>
        <label style="font-size:0.85rem; display:block; margin:0.8rem 0 0.5rem; color:var(--text-dim); font-weight:700; text-transform:uppercase;">Test timeout (seconds):</label>
        <input type="number" id="sourceTimeoutInput" min="1" max="10" value="3" onchange="setSourceTimeout()">
      </div>
      <div class="setting-card">
        <h3><svg viewBox="0 0 24 24"><use href="#i-globe"/></svg> Proxy Engine</h3>
        <label style="font-size:0.85rem; display:block; margin-bottom:0.5rem; color:var(--text-dim); font-weight:700; text-transform:uppercase;">Primary:</label>
        <select id="primaryEngineSelect" onchange="setPrimaryEngine()">
          <option value="public">Public (works offline / static)</option>
          <option value="uv">Ultraviolet (needs server)</option>
          <option value="sj">Scramjet (needs server)</option>
        </select>
      </div>
      <div class="setting-card">
        <h3><svg viewBox="0 0 24 24"><use href="#i-trash"/></svg> Data</h3>
        <button onclick="clearSourceCache()">Clear Source Cache</button>
        <button onclick="clearAllData()" style="background:var(--danger); margin-top:0.5rem;">Clear All Data</button>
      </div>
    </div>
  
      <div style="margin-top:1.5rem;padding:1rem;border:1px solid var(--border);border-radius:14px;background:var(--surface);">
        <h3 style="margin:0 0 0.6rem;font-size:1rem;">Data & advanced</h3>
        <p style="font-size:0.8rem;color:var(--text-dim);margin:0 0 0.8rem;">Backup bookmarks, watchlist, notes, continue watching, and preferences.</p>
        <div style="display:flex;flex-wrap:wrap;gap:0.5rem;">
          <button class="tv-chip active" onclick="exportAllData()">Export JSON</button>
          <button class="tv-chip" onclick="document.getElementById('importDataInput')?.click()">Import JSON</button>
          <button class="tv-chip" onclick="openCmdPalette()">Command palette</button>
          <button class="tv-chip" onclick="openShortcutsModal()">Shortcuts</button>
          <button class="tv-chip" onclick="togglePageFullscreen()">Fullscreen</button>
        </div>
        <p style="font-size:0.72rem;color:var(--text-dim);margin-top:0.7rem;">Tip: press <kbd style="border:1px solid var(--border);padding:0.1rem 0.35rem;border-radius:4px;">Ctrl</kbd>+<kbd style="border:1px solid var(--border);padding:0.1rem 0.35rem;border-radius:4px;">K</kbd> for commands, or <kbd style="border:1px solid var(--border);padding:0.1rem 0.35rem;border-radius:4px;">Ctrl</kbd>+<kbd style="border:1px solid var(--border);padding:0.1rem 0.35rem;border-radius:4px;">Space</kbd> to search.</p>
      </div>

  </section>

  <!-- ACCOUNT -->
  <section id="tab-account" class="content-section">
    <div class="section-title"><svg><use href="#i-user"/></svg> Account</div>
    <div id="accountSignedIn" style="display:none;">
      <div class="account-header">
        <div class="account-avatar" id="accountAvatar">?</div>
        <div class="account-info"><h2 id="accountName">Username</h2><p id="accountCreated">Member since ...</p></div>
      </div>
      <div class="stats-row">
        <div class="stat-box"><div class="num" id="statFavs">0</div><div class="lbl">Favorites</div></div>
        <div class="stat-box"><div class="num" id="statBookmarks">0</div><div class="lbl">Bookmarks</div></div>
        <div class="stat-box"><div class="num" id="statRecent">0</div><div class="lbl">Recent</div></div>
        <div class="stat-box"><div class="num" id="statPlaytime">0</div><div class="lbl">Sessions</div></div>
      </div>
      <div class="setting-card">
        <h3><svg viewBox="0 0 24 24"><use href="#i-logout"/></svg> Session</h3>
        <p style="font-size:0.85rem; color:var(--text-dim); margin-bottom:1rem;">All your data is stored locally and linked to your account.</p>
        <button onclick="logout()" style="background:var(--danger);">Sign Out</button>
      </div>
    </div>
    <div id="accountSignedOut" class="setting-card" style="max-width:480px; margin:0 auto;">
      <h3><svg viewBox="0 0 24 24"><use href="#i-user"/></svg> Sign In / Sign Up</h3>
      <p style="font-size:0.85rem; color:var(--text-dim); margin-bottom:1rem;">Create a local account to save favorites, bookmarks, history, and settings.</p>
      <div class="form-group"><label>Username</label><input type="text" id="authUsername" autocomplete="username"></div>
      <div class="form-group"><label>Password</label><input type="password" id="authPassword" autocomplete="current-password"></div>
      <div class="form-error" id="authError"></div>
      <button class="btn-primary" onclick="doLogin()" id="btnLogin">Sign In</button>
      <button class="btn-primary" onclick="doSignup()" id="btnSignup" style="background:linear-gradient(135deg,#ff2d55,#dc143c); display:none;">Create Account</button>
      <div class="form-switch"><button class="btn-link" onclick="toggleAuthMode()" id="authSwitchBtn">Need an account? Sign up</button></div>
    </div>
  </section>
</main>

<!-- PLAYER -->
<div id="playerModal" class="modal">
  <button class="close-btn" onclick="closePlayer()"><svg viewBox="0 0 24 24"><use href="#i-close"/></svg></button>
  <iframe id="playerFrame" src="" allowfullscreen allow="fullscreen; gamepad; autoplay; clipboard-write" sandbox="allow-scripts allow-same-origin allow-forms allow-popups allow-popups-to-escape-sandbox allow-presentation allow-downloads allow-modals allow-pointer-lock" referrerpolicy="no-referrer"></iframe>
  <div id="playerControls">
    <select id="qualitySelect" onchange="changeQuality()">
      <option value="144">144p</option>
      <option value="240">240p</option>
      <option value="360" selected>360p</option>
      <option value="480">480p</option>
      <option value="720">720p</option>
      <option value="1080">1080p</option>
      <option value="2160">4K</option>
    </select>
    <select id="speedSelect" onchange="changeSpeed()">
      <option value="0.5">0.5x</option>
      <option value="0.75">0.75x</option>
      <option value="1" selected>1x</option>
      <option value="1.25">1.25x</option>
      <option value="1.5">1.5x</option>
      <option value="2">2x</option>
    </select>
    <button onclick="toggleCaptions()" id="captionBtn"><svg viewBox="0 0 24 24"><use href="#i-captions"/></svg> CC</button>
    <button onclick="skipCurrentSource()"><svg viewBox="0 0 24 24"><use href="#i-skip"/></svg> Skip</button>
    <button onclick="tryAllSourcesAgain()"><svg viewBox="0 0 24 24"><use href="#i-refresh"/></svg> Retry</button>
    <button onclick="togglePlayerPiP()" id="pipBtn" title="Picture in Picture"><svg viewBox="0 0 24 24"><use href="#i-pip"/></svg> PiP</button>
    <button onclick="startWatchParty()" id="watchPartyBtn" title="Watch party">👥 Party</button>
    <span id="currentSourceLabel" style="font-size:0.75rem; color:var(--text-dim); align-self:center; font-weight:600;"></span>
  </div>
</div>

<!-- SOURCE HUD -->
<div id="sourceTestHUD">
  <div class="hud-title" id="hudTitle">Finding working source...</div>
  <div class="hud-sub" id="hudSub">Testing each provider until one plays your title.</div>
  <div class="hud-progress"><div class="hud-progress-fill" id="hudProgressFill"></div></div>
  <div class="hud-source-list" id="hudSourceList"></div>
  <div class="hud-actions">
    <button class="skip" onclick="skipCurrentSource()">Skip current</button>
    <button class="cancel" onclick="cancelSourceTest()">Cancel</button>
  </div>
</div>

<!-- MUSIC PLAYER -->
<div class="music-player" id="musicPlayer">
  <div class="mp-left">
    <div class="mp-art" id="mpArt"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M9 18V5l12-2v13"/><circle cx="6" cy="18" r="3"/><circle cx="18" cy="16" r="3"/></svg></div>
    <div class="mp-meta"><div class="mp-title" id="mpTitle">Nothing playing</div><div class="mp-artist" id="mpArtist">Pick a track to begin</div></div>
  </div>
  <div class="mp-center">
    <div class="mp-controls">
      <button class="mp-btn" id="mpShuffle" onclick="musicShuffle()"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><polyline points="16 3 21 3 21 8"/><line x1="4" y1="20" x2="21" y2="3"/><polyline points="21 16 21 21 16 21"/><line x1="15" y1="15" x2="21" y2="21"/><line x1="4" y1="4" x2="9" y2="9"/></svg></button>
      <button class="mp-btn" onclick="musicPrev()"><svg viewBox="0 0 24 24" fill="currentColor"><path d="M6 6h2v12H6zM9.5 12l8.5 6V6z"/></svg></button>
      <button class="mp-btn main" id="mpPlay" onclick="musicToggle()"><svg viewBox="0 0 24 24" fill="currentColor" id="mpPlayIcon"><path d="M8 5v14l11-7z"/></svg></button>
      <button class="mp-btn" onclick="musicNext()"><svg viewBox="0 0 24 24" fill="currentColor"><path d="M16 6h2v12h-2zM6 6v12l8.5-6z"/></svg></button>
      <button class="mp-btn" id="mpRepeat" onclick="musicRepeat()"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><polyline points="17 1 21 5 17 9"/><path d="M3 11V9a4 4 0 0 1 4-4h14"/><polyline points="7 23 3 19 7 15"/><path d="M21 13v2a4 4 0 0 1-4 4H3"/></svg></button>
    </div>
    <div class="mp-progress-row">
      <span class="mp-time" id="mpCur">0:00</span>
      <div class="mp-progress" id="mpProgress" onclick="musicSeek(event)"><div class="mp-progress-fill" id="mpProgressFill"></div></div>
      <span class="mp-time" id="mpDur">0:00</span>
    </div>
  </div>
  <div class="mp-right">
    <button class="mp-btn" id="mpFav" onclick="musicToggleFavCurrent()"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><use href="#i-heart"/></svg></button>
    <button class="mp-btn" onclick="musicShowQueue()"><svg viewBox="0 0 24 24"><use href="#i-list"/></svg></button>
    <div class="mp-volume">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><polygon points="11 5 6 9 2 9 2 15 6 15 11 19 11 5"/><path d="M15.54 8.46a5 5 0 0 1 0 7.07"/></svg>
      <input type="range" id="mpVolume" min="0" max="100" value="80" oninput="musicVolume(this.value)">
    </div>
  </div>
</div>

<canvas id="musicViz" class="music-viz" width="200" height="40"></canvas>
<audio id="musicAudio" crossorigin="anonymous"></audio>
<iframe id="testFrame" style="display:none;" sandbox="allow-scripts allow-same-origin allow-forms allow-popups"></iframe>
<div id="toastContainer"></div>

<script>
/* ============================================================
   XORWEL — Constants & State
   ============================================================ */
const GAMES_JSON_URL = 'https://raw.githubusercontent.com/3kh0/3kh0-lite/main/config/games.json';
const GAMES_JSON_FALLBACK = 'https://cdn.jsdelivr.net/gh/3kh0/3kh0-lite@main/config/games.json';
const GAMES_PLAY_BASES = [
  'https://lite.3kh0.net/',
  'https://3kh0.github.io/',
  'https://cdn.jsdelivr.net/gh/Nailington/3kh0-assets@fixy/'
];
const GAMES_ICON_BASES = [
  'https://lite.3kh0.net/',
  'https://3kh0.github.io/',
  'https://cdn.jsdelivr.net/gh/Nailington/3kh0-assets@fixy/',
  'https://raw.githubusercontent.com/Nailington/3kh0-assets/fixy/'
];
const TMDB_API_KEY = '15d2ea6d0dc1d476efbca3eba2b9bbfb';
const THUMB_BASE = 'https://cdn.jsdelivr.net/gh/Nailington/3kh0-assets@fixy/';

let games = [];
let allTVItems = [];
let tvPageOffset = 4;
let tvCurrentCategory = 'home';
let tvGenresCache = { movie: [], tv: [] };
let tvWatchlist = [];

let filteredTVItems = [];
let proxyHistory = [];
let proxyHistoryIndex = -1;
let currentProxyUrl = '';
let proxyIncognito = false;
let ccEnabled = false;
let currentQuality = '360';
let currentSpeed = '1';
let currentPlayData = null;
let sourceTestActive = false;
let currentTestSourceIndex = 0;
let currentTestOrder = [];
let testCancelled = false;
let testTimeoutHandle = null;
let preferredSource = '';
let sourceTestTimeout = 4;
let currentTab = 'games';
let activeEngine = 'public';
let primaryEngine = 'public';
let sjReady = false;
let uvReady = false;

/* ============================================================
   DATA LAYER (per-user)
   ============================================================ */
let currentUser = localStorage.getItem('xorwel_current_user') || null;

function getUsers() { try { return JSON.parse(localStorage.getItem('xorwel_users') || '{}'); } catch(e) { return {}; } }
function saveUsers(u) { localStorage.setItem('xorwel_users', JSON.stringify(u)); }
function hashPass(s) { let h = 0; for (let i = 0; i < s.length; i++) { h = ((h << 5) - h) + s.charCodeAt(i); h |= 0; } return 'h' + Math.abs(h).toString(36); }
function defaultUserData() {
  return {
    favorites: [], bookmarks: [], recent: [], recentTV: [],
    playtime: 0, theme: 'dark', preferredSource: '',
    sourceTestTimeout: 4, customBackground: null,
    musicFavorites: [], musicRecent: [],
    primaryEngine: 'uv'
  };
}
function getData(key, fallback) {
  if (currentUser) {
    const users = getUsers();
    const u = users[currentUser];
    if (!u) return fallback;
    return u.data[key] !== undefined ? u.data[key] : fallback;
  }
  const g = localStorage.getItem('xorwel_guest_' + key);
  if (g === null) return fallback;
  try { return JSON.parse(g); } catch(e) { return fallback; }
}
function setData(key, value) {
  if (currentUser) {
    const users = getUsers();
    const u = users[currentUser];
    if (!u) return;
    u.data[key] = value;
    saveUsers(users);
  } else {
    localStorage.setItem('xorwel_guest_' + key, JSON.stringify(value));
  }
}

/* ============================================================
   AUTH
   ============================================================ */
let authMode = 'login';
function toggleAuthMode() {
  authMode = authMode === 'login' ? 'signup' : 'login';
  const login = document.getElementById('btnLogin');
  const signup = document.getElementById('btnSignup');
  const sw = document.getElementById('authSwitchBtn');
  if (authMode === 'login') { login.style.display='block'; signup.style.display='none'; sw.textContent='Need an account? Sign up'; }
  else { login.style.display='none'; signup.style.display='block'; sw.textContent='Already have an account? Sign in'; }
  document.getElementById('authError').textContent = '';
}
function doLogin() {
  const username = document.getElementById('authUsername').value.trim();
  const password = document.getElementById('authPassword').value;
  const err = document.getElementById('authError'); err.textContent = '';
  if (!username || !password) { err.textContent = 'Enter username and password'; return; }
  const users = getUsers();
  if (!users[username]) { err.textContent = 'User not found'; return; }
  if (users[username].pass !== hashPass(password)) { err.textContent = 'Wrong password'; return; }
  currentUser = username; localStorage.setItem('xorwel_current_user', username);
  toast('Welcome back, ' + username, 'success');
  refreshAccountUI(); loadUserData();
}
function doSignup() {
  const username = document.getElementById('authUsername').value.trim();
  const password = document.getElementById('authPassword').value;
  const err = document.getElementById('authError'); err.textContent = '';
  if (!username || !password) { err.textContent = 'Enter username and password'; return; }
  if (username.length < 3) { err.textContent = 'Username must be 3+ characters'; return; }
  if (password.length < 4) { err.textContent = 'Password must be 4+ characters'; return; }
  if (!/^[a-zA-Z0-9_-]+$/.test(username)) { err.textContent = 'Only letters, numbers, _ and -'; return; }
  const users = getUsers();
  if (users[username]) { err.textContent = 'Username taken'; return; }
  users[username] = { pass: hashPass(password), created: Date.now(), data: defaultUserData() };
  saveUsers(users);
  currentUser = username; localStorage.setItem('xorwel_current_user', username);
  toast('Account created! Welcome, ' + username, 'success');
  refreshAccountUI(); loadUserData();
}
function logout() {
  if (!confirm('Sign out? Your data stays saved on this device.')) return;
  currentUser = null; localStorage.removeItem('xorwel_current_user');
  toast('Signed out', 'info'); refreshAccountUI(); loadUserData();
}
function refreshAccountUI() {
  const miniAvatar = document.getElementById('avatarMini');
  const btnText = document.getElementById('accountBtnText');
  if (currentUser) {
    miniAvatar.textContent = currentUser[0].toUpperCase();
    btnText.textContent = currentUser;
    document.getElementById('accountSignedIn').style.display = 'block';
    document.getElementById('accountSignedOut').style.display = 'none';
    document.getElementById('accountAvatar').textContent = currentUser[0].toUpperCase();
    document.getElementById('accountName').textContent = currentUser;
    const users = getUsers(); const u = users[currentUser];
    if (u) { const d = new Date(u.created); document.getElementById('accountCreated').textContent = 'Member since ' + d.toLocaleDateString(); }
    updateAccountStats();
  } else {
    miniAvatar.textContent = '?'; btnText.textContent = 'Sign In';
    document.getElementById('accountSignedIn').style.display = 'none';
    document.getElementById('accountSignedOut').style.display = 'block';
  }
}
function updateAccountStats() {
  document.getElementById('statFavs').textContent = getData('favorites', []).length;
  document.getElementById('statBookmarks').textContent = getData('bookmarks', []).length;
  document.getElementById('statRecent').textContent = getData('recent', []).length;
  document.getElementById('statPlaytime').textContent = getData('playtime', 0);
}
function loadUserData() {
  setTheme(getData('theme', 'dark'), false);
  preferredSource = getData('preferredSource', '');
  sourceTestTimeout = getData('sourceTestTimeout', 3);
  primaryEngine = getData('primaryEngine', 'uv');
  document.getElementById('preferredSourceSelect').value = preferredSource;
  document.getElementById('sourceTimeoutInput').value = sourceTestTimeout;
  document.getElementById('primaryEngineSelect').value = primaryEngine;
  renderBookmarks();
  if (games.length) renderGames(games);
  if (allTVItems.length) renderTV(filteredTVItems);
  updateAccountStats();
  musicState.favorites = getData('musicFavorites', []);
  musicState.recent = getData('musicRecent', []);
}

/* ============================================================
   THEMES
   ============================================================ */
const SVG_CURSORS = {
  sakura: `url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='28' height='28' viewBox='0 0 28 28'><path d='M14 4C10 8 4 10 4 14s4 10 10 10 10-6 10-10-6-6-10-10z' fill='%23ffb7c5' stroke='%23ff85a1' stroke-width='1.5' opacity='0.9'/></svg>") 14 4, auto`,
  akihabara: `url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='24' height='24' viewBox='0 0 24 24'><circle cx='12' cy='12' r='6' fill='%23ff00cc' opacity='0.7'/><circle cx='12' cy='12' r='3' fill='%23ffffff'/></svg>") 12 12, auto`,
  tokyo: `url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='24' height='24' viewBox='0 0 24 24'><path d='M12 2L4 12l8 10 8-10z' fill='%23b57edc' opacity='0.85' stroke='%234a1e6e' stroke-width='1'/></svg>") 12 12, auto`,
  ghibli: `url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='28' height='28' viewBox='0 0 28 28'><circle cx='14' cy='14' r='8' fill='%23a8d5ba' opacity='0.85'/><circle cx='11' cy='12' r='1.5' fill='%23fff'/><circle cx='17' cy='12' r='1.5' fill='%23fff'/><path d='M11 17 Q14 20 17 17' stroke='%234a7c59' stroke-width='1.5' fill='none'/></svg>") 14 14, auto`,
  cartoon: `url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='28' height='28' viewBox='0 0 28 28'><circle cx='14' cy='14' r='10' fill='%23ffcc00' stroke='%23000' stroke-width='2'/><circle cx='11' cy='12' r='1.5' fill='%23000'/><circle cx='17' cy='12' r='1.5' fill='%23000'/><path d='M10 18 Q14 21 18 18' stroke='%23000' stroke-width='2' fill='none' stroke-linecap='round'/></svg>") 14 14, auto`,
  nick: `url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='28' height='28' viewBox='0 0 28 28'><path d='M14 4 L18 10 L24 12 L20 18 L18 24 L14 20 L10 24 L8 18 L4 12 L10 10 Z' fill='%23ff8800' stroke='%23000' stroke-width='1.5'/></svg>") 14 14, auto`,
  retro: `url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='24' height='24' viewBox='0 0 24 24'><polygon points='12,2 22,12 12,22 2,12' fill='%23e63946' stroke='%23000' stroke-width='1.5'/></svg>") 12 12, auto`,
  adventure: `url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='28' height='28' viewBox='0 0 28 28'><circle cx='14' cy='14' r='8' fill='%23ffd166' stroke='%23000' stroke-width='1.5'/><path d='M9 14 L14 9 L19 14 L14 19 Z' fill='%2306d6a0'/></svg>") 14 14, auto`,
  basketball: `url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='32' height='32' viewBox='0 0 32 32'><circle cx='16' cy='16' r='13' fill='%23d35400' stroke='%232c1810' stroke-width='2'/><path d='M3 16 Q16 12 29 16' stroke='%232c1810' stroke-width='1.8' fill='none'/><path d='M3 16 Q16 20 29 16' stroke='%232c1810' stroke-width='1.8' fill='none'/><path d='M16 3 Q13 16 16 29' stroke='%232c1810' stroke-width='1.8' fill='none'/><path d='M16 3 Q19 16 16 29' stroke='%232c1810' stroke-width='1.8' fill='none'/></svg>") 16 16, auto`
};

const themes = {
  dark: { cat:'standard', name:'Dark', bg:'#000', surface:'rgba(220,20,60,0.05)', solid:'#14000a', hover:'rgba(220,20,60,0.12)', text:'#ff4d6d', secondary:'#b03a52', accent:'#dc143c', border:'rgba(220,20,60,0.22)' },
  light: { cat:'standard', name:'Light', bg:'#fff5f7', surface:'rgba(220,20,60,0.06)', solid:'#fff', hover:'rgba(220,20,60,0.12)', text:'#8b0020', secondary:'#b03a52', accent:'#dc143c', border:'rgba(220,20,60,0.2)' },
  beige: { cat:'standard', name:'Beige', bg:'#1a0508', surface:'rgba(220,20,60,0.08)', solid:'#2a0a10', hover:'rgba(220,20,60,0.15)', text:'#ff8095', secondary:'#b03a52', accent:'#dc143c', border:'rgba(220,20,60,0.3)' },
  cherry: { cat:'standard', name:'Cherry', bg:'#1a0000', surface:'rgba(255,0,0,0.1)', solid:'#2a0000', hover:'rgba(255,0,0,0.2)', text:'#ff4d6d', secondary:'#ff9999', accent:'#ff0000', border:'rgba(255,0,0,0.35)' },
  forest: { cat:'standard', name:'Forest', bg:'#000a05', surface:'rgba(220,20,60,0.08)', solid:'#001a10', hover:'rgba(220,20,60,0.15)', text:'#ff4d6d', secondary:'#b03a52', accent:'#dc143c', border:'rgba(220,20,60,0.3)' },
  camo: { cat:'standard', name:'Camo', bg:'#0a0a00', surface:'rgba(128,128,0,0.12)', solid:'#1a1a00', hover:'rgba(128,128,0,0.22)', text:'#ff4d6d', secondary:'#999966', accent:'#dc143c', border:'rgba(220,20,60,0.3)' },
  strawberry: { cat:'standard', name:'Strawberry', bg:'#1a0010', surface:'rgba(255,0,128,0.12)', solid:'#2a001a', hover:'rgba(255,0,128,0.22)', text:'#ff4d6d', secondary:'#ff99bb', accent:'#dc143c', border:'rgba(255,0,128,0.35)' },
  midnight: { cat:'standard', name:'Midnight', bg:'#050010', surface:'rgba(220,20,60,0.06)', solid:'#0a0520', hover:'rgba(220,20,60,0.15)', text:'#ff4d6d', secondary:'#8888bb', accent:'#dc143c', border:'rgba(220,20,60,0.25)' },
  pizza: { cat:'standard', name:'Pizza', bg:'#150500', surface:'rgba(220,20,60,0.1)', solid:'#250800', hover:'rgba(220,20,60,0.2)', text:'#ff8095', secondary:'#ffb380', accent:'#dc143c', border:'rgba(220,20,60,0.35)' },
  sakura: { cat:'anime', name:'Sakura', bg:'#1a0005', surface:'rgba(255,182,193,0.15)', solid:'#2a0010', hover:'rgba(255,182,193,0.25)', text:'#ff8095', secondary:'#a86378', accent:'#ff69b4', border:'rgba(255,105,180,0.35)', cursor: SVG_CURSORS.sakura },
  akihabara: { cat:'anime', name:'Akihabara', bg:'#0d0221', surface:'rgba(220,20,60,0.15)', solid:'#1a0433', hover:'rgba(220,20,60,0.28)', text:'#ff4d6d', secondary:'#c77dff', accent:'#dc143c', border:'rgba(255,0,204,0.4)', cursor: SVG_CURSORS.akihabara },
  tokyo: { cat:'anime', name:'Tokyo Night', bg:'#0f0524', surface:'rgba(220,20,60,0.1)', solid:'#1a0b38', hover:'rgba(220,20,60,0.2)', text:'#ff4d6d', secondary:'#a880d0', accent:'#dc143c', border:'rgba(180,126,220,0.35)', cursor: SVG_CURSORS.tokyo },
  ghibli: { cat:'anime', name:'Ghibli', bg:'#0a150f', surface:'rgba(220,20,60,0.1)', solid:'#152520', hover:'rgba(220,20,60,0.18)', text:'#ff8095', secondary:'#6b8f7c', accent:'#dc143c', border:'rgba(74,124,89,0.35)', cursor: SVG_CURSORS.ghibli },
  cartoon: { cat:'cartoon', name:'Cartoon', bg:'#150a00', surface:'rgba(255,204,0,0.15)', solid:'#2a1500', hover:'rgba(255,204,0,0.28)', text:'#ff4d6d', secondary:'#8a5a2a', accent:'#dc143c', border:'rgba(255,102,0,0.35)', cursor: SVG_CURSORS.cartoon },
  nick: { cat:'cartoon', name:'Nickelodeon', bg:'#1a0a00', surface:'rgba(255,136,0,0.15)', solid:'#2a1500', hover:'rgba(255,136,0,0.28)', text:'#ff4d6d', secondary:'#8a5a2a', accent:'#dc143c', border:'rgba(255,102,0,0.4)', cursor: SVG_CURSORS.nick },
  retro: { cat:'cartoon', name:'Retro Cartoon', bg:'#1a0005', surface:'rgba(230,57,70,0.15)', solid:'#2a0510', hover:'rgba(230,57,70,0.28)', text:'#ff4d6d', secondary:'#7a3a42', accent:'#dc143c', border:'rgba(230,57,70,0.4)', cursor: SVG_CURSORS.retro },
  adventure: { cat:'cartoon', name:'Adventure', bg:'#0a0f0a', surface:'rgba(255,209,102,0.15)', solid:'#1a1f15', hover:'rgba(255,209,102,0.25)', text:'#ff4d6d', secondary:'#3a6d5a', accent:'#dc143c', border:'rgba(6,214,160,0.35)', cursor: SVG_CURSORS.adventure },
  nba2k27: { cat:'cartoon', name:'NBA 2K27', bg:'#0a0a1a', surface:'rgba(255,107,0,0.12)', solid:'#16162a', hover:'rgba(255,107,0,0.22)', text:'#ff4d6d', secondary:'#a0a0c0', accent:'#dc143c', border:'rgba(255,107,0,0.4)', cursor: SVG_CURSORS.basketball,
    background: 'linear-gradient(135deg, rgba(10,10,26,0.94), rgba(40,10,0,0.94)), radial-gradient(circle at 20% 80%, #dc143c 0%, transparent 55%), radial-gradient(circle at 80% 20%, #1e3a8a 0%, transparent 55%), linear-gradient(180deg, #0a0a1a, #2a0a00)' },
  codwarzone: { cat:'cartoon', name:'COD Warzone', bg:'#0a0a0a', surface:'rgba(220,20,60,0.08)', solid:'#14161a', hover:'rgba(220,20,60,0.18)', text:'#ff4d6d', secondary:'#8a8a80', accent:'#dc143c', border:'rgba(220,20,60,0.35)', cursor: null,
    background: 'linear-gradient(135deg, rgba(10,10,10,0.9), rgba(20,22,15,0.9)), radial-gradient(circle at 15% 85%, rgba(220,20,60,0.4) 0%, transparent 50%), radial-gradient(circle at 85% 15%, rgba(255,140,50,0.3) 0%, transparent 50%), linear-gradient(180deg, #0a0a0a, #1a0a05)' }
};

function hexToRgb(hex) {
  const h = hex.replace('#','');
  if (h.length !== 6) return null;
  return [parseInt(h.substring(0,2),16), parseInt(h.substring(2,4),16), parseInt(h.substring(4,6),16)];
}

function buildThemeGrid() {
  const cats = { standard: 'themeGridStandard', anime: 'themeGridAnime', cartoon: 'themeGridCartoon' };
  Object.entries(cats).forEach(([cat, id]) => {
    const el = document.getElementById(id); if (!el) return;
    el.innerHTML = '';
    Object.entries(themes).forEach(([key, t]) => {
      if (t.cat !== cat) return;
      const chip = document.createElement('div');
      chip.className = 'theme-chip';
      chip.dataset.theme = key;
      chip.innerHTML = `<div class="swatch" style="background:${t.accent}"></div>${t.name}`;
      chip.onclick = () => setTheme(key);
      el.appendChild(chip);
    });
  });
}

function setTheme(name, save = true) {
  const t = themes[name] || themes.dark;
  const r = document.documentElement;
  r.style.setProperty('--bg', t.bg);
  r.style.setProperty('--surface', t.surface);
  r.style.setProperty('--surface-solid', t.solid);
  r.style.setProperty('--surface-hover', t.hover);
  r.style.setProperty('--text', t.text);
  r.style.setProperty('--text-secondary', t.secondary);
  r.style.setProperty('--accent', t.accent);
  r.style.setProperty('--accent-hover', t.accent);
  r.style.setProperty('--border', t.border);
  r.style.setProperty('--cursor', t.cursor || 'auto');
  const rgb = hexToRgb(t.accent);
  if (rgb) r.style.setProperty('--accent-rgb', rgb.join(','));

  const cb = getData('customBackground', null);
  if (cb && cb.startsWith('image:')) {
    document.body.style.background = `linear-gradient(rgba(0,0,0,0.6), rgba(0,0,0,0.6)), url(${cb.substring(6)}) no-repeat center center fixed`;
    document.body.style.backgroundSize = 'cover';
  } else if (cb && cb.startsWith('video:')) {
    const id = cb.substring(6);
    document.body.style.background = `linear-gradient(rgba(0,0,0,0.7), rgba(0,0,0,0.7)), url(https://img.youtube.com/vi/${id}/hqdefault.jpg) no-repeat center center fixed`;
    document.body.style.backgroundSize = 'cover';
  } else if (t.background) {
    document.body.style.background = t.background;
    document.body.style.backgroundAttachment = 'fixed';
    document.body.style.backgroundSize = 'cover';
  } else {
    document.body.style.background = `radial-gradient(circle at 15% 15%, rgba(220,20,60,0.08) 0%, transparent 50%), radial-gradient(circle at 85% 85%, rgba(220,20,60,0.06) 0%, transparent 50%), linear-gradient(180deg, #000000 0%, #0a0005 100%)`;
    document.body.style.backgroundAttachment = 'fixed';
  }

  if (save) setData('theme', name);
  document.querySelectorAll('.theme-chip').forEach(c => c.classList.toggle('active', c.dataset.theme === name));
}

/* ============================================================
   TV / MOVIE EMBED SOURCES
   ============================================================ */
const EMBED_SOURCES = [
  { name: 'ezvidapi', base: 'https://ezvidapi.com/embed/', type: 'tmdb', tier: 1 },
  { name: 'filmu', base: 'https://embed.filmu.in/', type: 'tmdb', tier: 1 },
  { name: 'vidrift', base: 'https://embed.vidrift.in/embed/', type: 'tmdb', tier: 1 },
  { name: 'vidspark', base: 'https://vidspark.to/', type: 'tmdb', tier: 1 },
  { name: 'vidnest', base: 'https://vidnest.fun/', type: 'tmdb', tier: 1 },
  { name: 'moviesapi.to', base: 'https://moviesapi.to/', type: 'tmdb', tier: 1 },
  { name: 'vidfast', base: 'https://vidfast.pro/', type: 'tmdb', tier: 2 },
  { name: 'vidlink.pro', base: 'https://vidlink.pro/', type: 'tmdb', tier: 2 },
  { name: 'vidsrc.pm', base: 'https://vidsrc.pm/embed/', type: 'tmdb', tier: 2 },
  { name: 'vidsrc.to', base: 'https://vidsrc.to/embed/', type: 'tmdb', tier: 2 },
  { name: 'vidsrc.cc', base: 'https://vidsrc.cc/v2/embed/', type: 'tmdb', tier: 2 },
  { name: '2embed.skin', base: 'https://www.2embed.skin/embed/', type: 'imdb', tier: 2 },
  { name: '2embed.cc', base: 'https://www.2embed.cc/embed/', type: 'imdb', tier: 2 },
  { name: 'vidsrc.wtf', base: 'https://vidsrc.wtf/api/1/', type: 'tmdb', tier: 2 },
  { name: 'nontongo', base: 'https://nontongo.win/embed/', type: 'tmdb', tier: 3 },
  { name: 'autoembed.co', base: 'https://autoembed.co/movie/tmdb/', type: 'tmdb', tier: 3 },
  { name: 'smashystream', base: 'https://player.smashy.stream/', type: 'tmdb', tier: 3 },
  { name: 'frembed', base: 'https://frembed.xyz/api/film.php?id=', type: 'imdb', tier: 3 },
  { name: 'vidsrc.link', base: 'https://vidsrc.link/embed/', type: 'tmdb', tier: 3 },
  { name: 'vidsrc.hair', base: 'https://vidsrc.hair/embed/', type: 'tmdb', tier: 3 },
  { name: 'vid-src.top', base: 'https://vid-src.top/embed/', type: 'tmdb', tier: 3 },
  { name: '111movies', base: 'https://111movies.com/', type: 'tmdb', tier: 3 },
  { name: 'multiembed', base: 'https://multiembed.mov/?video_id=', type: 'imdb', tier: 3 },
  { name: 'superembed', base: 'https://multiembed.mov/directstream.php?video_id=', type: 'imdb', tier: 3 }
];

const ANIME_SOURCES = [
  { name: 'dropfile-mal', base: 'https://dropfile.cc/player/tv/mal-', type: 'malid', tier: 1 },
  { name: 'dropfile-tmdb', base: 'https://dropfile.cc/player/tv/', type: 'tmdb', tier: 1 },
  { name: 'ezvidapi-anime', base: 'https://ezvidapi.com/embed/', type: 'tmdb', tier: 1 },
  { name: 'vidnest-anime', base: 'https://vidnest.fun/anime/', type: 'malid', tier: 2 },
  { name: 'vibeplayer', base: 'https://vibeplayer.site/embed/', type: 'malid', tier: 2 },
  { name: 'filmu-anime', base: 'https://embed.filmu.in/', type: 'tmdb', tier: 2 },
  { name: 'vidfast-anime', base: 'https://vidfast.pro/', type: 'tmdb', tier: 2 }
];

/* ============================================================
   FALLBACK GAMES + CLOUD GAMES
   ============================================================ */
const MANUAL_GAMES = [
  { title: "2048", link: "projects/2048/index.html", imgSrc: "projects/2048/thumb.png" },
  { title: "Slope", link: "projects/slope/index.html", imgSrc: "projects/slope/splash.png" },
  { title: "1v1.LOL", link: "projects/1v1lol/index.html", imgSrc: "projects/1v1lol/splash.png" },
  { title: "Cookie Clicker", link: "projects/cookie-clicker/index.html", imgSrc: "projects/cookie-clicker/splash.png" },
  { title: "Retro Bowl", link: "projects/retro-bowl/index.html", imgSrc: "projects/retro-bowl/splash.png" },
  { title: "Basketball Stars", link: "projects/basketball-stars/index.html", imgSrc: "projects/basketball-stars/splash.png" },
  { title: "Crossy Road", link: "projects/crossyroad/index.html", imgSrc: "projects/crossyroad/splash.png" },
  { title: "Flappy Bird", link: "projects/flappy-bird/index.html", imgSrc: "projects/flappy-bird/splash.png" },
  { title: "Geometry Dash", link: "projects/geometry-dash/index.html", imgSrc: "projects/geometry-dash/splash.png" },
  { title: "Subway Surfers", link: "projects/subway-surfers/index.html", imgSrc: "projects/subway-surfers/splash.png" },
  { title: "Temple Run 2", link: "projects/temple-run-2/index.html", imgSrc: "projects/temple-run-2/img/og-icon.png" },
  { title: "Doodle Jump", link: "projects/doodle-jump/index.html", imgSrc: "projects/doodle-jump/splash.png" },
  { title: "Moto X3M", link: "projects/motox3m/index.html", imgSrc: "projects/motox3m/splash.png" },
  { title: "Among Us", link: "projects/among-us/index.html", imgSrc: "projects/among-us/red.png" },
  { title: "Paper.io 2", link: "projects/paperio2/index.html", imgSrc: "projects/paperio2/splash.png" },
  { title: "Snake", link: "projects/snake/index.html", imgSrc: "projects/snake/splash.png" },
  { title: "Tetris", link: "projects/tetris/index.html", imgSrc: "projects/tetris/splash.png" },
  { title: "Chess", link: "projects/chess/index.html", imgSrc: "projects/chess/splash.png" },
  { title: "Solitaire", link: "projects/solitaire/index.html", imgSrc: "projects/solitaire/splash.png" },
  { title: "Wordle", link: "projects/wordle/index.html", imgSrc: "projects/wordle/splash.png" },
  { title: "Run 3", link: "projects/run3/index.html", imgSrc: "projects/run3/splash.png" },
  { title: "Vex 3", link: "projects/vex3/index.html", imgSrc: "projects/vex3/splash.png" },
  { title: "Vex 4", link: "projects/vex4/index.html", imgSrc: "projects/vex4/splash.png" },
  { title: "Vex 5", link: "projects/vex5/index.html", imgSrc: "projects/vex5/splash.png" },
  { title: "Vex 6", link: "projects/vex6/index.html", imgSrc: "projects/vex6/splash.png" },
  { title: "Vex 7", link: "projects/vex7/index.html", imgSrc: "projects/vex7/splash.png" },
  { title: "Idle Breakout", link: "projects/idle-breakout/index.html", imgSrc: "projects/idle-breakout/splash.png" },
  { title: "Getaway Shootout", link: "projects/getaway-shootout/index.html", imgSrc: "projects/getaway-shootout/splash.png" },
  { title: "Tanuki Sunset", link: "projects/tanuki-sunset/index.html", imgSrc: "projects/tanuki-sunset/img/cover.png" },
  { title: "There Is No Game", link: "projects/there-is-no-game/index.html", imgSrc: "projects/there-is-no-game/logo.png" },
  { title: "HexGL", link: "projects/hexgl/index.html", imgSrc: "projects/hexgl/splash.png" },
  { title: "Doge Miner", link: "projects/DogeMiner/index.html", imgSrc: "projects/DogeMiner/splash.png" },
  { title: "Chrome Dino", link: "projects/chrome-dino/index.html", imgSrc: "projects/chrome-dino/splash.png" },
  { title: "Cut the Rope", link: "projects/cut-the-rope/index.html", imgSrc: "projects/cut-the-rope/splash.png" },
  { title: "Bloons TD", link: "projects/bloonstd/index.html", imgSrc: "projects/bloonstd/splash.png" },
  { title: "World's Hardest Game", link: "projects/worlds-hardest-game/index.html", imgSrc: "projects/worlds-hardest-game/splash.png" },
  { title: "Fireboy and Watergirl", link: "projects/fireboywatergirl/index.html", imgSrc: "projects/fireboywatergirl/splash.png" },
  { title: "Shell Shockers", url: "https://shellshock.io/", icon: "https://shellshock.io/favicon.ico" },
  { title: "Krunker", url: "https://krunker.io/", icon: "https://krunker.io/img/favicon.png" },
  { title: "Minecraft Classic", url: "https://classic.minecraft.net/", icon: "https://classic.minecraft.net/favicon.ico" }
];

const CLOUD_GAMES = [
  { title: "GeForce NOW", url: "https://play.geforcenow.com", icon: "https://img.icons8.com/color/96/nvidia.png", desc: "Stream your Steam, Epic & Ubisoft library on RTX servers. Free + Priority/Ultimate tiers.", tags: ["aaa", "browser", "free"], badge: "Popular", featured: true, openMode: "tab" },
  { title: "Xbox Cloud Gaming", url: "https://www.xbox.com/play", icon: "https://img.icons8.com/color/96/xbox.png", desc: "Game Pass library in the browser or app. Hundreds of titles with Game Pass Ultimate.", tags: ["aaa", "console", "browser"], badge: "Game Pass", featured: true, openMode: "tab" },
  { title: "Amazon Luna", url: "https://luna.amazon.com", icon: "https://img.icons8.com/color/96/amazon.png", desc: "Amazon's cloud platform with channels and Ubisoft+ integration. Browser & Fire TV.", tags: ["aaa", "browser"], badge: "Channels", featured: true, openMode: "tab" },
  { title: "Boosteroid", url: "https://boosteroid.com", icon: "https://img.icons8.com/color/96/controller.png", desc: "Play your own PC games on remote machines. Strong European coverage.", tags: ["aaa", "pc", "browser"], badge: "Own library", featured: false, openMode: "tab" },
  { title: "Shadow PC", url: "https://shadow.tech", icon: "https://img.icons8.com/color/96/cloud.png", desc: "Full Windows cloud PC — install anything, not just games. Subscription required.", tags: ["pc", "aaa"], badge: "Full PC", featured: true, openMode: "tab" },
  { title: "PlayStation Plus Premium", url: "https://www.playstation.com/en-us/ps-plus/", icon: "https://img.icons8.com/color/96/playstation.png", desc: "PS5 / classic streaming via Premium tier. Best on PS5 or companion apps.", tags: ["console", "aaa"], badge: "PS+", featured: false, openMode: "tab" },
  { title: "NVIDIA GeForce NOW (info)", url: "https://www.nvidia.com/en-us/geforce-now/", icon: "https://img.icons8.com/color/96/nvidia-geforce.png", desc: "Plans, supported games, and download links for GeForce NOW apps.", tags: ["aaa", "browser"], badge: "Info", featured: false, openMode: "tab" },
  { title: "AirGPU", url: "https://www.airgpu.com", icon: "https://img.icons8.com/color/96/video-card.png", desc: "Rent high-end GPUs by the hour for gaming or creative work.", tags: ["pc", "aaa"], badge: "Hourly", featured: false, openMode: "tab" },
  { title: "PaperSpace", url: "https://www.paperspace.com", icon: "https://img.icons8.com/color/96/server.png", desc: "Cloud workstations and GPU machines — flexible for gaming setups.", tags: ["pc"], badge: "Workstation", featured: false, openMode: "tab" },
  { title: "CrazyGames", url: "https://www.crazygames.com", icon: "https://img.icons8.com/color/96/joystick.png", desc: "Instant browser games — no account required for most titles.", tags: ["browser", "free"], badge: "Free", featured: true, openMode: "iframe" },
  { title: "Poki", url: "https://poki.com", icon: "https://img.icons8.com/color/96/controller.png", desc: "Curated free browser games, great on school networks.", tags: ["browser", "free"], badge: "Free", featured: false, openMode: "iframe" },
  { title: "Armor Games", url: "https://armorgames.com", icon: "https://img.icons8.com/color/96/sword.png", desc: "Classic & modern browser games library.", tags: ["browser", "free"], badge: "Free", featured: false, openMode: "iframe" },
  { title: "itch.io", url: "https://itch.io/games/html5", icon: "https://img.icons8.com/color/96/game-controller.png", desc: "Indie HTML5 games — many free, play in browser.", tags: ["browser", "free"], badge: "Indie", featured: false, openMode: "iframe" },
  { title: "Steam Remote Play", url: "https://store.steampowered.com/remoteplay", icon: "https://img.icons8.com/color/96/steam.png", desc: "Stream from your own PC to another device via Steam.", tags: ["pc", "free"], badge: "Your PC", featured: false, openMode: "tab" },
  { title: "Parsec", url: "https://parsec.app", icon: "https://img.icons8.com/color/96/network.png", desc: "Ultra-low latency remote desktop for gaming with friends.", tags: ["pc", "free"], badge: "Low latency", featured: false, openMode: "tab" },
  { title: "Moonlight + Sunshine", url: "https://moonlight-stream.org", icon: "https://img.icons8.com/color/96/moon-symbol.png", desc: "Open-source GameStream client — stream from your NVIDIA/Sunshine host.", tags: ["pc", "free"], badge: "Open source", featured: false, openMode: "tab" },
  { title: "Blacknut", url: "https://www.blacknut.com", icon: "https://img.icons8.com/color/96/cloud-gaming.png", desc: "Subscription cloud catalog with family profiles.", tags: ["aaa", "browser"], badge: "Catalog", featured: false, openMode: "tab" },
  { title: "Antstream Arcade", url: "https://www.antstream.com", icon: "https://img.icons8.com/color/96/arcade-cabinet.png", desc: "Cloud classic arcade & console games with challenges.", tags: ["browser", "console"], badge: "Retro", featured: false, openMode: "tab" },
  { title: "Netflix Games", url: "https://www.netflix.com/games", icon: "https://img.icons8.com/color/96/netflix.png", desc: "Included with Netflix — mobile & some TV platforms.", tags: ["free", "console"], badge: "Included", featured: false, openMode: "tab" },
  { title: "Apple Arcade", url: "https://www.apple.com/apple-arcade/", icon: "https://img.icons8.com/color/96/apple-logo.png", desc: "Ad-free subscription games on Apple devices (not browser cloud).", tags: ["console"], badge: "Apple", featured: false, openMode: "tab" },
  { title: "VK Play Cloud", url: "https://cloud.vkplay.ru", icon: "https://img.icons8.com/color/96/cloud.png", desc: "Cloud gaming popular in CIS regions.", tags: ["aaa", "browser"], badge: "Regional", featured: false, openMode: "tab" },
  { title: "OnePlay", url: "https://oneplay.in", icon: "https://img.icons8.com/color/96/controller.png", desc: "Cloud gaming focused on India & nearby regions.", tags: ["aaa", "browser"], badge: "Regional", featured: false, openMode: "tab" },
  { title: "GeForce NOW Free Games", url: "https://www.nvidia.com/en-us/geforce-now/free-games/", icon: "https://img.icons8.com/color/96/nvidia.png", desc: "List of titles playable on the free GeForce NOW tier.", tags: ["free", "aaa", "browser"], badge: "Free tier", featured: false, openMode: "tab" },
  { title: "Xbox Cloud Free (with ads)", url: "https://www.xbox.com/en-US/cloud-gaming", icon: "https://img.icons8.com/color/96/xbox.png", desc: "Select titles available free with ads in supported regions.", tags: ["free", "console", "browser"], badge: "Free+", featured: false, openMode: "tab" }
];


/* ============================================================
   GAMES DATABASE
   ============================================================ */
function normalizeGameEntry(raw) {
  const title = raw.title || raw.name || 'Game';
  let link = raw.url || raw.link || '';
  let icon = raw.icon || raw.imgSrc || '';

  if (link && !/^https?:\/\//i.test(link)) {
    const rel = link.replace(/^\/+/, '');
    link = GAMES_PLAY_BASES[0] + rel;
  }
  if (icon && !/^https?:\/\//i.test(icon)) {
    const rel = icon.replace(/^\/+/, '');
    icon = GAMES_ICON_BASES[0] + rel;
  }
  let slug = '';
  try {
    const pathPart = (raw.link || raw.url || '').replace(/^projects\//, '').split('/')[0];
    slug = pathPart || '';
  } catch (e) {}
  if (!icon && slug) {
    icon = THUMB_BASE + slug + '/splash.png';
  }
  return { title, url: link, icon, slug };
}

async function loadGames() {
  const grid = document.getElementById('gameGrid');
  const counter = document.getElementById('gameCountLabel');
  grid.innerHTML = '<div class="loading"><div class="spinner"></div>Loading games...</div>';
  let list = [];
  try {
    let res = await fetch(GAMES_JSON_URL);
    if (!res.ok) throw new Error('primary failed');
    list = await res.json();
  } catch (e1) {
    try {
      const res2 = await fetch(GAMES_JSON_FALLBACK);
      if (!res2.ok) throw new Error('fallback failed');
      list = await res2.json();
    } catch (e2) {
      list = MANUAL_GAMES;
      toast('Using offline game list', 'warning');
    }
  }
  games = (Array.isArray(list) ? list : []).map(normalizeGameEntry).filter(g => g.url);
  const titles = new Set(games.map(g => g.title.toLowerCase()));
  MANUAL_GAMES.forEach(m => {
    const n = normalizeGameEntry(m);
    if (n.url && !titles.has(n.title.toLowerCase())) {
      games.push(n);
      titles.add(n.title.toLowerCase());
    }
  });
  counter.textContent = `(${games.length} games)`;
  renderGames(games);
  if (games.length) toast(`Loaded ${games.length} games`, 'success');
}

function renderGames(list) {
  const grid = document.getElementById('gameGrid');
  grid.innerHTML = '';
  if (!list.length) {
    grid.innerHTML = '<div class="loading">No games available. Check your connection.</div>';
    return;
  }
  list.forEach((game, i) => {
    const card = document.createElement('div');
    card.className = 'card';
    card.style.animationDelay = Math.min(i * 3, 350) + 'ms';
    const iconUrl = game.icon || '';
    const safeUrl = (game.url || '').replace(/'/g, "\\'");
    const safeTitle = (game.title || '').replace(/'/g, "\\'");
    const slug = (game.slug || '').replace(/'/g, "\\'");
    card.innerHTML = `
      <div class="poster">
        ${iconUrl ? `<img src="${iconUrl}" loading="lazy" data-step="0" onload="this.classList.add('loaded')" onerror="gameIconFallback(this,'${slug}')">` : ''}
        <div class="skeleton"></div>
        <div class="fallback-icon" style="display:${iconUrl ? 'none' : 'flex'};"><svg viewBox="0 0 24 24"><use href="#i-games"/></svg></div>
      </div>
      <h3>${escapeHtml(game.title)}</h3>
      <button class="play-btn" onclick="event.stopPropagation();playGame('${safeUrl}','${safeTitle}')">
        <svg viewBox="0 0 24 24"><use href="#i-play"/></svg>
      </button>
    `;
    card.addEventListener('click', () => playGame(game.url, game.title));
    grid.appendChild(card);
    pushRecentItem(game.title, 'games');
  });
}

function gameIconFallback(img, slug) {
  const step = (+img.dataset.step || 0) + 1;
  img.dataset.step = step;
  if (!slug) {
    img.style.display = 'none';
    const fb = img.parentElement && img.parentElement.querySelector('.fallback-icon');
    if (fb) fb.style.display = 'flex';
    return;
  }
  if (step === 1) img.src = THUMB_BASE + slug + '/splash.png';
  else if (step === 2) img.src = THUMB_BASE + slug + '/' + slug + '.png';
  else if (step === 3) img.src = 'https://lite.3kh0.net/projects/' + slug + '/splash.png';
  else if (step === 4) img.src = 'https://3kh0.github.io/projects/' + slug + '/splash.png';
  else {
    img.style.display = 'none';
    const fb = img.parentElement && img.parentElement.querySelector('.fallback-icon');
    if (fb) fb.style.display = 'flex';
  }
}


function playGame(url, title) {
  if (!url) { toast('No game URL', 'error'); return; }
  if (!/^https?:\/\//i.test(url)) {
    url = GAMES_PLAY_BASES[0] + url.replace(/^\/+/, '');
  }
  if (title) {
    const r = getData('recent', []).filter(x => x.title !== title);
    r.unshift({ title, url, ts: Date.now() });
    setData('recent', r.slice(0, 20));
  }
  openPlayer(url);
  toast('Loading ' + (title || 'game') + '...', 'info', 2000);
}

let cloudFilter = 'all';

function getCloudFiltered() {
  const q = (document.getElementById('cloudSearch')?.value || '').toLowerCase().trim();
  return CLOUD_GAMES.filter(g => {
    if (cloudFilter !== 'all' && !(g.tags || []).includes(cloudFilter)) return false;
    if (!q) return true;
    const hay = (g.title + ' ' + (g.desc || '') + ' ' + (g.tags || []).join(' ')).toLowerCase();
    return hay.includes(q);
  });
}

function filterCloud(tag) {
  if (tag != null && tag !== '') {
    cloudFilter = tag;
    document.querySelectorAll('#cloudFilters .theme-chip').forEach(b => {
      b.classList.toggle('active', b.getAttribute('data-cloud-filter') === cloudFilter);
    });
  }
  renderCloudGames();
}

function launchCloud(url, title, mode) {
  if (!url) return;
  if (mode === 'tab') {
    window.open(url, '_blank', 'noopener,noreferrer');
    toast('Opened ' + title + ' in a new tab', 'success');
    const r = getData('recent', []).filter(x => x.title !== title);
    r.unshift({ title, url, ts: Date.now(), type: 'cloud' });
    setData('recent', r.slice(0, 20));
    return;
  }
  playGame(url, title);
  toast('Loading in player — use Open tab if blank', 'info', 3500);
}

function renderCloudGames() {
  const grid = document.getElementById('cloudGrid');
  const featured = document.getElementById('cloudFeatured');
  if (!grid) return;
  grid.innerHTML = '';
  if (featured) featured.innerHTML = '';

  const list = getCloudFiltered();
  const featuredList = list.filter(g => g.featured);
  const rest = list.filter(g => !g.featured);

  if (featured && featuredList.length && cloudFilter === 'all' && !(document.getElementById('cloudSearch')?.value || '').trim()) {
    featuredList.forEach((game, i) => {
      const el = document.createElement('div');
      el.className = 'card';
      el.style.cssText = 'flex-direction:row;align-items:stretch;min-height:120px;padding:0;overflow:hidden;cursor:pointer;';
      el.style.animationDelay = (i * 40) + 'ms';
      const safeTitle = (game.title || '').replace(/'/g, "\\'");
      const mode = game.openMode || 'tab';
      el.innerHTML = `
        <div style="width:100px;flex-shrink:0;background:linear-gradient(135deg,rgba(220,20,60,0.25),rgba(0,0,0,0.4));display:flex;align-items:center;justify-content:center;position:relative;">
          <img src="${game.icon}" style="width:56px;height:56px;object-fit:contain;" onerror="this.style.display='none'">
          ${game.badge ? `<span style="position:absolute;top:8px;left:8px;font-size:0.65rem;background:var(--accent);color:#fff;padding:0.15rem 0.4rem;border-radius:6px;font-weight:700;">${escapeHtml(game.badge)}</span>` : ''}
        </div>
        <div style="padding:0.85rem 1rem;flex:1;display:flex;flex-direction:column;justify-content:center;gap:0.35rem;">
          <h3 style="margin:0;font-size:1rem;">${escapeHtml(game.title)}</h3>
          <p style="margin:0;font-size:0.78rem;color:var(--text-dim);line-height:1.35;">${escapeHtml(game.desc || '')}</p>
          <div style="display:flex;gap:0.4rem;flex-wrap:wrap;margin-top:0.25rem;">
            ${(game.tags || []).map(t => `<span style="font-size:0.65rem;padding:0.15rem 0.45rem;border-radius:999px;border:1px solid var(--border);color:var(--text-dim);">${escapeHtml(t)}</span>`).join('')}
          </div>
          <div style="display:flex;gap:0.4rem;margin-top:0.4rem;">
            <button class="play-btn" style="position:static;opacity:1;transform:none;width:auto;padding:0.35rem 0.75rem;border-radius:8px;"
              onclick="event.stopPropagation();launchCloud('${game.url}','${safeTitle}','${mode}')">
              <svg viewBox="0 0 24 24" style="width:14px;height:14px;"><use href="#i-play"/></svg> Launch
            </button>
            <button class="play-btn" style="position:static;opacity:1;transform:none;width:auto;padding:0.35rem 0.75rem;border-radius:8px;background:rgba(255,255,255,0.08);"
              onclick="event.stopPropagation();launchCloud('${game.url}','${safeTitle}','tab')">
              Open tab
            </button>
          </div>
        </div>
      `;
      el.addEventListener('click', () => launchCloud(game.url, game.title, mode));
      featured.appendChild(el);
    });
  }

  const toRender = (cloudFilter === 'all' && !(document.getElementById('cloudSearch')?.value || '').trim())
    ? rest
    : list;

  if (!toRender.length && !(featured && featured.children.length)) {
    grid.innerHTML = '<div class="loading">No cloud services match your filter.</div>';
    return;
  }

  toRender.forEach((game, i) => {
    const card = document.createElement('div');
    card.className = 'card';
    card.style.animationDelay = Math.min(i * 3, 350) + 'ms';
    const safeTitle = (game.title || '').replace(/'/g, "\\'");
    const mode = game.openMode || 'tab';
    card.innerHTML = `
      <div class="poster" style="aspect-ratio:1;display:flex;align-items:center;justify-content:center;background:linear-gradient(160deg,rgba(220,20,60,0.12),rgba(0,0,0,0.35));">
        <img src="${game.icon}" loading="lazy" style="width:48%;height:48%;object-fit:contain;padding:0;" onload="this.classList.add('loaded')" onerror="this.style.display='none';this.nextElementSibling.style.display='flex';">
        <div class="skeleton"></div>
        <div class="fallback-icon" style="display:none;"><svg viewBox="0 0 24 24"><use href="#i-cloud"/></svg></div>
        ${game.badge ? `<span style="position:absolute;top:8px;left:8px;font-size:0.65rem;background:var(--accent);color:#fff;padding:0.15rem 0.4rem;border-radius:6px;font-weight:700;z-index:2;">${escapeHtml(game.badge)}</span>` : ''}
      </div>
      <h3>${escapeHtml(game.title)}</h3>
      <div class="meta">${escapeHtml((game.tags || ['cloud']).slice(0, 2).join(' · '))}</div>
      <p style="font-size:0.72rem;color:var(--text-dim);padding:0 0.6rem 0.4rem;line-height:1.3;display:-webkit-box;-webkit-line-clamp:2;-webkit-box-orient:vertical;overflow:hidden;">${escapeHtml(game.desc || '')}</p>
      <div style="display:flex;gap:0.35rem;padding:0 0.6rem 0.75rem;margin-top:auto;">
        <button class="play-btn" style="position:static;opacity:1;transform:none;width:auto;flex:1;padding:0.4rem;border-radius:8px;"
          onclick="event.stopPropagation();launchCloud('${game.url}','${safeTitle}','${mode}')">
          <svg viewBox="0 0 24 24" style="width:14px;height:14px;"><use href="#i-play"/></svg>
        </button>
        <button class="play-btn" style="position:static;opacity:1;transform:none;width:auto;padding:0.4rem 0.55rem;border-radius:8px;background:rgba(255,255,255,0.08);"
          title="Open in new tab" onclick="event.stopPropagation();launchCloud('${game.url}','${safeTitle}','tab')">↗</button>
      </div>
    `;
    card.addEventListener('click', () => launchCloud(game.url, game.title, mode));
    grid.appendChild(card);
  });
}


/* ============================================================
   TV — Movies, Shows, Anime
   ============================================================ */
async function loadTV() {
  const grid = document.getElementById('tvGrid');
  grid.innerHTML = '<div class="loading"><div class="spinner"></div>Loading movies, shows & anime from TMDB + MAL...</div>';
  allTVItems = [];
  tvPageOffset = 4;

  loadTvGenres();

  const tmdbRequests = [];
  for (let p = 1; p <= 4; p++) {
    tmdbRequests.push(
      fetch(`https://api.themoviedb.org/3/movie/popular?api_key=${TMDB_API_KEY}&page=${p}`)
        .then(r => r.json()).then(d => (d.results || []).map(x => ({...x, media_type:'movie', _src:'popular'})))
        .catch(() => [])
    );
    tmdbRequests.push(
      fetch(`https://api.themoviedb.org/3/movie/top_rated?api_key=${TMDB_API_KEY}&page=${p}`)
        .then(r => r.json()).then(d => (d.results || []).map(x => ({...x, media_type:'movie', _src:'top'})))
        .catch(() => [])
    );
  }
  for (let p = 1; p <= 2; p++) {
    tmdbRequests.push(
      fetch(`https://api.themoviedb.org/3/movie/now_playing?api_key=${TMDB_API_KEY}&page=${p}`)
        .then(r => r.json()).then(d => (d.results || []).map(x => ({...x, media_type:'movie', _src:'now_playing'})))
        .catch(() => [])
    );
    tmdbRequests.push(
      fetch(`https://api.themoviedb.org/3/movie/upcoming?api_key=${TMDB_API_KEY}&page=${p}`)
        .then(r => r.json()).then(d => (d.results || []).map(x => ({...x, media_type:'movie', _src:'upcoming'})))
        .catch(() => [])
    );
  }
  tmdbRequests.push(
    fetch(`https://api.themoviedb.org/3/trending/movie/day?api_key=${TMDB_API_KEY}`)
      .then(r => r.json()).then(d => (d.results || []).map(x => ({...x, media_type:'movie', _src:'trending'})))
      .catch(() => [])
  );
  tmdbRequests.push(
    fetch(`https://api.themoviedb.org/3/trending/tv/day?api_key=${TMDB_API_KEY}`)
      .then(r => r.json()).then(d => (d.results || []).map(x => ({...x, media_type:'tv', _src:'trending'})))
      .catch(() => [])
  );
  for (let p = 1; p <= 4; p++) {
    tmdbRequests.push(
      fetch(`https://api.themoviedb.org/3/tv/popular?api_key=${TMDB_API_KEY}&page=${p}`)
        .then(r => r.json()).then(d => (d.results || []).map(x => ({...x, media_type:'tv', _src:'popular'})))
        .catch(() => [])
    );
    tmdbRequests.push(
      fetch(`https://api.themoviedb.org/3/tv/top_rated?api_key=${TMDB_API_KEY}&page=${p}`)
        .then(r => r.json()).then(d => (d.results || []).map(x => ({...x, media_type:'tv', _src:'top'})))
        .catch(() => [])
    );
  }
  for (let p = 1; p <= 3; p++) {
    tmdbRequests.push(
      fetch(`https://api.themoviedb.org/3/discover/tv?api_key=${TMDB_API_KEY}&with_genres=16&sort_by=popularity.desc&page=${p}`)
        .then(r => r.json()).then(d => (d.results || []).map(x => ({...x, media_type:'tv', is_anime_genre:true, _src:'anime_genre'})))
        .catch(() => [])
    );
  }
  tmdbRequests.push(
    fetch(`https://api.themoviedb.org/3/discover/movie?api_key=${TMDB_API_KEY}&with_genres=16&sort_by=popularity.desc&page=1`)
      .then(r => r.json()).then(d => (d.results || []).map(x => ({...x, media_type:'movie', is_anime_genre:true, _src:'anime_genre'})))
      .catch(() => [])
  );

  const results = await Promise.all(tmdbRequests);
  const seenIds = new Set();
  results.forEach(arr => {
    arr.forEach(item => {
      const key = item.media_type + '_' + item.id;
      if (!seenIds.has(key)) {
        seenIds.add(key);
        allTVItems.push(item);
      }
    });
  });

  try {
    const [topAnime, airingAnime, animeMovies, topOva, upcomingAnime] = await Promise.all([
      fetch('https://api.jikan.moe/v4/top/anime?limit=25&page=1').then(r => r.json()).catch(() => ({data:[]})),
      fetch('https://api.jikan.moe/v4/top/anime?filter=airing&limit=25').then(r => r.json()).catch(() => ({data:[]})),
      fetch('https://api.jikan.moe/v4/top/anime?type=movie&limit=20').then(r => r.json()).catch(() => ({data:[]})),
      fetch('https://api.jikan.moe/v4/top/anime?type=ova&limit=15').then(r => r.json()).catch(() => ({data:[]})),
      fetch('https://api.jikan.moe/v4/seasons/upcoming?limit=20').then(r => r.json()).catch(() => ({data:[]}))
    ]);
    [topAnime, airingAnime, animeMovies, topOva, upcomingAnime].forEach(pack => {
      (pack.data || []).forEach(a => {
        const key = 'anime_' + a.mal_id;
        if (!seenIds.has(key)) {
          seenIds.add(key);
          allTVItems.push({...a, media_type:'anime', _src:'jikan'});
        }
      });
    });
  } catch (e) { console.warn('Jikan anime load failed', e); }

  try { tvWatchlist = JSON.parse(localStorage.getItem('xorwel_watchlist') || '[]'); } catch(e) { tvWatchlist = []; }

  filteredTVItems = allTVItems.slice();
  filterTV();
  updateTvHero();
  toast(`Loaded ${allTVItems.length} titles`, 'success');
}

async function loadTvGenres() {
  try {
    const [mg, tg] = await Promise.all([
      fetch(`https://api.themoviedb.org/3/genre/movie/list?api_key=${TMDB_API_KEY}`).then(r => r.json()),
      fetch(`https://api.themoviedb.org/3/genre/tv/list?api_key=${TMDB_API_KEY}`).then(r => r.json())
    ]);
    tvGenresCache.movie = mg.genres || [];
    tvGenresCache.tv = tg.genres || [];
    const sel = document.getElementById('tvGenre');
    if (!sel) return;
    const combined = new Map();
    [...tvGenresCache.movie, ...tvGenresCache.tv].forEach(g => combined.set(g.id, g.name));
    const opts = ['<option value="">All genres</option>'];
    [...combined.entries()].sort((a,b) => a[1].localeCompare(b[1])).forEach(([id, name]) => {
      opts.push(`<option value="${id}">${name}</option>`);
    });
    sel.innerHTML = opts.join('');
  } catch (e) {}
}

async function loadTVMore() {
  const btn = document.getElementById('tvLoadMoreBtn');
  if (btn) { btn.disabled = true; btn.textContent = 'Loading…'; }
  const next = tvPageOffset + 1;
  const reqs = [];
  for (let p = tvPageOffset + 1; p <= next + 1; p++) {
    reqs.push(
      fetch(`https://api.themoviedb.org/3/movie/popular?api_key=${TMDB_API_KEY}&page=${p}`)
        .then(r => r.json()).then(d => (d.results || []).map(x => ({...x, media_type:'movie'}))).catch(() => [])
    );
    reqs.push(
      fetch(`https://api.themoviedb.org/3/tv/popular?api_key=${TMDB_API_KEY}&page=${p}`)
        .then(r => r.json()).then(d => (d.results || []).map(x => ({...x, media_type:'tv'}))).catch(() => [])
    );
  }
  const results = await Promise.all(reqs);
  const seen = new Set(allTVItems.map(i => (i.media_type||'') + '_' + (i.id || i.mal_id)));
  let added = 0;
  results.forEach(arr => arr.forEach(item => {
    const key = item.media_type + '_' + item.id;
    if (!seen.has(key)) { seen.add(key); allTVItems.push(item); added++; }
  }));
  tvPageOffset = next + 1;
  filterTV();
  if (btn) { btn.disabled = false; btn.textContent = 'Load more'; }
  toast(added ? `Added ${added} more titles` : 'No more results', added ? 'success' : 'info');
}

async function onTvGenreChange() {
  const gid = document.getElementById('tvGenre')?.value;
  if (!gid) { filterTV(); return; }
  toast('Loading genre…', 'info', 1200);
  try {
    const [movies, shows] = await Promise.all([
      fetch(`https://api.themoviedb.org/3/discover/movie?api_key=${TMDB_API_KEY}&with_genres=${gid}&sort_by=popularity.desc&page=1`).then(r => r.json()),
      fetch(`https://api.themoviedb.org/3/discover/tv?api_key=${TMDB_API_KEY}&with_genres=${gid}&sort_by=popularity.desc&page=1`).then(r => r.json())
    ]);
    const seen = new Set(allTVItems.map(i => (i.media_type||'') + '_' + i.id));
    [...(movies.results||[]).map(x => ({...x, media_type:'movie'})),
     ...(shows.results||[]).map(x => ({...x, media_type:'tv'}))].forEach(item => {
      const key = item.media_type + '_' + item.id;
      if (!seen.has(key)) { seen.add(key); allTVItems.unshift(item); }
    });
  } catch (e) {}
  filterTV();
}

function tvCategory(cat) {
  tvCurrentCategory = cat;
  document.querySelectorAll('#tvCategoryChips .tv-chip').forEach(b => {
    b.classList.toggle('active', b.getAttribute('data-cat') === cat);
  });
  const typeSel = document.getElementById('tvFilter');
  if (cat === 'anime' && typeSel) typeSel.value = 'anime';
  else if ((cat === 'top_movies' || cat === 'now_playing' || cat === 'upcoming') && typeSel) typeSel.value = 'movie';
  else if (cat === 'top_tv' && typeSel) typeSel.value = 'tv';
  else if (typeSel && cat === 'home') typeSel.value = 'all';
  filterTV();
  updateTvHero();
}

function updateTvHero() {
  const hero = document.getElementById('tvHero');
  if (!hero) return;
  const pool = filteredTVItems.filter(i => i.backdrop_path || i.poster_path);
  if (!pool.length) { hero.style.display = 'none'; return; }
  const item = pool[Math.floor(Math.random() * Math.min(12, pool.length))];
  const title = item.title || item.name || item.title_english || 'Featured';
  const overview = item.overview || '';
  const backdrop = item.backdrop_path ? `https://image.tmdb.org/t/p/w1280${item.backdrop_path}` :
    (item.poster_path ? `https://image.tmdb.org/t/p/w780${item.poster_path}` : '');
  const poster = item.poster_path ? `https://image.tmdb.org/t/p/w300${item.poster_path}` :
    (item.images?.jpg?.large_image_url || item.images?.jpg?.image_url || '');
  const type = item.media_type || 'movie';
  const id = item.id || item.mal_id || '';
  const rating = item.vote_average ? item.vote_average.toFixed(1) : (item.score ? Number(item.score).toFixed(1) : '');
  const safeTitle = title.replace(/'/g, "\\'");
  hero.style.display = 'block';
  hero.innerHTML = `
    <div class="tv-hero-bg" style="background-image:url('${backdrop}')"></div>
    <div class="tv-hero-content">
      ${poster ? `<img class="tv-hero-poster" src="${poster}" alt="">` : ''}
      <div class="tv-hero-text">
        <div style="font-size:0.72rem;color:var(--accent-bright);font-weight:700;letter-spacing:0.5px;text-transform:uppercase;margin-bottom:0.25rem;">Featured · ${type}${rating ? ' · ★ ' + rating : ''}</div>
        <h2>${escapeHtml(title)}</h2>
        <p>${escapeHtml(overview || 'Tap play to stream with automatic source failover.')}</p>
        <div class="tv-hero-actions">
          <button onclick="playTVItem('${type}','${id}','','${safeTitle}','${type==='anime'?id:''}')">▶ Play</button>
          <button class="secondary" onclick="openTvDetailById('${type}','${id}')">Details</button>
          <button class="secondary" onclick="toggleWatchlistFromHero('${type}','${id}','${safeTitle}')">★ Watchlist</button>
        </div>
      </div>
    </div>`;
}

function renderTV(list) {
  const grid = document.getElementById('tvGrid');
  const countBar = document.getElementById('tvCountBar');
  grid.innerHTML = '';
  if (countBar) countBar.textContent = list.length ? `${list.length} titles` : '';
  if (!list.length) {
    grid.innerHTML = '<div class="loading">No titles match. Try another filter or Load more.</div>';
    return;
  }
  const MAX = 240;
  const slice = list.slice(0, MAX);
  slice.forEach((item, i) => {
    let title = '', poster = '', id = '', type = '', imdbId = '', year = '', malId = '';
    if (item.media_type === 'movie') {
      title = item.title;
      poster = item.poster_path ? `https://image.tmdb.org/t/p/w500${item.poster_path}` : '';
      id = item.id; type = 'movie'; imdbId = item.imdb_id || '';
      year = item.release_date ? item.release_date.substring(0,4) : '';
    } else if (item.media_type === 'tv') {
      title = item.name;
      poster = item.poster_path ? `https://image.tmdb.org/t/p/w500${item.poster_path}` : '';
      id = item.id; type = 'tv'; imdbId = item.imdb_id || '';
      year = item.first_air_date ? item.first_air_date.substring(0,4) : '';
    } else if (item.media_type === 'anime') {
      title = item.title_english || item.title;
      poster = item.images?.jpg?.large_image_url || item.images?.jpg?.image_url || '';
      id = item.mal_id; malId = item.mal_id; type = 'anime'; imdbId = '';
      year = (item.year || (item.aired?.prop?.from?.year)) || '';
    }
    const card = document.createElement('div');
    card.className = 'card';
    card.style.animationDelay = Math.min(i * 3, 350) + 'ms';
    const typeLabel = type === 'movie' ? 'Movie' : type === 'anime' ? 'Anime' : 'TV';
    const rating = item.vote_average ? item.vote_average.toFixed(1) : (item.score ? Number(item.score).toFixed(1) : '');
    const wlKey = type + '_' + id;
    const onWl = tvWatchlist.some(w => w.key === wlKey);
    const safeTitle = (title || '').replace(/'/g, "\\'");
    card.innerHTML = `
      <div class="poster">
        ${poster ? `<img src="${poster}" loading="lazy" onload="this.classList.add('loaded')" onerror="this.style.display='none'">` : ''}
        <div class="skeleton"></div>
        <div class="fallback-icon" style="display:${poster ? 'none' : 'flex'};"><svg viewBox="0 0 24 24"><use href="#i-tv"/></svg></div>
        <span style="position:absolute;top:8px;left:8px;background:rgba(0,0,0,0.78);color:var(--accent-bright);font-size:0.62rem;font-weight:700;padding:3px 7px;border-radius:6px;letter-spacing:0.4px;text-transform:uppercase;z-index:2;border:1px solid rgba(220,20,60,0.3);">${typeLabel}</span>
        ${rating ? `<span style="position:absolute;top:8px;right:8px;background:rgba(0,0,0,0.78);color:#ffd166;font-size:0.68rem;font-weight:700;padding:3px 7px;border-radius:6px;z-index:2;">★ ${rating}</span>` : ''}
      </div>
      <h3>${escapeHtml(title)}</h3>
      <div class="meta">${year || '—'}${item._src === 'trending' ? ' · Trending' : ''}</div>
      <div class="card-actions">
        <button onclick="event.stopPropagation();playTVItem('${type}','${id}','${imdbId}','${safeTitle}','${malId}')">▶ Play</button>
        <button onclick="event.stopPropagation();openTvDetailById('${type}','${id}')">Info</button>
        <button class="wl-btn ${onWl ? 'on' : ''}" title="Watchlist" onclick="event.stopPropagation();toggleWatchlist('${type}','${id}','${safeTitle}','${poster.replace(/'/g,"\\'")}', this)">★</button>
      </div>
    `;
    card.addEventListener('click', () => openTvDetailById(type, id));
    grid.appendChild(card);
  });
  if (list.length > MAX && countBar) {
    countBar.textContent = `Showing ${MAX} of ${list.length} — refine filters or search`;
  }
}

async function playTVItem(type, id, imdbId, title, malId, presetSeason, presetEpisode) {
  let season = null, episode = null;
  if (type === 'tv' || type === 'anime') {
    if (presetSeason != null && presetEpisode != null) {
      season = String(presetSeason);
      episode = String(presetEpisode);
    } else {
      season = prompt('Season number:', '1');
      if (!season) return;
      episode = prompt('Episode number:', '1');
      if (!episode) return;
    }
    closeTvDetail();
  }
  let resolvedImdb = imdbId || '';
  if (!resolvedImdb && id && type !== 'anime') {
    try {
      toast('Resolving best sources...', 'info', 1500);
      const path = type === 'movie' ? 'movie' : 'tv';
      const res = await fetch(`https://api.themoviedb.org/3/${path}/${id}/external_ids?api_key=${TMDB_API_KEY}`);
      const ext = await res.json();
      if (ext && ext.imdb_id) resolvedImdb = ext.imdb_id;
    } catch (e) {}
  }
  let tmdbId = (type === 'anime') ? '' : id;
  let mal = malId || (type === 'anime' ? id : '');
  if (type === 'anime' && title) {
    try {
      const q = encodeURIComponent(title);
      const res = await fetch(`https://api.themoviedb.org/3/search/tv?api_key=${TMDB_API_KEY}&query=${q}`);
      const d = await res.json();
      if (d.results && d.results[0]) tmdbId = d.results[0].id;
    } catch (e) {}
  }
  const data = {
    tmdbId: tmdbId || id,
    imdbId: resolvedImdb,
    type,
    season,
    episode,
    title,
    malId: mal
  };
  currentPlayData = data;
  startSourceTest(data);
}

function filterTV() {
  const f = document.getElementById('tvFilter')?.value || 'all';
  const qGlobal = (document.getElementById('searchInput')?.value || '').toLowerCase().trim();
  const qLocal = (document.getElementById('tvSearchLocal')?.value || '').toLowerCase().trim();
  const q = qLocal || qGlobal;
  const sort = document.getElementById('tvSort')?.value || 'rating';
  const genreId = document.getElementById('tvGenre')?.value || '';
  const cat = tvCurrentCategory || 'home';

  let list = allTVItems.slice();

  if (cat === 'trending') list = list.filter(i => i._src === 'trending');
  else if (cat === 'now_playing') list = list.filter(i => i._src === 'now_playing' || i.media_type === 'movie');
  else if (cat === 'upcoming') list = list.filter(i => i._src === 'upcoming');
  else if (cat === 'top_movies') list = list.filter(i => i.media_type === 'movie');
  else if (cat === 'top_tv') list = list.filter(i => i.media_type === 'tv');
  else if (cat === 'anime') list = list.filter(i => i.media_type === 'anime' || i.is_anime_genre);

  if (f !== 'all') list = list.filter(i => i.media_type === f);

  if (genreId) {
    const gid = parseInt(genreId, 10);
    list = list.filter(i => Array.isArray(i.genre_ids) && i.genre_ids.includes(gid));
  }

  if (q) {
    list = list.filter(i => {
      const t = (i.title || i.name || i.title_english || i.title_japanese || '').toLowerCase();
      return t.includes(q);
    });
  }

  const yearOf = (i) => {
    const d = i.release_date || i.first_air_date || '';
    return parseInt((d || '').substring(0,4), 10) || (i.year || 0);
  };
  if (sort === 'rating') list.sort((a, b) => (b.vote_average || b.score || 0) - (a.vote_average || a.score || 0));
  else if (sort === 'popularity') list.sort((a, b) => (b.popularity || 0) - (a.popularity || 0));
  else if (sort === 'year_desc') list.sort((a, b) => yearOf(b) - yearOf(a));
  else if (sort === 'year_asc') list.sort((a, b) => yearOf(a) - yearOf(b));
  else if (sort === 'title') list.sort((a, b) => (a.title || a.name || '').localeCompare(b.title || b.name || ''));

  filteredTVItems = list;
  renderTV(filteredTVItems);
}

/* —— Watchlist —— */
function toggleWatchlist(type, id, title, poster, btn) {
  const key = type + '_' + id;
  const idx = tvWatchlist.findIndex(w => w.key === key);
  if (idx >= 0) {
    tvWatchlist.splice(idx, 1);
    if (btn) btn.classList.remove('on');
    toastAction('Removed from watchlist', 'info', [{ label: 'Undo', run: () => { tvWatchlist.unshift({ key, type, id, title, poster, ts: Date.now() }); try { localStorage.setItem('xorwel_watchlist', JSON.stringify(tvWatchlist)); } catch(e){} toast('Restored', 'success'); }}]);
  } else {
    tvWatchlist.unshift({ key, type, id, title, poster, ts: Date.now() });
    tvWatchlist = tvWatchlist.slice(0, 100);
    if (btn) btn.classList.add('on');
    toast('Added to watchlist', 'success');
  }
  try { localStorage.setItem('xorwel_watchlist', JSON.stringify(tvWatchlist)); } catch(e) {}
  pushRecentItem(title, 'tv', () => openTvDetailById(type, id));
}
function toggleWatchlistFromHero(type, id, title) {
  toggleWatchlist(type, id, title, '', null);
}
function showWatchlist() {
  if (!tvWatchlist.length) { toast('Watchlist is empty — star any title', 'info'); return; }
  const grid = document.getElementById('tvGrid');
  grid.innerHTML = '';
  document.getElementById('tvCountBar').textContent = `Watchlist · ${tvWatchlist.length}`;
  tvWatchlist.forEach((w, i) => {
    const card = document.createElement('div');
    card.className = 'card';
    const safe = (w.title || '').replace(/'/g, "\\'");
    card.innerHTML = `
      <div class="poster">
        ${w.poster ? `<img src="${w.poster}" loading="lazy" onload="this.classList.add('loaded')">` : ''}
        <div class="skeleton"></div>
        <div class="fallback-icon" style="display:${w.poster?'none':'flex'}"><svg viewBox="0 0 24 24"><use href="#i-tv"/></svg></div>
      </div>
      <h3>${escapeHtml(w.title)}</h3>
      <div class="meta">${w.type}</div>
      <div class="card-actions">
        <button onclick="playTVItem('${w.type}','${w.id}','','${safe}','${w.type==='anime'?w.id:''}')">▶ Play</button>
        <button class="wl-btn on" onclick="toggleWatchlist('${w.type}','${w.id}','${safe}','',this);showWatchlist()">★</button>
      </div>`;
    grid.appendChild(card);
  });
}

/* —— Enhanced Detail modal with tabs (Info / Episodes / Similar) —— */
async function openTvDetailById(type, id) {
  const modal = document.getElementById('tvDetailModal');
  const body = document.getElementById('tvDetailBody');
  if (!modal || !body) return;
  modal.style.display = 'flex';
  body.innerHTML = '<div class="loading"><div class="spinner"></div>Loading details…</div>';

  let item = allTVItems.find(i => String(i.id || i.mal_id) === String(id) && i.media_type === type);
  let details = item || {};
  let credits = null;
  let seasons = [];
  let similar = [];

  try {
    if (type === 'movie') {
      const [d, c, sim] = await Promise.all([
        fetch(`https://api.themoviedb.org/3/movie/${id}?api_key=${TMDB_API_KEY}`).then(r => r.json()),
        fetch(`https://api.themoviedb.org/3/movie/${id}/credits?api_key=${TMDB_API_KEY}`).then(r => r.json()).catch(() => null),
        fetch(`https://api.themoviedb.org/3/movie/${id}/similar?api_key=${TMDB_API_KEY}`).then(r => r.json()).catch(() => ({results:[]}))
      ]);
      details = { ...details, ...d, media_type: 'movie' };
      credits = c;
      similar = (sim.results || []).map(x => ({...x, media_type:'movie'}));
    } else if (type === 'tv') {
      const [d, c, sim] = await Promise.all([
        fetch(`https://api.themoviedb.org/3/tv/${id}?api_key=${TMDB_API_KEY}`).then(r => r.json()),
        fetch(`https://api.themoviedb.org/3/tv/${id}/credits?api_key=${TMDB_API_KEY}`).then(r => r.json()).catch(() => null),
        fetch(`https://api.themoviedb.org/3/tv/${id}/similar?api_key=${TMDB_API_KEY}`).then(r => r.json()).catch(() => ({results:[]}))
      ]);
      details = { ...details, ...d, media_type: 'tv' };
      credits = c;
      seasons = d.seasons || [];
      similar = (sim.results || []).map(x => ({...x, media_type:'tv'}));
    } else if (type === 'anime') {
      const d = await fetch(`https://api.jikan.moe/v4/anime/${id}/full`).then(r => r.json()).catch(() => null);
      if (d && d.data) details = { ...details, ...d.data, media_type: 'anime' };
    }
  } catch (e) {}

  const title = details.title || details.name || details.title_english || 'Title';
  const poster = details.poster_path
    ? `https://image.tmdb.org/t/p/w342${details.poster_path}`
    : (details.images?.jpg?.large_image_url || details.images?.jpg?.image_url || '');
  const overview = details.overview || details.synopsis || 'No overview available.';
  const rating = details.vote_average ? details.vote_average.toFixed(1) : (details.score ? Number(details.score).toFixed(1) : '—');
  const year = (details.release_date || details.first_air_date || '').substring(0,4) || details.year || '';
  const runtime = details.runtime ? details.runtime + ' min' : (details.episode_run_time && details.episode_run_time[0] ? details.episode_run_time[0] + ' min/ep' : '');
  const genres = (details.genres || []).map(g => g.name).join(' · ') || (details.genres?.map?.(g => g.name).join(' · ')) || '';
  const cast = (credits && credits.cast) ? credits.cast.slice(0, 10).map(c => c.name).join(', ') : '';
  const safeTitle = title.replace(/'/g, "\\'");
  const mal = type === 'anime' ? id : '';
  const trailer = details.videos?.results?.find(v => v.type === 'Trailer' && v.site === 'YouTube');

  const seasonsToShow = seasons.filter(s => s.season_number > 0);
  const maxSeasons = Math.min(Math.max(seasonsToShow.length || 1, 1), 20);

  const episodeTabUI = (type === 'tv' || type === 'anime') ? `
    <div class="tv-season-row" id="tvSeasonPick">
      ${Array.from({length: maxSeasons}, (_, i) => i+1).map(s =>
        `<button class="tv-chip ${s===1?'active':''}" data-s="${s}" onclick="selectSeason(${s})">Season ${s}</button>`
      ).join('')}
    </div>
    <div class="tv-ep-grid" id="tvEpGrid">
      ${Array.from({length: 24}, (_, i) => i+1).map(e =>
        `<button class="tv-ep-btn" onclick="playTVItem('${type}','${id}','','${safeTitle}','${mal}', document.querySelector('#tvSeasonPick .active')?.getAttribute('data-s')||1, ${e})">E${e}</button>`
      ).join('')}
    </div>
    <p style="font-size:0.75rem;color:var(--text-dim);margin-top:0.6rem;">Select a season, then episode · auto-failover picks a working source.</p>
  ` : '';

  const similarUI = similar.length ? `
    <div class="tv-similar-grid">
      ${similar.slice(0, 18).map(s => {
        const st = s.title || s.name || 'Untitled';
        const sp = s.poster_path ? `https://image.tmdb.org/t/p/w200${s.poster_path}` : '';
        return `<div class="tv-similar-item" onclick="closeTvDetail();openTvDetailById('${s.media_type}','${s.id}')">
          ${sp ? `<img src="${sp}" loading="lazy" alt="">` : '<div style="aspect-ratio:2/3;background:#111;border-radius:8px;"></div>'}
          <div class="tv-similar-title">${escapeHtml(st)}</div>
        </div>`;
      }).join('')}
    </div>
  ` : '<p style="color:var(--text-dim);font-size:0.85rem;">No similar titles found.</p>';

  body.innerHTML = `
    <div class="tv-detail-header">
      ${poster ? `<img src="${poster}" alt="">` : ''}
      <div style="flex:1;min-width:0;">
        <div style="font-size:0.72rem;color:var(--accent-bright);font-weight:700;text-transform:uppercase;">${type} ${year ? '· '+year : ''} ${runtime ? '· '+runtime : ''}</div>
        <h2 style="margin:0.25rem 0 0.4rem;font-size:1.35rem;">${escapeHtml(title)}</h2>
        <div style="color:#ffd166;font-weight:700;margin-bottom:0.5rem;">★ ${rating}${genres ? ' · ' + escapeHtml(genres) : ''}</div>
        <div style="display:flex;gap:0.4rem;flex-wrap:wrap;">
          <button class="tv-chip active" onclick="playTVItem('${type}','${id}','','${safeTitle}','${mal}')">▶ Play${type==='movie'?'':' S1E1'}</button>
          ${trailer ? `<button class="tv-chip" onclick="window.open('https://www.youtube.com/watch?v=${trailer.key}','_blank')">▶ Trailer</button>` : ''}
          <button class="tv-chip" onclick="toggleWatchlist('${type}','${id}','${safeTitle}','${poster.replace(/'/g,"\\'")}',null)">★ Watchlist</button>
          <button class="tv-chip" onclick="shareTvItem('${type}','${id}','${safeTitle}')">Share</button>
        </div>
      </div>
    </div>
    <div class="tv-detail-tabs">
      <button class="tv-detail-tab active" data-tvtab="info" onclick="switchTvTab('info')">Overview</button>
      ${(type === 'tv' || type === 'anime') ? `<button class="tv-detail-tab" data-tvtab="episodes" onclick="switchTvTab('episodes')">Episodes</button>` : ''}
      ${similar.length ? `<button class="tv-detail-tab" data-tvtab="similar" onclick="switchTvTab('similar')">Similar</button>` : ''}
      ${cast ? `<button class="tv-detail-tab" data-tvtab="cast" onclick="switchTvTab('cast')">Cast</button>` : ''}
    </div>
    <div class="tv-tab-panel" data-tvpanel="info">
      <p style="font-size:0.9rem;line-height:1.55;color:var(--text-dim);">${escapeHtml(overview)}</p>
    </div>
    ${(type === 'tv' || type === 'anime') ? `<div class="tv-tab-panel" data-tvpanel="episodes" style="display:none;">${episodeTabUI}</div>` : ''}
    ${similar.length ? `<div class="tv-tab-panel" data-tvpanel="similar" style="display:none;">${similarUI}</div>` : ''}
    ${cast ? `<div class="tv-tab-panel" data-tvpanel="cast" style="display:none;"><p style="font-size:0.9rem;line-height:1.6;color:var(--text-dim);">${escapeHtml(cast)}</p></div>` : ''}
  `;
  pushRecentItem(title, type, () => openTvDetailById(type, id));
}

function switchTvTab(name) {
  document.querySelectorAll('.tv-detail-tab').forEach(t => t.classList.toggle('active', t.dataset.tvtab === name));
  document.querySelectorAll('.tv-tab-panel').forEach(p => p.style.display = p.dataset.tvpanel === name ? '' : 'none');
}

function shareTvItem(type, id, title) {
  const url = location.origin + location.pathname + '#' + type + '/' + id;
  if (navigator.share) {
    navigator.share({ title, url }).catch(() => {});
  } else {
    navigator.clipboard?.writeText(url).then(() => toast('Link copied', 'success')).catch(() => toast(url, 'info'));
  }
}

function selectSeason(n) {
  document.querySelectorAll('#tvSeasonPick .tv-chip').forEach(b => {
    b.classList.toggle('active', b.getAttribute('data-s') === String(n));
  });
}
function closeTvDetail() {
  const m = document.getElementById('tvDetailModal');
  if (m) m.style.display = 'none';
}



/* ============================================================
   SOURCE TESTING — enhanced with parallel probe
   ============================================================ */
function getTitleKey(data) {
  return `${data.type}_${data.tmdbId || data.imdbId || data.malId}_${data.season || 0}_${data.episode || 0}`;
}
function getWorkingSources(key) {
  try { const cache = JSON.parse(localStorage.getItem('xorwel_source_cache') || '{}'); return cache[key] || []; }
  catch (e) { return []; }
}
function setWorkingSources(key, sourceName) {
  try {
    const cache = JSON.parse(localStorage.getItem('xorwel_source_cache') || '{}');
    const arr = cache[key] || [];
    if (!arr.includes(sourceName)) arr.unshift(sourceName);
    cache[key] = arr.slice(0, 5);
    localStorage.setItem('xorwel_source_cache', JSON.stringify(cache));
  } catch (e) {}
}

function startSourceTest(data) {
  sourceTestActive = true;
  testCancelled = false;
  currentTestSourceIndex = 0;

  const isAnime = data.type === 'anime';
  let sourcePool = isAnime
    ? ANIME_SOURCES.concat(EMBED_SOURCES.filter(s => s.type === 'tmdb' || s.tier === 1))
    : EMBED_SOURCES.slice();

  sourcePool = sourcePool.slice().sort((a, b) => (a.tier || 9) - (b.tier || 9));

  const titleKey = getTitleKey(data);
  const knownWorking = getWorkingSources(titleKey);

  let order = [];
  knownWorking.forEach(name => {
    const src = sourcePool.find(s => s.name === name);
    if (src && !order.includes(src)) order.push(src);
  });
  if (preferredSource) {
    const pref = sourcePool.find(s => s.name === preferredSource);
    if (pref && !order.includes(pref)) order.push(pref);
  }
  sourcePool.forEach(s => {
    if (!order.includes(s)) order.push(s);
  });
  currentTestOrder = order;

  const hud = document.getElementById('sourceTestHUD');
  hud.classList.add('show');
  document.getElementById('hudTitle').textContent = 'Finding working source...';
  document.getElementById('hudSub').textContent = `Testing ${order.length} providers for "${data.title}"`;
  document.getElementById('hudProgressFill').style.width = '0%';

  const chipContainer = document.getElementById('hudSourceList');
  chipContainer.innerHTML = order.map((s, i) =>
    `<span class="hud-source-chip" data-idx="${i}">${s.name}</span>`
  ).join('');

  testNextSource();
}

function getSourceUrl(source, data) {
  const name = (source.name || '').toLowerCase();
  const tmdb = data.tmdbId;
  const imdb = data.imdbId;
  const mal = data.malId;
  const season = data.season || 1;
  const episode = data.episode || 1;
  const isMovie = data.type === 'movie';
  const isAnime = data.type === 'anime';

  if (isAnime) {
    if (name === 'dropfile-mal' && mal) return source.base + mal + '/' + season + '/' + episode;
    if (name === 'dropfile-tmdb' && tmdb) return source.base + tmdb + '/' + season + '/' + episode;
    if (name === 'vidnest-anime' && mal) return source.base + mal + '/' + season + '/' + episode;
    if (name === 'vibeplayer' && mal) return source.base + mal;
    if (name.includes('ezvidapi') && tmdb) return source.base + 'tv/' + tmdb + '/' + season + '/' + episode;
    if (name.includes('filmu') && tmdb) return source.base + 'tv/' + tmdb + '/' + season + '/' + episode;
    if (name.includes('vidfast') && tmdb) return source.base + 'tv/' + tmdb + '/' + season + '/' + episode;
  }

  let id = source.type === 'imdb' ? (imdb || tmdb) : (tmdb || imdb);
  if (!id) id = encodeURIComponent(data.title || '');

  if (name === 'ezvidapi' || name === 'ezvidapi-anime') {
    return isMovie ? 'https://ezvidapi.com/embed/movie/' + (tmdb || id) : 'https://ezvidapi.com/embed/tv/' + (tmdb || id) + '/' + season + '/' + episode;
  }
  if (name === 'filmu' || name === 'filmu-anime') {
    return isMovie ? 'https://embed.filmu.in/movie/' + (tmdb || id) : 'https://embed.filmu.in/tv/' + (tmdb || id) + '/' + season + '/' + episode;
  }
  if (name === 'vidrift') {
    return isMovie ? 'https://embed.vidrift.in/embed/movie/' + (tmdb || id) : 'https://embed.vidrift.in/embed/tv/' + (tmdb || id) + '/' + season + '/' + episode;
  }
  if (name === 'vidspark') {
    return isMovie ? 'https://vidspark.to/movie/' + (tmdb || id) : 'https://vidspark.to/tv/' + (tmdb || id) + '/' + season + '/' + episode;
  }
  if (name === 'vidnest') {
    return isMovie ? 'https://vidnest.fun/movie/' + (tmdb || id) : 'https://vidnest.fun/tv/' + (tmdb || id) + '/' + season + '/' + episode;
  }
  if (name === 'moviesapi.to') {
    return isMovie ? 'https://moviesapi.to/movie/' + (tmdb || id) : 'https://moviesapi.to/tv/' + (tmdb || id) + '-' + season + '-' + episode;
  }
  if (name === 'vidfast' || name === 'vidfast-anime') {
    return isMovie ? 'https://vidfast.pro/movie/' + (tmdb || id) : 'https://vidfast.pro/tv/' + (tmdb || id) + '/' + season + '/' + episode;
  }
  if (name === 'vidlink.pro') {
    return isMovie ? 'https://vidlink.pro/movie/' + (tmdb || id) : 'https://vidlink.pro/tv/' + (tmdb || id) + '/' + season + '/' + episode;
  }
  if (name === '2embed.cc') {
    return isMovie ? 'https://www.2embed.cc/embed/' + (imdb || id) : 'https://www.2embed.cc/embedtv/' + (imdb || id) + '?s=' + season + '&e=' + episode;
  }
  if (name === '2embed.skin') {
    return isMovie ? 'https://www.2embed.skin/embed/' + (imdb || id) : 'https://www.2embed.skin/embedtv/' + (imdb || id) + '?s=' + season + '&e=' + episode;
  }
  if (name === 'vidsrc.wtf') {
    return isMovie ? 'https://vidsrc.wtf/api/1/movie/' + (tmdb || id) : 'https://vidsrc.wtf/api/1/tv/' + (tmdb || id) + '/' + season + '/' + episode;
  }
  if (name === 'smashystream') {
    return isMovie ? 'https://player.smashy.stream/movie/' + (tmdb || id) : 'https://player.smashy.stream/tv/' + (tmdb || id) + '?s=' + season + '&e=' + episode;
  }
  if (name === 'nontongo') {
    return isMovie ? 'https://nontongo.win/embed/movie/' + (tmdb || id) : 'https://nontongo.win/embed/tv/' + (tmdb || id) + '/' + season + '/' + episode;
  }
  if (name === 'autoembed.co') {
    return isMovie ? 'https://autoembed.co/movie/tmdb/' + (tmdb || id) : 'https://autoembed.co/tv/tmdb/' + (tmdb || id) + '/' + season + '/' + episode;
  }
  if (name === '111movies') {
    return isMovie ? 'https://111movies.com/movie/' + (tmdb || id) : 'https://111movies.com/tv/' + (tmdb || id) + '/' + season + '/' + episode;
  }
  if (name === 'multiembed' || name === 'superembed') {
    const vid = imdb || id;
    return isMovie ? source.base + vid : source.base + vid + '&s=' + season + '&e=' + episode;
  }
  if (name === 'frembed') {
    return source.base + (imdb || id);
  }
  if (source.base.includes('vidsrc') || source.base.includes('vid-src')) {
    return isMovie ? source.base + 'movie/' + id : source.base + 'tv/' + id + '/' + season + '/' + episode;
  }
  if (isMovie) return source.base + 'movie/' + id;
  return source.base + 'tv/' + id + '/' + season + '/' + episode;
}

function testNextSource() {
  if (testCancelled || !sourceTestActive) return;
  if (currentTestSourceIndex >= currentTestOrder.length) { finishSourceTest(); return; }
  const source = currentTestOrder[currentTestSourceIndex];
  const url = getSourceUrl(source, currentPlayData);
  const testFrame = document.getElementById('testFrame');

  document.querySelectorAll('#hudSourceList .hud-source-chip').forEach((chip, i) => {
    chip.classList.remove('testing');
    if (i === currentTestSourceIndex) chip.classList.add('testing');
  });

  const pct = (currentTestSourceIndex / currentTestOrder.length) * 100;
  document.getElementById('hudProgressFill').style.width = pct + '%';
  document.getElementById('hudSub').textContent = `Testing ${source.name} (${currentTestSourceIndex + 1} of ${currentTestOrder.length})`;

  testFrame.onload = null;
  testFrame.onerror = null;
  testFrame.src = 'about:blank';
  if (testTimeoutHandle) clearTimeout(testTimeoutHandle);
  let failureReason = null;

  testFrame.onerror = () => {
    failureReason = 'load-error';
    clearTimeout(testTimeoutHandle);
    handleSourceFailure(source);
  };
  testFrame.onload = () => {};

  testTimeoutHandle = setTimeout(() => {
    if (failureReason) return;
    const curSrc = testFrame.src;
    if (curSrc && curSrc !== 'about:blank' && !curSrc.startsWith('about:')) {
      handleSourceSuccess(source, url);
    } else {
      handleSourceFailure(source);
    }
  }, sourceTestTimeout * 1000);

  testFrame.src = url;
}

function handleSourceSuccess(source, url) {
  if (testCancelled || !sourceTestActive) return;
  sourceTestActive = false;
  if (testTimeoutHandle) clearTimeout(testTimeoutHandle);
  setWorkingSources(getTitleKey(currentPlayData), source.name);
  if (currentPlayData) saveContinueWatching(currentPlayData, source.name, url);

  document.querySelectorAll('#hudSourceList .hud-source-chip').forEach((chip, i) => {
    chip.classList.remove('testing');
    if (i === currentTestSourceIndex) chip.classList.add('success');
    else if (i < currentTestSourceIndex) chip.classList.add('failed');
  });
  document.getElementById('hudProgressFill').style.width = '100%';
  document.getElementById('hudTitle').textContent = 'Source found!';
  document.getElementById('hudSub').textContent = `Playing via ${source.name}`;

  setTimeout(() => {
    document.getElementById('sourceTestHUD').classList.remove('show');
    document.getElementById('playerFrame').src = url;
    document.getElementById('playerModal').classList.add('show');
    document.getElementById('currentSourceLabel').textContent = 'Source: ' + source.name;
    toast('Playing via ' + source.name, 'success');
    setData('playtime', getData('playtime', 0) + 1);
  }, 700);
}

function handleSourceFailure(source) {
  if (testCancelled) return;
  const chip = document.querySelector(`#hudSourceList .hud-source-chip[data-idx="${currentTestSourceIndex}"]`);
  if (chip) { chip.classList.remove('testing'); chip.classList.add('failed'); }
  currentTestSourceIndex++;
  setTimeout(testNextSource, 120);
}

function finishSourceTest() {
  sourceTestActive = false;
  if (testTimeoutHandle) clearTimeout(testTimeoutHandle);
  document.querySelectorAll('#hudSourceList .hud-source-chip').forEach(chip => {
    chip.classList.remove('testing'); chip.classList.add('failed');
  });
  document.getElementById('hudTitle').textContent = 'No working source found';
  document.getElementById('hudSub').textContent = 'All sources failed. Try a different title.';
  document.getElementById('hudProgressFill').style.width = '100%';
  setTimeout(() => {
    document.getElementById('sourceTestHUD').classList.remove('show');
    toastAction('No working source for this title', 'error', [
      { label: 'Retry all', run: () => tryAllSourcesAgain() },
      { label: 'Pick another', run: () => switchTab('tv') }
    ], 6000);
  }, 1500);
}

function cancelSourceTest() {
  testCancelled = true; sourceTestActive = false;
  if (testTimeoutHandle) clearTimeout(testTimeoutHandle);
  document.getElementById('testFrame').src = 'about:blank';
  document.getElementById('sourceTestHUD').classList.remove('show');
  toast('Cancelled', 'info');
}

function skipCurrentSource() {
  if (sourceTestActive) {
    if (testTimeoutHandle) clearTimeout(testTimeoutHandle);
    handleSourceFailure(currentTestOrder[currentTestSourceIndex]);
  } else if (document.getElementById('playerModal').classList.contains('show')) {
    const currentName = document.getElementById('currentSourceLabel').textContent.replace('Source: ', '');
    closePlayer();
    const idx = currentTestOrder.findIndex(s => s.name === currentName);
    currentTestSourceIndex = idx >= 0 ? idx + 1 : 0;
    sourceTestActive = true; testCancelled = false;
    document.getElementById('sourceTestHUD').classList.add('show');
    testNextSource();
  }
}

function tryAllSourcesAgain() {
  if (!currentPlayData) return;
  closePlayer();
  startSourceTest(currentPlayData);
}

/* ============================================================
   PLAYER — with PiP + Watch Party
   ============================================================ */
function openPlayer(url) {
  document.getElementById('playerFrame').src = url;
  document.getElementById('playerModal').classList.add('show');
  document.getElementById('currentSourceLabel').textContent = '';
}
function closePlayer() {
  document.getElementById('playerModal').classList.remove('show');
  document.getElementById('playerFrame').src = '';
}
function changeQuality() {
  currentQuality = document.getElementById('qualitySelect').value;
  try {
    const win = document.getElementById('playerFrame').contentWindow;
    win.document.querySelectorAll('video').forEach(v => {
      v.querySelectorAll('source').forEach(s => {
        if (s.dataset.quality === currentQuality) {
          const t = v.currentTime; v.src = s.src; v.currentTime = t; v.play();
        }
      });
    });
  } catch(e){}
  toast('Quality: ' + currentQuality + 'p', 'info');
}
function changeSpeed() {
  currentSpeed = document.getElementById('speedSelect').value;
  try {
    const win = document.getElementById('playerFrame').contentWindow;
    win.document.querySelectorAll('video').forEach(v => v.playbackRate = parseFloat(currentSpeed));
  } catch(e){}
  toast('Speed: ' + currentSpeed + 'x', 'info');
}
function toggleCaptions() {
  ccEnabled = !ccEnabled;
  document.getElementById('captionBtn').classList.toggle('active', ccEnabled);
  try {
    const win = document.getElementById('playerFrame').contentWindow;
    win.document.querySelectorAll('video').forEach(v => {
      const tracks = v.textTracks;
      for (let i = 0; i < tracks.length; i++) tracks[i].mode = ccEnabled ? 'showing' : 'hidden';
    });
  } catch(e){}
  toast('Captions ' + (ccEnabled ? 'ON' : 'OFF'), 'info');
}

/* PiP — works on videos inside the iframe if same-origin, otherwise on the top-level player */
async function togglePlayerPiP() {
  const frame = document.getElementById('playerFrame');
  try {
    const videos = frame.contentWindow?.document?.querySelectorAll('video');
    if (videos && videos.length) {
      const v = videos[0];
      if (document.pictureInPictureElement) {
        await document.exitPictureInPicture();
        toast('PiP off', 'info');
      } else {
        await v.requestPictureInPicture();
        toast('PiP on — drag the mini window', 'success');
      }
      return;
    }
  } catch (e) {}
  toast('PiP unavailable for this stream (cross-origin)', 'warning');
}

/* Watch Party — sync via BroadcastChannel (same-origin tabs) */
let watchPartyChannel = null;
let watchPartyActive = false;
let watchPartyMembers = 1;

function startWatchParty() {
  if (watchPartyActive) { endWatchParty(); return; }
  try {
    watchPartyChannel = new BroadcastChannel('xorwel-watch-party');
    watchPartyChannel.onmessage = (ev) => {
      if (!ev.data) return;
      if (ev.data.type === 'ping') {
        watchPartyMembers++;
        updateWatchPartyBar();
        watchPartyChannel.postMessage({ type: 'pong', from: 'self' });
      } else if (ev.data.type === 'pong') {
        watchPartyMembers++;
        updateWatchPartyBar();
      } else if (ev.data.type === 'navigate' && ev.data.url) {
        openPlayer(ev.data.url);
        toast('Host changed the video', 'info');
      }
    };
    watchPartyChannel.postMessage({ type: 'ping' });
    watchPartyActive = true;
    document.getElementById('watchPartyBar').classList.add('show');
    updateWatchPartyBar();
    toast('Watch party started — open this tab in another window', 'success');
  } catch (e) {
    toast('Watch party unavailable', 'error');
  }
}
function updateWatchPartyBar() {
  const el = document.getElementById('wpMemberCount');
  if (el) el.textContent = watchPartyMembers + (watchPartyMembers === 1 ? ' member' : ' members');
}
function endWatchParty() {
  watchPartyActive = false;
  try { watchPartyChannel?.close(); } catch(e){}
  watchPartyChannel = null;
  document.getElementById('watchPartyBar').classList.remove('show');
  toast('Watch party ended', 'info');
}
function syncWatchParty(url) {
  if (watchPartyActive && watchPartyChannel) {
    watchPartyChannel.postMessage({ type: 'navigate', url });
  }
}

/* ============================================================
   PROXY — with multi-tab
   ============================================================ */
const PUBLIC_PROXY_GATEWAYS = [
  { name: 'CorsProxy', encode: (u) => 'https://corsproxy.io/?url=' + encodeURIComponent(u) },
  { name: 'AllOrigins', encode: (u) => 'https://api.allorigins.win/raw?url=' + encodeURIComponent(u) },
  { name: 'CodeTabs', encode: (u) => 'https://api.codetabs.com/v1/proxy?quest=' + encodeURIComponent(u) },
  { name: 'ThingProxy', encode: (u) => 'https://thingproxy.freeboard.io/fetch/' + u },
  { name: 'YaCDN', encode: (u) => 'https://yacdn.org/proxy/' + u }
];

let publicGatewayIndex = 0;
let publicReady = true;

async function initProxy() {
  const dot = document.getElementById('proxyDot');
  const statusText = document.getElementById('proxyStatusText');
  let messages = [];

  messages.push('Public proxy ready');
  publicReady = true;

  if ('serviceWorker' in navigator && typeof __uv$config !== 'undefined') {
    try {
      await navigator.serviceWorker.register('/uv/sw.js', { scope: __uv$config.prefix });
      uvReady = true;
      messages.push('Ultraviolet ready');
    } catch (e) {
      messages.push('UV needs server');
    }
  } else {
    messages.push('UV needs server');
  }

  try {
    if (window.$scramjetLoadWorker) {
      sjReady = true;
      messages.push('Scramjet ready');
    } else {
      messages.push('SJ needs server');
    }
  } catch(e) {
    messages.push('SJ needs server');
  }

  if (!uvReady && !sjReady) {
    activeEngine = 'public';
    primaryEngine = 'public';
  }

  if (publicReady || uvReady || sjReady) {
    dot.classList.remove('err');
    dot.classList.add('ok');
  } else {
    dot.classList.add('err');
  }
  statusText.textContent = messages.join(' · ') + ' · Engine: ' + (activeEngine === 'public' ? 'Public' : activeEngine === 'uv' ? 'Ultraviolet' : 'Scramjet');
  updateEngineUI();
}

function updateEngineUI() {
  const uvBtn = document.getElementById('engineUV');
  const sjBtn = document.getElementById('engineSJ');
  const pubBtn = document.getElementById('enginePublic');
  if (uvBtn) uvBtn.classList.toggle('active', activeEngine === 'uv');
  if (sjBtn) sjBtn.classList.toggle('active', activeEngine === 'sj');
  if (pubBtn) pubBtn.classList.toggle('active', activeEngine === 'public');
}

function setEngine(engine) {
  activeEngine = engine;
  updateEngineUI();
  const labels = { uv: 'Ultraviolet', sj: 'Scramjet', public: 'Public' };
  toast('Engine: ' + (labels[engine] || engine), 'info');
  const statusText = document.getElementById('proxyStatusText');
  if (statusText) {
    const base = statusText.textContent.split(' · Engine:')[0];
    statusText.textContent = base + ' · Engine: ' + (labels[engine] || engine);
  }
}

function normalizeProxyUrl(input) {
  let url = (input || '').trim();
  if (!url) return '';
  if (!/^https?:\/\//i.test(url) && !url.includes('.') && !url.startsWith('//')) {
    return 'https://duckduckgo.com/?q=' + encodeURIComponent(url);
  }
  if (url.startsWith('//')) url = 'https:' + url;
  if (!/^https?:\/\//i.test(url)) url = 'https://' + url;
  try {
    new URL(url);
    return url;
  } catch (e) {
    return 'https://duckduckgo.com/?q=' + encodeURIComponent(input);
  }
}

function encodeProxyUrl(url) {
  if (activeEngine === 'public') {
    const gw = PUBLIC_PROXY_GATEWAYS[publicGatewayIndex % PUBLIC_PROXY_GATEWAYS.length];
    return gw.encode(url);
  }
  if (activeEngine === 'sj' && window.$scramjetLoadWorker) {
    try { return __uv$config.prefix + __uv$config.encodeUrl(url); } catch (e) {}
  }
  if (activeEngine === 'uv' && typeof __uv$config !== 'undefined') {
    try { return __uv$config.prefix + __uv$config.encodeUrl(url); } catch (e) {}
  }
  const gw = PUBLIC_PROXY_GATEWAYS[publicGatewayIndex % PUBLIC_PROXY_GATEWAYS.length];
  return gw.encode(url);
}

/* Enhanced proxy with multi-tab */
let proxyTabs = [{ title: 'Home', url: '', encoded: '' }];
let activeProxyTab = 0;

function newProxyTab() {
  proxyTabs.push({ title: 'New tab', url: '', encoded: '' });
  activeProxyTab = proxyTabs.length - 1;
  renderProxyTabs();
  document.getElementById('proxyUrl').value = '';
  document.getElementById('proxyFrame').src = 'about:blank';
  document.getElementById('proxyUrl').focus();
  toast('New tab', 'info', 1200);
}
function switchProxyTab(i) {
  if (i < 0 || i >= proxyTabs.length) return;
  activeProxyTab = i;
  const tab = proxyTabs[i];
  document.getElementById('proxyUrl').value = tab.url || '';
  if (tab.encoded) {
    document.getElementById('proxyFrameContainer').classList.add('show');
    document.getElementById('proxyFrame').src = tab.encoded;
  } else {
    document.getElementById('proxyFrameContainer').classList.remove('show');
    document.getElementById('proxyFrame').src = 'about:blank';
  }
  renderProxyTabs();
}
function closeProxyTab(i, e) {
  e && e.stopPropagation();
  if (proxyTabs.length === 1) { proxyTabs[0] = { title: 'Home', url: '', encoded: '' }; activeProxyTab = 0; }
  else {
    proxyTabs.splice(i, 1);
    if (activeProxyTab >= proxyTabs.length) activeProxyTab = proxyTabs.length - 1;
  }
  renderProxyTabs();
  switchProxyTab(activeProxyTab);
}
function renderProxyTabs() {
  const c = document.getElementById('proxyTabs');
  if (!c) return;
  c.innerHTML = '';
  proxyTabs.forEach((t, i) => {
    const btn = document.createElement('button');
    btn.className = 'proxy-tab' + (i === activeProxyTab ? ' active' : '');
    btn.innerHTML = `<span class="pt-title">${escapeHtml(t.title)}</span>${proxyTabs.length > 1 ? '<span class="pt-close">×</span>' : ''}`;
    btn.onclick = () => switchProxyTab(i);
    if (proxyTabs.length > 1) btn.querySelector('.pt-close').onclick = (e) => closeProxyTab(i, e);
    c.appendChild(btn);
  });
  const add = document.createElement('button');
  add.className = 'proxy-tab-add';
  add.textContent = '+';
  add.title = 'New tab';
  add.onclick = newProxyTab;
  c.appendChild(add);
}
function updateProxyTabTitle(url) {
  try {
    const u = new URL(url);
    proxyTabs[activeProxyTab].title = u.hostname.replace(/^www\./, '') || 'Tab';
  } catch (e) {
    proxyTabs[activeProxyTab].title = url.slice(0, 20) || 'Tab';
  }
  renderProxyTabs();
}

function proxyGo() {
  const input = document.getElementById('proxyUrl').value.trim();
  if (!input) return;
  const url = normalizeProxyUrl(input);
  document.getElementById('proxyUrl').value = url;

  try {
    const encoded = encodeProxyUrl(url);
    const container = document.getElementById('proxyFrameContainer');
    const frame = document.getElementById('proxyFrame');
    container.classList.add('show');

    frame.src = 'about:blank';
    setTimeout(() => {
      frame.src = encoded;
    }, 30);

    currentProxyUrl = url;
    if (!proxyIncognito) {
      proxyHistory = proxyHistory.slice(0, proxyHistoryIndex + 1);
      proxyHistory.push(url);
      proxyHistoryIndex = proxyHistory.length - 1;
    }

    // Update current tab
    proxyTabs[activeProxyTab] = { title: proxyTabs[activeProxyTab].title, url, encoded };
    updateProxyTabTitle(url);

    const gwName = activeEngine === 'public'
      ? PUBLIC_PROXY_GATEWAYS[publicGatewayIndex % PUBLIC_PROXY_GATEWAYS.length].name
      : (activeEngine === 'uv' ? 'Ultraviolet' : 'Scramjet');
    toast('Loading via ' + gwName + ': ' + url.replace(/^https?:\/\//, ''), 'info');
  } catch (e) {
    toast('Proxy error: ' + e.message, 'error');
  }
}

function cyclePublicGateway() {
  publicGatewayIndex = (publicGatewayIndex + 1) % PUBLIC_PROXY_GATEWAYS.length;
  const name = PUBLIC_PROXY_GATEWAYS[publicGatewayIndex].name;
  toast('Public gateway: ' + name, 'info');
  if (currentProxyUrl && activeEngine === 'public') {
    const enc = encodeProxyUrl(currentProxyUrl);
    document.getElementById('proxyFrame').src = enc;
    proxyTabs[activeProxyTab].encoded = enc;
  }
}

function proxyGoTo(url) {
  document.getElementById('proxyUrl').value = url;
  proxyGo();
}
function proxyBack() {
  if (proxyHistoryIndex > 0) {
    proxyHistoryIndex--;
    const url = proxyHistory[proxyHistoryIndex];
    document.getElementById('proxyUrl').value = url;
    document.getElementById('proxyFrame').src = encodeProxyUrl(url);
    currentProxyUrl = url;
  }
}
function proxyForward() {
  if (proxyHistoryIndex < proxyHistory.length - 1) {
    proxyHistoryIndex++;
    const url = proxyHistory[proxyHistoryIndex];
    document.getElementById('proxyUrl').value = url;
    document.getElementById('proxyFrame').src = encodeProxyUrl(url);
    currentProxyUrl = url;
  }
}
function proxyReload() {
  if (currentProxyUrl) {
    document.getElementById('proxyFrame').src = encodeProxyUrl(currentProxyUrl);
  }
}
function proxyHome() {
  document.getElementById('proxyFrameContainer').classList.remove('show');
  document.getElementById('proxyFrame').src = 'about:blank';
  currentProxyUrl = '';
  proxyHistory = [];
  proxyHistoryIndex = -1;
  proxyTabs[activeProxyTab] = { title: 'Home', url: '', encoded: '' };
  renderProxyTabs();
}
function toggleIncognito() {
  proxyIncognito = !proxyIncognito;
  document.getElementById('incognitoBtn').classList.toggle('active', proxyIncognito);
  toast('Incognito ' + (proxyIncognito ? 'ON' : 'OFF'), 'info');
}

/* ============================================================
   BOOKMARKS
   ============================================================ */
function renderBookmarks() {
  const list = document.getElementById('bookmarkList');
  if (!list) return;
  let bookmarks = getData('bookmarks', []);
  const q = (document.getElementById('bmSearch')?.value || '').toLowerCase().trim();
  if (q) {
    bookmarks = bookmarks.filter(b =>
      (b.title || '').toLowerCase().includes(q) ||
      (b.url || '').toLowerCase().includes(q) ||
      (b.tag || '').toLowerCase().includes(q)
    );
  }
  if (!bookmarks.length) {
    list.innerHTML = '<div class="loading">No bookmarks yet. Add one above or from the proxy.</div>';
    return;
  }
  list.innerHTML = bookmarks.map((bm) => {
    const all = getData('bookmarks', []);
    const realIdx = all.findIndex(x => x.url === bm.url && x.title === bm.title);
    let host = '';
    try { host = new URL(bm.url).hostname; } catch (e) { host = bm.url; }
    const fav = 'https://www.google.com/s2/favicons?domain=' + encodeURIComponent(host) + '&sz=32';
    return `<div style="display:flex;align-items:center;gap:0.75rem;padding:0.65rem 0.75rem;border-bottom:1px solid var(--border);">
      <img src="${fav}" width="20" height="20" style="border-radius:4px;flex-shrink:0;" onerror="this.style.display='none'">
      <div style="flex:1;min-width:0;cursor:pointer;" onclick="openBookmark(${realIdx})">
        <div style="font-weight:600;font-size:0.9rem;white-space:nowrap;overflow:hidden;text-overflow:ellipsis;">${escapeHtml(bm.title || host)}</div>
        <div style="font-size:0.72rem;color:var(--text-dim);white-space:nowrap;overflow:hidden;text-overflow:ellipsis;">${escapeHtml(bm.url || '')}</div>
      </div>
      ${bm.tag ? `<span class="tv-chip" style="padding:0.2rem 0.5rem;font-size:0.68rem;">${escapeHtml(bm.tag)}</span>` : ''}
      <button class="tv-chip" onclick="openBookmark(${realIdx})" title="Open">Open</button>
      <button class="tv-chip" onclick="deleteBookmark(${realIdx})" title="Delete">✕</button>
    </div>`;
  }).join('');
}

function addBookmark() {
  const title = document.getElementById('bmTitle').value.trim();
  const url = document.getElementById('bmUrl').value.trim();
  const tag = (document.getElementById('bmTag')?.value || '').trim();
  if (!url) { toast('URL required', 'warning'); return; }
  let finalUrl = url;
  if (!/^https?:\/\//i.test(finalUrl)) finalUrl = 'https://' + finalUrl;
  const bookmarks = getData('bookmarks', []);
  const bm = { title: title || finalUrl, url: finalUrl, tag, ts: Date.now() };
  bookmarks.unshift(bm);
  setData('bookmarks', bookmarks.slice(0, 200));
  document.getElementById('bmTitle').value = '';
  document.getElementById('bmUrl').value = '';
  if (document.getElementById('bmTag')) document.getElementById('bmTag').value = '';
  renderBookmarks();
  updateDashStats();
  toastAction('Bookmark added', 'success', [{ label: 'Undo', run: () => {
    const bs = getData('bookmarks', []).filter(b => b.url !== bm.url || b.title !== bm.title);
    setData('bookmarks', bs);
    renderBookmarks();
    toast('Undone', 'info');
  }}]);
}

function deleteBookmark(i) {
  const bookmarks = getData('bookmarks', []);
  const removed = bookmarks[i];
  bookmarks.splice(i, 1);
  setData('bookmarks', bookmarks);
  renderBookmarks();
  toastAction('Bookmark deleted', 'info', [{ label: 'Undo', run: () => {
    const bs = getData('bookmarks', []);
    bs.splice(i, 0, removed);
    setData('bookmarks', bs);
    renderBookmarks();
    toast('Restored', 'success');
  }}]);
}
function openBookmark(i) {
  const bm = getData('bookmarks', [])[i];
  if (!bm) return;
  switchTab('proxy');
  proxyGoTo(bm.url);
}
function addBookmarkFromProxy() {
  if (!currentProxyUrl) { toast('No page loaded', 'error'); return; }
  const title = prompt('Bookmark title:', currentProxyUrl);
  if (!title) return;
  const bookmarks = getData('bookmarks', []);
  bookmarks.push({ title, url: currentProxyUrl });
  setData('bookmarks', bookmarks);
  renderBookmarks();
  toast('Bookmark added', 'success');
}

/* ============================================================
   AI — with streaming
   ============================================================ */
let aiPipeline = null;
let aiReady = false;
let aiLoading = false;
let aiHistory = [];

async function initAI() {
  const status = document.getElementById('aiModelStatus');
  if (aiLoading || aiReady) return;
  aiLoading = true;
  try {
    status.textContent = '(loading model — first time may take 1–2 min)...';
    status.style.color = 'var(--warning)';
    const mod = await import('https://cdn.jsdelivr.net/npm/@huggingface/transformers@3.0.0');
    const { pipeline, env } = mod;
    env.allowLocalModels = false;
    env.useBrowserCache = true;
    try { env.backends.onnx.wasm.numThreads = Math.min(4, navigator.hardwareConcurrency || 2); } catch(e){}
    aiPipeline = await pipeline(
      'text-generation',
      'onnx-community/Qwen2.5-0.5B-Instruct',
      { dtype: 'q4', progress_callback: (p) => {
        if (p && p.status === 'progress' && p.progress != null) {
          status.textContent = `(downloading ${Math.round(p.progress)}%)`;
        } else if (p && p.status) {
          status.textContent = `(${p.status})`;
        }
      }}
    );
    aiReady = true;
    status.textContent = '(ready · Qwen2.5-0.5B)';
    status.style.color = 'var(--accent-bright)';
    toast('AI model ready', 'success');
  } catch (e) {
    console.warn('AI model load failed, using advanced fallback:', e);
    status.textContent = '(smart fallback mode)';
    status.style.color = 'var(--text-dim)';
    aiReady = false;
  } finally {
    aiLoading = false;
  }
}

async function sendAI() {
  const input = document.getElementById('aiInput');
  const msg = input.value.trim();
  if (!msg) return;
  const chat = document.getElementById('aiChat');
  chat.innerHTML += `<div class="ai-msg user"><strong>You</strong> ${escapeHtml(msg)}</div>`;
  input.value = '';
  chat.scrollTop = chat.scrollHeight;
  const sendBtn = document.getElementById('aiSendBtn');
  sendBtn.disabled = true;
  sendBtn.textContent = 'Thinking...';

  const botId = 'bot-' + Date.now();
  chat.innerHTML += `<div class="ai-msg bot" id="${botId}"><strong>AI</strong> <span class="ai-typing">…</span></div>`;
  chat.scrollTop = chat.scrollHeight;

  let reply = '';
  try {
    if (aiReady && aiPipeline) {
      const messages = [
        { role: 'system', content: 'You are Xorwel AI, a helpful, concise assistant embedded in an entertainment hub. Answer clearly. For math, show steps. For code, use fenced blocks.' },
        ...aiHistory.slice(-8),
        { role: 'user', content: msg }
      ];
      const out = await aiPipeline(messages, {
        max_new_tokens: 256,
        temperature: 0.7,
        do_sample: true,
        top_p: 0.9
      });
      const gen = out[0].generated_text;
      if (Array.isArray(gen)) {
        const last = gen[gen.length - 1];
        reply = (last && last.content) ? last.content : JSON.stringify(gen);
      } else if (typeof gen === 'string') {
        reply = gen.replace(msg, '').trim() || gen;
      } else {
        reply = String(gen);
      }
    } else {
      if (!aiLoading) initAI();
      reply = fallbackAI(msg);
    }
  } catch (e) {
    console.warn('AI gen error', e);
    reply = fallbackAI(msg);
  }

  reply = (reply || '').trim() || "I couldn't generate a reply. Try again.";
  aiHistory.push({ role: 'user', content: msg });
  aiHistory.push({ role: 'assistant', content: reply });
  if (aiHistory.length > 20) aiHistory = aiHistory.slice(-20);

  // Streaming reveal for nicer UX
  const el = document.getElementById(botId);
  if (el) {
    const label = el.querySelector('strong');
    el.innerHTML = '';
    if (label) el.appendChild(label);
    const span = document.createElement('span');
    el.appendChild(span);
    const html = formatAIReply(reply);
    let i = 0;
    const step = Math.max(1, Math.floor(html.length / 220));
    const tick = () => {
      i += step;
      span.innerHTML = html.slice(0, i);
      chat.scrollTop = chat.scrollHeight;
      if (i < html.length) requestAnimationFrame(tick);
    };
    tick();
  }
  sendBtn.disabled = false;
  sendBtn.textContent = 'Send';
}

function formatAIReply(text) {
  let s = escapeHtml(text);
  s = s.replace(/```([\s\S]*?)```/g, '<pre style="background:rgba(0,0,0,0.4);padding:0.6rem;border-radius:8px;overflow:auto;margin:0.4rem 0;font-size:0.82rem;">$1</pre>');
  s = s.replace(/`([^`]+)`/g, '<code style="background:rgba(220,20,60,0.15);padding:0.1rem 0.35rem;border-radius:4px;">$1</code>');
  s = s.replace(/\*\*([^*]+)\*\*/g, '<strong>$1</strong>');
  s = s.replace(/\n/g, '<br>');
  return s;
}

function fallbackAI(msg) {
  const lower = msg.toLowerCase().trim();
  const mathMatch = lower.match(/(?:solve|calculate|compute|what is|what's)?\s*([0-9+\-*/().\s]+)=?/);
  if (mathMatch || /^[\d+\-*/().\s]+$/.test(lower)) {
    const expr = (mathMatch ? mathMatch[1] : lower).replace(/[^0-9+\-*/().]/g, '');
    try {
      const result = Function('"use strict"; return (' + expr + ')')();
      if (typeof result === 'number' && isFinite(result)) return `**Result:** ${result}\n\nExpression: \`${expr}\``;
    } catch (e) {}
  }
  if (lower.includes('joke')) {
    const jokes = [
      "Why did the football coach go to the bank? To get his quarter back!",
      "Why don't scientists trust atoms? Because they make up everything!",
      "I told my computer I needed a break — it said 'No problem, I'll go to sleep.'",
      "Why do programmers prefer dark mode? Because light attracts bugs.",
      "A SQL query walks into a bar, walks up to two tables and asks: 'Can I join you?'"
    ];
    return jokes[Math.floor(Math.random() * jokes.length)];
  }
  if (lower.includes('resume') || lower.includes('cv')) {
    return "**Resume outline**\n\n1. **Contact** — name, email, phone, links\n2. **Summary** — 2–3 lines of impact\n3. **Experience** — role, company, bullets with metrics\n4. **Skills** — languages, tools, soft skills\n5. **Education** — school, degree, year\n6. **Projects** — 1–3 highlights\n\nTip: quantify results (e.g. \"reduced load time 40%\").";
  }
  if (lower.includes('essay') || lower.includes('write about') || lower.includes('paragraph on')) {
    const topic = lower.replace(/.*(?:essay|write about|paragraph on)\s*(?:on|about)?\s*/, '').trim() || 'the topic';
    return `**Short essay on ${topic}**\n\n${topic.charAt(0).toUpperCase() + topic.slice(1)} shapes how we understand the world. Key factors include context, evidence, and perspective. A clear thesis, supporting examples, and a concise conclusion make the argument persuasive.\n\n*(Local model still loading — this is a structured outline you can expand.)*`;
  }
  if (lower.includes('hello') || lower.includes('hi ') || lower === 'hi' || lower.includes('hey')) {
    return "Hey! I'm **Xorwel AI**. Ask me math, jokes, resume help, explanations, or anything else. The full local model loads in the background for richer answers.";
  }
  if (lower.includes('help') || lower.includes('what can you')) {
    return "I can help with:\n• **Math** — solve expressions\n• **Writing** — essays, resumes, summaries\n• **Code** — ideas and snippets\n• **Jokes & trivia**\n• **General Q&A**\n\nType a question or use the quick chips above.";
  }
  if (lower.includes('code') || lower.includes('javascript') || lower.includes('python') || lower.includes('function')) {
    return "Describe what you want the code to do (language + goal). Example: *\"Python function to reverse a string\"* or *\"JS debounce helper\"*. I'll draft a concise snippet.";
  }
  return `You asked: *"${msg.slice(0, 120)}${msg.length > 120 ? '…' : ''}"*\n\nI'm on **smart fallback** while the local model loads (or if WebGPU/WASM isn't available). Try:\n• A math problem like \`2x + 5 = 15\`\n• \"Tell me a joke\"\n• \"Write a resume summary\"\n• \"Essay on climate change\"\n\nOnce the model finishes downloading, answers get much richer.`;
}

function quickAI(prompt) {
  document.getElementById('aiInput').value = prompt;
  sendAI();
}
function clearAI() {
  document.getElementById('aiChat').innerHTML = '';
  aiHistory = [];
  toast('Chat cleared', 'info');
}

function escapeHtml(s) {
  return String(s).replace(/[&<>"']/g, c => ({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c]));
}

/* ============================================================
   SETTINGS
   ============================================================ */
function setPreferredSource() {
  preferredSource = document.getElementById('preferredSourceSelect').value;
  setData('preferredSource', preferredSource);
  toast('Preferred source: ' + (preferredSource || 'Auto'), 'info');
}
function setSourceTimeout() {
  const v = parseInt(document.getElementById('sourceTimeoutInput').value);
  if (v >= 1 && v <= 10) { sourceTestTimeout = v; setData('sourceTestTimeout', v); toast('Timeout: ' + v + 's', 'info'); }
}
function setPrimaryEngine() {
  primaryEngine = document.getElementById('primaryEngineSelect').value;
  setData('primaryEngine', primaryEngine);
  setEngine(primaryEngine);
  toast('Primary engine saved: ' + primaryEngine, 'success');
}
function clearSourceCache() {
  localStorage.removeItem('xorwel_source_cache');
  toast('Source cache cleared', 'success');
}
function clearAllData() {
  if (!confirm('Clear all data?')) return;
  Object.keys(localStorage).forEach(k => { if (k.startsWith('xorwel_')) localStorage.removeItem(k); });
  location.reload();
}

/* ============================================================
   SEARCH — with full global overlay
   ============================================================ */
async function globalSearch() {
  const raw = document.getElementById('searchInput').value.trim();
  const q = raw.toLowerCase();
  if (!q) {
    document.querySelectorAll('#gameGrid .card, #cloudGrid .card, #tvGrid .card').forEach(c => c.style.display = 'flex');
    toast('Cleared filters', 'info');
    return;
  }
  let localCount = 0;
  document.querySelectorAll('#gameGrid .card, #cloudGrid .card, #tvGrid .card').forEach(c => {
    const title = c.querySelector('h3')?.textContent.toLowerCase() || '';
    const match = title.includes(q);
    c.style.display = match ? 'flex' : 'none';
    if (match) localCount++;
  });

  try {
    toast('Searching TMDB…', 'info', 1200);
    const [movies, shows] = await Promise.all([
      fetch(`https://api.themoviedb.org/3/search/movie?api_key=${TMDB_API_KEY}&query=${encodeURIComponent(raw)}&page=1`).then(r => r.json()).catch(() => ({results:[]})),
      fetch(`https://api.themoviedb.org/3/search/tv?api_key=${TMDB_API_KEY}&query=${encodeURIComponent(raw)}&page=1`).then(r => r.json()).catch(() => ({results:[]}))
    ]);
    const extra = [
      ...(movies.results || []).map(x => ({...x, media_type:'movie'})),
      ...(shows.results || []).map(x => ({...x, media_type:'tv'}))
    ];
    if (extra.length) {
      const seen = new Set(allTVItems.map(i => (i.media_type||'') + '_' + i.id));
      const fresh = extra.filter(i => !seen.has(i.media_type + '_' + i.id));
      allTVItems = fresh.concat(allTVItems);
      filteredTVItems = allTVItems.filter(i => {
        const t = (i.title || i.name || '').toLowerCase();
        return t.includes(q);
      });
      if (filteredTVItems.length === 0) filteredTVItems = fresh;
      renderTV(filteredTVItems);
      switchTab('tv');
      toast(`Found ${fresh.length} titles on TMDB + ${localCount} local`, 'success');
      return;
    }
  } catch (e) {}

  const gamesVisible = Array.from(document.querySelectorAll('#gameGrid .card')).some(c => c.style.display !== 'none');
  const tvVisible = Array.from(document.querySelectorAll('#tvGrid .card')).some(c => c.style.display !== 'none');
  if (tvVisible && !gamesVisible) switchTab('tv');
  else if (gamesVisible) switchTab('games');
  toast(localCount ? `Found ${localCount} local result${localCount!==1?'s':''}` : `No results for "${raw}"`, localCount ? 'success' : 'warning');
}

/* Enhanced overlay search */
let searchOverlayResults = [];
let searchOverlayIndex = 0;
let searchDebounce = null;

function openSearchOverlay() {
  const el = document.getElementById('searchOverlay');
  if (!el) return;
  el.classList.add('show');
  const inp = document.getElementById('searchOverlayInput');
  if (inp) { setTimeout(() => inp.focus(), 30); inp.value = ''; }
  doGlobalSearch('');
}
function closeSearchOverlay() {
  document.getElementById('searchOverlay')?.classList.remove('show');
}
function searchOverlayKey(e) {
  if (e.key === 'Escape') { closeSearchOverlay(); }
  else if (e.key === 'ArrowDown') { e.preventDefault(); searchOverlayIndex = Math.min(searchOverlayIndex + 1, searchOverlayResults.length - 1); highlightSearchResult(); }
  else if (e.key === 'ArrowUp') { e.preventDefault(); searchOverlayIndex = Math.max(searchOverlayIndex - 1, 0); highlightSearchResult(); }
  else if (e.key === 'Enter') {
    e.preventDefault();
    const r = searchOverlayResults[searchOverlayIndex];
    if (r) { closeSearchOverlay(); r.run(); }
  }
}
function highlightSearchResult() {
  document.querySelectorAll('.search-result-item').forEach((el, i) => el.classList.toggle('active', i === searchOverlayIndex));
  const active = document.querySelector('.search-result-item.active');
  if (active) active.scrollIntoView({ block: 'nearest' });
}
function doGlobalSearch(q) {
  clearTimeout(searchDebounce);
  searchDebounce = setTimeout(() => runGlobalSearch(q), 160);
}
function runGlobalSearch(q) {
  const box = document.getElementById('searchOverlayResults');
  if (!box) return;
  q = (q || '').trim();
  searchOverlayResults = [];
  searchOverlayIndex = 0;

  if (!q) {
    box.innerHTML = `
      <div class="search-group-title">Quick access</div>
      ${[
        { title: 'Browse Games', sub: 'Thousands of HTML5 games', run: () => switchTab('games') },
        { title: 'Browse TV & Movies', sub: 'Powered by TMDB + MAL', run: () => switchTab('tv') },
        { title: 'Music', sub: 'Audius streaming', run: () => switchTab('music') },
        { title: 'Cloud Gaming', sub: 'GeForce NOW, Xbox, Luna…', run: () => switchTab('cloud') },
        { title: 'Proxy Browser', sub: 'Private web access', run: () => switchTab('proxy') },
        { title: 'Ask AI', sub: 'Local model in your browser', run: () => switchTab('ai') }
      ].map(r => {
        searchOverlayResults.push(r);
        return `<button class="search-result-item"><div class="sr-info"><div class="sr-title">${r.title}</div><div class="sr-sub">${r.sub}</div></div><span class="sr-badge">Go</span></button>`;
      }).join('')}
    `;
    return;
  }

  const lq = q.toLowerCase();

  // Games
  const gameHits = games.filter(g => g.title.toLowerCase().includes(lq)).slice(0, 6);
  // Cloud
  const cloudHits = CLOUD_GAMES.filter(g => (g.title + ' ' + (g.desc || '')).toLowerCase().includes(lq)).slice(0, 5);
  // TV
  const tvHits = allTVItems.filter(i => (i.title || i.name || i.title_english || '').toLowerCase().includes(lq)).slice(0, 8);
  // Bookmarks
  const bmHits = (getData('bookmarks', []) || []).filter(b => (b.title + ' ' + b.url).toLowerCase().includes(lq)).slice(0, 5);
  // Music (from last list)
  const musicHits = (musicState?.lastList || []).filter(t => (t.title || '').toLowerCase().includes(lq)).slice(0, 6);

  let html = '';
  if (gameHits.length) {
    html += `<div class="search-group-title">Games</div>`;
    gameHits.forEach(g => {
      const idx = searchOverlayResults.length;
      searchOverlayResults.push({ title: g.title, run: () => { switchTab('games'); setTimeout(() => playGame(g.url, g.title), 80); } });
      html += `<button class="search-result-item" data-idx="${idx}" onclick="runSearchResult(${idx})">
        <img src="${g.icon}" onerror="this.style.display='none'">
        <div class="sr-info"><div class="sr-title">${escapeHtml(g.title)}</div><div class="sr-sub">Game</div></div>
        <span class="sr-badge">Play</span>
      </button>`;
    });
  }
  if (tvHits.length) {
    html += `<div class="search-group-title">Movies · Shows · Anime</div>`;
    tvHits.forEach(i => {
      const idx = searchOverlayResults.length;
      const type = i.media_type || 'movie';
      const title = i.title || i.name || i.title_english || 'Untitled';
      const poster = i.poster_path ? `https://image.tmdb.org/t/p/w92${i.poster_path}` : (i.images?.jpg?.image_url || '');
      const id = i.id || i.mal_id;
      searchOverlayResults.push({ title, run: () => { switchTab('tv'); setTimeout(() => openTvDetailById(type, id), 80); } });
      html += `<button class="search-result-item" data-idx="${idx}" onclick="runSearchResult(${idx})">
        ${poster ? `<img src="${poster}" onerror="this.style.display='none'">` : '<div style="width:36px;height:36px;border-radius:6px;background:#1a0010;"></div>'}
        <div class="sr-info"><div class="sr-title">${escapeHtml(title)}</div><div class="sr-sub">${type}</div></div>
        <span class="sr-badge">${type}</span>
      </button>`;
    });
  }
  if (cloudHits.length) {
    html += `<div class="search-group-title">Cloud Gaming</div>`;
    cloudHits.forEach(g => {
      const idx = searchOverlayResults.length;
      searchOverlayResults.push({ title: g.title, run: () => launchCloud(g.url, g.title, g.openMode || 'tab') });
      html += `<button class="search-result-item" data-idx="${idx}" onclick="runSearchResult(${idx})">
        <img src="${g.icon}" onerror="this.style.display='none'">
        <div class="sr-info"><div class="sr-title">${escapeHtml(g.title)}</div><div class="sr-sub">${escapeHtml(g.desc || '')}</div></div>
        <span class="sr-badge">Launch</span>
      </button>`;
    });
  }
  if (bmHits.length) {
    html += `<div class="search-group-title">Bookmarks</div>`;
    bmHits.forEach(b => {
      const idx = searchOverlayResults.length;
      searchOverlayResults.push({ title: b.title, run: () => { switchTab('proxy'); proxyGoTo(b.url); } });
      html += `<button class="search-result-item" data-idx="${idx}" onclick="runSearchResult(${idx})">
        <div class="sr-info"><div class="sr-title">${escapeHtml(b.title)}</div><div class="sr-sub">${escapeHtml(b.url)}</div></div>
        <span class="sr-badge">Open</span>
      </button>`;
    });
  }
  if (musicHits.length) {
    html += `<div class="search-group-title">Music (loaded)</div>`;
    musicHits.forEach(t => {
      const idx = searchOverlayResults.length;
      searchOverlayResults.push({ title: t.title, run: () => { switchTab('music'); setTimeout(() => musicPlayTrack(t), 80); } });
      html += `<button class="search-result-item" data-idx="${idx}" onclick="runSearchResult(${idx})">
        <div class="sr-info"><div class="sr-title">${escapeHtml(t.title)}</div><div class="sr-sub">${escapeHtml(t.user?.name || '')}</div></div>
        <span class="sr-badge">Play</span>
      </button>`;
    });
  }

  // Always append "Search TMDB for X"
  const idx = searchOverlayResults.length;
  searchOverlayResults.push({ title: 'Search TMDB for "' + q + '"', run: () => { document.getElementById('searchInput').value = q; globalSearch(); switchTab('tv'); } });
  html += `<div class="search-group-title">More</div>
    <button class="search-result-item" data-idx="${idx}" onclick="runSearchResult(${idx})">
      <div class="sr-info"><div class="sr-title">Search TMDB for "${escapeHtml(q)}"</div><div class="sr-sub">Find more titles online</div></div>
      <span class="sr-badge">TMDB</span>
    </button>`;

  if (!gameHits.length && !tvHits.length && !cloudHits.length && !bmHits.length && !musicHits.length) {
    // still show TMDB row; no empty state needed
  }

  box.innerHTML = html;
  highlightSearchResult();
}
function runSearchResult(i) {
  const r = searchOverlayResults[i];
  if (r) { closeSearchOverlay(); r.run(); }
}

/* ============================================================
   MUSIC — Audius API
   ============================================================ */
const AUDIUS_HOSTS = [
  'https://discoveryprovider.audius.co',
  'https://discoveryprovider2.audius.co',
  'https://discoveryprovider3.audius.co'
];
const AUDIUS_APP = 'xorwel';
let musicState = {
  audio: null, queue: [], queueIndex: 0, shuffle: false, repeat: 'off',
  currentTrack: null, favorites: [], recent: [],
  visualizer: null, audioCtx: null, analyser: null,
  currentHost: AUDIUS_HOSTS[0], lastList: [], initialized: false, playing: false
};

async function audiusFetch(path) {
  for (const host of AUDIUS_HOSTS) {
    try {
      const url = `${host}/v1${path}${path.includes('?') ? '&' : '?'}app_name=${AUDIUS_APP}`;
      const res = await fetch(url);
      if (res.ok) { musicState.currentHost = host; return await res.json(); }
    } catch (e) {}
  }
  throw new Error('All Audius hosts failed');
}

async function musicInit() {
  if (musicState.initialized) return;
  musicState.initialized = true;
  musicState.audio = document.getElementById('musicAudio');
  musicState.audio.volume = 0.8;
  musicState.audio.addEventListener('timeupdate', musicUpdateProgress);
  musicState.audio.addEventListener('loadedmetadata', musicUpdateDuration);
  musicState.audio.addEventListener('ended', musicOnEnded);
  musicState.audio.addEventListener('play', () => musicUpdatePlayIcon(true));
  musicState.audio.addEventListener('pause', () => musicUpdatePlayIcon(false));
  musicState.audio.addEventListener('error', (e) => {
    console.warn('Audio error:', e);
    toast('Playback error, skipping...', 'warning');
    setTimeout(musicNext, 800);
  });
  musicState.favorites = getData('musicFavorites', []);
  musicState.recent = getData('musicRecent', []);
  document.querySelectorAll('.music-tab').forEach(tab => {
    tab.addEventListener('click', () => {
      document.querySelectorAll('.music-tab').forEach(t => t.classList.remove('active'));
      tab.classList.add('active');
      musicLoadView(tab.dataset.mtab);
    });
  });
  await musicLoadView('trending');
}

function setupMusicVisualizerAnalyser() {
  if (musicState.audioCtx) return;
  try {
    const AudioCtx = window.AudioContext || window.webkitAudioContext;
    musicState.audioCtx = new AudioCtx();
    const source = musicState.audioCtx.createMediaElementSource(musicState.audio);
    const analyser = musicState.audioCtx.createAnalyser();
    analyser.fftSize = 128;
    source.connect(analyser);
    analyser.connect(musicState.audioCtx.destination);
    musicState.analyser = analyser;
    drawMusicViz();
  } catch (e) {}
}
function drawMusicViz() {
  if (!musicState.analyser) return;
  const canvas = document.getElementById('musicViz');
  if (!canvas) return;
  const ctx = canvas.getContext('2d');
  const data = new Uint8Array(musicState.analyser.frequencyBinCount);
  function loop() {
    requestAnimationFrame(loop);
    musicState.analyser.getByteFrequencyData(data);
    ctx.clearRect(0, 0, canvas.width, canvas.height);
    const barW = canvas.width / data.length;
    ctx.fillStyle = getComputedStyle(document.documentElement).getPropertyValue('--accent').trim() || '#dc143c';
    for (let i = 0; i < data.length; i++) {
      const h = (data[i] / 255) * canvas.height;
      ctx.globalAlpha = 0.4 + (data[i] / 255) * 0.55;
      ctx.fillRect(i * barW, canvas.height - h, barW - 1, h);
    }
    ctx.globalAlpha = 1;
  }
  loop();
}

async function musicLoadView(view) {
  const content = document.getElementById('musicContent');
  content.innerHTML = '<div class="loading"><div class="spinner"></div>Loading...</div>';
  try {
    if (view === 'trending') {
      const data = await audiusFetch('/tracks/trending?time=week&limit=50');
      musicRenderTrackList(data.data || [], 'Trending This Week');
    } else if (view === 'genres') {
      musicLoadGenres();
    } else if (view === 'favorites') {
      const favs = getData('musicFavorites', []);
      if (!favs.length) content.innerHTML = '<div class="loading">No favorites yet. Click the heart on any track.</div>';
      else musicRenderTrackList(favs, 'My Favorites');
    } else if (view === 'recent') {
      const recent = getData('musicRecent', []);
      if (!recent.length) content.innerHTML = '<div class="loading">No recent tracks yet.</div>';
      else musicRenderTrackList(recent, 'Recently Played');
    } else if (view === 'playlists') {
      const data = await audiusFetch('/playlists/search?query=popular&limit=30');
      musicLoadPlaylistsData(data.data || []);
    }
  } catch (e) {
    content.innerHTML = `<div class="loading">Failed to load: ${e.message}</div>`;
  }
}

function musicLoadGenres() {
  const genres = [
    { name: 'Electronic', color: '#8b5cf6' }, { name: 'Hip-Hop/Rap', color: '#ef4444' },
    { name: 'Rock', color: '#f97316' }, { name: 'Pop', color: '#ec4899' },
    { name: 'R&B/Soul', color: '#a855f7' }, { name: 'House', color: '#06b6d4' },
    { name: 'Techno', color: '#0891b2' }, { name: 'Dubstep', color: '#7c3aed' },
    { name: 'Trap', color: '#dc2626' }, { name: 'Drum & Bass', color: '#059669' },
    { name: 'Lo-Fi', color: '#6366f1' }, { name: 'Ambient', color: '#0ea5e9' },
    { name: 'Jazz', color: '#f59e0b' }, { name: 'Classical', color: '#78716c' },
    { name: 'Country', color: '#b45309' }, { name: 'Reggae', color: '#16a34a' },
    { name: 'Latin', color: '#f43f5e' }, { name: 'Metal', color: '#374151' },
    { name: 'Punk', color: '#e11d48' }, { name: 'Folk', color: '#65a30d' },
    { name: 'Soundtrack', color: '#4f46e5' }, { name: 'Experimental', color: '#a78bfa' },
    { name: 'Alternative', color: '#84cc16' }, { name: 'Disco', color: '#eab308' }
  ];
  const content = document.getElementById('musicContent');
  content.innerHTML = `
    <div class="music-section-title">Browse Genres</div>
    <div class="genre-grid">
      ${genres.map((g, i) => `<div class="genre-card" style="background: linear-gradient(135deg, ${g.color}, ${shadeColor(g.color, -30)}); animation-delay: ${Math.min(i * 20, 300)}ms;" onclick="musicLoadGenre('${g.name.replace(/'/g, "\\'")}')">${g.name}</div>`).join('')}
    </div>
  `;
}
function shadeColor(hex, pct) {
  const h = hex.replace('#',''); const n = parseInt(h, 16);
  let r = Math.max(0, Math.min(255, (n >> 16) + pct));
  let g = Math.max(0, Math.min(255, ((n >> 8) & 0xff) + pct));
  let b = Math.max(0, Math.min(255, (n & 0xff) + pct));
  return '#' + ((r << 16) | (g << 8) | b).toString(16).padStart(6, '0');
}
async function musicLoadGenre(genre) {
  const content = document.getElementById('musicContent');
  content.innerHTML = '<div class="loading"><div class="spinner"></div>Loading...</div>';
  try {
    const data = await audiusFetch(`/tracks/trending?genre=${encodeURIComponent(genre)}&time=week&limit=50`);
    const tracks = data.data || [];
    if (!tracks.length) content.innerHTML = `<div class="loading">No tracks for ${genre}</div>`;
    else musicRenderTrackList(tracks, genre);
  } catch (e) { content.innerHTML = `<div class="loading">Failed: ${e.message}</div>`; }
}
function musicLoadPlaylistsData(playlists) {
  const content = document.getElementById('musicContent');
  if (!playlists.length) { content.innerHTML = '<div class="loading">No playlists found.</div>'; return; }
  content.innerHTML = `
    <div class="music-section-title">Popular Playlists</div>
    <div class="playlist-grid">
      ${playlists.map((p, i) => `
        <div class="playlist-card" style="animation-delay: ${Math.min(i * 20, 300)}ms;" onclick="musicLoadPlaylist('${p.id}')">
          <div class="pl-art">${p.artwork ? `<img src="${p.artwork['480x480'] || p.artwork['150x150'] || ''}" loading="lazy" onerror="this.style.display='none'">` : ''}</div>
          <div class="pl-title">${escapeHtml(p.playlist_name)}</div>
          <div class="pl-artist">${escapeHtml(p.user?.name || '')}</div>
        </div>`).join('')}
    </div>
  `;
}
async function musicLoadPlaylist(id) {
  const content = document.getElementById('musicContent');
  content.innerHTML = '<div class="loading"><div class="spinner"></div>Loading playlist...</div>';
  try {
    const data = await audiusFetch(`/playlists/${id}/tracks`);
    const tracks = data.data || [];
    if (!tracks.length) content.innerHTML = '<div class="loading">Empty playlist</div>';
    else musicRenderTrackList(tracks, 'Playlist');
  } catch (e) { content.innerHTML = `<div class="loading">Failed: ${e.message}</div>`; }
}

function musicArtUrl(track) {
  if (!track || !track.artwork) return '';
  return track.artwork['480x480'] || track.artwork['150x150'] || track.artwork['1000x1000'] || '';
}

function musicRenderTrackList(tracks, title) {
  const content = document.getElementById('musicContent');
  const favs = getData('musicFavorites', []);
  const favIds = favs.map(f => f.id);
  content.innerHTML = `
    <div class="music-section-title">${escapeHtml(title)}</div>
    <div class="track-list">
      ${tracks.map((t, i) => {
        const art = musicArtUrl(t);
        const isFav = favIds.includes(t.id);
        const isPlaying = musicState.currentTrack && musicState.currentTrack.id === t.id;
        return `
          <div class="track-row ${isPlaying ? 'playing' : ''}" data-track-id="${t.id}" style="animation-delay: ${Math.min(i * 10, 250)}ms;" onclick="musicPlayTrackById('${t.id}')">
            <div class="track-num">${i + 1}</div>
            <div class="track-art">${art ? `<img src="${art}" loading="lazy" onerror="this.style.display='none'">` : ''}</div>
            <div class="track-info">
              <div class="track-title">${escapeHtml(t.title)}</div>
              <div class="track-artist">${escapeHtml(t.user?.name || 'Unknown')}</div>
            </div>
            <div class="track-album">${escapeHtml(t.album || t.genre || '')}</div>
            <div class="track-dur">${formatMusicDuration(t.duration)}</div>
            <div class="track-actions">
              <button class="${isFav ? 'fav-on' : ''}" onclick="event.stopPropagation();musicToggleFav('${t.id}')">
                <svg width="14" height="14" viewBox="0 0 24 24" fill="${isFav ? 'currentColor' : 'none'}" stroke="currentColor" stroke-width="2"><path d="M20.84 4.61a5.5 5.5 0 0 0-7.78 0L12 5.67l-1.06-1.06a5.5 5.5 0 0 0-7.78 7.78l1.06 1.06L12 21.23l7.78-7.78 1.06-1.06a5.5 5.5 0 0 0 0-7.78z"/></svg>
              </button>
              <button onclick="event.stopPropagation();musicAddToQueue('${t.id}')">
                <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><line x1="12" y1="5" x2="12" y2="19"/><line x1="5" y1="12" x2="19" y2="12"/></svg>
              </button>
            </div>
          </div>`;
      }).join('')}
    </div>
  `;
  musicState.lastList = tracks;
}

function formatMusicDuration(seconds) {
  if (!seconds) return '—';
  const m = Math.floor(seconds / 60), s = seconds % 60;
  return m + ':' + s.toString().padStart(2, '0');
}

async function musicSearch() {
  const q = document.getElementById('musicSearchInput').value.trim();
  if (!q) return;
  const content = document.getElementById('musicContent');
  content.innerHTML = '<div class="loading"><div class="spinner"></div>Searching...</div>';
  try {
    const [tracks, users] = await Promise.all([
      audiusFetch(`/tracks/search?query=${encodeURIComponent(q)}&limit=40`).catch(() => ({ data: [] })),
      audiusFetch(`/users/search?query=${encodeURIComponent(q)}&limit=20`).catch(() => ({ data: [] }))
    ]);
    let html = '';
    if (users.data && users.data.length) {
      html += `<div class="music-section-title">Artists</div><div class="playlist-grid">`;
      users.data.forEach((u, i) => {
        const art = u.profile_picture?.['480x480'] || u.profile_picture?.['150x150'] || '';
        html += `<div class="playlist-card" style="animation-delay: ${Math.min(i * 20, 300)}ms;" onclick="musicLoadArtist('${u.id}')">
          <div class="pl-art" style="border-radius:50%;">${art ? `<img src="${art}" loading="lazy">` : ''}</div>
          <div class="pl-title">${escapeHtml(u.name)}</div>
          <div class="pl-artist">${u.follower_count ? u.follower_count.toLocaleString() + ' followers' : 'Artist'}</div>
        </div>`;
      });
      html += `</div>`;
    }
    if (tracks.data && tracks.data.length) {
      html += `<div class="music-section-title">Tracks</div><div class="track-list">`;
      tracks.data.forEach((t, i) => {
        const art = musicArtUrl(t);
        html += `<div class="track-row" onclick="musicPlayTrackById('${t.id}')">
          <div class="track-num">${i + 1}</div>
          <div class="track-art">${art ? `<img src="${art}" loading="lazy">` : ''}</div>
          <div class="track-info"><div class="track-title">${escapeHtml(t.title)}</div><div class="track-artist">${escapeHtml(t.user?.name || 'Unknown')}</div></div>
          <div class="track-album">${escapeHtml(t.album || '')}</div>
          <div class="track-dur">${formatMusicDuration(t.duration)}</div>
          <div class="track-actions"></div>
        </div>`;
      });
      html += `</div>`;
      musicState.lastList = tracks.data;
    }
    if (!html) html = '<div class="loading">No results found.</div>';
    content.innerHTML = html;
  } catch (e) { content.innerHTML = `<div class="loading">Search failed: ${e.message}</div>`; }
}

async function musicLoadArtist(id) {
  const content = document.getElementById('musicContent');
  content.innerHTML = '<div class="loading"><div class="spinner"></div>Loading artist...</div>';
  try {
    const data = await audiusFetch(`/users/${id}/tracks?limit=40`);
    const tracks = data.data || [];
    if (!tracks.length) content.innerHTML = '<div class="loading">No tracks.</div>';
    else musicRenderTrackList(tracks, tracks[0].user?.name || 'Artist');
  } catch (e) { content.innerHTML = `<div class="loading">Failed: ${e.message}</div>`; }
}

async function musicPlayTrackById(id) {
  try {
    const data = await audiusFetch(`/tracks/${id}`);
    if (!data.data) return;
    await musicPlayTrack(data.data);
  } catch (e) { toast('Failed to load track: ' + e.message, 'error'); }
}

async function musicPlayTrack(track) {
  musicState.currentTrack = track;
  const streamUrl = `${musicState.currentHost}/v1/tracks/${track.id}/stream?app_name=${AUDIUS_APP}`;
  musicState.audio.src = streamUrl;
  try { await musicState.audio.play(); setupMusicVisualizerAnalyser(); }
  catch (e) { toast('Playback failed: ' + e.message, 'error'); return; }

  document.getElementById('mpTitle').textContent = track.title || 'Unknown';
  document.getElementById('mpArtist').textContent = track.user?.name || 'Unknown';
  const art = musicArtUrl(track);
  const mpArt = document.getElementById('mpArt');
  if (art) mpArt.innerHTML = `<img src="${art}">`;
  else mpArt.innerHTML = '<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M9 18V5l12-2v13"/><circle cx="6" cy="18" r="3"/><circle cx="18" cy="16" r="3"/></svg>';

  musicUpdateFavIcon();
  const recent = getData('musicRecent', []).filter(r => r.id !== track.id);
  recent.unshift({ id: track.id, title: track.title, user: track.user, artwork: track.artwork, duration: track.duration, album: track.album });
  setData('musicRecent', recent.slice(0, 30));

  document.querySelectorAll('.track-row').forEach(row => row.classList.toggle('playing', row.dataset.trackId === track.id));
  document.getElementById('musicPlayer').classList.add('visible');
  renderMusicQueue();
}

function musicUpdatePlayIcon(isPlaying) {
  const icon = document.getElementById('mpPlayIcon');
  if (!icon) return;
  if (isPlaying) icon.innerHTML = '<path d="M6 4h4v16H6zM14 4h4v16h-4z"/>';
  else icon.innerHTML = '<path d="M8 5v14l11-7z"/>';
}
function musicUpdateProgress() {
  const audio = musicState.audio;
  if (!audio || !audio.duration) return;
  const pct = (audio.currentTime / audio.duration) * 100;
  document.getElementById('mpProgressFill').style.width = pct + '%';
  document.getElementById('mpCur').textContent = formatMusicDuration(Math.floor(audio.currentTime));
}
function musicUpdateDuration() {
  if (musicState.audio && musicState.audio.duration) {
    document.getElementById('mpDur').textContent = formatMusicDuration(Math.floor(musicState.audio.duration));
  }
}
function musicToggle() {
  if (!musicState.currentTrack) {
    if (musicState.lastList && musicState.lastList.length) musicPlayTrack(musicState.lastList[0]);
    return;
  }
  if (musicState.audio.paused) musicState.audio.play().catch(e => toast('Play failed: ' + e.message, 'error'));
  else musicState.audio.pause();
}
function musicNext() {
  const list = musicState.lastList || [];
  if (!list.length) return;
  if (musicState.shuffle) { musicPlayTrack(list[Math.floor(Math.random() * list.length)]); return; }
  const idx = list.findIndex(t => t.id === musicState.currentTrack?.id);
  const nextIdx = idx >= 0 ? (idx + 1) % list.length : 0;
  musicPlayTrack(list[nextIdx]);
}
function musicPrev() {
  const list = musicState.lastList || [];
  if (!list.length) return;
  if (musicState.audio.currentTime > 3) { musicState.audio.currentTime = 0; return; }
  const idx = list.findIndex(t => t.id === musicState.currentTrack?.id);
  const prevIdx = idx > 0 ? idx - 1 : list.length - 1;
  musicPlayTrack(list[prevIdx]);
}
function musicOnEnded() {
  if (musicState.repeat === 'one') { musicState.audio.currentTime = 0; musicState.audio.play(); return; }
  if (musicState.lastList && musicState.lastList.length > 1) musicNext();
  else musicUpdatePlayIcon(false);
}
function musicShuffle() {
  musicState.shuffle = !musicState.shuffle;
  document.getElementById('mpShuffle').classList.toggle('active', musicState.shuffle);
  toast('Shuffle ' + (musicState.shuffle ? 'ON' : 'OFF'), 'info');
}
function musicRepeat() {
  const modes = ['off', 'all', 'one'];
  const idx = modes.indexOf(musicState.repeat);
  musicState.repeat = modes[(idx + 1) % 3];
  document.getElementById('mpRepeat').classList.toggle('active', musicState.repeat !== 'off');
  toast('Repeat: ' + musicState.repeat.toUpperCase(), 'info');
}
function musicSeek(e) {
  if (!musicState.audio || !musicState.audio.duration) return;
  const rect = document.getElementById('mpProgress').getBoundingClientRect();
  const pct = (e.clientX - rect.left) / rect.width;
  musicState.audio.currentTime = pct * musicState.audio.duration;
}
function musicVolume(v) { if (musicState.audio) musicState.audio.volume = v / 100; }
function musicToggleFavCurrent() { if (musicState.currentTrack) musicToggleFav(musicState.currentTrack.id); }
function musicToggleFav(trackId) {
  let favs = getData('musicFavorites', []);
  const idx = favs.findIndex(f => f.id === trackId);
  if (idx >= 0) { favs.splice(idx, 1); toast('Removed from favorites', 'info'); }
  else {
    const track = (musicState.lastList || []).find(t => t.id === trackId);
    if (track) { favs.unshift({ id: track.id, title: track.title, user: track.user, artwork: track.artwork, duration: track.duration, album: track.album }); toast('Added to favorites', 'success'); }
  }
  setData('musicFavorites', favs);
  musicUpdateFavIcon();
  document.querySelectorAll(`.track-row[data-track-id="${trackId}"] .track-actions button`).forEach(b => {
    const svg = b.querySelector('svg path');
    if (svg) { const isFav = favs.some(f => f.id === trackId); svg.setAttribute('fill', isFav ? 'currentColor' : 'none'); b.classList.toggle('fav-on', isFav); }
  });
}
function musicUpdateFavIcon() {
  if (!musicState.currentTrack) return;
  const favs = getData('musicFavorites', []);
  const isFav = favs.some(f => f.id === musicState.currentTrack.id);
  const icon = document.querySelector('#mpFav svg path');
  if (icon) { icon.setAttribute('fill', isFav ? 'currentColor' : 'none'); document.getElementById('mpFav').style.color = isFav ? 'var(--accent-bright)' : ''; }
}

/* Music queue — enhanced panel */
function musicAddToQueue(trackId) {
  const track = (musicState.lastList || []).find(t => t.id === trackId);
  if (!track) return;
  musicState.queue.push(track);
  toast('Added to queue', 'success');
  renderMusicQueue();
  openMusicQueue();
}
function musicShowQueue() {
  openMusicQueue();
}
function openMusicQueue() {
  document.getElementById('musicQueuePanel').classList.add('show');
  renderMusicQueue();
}
function closeMusicQueue() {
  document.getElementById('musicQueuePanel').classList.remove('show');
}
function renderMusicQueue() {
  const body = document.getElementById('mqBody');
  if (!body) return;
  const cur = musicState.currentTrack;
  const list = musicState.lastList || [];
  const queue = musicState.queue || [];
  let html = '';
  if (cur) {
    html += `<div class="mq-section">Now playing</div>`;
    html += renderMqItem(cur, -1, true);
  }
  if (list.length) {
    html += `<div class="mq-section">Up next</div>`;
    list.slice(0, 30).forEach((t, i) => {
      if (cur && t.id === cur.id) return;
      html += renderMqItem(t, i, false);
    });
  }
  if (queue.length) {
    html += `<div class="mq-section">Manually queued</div>`;
    queue.forEach((t, i) => {
      html += renderMqItem(t, i, false, true);
    });
  }
  if (!cur && !list.length && !queue.length) {
    html = '<div class="mq-empty">Queue is empty — play a track to start.</div>';
  }
  body.innerHTML = html;
}
function renderMqItem(t, i, isCurrent, isManual) {
  const art = musicArtUrl(t);
  const artist = t.user?.name || 'Unknown';
  const onclick = isManual ? `onclick="musicPlayTrack(musicState.queue[${i}])"` : (isCurrent ? '' : `onclick="musicPlayTrack(musicState.lastList[${i}])"`);
  const removeBtn = isManual ? `<button class="mq-remove" title="Remove" onclick="event.stopPropagation();musicState.queue.splice(${i},1);renderMusicQueue()">×</button>` : '';
  return `<div class="mq-item ${isCurrent ? 'current' : ''}" ${onclick}>
    ${art ? `<img src="${art}" loading="lazy" onerror="this.style.display='none'">` : '<div style="width:40px;height:40px;border-radius:6px;background:rgba(220,20,60,0.15);flex-shrink:0;"></div>'}
    <div class="mq-info">
      <div class="mq-title">${escapeHtml(t.title || 'Untitled')}</div>
      <div class="mq-artist">${escapeHtml(artist)}</div>
    </div>
    ${removeBtn}
  </div>`;
}

/* Keyboard: music */
document.addEventListener('keydown', (e) => {
  if (currentTab !== 'music') return;
  if (e.target.tagName === 'INPUT' || e.target.tagName === 'TEXTAREA') return;
  if (e.code === 'Space') { e.preventDefault(); musicToggle(); }
  else if (e.code === 'ArrowRight' && e.shiftKey) musicNext();
  else if (e.code === 'ArrowLeft' && e.shiftKey) musicPrev();
});

/* ============================================================
   CHAT ROOMS — local BroadcastChannel + public embeds
   ============================================================ */
const CHAT_PUBLIC_ROOMS = [
  { title: "Libera · #general", desc: "Kiwi IRC · Libera.Chat", url: "https://kiwiirc.com/nextclient/irc.libera.chat/#general", icon: "💬" },
  { title: "Libera · #javascript", desc: "Dev chat on Libera", url: "https://kiwiirc.com/nextclient/irc.libera.chat/#javascript", icon: "💻" },
  { title: "Libera · #gaming", desc: "Gaming discussion", url: "https://kiwiirc.com/nextclient/irc.libera.chat/#gaming", icon: "🎮" },
  { title: "OFTC · #debian", desc: "OFTC network", url: "https://kiwiirc.com/nextclient/irc.oftc.net/#debian", icon: "🐧" },
  { title: "TiTAN Network", desc: "Public web IRC", url: "https://kiwiirc.com/nextclient/irc.titanirc.net/#general", icon: "🌐" },
  { title: "IRCnet sample", desc: "Kiwi public client", url: "https://kiwiirc.com/nextclient/", icon: "📡" }
];

let chatChannel = null;
let chatChannelName = 'xorwel-general';
let chatMyId = localStorage.getItem('xorwel_chat_id') || ('u' + Math.random().toString(36).slice(2, 10));
localStorage.setItem('xorwel_chat_id', chatMyId);

function showChatView(view) {
  document.querySelectorAll('[data-chat-view]').forEach(b => b.classList.toggle('active', b.getAttribute('data-chat-view') === view));
  document.getElementById('chatViewLocal').style.display = view === 'local' ? 'block' : 'none';
  document.getElementById('chatViewRooms').style.display = view === 'rooms' ? 'block' : 'none';
  document.getElementById('chatViewCustom').style.display = view === 'custom' ? 'block' : 'none';
  if (view === 'local') initLocalChat();
  if (view === 'rooms') renderChatRooms();
}

function renderChatRooms() {
  const grid = document.getElementById('chatRoomsGrid');
  if (!grid) return;
  grid.innerHTML = '';
  CHAT_PUBLIC_ROOMS.forEach((room, i) => {
    const card = document.createElement('div');
    card.className = 'card';
    card.style.cursor = 'pointer';
    card.innerHTML = `
      <div class="poster" style="aspect-ratio:1.4;display:flex;align-items:center;justify-content:center;font-size:2.5rem;background:linear-gradient(160deg,rgba(220,20,60,0.2),rgba(0,0,0,0.4));">
        ${room.icon}
      </div>
      <h3>${escapeHtml(room.title)}</h3>
      <div class="meta">${escapeHtml(room.desc)}</div>
      <div class="card-actions">
        <button onclick="event.stopPropagation();openChatRoom('${room.url.replace(/'/g,"\\'")}','${room.title.replace(/'/g,"\\'")}')">Join</button>
        <button onclick="event.stopPropagation();window.open('${room.url.replace(/'/g,"\\'")}','_blank')">↗</button>
      </div>`;
    card.addEventListener('click', () => openChatRoom(room.url, room.title));
    grid.appendChild(card);
  });
}

function openChatRoom(url, title) {
  document.getElementById('chatEmbedWrap').style.display = 'block';
  document.getElementById('chatEmbedTitle').textContent = title || 'Room';
  document.getElementById('chatEmbedFrame').src = url;
  toast('Joined ' + (title || 'room'), 'success');
}
function closeChatEmbed() {
  document.getElementById('chatEmbedWrap').style.display = 'none';
  document.getElementById('chatEmbedFrame').src = '';
}
function openCustomChat() {
  const url = document.getElementById('chatCustomUrl').value.trim();
  if (!url) { toast('Paste a URL', 'warning'); return; }
  showChatView('rooms');
  openChatRoom(url, 'Custom');
}
function openCustomChatTab() {
  const url = document.getElementById('chatCustomUrl').value.trim();
  if (!url) { toast('Paste a URL', 'warning'); return; }
  window.open(url, '_blank', 'noopener');
}

function getChatNick() {
  const el = document.getElementById('chatNick');
  let nick = (el && el.value.trim()) || localStorage.getItem('xorwel_chat_nick') || '';
  if (!nick) {
    nick = 'User' + chatMyId.slice(-4);
    if (el) el.value = nick;
  }
  localStorage.setItem('xorwel_chat_nick', nick);
  return nick;
}

function initLocalChat() {
  const sel = document.getElementById('chatChannel');
  chatChannelName = (sel && sel.value) || 'xorwel-general';
  const nickEl = document.getElementById('chatNick');
  if (nickEl && !nickEl.value) nickEl.value = localStorage.getItem('xorwel_chat_nick') || '';
  if (nickEl) nickEl.onchange = () => localStorage.setItem('xorwel_chat_nick', nickEl.value.trim());

  if (chatChannel) {
    try { chatChannel.close(); } catch (e) {}
  }
  try {
    chatChannel = new BroadcastChannel(chatChannelName);
    chatChannel.onmessage = (ev) => handleChatMessage(ev.data);
  } catch (e) {
    appendChatSystem('BroadcastChannel not supported in this browser.');
    return;
  }
  const box = document.getElementById('chatMessages');
  if (box && !box.dataset.welcomed) {
    box.innerHTML = '';
    appendChatSystem('Connected to #' + chatChannelName.replace('xorwel-', '') + ' — open another tab to chat with yourself.');
    box.dataset.welcomed = '1';
  }
  broadcastChat({ type: 'join', nick: getChatNick(), id: chatMyId });
  updateChatPresence('Connected');
}

function switchChatChannel() {
  const box = document.getElementById('chatMessages');
  if (box) { box.innerHTML = ''; box.dataset.welcomed = ''; }
  initLocalChat();
}

function broadcastChat(payload) {
  if (!chatChannel) return;
  try {
    chatChannel.postMessage({ ...payload, ts: Date.now() });
  } catch (e) {}
}

function handleChatMessage(data) {
  if (!data || !data.type) return;
  if (data.type === 'chat' && data.id !== chatMyId) {
    appendChatBubble(data.nick || 'User', data.text, false);
  } else if (data.type === 'join' && data.id !== chatMyId) {
    appendChatSystem((data.nick || 'Someone') + ' joined');
  } else if (data.type === 'leave' && data.id !== chatMyId) {
    appendChatSystem((data.nick || 'Someone') + ' left');
  }
}

function appendChatBubble(nick, text, isMe) {
  const box = document.getElementById('chatMessages');
  if (!box) return;
  const div = document.createElement('div');
  div.className = 'chat-bubble ' + (isMe ? 'me' : 'other');
  const time = new Date().toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' });
  div.innerHTML = `<div class="chat-meta">${escapeHtml(nick)} · ${time}</div>${escapeHtml(text)}`;
  box.appendChild(div);
  box.scrollTop = box.scrollHeight;
}

function appendChatSystem(text) {
  const box = document.getElementById('chatMessages');
  if (!box) return;
  const div = document.createElement('div');
  div.className = 'chat-bubble system';
  div.textContent = text;
  box.appendChild(div);
  box.scrollTop = box.scrollHeight;
}

function sendChatMessage() {
  const input = document.getElementById('chatInput');
  const text = (input && input.value.trim()) || '';
  if (!text) return;
  const nick = getChatNick();
  appendChatBubble(nick, text, true);
  broadcastChat({ type: 'chat', nick, text, id: chatMyId });
  input.value = '';
  input.focus();
}

function updateChatPresence(msg) {
  const el = document.getElementById('chatPresence');
  if (el) el.textContent = msg || '';
}


/* ============================================================
   DASHBOARD — weather, notes, continue watching
   ============================================================ */
const WMO_CODES = {
  0: ['Clear', '☀️'], 1: ['Mainly clear', '🌤️'], 2: ['Partly cloudy', '⛅'], 3: ['Overcast', '☁️'],
  45: ['Fog', '🌫️'], 48: ['Rime fog', '🌫️'], 51: ['Light drizzle', '🌦️'], 53: ['Drizzle', '🌦️'],
  55: ['Heavy drizzle', '🌧️'], 61: ['Light rain', '🌧️'], 63: ['Rain', '🌧️'], 65: ['Heavy rain', '🌧️'],
  71: ['Light snow', '🌨️'], 73: ['Snow', '❄️'], 75: ['Heavy snow', '❄️'], 77: ['Snow grains', '🌨️'],
  80: ['Rain showers', '🌦️'], 81: ['Rain showers', '🌧️'], 82: ['Violent showers', '⛈️'],
  85: ['Snow showers', '🌨️'], 86: ['Heavy snow showers', '❄️'], 95: ['Thunderstorm', '⛈️'],
  96: ['Thunder + hail', '⛈️'], 99: ['Heavy thunder + hail', '⛈️']
};

function saveContinueWatching(data, sourceName, url) {
  try {
    const key = getTitleKey(data);
    let list = JSON.parse(localStorage.getItem('xorwel_continue') || '[]');
    list = list.filter(x => x.key !== key);
    let poster = '';
    const found = (typeof allTVItems !== 'undefined' ? allTVItems : []).find(i => {
      if (data.type === 'anime') return String(i.mal_id) === String(data.malId || data.tmdbId);
      return String(i.id) === String(data.tmdbId) && i.media_type === data.type;
    });
    if (found) {
      if (found.poster_path) poster = 'https://image.tmdb.org/t/p/w300' + found.poster_path;
      else if (found.images?.jpg?.image_url) poster = found.images.jpg.image_url;
    }
    list.unshift({
      key,
      title: data.title || 'Untitled',
      type: data.type,
      tmdbId: data.tmdbId || '',
      imdbId: data.imdbId || '',
      malId: data.malId || '',
      season: data.season || null,
      episode: data.episode || null,
      sourceName: sourceName || '',
      url: url || '',
      poster,
      ts: Date.now()
    });
    localStorage.setItem('xorwel_continue', JSON.stringify(list.slice(0, 24)));
  } catch (e) {}
  renderContinueWatching();
  updateDashStats();
}

function renderContinueWatching() {
  const row = document.getElementById('continueWatchRow');
  if (!row) return;
  let list = [];
  try { list = JSON.parse(localStorage.getItem('xorwel_continue') || '[]'); } catch (e) {}
  try {
    const cache = JSON.parse(localStorage.getItem('xorwel_source_cache') || '{}');
    Object.keys(cache).forEach(key => {
      if (list.some(x => x.key === key)) return;
      const parts = key.split('_');
      if (parts.length < 2) return;
      const type = parts[0];
      const id = parts[1];
      if (!id || id === 'undefined') return;
      list.push({
        key, type, tmdbId: type !== 'anime' ? id : '', malId: type === 'anime' ? id : '',
        title: type + ' · ' + id, season: parts[2] || null, episode: parts[3] || null,
        sourceName: (cache[key] || [])[0] || '', poster: '', ts: 0
      });
    });
  } catch (e) {}

  if (!list.length) {
    row.innerHTML = '<div style="padding:0.75rem;color:var(--text-dim);font-size:0.85rem;">Play a movie or show — working sources are remembered here.</div>';
    return;
  }
  list.sort((a, b) => (b.ts || 0) - (a.ts || 0));
  row.innerHTML = list.slice(0, 16).map(item => {
    const sub = [
      item.type,
      item.season != null && item.season !== 'null' ? `S${item.season}E${item.episode || 1}` : null,
      item.sourceName ? item.sourceName : null
    ].filter(Boolean).join(' · ');
    return `<div class="continue-item" onclick="resumeContinueWatch('${item.key.replace(/'/g,"\\'")}')" title="${escapeHtml(item.title || '')}">
      ${item.poster ? `<img src="${item.poster}" loading="lazy" onerror="this.style.opacity=0.3">` : `<div style="aspect-ratio:2/3;display:flex;align-items:center;justify-content:center;background:rgba(220,20,60,0.15);font-size:1.5rem;">▶</div>`}
      <div class="ci-info">
        <div class="ci-title">${escapeHtml(item.title || 'Title')}</div>
        <div class="ci-sub">${escapeHtml(sub)}</div>
      </div>
    </div>`;
  }).join('');
}

function resumeContinueWatch(key) {
  let list = [];
  try { list = JSON.parse(localStorage.getItem('xorwel_continue') || '[]'); } catch (e) {}
  const item = list.find(x => x.key === key);
  if (!item) {
    const parts = key.split('_');
    playTVItem(parts[0], parts[1], '', parts[0] + ' ' + parts[1], parts[0] === 'anime' ? parts[1] : '', parts[2], parts[3]);
    return;
  }
  if (item.url && item.sourceName) {
    currentPlayData = {
      tmdbId: item.tmdbId, imdbId: item.imdbId, malId: item.malId,
      type: item.type, season: item.season, episode: item.episode, title: item.title
    };
    openPlayer(item.url);
    document.getElementById('currentSourceLabel').textContent = 'Source: ' + (item.sourceName || 'cached');
    toast('Resuming ' + item.title, 'success');
  } else {
    playTVItem(
      item.type,
      item.type === 'anime' ? item.malId : item.tmdbId,
      item.imdbId || '',
      item.title,
      item.malId || '',
      item.season,
      item.episode
    );
  }
}

/* Notes */
function loadNotes() {
  const area = document.getElementById('notesArea');
  if (!area) return;
  try { area.value = localStorage.getItem('xorwel_notes') || ''; } catch (e) {}
}
function saveNotes() {
  const area = document.getElementById('notesArea');
  if (!area) return;
  try {
    localStorage.setItem('xorwel_notes', area.value);
    const st = document.getElementById('notesStatus');
    if (st) {
      st.textContent = 'Saved · ' + new Date().toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' });
    }
  } catch (e) {}
}
function clearNotes() {
  const area = document.getElementById('notesArea');
  if (area) area.value = '';
  localStorage.removeItem('xorwel_notes');
  const st = document.getElementById('notesStatus');
  if (st) st.textContent = 'Cleared';
  toast('Notes cleared', 'info');
}

/* Weather — Open-Meteo */
async function refreshWeather() {
  const body = document.getElementById('weatherBody');
  if (!body) return;
  body.innerHTML = '<div class="loading"><div class="spinner"></div>Loading weather…</div>';
  const saved = localStorage.getItem('xorwel_weather_loc');
  if (saved) {
    try {
      const loc = JSON.parse(saved);
      await fetchWeatherAt(loc.lat, loc.lon, loc.name);
      return;
    } catch (e) {}
  }
  if (navigator.geolocation) {
    navigator.geolocation.getCurrentPosition(
      async (pos) => {
        await fetchWeatherAt(pos.coords.latitude, pos.coords.longitude, 'Your location');
      },
      async () => {
        await fetchWeatherAt(40.71, -74.01, 'New York');
      },
      { timeout: 8000 }
    );
  } else {
    await fetchWeatherAt(40.71, -74.01, 'New York');
  }
}

async function searchWeatherCity() {
  const q = (document.getElementById('weatherCity')?.value || '').trim();
  if (!q) { toast('Enter a city name', 'warning'); return; }
  const body = document.getElementById('weatherBody');
  if (body) body.innerHTML = '<div class="loading"><div class="spinner"></div>Searching…</div>';
  try {
    const res = await fetch(`https://geocoding-api.open-meteo.com/v1/search?name=${encodeURIComponent(q)}&count=1&language=en&format=json`);
    const data = await res.json();
    if (!data.results || !data.results.length) {
      if (body) body.innerHTML = '<div style="color:var(--text-dim);">City not found</div>';
      toast('City not found', 'warning');
      return;
    }
    const r = data.results[0];
    const name = [r.name, r.admin1, r.country_code].filter(Boolean).join(', ');
    await fetchWeatherAt(r.latitude, r.longitude, name);
  } catch (e) {
    if (body) body.innerHTML = '<div style="color:var(--text-dim);">Lookup failed</div>';
    toast('Weather lookup failed', 'error');
  }
}

async function fetchWeatherAt(lat, lon, label) {
  const body = document.getElementById('weatherBody');
  try {
    localStorage.setItem('xorwel_weather_loc', JSON.stringify({ lat, lon, name: label }));
    const url = `https://api.open-meteo.com/v1/forecast?latitude=${lat}&longitude=${lon}&current=temperature_2m,relative_humidity_2m,weather_code,wind_speed_10m,apparent_temperature&temperature_unit=fahrenheit&wind_speed_unit=mph&timezone=auto`;
    const res = await fetch(url);
    const data = await res.json();
    const cur = data.current || {};
    const code = cur.weather_code;
    const info = WMO_CODES[code] || ['Unknown', '🌡️'];
    const temp = Math.round(cur.temperature_2m);
    const feels = Math.round(cur.apparent_temperature);
    const hum = cur.relative_humidity_2m;
    const wind = Math.round(cur.wind_speed_10m);
    if (body) {
      body.innerHTML = `
        <div class="weather-main">
          <div style="font-size:2.6rem;line-height:1;">${info[1]}</div>
          <div>
            <div class="weather-temp">${temp}°F</div>
            <div class="weather-meta">
              <strong style="color:var(--text);">${escapeHtml(label)}</strong><br>
              ${info[0]} · Feels ${feels}°F<br>
              Humidity ${hum}% · Wind ${wind} mph
            </div>
          </div>
        </div>`;
    }
  } catch (e) {
    if (body) body.innerHTML = '<div style="color:var(--text-dim);">Could not load weather</div>';
  }
}

function initDashboard() {
  loadNotes();
  renderContinueWatching();
  refreshWeather();
  updateDashStats();
}


/* ============================================================
   ADVANCED GLOBAL — command palette, scroll, net, data tools
   ============================================================ */
const CMD_ACTIONS = [
  { id: 'home', label: 'Go to Home', keys: 'Alt+1', run: () => switchTab('home') },
  { id: 'games', label: 'Go to Games', keys: 'Alt+2', run: () => switchTab('games') },
  { id: 'cloud', label: 'Go to Cloud Gaming', run: () => switchTab('cloud') },
  { id: 'tv', label: 'Go to TV & Movies', run: () => switchTab('tv') },
  { id: 'music', label: 'Go to Music', run: () => switchTab('music') },
  { id: 'proxy', label: 'Go to Proxy', run: () => switchTab('proxy') },
  { id: 'bookmarks', label: 'Go to Bookmarks', run: () => switchTab('bookmarks') },
  { id: 'chat', label: 'Go to Chat', run: () => switchTab('chat') },
  { id: 'ai', label: 'Go to AI', run: () => switchTab('ai') },
  { id: 'settings', label: 'Go to Settings', run: () => switchTab('settings') },
  { id: 'search', label: 'Open global search', keys: 'Ctrl+Space', run: () => openSearchOverlay() },
  { id: 'search-focus', label: 'Focus search', keys: '/', run: () => { document.getElementById('searchInput')?.focus(); openSearchOverlay(); } },
  { id: 'shortcuts', label: 'Show keyboard shortcuts', keys: '?', run: () => openShortcutsModal() },
  { id: 'weather', label: 'Refresh weather', run: () => { switchTab('home'); refreshWeather(); } },
  { id: 'watchlist', label: 'Open watchlist', run: () => { switchTab('tv'); showWatchlist(); } },
  { id: 'export', label: 'Export all data (JSON)', run: () => exportAllData() },
  { id: 'import', label: 'Import data (JSON file)', run: () => document.getElementById('importDataInput')?.click() },
  { id: 'theme-dark', label: 'Theme: Dark', run: () => setTheme('dark') },
  { id: 'theme-sakura', label: 'Theme: Sakura', run: () => setTheme('sakura') },
  { id: 'theme-tokyo', label: 'Theme: Tokyo Night', run: () => setTheme('tokyo') },
  { id: 'fullscreen', label: 'Toggle fullscreen', run: () => togglePageFullscreen() },
  { id: 'clear-cache', label: 'Clear source cache', run: () => { localStorage.removeItem('xorwel_source_cache'); toast('Source cache cleared', 'info'); } },
  { id: 'queue', label: 'Open music queue', run: () => { switchTab('music'); openMusicQueue(); } },
  { id: 'pip', label: 'Toggle PiP', run: () => togglePlayerPiP() },
];

let cmdIndex = 0;
let cmdFiltered = [];

function openCmdPalette() {
  const el = document.getElementById('cmdPalette');
  if (!el) return;
  el.classList.add('show');
  const input = document.getElementById('cmdInput');
  if (input) { input.value = ''; input.focus(); }
  filterCmdPalette();
}
function closeCmdPalette() {
  document.getElementById('cmdPalette')?.classList.remove('show');
}
function filterCmdPalette() {
  const q = (document.getElementById('cmdInput')?.value || '').toLowerCase().trim();
  cmdFiltered = CMD_ACTIONS.filter(a => !q || a.label.toLowerCase().includes(q) || a.id.includes(q));
  cmdIndex = 0;
  renderCmdResults();
}
function renderCmdResults() {
  const box = document.getElementById('cmdResults');
  if (!box) return;
  if (!cmdFiltered.length) {
    box.innerHTML = '<div style="padding:1rem;color:var(--text-dim);font-size:0.85rem;">No matching commands</div>';
    return;
  }
  box.innerHTML = cmdFiltered.map((a, i) =>
    `<button class="cmd-item ${i === cmdIndex ? 'active' : ''}" data-i="${i}" onclick="runCmd(${i})">
      <span>${escapeHtml(a.label)}</span>
      ${a.keys ? `<kbd>${escapeHtml(a.keys)}</kbd>` : ''}
    </button>`
  ).join('');
}
function runCmd(i) {
  const a = cmdFiltered[i];
  closeCmdPalette();
  if (a && typeof a.run === 'function') {
    try { a.run(); } catch (e) { console.warn(e); }
  }
}
function cmdKey(e) {
  if (e.key === 'ArrowDown') { e.preventDefault(); cmdIndex = Math.min(cmdIndex + 1, cmdFiltered.length - 1); renderCmdResults(); }
  else if (e.key === 'ArrowUp') { e.preventDefault(); cmdIndex = Math.max(cmdIndex - 1, 0); renderCmdResults(); }
  else if (e.key === 'Enter') { e.preventDefault(); runCmd(cmdIndex); }
  else if (e.key === 'Escape') { closeCmdPalette(); }
}

function togglePageFullscreen() {
  if (!document.fullscreenElement) {
    document.documentElement.requestFullscreen?.().catch(() => {});
  } else {
    document.exitFullscreen?.();
  }
}

function exportAllData() {
  const dump = {};
  for (let i = 0; i < localStorage.length; i++) {
    const k = localStorage.key(i);
    if (k && k.startsWith('xorwel')) dump[k] = localStorage.getItem(k);
  }
  const blob = new Blob([JSON.stringify(dump, null, 2)], { type: 'application/json' });
  const a = document.createElement('a');
  a.href = URL.createObjectURL(blob);
  a.download = 'xorwel-backup-' + new Date().toISOString().slice(0, 10) + '.json';
  a.click();
  URL.revokeObjectURL(a.href);
  toast('Data exported', 'success');
}

function importAllData(file) {
  if (!file) return;
  const reader = new FileReader();
  reader.onload = () => {
    try {
      const data = JSON.parse(reader.result);
      Object.entries(data).forEach(([k, v]) => {
        if (k.startsWith('xorwel')) localStorage.setItem(k, v);
      });
      toast('Data imported — reloading…', 'success');
      setTimeout(() => location.reload(), 800);
    } catch (e) {
      toast('Invalid backup file', 'error');
    }
  };
  reader.readAsText(file);
}

function updateScrollProgress() {
  const el = document.getElementById('scrollProgress');
  if (!el) return;
  const scrollTop = window.scrollY || document.documentElement.scrollTop;
  const height = document.documentElement.scrollHeight - window.innerHeight;
  const pct = height > 0 ? (scrollTop / height) * 100 : 0;
  el.style.width = pct + '%';
  const btn = document.getElementById('scrollTopBtn');
  if (btn) btn.classList.toggle('show', scrollTop > 400);
}

function updateNetStatus() {
  const el = document.getElementById('netStatus');
  const tx = document.getElementById('netStatusText');
  if (!el) return;
  if (!navigator.onLine) {
    el.classList.add('offline');
    el.classList.remove('online-flash');
    if (tx) tx.textContent = 'Offline';
  } else {
    el.classList.remove('offline');
    el.classList.add('online-flash');
    if (tx) tx.textContent = 'Back online';
    setTimeout(() => el.classList.remove('online-flash'), 2500);
  }
}

function updateDashStats() {
  const el = document.getElementById('dashStats');
  if (!el) return;
  let cont = 0, wl = 0, bm = 0, notes = 0;
  try { cont = JSON.parse(localStorage.getItem('xorwel_continue') || '[]').length; } catch (e) {}
  try { wl = JSON.parse(localStorage.getItem('xorwel_watchlist') || '[]').length; } catch (e) {}
  try { bm = (getData('bookmarks', []) || []).length; } catch (e) {}
  try { notes = (localStorage.getItem('xorwel_notes') || '').length; } catch (e) {}
  const plays = getData('playtime', 0) || 0;
  el.innerHTML = `
    <span class="dash-stat"><strong>${cont}</strong> continue</span>
    <span class="dash-stat"><strong>${wl}</strong> watchlist</span>
    <span class="dash-stat"><strong>${bm}</strong> bookmarks</span>
    <span class="dash-stat"><strong>${plays}</strong> streams</span>
    <span class="dash-stat"><strong>${notes}</strong> note chars</span>
  `;
}

const cardObserver = (typeof IntersectionObserver !== 'undefined')
  ? new IntersectionObserver((entries) => {
      entries.forEach(en => {
        if (en.isIntersecting) {
          en.target.classList.add('reveal');
          cardObserver.unobserve(en.target);
        }
      });
    }, { rootMargin: '40px', threshold: 0.08 })
  : null;

function observeCards(root) {
  if (!cardObserver) return;
  (root || document).querySelectorAll('.card:not(.reveal)').forEach(c => cardObserver.observe(c));
}

const gridObserver = (typeof MutationObserver !== 'undefined')
  ? new MutationObserver(() => observeCards(document))
  : null;
if (gridObserver) {
  document.addEventListener('DOMContentLoaded', () => {
    const main = document.querySelector('.main-content') || document.body;
    gridObserver.observe(main, { childList: true, subtree: true });
  });
}

function initAdvancedUI() {
  window.addEventListener('scroll', updateScrollProgress, { passive: true });
  window.addEventListener('online', updateNetStatus);
  window.addEventListener('offline', updateNetStatus);
  updateNetStatus();
  updateScrollProgress();
  updateDashStats();
  if (!document.getElementById('importDataInput')) {
    const inp = document.createElement('input');
    inp.type = 'file';
    inp.accept = 'application/json,.json';
    inp.id = 'importDataInput';
    inp.style.display = 'none';
    inp.onchange = () => importAllData(inp.files[0]);
    document.body.appendChild(inp);
  }
  // render shortcuts
  renderShortcuts();
  // recent bar
  renderRecentBar();
  // render proxy tabs
  renderProxyTabs();
}

/* ============================================================
   ENHANCEMENT — Keyboard shortcuts modal, FAB, recent bar, toasts w/ actions
   ============================================================ */
const SHORTCUTS = [
  { cat: 'Navigation', items: [
    { label: 'Go to Home', keys: ['Alt','1'] },
    { label: 'Go to Games', keys: ['Alt','2'] },
    { label: 'Go to Cloud', keys: ['Alt','3'] },
    { label: 'Go to TV & Movies', keys: ['Alt','4'] },
    { label: 'Go to Music', keys: ['Alt','5'] },
    { label: 'Go to Proxy', keys: ['Alt','6'] },
    { label: 'Go to Bookmarks', keys: ['Alt','7'] },
    { label: 'Go to Chat', keys: ['Alt','8'] },
    { label: 'Go to AI', keys: ['Alt','9'] },
    { label: 'Go to Settings', keys: ['Alt','0'] },
  ]},
  { cat: 'Search & Commands', items: [
    { label: 'Command palette', keys: ['Ctrl','K'] },
    { label: 'Global search', keys: ['Ctrl','Space'] },
    { label: 'Focus search', keys: ['/'] },
    { label: 'Keyboard shortcuts', keys: ['?'] },
    { label: 'Close dialog', keys: ['Esc'] },
  ]},
  { cat: 'Music', items: [
    { label: 'Play / pause', keys: ['Space'] },
    { label: 'Next track', keys: ['Shift','→'] },
    { label: 'Previous track', keys: ['Shift','←'] },
  ]},
  { cat: 'Player', items: [
    { label: 'Skip source', keys: ['S'] },
    { label: 'Retry all sources', keys: ['R'] },
    { label: 'Toggle captions', keys: ['C'] },
    { label: 'Picture in Picture', keys: ['P'] },
  ]},
];

function renderShortcuts() {
  const grid = document.getElementById('shortcutsGrid');
  if (!grid) return;
  grid.innerHTML = SHORTCUTS.map(sec => `
    <div class="shortcut-category">${escapeHtml(sec.cat)}</div>
    ${sec.items.map(it => `
      <div class="shortcut-row">
        <span>${escapeHtml(it.label)}</span>
        <span class="shortcut-keys">${it.keys.map(k => `<kbd>${escapeHtml(k)}</kbd>`).join('')}</span>
      </div>
    `).join('')}
  `).join('');
}
function openShortcutsModal() {
  document.getElementById('shortcutsModal')?.classList.add('show');
}
function closeShortcutsModal() {
  document.getElementById('shortcutsModal')?.classList.remove('show');
}

/* FAB */
function toggleFab() {
  document.getElementById('fabMenu')?.classList.toggle('open');
}
function closeFab() {
  document.getElementById('fabMenu')?.classList.remove('open');
}
document.addEventListener('click', (e) => {
  const fab = document.getElementById('fabMenu');
  if (fab && fab.classList.contains('open') && !fab.contains(e.target)) fab.classList.remove('open');
});

/* Recent bar */
const RECENT_MAX = 6;
function getRecentItems() {
  try { return JSON.parse(localStorage.getItem('xorwel_recent_bar') || '[]'); } catch (e) { return []; }
}
function pushRecentItem(title, type, onClick) {
  if (!title) return;
  try {
    let list = getRecentItems();
    list = list.filter(x => !(x.title === title && x.type === type));
    list.unshift({ title, type, ts: Date.now(), id: 'r_' + Math.random().toString(36).slice(2, 8) });
    list = list.slice(0, RECENT_MAX);
    localStorage.setItem('xorwel_recent_bar', JSON.stringify(list));
    renderRecentBar();
  } catch (e) {}
}
function renderRecentBar() {
  const bar = document.getElementById('recentBar');
  const items = document.getElementById('recentBarItems');
  if (!bar || !items) return;
  const list = getRecentItems();
  if (!list.length) { bar.classList.remove('show'); return; }
  items.innerHTML = '';
  list.slice(0, 5).forEach(item => {
    const b = document.createElement('button');
    b.className = 'rb-item';
    b.title = item.title;
    b.textContent = item.title;
    b.onclick = () => {
      if (item.type === 'games') { switchTab('games'); }
      else if (item.type === 'tv') { switchTab('tv'); }
      else if (item.type === 'music') { switchTab('music'); }
    };
    items.appendChild(b);
  });
  if (window.scrollY > 200) bar.classList.add('show');
}
function hideRecentBar() {
  document.getElementById('recentBar')?.classList.remove('show');
  try { localStorage.removeItem('xorwel_recent_bar'); } catch (e) {}
}
window.addEventListener('scroll', () => {
  const bar = document.getElementById('recentBar');
  if (!bar) return;
  const list = getRecentItems();
  if (!list.length) return;
  if (window.scrollY > 200) bar.classList.add('show');
  else bar.classList.remove('show');
}, { passive: true });

/* Toast with actions */
function toastAction(message, type = 'info', actions = [], timeout = 5200) {
  const c = document.getElementById('toastContainer');
  if (!c) { return toast(message, type, timeout); }
  const el = document.createElement('div');
  el.className = 'toast ' + type;
  const icons = { info:'#i-info', success:'#i-check', error:'#i-alert', warning:'#i-alert' };
  el.innerHTML = `
    <div class="toast-row">
      <svg viewBox="0 0 24 24" style="width:18px;height:18px;flex-shrink:0;stroke:currentColor;fill:none;stroke-width:2;"><use href="${icons[type]}"/></svg>
      <span>${escapeHtml(message)}</span>
    </div>
    ${actions.length ? `<div class="toast-actions">${actions.map((a, i) => `<button class="toast-action-btn" data-i="${i}">${escapeHtml(a.label)}</button>`).join('')}</div>` : ''}
  `;
  c.appendChild(el);
  requestAnimationFrame(() => el.classList.add('show'));
  el.querySelectorAll('.toast-action-btn').forEach((btn, i) => {
    btn.onclick = () => {
      try { actions[i].run(); } catch(e){}
      el.classList.remove('show');
      setTimeout(() => el.remove(), 400);
    };
  });
  setTimeout(() => { el.classList.remove('show'); setTimeout(() => el.remove(), 400); }, timeout);
}


function switchTab(name) {
  document.querySelectorAll('.nav-item').forEach(t => t.classList.remove('active'));
  document.querySelectorAll('.content-section').forEach(s => s.classList.remove('active'));
  const nav = document.querySelector(`.nav-item[data-tab="${name}"]`);
  if (nav) nav.classList.add('active');
  const sec = document.getElementById('tab-' + name);
  if (sec) sec.classList.add('active');
  if (name === 'games' && document.getElementById('gameGrid').children.length === 0) loadGames();
  if (name === 'cloud' && document.getElementById('cloudGrid').children.length === 0) renderCloudGames();
  if (name === 'tv' && document.getElementById('tvGrid').children.length === 0) loadTV();
  if (name === 'bookmarks') renderBookmarks();
  if (name === 'account') { refreshAccountUI(); updateAccountStats(); }
  if (name === 'music') {
    musicInit();
    document.getElementById('musicPlayer').classList.add('visible');
  } else {
    document.getElementById('musicPlayer').classList.remove('visible');
  }
  if (name === 'ai') {
    initAI();
  }
  if (name === 'chat') {
    showChatView('local');
  }
  if (name === 'home') {
    initDashboard();
  }
  if (window.innerWidth < 900) document.getElementById('sidebar').classList.remove('open');
  currentTab = name;
}
function goHome() { switchTab('home'); }

document.querySelectorAll('.nav-item').forEach(item => {
  item.addEventListener('click', () => switchTab(item.dataset.tab));
});
function toggleSidebar() { document.getElementById('sidebar').classList.toggle('open'); }

/* ============================================================
   CLOCK + TOASTS
   ============================================================ */
function formatTime(d) { return d.toLocaleTimeString('en-GB', { hour:'2-digit', minute:'2-digit', second:'2-digit', hour12:false }); }
function updateClock() { document.getElementById('clockDisplay').textContent = formatTime(new Date()); }
setInterval(updateClock, 1000);
updateClock();

function toast(msg, type = 'info', timeout = 3200) {
  const c = document.getElementById('toastContainer');
  const el = document.createElement('div');
  el.className = 'toast ' + type;
  const icons = { info:'#i-info', success:'#i-check', error:'#i-alert', warning:'#i-alert' };
  el.innerHTML = `<div class="toast-row"><svg viewBox="0 0 24 24" style="width:18px;height:18px;flex-shrink:0;stroke:currentColor;fill:none;stroke-width:2;"><use href="${icons[type]}"/></svg><span>${escapeHtml(msg)}</span></div>`;
  c.appendChild(el);
  requestAnimationFrame(() => el.classList.add('show'));
  setTimeout(() => { el.classList.remove('show'); setTimeout(() => el.remove(), 400); }, timeout);
}

/* ============================================================
   INIT
   ============================================================ */

document.addEventListener('keydown', (e) => {
  const tag = (e.target.tagName || '').toLowerCase();
  const typing = tag === 'input' || tag === 'textarea' || e.target.isContentEditable;

  if (!typing && currentTab === 'music') {
    if (e.code === 'Space') { e.preventDefault(); musicToggle(); }
    else if (e.code === 'ArrowRight' && e.shiftKey) { e.preventDefault(); musicNext(); }
    else if (e.code === 'ArrowLeft' && e.shiftKey) { e.preventDefault(); musicPrev(); }
  }

  if (e.key === 'k' && (e.metaKey || e.ctrlKey)) {
    e.preventDefault();
    openCmdPalette();
    return;
  }
  if (e.code === 'Space' && (e.metaKey || e.ctrlKey)) {
    e.preventDefault();
    openSearchOverlay();
    return;
  }
  if (e.key === 'Escape') {
    if (document.getElementById('searchOverlay')?.classList.contains('show')) { closeSearchOverlay(); return; }
    if (document.getElementById('shortcutsModal')?.classList.contains('show')) { closeShortcutsModal(); return; }
    if (document.getElementById('tvDetailModal')?.style.display === 'flex') { closeTvDetail(); return; }
    if (document.getElementById('playerModal').classList.contains('show')) closePlayer();
    if (document.getElementById('sourceTestHUD').classList.contains('show')) cancelSourceTest();
    if (document.getElementById('sidebar').classList.contains('open')) document.getElementById('sidebar').classList.remove('open');
    if (document.getElementById('cmdPalette')?.classList.contains('show')) closeCmdPalette();
    if (document.getElementById('musicQueuePanel')?.classList.contains('show')) closeMusicQueue();
  }
  if (!typing) {
    if (e.key === '/' && !e.metaKey && !e.ctrlKey) {
      e.preventDefault();
      openSearchOverlay();
    }
    if (e.key === '?') { e.preventDefault(); openShortcutsModal(); }
    if (e.altKey && e.key >= '0' && e.key <= '9') {
      const tabs = ['home','games','cloud','tv','music','proxy','bookmarks','chat','ai','settings'];
      const i = e.key === '0' ? 9 : parseInt(e.key, 10) - 1;
      if (tabs[i]) { e.preventDefault(); switchTab(tabs[i]); }
    }
    if (e.key === 's' && currentTab === 'tv') { skipCurrentSource(); }
    if (e.key === 'r' && currentTab === 'tv') { tryAllSourcesAgain(); }
    if (e.key === 'c' && document.getElementById('playerModal').classList.contains('show')) { toggleCaptions(); }
    if (e.key === 'p' && document.getElementById('playerModal').classList.contains('show')) { togglePlayerPiP(); }
  }
});

/* Handle hash routes: #movie/123, #tv/456, #anime/789 */
function handleHashRoute() {
  const h = (location.hash || '').replace(/^#/, '');
  if (!h) return;
  const [type, id] = h.split('/');
  if (['movie','tv','anime'].includes(type) && id) {
    switchTab('tv');
    setTimeout(() => openTvDetailById(type, id), 400);
  }
}

(async function init() {
  buildThemeGrid();
  setTheme(getData('theme', 'dark'), false);
  const srcSel = document.getElementById('preferredSourceSelect');
  if (srcSel && srcSel.options.length <= 1) {
    EMBED_SOURCES.forEach(s => {
      const opt = document.createElement('option');
      opt.value = s.name;
      opt.textContent = s.name;
      srcSel.appendChild(opt);
    });
  }
  renderBookmarks();
  refreshAccountUI();
  initAdvancedUI();
  loadGames();
  initAI();
  initProxy();
  handleHashRoute();
  window.addEventListener('hashchange', handleHashRoute);
})();
</script>
</body>
</html>
