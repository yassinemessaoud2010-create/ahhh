[ahh-dashboard.html](https://github.com/user-attachments/files/26067466/ahh-dashboard.html)
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>ahh — Bot Dashboard</title>
<link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;500;600;700;800&family=IBM+Plex+Mono:wght@400;500;600&display=swap" rel="stylesheet">
<style>
/* =========================================================
   ROOT / TOKENS
   ========================================================= */
:root {
  --bg0: #0a0b0d;
  --bg1: #0f1115;
  --bg2: #141619;
  --bg3: #1a1d22;
  --bg4: #21252c;
  --bg5: #272c35;
  --surface: #1c1f26;
  --surface2: #222730;

  --blurple: #5865F2;
  --blurple2: #4752c4;
  --blurple-glow: rgba(88,101,242,.4);
  --blurple-dim: rgba(88,101,242,.12);

  --green: #23d18b;
  --green-glow: rgba(35,209,139,.35);
  --green-dim: rgba(35,209,139,.1);

  --red: #f04747;
  --red-dim: rgba(240,71,71,.12);
  --red-glow: rgba(240,71,71,.3);

  --yellow: #faa81a;
  --yellow-dim: rgba(250,168,26,.1);

  --cyan: #00d2ff;
  --cyan-dim: rgba(0,210,255,.1);

  --pink: #ff73fa;
  --pink-dim: rgba(255,115,250,.1);

  --orange: #ff8c42;
  --orange-dim: rgba(255,140,66,.1);

  --teal: #1de9b6;

  --text0: #ffffff;
  --text1: #e3e5e8;
  --text2: #b5bac1;
  --text3: #80848e;
  --text4: #4e5058;

  --border: rgba(255,255,255,.05);
  --border2: rgba(255,255,255,.09);
  --border-accent: rgba(88,101,242,.35);

  --sidebar-w: 272px;
  --header-h: 60px;

  --radius-sm: 6px;
  --radius-md: 10px;
  --radius-lg: 16px;
  --radius-xl: 22px;

  --shadow-card: 0 4px 24px rgba(0,0,0,.4);
  --shadow-glow: 0 0 40px var(--blurple-glow);
}

*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

html { scroll-behavior: smooth; }

body {
  font-family: 'Syne', sans-serif;
  background: var(--bg0);
  color: var(--text1);
  min-height: 100vh;
  overflow-x: hidden;
  cursor: default;
}

/* =========================================================
   SCROLLBAR
   ========================================================= */
::-webkit-scrollbar { width: 5px; height: 5px; }
::-webkit-scrollbar-track { background: transparent; }
::-webkit-scrollbar-thumb { background: var(--bg5); border-radius: 99px; }
::-webkit-scrollbar-thumb:hover { background: var(--text4); }

/* =========================================================
   BACKGROUND CANVAS
   ========================================================= */
#bg-canvas {
  position: fixed; inset: 0;
  pointer-events: none; z-index: 0;
}

.bg-noise {
  position: fixed; inset: 0;
  background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noise'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noise)' opacity='1'/%3E%3C/svg%3E");
  opacity: .018;
  pointer-events: none; z-index: 0;
}

/* =========================================================
   LAYOUT
   ========================================================= */
.app { display: flex; min-height: 100vh; position: relative; z-index: 1; }

/* =========================================================
   SIDEBAR
   ========================================================= */
.sidebar {
  width: var(--sidebar-w);
  background: var(--bg1);
  border-right: 1px solid var(--border);
  display: flex; flex-direction: column;
  position: fixed; top: 0; left: 0; bottom: 0;
  z-index: 200;
  overflow: hidden;
}

/* top shine */
.sidebar::after {
  content: '';
  position: absolute; top: 0; left: 0; right: 0; height: 1px;
  background: linear-gradient(90deg, transparent 0%, var(--blurple) 50%, transparent 100%);
  animation: shimmer 4s ease-in-out infinite;
  opacity: .6;
}
@keyframes shimmer { 0%,100%{opacity:.3} 50%{opacity:.9} }

/* LOGO */
.sb-logo {
  padding: 20px 20px 16px;
  display: flex; align-items: center; gap: 12px;
  border-bottom: 1px solid var(--border);
  position: relative;
}
.sb-logo-mark {
  width: 44px; height: 44px; border-radius: 14px;
  background: linear-gradient(135deg, var(--blurple) 0%, var(--pink) 100%);
  display: flex; align-items: center; justify-content: center;
  font-size: 22px; flex-shrink: 0;
  box-shadow: 0 4px 20px var(--blurple-glow), inset 0 1px 0 rgba(255,255,255,.2);
  animation: logoBreathe 3s ease-in-out infinite;
  position: relative; overflow: hidden;
}
.sb-logo-mark::after {
  content: '';
  position: absolute; inset: 0;
  background: linear-gradient(135deg, rgba(255,255,255,.15) 0%, transparent 60%);
}
@keyframes logoBreathe {
  0%,100% { box-shadow: 0 4px 20px var(--blurple-glow); }
  50%      { box-shadow: 0 4px 35px var(--blurple-glow), 0 0 60px rgba(255,115,250,.15); }
}
.sb-logo-text { line-height: 1; }
.sb-logo-name { font-size: 22px; font-weight: 800; letter-spacing: -1px; color: var(--text0); }
.sb-logo-name em { font-style: normal; color: var(--blurple); }
.sb-logo-tag {
  font-size: 10px; font-family: 'IBM Plex Mono'; font-weight: 500;
  color: var(--text3); letter-spacing: .5px; margin-top: 2px;
}

