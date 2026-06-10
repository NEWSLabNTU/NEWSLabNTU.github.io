# AutoVeh Landing Page Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build the NEWSLab × Autoware Foundation autonomous-vehicle landing page as a static page at `NEWSLabNTU.github.io/autoveh/`.

**Architecture:** Plain static HTML + CSS + a tiny inline vanilla-JS slideshow, served by GitHub Pages from a new `autoveh/` subdirectory. No build system, no framework. All asset paths relative so it works under the `/autoveh/` subpath. The approved mockup `.superpowers/brainstorm/2150588-1781111591/content/v8-nohdr.html` is the visual reference — this plan ports it into a clean `index.html` + `style.css` with real asset paths.

**Tech Stack:** HTML5, CSS3, vanilla JS. Google Fonts (Source Serif 4 + Inter) via `<link>`. ImageMagick (`convert`) for asset resizing.

**Verification model:** No unit-test framework (static site). "Tests" = deterministic checks: files exist, `curl` returns HTTP 200, HTML references only assets that exist on disk, page renders in a browser matching the mockup. The design spec is `docs/superpowers/specs/2026-06-11-autoveh-landing-page-design.md`.

**Reference paths:**
- Mockup: `/home/aeon/repos/NEWSLabNTU.github.io/.superpowers/brainstorm/2150588-1781111591/content/v8-nohdr.html`
- Poster asset pool: `/home/aeon/repos/IV26_Workshop/deliverables/02-poster-newslab-awf/assets/`
- Target repo root: `/home/aeon/repos/NEWSLabNTU.github.io/`

---

## Task 1: Scaffold the `autoveh/` directory and assets

**Files:**
- Create: `autoveh/assets/logo/`, `autoveh/assets/hero/`, `autoveh/assets/vehicles/` (directories)
- Create (copied + resized): the image assets below

- [ ] **Step 1: Create directories**

```bash
cd /home/aeon/repos/NEWSLabNTU.github.io
mkdir -p autoveh/assets/logo autoveh/assets/hero autoveh/assets/vehicles
```

- [ ] **Step 2: Copy logos (PNG, keep transparency — do not resize hard)**

```bash
P=/home/aeon/repos/IV26_Workshop/deliverables/02-poster-newslab-awf/assets
cp "$P/logo/ntu_emblem_t.png"        autoveh/assets/logo/ntu.png
cp "$P/logo/autoware_foundation.png" autoveh/assets/logo/awf.png
```

- [ ] **Step 3: Copy + resize hero slide photos (≤1600px, q82)**

```bash
P=/home/aeon/repos/IV26_Workshop/deliverables/02-poster-newslab-awf/assets
convert "$P/autosdv/coss_driving.jpg"          -auto-orient -resize '1600x1600>' -quality 82 autoveh/assets/hero/slide1-driving.jpg
convert "$P/golfcart/cart_front.jpg"           -auto-orient -resize '1600x1600>' -quality 82 autoveh/assets/hero/slide2-cart.jpg
convert "$P/perception/coop_perception_rgb.jpg" -auto-orient -resize '1600x1600>' -quality 82 autoveh/assets/hero/slide3-coop.jpg
convert "$P/autosdv/vehicle_right.jpg"         -auto-orient -resize '1600x1600>' -quality 82 autoveh/assets/hero/slide4-autosdv.jpg
```

- [ ] **Step 4: Copy + resize vehicle card photos (≤1200px, q82)**

```bash
P=/home/aeon/repos/IV26_Workshop/deliverables/02-poster-newslab-awf/assets
convert "$P/autosdv/coss_driving.jpg" -auto-orient -resize '1200x1200>' -quality 82 autoveh/assets/vehicles/autosdv.jpg
convert "$P/golfcart/cart_front.jpg"  -auto-orient -resize '1200x1200>' -quality 82 autoveh/assets/vehicles/golfcart.jpg
convert "$P/bus/yuntech_bus.jpg"      -auto-orient -resize '1200x1200>' -quality 82 autoveh/assets/vehicles/bus.jpg
```

- [ ] **Step 5: Verify all assets exist and are non-trivial (>5KB each)**

