# TOTEM — Norwegian keymap

ZMK keymap for the [TOTEM](https://github.com/GEIGEIGEIST/TOTEM) (38 keys), built for Norwegian (Nynorsk) and English. QWERTY base with homerow mods, a togglable [Hands Down Promethium](https://sites.google.com/alanreiser.com/handsdown) layer adapted for Norwegian, and æ/ø/å as real keys on both writing layers.

**Assumes the OS keyboard layout is standard Norwegian (`no`).** All scancodes are chosen against it:

| Letter | Scancode | | Symbol | Scancode |
|---|---|---|---|---|
| å | `LBKT` | | `'` | `BSLH` |
| ø | `SEMI` | | `?` | `LS(MINUS)` |
| æ | `SQT` | | | |

## Physical key numbering

Combo definitions and the positional homerow mods reference these ZMK positions:

```
         0  1  2  3  4        5  6  7  8  9
        10 11 12 13 14       15 16 17 18 19
    20  21 22 23 24 25       26 27 28 29 30  31
               32 33 34       35 36 37
```

Positions 20 and 31 are the outer pinky keys; 32–37 are the thumbs.

## Layer stack

![Layer stack](img/stack.svg)

Ordering is load-bearing: ZMK resolves each keypress from the highest-numbered **active** layer downward, and `&trans` only falls *down*. PROM is a toggle, so it sits at 1 — *below* the held NAV/SYM layers — otherwise holding NAV with PROM toggled would surface Promethium letters instead of arrows. GAME sits *above* NAV/SYM deliberately: it replaces everything and never needs them.

## Layers

### BASE (0)

![BASE layer](img/base.svg)

QWERTY with GACS-order homerow mods. Norwegian letters are real keys: **å** on the left outer, **æ** on the right outer, **ø** on the right pinky home as a mod-tap (`&mt RGUI SEMI`) — the position Norwegian keyboards put it anyway. No æøå combos.

Thumbs: `ESC · SPC/sym · TAB — R · BSP/nav · RET`. The **R on the right inner thumb is a duplicate** of the alpha R — a zero-cost trial key for Promethium's thumb-R: type QWERTY normally, opt into thumb-R when you think of it, and judge over weeks whether a high-frequency thumb letter holds up. If it doesn't earn its place, rebind it (DEL is the natural fallback).

### PROM (1)

![PROM layer](img/prom.svg)

Hands Down Promethium adapted for Norwegian; dashed keys deviate from stock:

- **J ↔ W swapped** — stock puts J directly under K on the same index column, making the very frequent Norwegian *kj* (ikkje, kjem, kanskje) a same-finger skip. Cost: W lands on the inner bottom, slightly worse for English.
- **Å and Ø replace `=` and `/`** — both symbols live on SYM; the letters (1.5 % and 0.9 % of Norwegian text) outrank them.
- **Æ replaces `;`** — `;` can't be `&kp SEMI` on a Norwegian OS (that scancode *is* ø), and it was the weakest allocation on the board.
- **Z and Q on the outer keys** — stock leaves them off the main 30.

R on the thumb and everything undashed is stock Promethium. Thumbs are all `&trans`, falling through to BASE — one thumb scheme everywhere. The home row uses the positional `hml`/`hmr` behaviors (below), not `&mt`.

### NAV (2) and SYM (3)

![NAV layer](img/nav.svg)

![SYM layer](img/sym.svg)

Held from the middle thumbs. Blank keys are `&none` — dead, not transparent — so no QWERTY or Promethium letter is reachable from a held layer regardless of which base is toggled. NAV's right inner thumb is `DEL` (forward-delete while navigating, and it restores the dedicated DEL the thumb reshuffle removed); SYM's is `&none`. New on SYM: `?` (displaced from BASE by ø) and a plain `'`.

### GAME (4) and GAME2 (5)

![GAME layer](img/game.svg)

![GAME2 layer](img/game2.svg)

Left-hand gaming cluster with its own plain SPACE (so jump-hold never goes through a layer-tap). GAME2 is momentary from GAME's middle thumb.

## Combos

![Combos](img/combos.svg)

| Combo | Positions | On BASE | On PROM | Output | Active on | Guard |
|---|---|---|---|---|---|---|
| `combo_esc` | 0 + 1 | Q+W | F+P | `ESC` | all layers | — |
| `combo_prom` | 0 + 9 | Q+P | F+B | `&tog PROM` | all layers | 125 ms idle |
| `combo_game` | 20 + 31 | Å+Æ | Z+Q | `&tog GAME` | all layers | — |

All use `timeout-ms = <50>` (both keys within 50 ms). The PROM toggle carries `require-prior-idle-ms = <125>`: it cannot fire within 125 ms of normal typing, so no fast letter sequence ever swaps the whole layout mid-sentence. A misfired ESC is merely annoying, hence no guard there. `&tog` means the same chord enters and exits a layer.

## Behaviors

### `&mt` — BASE homerow mods

`tap-preferred`, 220 ms tapping term, 100 ms quick-tap, 100 ms prior-idle. Tap for the letter, hold-then-press-another-key for the modifier, double-tap-and-hold to repeat the letter. QWERTY's home row has few frequent rolls, so the simple flavor is safe here.

### `&lt` — thumb layer-taps

`balanced`, 200 ms tapping term, **`quick-tap-ms = <175>`**. The quick-tap matters because SPC and BSP sit on layer-taps: tap-then-hold within 175 ms **repeats the key** instead of opening the layer — so double-tap-and-hold backspace still chews through characters, and space still auto-repeats.

### `hml` / `hmr` — positional homerow mods (PROM only)

Custom hold-taps that only allow the *hold* interpretation when the **next key is on the other half** (or a thumb). What this means for keypresses:

- **Typing at speed → always letters.** Any key pressed within 125 ms of the previous one resolves as a tap (`require-prior-idle-ms`). Mods are unreachable mid-flow.
- **Same-hand sequences → always letters.** Hold T, press S (both left): "ts", never Ctrl+S. PROM's home row carries frequent same-hand rolls (*ei*, *st*, *nt*) that plain `&mt` would misfire on; `hold-trigger-key-positions` makes that physically impossible.
- **Cross-hand chords → mods.** Pause, hold T, press any right-hand key: Ctrl+key. Modifiers for a left-hand letter come from the right hand and vice versa.
- **Stacked mods work** via `hold-trigger-on-release`: hold S+N (Gui+Alt), press a right-hand key.
- **Doubles work** via `quick-tap-ms`: *tt*, *nn*, *ss* type cleanly, and the second press auto-repeats if held.
- **Thumbs count as cross-hand** from either side, so Ctrl+Space and Shift+Enter work from any mod.

One line: **letters while moving, mods while pausing, mods only chord across the hands.**

The position lists are simply "the other half plus all thumbs": `hml` (left-hand mods) triggers on 5–9, 15–19, 26–31, 32–37; `hmr` mirrors with 0–4, 10–14, 20–25, 32–37.

## Toggles

| Layer | In/out | Where | Note |
|---|---|---|---|
| PROM | `&tog` combo | top corners (0+9) | idle-guarded; below NAV/SYM in the stack |
| GAME | `&tog` combo | outer keys (20+31) | above NAV/SYM; own thumb bindings |
| GAME2 | `&mo` hold | GAME middle thumb | momentary, not a toggle |

## Regenerating the images

All diagrams are generated: `python3 gen.py` rewrites everything in `img/`.