/* SERVER PICKER */
.sb-server {
  margin: 14px 14px 0;
  padding: 11px 14px;
  background: var(--bg3);
  border: 1px solid var(--border2);
  border-radius: var(--radius-md);
  display: flex; align-items: center; gap: 10px;
  cursor: pointer; transition: all .2s;
  position: relative; overflow: hidden;
}
.sb-server::before {
  content: ''; position: absolute; inset: 0;
  background: linear-gradient(135deg, var(--blurple-dim), transparent);
  opacity: 0; transition: opacity .2s;
}
.sb-server:hover { border-color: var(--border-accent); }
.sb-server:hover::before { opacity: 1; }
.sb-server-icon {
  width: 34px; height: 34px; border-radius: 10px; flex-shrink: 0;
  background: linear-gradient(135deg, var(--blurple), var(--cyan));
  display: flex; align-items: center; justify-content: center;
  font-size: 16px;
}
.sb-server-info { flex: 1; min-width: 0; }
.sb-server-name { font-size: 13px; font-weight: 700; color: var(--text0); white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
.sb-server-id { font-size: 10px; font-family: 'IBM Plex Mono'; color: var(--text4); margin-top: 1px; }
.sb-server-caret { color: var(--text3); font-size: 10px; flex-shrink: 0; }

/* BOT ONLINE PILL */
.sb-status {
  margin: 10px 14px;
  padding: 8px 14px;
  background: var(--green-dim);
  border: 1px solid rgba(35,209,139,.2);
  border-radius: var(--radius-md);
  display: flex; align-items: center; gap: 8px;
}
.sb-status-dot {
  width: 8px; height: 8px; border-radius: 50%;
  background: var(--green);
  box-shadow: 0 0 8px var(--green-glow);
  animation: pulse 2s ease-in-out infinite;
  flex-shrink: 0;
}
@keyframes pulse {
  0%,100% { box-shadow: 0 0 8px var(--green-glow); transform: scale(1); }
  50%      { box-shadow: 0 0 18px var(--green-glow); transform: scale(1.1); }
}
.sb-status-label { font-size: 12px; font-weight: 700; color: var(--green); }
.sb-status-latency { margin-left: auto; font-size: 10px; font-family: 'IBM Plex Mono'; color: var(--text3); }

/* NAV */
.sb-nav { flex: 1; overflow-y: auto; padding: 6px 10px 10px; }
.sb-nav-section { margin-top: 16px; }
.sb-nav-label {
  font-size: 9px; font-weight: 700; letter-spacing: 2px;
  text-transform: uppercase; color: var(--text4);
  padding: 0 8px; margin-bottom: 4px;
}
.nav-item {
  display: flex; align-items: center; gap: 10px;
  padding: 9px 10px;
  border-radius: var(--radius-sm);
  cursor: pointer; transition: all .18s;
  font-size: 13.5px; font-weight: 500;
  color: var(--text3);
  position: relative; overflow: hidden;
  user-select: none;
}
.nav-item::before {
  content: ''; position: absolute; left: 0; top: 20%; bottom: 20%;
  width: 3px; background: var(--blurple); border-radius: 0 2px 2px 0;
  transform: scaleY(0); transition: transform .2s;
}
.nav-item:hover { background: var(--bg3); color: var(--text1); }
.nav-item.active {
  background: var(--blurple-dim);
  color: var(--text0); font-weight: 600;
  border: 1px solid var(--border-accent);
}
.nav-item.active::before { transform: scaleY(1); }
.nav-item-icon { font-size: 17px; width: 26px; text-align: center; flex-shrink: 0; }
.nav-item-label { flex: 1; }
.nav-badge {
  font-size: 9px; font-weight: 700;
  background: var(--red); color: #fff;
  padding: 2px 6px; border-radius: 99px;
  min-width: 18px; text-align: center;
}
.nav-badge.blue { background: var(--blurple); }
.nav-badge.green { background: var(--green); color: #000; }
.nav-divider { height: 1px; background: var(--border); margin: 8px 8px; }

/* SIDEBAR FOOTER */
.sb-footer {
  padding: 12px 14px;
  border-top: 1px solid var(--border);
}
.sb-user {
  display: flex; align-items: center; gap: 10px;
  padding: 9px 12px;
  background: var(--bg3); border-radius: var(--radius-md);
  cursor: pointer; transition: background .2s;
}
.sb-user:hover { background: var(--bg4); }
.sb-user-avatar {
  width: 32px; height: 32px; border-radius: 50%;
  background: linear-gradient(135deg, var(--blurple), var(--pink));
  display: flex; align-items: center; justify-content: center;
  font-size: 14px; flex-shrink: 0;
}
.sb-user-name { font-size: 13px; font-weight: 700; color: var(--text0); }
.sb-user-role { font-size: 10px; color: var(--text4); }
.sb-user-settings { margin-left: auto; color: var(--text3); font-size: 15px; }

/* =========================================================
   MAIN CONTENT AREA
   ========================================================= */
.main {
  margin-left: var(--sidebar-w);
  flex: 1; min-height: 100vh;
  display: flex; flex-direction: column;
}

/* TOP BAR */
.topbar {
  height: var(--header-h);
  background: rgba(10,11,13,.85);
  backdrop-filter: blur(20px);
  border-bottom: 1px solid var(--border);
  display: flex; align-items: center;
  padding: 0 32px;
  position: sticky; top: 0; z-index: 100;
  gap: 12px;
}
.topbar-title { font-size: 15px; font-weight: 700; color: var(--text0); flex: 1; }
.topbar-title span { color: var(--text3); font-weight: 400; }
.topbar-actions { display: flex; align-items: center; gap: 8px; }
.tb-btn {
  padding: 7px 14px; border-radius: var(--radius-sm);
  background: var(--bg4); border: 1px solid var(--border2);
  color: var(--text2); font-family: 'Syne'; font-size: 12px; font-weight: 600;
  cursor: pointer; transition: all .2s;
}
.tb-btn:hover { background: var(--bg5); color: var(--text0); }
.tb-btn.primary {
  background: var(--blurple); border-color: var(--blurple);
  color: #fff;
}
.tb-btn.primary:hover { background: var(--blurple2); box-shadow: 0 0 20px var(--blurple-glow); }

/* PAGE */
.page { padding: 32px; flex: 1; }
.page-section { display: none; }
.page-section.active { display: block; }

/* PAGE HEADER */
.page-head { margin-bottom: 28px; }
.page-head-row { display: flex; align-items: flex-start; justify-content: space-between; gap: 16px; }
.page-title {
  font-size: 26px; font-weight: 800; letter-spacing: -1px;
  line-height: 1.1; color: var(--text0);
}
.page-title .hl {
  background: linear-gradient(90deg, var(--blurple), var(--cyan));
  -webkit-background-clip: text; -webkit-text-fill-color: transparent;
  background-clip: text;
}
.page-desc { font-size: 13px; color: var(--text3); margin-top: 5px; line-height: 1.5; }

/* BREADCRUMB */
.breadcrumb {
  display: flex; align-items: center; gap: 6px;
  font-size: 11px; color: var(--text4); margin-bottom: 6px;
  font-family: 'IBM Plex Mono';
}
.breadcrumb span { color: var(--text3); }

/* =========================================================
   STAT CARDS
   ========================================================= */
.stats-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 14px; margin-bottom: 24px;
}
.stat-card {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: var(--radius-lg);
  padding: 18px 20px;
  position: relative; overflow: hidden;
  cursor: default; transition: all .25s;
}
.stat-card::before {
  content: ''; position: absolute;
  inset: 0;
  background: linear-gradient(135deg, var(--card-glow, rgba(88,101,242,.06)) 0%, transparent 60%);
  transition: opacity .3s;
}
.stat-card:hover { transform: translateY(-2px); border-color: var(--border2); box-shadow: var(--shadow-card); }
.stat-card-emoji { font-size: 28px; margin-bottom: 10px; display: block; }
.stat-card-label { font-size: 10px; font-weight: 700; text-transform: uppercase; letter-spacing: 1.5px; color: var(--text3); margin-bottom: 5px; }
.stat-card-value { font-size: 28px; font-weight: 800; letter-spacing: -1px; line-height: 1; }
.stat-card-delta { font-size: 11px; margin-top: 6px; display: flex; align-items: center; gap: 4px; font-family: 'IBM Plex Mono'; }
.stat-card-delta.up { color: var(--green); }
.stat-card-delta.down { color: var(--red); }

/* =========================================================
   GRID LAYOUTS
   ========================================================= */
.grid-2 { display: grid; grid-template-columns: 1fr 1fr; gap: 18px; }
.grid-3 { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 18px; }
.col-full { grid-column: 1 / -1; }
.col-span2 { grid-column: span 2; }

/* =========================================================
   PANEL / CARD
   ========================================================= */
.panel {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: var(--radius-lg);
  overflow: hidden;
  transition: border-color .3s;
}
.panel:hover { border-color: var(--border2); }

.panel-head {
  padding: 16px 22px;
  border-bottom: 1px solid var(--border);
  display: flex; align-items: center; gap: 12px;
  position: relative;
}
.panel-head-glow {
  position: absolute; bottom: 0; left: 0; right: 0; height: 1px;
  background: linear-gradient(90deg, transparent, var(--panel-accent, var(--blurple)), transparent);
  opacity: 0; transition: opacity .3s;
}
.panel:hover .panel-head-glow { opacity: .5; }
.panel-icon {
  width: 36px; height: 36px; border-radius: 10px;
  display: flex; align-items: center; justify-content: center;
  font-size: 17px; flex-shrink: 0;
}
.pi-blue   { background: var(--blurple-dim); border: 1px solid rgba(88,101,242,.25); }
.pi-green  { background: var(--green-dim);   border: 1px solid rgba(35,209,139,.25); }
.pi-red    { background: var(--red-dim);     border: 1px solid rgba(240,71,71,.25); }
.pi-yellow { background: var(--yellow-dim);  border: 1px solid rgba(250,168,26,.25); }
.pi-pink   { background: var(--pink-dim);    border: 1px solid rgba(255,115,250,.25); }
.pi-cyan   { background: var(--cyan-dim);    border: 1px solid rgba(0,210,255,.25); }
.pi-orange { background: var(--orange-dim);  border: 1px solid rgba(255,140,66,.25); }

.panel-title { font-size: 14px; font-weight: 700; color: var(--text0); }
.panel-sub   { font-size: 11px; color: var(--text3); margin-top: 1px; }

.panel-extra { margin-left: auto; display: flex; align-items: center; gap: 8px; }

.panel-body { padding: 22px; display: flex; flex-direction: column; gap: 18px; }
.panel-body-compact { padding: 16px 22px; display: flex; flex-direction: column; gap: 12px; }

/* =========================================================
   FORM ELEMENTS
   ========================================================= */
.field { display: flex; flex-direction: column; gap: 6px; }
.field-label {
  font-size: 11px; font-weight: 700;
  text-transform: uppercase; letter-spacing: 1px;
  color: var(--text3);
}
.field-hint { font-size: 11px; color: var(--text4); margin-top: -3px; }

.field-row-inline {
  display: flex; align-items: center;
  justify-content: space-between; gap: 12px;
}
.field-row-label .field-label { margin-bottom: 1px; font-size: 13px; text-transform: none; letter-spacing: 0; color: var(--text1); font-weight: 600; }
.field-row-label .field-hint { margin-top: 1px; font-size: 11px; }

.inp {
  width: 100%;
  background: var(--bg3);
  border: 1px solid var(--border2);
  border-radius: var(--radius-sm);
  padding: 10px 13px;
  color: var(--text0);
  font-family: 'Syne'; font-size: 13px;
  outline: none; transition: all .2s;
}
.inp::placeholder { color: var(--text4); }
.inp:focus { border-color: var(--blurple); box-shadow: 0 0 0 3px rgba(88,101,242,.18); background: var(--bg4); }
.inp:hover:not(:focus) { border-color: var(--text4); }

textarea.inp { resize: vertical; min-height: 80px; line-height: 1.6; font-family: 'IBM Plex Mono'; font-size: 12px; }

select.inp {
  cursor: pointer; appearance: none;
  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='10' height='10' viewBox='0 0 10 10'%3E%3Cpath fill='%2380848e' d='M5 7L0 2h10z'/%3E%3C/svg%3E");
  background-repeat: no-repeat; background-position: right 12px center;
  padding-right: 32px;
}
select.inp option { background: var(--bg3); }

/* TOGGLE */
.toggle-wrap { position: relative; flex-shrink: 0; }
.toggle-input { opacity: 0; width: 0; height: 0; position: absolute; }
.toggle-track {
  display: block; width: 44px; height: 24px;
  background: var(--bg5); border: 1px solid var(--border2);
  border-radius: 12px; cursor: pointer; transition: all .25s;
  position: relative;
}
.toggle-track::after {
  content: ''; position: absolute;
  width: 16px; height: 16px; border-radius: 50%;
  background: var(--text4); top: 3px; left: 4px;
  transition: all .25s cubic-bezier(.34,1.56,.64,1);
  box-shadow: 0 1px 4px rgba(0,0,0,.5);
}
.toggle-input:checked + .toggle-track {
  background: var(--blurple); border-color: var(--blurple);
  box-shadow: 0 0 16px var(--blurple-glow);
}
.toggle-input:checked + .toggle-track::after {
  background: #fff; left: 24px;
}

/* =========================================================
   DIVIDERS & SPACERS
   ========================================================= */
.sep { height: 1px; background: var(--border); }
.sep-v { width: 1px; background: var(--border); align-self: stretch; }

/* =========================================================
   BADGE / CHIP
   ========================================================= */
.chip {
  display: inline-flex; align-items: center; gap: 4px;
  padding: 3px 9px; border-radius: 99px;
  font-size: 11px; font-weight: 700;
}
.chip-blue   { background: var(--blurple-dim); color: #c7cbff; border: 1px solid rgba(88,101,242,.25); }
.chip-green  { background: var(--green-dim);   color: #a8ffd8; border: 1px solid rgba(35,209,139,.25); }
.chip-red    { background: var(--red-dim);     color: #ffb3b3; border: 1px solid rgba(240,71,71,.2); }
.chip-yellow { background: var(--yellow-dim);  color: #ffe59e; border: 1px solid rgba(250,168,26,.2); }
.chip-gray   { background: var(--bg4);         color: var(--text2); border: 1px solid var(--border2); }

/* =========================================================
   ROLE TAG
   ========================================================= */
.role-tag {
  display: inline-flex; align-items: center; gap: 5px;
  padding: 3px 9px 3px 7px; border-radius: 5px;
  font-size: 12px; font-weight: 700;
  background: var(--blurple-dim); color: #c9cdfb;
  border: 1px solid rgba(88,101,242,.2);
}
.role-tag::before { content: '@'; color: var(--blurple); font-size: 11px; }

/* =========================================================
   BUTTONS
   ========================================================= */
.btn {
  display: inline-flex; align-items: center; gap: 7px;
  padding: 9px 18px; border-radius: var(--radius-sm);
  font-family: 'Syne'; font-size: 13px; font-weight: 700;
  cursor: pointer; border: none; transition: all .2s;
  white-space: nowrap;
}
.btn-primary {
  background: var(--blurple); color: #fff;
  box-shadow: 0 2px 12px rgba(88,101,242,.3);
}
.btn-primary:hover {
  background: var(--blurple2);
  box-shadow: 0 4px 24px var(--blurple-glow);
  transform: translateY(-1px);
}
.btn-ghost {
  background: transparent; color: var(--text2);
  border: 1px solid var(--border2);
}
.btn-ghost:hover { background: var(--bg4); color: var(--text0); }
.btn-danger {
  background: var(--red-dim); color: var(--red);
  border: 1px solid rgba(240,71,71,.2);
}
.btn-danger:hover { background: rgba(240,71,71,.2); }
.btn-success {
  background: var(--green-dim); color: var(--green);
  border: 1px solid rgba(35,209,139,.2);
}
.btn-success:hover { background: rgba(35,209,139,.2); }
.btn-sm { padding: 6px 12px; font-size: 12px; }
.btn-icon { padding: 8px; border-radius: var(--radius-sm); }

/* =========================================================
   SAVE BAR
   ========================================================= */
.save-bar {
  position: sticky; bottom: 0;
  padding: 18px 0 4px;
  background: linear-gradient(to top, var(--bg0) 60%, transparent);
  display: flex; align-items: center; gap: 10px;
  justify-content: flex-end;
  margin-top: 28px; z-index: 50;
}

/* =========================================================
   DISCORD PREVIEW BOX
   ========================================================= */
.discord-preview {
  background: #313338;
  border-radius: var(--radius-md);
  padding: 14px;
}
.preview-badge {
  display: flex; align-items: center; gap: 6px;
  font-size: 9px; font-weight: 700; letter-spacing: 1.5px;
  text-transform: uppercase; color: var(--text4); margin-bottom: 10px;
}
.preview-badge::before {
  content: ''; width: 6px; height: 6px; border-radius: 50%;
  background: var(--green); box-shadow: 0 0 6px var(--green-glow);
}
.dc-embed {
  background: #2b2d31; border-radius: 4px;
  border-left: 4px solid var(--blurple);
  padding: 12px 14px; position: relative; overflow: hidden;
}
.dc-embed::after {
  content: ''; position: absolute; inset: 0;
  background: linear-gradient(135deg, rgba(88,101,242,.04), transparent);
  pointer-events: none;
}
.dc-embed-title { font-size: 14px; font-weight: 700; margin-bottom: 5px; color: var(--blurple); }
.dc-embed-body  { font-size: 12px; color: var(--text2); line-height: 1.6; }
.dc-embed-footer { font-size: 10px; color: var(--text4); margin-top: 8px; display: flex; align-items: center; gap: 5px; }
.dc-embed-field { margin-top: 8px; }
.dc-embed-field-name { font-size: 11px; font-weight: 700; color: var(--text1); margin-bottom: 2px; }
.dc-embed-field-val  { font-size: 12px; color: var(--text2); font-family: 'IBM Plex Mono'; }
.dc-msg-row { display: flex; align-items: flex-start; gap: 10px; margin-bottom: 6px; }
.dc-avatar {
  width: 34px; height: 34px; border-radius: 50%; flex-shrink: 0;
  background: linear-gradient(135deg, var(--blurple), var(--pink));
  display: flex; align-items: center; justify-content: center; font-size: 15px;
}
.dc-msg-author { font-size: 13px; font-weight: 700; color: var(--text0); display: flex; align-items: center; gap: 5px; }
.dc-bot-tag { background: var(--blurple); color: #fff; font-size: 9px; font-weight: 700; padding: 1px 5px; border-radius: 3px; letter-spacing: .3px; }
.dc-msg-text { font-size: 13px; color: var(--text2); margin-top: 2px; line-height: 1.5; }
.dc-btn {
  display: inline-flex; align-items: center; gap: 6px;
  padding: 8px 16px; border-radius: 4px;
  font-size: 13px; font-weight: 600; cursor: pointer;
  transition: opacity .15s; margin-top: 8px;
}
.dc-btn:hover { opacity: .85; }

/* =========================================================
   LOGS PAGE - SPECIAL STYLES
   ========================================================= */
.log-cat-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 14px;
}
.log-cat-card {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: var(--radius-lg);
  padding: 16px 18px;
  cursor: default; transition: all .2s;
  position: relative; overflow: hidden;
}
.log-cat-card::before {
  content: ''; position: absolute; top: 0; left: 0; right: 0; height: 2px;
  background: var(--lcc, var(--blurple)); opacity: .6;
  transition: opacity .2s;
}
.log-cat-card:hover { border-color: var(--border2); transform: translateY(-2px); }
.log-cat-card:hover::before { opacity: 1; }
.log-cat-header { display: flex; align-items: center; gap: 10px; margin-bottom: 12px; }
.log-cat-icon { font-size: 22px; }
.log-cat-name { font-size: 13px; font-weight: 700; color: var(--text0); }
.log-cat-count { font-size: 10px; color: var(--text4); margin-top: 1px; font-family: 'IBM Plex Mono'; }
.log-cat-items { display: flex; flex-direction: column; gap: 6px; }
.log-item-row {
  display: flex; align-items: center; justify-content: space-between;
  padding: 7px 10px;
  background: var(--bg3); border-radius: var(--radius-sm);
  border: 1px solid var(--border);
  transition: all .15s;
}
.log-item-row:hover { border-color: var(--border2); background: var(--bg4); }
.log-item-name { font-size: 12px; font-weight: 600; color: var(--text1); display: flex; align-items: center; gap: 6px; }
.log-item-dot { width: 6px; height: 6px; border-radius: 50%; flex-shrink: 0; }

/* =========================================================
   TICKET BUILDER
   ========================================================= */
.ticket-color-grid { display: flex; gap: 8px; }
.tc-swatch {
  width: 34px; height: 34px; border-radius: 8px;
  cursor: pointer; transition: all .2s; position: relative;
  border: 2px solid transparent;
  display: flex; align-items: center; justify-content: center;
}
.tc-swatch:hover { transform: scale(1.1); }
.tc-swatch.sel { border-color: #fff; transform: scale(1.12); }
.tc-swatch.sel::after {
  content: '✓'; font-size: 13px; font-weight: 800; color: #fff;
  text-shadow: 0 1px 4px rgba(0,0,0,.5);
}

/* =========================================================
   REACTION ROLES LIST
   ========================================================= */
.rr-list { display: flex; flex-direction: column; gap: 8px; }
.rr-row {
  display: flex; align-items: center; gap: 12px;
  padding: 11px 14px;
  background: var(--bg3); border: 1px solid var(--border);
  border-radius: var(--radius-md);
  transition: all .2s;
}
.rr-row:hover { border-color: var(--border2); background: var(--bg4); }
.rr-emoji-box {
  width: 38px; height: 38px; border-radius: 10px;
  background: var(--bg5); display: flex; align-items: center;
  justify-content: center; font-size: 20px; flex-shrink: 0;
}
.rr-info { flex: 1; min-width: 0; }
.rr-name { font-size: 13px; font-weight: 700; color: var(--text0); }
.rr-meta { font-size: 11px; color: var(--text4); display: flex; align-items: center; gap: 6px; margin-top: 1px; }
.rr-actions { display: flex; align-items: center; gap: 6px; margin-left: auto; }

/* =========================================================
   ADD DASHED BUTTON
   ========================================================= */
.add-btn-dashed {
  display: flex; align-items: center; justify-content: center; gap: 7px;
  padding: 10px; border-radius: var(--radius-md);
  border: 1px dashed rgba(88,101,242,.3); color: var(--blurple);
  background: transparent; font-family: 'Syne'; font-size: 12px; font-weight: 700;
  cursor: pointer; transition: all .2s;
}
.add-btn-dashed:hover { background: var(--blurple-dim); border-color: var(--blurple); }

/* =========================================================
   PROGRESS BARS
   ========================================================= */
.progress-track {
  height: 5px; background: var(--bg5); border-radius: 99px; overflow: hidden;
}
.progress-fill {
  height: 100%; border-radius: 99px;
  background: linear-gradient(90deg, var(--blurple), var(--cyan));
  animation: growW 1.4s cubic-bezier(.22,.61,.36,1) both;
  transform-origin: left;
}
@keyframes growW { from{width:0} }

/* =========================================================
   RECENT EVENTS LIST
   ========================================================= */
.event-list { display: flex; flex-direction: column; }
.event-row {
  display: flex; align-items: center; gap: 12px;
  padding: 11px 0;
  border-bottom: 1px solid var(--border);
}
.event-row:last-child { border-bottom: none; }
.event-icon-box {
  width: 36px; height: 36px; border-radius: 10px;
  display: flex; align-items: center; justify-content: center;
  font-size: 17px; flex-shrink: 0;
}
.event-body { flex: 1; min-width: 0; }
.event-title { font-size: 13px; font-weight: 600; color: var(--text1); white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
.event-meta { font-size: 11px; color: var(--text4); margin-top: 1px; display: flex; gap: 8px; }
.event-time { font-size: 10px; font-family: 'IBM Plex Mono'; color: var(--text4); flex-shrink: 0; }

/* =========================================================
   CHANNEL SELECTOR ROW
   ========================================================= */
.chan-row {
  display: flex; align-items: center; gap: 10px;
}
.chan-prefix { color: var(--text3); font-size: 15px; flex-shrink: 0; }

/* =========================================================
   INFO CALLOUT
   ========================================================= */
.callout {
  padding: 12px 14px;
  border-radius: var(--radius-md);
  font-size: 12px; line-height: 1.6;
  display: flex; gap: 10px; align-items: flex-start;
}
.callout-blue   { background: var(--blurple-dim); border: 1px solid rgba(88,101,242,.2); color: #c7cbff; }
.callout-green  { background: var(--green-dim);   border: 1px solid rgba(35,209,139,.2); color: #a8ffd8; }
.callout-yellow { background: var(--yellow-dim);  border: 1px solid rgba(250,168,26,.2); color: #ffe59e; }
.callout-red    { background: var(--red-dim);     border: 1px solid rgba(240,71,71,.2);  color: #ffb3b3; }
.callout-icon { font-size: 16px; flex-shrink: 0; margin-top: 1px; }

/* =========================================================
   TOAST
   ========================================================= */
.toast {
  position: fixed; bottom: 28px; right: 28px;
  background: var(--surface2);
  border: 1px solid var(--border2);
  border-radius: var(--radius-md);
  padding: 13px 18px;
  display: flex; align-items: center; gap: 10px;
  font-size: 13px; font-weight: 600;
  box-shadow: 0 8px 40px rgba(0,0,0,.5);
  transform: translateY(80px) scale(.95); opacity: 0;
  transition: all .35s cubic-bezier(.34,1.56,.64,1);
  z-index: 9999; max-width: 340px;
  pointer-events: none;
}
.toast.show { transform: translateY(0) scale(1); opacity: 1; pointer-events: auto; }
.toast-ico { font-size: 18px; }
.toast-txt { flex: 1; }

/* =========================================================
   ANIMATIONS
   ========================================================= */
@keyframes fadeUp {
  from { opacity: 0; transform: translateY(14px); }
  to   { opacity: 1; transform: translateY(0); }
}
.anim-fadeup { animation: fadeUp .4s ease both; }
.anim-d1 { animation-delay: .05s; }
.anim-d2 { animation-delay: .1s; }
.anim-d3 { animation-delay: .15s; }
.anim-d4 { animation-delay: .2s; }
.anim-d5 { animation-delay: .25s; }

/* number counter */
.count-num { display: inline-block; }

/* =========================================================
   CODE MONO SNIPPET
   ========================================================= */
.mono { font-family: 'IBM Plex Mono'; font-size: 12px; }
.code-snippet {
  background: var(--bg3); border: 1px solid var(--border2);
  border-radius: var(--radius-sm); padding: 8px 12px;
  font-family: 'IBM Plex Mono'; font-size: 11px; color: var(--text2);
  white-space: pre; overflow-x: auto;
}

/* =========================================================
   LOGS CHANNEL CHANNEL CONFIG TABLE
   ========================================================= */
.log-chan-table { display: flex; flex-direction: column; gap: 8px; }
.log-chan-row {
  display: grid;
  grid-template-columns: 200px 1fr 36px;
  align-items: center; gap: 10px;
  padding: 10px 14px;
  background: var(--bg3); border: 1px solid var(--border);
  border-radius: var(--radius-md); transition: border-color .2s;
}
.log-chan-row:hover { border-color: var(--border2); }
.log-chan-name { display: flex; align-items: center; gap: 8px; font-size: 13px; font-weight: 600; color: var(--text0); }
.log-chan-name .dot { width: 7px; height: 7px; border-radius: 50%; flex-shrink: 0; }

/* MISC */
.text-muted { color: var(--text3); }
.text-green  { color: var(--green); }
.text-red    { color: var(--red); }
.text-blurple{ color: var(--blurple); }
.text-yellow { color: var(--yellow); }
.text-cyan   { color: var(--cyan); }
.fw-700 { font-weight: 700; }
.font-mono { font-family: 'IBM Plex Mono'; }
.gap-4 { gap: 4px; }
</style>
</head>
<body>

<!-- noise overlay -->
<div class="bg-noise"></div>
<!-- animated canvas -->
<canvas id="bg-canvas"></canvas>

<div class="app">
<!-- ======================= SIDEBAR ======================= -->
<aside class="sidebar">
  <div class="sb-logo">
    <div class="sb-logo-mark">🤖</div>
    <div class="sb-logo-text">
      <div class="sb-logo-name"><em>ahh</em></div>
      <div class="sb-logo-tag">discord bot • v1.0.0</div>
    </div>
  </div>

  <div class="sb-server">
    <div class="sb-server-icon">🏰</div>
    <div class="sb-server-info">
      <div class="sb-server-name">My Awesome Server</div>
      <div class="sb-server-id">ID: 987654321098765</div>
    </div>
    <div class="sb-server-caret">▾</div>
  </div>

  <div class="sb-status">
    <div class="sb-status-dot"></div>
    <div class="sb-status-label">ahh is Online</div>
    <div class="sb-status-latency">12ms</div>
  </div>

  <nav class="sb-nav">
    <div class="sb-nav-section">
      <div class="sb-nav-label">Overview</div>
      <div class="nav-item active" onclick="nav('dashboard',this)">
        <span class="nav-item-icon">🏠</span>
        <span class="nav-item-label">Dashboard</span>
      </div>
      <div class="nav-item" onclick="nav('general',this)">
        <span class="nav-item-icon">⚙️</span>
        <span class="nav-item-label">General Settings</span>
      </div>
    </div>

    <div class="sb-nav-section">
      <div class="sb-nav-label">Features</div>
      <div class="nav-item" onclick="nav('welcome',this)">
        <span class="nav-item-icon">👋</span>
        <span class="nav-item-label">Welcome & Leave</span>
      </div>
      <div class="nav-item" onclick="nav('moderation',this)">
        <span class="nav-item-icon">🛡️</span>
        <span class="nav-item-label">Moderation</span>
        <span class="nav-badge">3</span>
      </div>
      <div class="nav-item" onclick="nav('tickets',this)">
        <span class="nav-item-icon">🎫</span>
        <span class="nav-item-label">Ticket System</span>
      </div>
      <div class="nav-item" onclick="nav('roles',this)">
        <span class="nav-item-icon">🎭</span>
        <span class="nav-item-label">Reaction Roles</span>
      </div>
      <div class="nav-item" onclick="nav('autorole',this)">
        <span class="nav-item-icon">🎖️</span>
        <span class="nav-item-label">Auto-Role</span>
      </div>
    </div>

    <div class="sb-nav-section">
      <div class="sb-nav-label">Logs</div>
      <div class="nav-item" onclick="nav('logs',this)">
        <span class="nav-item-icon">📋</span>
        <span class="nav-item-label">All Logs</span>
        <span class="nav-badge blue">12</span>
      </div>
      <div class="nav-item" onclick="nav('logs-msg',this)">
        <span class="nav-item-icon">💬</span>
        <span class="nav-item-label">Message Logs</span>
      </div>
      <div class="nav-item" onclick="nav('logs-member',this)">
        <span class="nav-item-icon">👥</span>
        <span class="nav-item-label">Member Logs</span>
      </div>
      <div class="nav-item" onclick="nav('logs-server',this)">
        <span class="nav-item-icon">🏛️</span>
        <span class="nav-item-label">Server Logs</span>
      </div>
      <div class="nav-item" onclick="nav('logs-mod',this)">
        <span class="nav-item-icon">⚖️</span>
        <span class="nav-item-label">Mod Action Logs</span>
        <span class="nav-badge" style="background:var(--red)">!</span>
      </div>
      <div class="nav-item" onclick="nav('logs-voice',this)">
        <span class="nav-item-icon">🎙️</span>
        <span class="nav-item-label">Voice Logs</span>
      </div>
      <div class="nav-item" onclick="nav('logs-role',this)">
        <span class="nav-item-icon">🏷️</span>
        <span class="nav-item-label">Role Logs</span>
      </div>
    </div>

    <div class="sb-nav-section">
      <div class="sb-nav-label">Advanced</div>
      <div class="nav-item" onclick="nav('antilink',this)">
        <span class="nav-item-icon">🔗</span>
        <span class="nav-item-label">Anti-Link</span>
      </div>
      <div class="nav-item" onclick="nav('antispam',this)">
        <span class="nav-item-icon">📨</span>
        <span class="nav-item-label">Anti-Spam</span>
      </div>
      <div class="nav-item" onclick="nav('api',this)">
        <span class="nav-item-icon">🔌</span>
        <span class="nav-item-label">API / Config</span>
        <span class="nav-badge green">JS</span>
      </div>
    </div>
  </nav>

  <div class="sb-footer">
    <div class="sb-user">
      <div class="sb-user-avatar">👑</div>
      <div>
        <div class="sb-user-name">Admin</div>
        <div class="sb-user-role">Server Owner</div>
      </div>
      <div class="sb-user-settings">⚙</div>
    </div>
  </div>
</aside>

<!-- ======================= MAIN ======================= -->
<div class="main">

  <!-- TOP BAR -->
  <header class="topbar">
    <div class="topbar-title" id="tb-title">Dashboard <span>/ Overview</span></div>
    <div class="topbar-actions">
      <button class="tb-btn" onclick="showToast('🔄 Refreshing data...','blue')">↺ Refresh</button>
      <button class="tb-btn" onclick="nav('api',document.querySelector(\'[onclick*=api]\'))">🔌 API Config</button>
      <button class="tb-btn primary" onclick="saveAll()">💾 Save All</button>
    </div>
  </header>

  <div class="page">

<!-- ============================================================
     PAGE: DASHBOARD
     ============================================================ -->
<div class="page-section active anim-fadeup" id="page-dashboard">
  <div class="page-head">
    <div class="breadcrumb">ahh <span>/</span> overview</div>
    <div class="page-head-row">
      <div>
        <div class="page-title">Welcome back, <span class="hl">Admin</span> 👋</div>
        <div class="page-desc">Here's a live overview of your server powered by <strong style="color:var(--blurple)">ahh</strong></div>
      </div>
      <div style="display:flex;gap:8px">
        <span class="chip chip-green">● Online</span>
        <span class="chip chip-blue">v1.0.0</span>
      </div>
    </div>
  </div>

  <!-- STATS -->
  <div class="stats-grid">
    <div class="stat-card anim-fadeup anim-d1" style="--card-glow:rgba(88,101,242,.07)">
      <span class="stat-card-emoji">🖥️</span>
      <div class="stat-card-label">Servers</div>
      <div class="stat-card-value text-blurple count-num" data-target="1247">0</div>
      <div class="stat-card-delta up">↑ +12 this week</div>
    </div>
    <div class="stat-card anim-fadeup anim-d2" style="--card-glow:rgba(0,210,255,.06)">
      <span class="stat-card-emoji">👥</span>
      <div class="stat-card-label">Total Users</div>
      <div class="stat-card-value text-cyan count-num" data-target="89432">0</div>
      <div class="stat-card-delta up">↑ +340 today</div>
    </div>
    <div class="stat-card anim-fadeup anim-d3" style="--card-glow:rgba(35,209,139,.06)">
      <span class="stat-card-emoji">✅</span>
      <div class="stat-card-label">Commands Run</div>
      <div class="stat-card-value text-green count-num" data-target="45821">0</div>
      <div class="stat-card-delta up">↑ +1.2k today</div>
    </div>
    <div class="stat-card anim-fadeup anim-d4" style="--card-glow:rgba(250,168,26,.05)">
      <span class="stat-card-emoji">🎫</span>
      <div class="stat-card-label">Open Tickets</div>
      <div class="stat-card-value text-yellow count-num" data-target="7">0</div>
      <div class="stat-card-delta down">↓ -2 from yesterday</div>
    </div>
  </div>

  <!-- 2 col -->
  <div class="grid-2" style="margin-bottom:18px">
    <!-- Activity -->
    <div class="panel anim-fadeup anim-d2">
      <div class="panel-head">
        <div class="panel-icon pi-blue">📊</div>
        <div><div class="panel-title">Activity</div><div class="panel-sub">Today's server stats</div></div>
      </div>
      <div class="panel-body">
        <div style="display:flex;flex-direction:column;gap:14px">
          <div>
            <div style="display:flex;justify-content:space-between;margin-bottom:6px">
              <span style="font-size:12px;color:var(--text3)">Messages</span>
              <span class="mono text-blurple" style="font-size:12px">1,248</span>
            </div>
            <div class="progress-track"><div class="progress-fill" style="width:72%;animation-delay:.2s"></div></div>
          </div>
          <div>
            <div style="display:flex;justify-content:space-between;margin-bottom:6px">
              <span style="font-size:12px;color:var(--text3)">New Members</span>
              <span class="mono text-green" style="font-size:12px">34</span>
            </div>
            <div class="progress-track"><div class="progress-fill" style="width:34%;background:linear-gradient(90deg,var(--green),var(--teal));animation-delay:.35s"></div></div>
          </div>
          <div>
            <div style="display:flex;justify-content:space-between;margin-bottom:6px">
              <span style="font-size:12px;color:var(--text3)">Moderation Actions</span>
              <span class="mono text-red" style="font-size:12px">5</span>
            </div>
            <div class="progress-track"><div class="progress-fill" style="width:12%;background:linear-gradient(90deg,var(--red),var(--orange));animation-delay:.5s"></div></div>
          </div>
          <div>
            <div style="display:flex;justify-content:space-between;margin-bottom:6px">
              <span style="font-size:12px;color:var(--text3)">Tickets Opened</span>
              <span class="mono text-yellow" style="font-size:12px">7</span>
            </div>
            <div class="progress-track"><div class="progress-fill" style="width:18%;background:linear-gradient(90deg,var(--yellow),var(--orange));animation-delay:.65s"></div></div>
          </div>
          <div>
            <div style="display:flex;justify-content:space-between;margin-bottom:6px">
              <span style="font-size:12px;color:var(--text3)">Anti-Spam Blocks</span>
              <span class="mono text-cyan" style="font-size:12px">23</span>
            </div>
            <div class="progress-track"><div class="progress-fill" style="width:28%;background:linear-gradient(90deg,var(--cyan),var(--blurple));animation-delay:.8s"></div></div>
          </div>
        </div>
      </div>
    </div>

    <!-- Recent Events -->
    <div class="panel anim-fadeup anim-d3">
      <div class="panel-head">
        <div class="panel-icon pi-green">🔔</div>
        <div><div class="panel-title">Recent Events</div><div class="panel-sub">Live server activity</div></div>
        <div class="panel-extra"><span class="chip chip-green" style="font-size:9px">LIVE</span></div>
      </div>
      <div class="panel-body-compact" style="padding:0 22px">
        <div class="event-list">
          <div class="event-row">
            <div class="event-icon-box" style="background:var(--green-dim)">👋</div>
            <div class="event-body">
              <div class="event-title">JohnDoe#1234 joined the server</div>
              <div class="event-meta"><span>Member Join</span><span class="chip chip-green" style="padding:1px 6px">+1</span></div>
            </div>
            <div class="event-time">2m ago</div>
          </div>
          <div class="event-row">
            <div class="event-icon-box" style="background:var(--blurple-dim)">🎫</div>
            <div class="event-body">
              <div class="event-title">Ticket #42 opened by Alice</div>
              <div class="event-meta"><span>Support</span></div>
            </div>
            <div class="event-time">15m ago</div>
          </div>
          <div class="event-row">
            <div class="event-icon-box" style="background:var(--red-dim)">🛡️</div>
            <div class="event-body">
              <div class="event-title">Spam blocked — 5 messages deleted</div>
              <div class="event-meta"><span>Auto-Mod</span><span class="chip chip-red" style="padding:1px 6px">Blocked</span></div>
            </div>
            <div class="event-time">1h ago</div>
          </div>
          <div class="event-row">
            <div class="event-icon-box" style="background:var(--yellow-dim)">🔗</div>
            <div class="event-body">
              <div class="event-title">Anti-link triggered by Bob#9999</div>
              <div class="event-meta"><span>Anti-Link</span></div>
            </div>
            <div class="event-time">2h ago</div>
          </div>
          <div class="event-row">
            <div class="event-icon-box" style="background:var(--pink-dim)">🎭</div>
            <div class="event-body">
              <div class="event-title">Gamer role assigned via reaction</div>
              <div class="event-meta"><span>Reaction Role</span></div>
            </div>
            <div class="event-time">3h ago</div>
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- quick enable strip -->
  <div class="panel anim-fadeup anim-d4">
    <div class="panel-head">
      <div class="panel-icon pi-cyan">⚡</div>
      <div><div class="panel-title">Quick Toggles</div><div class="panel-sub">Enable/disable features instantly</div></div>
    </div>
    <div style="padding:18px 22px;display:grid;grid-template-columns:repeat(5,1fr);gap:12px">
      <div style="display:flex;flex-direction:column;align-items:center;gap:8px;padding:14px;background:var(--bg3);border-radius:var(--radius-md);border:1px solid var(--border)">
        <span style="font-size:24px">👋</span>
        <span style="font-size:11px;font-weight:700;color:var(--text2)">Welcome</span>
        <div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="qt1" checked><label class="toggle-track" for="qt1"></label></div>
      </div>
      <div style="display:flex;flex-direction:column;align-items:center;gap:8px;padding:14px;background:var(--bg3);border-radius:var(--radius-md);border:1px solid var(--border)">
        <span style="font-size:24px">🔗</span>
        <span style="font-size:11px;font-weight:700;color:var(--text2)">Anti-Link</span>
        <div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="qt2" checked><label class="toggle-track" for="qt2"></label></div>
      </div>
      <div style="display:flex;flex-direction:column;align-items:center;gap:8px;padding:14px;background:var(--bg3);border-radius:var(--radius-md);border:1px solid var(--border)">
        <span style="font-size:24px">📨</span>
        <span style="font-size:11px;font-weight:700;color:var(--text2)">Anti-Spam</span>
        <div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="qt3" checked><label class="toggle-track" for="qt3"></label></div>
      </div>
      <div style="display:flex;flex-direction:column;align-items:center;gap:8px;padding:14px;background:var(--bg3);border-radius:var(--radius-md);border:1px solid var(--border)">
        <span style="font-size:24px">🎫</span>
        <span style="font-size:11px;font-weight:700;color:var(--text2)">Tickets</span>
        <div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="qt4"><label class="toggle-track" for="qt4"></label></div>
      </div>
      <div style="display:flex;flex-direction:column;align-items:center;gap:8px;padding:14px;background:var(--bg3);border-radius:var(--radius-md);border:1px solid var(--border)">
        <span style="font-size:24px">📋</span>
        <span style="font-size:11px;font-weight:700;color:var(--text2)">Logging</span>
        <div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="qt5" checked><label class="toggle-track" for="qt5"></label></div>
      </div>
    </div>
  </div>
</div>

<!-- ============================================================
     PAGE: GENERAL
     ============================================================ -->
<div class="page-section" id="page-general">
  <div class="page-head">
    <div class="breadcrumb">ahh <span>/</span> settings</div>
    <div class="page-title">General <span class="hl">Settings</span></div>
    <div class="page-desc">Core bot configuration — prefix, language, embed colors</div>
  </div>
  <div class="grid-2">
    <div class="panel">
      <div class="panel-head">
        <div class="panel-icon pi-blue">⚙️</div>
        <div><div class="panel-title">Basic Config</div><div class="panel-sub">Prefix, language, timezone</div></div>
      </div>
      <div class="panel-body">
        <div class="field">
          <div class="field-label">Command Prefix</div>
          <input class="inp mono" style="max-width:80px;font-size:18px;font-weight:800;text-align:center" value="!">
          <div class="field-hint">Trigger all ahh commands with this prefix</div>
        </div>
        <div class="field">
          <div class="field-label">Bot Language</div>
          <select class="inp">
            <option>🇺🇸 English (US)</option><option>🇫🇷 Français</option>
            <option>🇦🇪 العربية</option><option>🇩🇪 Deutsch</option><option>🇪🇸 Español</option>
          </select>
        </div>
        <div class="field">
          <div class="field-label">Timezone</div>
          <select class="inp">
            <option>UTC+0 (GMT)</option><option>UTC+1 (CET)</option>
            <option>UTC+3 (AST / Tunis / Cairo)</option><option>UTC-5 (EST)</option>
            <option>UTC-8 (PST)</option>
          </select>
        </div>
        <div class="field">
          <div class="field-label">Date Format</div>
          <select class="inp">
            <option>DD/MM/YYYY HH:mm</option><option>MM/DD/YYYY HH:mm</option>
            <option>YYYY-MM-DD HH:mm</option>
          </select>
        </div>
      </div>
    </div>
    <div class="panel">
      <div class="panel-head">
        <div class="panel-icon pi-pink">🎨</div>
        <div><div class="panel-title">Appearance</div><div class="panel-sub">Embed color & bot nickname</div></div>
      </div>
      <div class="panel-body">
        <div class="field">
          <div class="field-label">Embed Accent Color</div>
          <div style="display:flex;gap:10px;align-items:center">
            <input type="color" value="#5865F2" id="accentColor" oninput="updateAccent(this)" style="width:46px;height:46px;border:2px solid var(--border2);border-radius:10px;cursor:pointer;background:none;padding:2px">
            <input class="inp mono" value="#5865F2" id="accentHex" style="max-width:110px" oninput="updateAccentFromHex(this)">
          </div>
          <div class="field-hint">Used in all embed borders throughout your server</div>
        </div>
        <div class="field">
          <div class="field-label">Bot Nickname (this server)</div>
          <input class="inp" placeholder="ahh" value="ahh">
        </div>
        <div class="field">
          <div class="field-label">Bot Status Message</div>
          <input class="inp" value="!help | ahh bot" placeholder="Playing ...">
        </div>
        <div id="accent-preview" style="padding:12px 14px;background:var(--bg3);border-radius:var(--radius-md);border-left:4px solid var(--blurple)">
          <div style="font-size:13px;font-weight:700;color:var(--blurple)" id="accent-preview-title">Color Preview</div>
          <div style="font-size:12px;color:var(--text3);margin-top:3px">This accent will be used for all bot embeds.</div>
        </div>
      </div>
    </div>
  </div>
  <div class="save-bar">
    <button class="btn btn-ghost" onclick="showToast('Changes discarded','red')">Discard</button>
    <button class="btn btn-primary" onclick="save('general')">💾 Save Changes</button>
  </div>
</div>

<!-- ============================================================
     PAGE: WELCOME
     ============================================================ -->
<div class="page-section" id="page-welcome">
  <div class="page-head">
    <div class="breadcrumb">ahh <span>/</span> welcome</div>
    <div class="page-title"><span class="hl">Welcome</span> & Leave System</div>
    <div class="page-desc">Configure greeting messages when members join or leave your server</div>
  </div>
  <div class="grid-2">
    <div class="panel">
      <div class="panel-head" style="--panel-accent:var(--green)">
        <div class="panel-icon pi-green">👋</div>
        <div><div class="panel-title">Welcome System</div><div class="panel-sub">Configure join messages</div></div>
        <div class="panel-head-glow"></div>
      </div>
      <div class="panel-body">
        <div class="field-row-inline">
          <div class="field-row-label"><div class="field-label">Enable Welcome Messages</div><div class="field-hint">Send embed when someone joins</div></div>
          <div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="tw1" checked onchange="updateWelcomePreview()"><label class="toggle-track" for="tw1"></label></div>
        </div>
        <div class="sep"></div>
        <div class="field">
          <div class="field-label">Welcome Channel</div>
          <div class="chan-row"><span class="chan-prefix">#</span>
            <select class="inp"><option>welcome</option><option>general</option><option>introductions</option></select>
          </div>
        </div>
        <div class="field">
          <div class="field-label">Welcome Message</div>
          <textarea class="inp" id="wc-msg" oninput="updateWelcomePreview()">Welcome {user} to **{server}**! 🎉 You are member **#{count}**.\nWe hope you enjoy your stay!</textarea>
          <div class="field-hint">Variables: {user} {server} {count} {id}</div>
        </div>
        <div class="field">
          <div class="field-label">Embed Title</div>
          <input class="inp" id="wc-title" value="👋 New Member!" oninput="updateWelcomePreview()">
        </div>
        <div class="field">
          <div class="field-label">Banner / Image URL</div>
          <input class="inp" placeholder="https://i.imgur.com/banner.png">
        </div>
        <div class="field">
          <div class="field-label">Embed Color</div>
          <select class="inp" id="wc-color" onchange="updateWelcomePreview()">
            <option value="var(--green)">🟢 Green</option>
            <option value="var(--blurple)">🔵 Blurple</option>
            <option value="var(--cyan)">🩵 Cyan</option>
            <option value="var(--pink)">🩷 Pink</option>
          </select>
        </div>
      </div>
    </div>

    <div style="display:flex;flex-direction:column;gap:18px">
      <!-- Live Preview -->
      <div class="panel">
        <div class="panel-head">
          <div class="panel-icon pi-yellow">👁️</div>
          <div><div class="panel-title">Live Preview</div><div class="panel-sub">Realtime Discord embed preview</div></div>
          <div class="panel-extra"><span class="chip chip-green" style="font-size:9px">LIVE</span></div>
        </div>
        <div class="panel-body">
          <div class="discord-preview">
            <div class="preview-badge">Discord Preview</div>
            <div class="dc-msg-row">
              <div class="dc-avatar">🤖</div>
              <div>
                <div class="dc-msg-author">ahh <span class="dc-bot-tag">BOT</span></div>
                <div class="dc-embed" id="wc-preview-embed" style="border-left-color:var(--green);margin-top:6px">
                  <div class="dc-embed-title" id="wc-preview-title">👋 New Member!</div>
                  <div class="dc-embed-body" id="wc-preview-body">Welcome <strong style="color:var(--blurple)">@NewUser</strong> to <strong>My Awesome Server</strong>! 🎉 You are member <strong>#1,234</strong>.<br>We hope you enjoy your stay!</div>
                  <div class="dc-embed-footer"><span>🤖</span> ahh • Today at 12:00 PM</div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- Leave -->
      <div class="panel">
        <div class="panel-head" style="--panel-accent:var(--red)">
          <div class="panel-icon pi-red">🚪</div>
          <div><div class="panel-title">Leave System</div><div class="panel-sub">Configure leave messages</div></div>
          <div class="panel-head-glow"></div>
        </div>
        <div class="panel-body">
          <div class="field-row-inline">
            <div class="field-row-label"><div class="field-label">Enable Leave Messages</div><div class="field-hint">Notify when members leave</div></div>
            <div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="tl1"><label class="toggle-track" for="tl1"></label></div>
          </div>
          <div class="field">
            <div class="field-label">Leave Channel</div>
            <div class="chan-row"><span class="chan-prefix">#</span>
              <select class="inp"><option>welcome</option><option>logs</option></select>
            </div>
          </div>
          <div class="field">
            <div class="field-label">Leave Message</div>
            <input class="inp" value="Goodbye {user}, hope to see you again! 👋">
          </div>
        </div>
      </div>
    </div>
  </div>
  <div class="save-bar">
    <button class="btn btn-ghost">Discard</button>
    <button class="btn btn-primary" onclick="save('welcome')">💾 Save Changes</button>
  </div>
</div>

<!-- ============================================================
     PAGE: MODERATION
     ============================================================ -->
<div class="page-section" id="page-moderation">
  <div class="page-head">
    <div class="breadcrumb">ahh <span>/</span> moderation</div>
    <div class="page-title"><span class="hl">Moderation</span> & Security</div>
    <div class="page-desc">Auto-moderation, anti-abuse systems, and punishment settings</div>
  </div>
  <div class="grid-2">
    <div class="panel">
      <div class="panel-head"><div class="panel-icon pi-red">🛡️</div><div><div class="panel-title">Auto-Mod Toggles</div><div class="panel-sub">Protect your server automatically</div></div></div>
      <div class="panel-body">
        <div class="field-row-inline">
          <div class="field-row-label"><div class="field-label">🔗 Anti-Link</div><div class="field-hint">Block unauthorized links from being posted</div></div>
          <div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="tal" checked><label class="toggle-track" for="tal"></label></div>
        </div>
        <div class="sep"></div>
        <div class="field-row-inline">
          <div class="field-row-label"><div class="field-label">📨 Anti-Spam</div><div class="field-hint">Limit message flood attacks automatically</div></div>
          <div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="tas" checked><label class="toggle-track" for="tas"></label></div>
        </div>
        <div class="sep"></div>
        <div class="field-row-inline">
          <div class="field-row-label"><div class="field-label">🤖 Anti-Bot</div><div class="field-hint">Prevent unauthorized bots from joining</div></div>
          <div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="tab"><label class="toggle-track" for="tab"></label></div>
        </div>
        <div class="sep"></div>
        <div class="field-row-inline">
          <div class="field-row-label"><div class="field-label">🚫 Anti-Invite</div><div class="field-hint">Block Discord server invite links</div></div>
          <div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="tai" checked><label class="toggle-track" for="tai"></label></div>
        </div>
        <div class="sep"></div>
        <div class="field-row-inline">
          <div class="field-row-label"><div class="field-label">⚠️ Anti-Caps</div><div class="field-hint">Limit excessive CAPS LOCK usage</div></div>
          <div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="tac"><label class="toggle-track" for="tac"></label></div>
        </div>
        <div class="sep"></div>
        <div class="field-row-inline">
          <div class="field-row-label"><div class="field-label">😈 Anti-Profanity</div><div class="field-hint">Block blacklisted words/phrases</div></div>
          <div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="tap"><label class="toggle-track" for="tap"></label></div>
        </div>
      </div>
    </div>

    <div style="display:flex;flex-direction:column;gap:18px">
      <div class="panel">
        <div class="panel-head"><div class="panel-icon pi-orange">⚖️</div><div><div class="panel-title">Punishment Settings</div><div class="panel-sub">What happens when rules are broken</div></div></div>
        <div class="panel-body">
          <div class="field"><div class="field-label">Anti-Spam Punishment</div>
            <select class="inp"><option>🗑️ Delete Message</option><option>⏰ Timeout (5m)</option><option>⏰ Timeout (1h)</option><option>🔇 Mute</option><option>👢 Kick</option><option>🔨 Ban</option></select>
          </div>
          <div class="field"><div class="field-label">Anti-Link Punishment</div>
            <select class="inp"><option>🗑️ Delete Message</option><option>⚠️ Warn + Delete</option><option>⏰ Timeout (10m)</option><option>👢 Kick</option></select>
          </div>
          <div class="field"><div class="field-label">Spam Threshold (msgs/10s)</div>
            <input class="inp mono" type="number" value="5" min="3" max="20" style="max-width:80px">
          </div>
          <div class="field"><div class="field-label">Max Warns Before Ban</div>
            <input class="inp mono" type="number" value="3" min="1" max="10" style="max-width:80px">
          </div>
        </div>
      </div>

      <div class="panel">
        <div class="panel-head"><div class="panel-icon pi-yellow">📋</div><div><div class="panel-title">Whitelisted Roles</div><div class="panel-sub">These roles bypass auto-mod</div></div></div>
        <div class="panel-body">
          <div style="display:flex;flex-wrap:wrap;gap:6px">
            <span class="role-tag">Admin</span>
            <span class="role-tag">Moderator</span>
            <span class="role-tag">Staff</span>
          </div>
          <div class="field"><div class="field-label">Add Role</div>
            <div style="display:flex;gap:8px"><select class="inp"><option>Choose a role...</option><option>Helper</option><option>Trusted</option><option>VIP</option></select>
            <button class="btn btn-success btn-sm">Add</button></div>
          </div>
        </div>
      </div>
    </div>
  </div>
  <div class="save-bar">
    <button class="btn btn-ghost">Discard</button>
    <button class="btn btn-primary" onclick="save('moderation')">💾 Save Changes</button>
  </div>
</div>

<!-- ============================================================
     PAGE: TICKETS
     ============================================================ -->
<div class="page-section" id="page-tickets">
  <div class="page-head">
    <div class="breadcrumb">ahh <span>/</span> tickets</div>
    <div class="page-title"><span class="hl">Ticket</span> System</div>
    <div class="page-desc">Build your support ticket button and configure ticket channels</div>
  </div>
  <div class="grid-2">
    <div class="panel">
      <div class="panel-head"><div class="panel-icon pi-cyan">🎫</div><div><div class="panel-title">Button Builder</div><div class="panel-sub">Customize your open-ticket button</div></div></div>
      <div class="panel-body">
        <div class="field-row-inline">
          <div class="field-row-label"><div class="field-label">Enable Ticket System</div><div class="field-hint">Activate the whole system</div></div>
          <div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="tts" checked><label class="toggle-track" for="tts"></label></div>
        </div>
        <div class="sep"></div>
        <div class="field"><div class="field-label">Button Text</div>
          <input class="inp" id="tk-txt" value="🎫 Open a Ticket" oninput="updateTicketPreview()">
        </div>
        <div class="field"><div class="field-label">Button Color</div>
          <div class="ticket-color-grid">
            <div class="tc-swatch sel" style="background:var(--blurple)" onclick="pickTkColor(this,'var(--blurple)')"></div>
            <div class="tc-swatch" style="background:var(--green)" onclick="pickTkColor(this,'var(--green)')"></div>
            <div class="tc-swatch" style="background:var(--red)" onclick="pickTkColor(this,'var(--red)')"></div>
            <div class="tc-swatch" style="background:#4f545c" onclick="pickTkColor(this,'#4f545c')"></div>
          </div>
        </div>
        <div class="field"><div class="field-label">Ticket Category</div>
          <select class="inp"><option>📂 Support</option><option>📂 Appeals</option><option>📂 Reports</option><option>📂 Applications</option><option>📂 Billing</option></select>
        </div>
        <div class="field"><div class="field-label">Ticket Message</div>
          <textarea class="inp" style="min-height:60px">Need help? Click the button below to open a private support ticket with our team!</textarea>
        </div>
        <div class="field"><div class="field-label">Setup Command</div>
          <div class="code-snippet">!setup-ticket</div>
          <div class="field-hint">Run this in a channel to place the button</div>
        </div>
      </div>
    </div>

    <div style="display:flex;flex-direction:column;gap:18px">
      <div class="panel">
        <div class="panel-head"><div class="panel-icon pi-yellow">👁️</div><div><div class="panel-title">Button Preview</div><div class="panel-sub">How it looks in Discord</div></div></div>
        <div class="panel-body">
          <div class="discord-preview">
            <div class="preview-badge">Discord Preview</div>
            <div class="dc-msg-row">
              <div class="dc-avatar">🤖</div>
              <div style="flex:1">
                <div class="dc-msg-author">ahh <span class="dc-bot-tag">BOT</span></div>
                <div class="dc-embed" style="border-left-color:var(--blurple);margin-top:6px">
                  <div class="dc-embed-title">🎫 Support System</div>
                  <div class="dc-embed-body">Need help? Click the button below to open a private support ticket with our team!</div>
                </div>
                <div id="tk-btn-preview" class="dc-btn" style="background:var(--blurple);color:#fff">🎫 Open a Ticket</div>
              </div>
            </div>
          </div>
        </div>
      </div>

      <div class="panel">
        <div class="panel-head"><div class="panel-icon pi-green">⚙️</div><div><div class="panel-title">Ticket Options</div><div class="panel-sub">Advanced ticket configuration</div></div></div>
        <div class="panel-body">
          <div class="field-row-inline">
            <div class="field-row-label"><div class="field-label">Ping Support Role on Open</div><div class="field-hint">Notify staff when a ticket opens</div></div>
            <div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="tto1" checked><label class="toggle-track" for="tto1"></label></div>
          </div>
          <div class="sep"></div>
          <div class="field-row-inline">
            <div class="field-row-label"><div class="field-label">Transcript on Close</div><div class="field-hint">Save ticket logs as HTML</div></div>
            <div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="tto2" checked><label class="toggle-track" for="tto2"></label></div>
          </div>
          <div class="sep"></div>
          <div class="field"><div class="field-label">Support Role</div>
            <select class="inp"><option>👮 Moderator</option><option>🛡️ Staff</option><option>🆘 Support Team</option></select>
          </div>
          <div class="field"><div class="field-label">Max Open Tickets Per User</div>
            <input class="inp mono" type="number" value="1" min="1" max="5" style="max-width:70px">
          </div>
        </div>
      </div>
    </div>
  </div>
  <div class="save-bar">
    <button class="btn btn-ghost">Discard</button>
    <button class="btn btn-primary" onclick="save('tickets')">💾 Save Changes</button>
  </div>
</div>

<!-- ============================================================
     PAGE: REACTION ROLES
     ============================================================ -->
<div class="page-section" id="page-roles">
  <div class="page-head">
    <div class="breadcrumb">ahh <span>/</span> roles</div>
    <div class="page-title"><span class="hl">Reaction</span> Roles</div>
    <div class="page-desc">Let members self-assign roles by clicking emoji reactions. Run <code class="mono" style="font-size:11px;color:var(--blurple)">!setup-roles</code> in any channel.</div>
  </div>
  <div class="panel">
    <div class="panel-head">
      <div class="panel-icon pi-pink">🎭</div>
      <div><div class="panel-title">Configured Reaction Roles</div><div class="panel-sub" id="rr-count-sub">4 roles configured</div></div>
    </div>
    <div class="panel-body">
      <div class="rr-list" id="rr-list">
        <div class="rr-row">
          <div class="rr-emoji-box">🎮</div>
          <div class="rr-info"><div class="rr-name">Gamer</div><div class="rr-meta"><span class="role-tag" style="font-size:11px">Gamer</span><span>React with 🎮 to get</span></div></div>
          <div class="rr-actions"><span class="chip chip-blue">gamer_role</span><button class="btn btn-danger btn-sm btn-icon" onclick="removeRR(this)">✕</button></div>
        </div>
        <div class="rr-row">
          <div class="rr-emoji-box">🎵</div>
          <div class="rr-info"><div class="rr-name">Music Lover</div><div class="rr-meta"><span class="role-tag" style="font-size:11px">Music</span><span>React with 🎵 to get</span></div></div>
          <div class="rr-actions"><span class="chip chip-blue">music_role</span><button class="btn btn-danger btn-sm btn-icon" onclick="removeRR(this)">✕</button></div>
        </div>
        <div class="rr-row">
          <div class="rr-emoji-box">💻</div>
          <div class="rr-info"><div class="rr-name">Developer</div><div class="rr-meta"><span class="role-tag" style="font-size:11px">Dev</span><span>React with 💻 to get</span></div></div>
          <div class="rr-actions"><span class="chip chip-blue">dev_role</span><button class="btn btn-danger btn-sm btn-icon" onclick="removeRR(this)">✕</button></div>
        </div>
        <div class="rr-row">
          <div class="rr-emoji-box">🎨</div>
          <div class="rr-info"><div class="rr-name">Artist</div><div class="rr-meta"><span class="role-tag" style="font-size:11px">Artist</span><span>React with 🎨 to get</span></div></div>
          <div class="rr-actions"><span class="chip chip-blue">artist_role</span><button class="btn btn-danger btn-sm btn-icon" onclick="removeRR(this)">✕</button></div>
        </div>
      </div>
      <button class="add-btn-dashed" onclick="addRR()">+ Add Reaction Role</button>
    </div>
  </div>
  <div class="save-bar">
    <button class="btn btn-ghost">Discard</button>
    <button class="btn btn-primary" onclick="save('roles')">💾 Save Changes</button>
  </div>
</div>

<!-- ============================================================
     PAGE: AUTO-ROLE
     ============================================================ -->
<div class="page-section" id="page-autorole">
  <div class="page-head">
    <div class="breadcrumb">ahh <span>/</span> auto-role</div>
    <div class="page-title"><span class="hl">Auto-Role</span></div>
    <div class="page-desc">Automatically assign roles to new members when they join</div>
  </div>
  <div class="grid-2">
    <div class="panel">
      <div class="panel-head"><div class="panel-icon pi-blue">🎖️</div><div><div class="panel-title">Default Role</div><div class="panel-sub">Assigned on join</div></div></div>
      <div class="panel-body">
        <div class="field-row-inline">
          <div class="field-row-label"><div class="field-label">Enable Auto-Role</div><div class="field-hint">Assign role when member joins</div></div>
          <div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="tar1" checked><label class="toggle-track" for="tar1"></label></div>
        </div>
        <div class="sep"></div>
        <div class="field"><div class="field-label">Role to Assign</div>
          <select class="inp"><option>🟢 Member</option><option>🔵 Verified</option><option>⚪ Unverified</option><option>🟡 New Member</option></select>
        </div>
        <div class="field-row-inline">
          <div class="field-row-label"><div class="field-label">Also assign to bots</div><div class="field-hint">Give auto-role to bot accounts too</div></div>
          <div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="tar2"><label class="toggle-track" for="tar2"></label></div>
        </div>
        <div class="callout callout-blue"><span class="callout-icon">ℹ️</span><span>Make sure <strong>ahh</strong>'s role is <strong>above</strong> the target role in your server's role hierarchy, otherwise the bot cannot assign it.</span></div>
      </div>
    </div>
    <div class="panel">
      <div class="panel-head"><div class="panel-icon pi-green">✅</div><div><div class="panel-title">Role ID Config</div><div class="panel-sub">The role ID from your bot code</div></div></div>
      <div class="panel-body">
        <div class="field"><div class="field-label">Auto-Role ID (from bot config)</div>
          <input class="inp mono" placeholder="ID_AUTO_ROLE — paste your role ID here" value="">
          <div class="field-hint">Copy from <code class="mono">AUTO_ROLE_ID</code> in your bot's index.js</div>
        </div>
        <div class="field"><div class="field-label">Gamer Role ID</div>
          <input class="inp mono" placeholder="ID_GAMER_ROLE" value="">
          <div class="field-hint">Used in the <code class="mono">!setup-roles</code> command</div>
        </div>
        <div class="code-snippet">{`const AUTO_ROLE_ID  = 'YOUR_ROLE_ID_HERE';
const GAMER_ROLE_ID = 'YOUR_ROLE_ID_HERE';`}</div>
      </div>
    </div>
  </div>
  <div class="save-bar">
    <button class="btn btn-ghost">Discard</button>
    <button class="btn btn-primary" onclick="save('autorole')">💾 Save Changes</button>
  </div>
</div>

<!-- ============================================================
     PAGE: ALL LOGS
     ============================================================ -->
<div class="page-section" id="page-logs">
  <div class="page-head">
    <div class="breadcrumb">ahh <span>/</span> logs</div>
    <div class="page-title"><span class="hl">Logging</span> System</div>
    <div class="page-desc">Track every action in your server — messages, members, roles, voice, and moderation</div>
  </div>

  <!-- Channel Config Table -->
  <div class="panel" style="margin-bottom:18px">
    <div class="panel-head">
      <div class="panel-icon pi-blue">📡</div>
      <div><div class="panel-title">Log Channel Configuration</div><div class="panel-sub">Where each log category sends its embeds</div></div>
    </div>
    <div class="panel-body">
      <div class="log-chan-table">
        <div class="log-chan-row">
          <div class="log-chan-name"><div class="dot" style="background:var(--red)"></div>Message Logs</div>
          <div class="chan-row"><span class="chan-prefix">#</span><select class="inp"><option>msg-logs</option><option>logs</option><option>deleted-msgs</option></select></div>
          <div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="lcm" checked><label class="toggle-track" for="lcm"></label></div>
        </div>
        <div class="log-chan-row">
          <div class="log-chan-name"><div class="dot" style="background:var(--green)"></div>Member Logs</div>
          <div class="chan-row"><span class="chan-prefix">#</span><select class="inp"><option>member-logs</option><option>joins-leaves</option><option>logs</option></select></div>
          <div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="lcme" checked><label class="toggle-track" for="lcme"></label></div>
        </div>
        <div class="log-chan-row">
          <div class="log-chan-name"><div class="dot" style="background:var(--blurple)"></div>Role Logs</div>
          <div class="chan-row"><span class="chan-prefix">#</span><select class="inp"><option>role-logs</option><option>logs</option></select></div>
          <div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="lcr" checked><label class="toggle-track" for="lcr"></label></div>
        </div>
        <div class="log-chan-row">
          <div class="log-chan-name"><div class="dot" style="background:var(--yellow)"></div>Server Logs</div>
          <div class="chan-row"><span class="chan-prefix">#</span><select class="inp"><option>server-logs</option><option>admin-logs</option></select></div>
          <div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="lcs"><label class="toggle-track" for="lcs"></label></div>
        </div>
        <div class="log-chan-row">
          <div class="log-chan-name"><div class="dot" style="background:var(--orange)"></div>Mod Action Logs</div>
          <div class="chan-row"><span class="chan-prefix">#</span><select class="inp"><option>mod-logs</option><option>audit-log</option></select></div>
          <div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="lcmod" checked><label class="toggle-track" for="lcmod"></label></div>
        </div>
        <div class="log-chan-row">
          <div class="log-chan-name"><div class="dot" style="background:var(--cyan)"></div>Voice Logs</div>
          <div class="chan-row"><span class="chan-prefix">#</span><select class="inp"><option>voice-logs</option><option>logs</option></select></div>
          <div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="lcv"><label class="toggle-track" for="lcv"></label></div>
        </div>
      </div>
    </div>
  </div>

  <!-- All categories grid -->
  <div class="log-cat-grid">
    <!-- Messages -->
    <div class="log-cat-card" style="--lcc:var(--red)">
      <div class="log-cat-header"><span class="log-cat-icon">💬</span><div><div class="log-cat-name">Message Events</div><div class="log-cat-count">6 events</div></div></div>
      <div class="log-cat-items">
        <div class="log-item-row"><div class="log-item-name"><div class="log-item-dot" style="background:var(--red)"></div>Message Deleted</div><div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="lm1" checked><label class="toggle-track" style="transform:scale(.75);transform-origin:right" for="lm1"></label></div></div>
        <div class="log-item-row"><div class="log-item-name"><div class="log-item-dot" style="background:var(--yellow)"></div>Message Edited</div><div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="lm2" checked><label class="toggle-track" style="transform:scale(.75);transform-origin:right" for="lm2"></label></div></div>
        <div class="log-item-row"><div class="log-item-name"><div class="log-item-dot" style="background:var(--orange)"></div>Bulk Delete</div><div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="lm3"><label class="toggle-track" style="transform:scale(.75);transform-origin:right" for="lm3"></label></div></div>
        <div class="log-item-row"><div class="log-item-name"><div class="log-item-dot" style="background:var(--blurple)"></div>Message Pinned</div><div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="lm4"><label class="toggle-track" style="transform:scale(.75);transform-origin:right" for="lm4"></label></div></div>
        <div class="log-item-row"><div class="log-item-name"><div class="log-item-dot" style="background:var(--pink)"></div>Attachment Deleted</div><div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="lm5" checked><label class="toggle-track" style="transform:scale(.75);transform-origin:right" for="lm5"></label></div></div>
        <div class="log-item-row"><div class="log-item-name"><div class="log-item-dot" style="background:var(--cyan)"></div>Embed Suppressed</div><div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="lm6"><label class="toggle-track" style="transform:scale(.75);transform-origin:right" for="lm6"></label></div></div>
      </div>
    </div>

    <!-- Members -->
    <div class="log-cat-card" style="--lcc:var(--green)">
      <div class="log-cat-header"><span class="log-cat-icon">👥</span><div><div class="log-cat-name">Member Events</div><div class="log-cat-count">8 events</div></div></div>
      <div class="log-cat-items">
        <div class="log-item-row"><div class="log-item-name"><div class="log-item-dot" style="background:var(--green)"></div>Member Joined</div><div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="lme1" checked><label class="toggle-track" style="transform:scale(.75);transform-origin:right" for="lme1"></label></div></div>
        <div class="log-item-row"><div class="log-item-name"><div class="log-item-dot" style="background:var(--red)"></div>Member Left</div><div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="lme2" checked><label class="toggle-track" style="transform:scale(.75);transform-origin:right" for="lme2"></label></div></div>
        <div class="log-item-row"><div class="log-item-name"><div class="log-item-dot" style="background:var(--yellow)"></div>Nickname Changed</div><div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="lme3" checked><label class="toggle-track" style="transform:scale(.75);transform-origin:right" for="lme3"></label></div></div>
        <div class="log-item-row"><div class="log-item-name"><div class="log-item-dot" style="background:var(--blurple)"></div>Avatar Changed</div><div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="lme4"><label class="toggle-track" style="transform:scale(.75);transform-origin:right" for="lme4"></label></div></div>
        <div class="log-item-row"><div class="log-item-name"><div class="log-item-dot" style="background:var(--orange)"></div>Member Banned</div><div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="lme5" checked><label class="toggle-track" style="transform:scale(.75);transform-origin:right" for="lme5"></label></div></div>
        <div class="log-item-row"><div class="log-item-name"><div class="log-item-dot" style="background:var(--cyan)"></div>Member Unbanned</div><div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="lme6" checked><label class="toggle-track" style="transform:scale(.75);transform-origin:right" for="lme6"></label></div></div>
        <div class="log-item-row"><div class="log-item-name"><div class="log-item-dot" style="background:var(--pink)"></div>Member Kicked</div><div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="lme7" checked><label class="toggle-track" style="transform:scale(.75);transform-origin:right" for="lme7"></label></div></div>
        <div class="log-item-row"><div class="log-item-name"><div class="log-item-dot" style="background:var(--teal)"></div>Member Timed Out</div><div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="lme8" checked><label class="toggle-track" style="transform:scale(.75);transform-origin:right" for="lme8"></label></div></div>
      </div>
    </div>

    <!-- Roles -->
    <div class="log-cat-card" style="--lcc:var(--blurple)">
      <div class="log-cat-header"><span class="log-cat-icon">🏷️</span><div><div class="log-cat-name">Role Events</div><div class="log-cat-count">5 events</div></div></div>
      <div class="log-cat-items">
        <div class="log-item-row"><div class="log-item-name"><div class="log-item-dot" style="background:var(--green)"></div>Role Created</div><div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="lr1" checked><label class="toggle-track" style="transform:scale(.75);transform-origin:right" for="lr1"></label></div></div>
        <div class="log-item-row"><div class="log-item-name"><div class="log-item-dot" style="background:var(--red)"></div>Role Deleted</div><div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="lr2" checked><label class="toggle-track" style="transform:scale(.75);transform-origin:right" for="lr2"></label></div></div>
        <div class="log-item-row"><div class="log-item-name"><div class="log-item-dot" style="background:var(--yellow)"></div>Role Updated</div><div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="lr3" checked><label class="toggle-track" style="transform:scale(.75);transform-origin:right" for="lr3"></label></div></div>
        <div class="log-item-row"><div class="log-item-name"><div class="log-item-dot" style="background:var(--blurple)"></div>Role Assigned</div><div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="lr4" checked><label class="toggle-track" style="transform:scale(.75);transform-origin:right" for="lr4"></label></div></div>
        <div class="log-item-row"><div class="log-item-name"><div class="log-item-dot" style="background:var(--orange)"></div>Role Removed</div><div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="lr5" checked><label class="toggle-track" style="transform:scale(.75);transform-origin:right" for="lr5"></label></div></div>
      </div>
    </div>

    <!-- Server -->
    <div class="log-cat-card" style="--lcc:var(--yellow)">
      <div class="log-cat-header"><span class="log-cat-icon">🏛️</span><div><div class="log-cat-name">Server Events</div><div class="log-cat-count">10 events</div></div></div>
      <div class="log-cat-items">
        <div class="log-item-row"><div class="log-item-name"><div class="log-item-dot" style="background:var(--yellow)"></div>Server Updated</div><div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="ls1" checked><label class="toggle-track" style="transform:scale(.75);transform-origin:right" for="ls1"></label></div></div>
        <div class="log-item-row"><div class="log-item-name"><div class="log-item-dot" style="background:var(--green)"></div>Channel Created</div><div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="ls2" checked><label class="toggle-track" style="transform:scale(.75);transform-origin:right" for="ls2"></label></div></div>
        <div class="log-item-row"><div class="log-item-name"><div class="log-item-dot" style="background:var(--red)"></div>Channel Deleted</div><div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="ls3" checked><label class="toggle-track" style="transform:scale(.75);transform-origin:right" for="ls3"></label></div></div>
        <div class="log-item-row"><div class="log-item-name"><div class="log-item-dot" style="background:var(--blurple)"></div>Channel Updated</div><div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="ls4"><label class="toggle-track" style="transform:scale(.75);transform-origin:right" for="ls4"></label></div></div>
        <div class="log-item-row"><div class="log-item-name"><div class="log-item-dot" style="background:var(--cyan)"></div>Emoji Created</div><div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="ls5"><label class="toggle-track" style="transform:scale(.75);transform-origin:right" for="ls5"></label></div></div>
        <div class="log-item-row"><div class="log-item-name"><div class="log-item-dot" style="background:var(--pink)"></div>Emoji Deleted</div><div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="ls6"><label class="toggle-track" style="transform:scale(.75);transform-origin:right" for="ls6"></label></div></div>
        <div class="log-item-row"><div class="log-item-name"><div class="log-item-dot" style="background:var(--orange)"></div>Webhook Created</div><div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="ls7"><label class="toggle-track" style="transform:scale(.75);transform-origin:right" for="ls7"></label></div></div>
        <div class="log-item-row"><div class="log-item-name"><div class="log-item-dot" style="background:var(--teal)"></div>Invite Created</div><div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="ls8"><label class="toggle-track" style="transform:scale(.75);transform-origin:right" for="ls8"></label></div></div>
        <div class="log-item-row"><div class="log-item-name"><div class="log-item-dot" style="background:var(--red)"></div>Invite Deleted</div><div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="ls9"><label class="toggle-track" style="transform:scale(.75);transform-origin:right" for="ls9"></label></div></div>
        <div class="log-item-row"><div class="log-item-name"><div class="log-item-dot" style="background:var(--yellow)"></div>Boost Update</div><div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="ls10" checked><label class="toggle-track" style="transform:scale(.75);transform-origin:right" for="ls10"></label></div></div>
      </div>
    </div>

    <!-- Mod Actions -->
    <div class="log-cat-card" style="--lcc:var(--orange)">
      <div class="log-cat-header"><span class="log-cat-icon">⚖️</span><div><div class="log-cat-name">Mod Actions</div><div class="log-cat-count">6 events</div></div></div>
      <div class="log-cat-items">
        <div class="log-item-row"><div class="log-item-name"><div class="log-item-dot" style="background:var(--red)"></div>Ban Issued</div><div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="lmod1" checked><label class="toggle-track" style="transform:scale(.75);transform-origin:right" for="lmod1"></label></div></div>
        <div class="log-item-row"><div class="log-item-name"><div class="log-item-dot" style="background:var(--green)"></div>Ban Revoked</div><div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="lmod2" checked><label class="toggle-track" style="transform:scale(.75);transform-origin:right" for="lmod2"></label></div></div>
        <div class="log-item-row"><div class="log-item-name"><div class="log-item-dot" style="background:var(--yellow)"></div>Kick Issued</div><div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="lmod3" checked><label class="toggle-track" style="transform:scale(.75);transform-origin:right" for="lmod3"></label></div></div>
        <div class="log-item-row"><div class="log-item-name"><div class="log-item-dot" style="background:var(--blurple)"></div>Timeout Applied</div><div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="lmod4" checked><label class="toggle-track" style="transform:scale(.75);transform-origin:right" for="lmod4"></label></div></div>
        <div class="log-item-row"><div class="log-item-name"><div class="log-item-dot" style="background:var(--orange)"></div>Warning Issued</div><div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="lmod5" checked><label class="toggle-track" style="transform:scale(.75);transform-origin:right" for="lmod5"></label></div></div>
        <div class="log-item-row"><div class="log-item-name"><div class="log-item-dot" style="background:var(--cyan)"></div>Slow-Mode Changed</div><div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="lmod6"><label class="toggle-track" style="transform:scale(.75);transform-origin:right" for="lmod6"></label></div></div>
      </div>
    </div>

    <!-- Voice -->
    <div class="log-cat-card" style="--lcc:var(--cyan)">
      <div class="log-cat-header"><span class="log-cat-icon">🎙️</span><div><div class="log-cat-name">Voice Events</div><div class="log-cat-count">6 events</div></div></div>
      <div class="log-cat-items">
        <div class="log-item-row"><div class="log-item-name"><div class="log-item-dot" style="background:var(--green)"></div>Voice Join</div><div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="lv1" checked><label class="toggle-track" style="transform:scale(.75);transform-origin:right" for="lv1"></label></div></div>
        <div class="log-item-row"><div class="log-item-name"><div class="log-item-dot" style="background:var(--red)"></div>Voice Leave</div><div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="lv2" checked><label class="toggle-track" style="transform:scale(.75);transform-origin:right" for="lv2"></label></div></div>
        <div class="log-item-row"><div class="log-item-name"><div class="log-item-dot" style="background:var(--yellow)"></div>Voice Move</div><div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="lv3" checked><label class="toggle-track" style="transform:scale(.75);transform-origin:right" for="lv3"></label></div></div>
        <div class="log-item-row"><div class="log-item-name"><div class="log-item-dot" style="background:var(--orange)"></div>Self Muted/Deafened</div><div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="lv4"><label class="toggle-track" style="transform:scale(.75);transform-origin:right" for="lv4"></label></div></div>
        <div class="log-item-row"><div class="log-item-name"><div class="log-item-dot" style="background:var(--blurple)"></div>Server Muted by Mod</div><div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="lv5" checked><label class="toggle-track" style="transform:scale(.75);transform-origin:right" for="lv5"></label></div></div>
        <div class="log-item-row"><div class="log-item-name"><div class="log-item-dot" style="background:var(--pink)"></div>Screen Share Started</div><div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="lv6"><label class="toggle-track" style="transform:scale(.75);transform-origin:right" for="lv6"></label></div></div>
      </div>
    </div>
  </div>

  <div class="save-bar">
    <button class="btn btn-ghost">Discard</button>
    <button class="btn btn-primary" onclick="save('logs')">💾 Save Changes</button>
  </div>
</div>

<!-- ============================================================
     PAGE: API / CONFIG
     ============================================================ -->
<div class="page-section" id="page-api">
  <div class="page-head">
    <div class="breadcrumb">ahh <span>/</span> api</div>
    <div class="page-title"><span class="hl">API</span> Configuration</div>
    <div class="page-desc">Connect your bot to MongoDB and configure log channel IDs from your <code class="mono" style="color:var(--blurple)">index.js</code></div>
  </div>
  <div class="callout callout-red" style="margin-bottom:18px"><span class="callout-icon">⚠️</span><span><strong>Security Warning:</strong> Never share your bot token publicly. Regenerate it immediately at discord.com/developers if exposed. The dashboard uses <code class="mono">YOUR_BOT_TOKEN_HERE</code> as a placeholder.</span></div>
  <div class="grid-2">
    <div class="panel">
      <div class="panel-head"><div class="panel-icon pi-red">🔑</div><div><div class="panel-title">Bot Token</div><div class="panel-sub">Keep this secret at all times</div></div></div>
      <div class="panel-body">
        <div class="field"><div class="field-label">Bot Token (placeholder)</div>
          <input class="inp mono" type="password" value="YOUR_BOT_TOKEN_HERE" placeholder="Paste your bot token">
          <div class="field-hint">Stored as <code class="mono">TOKEN</code> in your bot code</div>
        </div>
        <div class="field"><div class="field-label">MongoDB URL</div>
          <input class="inp mono" placeholder="mongodb+srv://user:pass@cluster.mongodb.net/ahh">
          <div class="field-hint">Stored as <code class="mono">MONGO_URL</code> in your bot code</div>
        </div>
        <div class="callout callout-yellow"><span class="callout-icon">💡</span><span>Use a <code class="mono">.env</code> file with <code class="mono">dotenv</code> package instead of hardcoding values in your bot code.</span></div>
      </div>
    </div>
    <div class="panel">
      <div class="panel-head"><div class="panel-icon pi-blue">📡</div><div><div class="panel-title">Channel IDs</div><div class="panel-sub">Paste your Discord channel IDs</div></div></div>
      <div class="panel-body">
        <div class="field"><div class="field-label">MSG_LOGS_ID</div><input class="inp mono" placeholder="Message logs channel ID"></div>
        <div class="field"><div class="field-label">ROLE_LOGS_ID</div><input class="inp mono" placeholder="Role logs channel ID"></div>
        <div class="field"><div class="field-label">MEMBER_LOGS_ID</div><input class="inp mono" placeholder="Member logs channel ID"></div>
        <div class="field"><div class="field-label">SERVER_LOGS_ID</div><input class="inp mono" placeholder="Server logs channel ID"></div>
        <div class="field"><div class="field-label">WELCOME_CHAN_ID</div><input class="inp mono" placeholder="Welcome channel ID"></div>
      </div>
    </div>
  </div>

  <div class="panel" style="margin-top:18px">
    <div class="panel-head"><div class="panel-icon pi-cyan">📄</div><div><div class="panel-title">Generated Config Snippet</div><div class="panel-sub">Copy this into your bot's index.js</div></div></div>
    <div class="panel-body">
      <div class="code-snippet" id="config-snippet">// ==================== CONFIGURATION ====================
const TOKEN      = 'YOUR_BOT_TOKEN_HERE';  // ← Never share this!
const MONGO_URL  = 'YOUR_MONGODB_URL';

// Logs Channel IDs
const MSG_LOGS_ID    = 'PASTE_ID';
const ROLE_LOGS_ID   = 'PASTE_ID';
const MEMBER_LOGS_ID = 'PASTE_ID';
const SERVER_LOGS_ID = 'PASTE_ID';

// Other Config
const AUTO_ROLE_ID   = 'PASTE_ID';
const GAMER_ROLE_ID  = 'PASTE_ID';
const WELCOME_CHAN_ID = 'PASTE_ID';
// ========================================================</div>
      <button class="btn btn-ghost btn-sm" onclick="copyConfig()">📋 Copy Config</button>
    </div>
  </div>
  <div class="save-bar">
    <button class="btn btn-primary" onclick="save('api')">💾 Save Config</button>
  </div>
</div>

<!-- stub pages for sub-log nav items -->
<div class="page-section" id="page-logs-msg">
  <div class="page-head"><div class="page-title"><span class="hl">Message</span> Logs</div><div class="page-desc">Configure message delete/edit/bulk logging</div></div>
  <div class="callout callout-blue" style="margin-bottom:18px"><span class="callout-icon">ℹ️</span><span>Full settings are in <strong>All Logs</strong> → Message Events section. This page shows your message log preview.</span></div>
  <div class="panel">
    <div class="panel-head"><div class="panel-icon pi-red">🗑️</div><div><div class="panel-title">Message Delete Preview</div><div class="panel-sub">How deleted message logs look in Discord</div></div></div>
    <div class="panel-body">
      <div class="discord-preview">
        <div class="preview-badge">Example Log Entry</div>
        <div class="dc-embed" style="border-left-color:var(--red)">
          <div class="dc-embed-title" style="color:var(--red)">🗑️ Message Deleted</div>
          <div class="dc-embed-body">
            <div class="dc-embed-field"><div class="dc-embed-field-name">Author</div><div class="dc-embed-field-val">JohnDoe#1234 (ID: 123456789)</div></div>
            <div class="dc-embed-field"><div class="dc-embed-field-name">Channel</div><div class="dc-embed-field-val">#general</div></div>
            <div class="dc-embed-field"><div class="dc-embed-field-name">Content</div><div class="dc-embed-field-val">Hello this was my deleted message</div></div>
            <div class="dc-embed-field"><div class="dc-embed-field-name">Attachments</div><div class="dc-embed-field-val">None</div></div>
          </div>
          <div class="dc-embed-footer"><span>📋</span> ahh Logs • Today at 2:34 PM</div>
        </div>
      </div>
    </div>
  </div>
</div>
<div class="page-section" id="page-logs-member"><div class="page-head"><div class="page-title"><span class="hl">Member</span> Logs</div></div><div class="callout callout-blue"><span class="callout-icon">ℹ️</span><span>Manage member log events in the <strong>All Logs</strong> page.</span></div></div>
<div class="page-section" id="page-logs-server"><div class="page-head"><div class="page-title"><span class="hl">Server</span> Logs</div></div><div class="callout callout-blue"><span class="callout-icon">ℹ️</span><span>Manage server log events in the <strong>All Logs</strong> page.</span></div></div>
<div class="page-section" id="page-logs-mod"><div class="page-head"><div class="page-title"><span class="hl">Mod Action</span> Logs</div></div><div class="callout callout-blue"><span class="callout-icon">ℹ️</span><span>Manage mod log events in the <strong>All Logs</strong> page.</span></div></div>
<div class="page-section" id="page-logs-voice"><div class="page-head"><div class="page-title"><span class="hl">Voice</span> Logs</div></div><div class="callout callout-blue"><span class="callout-icon">ℹ️</span><span>Manage voice log events in the <strong>All Logs</strong> page.</span></div></div>
<div class="page-section" id="page-logs-role"><div class="page-head"><div class="page-title"><span class="hl">Role</span> Logs</div></div><div class="callout callout-blue"><span class="callout-icon">ℹ️</span><span>Manage role log events in the <strong>All Logs</strong> page.</span></div></div>
<div class="page-section" id="page-antilink">
  <div class="page-head"><div class="page-title"><span class="hl">Anti-Link</span> Config</div><div class="page-desc">Fine-tune which links are blocked and what happens when they're detected</div></div>
  <div class="grid-2">
    <div class="panel">
      <div class="panel-head"><div class="panel-icon pi-red">🔗</div><div><div class="panel-title">Anti-Link Settings</div></div></div>
      <div class="panel-body">
        <div class="field-row-inline"><div class="field-row-label"><div class="field-label">Enable Anti-Link</div></div><div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="al1" checked><label class="toggle-track" for="al1"></label></div></div>
        <div class="sep"></div>
        <div class="field-row-inline"><div class="field-row-label"><div class="field-label">Block Discord Invites</div></div><div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="al2" checked><label class="toggle-track" for="al2"></label></div></div>
        <div class="sep"></div>
        <div class="field-row-inline"><div class="field-row-label"><div class="field-label">Block External Links</div></div><div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="al3" checked><label class="toggle-track" for="al3"></label></div></div>
        <div class="sep"></div>
        <div class="field-row-inline"><div class="field-row-label"><div class="field-label">Block YouTube Links</div></div><div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="al4"><label class="toggle-track" for="al4"></label></div></div>
        <div class="sep"></div>
        <div class="field"><div class="field-label">Punishment</div><select class="inp"><option>🗑️ Delete</option><option>⚠️ Warn</option><option>⏰ Timeout</option><option>👢 Kick</option></select></div>
        <div class="field"><div class="field-label">Allowed Links (whitelist)</div>
          <textarea class="inp" placeholder="https://youtube.com&#10;https://twitch.tv&#10;One URL per line" style="min-height:90px"></textarea>
        </div>
      </div>
    </div>
  </div>
  <div class="save-bar"><button class="btn btn-ghost">Discard</button><button class="btn btn-primary" onclick="save('antilink')">💾 Save</button></div>
</div>
<div class="page-section" id="page-antispam">
  <div class="page-head"><div class="page-title"><span class="hl">Anti-Spam</span> Config</div><div class="page-desc">Control message flood detection thresholds</div></div>
  <div class="grid-2">
    <div class="panel">
      <div class="panel-head"><div class="panel-icon pi-orange">📨</div><div><div class="panel-title">Spam Detection</div></div></div>
      <div class="panel-body">
        <div class="field-row-inline"><div class="field-row-label"><div class="field-label">Enable Anti-Spam</div></div><div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="as1" checked><label class="toggle-track" for="as1"></label></div></div>
        <div class="sep"></div>
        <div class="field"><div class="field-label">Messages per 10 seconds (threshold)</div><input class="inp mono" type="number" value="5" min="2" max="20" style="max-width:80px"></div>
        <div class="field"><div class="field-label">Same-content repeat limit</div><input class="inp mono" type="number" value="3" min="1" max="10" style="max-width:80px"></div>
        <div class="field"><div class="field-label">Punishment</div><select class="inp"><option>🗑️ Delete Messages</option><option>⏰ Timeout (5m)</option><option>⏰ Timeout (30m)</option><option>🔇 Mute</option></select></div>
        <div class="field-row-inline"><div class="field-row-label"><div class="field-label">Warn user via DM</div></div><div class="toggle-wrap"><input type="checkbox" class="toggle-input" id="as2" checked><label class="toggle-track" for="as2"></label></div></div>
      </div>
    </div>
  </div>
  <div class="save-bar"><button class="btn btn-ghost">Discard</button><button class="btn btn-primary" onclick="save('antispam')">💾 Save</button></div>
</div>

  </div><!-- /page -->
</div><!-- /main -->
</div><!-- /app -->

<!-- TOAST -->
<div class="toast" id="toast">
  <span class="toast-ico" id="toast-ico">✅</span>
  <span class="toast-txt" id="toast-txt">Saved!</span>
</div>

<script>
// ===================== CANVAS BACKGROUND =====================
const canvas = document.getElementById('bg-canvas');
const ctx = canvas.getContext('2d');
let W, H, particles = [];
function resize(){ W = canvas.width = window.innerWidth; H = canvas.height = window.innerHeight; }
window.addEventListener('resize', resize); resize();
const cols = ['rgba(88,101,242,', 'rgba(0,210,255,', 'rgba(255,115,250,', 'rgba(35,209,139,'];
for(let i=0;i<55;i++){
  particles.push({
    x:Math.random()*W, y:Math.random()*H,
    r:Math.random()*1.5+.3,
    vx:(Math.random()-.5)*.25, vy:(Math.random()-.5)*.25,
    c:cols[Math.floor(Math.random()*cols.length)],
    a:Math.random()*.4+.05
  });
}
function drawCanvas(){
  ctx.clearRect(0,0,W,H);
  particles.forEach(p=>{
    p.x+=p.vx; p.y+=p.vy;
    if(p.x<0||p.x>W) p.vx*=-1;
    if(p.y<0||p.y>H) p.vy*=-1;
    ctx.beginPath();
    ctx.arc(p.x,p.y,p.r,0,Math.PI*2);
    ctx.fillStyle = p.c+p.a+')';
    ctx.fill();
  });
  // subtle glow orbs
  const g1=ctx.createRadialGradient(W*.15,H*.2,0,W*.15,H*.2,W*.35);
  g1.addColorStop(0,'rgba(88,101,242,.04)'); g1.addColorStop(1,'transparent');
  ctx.fillStyle=g1; ctx.fillRect(0,0,W,H);
  const g2=ctx.createRadialGradient(W*.85,H*.75,0,W*.85,H*.75,W*.3);
  g2.addColorStop(0,'rgba(255,115,250,.03)'); g2.addColorStop(1,'transparent');
  ctx.fillStyle=g2; ctx.fillRect(0,0,W,H);
  requestAnimationFrame(drawCanvas);
}
drawCanvas();

// ===================== COUNT ANIMATION =====================
function animCount(el, target, dur=1800){
  let s=0, step=target/70, iv=dur/70;
  const t=setInterval(()=>{
    s+=step; if(s>=target){el.textContent=target.toLocaleString();clearInterval(t);return;}
    el.textContent=Math.floor(s).toLocaleString();
  },iv);
}
setTimeout(()=>{
  document.querySelectorAll('.count-num').forEach(el=>{
    const tgt=parseInt(el.dataset.target||'0');
    animCount(el,tgt);
  });
},400);

// ===================== NAVIGATION =====================
const PAGE_TITLES = {
  dashboard:'Dashboard / Overview', general:'General / Settings',
  welcome:'Welcome / Leave System', moderation:'Moderation / Security',
  tickets:'Tickets / System', roles:'Reaction / Roles',
  autorole:'Auto-Role / Config', logs:'Logs / All Events',
  'logs-msg':'Logs / Messages','logs-member':'Logs / Members',
  'logs-server':'Logs / Server','logs-mod':'Logs / Mod Actions',
  'logs-voice':'Logs / Voice','logs-role':'Logs / Roles',
  antilink:'Anti-Link / Config', antispam:'Anti-Spam / Config',
  api:'API / Config',
};
function nav(page, el){
  document.querySelectorAll('.page-section').forEach(s=>s.classList.remove('active'));
  document.querySelectorAll('.nav-item').forEach(n=>n.classList.remove('active'));
  const sec = document.getElementById('page-'+page);
  if(sec){ sec.classList.add('active'); sec.classList.add('anim-fadeup'); setTimeout(()=>sec.classList.remove('anim-fadeup'),600); }
  if(el) el.classList.add('active');
  document.getElementById('tb-title').innerHTML = (PAGE_TITLES[page]||page).replace('/',' <span>/</span> ');
  window.scrollTo({top:0,behavior:'smooth'});
}

// ===================== WELCOME PREVIEW =====================
function updateWelcomePreview(){
  const msg = document.getElementById('wc-msg').value;
  const title = document.getElementById('wc-title').value;
  const col = document.getElementById('wc-color').value;
  const html = msg
    .replace(/\*\*(.*?)\*\*/g,'<strong>$1</strong>')
    .replace('{user}','<strong style="color:var(--blurple)">@NewUser</strong>')
    .replace('{server}','<strong>My Awesome Server</strong>')
    .replace('{count}','<strong>#1,234</strong>')
    .replace('{id}','<code class="mono">987654321</code>')
    .replace(/\n/g,'<br>');
  document.getElementById('wc-preview-title').textContent = title;
  document.getElementById('wc-preview-body').innerHTML = html;
  document.getElementById('wc-preview-embed').style.borderLeftColor = col;
}

// ===================== TICKET PREVIEW =====================
let tkColor = 'var(--blurple)';
function updateTicketPreview(){
  const t = document.getElementById('tk-txt').value || 'Open a Ticket';
  const b = document.getElementById('tk-btn-preview');
  if(b){ b.textContent = t; b.style.background = tkColor; }
}
function pickTkColor(el, col){
  document.querySelectorAll('.tc-swatch').forEach(s=>s.classList.remove('sel'));
  el.classList.add('sel'); tkColor = col;
  updateTicketPreview();
}

// ===================== REACTION ROLES =====================
const rrData = [
  {e:'⭐',n:'Star',r:'Star'},      {e:'🔥',n:'Fire',r:'Fire'},
  {e:'💎',n:'Diamond',r:'Diamond'},{e:'🌊',n:'Ocean',r:'Ocean'},
  {e:'⚡',n:'Thunder',r:'Thunder'},{e:'🍀',n:'Lucky',r:'Lucky'},
];
let rrIdx=0;
function removeRR(btn){
  const row=btn.closest('.rr-row');
  row.style.cssText='transform:translateX(20px);opacity:0;transition:all .3s;overflow:hidden;height:'+row.offsetHeight+'px';
  setTimeout(()=>{row.style.height='0';row.style.margin='0';row.style.padding='0';},300);
  setTimeout(()=>{row.remove();updateRRCount();},600);
}
function addRR(){
  const d=rrData[rrIdx%rrData.length]; rrIdx++;
  const row=document.createElement('div');
  row.className='rr-row'; row.style.cssText='transform:translateX(-20px);opacity:0;transition:all .35s';
  row.innerHTML=`<div class="rr-emoji-box">${d.e}</div><div class="rr-info"><div class="rr-name">${d.n}</div><div class="rr-meta"><span class="role-tag" style="font-size:11px">${d.r}</span><span>React with ${d.e} to get</span></div></div><div class="rr-actions"><span class="chip chip-blue">${d.r.toLowerCase()}_role</span><button class="btn btn-danger btn-sm btn-icon" onclick="removeRR(this)">✕</button></div>`;
  document.getElementById('rr-list').appendChild(row);
  requestAnimationFrame(()=>requestAnimationFrame(()=>{row.style.transform='translateX(0)';row.style.opacity='1';}));
  updateRRCount();
}
function updateRRCount(){
  const c=document.querySelectorAll('#rr-list .rr-row').length;
  const s=document.getElementById('rr-count-sub');
  if(s) s.textContent=c+' roles configured';
}

// ===================== ACCENT COLOR =====================
function updateAccent(input){
  const v=input.value;
  document.getElementById('accentHex').value=v;
  document.getElementById('accent-preview').style.borderLeftColor=v;
  document.getElementById('accent-preview-title').style.color=v;
}
function updateAccentFromHex(input){
  const v=input.value;
  if(/^#[0-9a-fA-F]{6}$/.test(v)){
    document.getElementById('accentColor').value=v;
    document.getElementById('accent-preview').style.borderLeftColor=v;
    document.getElementById('accent-preview-title').style.color=v;
  }
}

// ===================== TOAST =====================
let toastT;
const TOAST_THEMES = {
  green:{icon:'✅',border:'var(--green)',color:'var(--green)'},
  red:{icon:'❌',border:'var(--red)',color:'var(--red)'},
  blue:{icon:'🔄',border:'var(--blurple)',color:'var(--blurple)'},
  yellow:{icon:'⚠️',border:'var(--yellow)',color:'var(--yellow)'},
};
function showToast(msg,theme='green'){
  clearTimeout(toastT);
  const t=document.getElementById('toast');
  const i=document.getElementById('toast-ico');
  const m=document.getElementById('toast-txt');
  const th=TOAST_THEMES[theme]||TOAST_THEMES.green;
  t.style.borderColor=th.border;
  i.textContent=th.icon;
  m.style.color=th.color; m.textContent=msg;
  t.classList.add('show');
  toastT=setTimeout(()=>t.classList.remove('show'),3200);
}
function save(page){
  showToast('✅ '+page.charAt(0).toUpperCase()+page.slice(1)+' settings saved!','green');
}
function saveAll(){
  showToast('💾 All changes saved successfully!','green');
}
function copyConfig(){
  const txt=document.getElementById('config-snippet').textContent;
  navigator.clipboard.writeText(txt).then(()=>showToast('📋 Config copied to clipboard!','blue'));
}
</script>
</body>
</html>