Run:
```bash
cd /home/aeon/repos/NEWSLabNTU.github.io
find autoveh/assets -type f \( -name '*.jpg' -o -name '*.png' \) -size +5k | sort
```
Expected: 9 files listed — `logo/awf.png`, `logo/ntu.png`, 4 `hero/slide*.jpg`, 3 `vehicles/*.jpg`.

- [ ] **Step 6: Commit**

```bash
cd /home/aeon/repos/NEWSLabNTU.github.io
git add autoveh/assets
git commit -m "autoveh: add resized logo and photo assets"
```

---

## Task 2: Write `style.css`

**Files:**
- Create: `autoveh/style.css`

This is the full T2 stylesheet (extracted from the mockup `<style>`, with the
`.lab` wrapper rules dropped — those were mockup-frame only — and `body`/`html`
made the page root).

- [ ] **Step 1: Write the stylesheet**

```css
:root{
  --cream:#F6F4EE; --ink:#252320; --body:#3D3A33;
  --navy:#1C1850; --tan:#7C7565; --tan2:#9A9384;
  --hair:#E6E2D8; --btn:#FFFDF8; --book:#B1430A;
  --serif:'Source Serif 4',Georgia,serif;
  --sans:'Inter',system-ui,-apple-system,sans-serif;
}
*{box-sizing:border-box;}
html,body{margin:0;padding:0;}
body{font-family:var(--sans);background:var(--cream);color:var(--ink);line-height:1.5;}
a{color:var(--navy);text-decoration:none;}
img{display:block;}

/* hero */
.hero{position:relative;height:60vh;min-height:340px;max-height:560px;overflow:hidden;}
.hero .slide{position:absolute;inset:0;opacity:0;transition:opacity 1.1s ease;}
.hero .slide.on{opacity:1;}
.hero .slide img{width:100%;height:100%;object-fit:cover;}
.hero .scrim{position:absolute;inset:0;
  background:linear-gradient(90deg,rgba(20,18,14,.74) 32%,rgba(20,18,14,.28) 70%,rgba(20,18,14,.1));}
.hero .ov{position:absolute;inset:0;display:flex;flex-direction:column;justify-content:center;
  padding:0 clamp(24px,6vw,72px);max-width:760px;}
.hero .ey{font-size:12px;letter-spacing:2px;text-transform:uppercase;color:#efe9da;font-weight:600;opacity:.9;}
.hero h1{font-family:var(--serif);font-size:clamp(28px,5vw,40px);line-height:1.08;font-weight:700;
  color:#fff;margin:14px 0;letter-spacing:-.2px;}
.hero p{font-size:clamp(14px,1.6vw,15px);line-height:1.6;color:#ece7da;max-width:60ch;}
.hero .dots{position:absolute;bottom:16px;left:clamp(24px,6vw,72px);display:flex;gap:8px;}
.hero .dots i{width:7px;height:7px;border-radius:50%;background:rgba(255,255,255,.45);display:block;}
.hero .dots i.on{background:#fff;}

/* sections */
.sec{padding:clamp(32px,5vw,52px);max-width:1120px;margin:0 auto;}
.eyebrow{font-size:12px;letter-spacing:2px;text-transform:uppercase;color:var(--tan);font-weight:600;}
.sec h2{font-family:var(--serif);font-size:clamp(22px,3vw,28px);font-weight:700;color:var(--navy);
  margin:9px 0 26px;letter-spacing:-.2px;}
.divider{height:1px;background:var(--hair);max-width:1120px;margin:0 auto;}

/* vehicles */
.veh{display:grid;grid-template-columns:repeat(3,1fr);gap:24px;}
.vc .im{height:170px;overflow:hidden;border-radius:8px;}
.vc img{width:100%;height:100%;object-fit:cover;}
.vc h4{font-family:var(--serif);margin:15px 0 6px;font-size:18px;color:var(--navy);font-weight:700;}
.vc p{margin:0;font-size:13px;color:#6b6557;line-height:1.6;}

/* topics */
.topic{padding:32px 0;border-top:1px solid var(--hair);}
.topic:first-of-type{border-top:none;padding-top:4px;}
.topic .k{font-size:11px;letter-spacing:1.6px;text-transform:uppercase;color:var(--tan2);font-weight:600;}
.topic h3{font-family:var(--serif);margin:9px 0 14px;font-size:clamp(20px,2.4vw,23px);color:var(--navy);
  font-weight:700;letter-spacing:-.2px;}
.topic p{margin:0 0 18px;font-size:15px;line-height:1.78;color:var(--body);max-width:80ch;}
.topic .proj a{display:inline-flex;align-items:center;gap:7px;font-size:13px;font-weight:600;
  border:1px solid #d6d1c4;border-radius:8px;padding:9px 16px;margin:0 9px 9px 0;color:var(--ink);background:var(--btn);}
.topic .proj a:hover{border-color:var(--navy);}
.topic .proj a .gh{color:var(--tan2);}
.topic .proj a .bk{color:var(--book);}

/* footer */
.ft{background:var(--navy);padding:clamp(28px,4vw,34px) clamp(24px,6vw,72px);color:#b7b4e0;}
.ft .inner{max-width:1120px;margin:0 auto;}
.ft .lock{display:flex;align-items:center;gap:16px;flex-wrap:wrap;
  padding-bottom:18px;border-bottom:1px solid #322c6e;margin-bottom:16px;}
.ft .lock img{height:40px;background:#fff;border-radius:5px;padding:4px;}
.ft .lock .x{color:#6b66b0;font-size:16px;}
.ft .lock b{color:#fff;font-size:14px;font-weight:600;}
.ft .row{display:flex;justify-content:space-between;align-items:center;gap:12px;flex-wrap:wrap;font-size:12.5px;}
.ft .row a{color:#cfcdf0;}
.ft .row .c{color:#fff;}

/* responsive */
@media(max-width:760px){
  .veh{grid-template-columns:1fr;}
}

/* respect reduced motion: freeze slideshow crossfade */
@media(prefers-reduced-motion:reduce){
  .hero .slide{transition:none;}
}
```

