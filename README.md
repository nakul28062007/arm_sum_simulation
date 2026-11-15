# ARM Simulator: Sum of the First N Natural Numbers

An interactive, browser-based simulation of a small ARM assembly program that adds the numbers 1 to N using a loop. Built for the ARM Processor Architecture, Instruction Sets and Programming assignment (Q7).

**Live demo:** https://nakul28062007.github.io/REPO-NAME/

## What it does

- Runs the program one instruction at a time, or automatically at a chosen speed.
- Highlights the instruction being executed.
- Shows live values of `R0` (counter), `R1` (sum) and `PC`, in decimal and hex.
- Shows the `N`, `Z`, `C`, `V` condition flags and how `SUBS` updates them.
- Animates the loop: counter values are consumed while a bar fills up to the final sum.
- Keeps a trace table (R0 and R1 after each pass) and an execution log.
- Works for any N from 1 to 20. Default is N = 10, which gives **55**.

## The ARM program

```asm
        .global _start
_start:
        MOV     R0, #10         @ R0 = N (counter)
        MOV     R1, #0          @ R1 = sum = 0
loop:
        ADD     R1, R1, R0      @ sum = sum + counter
        SUBS    R0, R0, #1      @ counter = counter - 1, updates flags
        BNE     loop            @ repeat while counter != 0
end:
        B       end             @ halt
```

- Instructions used: `MOV`, `ADD`, `SUBS`, `BNE`, `B`.
- Final result: `R1 = 0x37 = 55`, `R0 = 0`, `Z = 1`.
- Check: N(N + 1) / 2 = 10 × 11 / 2 = 55.

## Verified on CPUlator

- The same code was assembled and run on the [CPUlator ARMv7 simulator](https://cpulator.01xz.net/?sys=arm).
- The values in this page's trace table match the registers observed in CPUlator at every step.
- Screenshots are in the `screenshots/` folder (if included).

## Run locally

- Download or clone the repo.
- Open `index.html` in any modern browser. There is no build step and nothing to install.

## Deploy on GitHub Pages

- Make sure the page file is named `index.html` in the repo root.
- Go to **Settings → Pages**.
- Under **Build and deployment**, choose **Deploy from a branch**, select `main` and `/ (root)`, then save.
- After a minute the site is live at `https://<username>.github.io/<repo-name>/`.

## Design decisions and tradeoffs

- **Single HTML file.** HTML, CSS and JavaScript live together, so it deploys anywhere with no tooling. The tradeoff is that the file is less modular than a split project.
- **Not a full ARM emulator.** It simulates only the five instructions this program uses. This keeps the code small and the behaviour easy to follow, but it cannot run arbitrary ARM code. CPUlator is the tool for that.
- **Program is fixed, N is a parameter.** The source stays identical to what was submitted for the assignment, and only the starting value changes.
- **N is limited to 1 to 20.** With N = 0, `SUBS` would produce −1 and never set `Z`, so the loop would never end. The limit also keeps the animation readable.
- **Flags are modelled the way `SUBS` sets them.** `Z` is set when the result is 0, `N` when it is negative, `C` when no borrow occurs, and `V` stays 0 for this program.
- **Works offline and on phones.** The layout is responsive, follows light and dark mode, and respects reduced-motion settings. Google Fonts are optional and fall back to system fonts.

## Tech

- HTML, CSS, vanilla JavaScript. No frameworks or dependencies.

## Project structure

```
.
├── index.html        # the simulator
├── README.md
└── screenshots/      # CPUlator screenshots (optional)
```

## Author

Nakul, B.Tech CSE, SRM Institute of Science and Technology.
