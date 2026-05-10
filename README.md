# TheoryOfAutomats

Theory of Automata lab — synthesis of a digital clock control circuit. Variant 39 (option code `110012221`). Includes a finite automaton model, Logisim circuit files, draw.io state graph, and a full LaTeX report.

## Task (Variant 110012221)

| Option | Value | Meaning |
|--------|-------|---------|
| A = 1 | seconds display | display and correction of seconds |
| B = 1 | dual time zone | two clocks differing by N hours |
| C = 0 | joint correction | tens and units adjusted together |
| D = 0 | 12-hour mode | a.m./p.m. indicator |
| E = 1 | energy save | display can be turned off |
| F = 2 | halt after correction | clock stops after time-set mode |
| G = 2 | stopwatch with lap | lap-time fixation + resume |
| H = 2 | quarter-hour chime | 1-second beep every 15 minutes |
| I = 1 | alarm | 10-second alarm with manual dismiss |

## Formal Model

The automaton is defined as `A = (S, X, Z, s₀, δ, λ)`:

- **S** — 16 states (4-bit encoding `q₁q₂q₃q₄`)
- **X** — 4 input buttons: `S`, `M`, `MS`, `P` (encoded as `x₁x₂`)
- **Z** — 8 output signals `z₀…z₇`
- **δ** — transition function
- **λ** — Mealy output function

### States

| Code | State | Description |
|------|-------|-------------|
| S0 `0000` | Display | Show current time |
| S1 `0001` | Time zone select | Choose/switch time zone |
| S2 `0010` | Time zone offset | Adjust hour offset |
| S3 `0011` | Stopwatch paused | Stopwatch in pause |
| S4 `0100` | Stopwatch running | Stopwatch counting |
| S5 `0101` | Stopwatch stop | Time fixed (lap) |
| S6 `0110` | Stopwatch reset | Reset stopwatch |
| S7 `0111` | Alarm settings | Alarm menu (on/off) |
| S8 `1000` | Set alarm hours | Adjust alarm hour |
| S9 `1001` | Set alarm minutes | Adjust alarm minute |
| S10 `1010` | Breakpoint | RUN/STOP main clock |
| S11 `1011` | Set hours | Adjust main hours |
| S12 `1100` | Set minutes | Adjust main minutes |
| S13 `1101` | Set seconds | Adjust main seconds |
| S14 `1110` | Screen off | Display disabled (power save) |
| S15 `1111` | Alarm active | Alarm ringing |

**Global rule:** button `P` from any state → S14; any button from S14 → S0.

### Inputs

| Button | x₁x₂ |
|--------|-------|
| S | 00 |
| M | 01 |
| MS | 10 |
| P | 11 |

### Outputs

| Signal | Type | Action |
|--------|------|--------|
| z₀ | — | No action |
| z₁ | Pulse | Increment value by 1 |
| z₂ | Pulse | Toggle energy save |
| z₃ | Pulse | Start/stop stopwatch |
| z₄ | Pulse | Reset stopwatch |
| z₅ | Pulse | Dismiss alarm (menu) |
| z₆ | Pulse | Dismiss active alarm |
| z₇ | Pulse | Toggle RUN/STOP main clock |

### Microcommands

**Pulse (impulse) signals** — one-shot on button press:

| Signal | Meaning |
|--------|---------|
| i₁ | Increment current value |
| i₂ | Toggle clock RUN/STOP |
| i₃ | Start/stop stopwatch |
| i₄ | Reset stopwatch |
| i₅ | Dismiss alarm in menu |
| i₆ | Dismiss active alarm |

**Potential (level) signals** — active while in a state:

| Signal | Meaning |
|--------|---------|
| L₁ | Display enable |
| L₂ | Main clock counting enable |
| L₃ | Stopwatch counting enable |
| L₄, L₅ | Multiplexer select (data to display) |
| L₆ | Blink hours digit during correction |
| L₇ | Blink minutes digit during correction |
| L₈ | Blink seconds digit during correction |

## Synthesis

The project includes:

1. **State graph** (`WatchAutomat.drawio`, `img/113.png`) — full automaton with all 16 states, 4 inputs, transitions, and output labels
2. **Transition table** `δ(Sᵢ, X) → Sⱼ` — 16×4 next-state table
3. **Output table** `λ(Sᵢ, X) → zₖ` — 16×4 pulse output table
4. **FL table** `λ_pot(Sᵢ) → L₁…L₈` — potential outputs per state
5. **Truth table (block F)** — full 64-row table with current state `q₁q₂q₃q₄`, input `x₁x₂`, next state `Q₁Q₂Q₃Q₄`, and pulse signals `i₁…i₆`
6. **Karnaugh maps** — logical minimization for each `Qₙ` and `Lₘ`
7. **Logisim circuits** (`Time.circ`, `ddd.circ`) — functional circuit implementation

## Files

| File | Description |
|------|-------------|
| `watch.tex` / `watch.pdf` | Full LaTeX report |
| `WatchAutomat.drawio` | Editable state graph |
| `WatchAutomat.pdf` | Exported state graph |
| `Time.circ` | Main Logisim circuit |
| `ddd.circ` | Supporting Logisim circuit |
| `KuR_MU.pdf` | Lab assignment |
| `img/` | Report screenshots and diagrams |

## Tools

- [Logisim Evolution](https://github.com/logisim-evolution/logisim-evolution) — circuit simulation
- [draw.io](https://draw.io) — state graph
- LaTeX — report