- [ ] **Step 2: Verify CSS is valid (no syntax errors)**

Run:
```bash
cd /home/aeon/repos/NEWSLabNTU.github.io
node -e "const c=require('fs').readFileSync('autoveh/style.css','utf8');const o=(c.match(/{/g)||[]).length,x=(c.match(/}/g)||[]).length;if(o!==x)throw new Error('brace mismatch '+o+'/'+x);console.log('braces ok',o)"
```
Expected: `braces ok <N>` (open == close).

- [ ] **Step 3: Commit**

```bash
cd /home/aeon/repos/NEWSLabNTU.github.io
git add autoveh/style.css
git commit -m "autoveh: add T2 stylesheet"
```

---

## Task 3: Write `index.html`

**Files:**
- Create: `autoveh/index.html`

Full page markup with the `<head>` (fonts, meta, stylesheet), the hero/vehicles/
topics/footer sections, and the inline slideshow script. Asset paths are relative
(`assets/…`). Real repo/book URLs from the spec are wired into the buttons/footer.

- [ ] **Step 1: Write the HTML**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Autoware as a Practical Research Platform — NEWSLab × Autoware Foundation</title>
  <meta name="description" content="The NTU NEWSLab Autonomous Vehicle team — software-defined vehicles, open tooling, and real-world deployments built on Autoware, in collaboration with the Autoware Foundation.">
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&family=Source+Serif+4:opsz,wght@8..60,600;8..60,700&display=swap" rel="stylesheet">
  <link rel="stylesheet" href="style.css">
