<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover">
<title>novusOS</title>
<style>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&display=swap');
:root {
  --bg: #0b0b10;
  --surface: rgba(255,255,255,0.04);
  --surface-solid: #15151c;
  --surface-hover: rgba(255,255,255,0.08);
  --text: #ffffff;
  --text-secondary: #b0b0c0;
  --accent: #6c8cff;
  --accent-hover: #8aa6ff;
  --danger: #ff4d6d;
  --success: #34c759;
  --warning: #ffcc00;
  --border: rgba(255,255,255,0.1);
  --radius: 16px;
  --radius-lg: 24px;
  --shadow: 0 8px 32px rgba(0,0,0,0.6);
  --glass-blur: blur(20px);
  --transition: 0.25s cubic-bezier(0.4, 0, 0.2, 1);
}
* { margin:0; padding:0; box-sizing:border-box; font-family:'Inter', -apple-system, sans-serif; }
body {
  background: var(--bg);
  color: var(--text);
  min-height: 100vh;
  padding: 24px 16px;
  overflow-x: hidden;
  transition: background 0.4s;
}
body::before {
  content: '';
  position: fixed;
  top: -50%;
  left: -50%;
  width: 200%;
  height: 200%;
  background: radial-gradient(circle at 30% 30%, rgba(108,140,255,0.08) 0%, transparent 60%);
  pointer-events: none;
  z-index: -1;
}
.container { max-width: 1500px; margin: 0 auto; }

.brand {
  text-align: center;
  font-size: 0.9rem;
  font-weight: 600;
  letter-spacing: 3px;
  text-transform: lowercase;
  color: var(--text-secondary);
  margin-bottom: 0.5rem;
  opacity: 0.8;
}
.clock-display {
  text-align: center;
  font-size: 4rem;
  font-weight: 800;
  letter-spacing: 4px;
  font-variant-numeric: tabular-nums;
  margin-bottom: 1.5rem;
  text-shadow: 0 0 20px rgba(108,140,255,0.3);
}

.search-wrapper {
  display: flex;
  justify-content: center;
  margin-bottom: 2rem;
}
.search-bar {
  display: flex;
  gap: 0.5rem;
  width: 100%;
  max-width: 700px;
  background: var(--surface);
  backdrop-filter: var(--glass-blur);
  -webkit-backdrop-filter: var(--glass-blur);
  border: 1px solid var(--border);
  border-radius: 50px;
  padding: 0.5rem 0.5rem 0.5rem 1.5rem;
  box-shadow: var(--shadow);
}
.search-bar input {
  flex: 1;
  background: transparent;
  border: none;
  color: var(--text);
  font-size: 1.1rem;
  outline: none;
}
.search-bar button {
  padding: 0.8rem 1.8rem;
  border-radius: 40px;
  border: none;
  background: var(--accent);
  color: #fff;
  font-weight: 600;
  cursor: pointer;
  transition: background var(--transition);
}
.search-bar button:hover { background: var(--accent-hover); }

.tabs {
  display: flex;
  justify-content: center;
  gap: 0.8rem;
  margin-bottom: 2rem;
  flex-wrap: wrap;
}
.tab {
  padding: 0.7rem 1.4rem;
  border-radius: 40px;
  cursor: pointer;
  background: var(--surface);
  border: 1px solid var(--border);
  color: var(--text);
  font-weight: 500;
  display: flex;
  align-items: center;
  gap: 0.4rem;
  transition: all var(--transition);
  font-size: 0.95rem;
  backdrop-filter: var(--glass-blur);
  -webkit-backdrop-filter: var(--glass-blur);
}
.tab:hover { background: var(--surface-hover); }
.tab.active { background: var(--accent); border-color: var(--accent); color: #fff; box-shadow: 0 0 20px rgba(108,140,255,0.4); }
.tab svg { width: 1.2em; height: 1.2em; }

.content-section { display: none; }
.content-section.active { display: block; animation: fadeIn 0.3s ease; }
@keyframes fadeIn { from { opacity:0; transform: translateY(10px); } to { opacity:1; transform: translateY(0); } }

.grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
  gap: 1.2rem;
}
.card {
  background: var(--surface);
  backdrop-filter: var(--glass-blur);
  -webkit-backdrop-filter: var(--glass-blur);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  padding: 1rem;
  text-align: center;
  cursor: pointer;
  transition: transform var(--transition), box-shadow var(--transition), background var(--transition);
  display: flex;
  flex-direction: column;
  gap: 0.6rem;
  overflow: hidden;
}
.card:hover {
  transform: translateY(-6px);
  box-shadow: 0 12px 30px rgba(0,0,0,0.8);
  background: var(--surface-hover);
}
.poster {
  width: 100%;
  aspect-ratio: 2/3;
  border-radius: 10px;
  overflow: hidden;
  background: #1e1e28;
  position: relative;
}
.poster img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.3s;
}
.card:hover .poster img { transform: scale(1.05); }
.poster .fallback-icon {
  position: absolute;
  inset: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 3rem;
}
.card h3 {
  font-size: 0.9rem;
  font-weight: 600;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
.card .meta {
  font-size: 0.75rem;
  color: var(--text-secondary);
}
.card button {
  padding: 0.5rem 0.8rem;
  border-radius: 8px;
  border: none;
  background: var(--accent);
  color: #fff;
  font-weight: 500;
  cursor: pointer;
  font-size: 0.85rem;
  transition: background var(--transition);
  margin-top: auto;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.3rem;
}
.card button svg { width: 1em; height: 1em; }
.card button:hover { background: var(--accent-hover); }

#playerModal {
  display: none;
  position: fixed;
  top: 0; left: 0; width: 100%; height: 100%;
  background: rgba(0,0,0,0.95);
  z-index: 2000;
  justify-content: center;
  align-items: center;
}
#playerModal iframe { width: 95%; height: 92%; border: none; border-radius: 16px; }
#playerModal .close-player {
  position: absolute;
  top: 20px; right: 30px;
  font-size: 2.5rem;
  color: #fff;
  cursor: pointer;
  background: none;
  border: none;
  z-index: 2001;
  transition: transform 0.2s;
}
#playerModal .close-player:hover { transform: scale(1.2); }

