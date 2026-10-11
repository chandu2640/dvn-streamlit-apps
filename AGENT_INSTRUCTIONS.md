# Task: deploy three Streamlit apps for DVN 36104 Session 2 (Parts C, D, E)

You are helping Chandan submit three artefacts for UTS subject 36104 (Data Visualisation and Narratives), Assignment 2 Part A, Session 2. All code is already written and tested. Your job is to run the apps locally, push them to a public GitHub repository, deploy each one on Streamlit Community Cloud, and create three plain-text submission files containing the deployed URLs.

**Deadline:** Friday 16 October 2026, 11:59pm Sydney time.

Do not rewrite or "improve" the app code. Part C and Part E are the official Streamlit tutorials, copied verbatim on purpose. Part D contains the exact changes the course exercise asks for.

---

## 1. Where everything is

Project folder (macOS, note the spaces and the misspellings in the path, they are real):

```
~/Desktop/university/Data Visualtisation and Nattatives/DVN Assignment 2/streamlit-apps/
```

Contents:

```
streamlit-apps/
├── AGENT_INSTRUCTIONS.md        <- this file
├── README.md
├── requirements.txt             <- one file for all three apps (streamlit, pandas, numpy, altair>=5, pydeck)
├── .gitignore                   <- ignores .venv/, __pycache__/, .DS_Store
├── part_c_uber/
│   └── uber_pickups.py          <- Part C: Streamlit "Create an app" tutorial (Uber pickups), final code
├── part_d_stars/
│   └── star_app.py              <- Part D: course starter + metallicity slider, pair-instability, HR diagram
└── part_e_multipage/
    ├── Hello.py                 <- Part E: Streamlit "Create a multipage app" tutorial entrypoint
    └── pages/
        ├── 1_📈_Plotting_Demo.py
        ├── 2_🌍_Mapping_Demo.py
        └── 3_📊_DataFrame_Demo.py
```

The emoji in the page filenames are intentional (Streamlit uses them as sidebar icons). Keep them exactly as they are.

---

## 2. Run each app locally (smoke test)

```bash
cd ~/Desktop/university/"Data Visualtisation and Nattatives"/"DVN Assignment 2"/streamlit-apps
python3 -m venv .venv
source .venv/bin/activate
pip install --upgrade pip
pip install -r requirements.txt
```