</head>
<body>

  <section class="hero" id="hero">
    <div class="slide on"><img src="assets/hero/slide1-driving.jpg" alt="Autonomous vehicle driving on the NTU campus"></div>
    <div class="slide"><img src="assets/hero/slide2-cart.jpg" alt="Autonomous golf cart platform"></div>
    <div class="slide"><img src="assets/hero/slide3-coop.jpg" alt="Cooperative perception view"></div>
    <div class="slide"><img src="assets/hero/slide4-autosdv.jpg" alt="AutoSDV vehicle platform"></div>
    <div class="scrim"></div>
    <div class="ov">
      <div class="ey">NTU NEWSLab &times; Autoware Foundation</div>
      <h1>Autoware as a Practical Research Platform</h1>
      <p>Software-defined autonomous vehicles built on Autoware — cross-compiled, deployed, and driven on real platforms from a research cart to a full-size bus.</p>
    </div>
    <div class="dots" id="dots"><i class="on"></i><i></i><i></i><i></i></div>
  </section>

  <section class="sec">
    <div class="eyebrow">Platforms</div>
    <h2>Vehicles</h2>
    <div class="veh">
      <div class="vc"><div class="im"><img src="assets/vehicles/autosdv.jpg" alt="AutoSDV"></div><h4>AutoSDV</h4><p>Software-defined vehicle reference platform — our flagship Autoware testbed.</p></div>
      <div class="vc"><div class="im"><img src="assets/vehicles/golfcart.jpg" alt="Golf cart"></div><h4>Golf Cart</h4><p>NTU &times; NTUST autonomous campus shuttle running the full Autoware stack.</p></div>
      <div class="vc"><div class="im"><img src="assets/vehicles/bus.jpg" alt="YunTech bus"></div><h4>YunTech Bus</h4><p>Full-size bus — scaling the stack toward public-road class vehicles.</p></div>
    </div>
  </section>

  <div class="divider"></div>

  <section class="sec">
    <div class="eyebrow">What we work on</div>
    <h2>Topics</h2>

    <div class="topic">
      <div class="k">Platform</div>
      <h3>Software-Defined Vehicle Platform</h3>
      <p>AutoSDV is our full Autoware-based autonomy stack, packaged so it actually runs on real hardware rather than only in simulation. We cross-compile and containerize the entire pipeline — from sensor drivers through localization, planning, and drive-by-wire — into a reproducible image that boots on an NVIDIA AGX-class compute unit. The accompanying book documents the architecture and walks through bring-up end to end.</p>
      <div class="proj">
        <a href="https://github.com/NEWSLabNTU/autosdv"><span class="gh">&#9636;</span> AutoSDV — GitHub</a>
        <a href="https://newslabntu.github.io/autosdv-book/"><span class="bk">&#9636;</span> AutoSDV Book</a>
      </div>
    </div>

    <div class="topic">
      <div class="k">Real-time core</div>
      <h3>ROS on Microcontrollers &amp; the Safety Island</h3>
      <p>nano-ros brings the ROS 2 programming model down onto microcontrollers and RTOS targets, letting the safety island speak the same language as the rest of the Autoware stack. This is the basis of our contribution to the Autoware Foundation reference-design working group, separating the high-compute autonomy domain from a certifiable real-time safety domain.</p>
      <div class="proj">
        <a href="https://github.com/NEWSLabNTU/nano-ros"><span class="gh">&#9636;</span> nano-ros — GitHub</a>
        <a href="https://newslabntu.github.io/nano-ros-book/"><span class="bk">&#9636;</span> nano-ros Book</a>
      </div>
    </div>

    <div class="topic">
      <div class="k">Build &amp; deploy</div>
      <h3>Launch Orchestration &amp; Deployment</h3>
      <p>A full Autoware bring-up launches hundreds of nodes with tangled dependencies. play_launch orchestrates startup and exposes a resource timeline so you can see what runs when and what competes for CPU and GPU. Alongside it, colcon2deb and autoware-localrepo turn a colcon workspace into versioned Debian packages, so a field deployment is an apt install rather than a from-source rebuild.</p>
      <div class="proj">
        <a href="https://github.com/NEWSLabNTU/play_launch"><span class="gh">&#9636;</span> play_launch — GitHub</a>
        <a href="https://github.com/NEWSLabNTU/colcon2deb"><span class="gh">&#9636;</span> colcon2deb — GitHub</a>
        <a href="https://github.com/NEWSLabNTU/autoware-localrepo"><span class="gh">&#9636;</span> autoware-localrepo — GitHub</a>
      </div>
    </div>

    <div class="topic">
      <div class="k">Sensing</div>
      <h3>Sensor Calibration &amp; Cooperative Perception</h3>
      <p>Reliable perception starts with sensors that agree on where the world is. LCTK, our Lidar-Camera Toolkit, provides the calibration and fusion utilities to align lidar and camera frames and project detections between them. On top of single-vehicle sensing, we work on cooperative perception — sharing detections between vehicles and roadside units over V2X so each agent sees beyond its own line of sight.</p>
      <div class="proj">
        <a href="https://github.com/NEWSLabNTU/LCTK"><span class="gh">&#9636;</span> LCTK — GitHub</a>
      </div>
    </div>

    <div class="topic">
      <div class="k">Localization &amp; networking</div>
      <h3>GPU-Accelerated Localization &amp; 5G</h3>
      <p>Map-based localization is on the critical path of every drive, and NDT scan matching is expensive. Our CUDA NDT matcher moves that work onto the GPU, cutting per-scan latency enough to free CPU budget for the rest of the stack. We pair this with 5G-enabled deployment, offloading heavier compute off-vehicle over low-latency links.</p>
      <div class="proj">
        <a href="https://github.com/NEWSLabNTU/cuda_ndt_matcher"><span class="gh">&#9636;</span> cuda_ndt_matcher — GitHub</a>
      </div>
    </div>
  </section>

  <footer class="ft">
    <div class="inner">
      <div class="lock">
        <img src="assets/logo/ntu.png" alt="National Taiwan University">
        <span class="x">&times;</span>
        <img src="assets/logo/awf.png" alt="Autoware Foundation">
        <b>National Taiwan University · NEWSLab &nbsp;&times;&nbsp; Autoware Foundation</b>
      </div>
      <div class="row">
        <span class="c">© NEWSLab, NTU CSIE · <a href="https://www.csie.ntu.edu.tw/~cshih/">Prof. Chi-Sheng Shih</a></span>
        <span>AutoSDV · nano-ros · play_launch · LCTK</span>
      </div>
    </div>
  </footer>

  <script>
  (function(){
    if (window.matchMedia && window.matchMedia('(prefers-reduced-motion: reduce)').matches) return;
    var slides=document.querySelectorAll('#hero .slide'),
        dots=document.querySelectorAll('#dots i'), i=0;
    setInterval(function(){
      slides[i].classList.remove('on'); dots[i].classList.remove('on');
      i=(i+1)%slides.length;
      slides[i].classList.add('on'); dots[i].classList.add('on');
    }, 3500);
  })();
  </script>

