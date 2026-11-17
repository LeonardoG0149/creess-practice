<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Creess AI · 企业能力认证</title>
<style>
/* ============================================================
   Liquid Glass tokens (guided by ui-ux-pro-max: liquid-glass,
   glassmorphism 15–30px blur, "Spatial Clear" Inter/system type)
   ============================================================ */
*,*::before,*::after{box-sizing:border-box;margin:0;padding:0}

:root{
  /* type — system stack first so it renders as SF on macOS */
  --font: -apple-system, BlinkMacSystemFont, "SF Pro Text", "SF Pro Display",
          "Inter", "PingFang SC", "Hiragino Sans GB", "Microsoft YaHei", sans-serif;
  --mono: ui-monospace, "SF Mono", "Menlo", "Consolas", monospace;

  /* ink */
  --ink:#1d1d1f; --ink-2:#54565c; --ink-3:#86868b; --ink-4:#a1a1a6;

  /* material */
  --glass-window: rgba(255,255,255,.58);
  --glass-bar:    rgba(255,255,255,.52);
  --glass-panel:  rgba(255,255,255,.40);
  --glass-inset:  rgba(120,120,128,.10);
  --glass-chip:   rgba(255,255,255,.72);
  --blur: 34px;
  --sat: 180%;

  /* strokes & shadows — glass rim = bright 1px, then hairline */
  --rim: inset 0 1px 0 rgba(255,255,255,.85), inset 0 0 0 1px rgba(255,255,255,.42);
  --hair: rgba(60,60,67,.12);
  --hair-strong: rgba(60,60,67,.20);
  --win-shadow: 0 32px 80px -12px rgba(28,38,64,.36), 0 8px 24px -8px rgba(28,38,64,.20), 0 0 0 .5px rgba(0,0,0,.10);

  /* semantic */
  --blue:#007aff; --blue-ink:#0062cc; --blue-tint: rgba(0,122,255,.12); --blue-tint-2: rgba(0,122,255,.18);
  --green:#1f9d4d; --green-tint: rgba(52,199,89,.16);
  --orange:#c47b12; --orange-tint: rgba(255,149,0,.16);
  --red:#c9382c; --red-tint: rgba(255,59,48,.14);

  /* metrics */
  --r-win:24px; --r-panel:14px; --r-field:9px; --r-row:7px;
}

@media (prefers-color-scheme: dark){
  :root{
    --ink:#f5f5f7; --ink-2:#c8c8cd; --ink-3:#95959c; --ink-4:#75757c;
    --glass-window: rgba(32,33,38,.72);
    --glass-bar:    rgba(46,47,53,.60);
    --glass-panel:  rgba(44,45,51,.52);
    --glass-inset:  rgba(255,255,255,.075);
    --glass-chip:   rgba(255,255,255,.09);
    --rim: inset 0 1px 0 rgba(255,255,255,.14), inset 0 0 0 1px rgba(255,255,255,.07);
    --hair: rgba(255,255,255,.11);
    --hair-strong: rgba(255,255,255,.18);
    --win-shadow: 0 32px 80px -12px rgba(0,0,0,.7), 0 0 0 .5px rgba(255,255,255,.10);
    --blue:#0a84ff; --blue-ink:#5aa9ff; --blue-tint: rgba(10,132,255,.22); --blue-tint-2: rgba(10,132,255,.30);
    --green:#32d74b; --green-tint: rgba(50,215,75,.18);
    --orange:#ffa62b; --orange-tint: rgba(255,166,43,.18);
    --red:#ff6961; --red-tint: rgba(255,105,97,.18);
  }
}

html,body{height:100%;overflow:hidden}
body{
  font-family:var(--font); color:var(--ink); font-size:13px; line-height:1.5;
  letter-spacing:-.006em;
  -webkit-font-smoothing:antialiased; text-rendering:optimizeLegibility;
  background:#e8edf5;
  min-height:100%;
}

