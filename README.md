# PLC Ladder Lab

An interactive PLC ladder logic simulator for a **bottle-filling conveyor**, built to teach ladder logic one scan at a time. Everything fits on one screen: the live machine, the ladder diagram, the TON timer, the I/O process image, a signal trend, Boolean equations, and a guided lesson.

Siemens S7-1200 style addresses (`I0.0`, `Q0.0`, `Q0.1`, `M0.0`) with IEC 61131-3 symbols.

## Features

- **Live machine:** stainless conveyor, juice tank, solenoid valve, photo sensor with beam, gear motor and a PLC with status LEDs. Bottles fill with a live percentage readout.
- **Live ladder diagram:** power flow is drawn in green. Click any contact, coil or timer to see what it is doing right now.
- **Scan by scan mode:** freeze time and step through *Read inputs → Rung 1 … Rung n → Write outputs* to watch the process image update.
- **TON timer card:** a dial from 0 to 3.0 s, IN/Q lamps, and the Idle → Timing → Done states.
- **Signal trend:** I0.0, Q0.0, Q0.1, TON ET, TON.Q and M0.0 over the last 15 s.
- **Boolean logic:** each rung as an equation with live values substituted in.
- **Guided lessons:** each step has a "Think first" question.
- **Block sensor:** put your hand in the beam to test the logic.

### Programs

| # | Program | Rungs |
|---|---------|-------|
| 1 | **Auto cycle** (default) | Full cycle: stop → fill 3 s → latch Full → bypass restart → reset |
| 2 | Stop at sensor | `Q0.0 = NOT I0.0` |
| 3 | Fill 3 s | adds `Q0.1 = I0.0 AND NOT Q0.0` and a TON timer |

Auto cycle ladder:

```
R1  Q0.0 = NOT I0.0 OR M0.0              (belt motor, with Full bypass)
R2  Q0.1 = I0.0 AND NOT Q0.0 AND NOT M0.0 (filling valve)
R3  TON(IN = Q0.1, PT = 3 s) → SET M0.0   (latch Full)
R4  NOT I0.0 → RESET M0.0                 (bottle left the sensor)
```

### Keyboard

`Space` play/pause · `N` next scan step · `←` `→` lesson steps · `Esc` close menu

## Project structure

```
public/
  index.html     # the whole app (HTML + CSS + JS, no build step)
  favicon.svg
vercel.json      # Vercel config (static, output = public)
netlify.toml     # Netlify config (static, publish = public)
```

No dependencies and no build. Fonts load from Google Fonts.

## Run locally

Open `public/index.html` in a browser, or serve the folder:

```bash
npx serve public
# or
python3 -m http.server -d public 8080
```

## Deploy

### Vercel

**Dashboard:** go to vercel.com → *Add New → Project* → import `zubayer/PLCLadderLogic`. Leave *Framework Preset* as **Other**. `vercel.json` already sets the output directory to `public`. Click **Deploy**.

**CLI:**

```bash
npm i -g vercel
vercel          # first time: link the project
vercel --prod
```

### Netlify

**Dashboard:** go to app.netlify.com → *Add new site → Import an existing project* → GitHub → `zubayer/PLCLadderLogic`. `netlify.toml` already sets the publish directory to `public` with no build command. Click **Deploy**.

**CLI:**

```bash
npm i -g netlify-cli
netlify init    # first time: link the site
netlify deploy --prod --dir=public
```

**Drag and drop:** drop the `public/` folder on app.netlify.com/drop.

Both platforms redeploy automatically on every push to `main` once the repo is connected.

## Course

Built for IE 5355 Computer-Aided Manufacturing, Texas Tech University.