</body>
</html>
```

- [ ] **Step 2: Verify every referenced local asset exists on disk**

Run:
```bash
cd /home/aeon/repos/NEWSLabNTU.github.io/autoveh
grep -oE 'src="assets/[^"]+"' index.html | sed -E 's/src="//;s/"//' | sort -u | while read f; do
  [ -f "$f" ] && echo "OK  $f" || echo "MISSING  $f";
done
```
Expected: every line starts with `OK` (9 assets). No `MISSING`.

- [ ] **Step 3: Verify no leftover absolute/mockup paths**

Run:
```bash
cd /home/aeon/repos/NEWSLabNTU.github.io/autoveh
grep -nE 'src="(veh-hero|d[0-9]|ntu\.png|awf\.png)' index.html || echo "clean"
```
Expected: `clean` (no mockup-flat filenames leaked in).

- [ ] **Step 4: Commit**

```bash
cd /home/aeon/repos/NEWSLabNTU.github.io
git add autoveh/index.html
git commit -m "autoveh: add landing page index.html"
```

---

## Task 4: Serve locally and verify rendering

**Files:** none (verification only)

- [ ] **Step 1: Start a static server at the repo root**

```bash
cd /home/aeon/repos/NEWSLabNTU.github.io
python3 -m http.server 8099 >/tmp/autoveh-serve.log 2>&1 &
echo $! > /tmp/autoveh-serve.pid
sleep 1
```

- [ ] **Step 2: Verify the page and assets return HTTP 200 under the /autoveh/ subpath**

Run:
```bash
for u in autoveh/ autoveh/style.css autoveh/assets/hero/slide1-driving.jpg autoveh/assets/logo/ntu.png autoveh/assets/vehicles/bus.jpg; do
  printf '%s  ' "$(curl -s -o /dev/null -w '%{http_code}' http://localhost:8099/$u)"; echo "$u";