Then start each app one at a time (each serves on http://localhost:8501; stop with Ctrl+C before starting the next):

```bash
streamlit run part_c_uber/uber_pickups.py
streamlit run part_d_stars/star_app.py
streamlit run part_e_multipage/Hello.py
```

If you can't open a browser, use headless mode and check that the server starts without a Python traceback:

```bash
streamlit run part_c_uber/uber_pickups.py --server.headless true
```

### What "working" looks like

| App | Expected behaviour |
|---|---|
| Part C | Title "Uber pickups in NYC", "Done! (using st.cache_data)", a 24-bar histogram of pickups by hour, an hour slider (default 17), and a map of pickups. Needs internet (data loads from an S3 URL). |
| Part D | Black page titled "AppStar". A "log10 metallicity" Streamlit slider at the top (default -1.7). Left panel: a coloured star disc with text readouts (mass, phase, temperature, luminosity). Right panel: a "Hertzsprung-Russell diagram" with a thick grey main-sequence band, a "Sun" marker, O B A F G K M letters, and a yellow star marker. Two in-chart sliders below: "log10 mass (suns)" and "log10 age (Gyr)". On load the star should show as "main sequence" sitting on the band near the Sun marker. |
| Part E | "Welcome to Streamlit! 👋" home page with a sidebar listing Plotting Demo, Mapping Demo, DataFrame Demo. Each page loads. The Mapping Demo basemap may render blank (Mapbox style); that is expected and fine as long as the coloured layers show. |

### Optional quick checks for Part D

- Mass 0 and age around 1.0 on the sliders: phase "red giant", marker moves up and to the right of the band.
- Age around 1.4: "white dwarf", marker drops to the bottom of the diagram.
- Metallicity slider down to about -3.5, mass slider to the far right (log10 mass about 2.3), any age after the star dies: phase "pair-instability: no remnant", and the marker disappears from the HR diagram.

If any app throws an error, stop and report the full traceback to Chandan. Do not change app logic to make it pass.

---

## 3. Push to a public GitHub repository

Repository name: `dvn-streamlit-apps`. It **must be public**, because Streamlit Community Cloud's free tier deploys from public repos.

### Option A: GitHub CLI (preferred if `gh` is installed and authenticated)

```bash
cd ~/Desktop/university/"Data Visualtisation and Nattatives"/"DVN Assignment 2"/streamlit-apps
gh auth status                      # must show a logged-in GitHub account
git init -b main
git add .
git status                          # confirm .venv/ is NOT listed (it is gitignored)
git commit -m "DVN 36104 Session 2 Streamlit apps (Parts C, D, E)"
gh repo create dvn-streamlit-apps --public --source=. --remote=origin --push
```

### Option B: plain git (if `gh` is not available)

1. Ask Chandan to create an empty **public** repo named `dvn-streamlit-apps` at https://github.com/new (no README, no .gitignore, no licence).
2. Then:

```bash
git init -b main
git add .
git commit -m "DVN 36104 Session 2 Streamlit apps (Parts C, D, E)"
git remote add origin https://github.com/<GITHUB_USERNAME>/dvn-streamlit-apps.git
git push -u origin main
```

### Check

Open `https://github.com/<GITHUB_USERNAME>/dvn-streamlit-apps` and confirm that `requirements.txt` sits at the repo root, that the three `part_*` folders are present, and that the three emoji-named files sit in `part_e_multipage/pages/`. Also confirm there is no `.venv` folder in the repo.

---

## 4. Deploy three apps on Streamlit Community Cloud

This step needs Chandan's browser login at https://share.streamlit.io ("Continue with GitHub"). If you can't operate a browser, give him the table below and wait for him to send back the URLs.

For each app: **Create app** → **Deploy a public app from GitHub** → fill in the fields → **Deploy**.

| Field | Part C | Part D | Part E |
|---|---|---|---|
| Repository | `<GITHUB_USERNAME>/dvn-streamlit-apps` | same | same |
| Branch | `main` | `main` | `main` |
| Main file path | `part_c_uber/uber_pickups.py` | `part_d_stars/star_app.py` | `part_e_multipage/Hello.py` |
| App URL (suggested) | `chandan-dvn-uber` | `chandan-dvn-appstar` | `chandan-dvn-multipage` |
| Python version (Advanced settings) | default is fine (3.11 or 3.12) | same | same |

Notes:
- Streamlit Cloud finds `requirements.txt` at the repo root automatically, so no per-folder requirements file is needed.
- The first build takes 2 to 5 minutes per app.
- If the suggested App URL is taken, add a suffix (for example `chandan-dvn-uber-36104`).
- If a build fails, open "Manage app" → logs, copy the error and report it.

### Verify each deployment

Open every deployed URL in a **private/incognito window** (the course explicitly asks for this, to prove it works for someone who isn't logged in). Each should behave as described in the table in section 2.

---

## 5. Create the three submission files

Once the three URLs are live, create these files in the parent folder `DVN Assignment 2/` (not inside `streamlit-apps/`). Each file is plain text containing a label line and the URL:

```bash
cd ~/Desktop/university/"Data Visualtisation and Nattatives"/"DVN Assignment 2"

printf 'Session 2 Part C · Streamlit 1 (Uber pickups tutorial)\n%s\n' "https://<PART_C_URL>.streamlit.app/" > S2C-streamlit1.txt
printf 'Session 2 Part D · Stars (AppStar with metallicity, pair-instability and HR diagram)\n%s\n' "https://<PART_D_URL>.streamlit.app/" > S2D-star-app.txt
printf 'Session 2 Part E · Streamlit 2 (multipage app tutorial)\n%s\n' "https://<PART_E_URL>.streamlit.app/" > S2E-multipage.txt

cat S2C-streamlit1.txt S2D-star-app.txt S2E-multipage.txt
```

Replace each `<PART_X_URL>` with the real app URL.

---

## 6. Done when

- [ ] All three apps ran locally without errors.
- [ ] Public repo `dvn-streamlit-apps` exists on GitHub with the structure above and no `.venv`.
- [ ] Three apps are deployed and load in a private browser window.
- [ ] `S2C-streamlit1.txt`, `S2D-star-app.txt` and `S2E-multipage.txt` exist in `DVN Assignment 2/`, each containing its live URL.
- [ ] Report the three URLs back to Chandan.

Chandan uploads these three .txt files, together with `screen-gamuts.png` (Part A) and `superlative-pairs.png` (Part B), to the **Session 2 Canvas assignment**.

---

## Reference: what changed in `part_d_stars/star_app.py` (do not redo these)

The course starter is at https://dvn36104.github.io/session2/star_app.py. The exercise (Session 2 Part D, section 7) asks for three changes, already made:

1. **Metallicity slider.** The constant `Z = 0.02` is replaced by `lz = st.slider("log10 metallicity", -4.0, -1.4, -1.7, step=0.05)`, `Z = 10 ** lz`, `zr = Z / 0.02`. The two existing lines that use `zr` (luminosity `L = mass**3.5 * zr**-0.1` and the remnant boundary `18 + 7*zr`) respond automatically.
2. **Pair-instability branch.** In `star_state()`, `elif 140 <= mass <= 260 and Z < 0.001: phase = "pair-instability: no remnant"` comes before the neutron-star/black-hole split, with T, L and R set to None. In the grid loop the disc is made transparent for that phase.
3. **HR diagram replaces the phase plane.** Temperature on a log x-axis with `reverse=True` (hot on the left), luminosity on a log y-axis from 1e-4 to 1e6. The main-sequence band is swept from masses with `L = M**3.5` and `T = SUN_T * M**0.475` (both times the same metallicity factor the star uses), drawn as one thick muted line. The moving star is the grid row selected by `transform_filter(pick)`, encoded at `temp_K` and `lum`; supernova and remnant rows have empty `lum` and drop off automatically.

One extra fix: the starter's age slider default was `0.66`, which is not on the precomputed grid (it holds 0.62 and 0.68), so no star appeared on load. The default is now `0.68` (about 4.8 Gyr), so the Sun shows on first load.