.settings-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 1.5rem;
}
.setting-card {
  background: var(--surface);
  backdrop-filter: var(--glass-blur);
  -webkit-backdrop-filter: var(--glass-blur);
  border: 1px solid var(--border);
  border-radius: var(--radius-lg);
  padding: 1.5rem;
}
.setting-card h3 { margin-bottom: 1rem; font-size: 1.1rem; display: flex; align-items: center; gap: 0.5rem; }
.setting-card h3 svg { width: 1.2em; height: 1.2em; }
.setting-card select, .setting-card input[type="text"], .setting-card input[type="file"] {
  width: 100%;
  padding: 0.8rem 1rem;
  border-radius: 10px;
  border: 1px solid var(--border);
  background: rgba(0,0,0,0.3);
  color: var(--text);
  margin-bottom: 0.8rem;
  outline: none;
  font-size: 0.9rem;
}
.setting-card button {
  padding: 0.7rem 1.2rem;
  border-radius: 10px;
  border: none;
  background: var(--accent);
  color: #fff;
  cursor: pointer;
  font-weight: 600;
  transition: background var(--transition);
  display: flex;
  align-items: center;
  gap: 0.4rem;
}
.setting-card button svg { width: 1em; height: 1em; }
.setting-card button:hover { background: var(--accent-hover); }
.theme-buttons {
  display: flex;
  flex-wrap: wrap;
  gap: 0.6rem;
}
.theme-btn {
  padding: 0.6rem 1rem;
  border-radius: 8px;
  cursor: pointer;
  border: 1px solid var(--border);
  background: var(--surface);
  color: var(--text);
  font-size: 0.8rem;
  transition: all var(--transition);
}
.theme-btn:hover { background: var(--surface-hover); }
.theme-btn.active { background: var(--accent); color: #fff; border-color: var(--accent); }

.loading {
  text-align: center;
  padding: 2rem;
  color: var(--text-secondary);
  font-size: 1rem;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1rem;
}
.spinner {
  width: 40px;
  height: 40px;
  border: 3px solid var(--border);
  border-top-color: var(--accent);
  border-radius: 50%;
  animation: spin 1s linear infinite;
}
@keyframes spin { to { transform: rotate(360deg); } }

::-webkit-scrollbar { width: 8px; }
::-webkit-scrollbar-track { background: transparent; }
::-webkit-scrollbar-thumb { background: rgba(255,255,255,0.2); border-radius: 4px; }
::-webkit-scrollbar-thumb:hover { background: rgba(255,255,255,0.3); }

@media (max-width: 768px) {
  .clock-display { font-size: 2.5rem; }
  .grid { grid-template-columns: repeat(auto-fill, minmax(120px, 1fr)); gap: 0.8rem; }
  .tabs { gap: 0.4rem; }
  .tab { padding: 0.5rem 0.9rem; font-size: 0.85rem; }
}
</style>
</head>
<body>
<div class="container">
  <div class="brand">novusOS</div>
  <div class="clock-display" id="clockDisplay">00:00:00</div>

  <div class="search-wrapper">
    <div class="search-bar">
      <input type="text" id="searchInput" placeholder="Search movies, shows, anime, games...">
      <button onclick="globalSearch()">
        <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="11" cy="11" r="8"/><line x1="21" y1="21" x2="16.65" y2="16.65"/></svg>
        Search
      </button>
    </div>
  </div>

  <div class="tabs">
    <div class="tab active" data-tab="games">
      <svg viewBox="0 0 24 24" fill="currentColor"><path d="M7.97 16.97L6.18 15.18C4.34 17.02 3.5 18.5 3.5 19.5C3.5 20.88 4.62 22 6 22C7 22 8.48 21.16 10.32 19.32L8.53 17.53C8.21 17.21 7.71 17 7.25 17C7 17 7 17 7.97 16.97M10 2C9.45 2 9 2.45 9 3V5H15V3C15 2.45 14.55 2 14 2H10M6 6C4.34 6 3 7.34 3 9V13C3 14.66 4.34 16 6 16H18C19.66 16 21 14.66 21 13V9C21 7.34 19.66 6 18 6H6M6 8H18V14H6V8M14 10V12H16V10H14M15.5 20.5C16.88 20.5 18 19.38 18 18C18 17 17.16 15.48 15.32 13.64L13.53 15.43C15.69 17.58 16.5 18.5 16.5 19C16.5 19.28 16.28 19.5 16 19.5C15.5 19.5 14.5 18.66 12.64 16.82L10.85 18.61C12.71 20.47 14.21 21.5 15.5 21.5M15.5 21.5C17.43 21.5 19 19.93 19 18C19 16.9 18.1 15.61 16.5 14.11C14.9 15.61 14 16.9 14 18C14 19.38 15.12 20.5 16.5 20.5"/></svg>
      Games
    </div>
    <div class="tab" data-tab="movies">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="2" y="2" width="20" height="20" rx="2.18" ry="2.18"/><line x1="7" y1="2" x2="7" y2="22"/><line x1="17" y1="2" x2="17" y2="22"/><line x1="2" y1="12" x2="22" y2="12"/><line x1="2" y1="7" x2="7" y2="7"/><line x1="2" y1="17" x2="7" y2="17"/><line x1="17" y1="17" x2="22" y2="17"/><line x1="17" y1="7" x2="22" y2="7"/></svg>
      Movies
    </div>
    <div class="tab" data-tab="shows">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="2" y="7" width="20" height="15" rx="2" ry="2"/><polyline points="17 2 12 7 7 2"/></svg>
      Shows
    </div>
    <div class="tab" data-tab="anime">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M12 2C7 6 5 10 5 14c0 3.866 3.134 7 7 7s7-3.134 7-7c0-4-2-8-7-12z"/><circle cx="9" cy="14" r="1"/><circle cx="15" cy="14" r="1"/><path d="M9 16c.5 1 2 2 3 2s2.5-1 3-2"/></svg>
      Anime
    </div>
    <div class="tab" data-tab="youtube">
      <svg viewBox="0 0 24 24" fill="currentColor"><path d="M23.498 6.186a3.016 3.016 0 0 0-2.122-2.136C19.505 3.545 12 3.545 12 3.545s-7.505 0-9.377.505A3.017 3.017 0 0 0 .502 6.186C0 8.07 0 12 0 12s0 3.93.502 5.814a3.016 3.016 0 0 0 2.122 2.136c1.871.505 9.376.505 9.376.505s7.505 0 9.377-.505a3.015 3.015 0 0 0 2.122-2.136C24 15.93 24 12 24 12s0-3.93-.502-5.814zM9.545 15.568V8.432L15.818 12l-6.273 3.568z"/></svg>
      YouTube
    </div>
    <div class="tab" data-tab="proxy">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="10"/><line x1="2" y1="12" x2="22" y2="12"/><path d="M12 2a15.3 15.3 0 0 1 4 10 15.3 15.3 0 0 1-4 10 15.3 15.3 0 0 1-4-10 15.3 15.3 0 0 1 4-10z"/></svg>
      Proxy
    </div>
    <div class="tab" data-tab="settings">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="3"/><path d="M19.4 15a1.65 1.65 0 0 0 .33 1.82l.06.06a2 2 0 0 1 0 2.83 2 2 0 0 1-2.83 0l-.06-.06a1.65 1.65 0 0 0-1.82-.33 1.65 1.65 0 0 0-1 1.51V21a2 2 0 0 1-2 2 2 2 0 0 1-2-2v-.09A1.65 1.65 0 0 0 9 19.4a1.65 1.65 0 0 0-1.82.33l-.06.06a2 2 0 0 1-2.83 0 2 2 0 0 1 0-2.83l.06-.06a1.65 1.65 0 0 0 .33-1.82 1.65 1.65 0 0 0-1.51-1H3a2 2 0 0 1-2-2 2 2 0 0 1 2-2h.09A1.65 1.65 0 0 0 4.6 9a1.65 1.65 0 0 0-.33-1.82l-.06-.06a2 2 0 0 1 0-2.83 2 2 0 0 1 2.83 0l.06.06a1.65 1.65 0 0 0 1.82.33H9a1.65 1.65 0 0 0 1-1.51V3a2 2 0 0 1 2-2 2 2 0 0 1 2 2v.09a1.65 1.65 0 0 0 1 1.51 1.65 1.65 0 0 0 1.82-.33l.06-.06a2 2 0 0 1 2.83 0 2 2 0 0 1 0 2.83l-.06.06a1.65 1.65 0 0 0-.33 1.82V9a1.65 1.65 0 0 0 1.51 1H21a2 2 0 0 1 2 2 2 2 0 0 1-2 2h-.09a1.65 1.65 0 0 0-1.51 1z"/></svg>
      Settings
    </div>
  </div>

  <div id="games" class="content-section active">
    <div class="grid" id="gameGrid"></div>
    <div id="gameLoadMore" class="loading" style="display:none;">
      <div class="spinner"></div> Loading more games...
    </div>
  </div>

  <div id="movies" class="content-section">
    <div class="grid" id="movieGrid"></div>
    <button id="movieLoadMore" class="theme-btn" style="margin:1rem auto; display:block;">Load More Movies</button>
  </div>

  <div id="shows" class="content-section">
    <div class="grid" id="showGrid"></div>
    <button id="showLoadMore" class="theme-btn" style="margin:1rem auto; display:block;">Load More Shows</button>
  </div>

  <div id="anime" class="content-section">
    <div class="grid" id="animeGrid"></div>
    <button id="animeLoadMore" class="theme-btn" style="margin:1rem auto; display:block;">Load More Anime</button>
  </div>

  <div id="youtube" class="content-section">
    <div class="setting-card" style="max-width:700px; margin:0 auto; text-align:center;">
      <h3><svg viewBox="0 0 24 24" fill="currentColor"><path d="M23.498 6.186a3.016 3.016 0 0 0-2.122-2.136C19.505 3.545 12 3.545 12 3.545s-7.505 0-9.377.505A3.017 3.017 0 0 0 .502 6.186C0 8.07 0 12 0 12s0 3.93.502 5.814a3.016 3.016 0 0 0 2.122 2.136c1.871.505 9.376.505 9.376.505s7.505 0 9.377-.505a3.015 3.015 0 0 0 2.122-2.136C24 15.93 24 12 24 12s0-3.93-.502-5.814zM9.545 15.568V8.432L15.818 12l-6.273 3.568z"/></svg> YouTube Proxy (Invidious)</h3>
      <input type="text" id="ytSearch" placeholder="Search YouTube or paste video URL">
      <button onclick="searchYouTube()">
        <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="11" cy="11" r="8"/><line x1="21" y1="21" x2="16.65" y2="16.65"/></svg>
        Search
      </button>
      <div id="ytResults" class="grid" style="margin-top:1rem;"></div>
    </div>
  </div>

  <div id="proxy" class="content-section">
    <div class="setting-card" style="max-width:700px; margin:0 auto;">
      <h3><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="10"/><line x1="2" y1="12" x2="22" y2="12"/><path d="M12 2a15.3 15.3 0 0 1 4 10 15.3 15.3 0 0 1-4 10 15.3 15.3 0 0 1-4-10 15.3 15.3 0 0 1 4-10z"/></svg> Web Proxy</h3>
      <div style="display:flex; gap:0.5rem;">
        <input type="text" id="proxyUrl" placeholder="Enter URL (https://...)" style="flex:1;">
        <button onclick="navigateProxy()">
          <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><polyline points="16 17 21 12 16 7"/><polyline points="21 12 9 12"/><polyline points="9 21 3 21 3 3 9 3"/></svg>
          Go
        </button>
      </div>
      <select id="proxyService" style="margin-top:0.5rem;">
        <option value="allorigins">AllOrigins</option>
        <option value="corsproxy">CORS Proxy</option>
        <option value="whateverorigin">Whatever Origin</option>
      </select>
      <div id="proxyFrameContainer" style="margin-top:1rem; display:none;">
        <iframe id="proxyFrame" style="width:100%; height:70vh; border:none; border-radius:12px;"></iframe>
      </div>
    </div>
  </div>

  <div id="settings" class="content-section">
    <div class="settings-grid">
      <div class="setting-card">
        <h3><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="11" cy="11" r="8"/><line x1="21" y1="21" x2="16.65" y2="16.65"/></svg> Search Engine</h3>
        <select id="searchEngine">
          <option value="default">Local Search</option>
          <option value="duckduckgo">DuckDuckGo</option>
          <option value="google">Google</option>
        </select>
      </div>
      <div class="setting-card">
        <h3><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M12 3a6 6 0 0 0 9 9 9 9 0 1 1-9-9z"/></svg> Theme</h3>
        <div class="theme-buttons" id="themeButtons">
          <button class="theme-btn" data-theme="dark">Dark</button>
          <button class="theme-btn" data-theme="light">Light</button>
          <button class="theme-btn" data-theme="cherry">Cherry</button>
          <button class="theme-btn" data-theme="forest">Forest</button>
          <button class="theme-btn" data-theme="camo">Camo</button>
          <button class="theme-btn" data-theme="strawberry">Strawberry</button>
          <button class="theme-btn" data-theme="blue">Blue</button>
          <button class="theme-btn" data-theme="pizza">Pizza</button>
        </div>
      </div>
      <div class="setting-card">
        <h3><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><image x="2" y="2" width="20" height="20" xlink:href=""/><rect x="3" y="3" width="18" height="18" rx="2"/><circle cx="8.5" cy="8.5" r="1.5"/><polyline points="21 15 16 10 5 21"/></svg> Custom Background</h3>
        <input type="text" id="customVideoUrl" placeholder="YouTube video URL for background">
        <button onclick="setCustomVideo()">
          <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><polyline points="6 3 6 15 21 15"/><polyline points="21 5 21 19 18 19"/><polyline points="3 19 6 15"/></svg>
          Set Video BG
        </button>
        <input type="file" id="customImageInput" accept="image/*">
        <button onclick="setCustomImage()">
          <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="3" y="3" width="18" height="18" rx="2"/><circle cx="8.5" cy="8.5" r="1.5"/><polyline points="21 15 16 10 5 21"/></svg>
          Set Image BG
        </button>
      </div>
    </div>
  </div>
</div>

<div id="playerModal">
  <button class="close-player" onclick="closePlayer()">×</button>
  <iframe id="playerFrame" src="" allowfullscreen></iframe>
</div>

<script>
// ==================== CONSTANTS & STATE ====================
const GAMES_JSON_URL = 'https://raw.githubusercontent.com/3kh0/3kh0.github.io/main/games.json';
const TMDB_API_KEY = '15d2ea6d0dc1d476efbca3eba2b9bbfb';
const INVIDIOUS_API = 'https://invidious.f5.si/api/v1';

let games = [];
let movies = [];
let shows = [];
let anime = [];
let currentMoviePage = 1;
let currentShowPage = 1;
let currentAnimePage = 1;
let currentSource = 'flystream';

const EMBED_SOURCES = [
  { name: 'flystream', base: 'https://flystream.net/embed/', type: 'path' },
  { name: 'cinera', base: 'https://cinera.cc/embed/', type: 'path' },
  { name: 'dulo', base: 'https://dulo.gd/embed/', type: 'path' },
  { name: 'vidsrc.me', base: 'https://vidsrc.me/embed/', type: 'path' },
  { name: 'vidsrc.to', base: 'https://vidsrc.to/embed/', type: 'path' },
  { name: '2embed', base: 'https://www.2embed.to/embed/tmdb/', type: 'tmdb' },
  { name: 'embed.su', base: 'https://embed.su/embed/', type: 'tmdb' },
  { name: 'multiembed', base: 'https://multiembed.mov/?video_id=', type: 'id' },
  { name: 'vidsrc.icu', base: 'https://vidsrc.icu/embed/', type: 'path' },
  { name: 'smashystream', base: 'https://player.smashy.stream/movie/', type: 'id' },
  { name: 'vidlink', base: 'https://vidlink.pro/movie/', type: 'id' },
  { name: 'autoembed', base: 'https://autoembed.co/movie/tmdb/', type: 'tmdb' }
];

// ==================== UTILITY ====================
function debounce(fn, delay) {
  let timeout;
  return function(...args) {
    clearTimeout(timeout);
    timeout = setTimeout(() => fn.apply(this, args), delay);
  };
}

function formatTime(date) {
  return date.toLocaleTimeString('en-GB', { hour: '2-digit', minute: '2-digit', second: '2-digit', hour12: false });
}

function getEmbedUrl(source, data) {
  const { imdbId, tmdbId, type, season, episode } = data;
  switch (source.name) {
    case 'flystream':
    case 'cinera':
    case 'dulo':
      if (type === 'movie') return source.base + 'movie/' + imdbId;
      else return source.base + 'tv/' + imdbId + '/' + season + '/' + episode;
    case 'vidsrc.me':
    case 'vidsrc.to':
    case 'vidsrc.icu':
      if (type === 'movie') return source.base + 'movie/' + imdbId;
      else return source.base + 'tv/' + imdbId + '/' + season + '/' + episode;
    case '2embed':
    case 'embed.su':
      return source.base + tmdbId;
    case 'multiembed':
      return source.base + imdbId;
    case 'smashystream':
    case 'vidlink':
      return source.base + imdbId;
    case 'autoembed':
      return source.base + tmdbId;
    default:
      return source.base + imdbId;
  }
}

// ==================== CLOCK ====================
function updateClock() {
  document.getElementById('clockDisplay').textContent = formatTime(new Date());
}
setInterval(updateClock, 1000);
updateClock();

// ==================== TAB SWITCHING ====================
document.querySelectorAll('.tab').forEach(tab => {
  tab.addEventListener('click', () => {
    document.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
    document.querySelectorAll('.content-section').forEach(s => s.classList.remove('active'));
    tab.classList.add('active');
    document.getElementById(tab.dataset.tab).classList.add('active');
    if (tab.dataset.tab === 'games' && !document.getElementById('gameGrid').children.length) loadGames();
    if (tab.dataset.tab === 'movies' && !document.getElementById('movieGrid').children.length) loadMovies();
    if (tab.dataset.tab === 'shows' && !document.getElementById('showGrid').children.length) loadShows();
    if (tab.dataset.tab === 'anime' && !document.getElementById('animeGrid').children.length) loadAnime();
  });
});

// ==================== GAMES ====================
async function loadGames() {
  const grid = document.getElementById('gameGrid');
  grid.innerHTML = '<div class="loading"><div class="spinner"></div>Loading games...</div>';
  try {
    const res = await fetch(GAMES_JSON_URL);
    if (!res.ok) throw new Error('Failed to fetch games');
    games = await res.json();
    renderGames(games.slice(0, 100));
    if (games.length > 100) {
      document.getElementById('gameLoadMore').style.display = 'flex';
      document.getElementById('gameLoadMore').onclick = () => renderGames(games.slice(100));
    }
  } catch(e) {
    grid.innerHTML = '<div class="loading">Failed to load games.</div>';
  }
}

function renderGames(list) {
  const grid = document.getElementById('gameGrid');
  list.forEach(game => {
    const card = document.createElement('div');
    card.className = 'card';
    card.innerHTML = `
      <div class="poster" style="display:flex; align-items:center; justify-content:center; background:#1e1e28;">
        <svg width="50" height="50" viewBox="0 0 24 24" fill="currentColor" style="opacity:0.6;"><path d="M7.97 16.97L6.18 15.18C4.34 17.02 3.5 18.5 3.5 19.5C3.5 20.88 4.62 22 6 22C7 22 8.48 21.16 10.32 19.32L8.53 17.53C8.21 17.21 7.71 17 7.25 17C7 17 7 17 7.97 16.97M10 2C9.45 2 9 2.45 9 3V5H15V3C15 2.45 14.55 2 14 2H10M6 6C4.34 6 3 7.34 3 9V13C3 14.66 4.34 16 6 16H18C19.66 16 21 14.66 21 13V9C21 7.34 19.66 6 18 6H6M6 8H18V14H6V8M14 10V12H16V10H14M15.5 20.5C16.88 20.5 18 19.38 18 18C18 17 17.16 15.48 15.32 13.64L13.53 15.43C15.69 17.58 16.5 18.5 16.5 19C16.5 19.28 16.28 19.5 16 19.5C15.5 19.5 14.5 18.66 12.64 16.82L10.85 18.61C12.71 20.47 14.21 21.5 15.5 21.5M15.5 21.5C17.43 21.5 19 19.93 19 18C19 16.9 18.1 15.61 16.5 14.11C14.9 15.61 14 16.9 14 18C14 19.38 15.12 20.5 16.5 20.5"/></svg>
      </div>
      <h3>${game.title}</h3>
      <button onclick="playGame('${game.url}')">
        <svg width="16" height="16" viewBox="0 0 24 24" fill="currentColor"><path d="M8 5v14l11-7z"/></svg>
        Play
      </button>
    `;
    grid.appendChild(card);
  });
}

function playGame(url) {
  openPlayer(url);
}

// ==================== MOVIES ====================
async function loadMovies() {
  const grid = document.getElementById('movieGrid');
  grid.innerHTML = '<div class="loading"><div class="spinner"></div>Loading movies...</div>';
  try {
    const res = await fetch(`https://api.themoviedb.org/3/movie/popular?api_key=${TMDB_API_KEY}&page=${currentMoviePage}`);
    const data = await res.json();
    movies = data.results;
    renderMovies(movies);
  } catch(e) {
    grid.innerHTML = '<div class="loading">Failed to load movies.</div>';
  }
}

function renderMovies(movieList) {
  const grid = document.getElementById('movieGrid');
  grid.innerHTML = '';
  movieList.forEach(movie => {
    const poster = movie.poster_path ? `https://image.tmdb.org/t/p/w500${movie.poster_path}` : '';
    const card = document.createElement('div');
    card.className = 'card';
    card.innerHTML = `
      <div class="poster">${poster ? `<img src="${poster}" alt="${movie.title}" loading="lazy">` : '<div class="fallback-icon"><svg width="60" height="60" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><rect x="2" y="2" width="20" height="20" rx="2.18" ry="2.18"/><line x1="7" y1="2" x2="7" y2="22"/><line x1="17" y1="2" x2="17" y2="22"/><line x1="2" y1="12" x2="22" y2="12"/><line x1="2" y1="7" x2="7" y2="7"/><line x1="2" y1="17" x2="7" y2="17"/><line x1="17" y1="17" x2="22" y2="17"/><line x1="17" y1="7" x2="22" y2="7"/></svg></div>'}</div>
      <h3>${movie.title}</h3>
      <div class="meta">${movie.release_date || ''}</div>
      <button onclick="playMovie('${movie.id}', 'movie', '${movie.title}', '${poster}', '${movie.overview || ''}', '${movie.imdb_id || ''}', null)">
        <svg width="16" height="16" viewBox="0 0 24 24" fill="currentColor"><path d="M8 5v14l11-7z"/></svg>
        Play
      </button>
    `;
    grid.appendChild(card);
  });
}

// ==================== SHOWS ====================
async function loadShows() {
  const grid = document.getElementById('showGrid');
  grid.innerHTML = '<div class="loading"><div class="spinner"></div>Loading shows...</div>';
  try {
    const res = await fetch(`https://api.themoviedb.org/3/tv/popular?api_key=${TMDB_API_KEY}&page=${currentShowPage}`);
    const data = await res.json();
    shows = data.results;
    renderShows(shows);
  } catch(e) {
    grid.innerHTML = '<div class="loading">Failed to load shows.</div>';
  }
}

function renderShows(showList) {
  const grid = document.getElementById('showGrid');
  grid.innerHTML = '';
  showList.forEach(show => {
    const poster = show.poster_path ? `https://image.tmdb.org/t/p/w500${show.poster_path}` : '';
    const card = document.createElement('div');
    card.className = 'card';
    card.innerHTML = `
      <div class="poster">${poster ? `<img src="${poster}" alt="${show.name}" loading="lazy">` : '<div class="fallback-icon"><svg width="60" height="60" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><rect x="2" y="7" width="20" height="15" rx="2" ry="2"/><polyline points="17 2 12 7 7 2"/></svg></div>'}</div>
      <h3>${show.name}</h3>
      <div class="meta">${show.first_air_date || ''}</div>
      <button onclick="playShow('${show.id}', '${show.name}', '${poster}', '${show.overview || ''}', '${show.imdb_id || ''}')">
        <svg width="16" height="16" viewBox="0 0 24 24" fill="currentColor"><path d="M8 5v14l11-7z"/></svg>
        Play
      </button>
    `;
    grid.appendChild(card);
  });
}

// ==================== ANIME ====================
async function loadAnime() {
  const grid = document.getElementById('animeGrid');
  grid.innerHTML = '<div class="loading"><div class="spinner"></div>Loading anime...</div>';
  try {
    const res = await fetch(`https://api.jikan.moe/v4/top/anime?page=${currentAnimePage}&limit=25`);
    const data = await res.json();
    anime = data.data;
    renderAnime(anime);
  } catch(e) {
    grid.innerHTML = '<div class="loading">Failed to load anime.</div>';
  }
}

function renderAnime(animeList) {
  const grid = document.getElementById('animeGrid');
  grid.innerHTML = '';
  animeList.forEach(item => {
    const poster = item.images?.jpg?.image_url || '';
    const title = item.title_english || item.title;
    const card = document.createElement('div');
    card.className = 'card';
    card.innerHTML = `
      <div class="poster">${poster ? `<img src="${poster}" alt="${title}" loading="lazy">` : '<div class="fallback-icon"><svg width="60" height="60" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M12 2C7 6 5 10 5 14c0 3.866 3.134 7 7 7s7-3.134 7-7c0-4-2-8-7-12z"/><circle cx="9" cy="14" r="1"/><circle cx="15" cy="14" r="1"/><path d="M9 16c.5 1 2 2 3 2s2.5-1 3-2"/></svg></div>'}</div>
      <h3>${title}</h3>
      <div class="meta">${item.score || ''}</div>
      <button onclick="playAnime('${item.mal_id}', '${title}', '${poster}', '${item.synopsis || ''}')">
        <svg width="16" height="16" viewBox="0 0 24 24" fill="currentColor"><path d="M8 5v14l11-7z"/></svg>
        Play
      </button>
    `;
    grid.appendChild(card);
  });
}

// ==================== PLAYBACK ====================
function openPlayer(url) {
  const modal = document.getElementById('playerModal');
  const iframe = document.getElementById('playerFrame');
  iframe.src = url;
  modal.style.display = 'flex';
}

function closePlayer() {
  document.getElementById('playerModal').style.display = 'none';
  document.getElementById('playerFrame').src = '';
}

function playMovie(tmdbId, type, title, poster, overview, imdbId) {
  const data = { tmdbId, imdbId: imdbId || '', type: 'movie', season: null, episode: null };
  trySources(data, title);
}

function playShow(tmdbId, title, poster, overview, imdbId) {
  const season = prompt('Enter season number:');
  const episode = prompt('Enter episode number:');
  if (!season || !episode) return;
  const data = { tmdbId, imdbId: imdbId || '', type: 'tv', season, episode };
  trySources(data, title);
}

function playAnime(malId, title, poster, synopsis) {
  const data = { tmdbId: malId, imdbId: 'tt' + malId, type: 'movie', season: null, episode: null };
  trySources(data, title);
}

function trySources(data, title) {
  let sourceIndex = 0;
  const iframe = document.getElementById('playerFrame');
  openPlayer('');
  iframe.onload = null;
  iframe.onerror = null;

  function attemptNext() {
    if (sourceIndex >= EMBED_SOURCES.length) {
      alert('No working source found for this title.');
      closePlayer();
      return;
    }
    const source = EMBED_SOURCES[sourceIndex];
    let url = getEmbedUrl(source, data);
    console.log(`Trying source ${source.name}: ${url}`);
    iframe.onload = () => {
      setTimeout(() => {
        try {
          const doc = iframe.contentDocument || iframe.contentWindow.document;
          if (!doc || doc.body.innerHTML.trim().length < 50) {
            sourceIndex++;
            attemptNext();
          }
        } catch(e) {
          // Cross-origin, can't inspect; assume loaded
        }
      }, 4000);
    };
    iframe.onerror = () => {
      sourceIndex++;
      attemptNext();
    };
    iframe.src = url;
  }
  attemptNext();
}

// ==================== YOUTUBE ====================
async function searchYouTube() {
  const query = document.getElementById('ytSearch').value.trim();
  if (!query) return;
  const resultsDiv = document.getElementById('ytResults');
  resultsDiv.innerHTML = '<div class="loading"><div class="spinner"></div>Searching...</div>';
  try {
    if (query.includes('youtube.com/watch') || query.includes('youtu.be/')) {
      const videoId = extractYouTubeId(query);
      if (videoId) {
        resultsDiv.innerHTML = '';
        openPlayer(`https://www.youtube.com/embed/${videoId}`);
        return;
      }
    }
    const res = await fetch(`${INVIDIOUS_API}/search?q=${encodeURIComponent(query)}`);
    const videos = await res.json();
    resultsDiv.innerHTML = '';
    videos.forEach(video => {
      if (video.type !== 'video') return;
      const card = document.createElement('div');
      card.className = 'card';
      card.innerHTML = `
        <div class="poster">${video.videoThumbnails?.[0]?.url ? `<img src="${video.videoThumbnails[0].url}" alt="${video.title}" loading="lazy">` : '<div class="fallback-icon"><svg width="60" height="60" viewBox="0 0 24 24" fill="currentColor"><path d="M23.498 6.186a3.016 3.016 0 0 0-2.122-2.136C19.505 3.545 12 3.545 12 3.545s-7.505 0-9.377.505A3.017 3.017 0 0 0 .502 6.186C0 8.07 0 12 0 12s0 3.93.502 5.814a3.016 3.016 0 0 0 2.122 2.136c1.871.505 9.376.505 9.376.505s7.505 0 9.377-.505a3.015 3.015 0 0 0 2.122-2.136C24 15.93 24 12 24 12s0-3.93-.502-5.814zM9.545 15.568V8.432L15.818 12l-6.273 3.568z"/></svg></div>'}</div>
        <h3>${video.title}</h3>
        <div class="meta">${video.author} · ${video.publishedText || ''}</div>
        <button onclick="openPlayer('https://www.youtube.com/embed/${video.videoId}')">
          <svg width="16" height="16" viewBox="0 0 24 24" fill="currentColor"><path d="M8 5v14l11-7z"/></svg>
          Watch
        </button>
      `;
      resultsDiv.appendChild(card);
    });
  } catch(e) {
    resultsDiv.innerHTML = '<div class="loading">Failed to search YouTube.</div>';
  }
}

function extractYouTubeId(url) {
  const match = url.match(/(?:youtube\.com\/(?:[^\/]+\/.+\/|(?:v|e(?:mbed)?)\/|.*[?&]v=)|youtu\.be\/)([^"&?\/\s]{11})/);
  return match ? match[1] : null;
}

// ==================== PROXY ====================
function navigateProxy() {
  const url = document.getElementById('proxyUrl').value.trim();
  if (!url) return;
  const service = document.getElementById('proxyService').value;
  let proxyUrl = '';
  switch(service) {
    case 'allorigins':
      proxyUrl = `https://api.allorigins.win/raw?url=${encodeURIComponent(url)}`;
      break;
    case 'corsproxy':
      proxyUrl = `https://corsproxy.io/?${encodeURIComponent(url)}`;
      break;
    case 'whateverorigin':
      proxyUrl = `http://www.whateverorigin.org/get?url=${encodeURIComponent(url)}`;
      break;
    default:
      proxyUrl = `https://api.allorigins.win/raw?url=${encodeURIComponent(url)}`;
  }
  const container = document.getElementById('proxyFrameContainer');
  container.style.display = 'block';
  document.getElementById('proxyFrame').src = proxyUrl;
}

// ==================== SEARCH ====================
function globalSearch() {
  const q = document.getElementById('searchInput').value.toLowerCase().trim();
  if (!q) return;
  const gameCards = document.querySelectorAll('#gameGrid .card');
  gameCards.forEach(c => {
    const title = c.querySelector('h3').textContent.toLowerCase();
    c.style.display = title.includes(q) ? 'flex' : 'none';
  });
  const movieCards = document.querySelectorAll('#movieGrid .card, #showGrid .card, #animeGrid .card');
  movieCards.forEach(c => {
    const title = c.querySelector('h3').textContent.toLowerCase();
    c.style.display = title.includes(q) ? 'flex' : 'none';
  });
}

// ==================== THEMES ====================
const themes = {
  dark: { bg:'#0b0b10', surface:'rgba(255,255,255,0.04)', solid:'#15151c', hover:'rgba(255,255,255,0.08)', text:'#ffffff', secondary:'#b0b0c0', accent:'#6c8cff', border:'rgba(255,255,255,0.1)' },
  light: { bg:'#f5f5f7', surface:'rgba(255,255,255,0.7)', solid:'#ffffff', hover:'rgba(0,0,0,0.05)', text:'#1c1c1e', secondary:'#6e6e73', accent:'#007aff', border:'rgba(0,0,0,0.1)' },
  cherry: { bg:'#1a0000', surface:'rgba(255,0,0,0.1)', solid:'#2a0000', hover:'rgba(255,0,0,0.2)', text:'#ffcccc', secondary:'#ff9999', accent:'#ff0000', border:'rgba(255,0,0,0.3)' },
  forest: { bg:'#001a00', surface:'rgba(0,255,0,0.08)', solid:'#002a00', hover:'rgba(0,255,0,0.15)', text:'#ccffcc', secondary:'#99cc99', accent:'#00ff00', border:'rgba(0,255,0,0.3)' },
  camo: { bg:'#1a1a00', surface:'rgba(128,128,0,0.1)', solid:'#2a2a00', hover:'rgba(128,128,0,0.2)', text:'#cccc99', secondary:'#999966', accent:'#808000', border:'rgba(128,128,0,0.3)' },
  strawberry: { bg:'#1a0010', surface:'rgba(255,0,128,0.1)', solid:'#2a001a', hover:'rgba(255,0,128,0.2)', text:'#ffccdd', secondary:'#ff99bb', accent:'#ff0080', border:'rgba(255,0,128,0.3)' },
  blue: { bg:'#001020', surface:'rgba(0,150,255,0.1)', solid:'#002a40', hover:'rgba(0,150,255,0.2)', text:'#cceeff', secondary:'#99ccff', accent:'#0099ff', border:'rgba(0,150,255,0.3)' },
  pizza: { bg:'#1a0a00', surface:'rgba(255,128,0,0.1)', solid:'#2a1000', hover:'rgba(255,128,0,0.2)', text:'#ffd9b3', secondary:'#ffb380', accent:'#ff8000', border:'rgba(255,128,0,0.3)' }
};

function setTheme(themeName) {
  const t = themes[themeName] || themes.dark;
  const root = document.documentElement;
  root.style.setProperty('--bg', t.bg);
  root.style.setProperty('--surface', t.surface);
  root.style.setProperty('--surface-solid', t.solid);
  root.style.setProperty('--surface-hover', t.hover);
  root.style.setProperty('--text', t.text);
  root.style.setProperty('--text-secondary', t.secondary);
  root.style.setProperty('--accent', t.accent);
  root.style.setProperty('--accent-hover', t.accent);
  root.style.setProperty('--border', t.border);
  localStorage.setItem('theme', themeName);
  document.querySelectorAll('.theme-btn').forEach(btn => {
    btn.classList.toggle('active', btn.dataset.theme === themeName);
  });
}

function setCustomVideo() {
  const url = document.getElementById('customVideoUrl').value.trim();
  if (!url) return;
  const videoId = extractYouTubeId(url);
  if (!videoId) return;
  document.body.style.background = `linear-gradient(rgba(0,0,0,0.7), rgba(0,0,0,0.7)), url(https://img.youtube.com/vi/${videoId}/hqdefault.jpg) no-repeat center center fixed`;
  document.body.style.backgroundSize = 'cover';
  localStorage.setItem('customTheme', 'video:' + videoId);
}

function setCustomImage() {
  const fileInput = document.getElementById('customImageInput');
  if (fileInput.files && fileInput.files[0]) {
    const reader = new FileReader();
    reader.onload = function(e) {
      document.body.style.background = `linear-gradient(rgba(0,0,0,0.6), rgba(0,0,0,0.6)), url(${e.target.result}) no-repeat center center fixed`;
      document.body.style.backgroundSize = 'cover';
      localStorage.setItem('customTheme', 'image:' + e.target.result);
    };
    reader.readAsDataURL(fileInput.files[0]);
  }
}

// ==================== INIT ====================
const savedTheme = localStorage.getItem('theme');
if (savedTheme) setTheme(savedTheme);
else setTheme('dark');

const savedEngine = localStorage.getItem('searchEngine');
if (savedEngine) document.getElementById('searchEngine').value = savedEngine;

document.querySelectorAll('.theme-btn').forEach(btn => {
  btn.addEventListener('click', () => setTheme(btn.dataset.theme));
});

document.getElementById('searchEngine').addEventListener('change', function() {
  localStorage.setItem('searchEngine', this.value);
});

loadGames();
</script>
</body>
</html>