/* ---- app shell: full-bleed web app (glass materials only, no window frame) ---- */
body{background:#f4f7fb}
.app{
  position:relative; height:100vh; display:flex; flex-direction:column;
  background:
    radial-gradient(820px 460px at 12% -8%, rgba(146,182,241,.34), transparent 62%),
    radial-gradient(700px 420px at 94% -2%, rgba(143,214,199,.30), transparent 58%),
    linear-gradient(180deg,#f7f9fd 0%,#f1f4fa 100%);
}
@media (prefers-color-scheme: dark){
  .app{
    background:
      radial-gradient(820px 460px at 12% -8%, rgba(36,62,106,.60), transparent 62%),
      radial-gradient(700px 420px at 94% -2%, rgba(18,58,54,.55), transparent 58%),
      linear-gradient(180deg,#0e1116 0%,#0b0e13 100%);
  }
}

/* ---- top toolbar (glass bar, content scrolls beneath it) ---- */
.toolbar{
  height:56px; flex:none; display:flex; align-items:center; gap:12px;
  padding:0 22px; background:var(--glass-bar); border-bottom:.5px solid var(--hair);
  -webkit-backdrop-filter:blur(24px) saturate(180%); backdrop-filter:blur(24px) saturate(180%);
  position:relative; z-index:5;
}
.toolbar .brand{display:flex; align-items:center; gap:10px; min-width:0}
.toolbar .brand .mk{
  width:26px; height:26px; border-radius:7px; flex:none; display:grid; place-items:center;
  background:var(--blue); color:#fff; font-size:12px; font-weight:700; line-height:1;
  box-shadow:inset 0 1px 0 rgba(255,255,255,.30), 0 1px 2px rgba(0,60,140,.22);
}
.toolbar .brand .nm{font-size:13.5px; font-weight:650; letter-spacing:-.012em; line-height:1.25; min-width:0}
.toolbar .brand .nm span{display:block; font-size:11px; font-weight:400; color:var(--ink-3); letter-spacing:0; margin-top:1px}
.toolbar .spacer{flex:1}

/* segmented control (language) */
.seg{
  display:flex; padding:2px; gap:2px; border-radius:8px;
  background:var(--glass-inset); box-shadow:inset 0 0 0 .5px var(--hair);
}
.seg button{
  border:0; background:transparent; font-family:var(--font); font-size:11.5px; font-weight:590;
  color:var(--ink-2); padding:3px 10px; border-radius:6px; cursor:pointer; line-height:1.4;
  transition:background .18s ease, color .18s ease, box-shadow .18s ease;
}
.seg button[aria-pressed="true"]{
  background:rgba(255,255,255,.92); color:var(--ink);
  box-shadow:0 1px 2px rgba(0,0,0,.10), inset 0 0 0 .5px rgba(0,0,0,.06);
}
@media (prefers-color-scheme: dark){
  .seg button[aria-pressed="true"]{background:rgba(255,255,255,.16);box-shadow:0 1px 2px rgba(0,0,0,.4),inset 0 0 0 .5px rgba(255,255,255,.12)}
}

/* ---- three panes ---- */
.shell{display:grid; grid-template-columns:232px minmax(0,1fr) 336px; flex:1; min-height:0; overflow:hidden}
.pane-b{border-right:.5px solid var(--hair)}

/* sidebar */
.sidebar{
  background:var(--glass-panel); padding:12px 10px 10px; min-height:0; overflow:auto;
  display:flex; flex-direction:column; gap:10px;
}
.co{display:flex; align-items:center; gap:9px; padding:2px 4px 10px; border-bottom:.5px solid var(--hair)}
.co .mark{
  width:30px;height:30px;border-radius:8px;flex:none;display:grid;place-items:center;
  background:linear-gradient(180deg,rgba(255,255,255,.9),rgba(255,255,255,.55));
  box-shadow:inset 0 0 0 .5px var(--hair-strong), 0 1px 2px rgba(0,0,0,.08);
  color:var(--blue); font-weight:700; font-size:13px;
}
@media (prefers-color-scheme: dark){.co .mark{background:rgba(255,255,255,.10)}}
.co .n{font-size:12.5px;font-weight:650;line-height:1.35}
.co .n span{display:block;font-weight:400;font-size:11px;color:var(--ink-3)}

.nav-label{font-size:10.5px;font-weight:600;color:var(--ink-4);letter-spacing:.04em;padding:2px 8px;text-transform:uppercase}
.nav{display:flex;flex-direction:column;gap:1px}
.nav button{
  display:flex; align-items:center; gap:8px; width:100%; text-align:left;
  border:0; background:transparent; font-family:var(--font); font-size:12.5px; color:var(--ink);
  padding:6px 8px; border-radius:var(--r-row); cursor:pointer; line-height:1.35;
  transition:background .16s ease;
}
.nav button:hover{background:var(--glass-inset)}
.nav button[aria-current="true"]{background:var(--blue-tint); color:var(--blue-ink); font-weight:600}
.nav button svg{width:15px;height:15px;flex:none;opacity:.72}
.nav button[aria-current="true"] svg{opacity:1}
.nav .dot{margin-left:auto;width:7px;height:7px;border-radius:50%;flex:none}
.dot.red{background:var(--red)}.dot.amber{background:var(--orange)}.dot.green{background:var(--green)}

.side-foot{margin-top:auto; padding-top:10px; border-top:.5px solid var(--hair)}
.prog{font-size:11px;color:var(--ink-3);display:flex;justify-content:space-between;margin-bottom:5px}
.prog b{color:var(--ink);font-variant-numeric:tabular-nums}
.pbar{height:4px;border-radius:99px;background:var(--glass-inset);overflow:hidden}
.pbar i{display:block;height:100%;border-radius:99px;background:var(--blue);transition:width .4s cubic-bezier(.4,0,.2,1)}

/* content */
.content{overflow:auto; min-height:0; background:rgba(255,255,255,.34); padding:24px 28px 36px; scroll-behavior:smooth}
.doc-head{margin-bottom:18px}
.doc-head h1{font-size:20px;font-weight:680;letter-spacing:-.02em;line-height:1.3}
.doc-head p{font-size:12.5px;color:var(--ink-3);margin-top:3px}

.group{
  background:var(--glass-chip); border-radius:var(--r-panel);
  box-shadow:inset 0 1px 0 rgba(255,255,255,.75), inset 0 0 0 .5px var(--hair), 0 1px 2px rgba(20,30,50,.06), 0 6px 18px -10px rgba(20,30,50,.18);
  margin-bottom:16px; scroll-margin-top:16px; transition:box-shadow .3s ease;
}
.group.focus{box-shadow:inset 0 0 0 1.5px rgba(0,122,255,.55), 0 0 0 3px var(--blue-tint)}
.g-head{
  display:flex; align-items:center; gap:9px; padding:12px 16px 10px;
  border-bottom:.5px solid var(--hair);
}
.g-head .ic{width:22px;height:22px;border-radius:6px;display:grid;place-items:center;background:var(--glass-inset);color:var(--ink-2);flex:none}
.g-head .ic svg{width:13px;height:13px}
.g-head h2{font-size:13.5px;font-weight:640;letter-spacing:-.01em}
.g-head .meta{margin-left:auto;font-size:11.5px;color:var(--ink-3);display:flex;align-items:center;gap:6px}
.g-body{padding:4px 16px 14px}

/* fields — macOS form rows */
.field{display:grid; grid-template-columns:132px minmax(0,1fr); gap:14px; align-items:center; padding:8px 0; border-bottom:.5px solid var(--hair)}
.field:last-of-type{border-bottom:0}
.field label{font-size:12px;color:var(--ink-2)}
.field label i{font-style:normal;color:var(--red)}
.field input{
  width:100%; font-family:var(--font); font-size:12.5px; color:var(--ink);
  padding:6px 9px; border:0; border-radius:var(--r-field);
  background:var(--glass-inset); box-shadow:inset 0 0 0 .5px var(--hair);
  transition:box-shadow .18s ease, background .18s ease;
}
.field input:focus{outline:none;background:rgba(255,255,255,.7);box-shadow:inset 0 0 0 .5px var(--blue), 0 0 0 3.5px var(--blue-tint)}
@media (prefers-color-scheme: dark){.field input:focus{background:rgba(255,255,255,.10)}}

/* path bar — where you are in S0–S5 */
.pathbar{
  display:flex; align-items:center; gap:1px; flex-wrap:nowrap; overflow-x:auto;
  padding:4px 5px; margin-bottom:16px; border-radius:10px; scrollbar-width:none;
  background:var(--glass-panel); box-shadow:inset 0 0 0 .5px var(--hair);
}
.pathbar::-webkit-scrollbar{display:none}
.pathbar .st{
  display:inline-flex; align-items:center; gap:4px; padding:3px 6px; border-radius:7px;
  font-size:11px; color:var(--ink-3); white-space:nowrap; flex:none;
}
.pathbar .st b{font-weight:600;color:var(--ink-2)}
.pathbar .st .b{width:5px;height:5px;border-radius:50%;background:var(--ink-4);opacity:.5}
.pathbar .st.done{color:var(--ink-2)}
.pathbar .st.done .b{background:var(--green);opacity:1}
.pathbar .st.now{background:var(--blue-tint);color:var(--blue-ink)}
.pathbar .st.now b{color:var(--blue-ink)}
.pathbar .st.now .b{background:var(--blue);opacity:1}
.pathbar .sep{font-size:10px;color:var(--ink-4);opacity:.6}

/* claims list */
.claim{padding:11px 0; border-bottom:.5px solid var(--hair)}
.claim:last-child{border-bottom:0}
.c-line{display:flex; gap:8px; align-items:flex-start}
.c-dot{width:7px;height:7px;border-radius:50%;margin-top:6px;flex:none}
.c-dot.ok{background:var(--green)}.c-dot.warn{background:var(--orange)}.c-dot.bad{background:var(--red)}
.c-txt{font-size:12.8px;line-height:1.5}
.c-note{font-size:11px;color:var(--ink-3);margin-left:6px}
.c-note.bad{color:var(--red)}
.c-tags{display:flex;flex-wrap:wrap;align-items:center;gap:6px;margin:8px 0 0 15px}

/* token / chip — glass capsule */
.chip{
  display:inline-flex; align-items:center; gap:5px;
  font-family:var(--font); font-size:11.5px; font-weight:500; color:var(--ink-2);
  padding:3.5px 10px 3.5px 8px; border:0; border-radius:999px; cursor:pointer;
  background:var(--glass-chip); box-shadow:inset 0 0 0 .5px var(--hair-strong), 0 1px 1px rgba(0,0,0,.04);
  transition:background .16s ease, color .16s ease, box-shadow .16s ease, transform .12s ease;
}
.chip{color:var(--ink-3)}
.chip svg{width:11px;height:11px;opacity:.45}
.chip:hover{color:var(--ink);box-shadow:inset 0 0 0 .5px var(--hair-strong), 0 1px 3px rgba(0,0,0,.09)}
.chip:active{transform:scale(.97)}
.chip[aria-pressed="true"]{
  background:var(--blue-tint); color:var(--blue-ink); font-weight:600;
  box-shadow:inset 0 0 0 .5px rgba(0,122,255,.35);
}
.chip[aria-pressed="true"] svg{opacity:1}
.tags-lb{font-size:11px;color:var(--ink-4);margin-right:2px}

/* ---- inspector ---- */
.inspector{
  background:var(--glass-panel); border-left:.5px solid var(--hair); min-height:0;
  padding:14px 14px 12px; display:flex; flex-direction:column; gap:11px; overflow:auto;
}
.insp-head{display:flex;align-items:center;gap:7px}
.insp-head h2{font-size:13px;font-weight:650}
.live{display:inline-flex;align-items:center;gap:4px;margin-left:auto;font-size:10.5px;color:var(--green);font-weight:600}
.live i{width:6px;height:6px;border-radius:50%;background:var(--green);animation:breathe 2.4s ease-in-out infinite}
@keyframes breathe{0%,100%{opacity:1}50%{opacity:.28}}

.gauge{display:flex;align-items:center;gap:14px;padding:2px 0 4px}
.ring{position:relative;width:86px;height:86px;flex:none}
.ring svg{transform:rotate(-90deg);display:block}
.ring .num{position:absolute;inset:0;display:grid;place-items:center}
.ring .num b{font-size:23px;font-weight:680;letter-spacing:-.03em;font-variant-numeric:tabular-nums}
.ring .num small{display:block;font-size:9.5px;color:var(--ink-4);text-align:center;margin-top:-1px}
.meters{flex:1;display:flex;flex-direction:column;gap:9px}
.meter .ml{display:flex;font-size:11px;color:var(--ink-2);margin-bottom:3px}
.meter .ml b{margin-left:auto;font-weight:600;font-variant-numeric:tabular-nums;color:var(--ink)}
.meter .track{height:3px;border-radius:99px;background:var(--glass-inset);overflow:hidden}
.meter .track i{display:block;height:100%;border-radius:99px;transition:width .45s cubic-bezier(.4,0,.2,1)}

.triage{border-radius:11px;padding:10px 12px;box-shadow:inset 0 0 0 .5px currentColor}
.triage .k{font-size:10px;font-weight:600;letter-spacing:.05em;text-transform:uppercase;opacity:.8}
.triage .v{font-size:13.5px;font-weight:660;margin-top:2px;letter-spacing:-.01em}
.triage .d{font-size:11.3px;margin-top:3px;line-height:1.5;color:var(--ink-2)}
.triage.pass{background:var(--green-tint);color:var(--green)}
.triage.quick{background:var(--orange-tint);color:var(--orange)}
.triage.deep{background:var(--red-tint);color:var(--red)}

.actions{display:flex;gap:7px}
.btn{
  flex:1; font-family:var(--font); font-size:12px; font-weight:600; cursor:pointer;
  padding:7px 10px; border:0; border-radius:9px; letter-spacing:-.005em;
  transition:filter .16s ease, background .16s ease, transform .12s ease;
}
.btn:active{transform:scale(.985)}
.btn-primary{
  color:#fff; background:var(--blue);
  box-shadow:inset 0 1px 0 rgba(255,255,255,.28), 0 1px 2px rgba(0,60,140,.28);
}
.btn-primary:hover{filter:brightness(1.07)}
.btn-primary:disabled{filter:grayscale(.35) opacity(.75);cursor:default}
.btn-ghost{
  color:var(--ink); background:var(--glass-chip);
  box-shadow:inset 0 0 0 .5px var(--hair-strong), 0 1px 1px rgba(0,0,0,.05);
}
.btn-ghost:hover{background:var(--glass-window)}

.issues{flex:1;min-height:0;overflow:auto;margin:0 -4px;padding:0 4px}
.iss{display:flex;gap:8px;padding:9px 0;border-bottom:.5px solid var(--hair)}
.iss:last-child{border-bottom:0}
.iss .sev{width:6px;height:6px;border-radius:50%;margin-top:6px;flex:none}
.sev.h{background:var(--red)}.sev.m{background:var(--orange)}.sev.l{background:var(--ink-4)}
.iss .b{flex:1;min-width:0}
.iss .txt{font-size:11.8px;line-height:1.5;color:var(--ink)}
.iss .where{font-size:10.5px;color:var(--ink-4);margin-top:2px}
.iss .fix{
  margin-top:3px;border:0;background:none;padding:0;cursor:pointer;
  font-family:var(--font); font-size:11px;font-weight:600;color:var(--blue);
}
.iss .fix:hover{text-decoration:underline}
.allgood{font-size:12px;color:var(--green);padding:10px 0;line-height:1.5}

.insp-foot{margin-top:auto;padding-top:10px;border-top:.5px solid var(--hair);font-size:10.5px;color:var(--ink-4);line-height:1.55}

/* ---- notification toast (macOS style) ---- */
.note{
  position:absolute; right:20px; top:66px; width:300px; border-radius:14px;
  padding:11px 14px; display:flex; gap:10px; align-items:flex-start;
  background:var(--glass-window); box-shadow:var(--win-shadow), var(--rim);
  -webkit-backdrop-filter:blur(28px) saturate(180%); backdrop-filter:blur(28px) saturate(180%);
  transform:translateY(-14px) scale(.98); opacity:0; pointer-events:none;
  transition:opacity .24s ease, transform .24s cubic-bezier(.2,.8,.3,1); z-index:80;
}
.note.show{opacity:1;transform:none}
.note .nic{width:28px;height:28px;border-radius:8px;flex:none;display:grid;place-items:center;background:var(--blue-tint);color:var(--blue)}
.note .nic svg{width:15px;height:15px}
.note h4{font-size:12px;font-weight:650}
.note p{font-size:11.5px;color:var(--ink-2);line-height:1.45;margin-top:1px}

/* focus visibility everywhere */
:focus-visible{outline:2.5px solid var(--blue);outline-offset:2px;border-radius:6px}

/* thin overlay scrollbars (macOS feel) */
.content::-webkit-scrollbar,.inspector::-webkit-scrollbar,.sidebar::-webkit-scrollbar,.issues::-webkit-scrollbar{width:9px;height:9px}
.content::-webkit-scrollbar-track,.inspector::-webkit-scrollbar-track,.sidebar::-webkit-scrollbar-track,.issues::-webkit-scrollbar-track{background:transparent}
.content::-webkit-scrollbar-thumb,.inspector::-webkit-scrollbar-thumb,.sidebar::-webkit-scrollbar-thumb,.issues::-webkit-scrollbar-thumb{
  background:rgba(60,60,67,.22); border-radius:99px; border:2.5px solid transparent; background-clip:content-box;
}
.content:hover::-webkit-scrollbar-thumb,.inspector:hover::-webkit-scrollbar-thumb,.issues:hover::-webkit-scrollbar-thumb{background:rgba(60,60,67,.34);background-clip:content-box}
@media (prefers-color-scheme: dark){
  .content::-webkit-scrollbar-thumb,.inspector::-webkit-scrollbar-thumb,.issues::-webkit-scrollbar-thumb{background:rgba(255,255,255,.22);background-clip:content-box}
}

/* ---- responsive ---- */
@media (max-width:1160px){
  .shell{grid-template-columns:206px minmax(0,1fr) 300px}
  .content{padding:20px 20px 32px}
}
@media (max-width:960px){
  html,body{overflow:auto;height:auto}
  .app{height:auto;min-height:100vh}
  .toolbar{position:sticky;top:0}
  .note{position:fixed;left:14px;right:14px;top:66px;width:auto}
  .shell{grid-template-columns:minmax(0,1fr);overflow:visible}
  .content{overflow:visible}
  .sidebar{flex-direction:row;align-items:center;overflow:auto;padding:8px}
  .co,.nav-label,.side-foot{display:none}
  .nav{flex-direction:row;gap:4px}
  .nav button{white-space:nowrap}
  .inspector{border-left:0;border-top:.5px solid var(--hair)}
  .iss .txt{font-size:12px}
}
@media (max-width:640px){
  .field{grid-template-columns:1fr;gap:5px}
  .doc-head h1{font-size:17px}
}

/* dark-mode material corrections (content layer must not stay white) */
@media (prefers-color-scheme: dark){
  .content{background:rgba(255,255,255,.035)}
  .group{background:rgba(255,255,255,.055);
    box-shadow:inset 0 1px 0 rgba(255,255,255,.06), inset 0 0 0 .5px var(--hair), 0 6px 18px -10px rgba(0,0,0,.55)}
  .chip{background:rgba(255,255,255,.085);box-shadow:inset 0 0 0 .5px rgba(255,255,255,.15), 0 1px 1px rgba(0,0,0,.25)}
  .chip:hover{background:rgba(255,255,255,.13);color:var(--ink)}
  .chip[aria-pressed="true"]{background:rgba(10,132,255,.26);color:#8cc0ff;box-shadow:inset 0 0 0 .5px rgba(90,169,255,.45)}
  .field input{background:rgba(255,255,255,.075);box-shadow:inset 0 0 0 .5px rgba(255,255,255,.10)}
  .side-foot .pbar{background:rgba(255,255,255,.10)}
  .btn-ghost{background:rgba(255,255,255,.085);box-shadow:inset 0 0 0 .5px rgba(255,255,255,.16)}
  .btn-ghost:hover{background:rgba(255,255,255,.13)}
  .triage .d{color:rgba(255,255,255,.72)}
}

/* accessibility: reduced transparency / motion */
@media (prefers-reduced-transparency: reduce){
  .toolbar,.sidebar,.inspector,.chip,.group,.note{backdrop-filter:none;-webkit-backdrop-filter:none}
  .app{background:#f7f8fa}.toolbar{background:#fff}.sidebar,.inspector{background:#f5f6f8}.content{background:#fbfbfc}
  .chip{background:#fff}.group{background:#fff}
}
@media (prefers-reduced-motion: reduce){
  *,*::before,*::after{animation:none!important;transition:none!important}
  .content{scroll-behavior:auto}
}
</style>
</head>
<body>

<div class="app" role="application" aria-label="企业能力认证">

  <header class="toolbar">
    <div class="brand">
      <div class="mk" aria-hidden="true">C</div>
      <div class="nm"><span id="tbTitle">企业能力认证</span><span id="tbSub">Creess AI · 提交与预审</span></div>
    </div>
    <div class="spacer"></div>
    <div class="seg" role="group" aria-label="Language">
      <button id="btnZh" aria-pressed="true">中文</button>
      <button id="btnEn" aria-pressed="false">EN</button>
    </div>
  </header>

  <div class="shell">

      <!-- sidebar -->
      <aside class="sidebar">
        <div class="co">
          <div class="mark" aria-hidden="true">星</div>
          <div class="n"><span id="coName">星野数字营销</span><span id="coSub">示例企业 · A 轮 · 42 人</span></div>
        </div>
        <div class="nav-label" id="navLabel">认证材料 · 六模块</div>
        <nav class="nav" id="nav" aria-label="模块"></nav>
        <div class="side-foot">
          <div class="prog"><span id="progLb">材料完整度</span><b id="progVal">—</b></div>
          <div class="pbar"><i id="progBar" style="width:0"></i></div>
        </div>
      </aside>

      <!-- content -->
      <main class="content" id="content">
        <div class="doc-head">
          <h1 id="docTitle">企业认证材料</h1>
          <p id="docSub">六模块 · 每条宣称都要挂证据 · 灰色胶囊表示证据尚未绑定</p>
        </div>
        <div class="pathbar" id="pathbar" aria-label="process"></div>
        <div id="board"></div>
      </main>

      <!-- inspector -->
      <aside class="inspector">
        <div class="insp-head">
          <h2 id="inspTitle">AI 预审</h2>
          <span class="live"><i></i><span id="liveLb">实时</span></span>
        </div>

        <div class="gauge">
          <div class="ring">
            <svg width="86" height="86" viewBox="0 0 86 86" aria-hidden="true">
              <circle cx="43" cy="43" r="36" fill="none" stroke="currentColor" stroke-opacity=".12" stroke-width="6"/>
              <circle id="arc" cx="43" cy="43" r="36" fill="none" stroke="currentColor" stroke-width="6"
                stroke-linecap="round" stroke-dasharray="226.2" stroke-dashoffset="226.2"
                style="transition:stroke-dashoffset .5s cubic-bezier(.4,0,.2,1),stroke .25s"/>
            </svg>
            <div class="num"><div><b id="scoreNum">0</b><small id="scoreLb">可信度</small></div></div>
          </div>
          <div class="meters">
            <div class="meter"><div class="ml"><span id="m1l">完整度</span><b id="m1v">0%</b></div>
              <div class="track"><i id="m1" style="width:0;background:var(--blue)"></i></div></div>
            <div class="meter"><div class="ml"><span id="m2l">证据覆盖</span><b id="m2v">0%</b></div>
              <div class="track"><i id="m2" style="width:0;background:var(--green)"></i></div></div>
            <div class="meter"><div class="ml"><span id="m3l">自洽性</span><b id="m3v">0%</b></div>
              <div class="track"><i id="m3" style="width:0;background:var(--orange)"></i></div></div>
          </div>
        </div>

        <div class="triage quick" id="triage">
          <div class="k" id="triK">分流结果</div>
          <div class="v" id="triV">—</div>
          <div class="d" id="triD">—</div>
        </div>

        <div class="actions">
          <button class="btn btn-primary" id="runBtn">运行预审</button>
          <button class="btn btn-ghost" id="fixBtn">补齐演示</button>
        </div>

        <div class="issues" id="issues"></div>
        <div class="insp-foot" id="inspFoot"></div>
      </aside>

  </div>

  <div class="note" id="toast" role="status" aria-live="polite">
    <div class="nic" aria-hidden="true">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><path d="M20 6 9 17l-5-5"/></svg>
    </div>
    <div><h4 id="toastTitle">预审完成</h4><p id="toastBody"></p></div>
  </div>
</div>


<script>
/* ================= i18n ================= */
const UI = {
  zh:{
    tbTitle:"企业能力认证", tbSub:"Creess AI · 提交与预审",
    coName:"星野数字营销", coSub:"示例企业 · A 轮 · 42 人",
    navLabel:"认证材料 · 六模块",
    docTitle:"企业认证材料", docSub:"六模块 · 每条宣称都要挂证据 · 灰色胶囊表示证据尚未绑定",
    inspTitle:"AI 预审", live:"实时", scoreLb:"可信度",
    m1:"完整度", m2:"证据覆盖", m3:"自洽性",
    triK:"分流结果", run:"运行预审", running:"扫描中…", fix:"补齐演示",
    evLb:"证据", req:"必填",
    claimHead:"能力 / 案例宣称",
    stOk:(n)=>`已绑定 ${n} 份`, stBad:"未绑定证据", stRw:"AI 改写 · 待人确认",
    pass:"自动通过 · 抽检 10%", passD:"低风险且证据齐全，进入自动化通道，人工只做 10% 随机抽检。",
    quick:"快审 · 5 分钟", quickD:"存在可自助补的缺口，AI 已生成待确认问题，审核员核对摘要即可。",
    deep:"深度人审 · 含回访", deepD:"高风险：宣称强、证据弱或夸张表述密集，需人工判断能力深度与授权合法性。",
    progLb:"材料完整度",
    foot:"分数由前端规则实时计算（非真实模型），用于验证「挂证据 → 预审 → 分流」是否可产品化。数据为虚构示例。",
    allGood:"未发现阻断性问题，可进入自动通过与抽检通道。",
    fixEv:"挂证据", fixRw:"改写表述",
    toastRunT:"预审完成", toastRunB:(t,c)=>`用时 ${t}s · 单份成本约 ¥${c} · 6 模块 · 21 项检查`,
    toastFixT:"已补齐", toastFixB:"7 处缺口已补（演示）",
    mods:{basic:"企业基本信息",cap:"核心业务能力",cas:"代表案例",cli:"服务客户",team:"团队介绍",proof:"能力证明材料"},
    steps:""
  },
  en:{
    tbTitle:"Enterprise certification", tbSub:"Creess AI · submission & pre-check",
    coName:"Xingye Digital (sample)", coSub:"Sample company · Series A · 42 staff",
    navLabel:"Materials · six modules",
    docTitle:"Certification materials", docSub:"Six modules · every claim carries evidence · grey pills mean unbound evidence",
    inspTitle:"Pre-check", live:"live", scoreLb:"trust score",
    m1:"Completeness", m2:"Evidence", m3:"Consistency",
    triK:"Triage", run:"Run pre-check", running:"Scanning…", fix:"Auto-fill demo",
    evLb:"Evidence", req:"required",
    claimHead:"Claims",
    stOk:(n)=>`${n} attached`, stBad:"No evidence", stRw:"AI rewrite · pending",
    pass:"Auto-pass · 10% sampled", passD:"Low risk with solid evidence: automated lane, humans sample 10%.",
    quick:"Quick review · 5 min", quickD:"Self-fixable gaps remain; AI drafted the open questions, reviewer clears the summary.",
    deep:"Deep review · +callback", deepD:"High risk: strong claims, weak evidence or heavy hype; a human must judge depth and authorization.",
    progLb:"Completeness",
    foot:"Scores are computed by front-end rules (not a real model) to test whether attach → pre-check → triage is productizable. All data is fictional.",
    allGood:"No blocking issues — eligible for auto-pass and sampling.",
    fixEv:"attach", fixRw:"rewrite",
    toastRunT:"Pre-check complete", toastRunB:(t,c)=>`${t}s · ≈ ¥${c} per submission · 6 modules · 21 checks`,
    toastFixT:"Gaps filled", toastFixB:"7 gaps auto-filled (demo)",
    mods:{basic:"Company profile",cap:"Core capabilities",cas:"Case studies",cli:"Clients",team:"Team",proof:"Proof"},
    steps:""
  }
};

/* ================= icons (1.6px line, SF-symbol feel) ================= */
const ICON = {
  basic:'<path d="M4 20h16M6 20V9l6-5 6 5v11M10 20v-5h4v5"/>',
  cap:'<path d="M12 3 3 8l9 5 9-5-9-5ZM3 13l9 5 9-5"/>',
  cas:'<path d="M4 6h9l3 3h4v9H4V6Z"/><path d="M4 11h16"/>',
  cli:'<path d="M9 11a3 3 0 1 0 0-6 3 3 0 0 0 0 6ZM3 20c0-3 2.7-5 6-5s6 2 6 5"/><path d="M16 8a2.5 2.5 0 1 1 0 5M18 20c0-2-.8-3.6-2-4.6"/>',
  team:'<circle cx="12" cy="8" r="3.2"/><path d="M5 20c0-3.4 3.1-6 7-6s7 2.6 7 6"/>',
  proof:'<path d="M12 3 4.5 6v6c0 4.2 3.1 7.6 7.5 9 4.4-1.4 7.5-4.8 7.5-9V6L12 3Z"/><path d="m9 12 2 2 4-4"/>',
  check:'<path d="M20 6 9 17l-5-5"/>',
  plus:'<path d="M12 5v14M5 12h14"/>'
};
const svg = (name, cls="") => `<svg class="${cls}" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true">${ICON[name]||""}</svg>`;

/* ================= data (fictional) ================= */
const DATA = [
  { id:"basic", icon:"basic",
    fields:[
      {k:"legal", z:"工商主体", e:"Legal entity", v:"上海星野数字营销有限公司", req:true},
      {k:"founded", z:"成立时间", e:"Founded", v:"2016-04", req:true},
      {k:"size", z:"团队规模", e:"Headcount", v:"42", req:true},
      {k:"biz", z:"主营业务", e:"Main business", v:"品牌内容营销与社交媒体代运营", req:true}
    ],
    claims:[
      {z:"营业执照与工商登记一致，备案信息可查", e:"License matches registry; ICP filing verifiable",
       ev:[{z:"营业执照",e:"Business license",on:true},{z:"备案信息",e:"ICP filing",on:true}]}
    ]},
  { id:"cap", icon:"cap", fields:[],
    claims:[
      {z:"全域内容策略：为品牌搭建内容中台，年产出 1,200 支短视频", e:"Content hub producing 1,200 short videos a year",
       ev:[{z:"方法论文档",e:"Method doc",on:true},{z:"结案报告",e:"Closing report",on:true},{z:"数据截图",e:"Screenshot",on:false}]},
      {z:"KOL 矩阵投放：覆盖 3,000+ 达人，客户 ROI 提升 320%", e:"KOL matrix: 3,000+ creators, client ROI +320%",
       ev:[{z:"方法论文档",e:"Method doc",on:false},{z:"结案报告",e:"Closing report",on:false},{z:"数据后台截图",e:"Platform screenshot",on:false}]},
      {z:"行业领先的品牌资产数字化方法论，一站式全链路赋能", e:"Industry-leading brand-asset method, one-stop end-to-end",
       ev:[{z:"方法论文档",e:"Method doc",on:false},{z:"软著 / 专利",e:"IP filing",on:false}]}
    ]},
  { id:"cas", icon:"cas", fields:[],
    claims:[
      {z:"案例 A · 某快消品牌新品上市：全网曝光 2.1 亿，互动率 4.8%", e:"Case A · FMCG launch: 210M impressions, 4.8% engagement",
       ev:[{z:"合同脱敏页",e:"Redacted contract",on:true},{z:"客户授权",e:"Client authorization",on:true},{z:"复盘报告",e:"Post-campaign report",on:true}]},
      {z:"案例 B · 某家电品牌社媒代运营：年 GMV +48%", e:"Case B · Appliance social ops: GMV +48%",
       ev:[{z:"合同脱敏页",e:"Redacted contract",on:true},{z:"客户授权",e:"Client authorization",on:false},{z:"复盘报告",e:"Post-campaign report",on:true}]},
      {z:"案例 C · 某车企内容中台搭建：交付 6 个月，3 条产品线接入", e:"Case C · Auto brand content hub: 6 months, 3 product lines",
       ev:[{z:"合同脱敏页",e:"Redacted contract",on:false},{z:"客户授权",e:"Client authorization",on:false}]}
    ]},
  { id:"cli", icon:"cli",
    fields:[
      {k:"cnt", z:"累计服务客户数", e:"Clients served", v:"68", req:true},
      {k:"mix", z:"行业分布", e:"Industry mix", v:"快消 40% / 家电 25% / 汽车 20% / 其他 15%", req:true}
    ],
    claims:[
      {z:"8 家客户连续合作 3 年以上，续约率 72%", e:"8 clients 3+ years; renewal 72%",
       ev:[{z:"logo 使用授权",e:"Logo authorization",on:true},{z:"合作声明",e:"Cooperation statement",on:false}]}
    ]},
  { id:"team", icon:"team",
    fields:[
      {k:"core", z:"核心成员", e:"Core members", v:"4 人（策略 2 / 内容 1 / 投放 1）", req:true},
      {k:"yrs", z:"平均相关年限", e:"Avg. years", v:"9 年", req:true}
    ],
    claims:[
      {z:"核心成员来自 4A 与头部平台，全职投入本项目", e:"Core members from 4A agencies and leading platforms",
       ev:[{z:"履历",e:"CVs",on:true},{z:"在职证明",e:"Employment proof",on:false}]}
    ]},
  { id:"proof", icon:"proof", fields:[],
    claims:[
      {z:"2024 金投赏内容营销类银奖（项目：某快消新品上市）", e:"2024 ROI Festival, Content Marketing — Silver",
       ev:[{z:"获奖证书",e:"Award certificate",on:true}]},
      {z:"2 项内容分发系统软件著作权", e:"2 software copyrights for content distribution",
       ev:[{z:"软著证书",e:"Copyright certificate",on:false}]},
      {z:"36 氪、界面等 7 篇媒体报道", e:"7 media features (36Kr, Jiemian…)",
       ev:[{z:"媒体报道链接",e:"Media links",on:true}]}
    ]}
];

const HYPE_GROUPS = [
  ["行业领先","industry-leading"], ["一站式","one-stop"], ["全链路","end-to-end"],
  ["头部"], ["第一","best-in-class"], ["领先"]
];
const hypeCount = (c) => HYPE_GROUPS.filter((g) => g.some((h) => (c.z + " " + c.e).includes(h))).length;
const NUM = /(\d+(\.\d+)?\s*%|\d+\s*倍|\+\s*\d+%)/;
const REWRITE = {
  "cap-2": {
    z:"品牌资产数字化：以统一素材库 + 标签体系替代人工分发，3 个项目平均减少 30% 制作工时",
    e:"Brand-asset digitisation: a unified asset library with tagging replaces manual distribution, cutting production hours by 30% across 3 projects"
  },
  "team-0": {
    z:"核心成员 4 人分别来自 4A 代理公司与媒体平台（策略 2 / 内容 1 / 投放 1），本项目全职投入",
    e:"Four core members from 4A agencies and media platforms (2 strategy, 1 content, 1 media buying), full-time on this project"
  }
};
const fallbackRewrite = (c) => {
  const strip = (str) => str.replace(/(行业领先的?|一站式|全链路|头部|第一|领先的?|industry-leading|one-stop|end-to-end|best-in-class)/gi, "").replace(/\s{2,}/g, " ").trim();
  return { z: strip(c.z), e: strip(c.e) };
};

/* ================= process steps (S0–S5) ================= */
const STEPS = [
  { id:"S0", z:"自动建档", e:"Auto-dossier", state:"done" },
  { id:"S1", z:"结构化填报", e:"Structured form", state:"done" },
  { id:"S2", z:"证据绑定", e:"Evidence binding", state:"done" },
  { id:"S3", z:"提交前预审", e:"Pre-check", state:"now" },
  { id:"S4", z:"分级受理", e:"Triage", state:"" },
  { id:"S5", z:"持续维护", e:"Upkeep", state:"" }
];

/* ================= state ================= */
const S = { lang:"zh", modules: JSON.parse(JSON.stringify(DATA)) };
const t = () => UI[S.lang];
const tx = (o) => S.lang === "zh" ? o.z : o.e;

/* ================= render ================= */
function renderChrome(){
  const u = t();
  document.documentElement.lang = S.lang === "zh" ? "zh-CN" : "en";
  document.getElementById("tbTitle").textContent = u.tbTitle;
  document.getElementById("tbSub").textContent = u.tbSub;
  document.getElementById("coName").textContent = u.coName;
  document.getElementById("coSub").textContent = u.coSub;
  document.getElementById("navLabel").textContent = u.navLabel;
  document.getElementById("docTitle").textContent = u.docTitle;
  document.getElementById("docSub").textContent = u.docSub;
  document.getElementById("inspTitle").textContent = u.inspTitle;
  document.getElementById("liveLb").textContent = u.live;
  document.getElementById("scoreLb").textContent = u.scoreLb;
  document.getElementById("m1l").textContent = u.m1;
  document.getElementById("m2l").textContent = u.m2;
  document.getElementById("m3l").textContent = u.m3;
  document.getElementById("triK").textContent = u.triK;
  document.getElementById("runBtn").textContent = u.run;
  document.getElementById("fixBtn").textContent = u.fix;
  document.getElementById("progLb").textContent = u.progLb;
  document.getElementById("inspFoot").textContent = u.foot;
  document.getElementById("btnZh").setAttribute("aria-pressed", String(S.lang === "zh"));
  document.getElementById("btnEn").setAttribute("aria-pressed", String(S.lang === "en"));
}

function renderPath(){
  const el = document.getElementById("pathbar");
  if(!el) return;
  el.innerHTML = STEPS.map((s, i)=>`
    ${i ? '<span class="sep" aria-hidden="true">›</span>' : ""}
    <span class="st ${s.state}"><span class="b" aria-hidden="true"></span><b>${s.id}</b>${S.lang==="zh"?s.z:s.e}</span>`).join("");
}

function renderNav(){
  const u = t(), r = compute();
  document.getElementById("nav").innerHTML = S.modules.map((m)=>`
    <button data-go="${m.id}" aria-current="${S.current === m.id}">
      ${svg(m.icon)}<span>${u.mods[m.id]}</span><span class="dot ${r.mods[m.id].level}"></span>
    </button>`).join("");
  document.querySelectorAll("#nav button").forEach((b)=>{
    b.addEventListener("click", ()=>{
      S.current = b.dataset.go;
      document.querySelectorAll("#nav button").forEach((x)=>x.setAttribute("aria-current", String(x.dataset.go===S.current)));
      const el = document.getElementById("g-"+S.current);
      el.scrollIntoView({behavior:"smooth", block:"start"});
      el.classList.add("focus");
      setTimeout(()=>el.classList.remove("focus"), 1200);
    });
  });
}

function renderBoard(){
  const u = t(), r = compute();
  document.getElementById("board").innerHTML = S.modules.map((m)=>{
    const st = r.mods[m.id];
    const fields = m.fields.length ? m.fields.map((f)=>`
      <div class="field">
        <label for="f-${m.id}-${f.k}">${tx(f)}${f.req?' <i>*</i>':''}</label>
        <input id="f-${m.id}-${f.k}" data-mod="${m.id}" data-k="${f.k}" value="${esc(f.v)}">
      </div>`).join("") : "";
    const claims = m.claims.map((c, ci)=>{
      const on = c.ev.filter((e)=>e.on).length;
      const dot = on === 0 ? "bad" : (c.ev.length > on ? "warn" : "ok");
      const chips = c.ev.map((e, ei)=>`
        <button class="chip" data-mod="${m.id}" data-c="${ci}" data-e="${ei}" aria-pressed="${e.on}">
          ${svg("check")}<span>${tx(e)}</span>
        </button>`).join("");
      return `<div class="claim">
        <div class="c-line">
          <span class="c-dot ${dot}" aria-hidden="true"></span>
          <div>
            <div class="c-txt">${tx(c)}${c.rewritten?`<span class="c-note">${u.stRw}</span>`:""}${on===0?`<span class="c-note bad">${u.stBad}</span>`:""}</div>
          </div>
        </div>
        <div class="c-tags"><span class="tags-lb">${u.evLb}</span>${chips}</div>
      </div>`;
    }).join("");
    return `<section class="group" id="g-${m.id}" aria-labelledby="h-${m.id}">
      <div class="g-head">
        <span class="ic">${svg(m.icon)}</span>
        <h2 id="h-${m.id}">${u.mods[m.id]}</h2>
        <span class="meta"><span class="dot ${st.level}" style="width:7px;height:7px;border-radius:50%;display:inline-block"></span>${st.label}</span>
      </div>
      <div class="g-body">${fields}${claims}</div>
    </section>`;
  }).join("");

  document.querySelectorAll("#board input").forEach((inp)=>{
    inp.addEventListener("input", (ev)=>{
      const m = S.modules.find((x)=>x.id===ev.target.dataset.mod);
      m.fields.find((x)=>x.k===ev.target.dataset.k).v = ev.target.value;
      renderPanel();
    });
  });
  document.querySelectorAll("#board .chip").forEach((ch)=>{
    ch.addEventListener("click", ()=>{
      const m = S.modules.find((x)=>x.id===ch.dataset.mod);
      const c = m.claims[+ch.dataset.c];
      const e = c.ev[+ch.dataset.e];
      e.on = !e.on;
      ch.setAttribute("aria-pressed", String(e.on));
      renderBoard();    // keep dots / notes in sync
      renderPanel();    // and re-score immediately
    });
  });
}

/* ================= scoring ================= */
function compute(){
  let filled=0, reqTotal=0, claimTotal=0, covered=0;
  const mods = {}, issues = [];

  S.modules.forEach((m)=>{
    let mF=0, mR=0, mC=0, mCov=0, mIss=0;
    m.fields.forEach((f)=>{ mR++; reqTotal++; if(String(f.v).trim()){filled++; mF++;} });
    m.claims.forEach((c, ci)=>{
      mC++; claimTotal++;
      const on = c.ev.filter((e)=>e.on).length;
      const hy = hypeCount(c);
      const num = NUM.test(c.z);
      if(on){ mCov++; covered++; }

      // 授权问题优先级最高：同一案例只报一条，避免重复
      const auth = m.id === "cas" ? c.ev.find((e)=>/授权|authorization/i.test(e.z+e.e)) : null;
      const authMissing = !!(auth && !auth.on);

      if(authMissing){
        mIss++;
        issues.push({s:"h", mod:m.id,
          zh:`「${clip(c.z)}」缺客户公开授权，涉及客户名称与数据`,
          en:`"${clip(c.e)}" lacks client authorization for public use`,
          fix:{kind:"ev", mod:m.id, c:ci, match:/授权|authorization/i}});
      } else if(!on){
        mIss++;
        issues.push({s:(num||hy)?"h":"m", mod:m.id,
          zh:`「${clip(c.z)}」未绑定任何证据${hy?"，且含夸张表述":""}${num?"，数字无法核验":""}`,
          en:`"${clip(c.e)}" has no evidence${hy?" and uses hype wording":""}${num?"; figures unverifiable":""}`,
          fix:{kind:"ev", mod:m.id, c:ci}});
      } else if(hy){
        mIss++;
        issues.push({s:"h", mod:m.id,
          zh:`「${clip(c.z)}」含 ${hy} 处无实义夸张表述`,
          en:`"${clip(c.e)}" uses ${hy} hype phrase(s)`,
          fix:{kind:"hype", mod:m.id, c:ci}});
      } else if(num && on < 2){
        mIss++;
        issues.push({s:"m", mod:m.id,
          zh:`「${clip(c.z)}」的量化指标只有 ${on} 份证据`,
          en:`"${clip(c.e)}" quantifies results with only ${on} evidence item(s)`,
          fix:{kind:"ev", mod:m.id, c:ci}});
      }
    });
    mods[m.id] = {complete: mR?mF/mR:1, covered: mC?mCov/mC:1, n:mIss};
  });

  const clients = +((S.modules.find(m=>m.id==="cli").fields.find(f=>f.k==="cnt")||{}).v||0);
  const cases = S.modules.find(m=>m.id==="cas").claims.length;
  if(clients >= 50 && cases < 5){
    issues.push({s:"l", mod:"cas",
      zh:`服务客户 ${clients} 家，案例仅 ${cases} 个（基准 5 个）`,
      en:`${clients} clients claimed, only ${cases} cases submitted (benchmark 5)`, fix:null});
  }

  const penalty = issues.reduce((a,i)=>a + (i.s==="h"?16:i.s==="m"?8:3), 0);
  const completeness = reqTotal ? filled/reqTotal : 1;
  const coverage = claimTotal ? covered/claimTotal : 1;
  const consistency = Math.max(0, 1 - penalty/170);
  const score = Math.round(100*(0.3*completeness + 0.4*coverage + 0.3*consistency));
  const lane = (score>=80 && !issues.some(i=>i.s==="h")) ? "pass" : score>=55 ? "quick" : "deep";

  Object.keys(mods).forEach((k)=>{
    const m = mods[k];
    const v = 0.5*m.complete + 0.5*m.covered - m.n*0.12;
    m.level = v>=0.85 ? "green" : v>=0.55 ? "amber" : "red";
    m.label = S.lang==="zh"
      ? (m.level==="green"?"证据充分":m.level==="amber"?"建议补充":"缺口较多")
      : (m.level==="green"?"Well-backed":m.level==="amber"?"Needs a fix":"Major gaps");
  });

  return {completeness, coverage, consistency, score, lane, issues, mods};
}
const clip = (s)=> s.length>28 ? s.slice(0,28)+"…" : s;

/* ================= inspector ================= */
function renderPanel(){
  const u = t(), r = compute();
  document.getElementById("scoreNum").textContent = r.score;
  const C = 2*Math.PI*36, arc = document.getElementById("arc");
  arc.setAttribute("stroke-dasharray", C.toFixed(1));
  arc.setAttribute("stroke-dashoffset", (C*(1-r.score/100)).toFixed(1));
  arc.style.color = r.lane==="pass" ? "var(--green)" : r.lane==="quick" ? "var(--orange)" : "var(--red)";

  const meter = (id, v)=>{ document.getElementById(id+"v").textContent = Math.round(v*100)+"%";
    document.getElementById(id).style.width = Math.round(v*100)+"%"; };
  meter("m1", r.completeness); meter("m2", r.coverage); meter("m3", r.consistency);

  document.getElementById("progVal").textContent = Math.round(r.completeness*100)+"%";
  document.getElementById("progBar").style.width = Math.round(r.completeness*100)+"%";

  const tri = document.getElementById("triage");
  tri.className = "triage " + r.lane;
  document.getElementById("triV").textContent = u[r.lane];
  document.getElementById("triD").textContent = r.lane==="pass"?u.passD : r.lane==="quick"?u.quickD : u.deepD;

  const box = document.getElementById("issues");
  if(!r.issues.length){
    box.innerHTML = `<div class="allgood">${u.allGood}</div>`;
  } else {
    box.innerHTML = r.issues.map((i, idx)=>`
      <div class="iss">
        <span class="sev ${i.s}" aria-hidden="true"></span>
        <div class="b">
          <div class="txt">${S.lang==="zh"?i.zh:i.en}</div>
          <div class="where">${u.mods[i.mod]}</div>
          ${i.fix?`<button class="fix" data-i="${idx}">${i.fix.kind==="hype"?u.fixRw:u.fixEv}</button>`:""}
        </div>
      </div>`).join("");
    box.querySelectorAll(".fix").forEach((b)=>{
      b.addEventListener("click", ()=> applyFix(r.issues[+b.dataset.i]));
    });
  }
  renderNav();
}

function applyFix(i){
  const m = S.modules.find((x)=>x.id===i.fix.mod);
  if(i.fix.kind === "hype"){
    const c = m.claims[i.fix.c];
    const rw = REWRITE[m.id + "-" + i.fix.c] || fallbackRewrite(c);
    c.z = rw.z; c.e = rw.e; c.rewritten = true;
    renderBoard(); renderNav(); renderPanel(); flash(m.id);
    return;
  }
  const c = m.claims[i.fix.c];
  const target = i.fix.match ? c.ev.find((e)=>i.fix.match.test(e.z+e.e)) : c.ev.find((e)=>!e.on);
  if(target) target.on = true;
  renderBoard(); renderNav(); renderPanel(); flash(m.id);
}

function flash(id){
  const el = document.getElementById("g-"+id);
  if(!el) return;
  el.classList.add("focus");
  el.scrollIntoView({behavior:"smooth", block:"nearest"});
  setTimeout(()=>el.classList.remove("focus"), 1100);
}

function autoFill(){
  S.modules.forEach((m)=>m.claims.forEach((c)=>c.ev.forEach((e)=>(e.on = true))));
  renderBoard(); renderPanel();
  notify(t().toastFixT, t().toastFixB);
}

/* ================= toast ================= */
let toastT = null;
function notify(title, body){
  const el = document.getElementById("toast");
  document.getElementById("toastTitle").textContent = title;
  document.getElementById("toastBody").textContent = body;
  el.classList.add("show");
  clearTimeout(toastT);
  toastT = setTimeout(()=>el.classList.remove("show"), 3200);
}

function esc(s){ return String(s).replace(/&/g,"&amp;").replace(/</g,"&lt;").replace(/>/g,"&gt;").replace(/"/g,"&quot;"); }

/* ================= events ================= */
document.getElementById("btnZh").addEventListener("click", ()=>{ if(S.lang!=="zh"){ S.lang="zh"; boot(); } });
document.getElementById("btnEn").addEventListener("click", ()=>{ if(S.lang!=="en"){ S.lang="en"; boot(); } });

document.getElementById("runBtn").addEventListener("click", (ev)=>{
  const b = ev.currentTarget, u = t();
  b.disabled = true;
  const old = b.textContent;
  b.textContent = u.running;
  setTimeout(()=>{
    b.disabled = false; b.textContent = old;
    const r = compute();
    notify(u.toastRunT, u.toastRunB((1.3+Math.random()*0.7).toFixed(1), (0.05+r.issues.length*0.01).toFixed(2)));
  }, 850);
});
document.getElementById("fixBtn").addEventListener("click", autoFill);

/* ================= boot ================= */
function boot(){
  S.current = S.current || "basic";
  renderChrome(); renderPath(); renderBoard(); renderPanel(); renderNav();
}
boot();
</script>
</body>
</html>