done
```
Expected: every line begins with `200`.

- [ ] **Step 3: Visual check in a browser**

Open `http://localhost:8099/autoveh/`. Confirm against the mockup `v8-nohdr`:
- Hero fills the top, photos cross-fade every ~3.5s, dots track the active slide.
- Overlay eyebrow/title/lede are legible over the scrim.
- "Vehicles" shows 3 columns (AutoSDV / Golf Cart / YunTech Bus) — and is partly visible above the fold.
- 5 topics render with serif headings + repo/book buttons; buttons hover to navy border.
- Footer is a navy band with the NTU × AWF lockup and the Prof. Shih link.
- Resize narrow (<760px): vehicle columns stack to one column.

- [ ] **Step 4: Stop the server**

```bash
kill "$(cat /tmp/autoveh-serve.pid)" 2>/dev/null; rm -f /tmp/autoveh-serve.pid
```

- [ ] **Step 5: Spot-check external links resolve (HTTP 200/3xx)**

Run:
```bash
for u in https://github.com/NEWSLabNTU/autosdv https://newslabntu.github.io/autosdv-book/ https://github.com/NEWSLabNTU/nano-ros https://newslabntu.github.io/nano-ros-book/ https://github.com/NEWSLabNTU/play_launch https://github.com/NEWSLabNTU/colcon2deb https://github.com/NEWSLabNTU/autoware-localrepo https://github.com/NEWSLabNTU/LCTK https://github.com/NEWSLabNTU/cuda_ndt_matcher https://www.csie.ntu.edu.tw/~cshih/; do
  printf '%s  ' "$(curl -s -L -o /dev/null -w '%{http_code}' "$u")"; echo "$u";
done
```
Expected: every line begins with `200`.

---

## Task 5: Final commit and deployment note

**Files:**
- Verify: `.gitignore` already contains `.superpowers/` (added during brainstorming)

- [ ] **Step 1: Confirm `.superpowers/` is ignored and the tree is clean except the new page**

Run:
```bash
cd /home/aeon/repos/NEWSLabNTU.github.io
grep -q '.superpowers/' .gitignore && echo "gitignore ok"
git status --porcelain
```
Expected: `gitignore ok`; `git status` shows nothing under `.superpowers/` and no stray files (only intended commits already made).

- [ ] **Step 2: Push (only if the user asks — see project note on git-svn / GitHub)**

This repo publishes via GitHub Pages on push to the default branch. Pushing will
make the page live at https://newslabntu.github.io/autoveh/ within ~1 minute.
Do not push unless the user requests it.

```bash
cd /home/aeon/repos/NEWSLabNTU.github.io
git push origin HEAD
```

- [ ] **Step 3: Confirm live (after push, if performed)**

```bash
curl -s -o /dev/null -w '%{http_code}\n' https://newslabntu.github.io/autoveh/
```
Expected: `200`.

---

## Self-Review (completed)

- **Spec coverage:** deploy at `/autoveh/` (T1,T4) · static no-build (T2,T3) · relative paths (T3 step3) · hero slideshow + reduced-motion (T2,T3) · 3 vehicle columns (T3) · 5 topics with prose + repo/book buttons (T3) · footer lockup + Prof. Shih (T3) · T2 fonts/palette (T2) · assets resized from poster pool (T1) · `.superpowers/` ignored (T5). All covered.
- **Placeholder scan:** none — full CSS/HTML/JS and exact commands included.
- **Type/path consistency:** asset filenames produced in T1 (`hero/slide1-driving.jpg`, `vehicles/autosdv.jpg`, `logo/ntu.png`, …) match exactly the `src=` paths in T3 and the verify globs in T3/T4.
- **Constraints honored:** root `index.html`/submodules untouched; no `autoware_sentinel`; no DBW spec.
