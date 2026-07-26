# TOTEM — Norwegian keymap

ZMK keymap for the 38-key TOTEM, built for Norwegian (Nynorsk) and English with heavy Emacs use. QWERTY base, a togglable Hands Down Promethium layer tuned for Norwegian, held NAV/SYM layers, a conditional FUN layer, and a GAME layer with its own thumbs.

Design principles: **bare modifiers on the left thumb** (Emacs chording with no hold-tap timing), **shift-only homerow mods**, **æ/ø/å as real keys** on both writing layers, and one thumb scheme shared by every typing layer.

**Assumes the OS keyboard layout is standard Norwegian (`no`).** Everything below is chosen against that layout's scancodes.

## Physical key numbering

```
         0  1  2  3  4        5  6  7  8  9
        10 11 12 13 14       15 16 17 18 19
    20  21 22 23 24 25       26 27 28 29 30  31
               32 33 34       35 36 37
```

20 and 31 are the outer pinky keys; 32–34 are the left thumb (outer→inner), 35–37 the right thumb (inner→outer).

## Thumb clusters — the core of the design

```
   LEFT                              RIGHT
   LALT  │   R   │  LCTL       BSP   │  SPC   │  RET
   bare  │  bare │  bare       ▸SYM  │  ▸NAV  │ ⌃RCTL-hold
```

- **LCTL and LALT are bare keys** — no tap side, no timing semantics, no misfire class. Chained Emacs sequences (`C-x C-s`, `C-c C-c`, held `C-n`) are one held thumb plus finger taps.
- **R on the resting left thumb** — stock Promethium's thumb and hand (opposite the vowels, so *er/ar/or/re* alternate). On BASE it duplicates the alpha R: a live trial of thumb-R at zero cost. Bare, because holding a letter has no use.
- **SPC on the resting right thumb** carrying **NAV**; **BSP inner** carrying **SYM**. Tap-then-hold within 175 ms repeats the key instead of opening the layer. Holding both raises **FUN**.
- **RET outer with RCTL on hold** — the only hold-tap modifier, safe because RET never occurs mid-roll. It gives Ctrl cross-hand from both sides and makes `C-M-` chords pressable (RCTL + LALT is two thumbs).
- **NAV and SYM re-add SHIFT on the left thumb** (position 33). Without it there is no Shift on those layers at all — Shift lives on the home row, which NAV/SYM overwrite — so Shift+arrow, Shift+Home/End and Shift+F-key would be impossible. With it the left thumb becomes a complete **ALT · SHIFT · CTRL** cluster while the right hand works the arrows.

## Layer stack

![Layer stack](img/stack.svg)

ZMK resolves each keypress from the highest-numbered **active** layer downward; `&trans` only falls *down*.

```
6  FUN    conditional — NAV+SYM held together, or the FUN key on NAV
5  GAME2  momentary (&mo from GAME)
4  GAME   toggle — replaces everything, own thumbs
3  SYM    hold (BSP thumb)
2  NAV    hold (SPC thumb)
1  PROM   toggle — below the held layers, so holds never surface Promethium letters
0  BASE   default, always active
```

PROM sits at 1 so a held NAV/SYM always wins over it. GAME sits above NAV/SYM deliberately: it replaces everything and never needs them. FUN sits on top; GAME's thumbs carry no NAV/SYM holds, so FUN is unreachable while gaming. FUN's thumbs are all `&trans` so the holds and the thumb mods stay alive underneath.

## Layers

### BASE (0)

![BASE layer](img/base.svg)
```
        Q    W    E    R    T          Y    U    I    O    P
        A    S    D    F    G          H    J    K    L    Ø
                      sft                   sft            gui
  Å     Z    X    C    V    B          N    M    ,    .    -     Æ
               LALT   R   LCTL        BSP  SPC  RET
                                      sym  nav  ctl
```

QWERTY with shift-only homerow mods (F/J) and GUI on Ø. Norwegian letters are real keys: **å** left outer, **æ** right outer, **ø** on the right pinky home — its standard national position. The thumb **R** duplicates the alpha R.

### PROM (1)

![PROM layer](img/prom.svg)
```
        F    P    D    L    X          Æ    U    O    Y    B
        S    N    T    H    K          ,    A    E    I    C
                      sft                   sft            gui
  Z     V    J    G    M    W          -    .    '    Å    Ø     Q
               LALT   R   LCTL        BSP  SPC  RET
                                      sym  nav  ctl
```

Hands Down Promethium, adhered to as closely as possible; dashed keys in the image deviate from stock:

- **J ↔ W swapped** — stock puts J directly under K on the same index column, making the frequent Norwegian *kj* (ikkje, kjem, kanskje) a same-finger skip. Cost: W lands on the inner bottom. Reversible; the corpus/analyzer run holds the veto.
- **Å, Ø, Æ replace `=`, `/`, `;`** — all three displaced symbols live on SYM; the letters outrank them by an order of magnitude in Norwegian. `;` additionally cannot be `&kp SEMI` here — that scancode *is* ø.
- **Z and Q on the outer keys** — stock leaves them off the main 30.

Thumbs are all `&trans`, falling through to BASE — including R, which is simply *the* R here. NAV, SYM and FUN are therefore identical on both writing layers.

### NAV (2)

![NAV layer](img/nav.svg)
```
        6    7    8    9    0         ⌃←   FUN   ·   ⌃→    ·
        1    2    3    4    5          ←    ↓    ↑    →   GUI
  ·     ·    ·    ·    ·    ·        Home PgDn PgUp End  Del     ·
               LALT SHIFT LCTL        BSP ▼NAV  RET
```

- **SHIFT on the left thumb** (see above) makes Shift+arrow selection work; combined with RCTL on RET, `C-S-<arrow>` (extend by word) is two thumbs plus a finger.
- **GUI on the right pinky home** — the digits live on this layer, so without it `Super+1…9` for MangoWC workspaces would need a 220 ms mod-tap hold on Ø first. Cross-hand from the digit grid.
- **FUN key at position 6** — a one-thumb path to FUN: hold SPC, press FUN with the right index, and the left hand is free for F-keys. The two-thumb conditional still works.
- Blank keys are `&none`, so no BASE or PROM letter leaks through.

### SYM (3)

![SYM layer](img/sym.svg)
```
        &    /    (    )    =          `    ~    <    >    ^
        !    "    #    $    %          {    [    ]    }    |
  ·     '    @    £    ?    €          \    *    +    -    ´     _
               LALT SHIFT LCTL       ▼SYM  SPC  RET
```

The left half is a **three-level map of the number row**, column by column: NAV gives the digit, SYM's home row gives Shift+digit, SYM's bottom row gives AltGr+digit in the *same column*.

| Column | NAV | SYM home | SYM bottom |
|---|---|---|---|
| ring | 2 / 7 | `"` | `@` (AltGr+2) |
| middle | 3 / 8 | `#` | `£` (AltGr+3) |
| index | 4 / 9 | **`$`** (AltGr+4) | `?` |
| inner | 5 / 0 | `%` | `€` (AltGr+5) |

**Currencies.** All three are AltGr+digit on the Norwegian layout (`3#£ 4¤$ 5%€`), so they fit without a second symbol layer. `$` is prioritised as requested: it takes the **index home** slot, displacing `¤` — the generic currency sign nobody types, still reachable as thumb-Shift+4 on NAV. `£` and `€` sit in their own digit columns on the row below, so the mnemonic is "same column, one row down". `?` takes the freed index-bottom slot since it's the more frequent character.

**Dead keys.** `` ` ``, `~` and `^` are *dead keys* on the Norwegian layout: pressed alone they emit nothing until a following character, which silently breaks code fences, `~/` and regex carets. They are macros here — dead key followed by space — which is the portable fix that needs no OS-side `nodeadkeys` variant, important on a locked-down work machine. `´` is left as a genuine dead key (`RA(EQUAL)`, corrected from a scancode that produced nothing) so `´`+`e` still gives `é` for *idé*, *kafé*.

Bluetooth moved to FUN. `_` is kept at the outer pinky as a one-key convenience even though thumb-Shift+`-` now also produces it.

### FUN (6)

![FUN layer](img/fun.svg)
```
       F6   F7   F8   F9   F10       PREV VOL- VOL+ NEXT PLAY
       F1   F2   F3   F4   F5        BT0  BT1  BT2  BTclr OUT
  ·     ·   F11  F12   ·    ·         ·   MUTE  ·    ·    ·     ·
               LALT SHIFT LCTL       ▼SYM ▼NAV  RET
```

Reached by holding both right thumbs, or by the FUN key on NAV. F-keys sit on the same left-hand grid as NAV's digits — one spatial map, three meanings: digit (NAV), shifted digit (SYM), F-key (FUN). Thumbs fall through, so `Alt+F4` and `Ctrl+F2` and `Shift+F5` all work.

`&bootloader` and `&sys_reset` are **no longer bare keys** — they are combos (23+24 and 26+30), restricted to `layers = <FUN>`, and both pairs sit on `&none` positions so a single press does nothing. That's three simultaneous conditions before anything destructive happens.

### GAME (4) and GAME2 (5)

![GAME layer](img/game.svg)
```
        G    Q    W    E    R          ·    ·    ·    ·    ·
        C    A    S    D    F          ·    ·    ·    ·    ·
 CTL   SFT   1    2    3    4          ·    ·    ·    ·    ·     ·
                 ALT  SPC  ▼G2         ·    ·    ·
```

![GAME2 layer](img/game2.svg)
```
       ESC   ·    ·    ·    ·          ·    ·    ·    ·    ·
        ·    ·    ·    ·    ·          ·    ·    ·    ·    ·
 TAB    ·    5    6    7    8          ·    ·    ·    ·    ·     ·
                  ·    ·   ▼G2         ·    ·    ·
```

Left-hand cluster with jump on the resting thumb; the right hand is entirely `&none` because it's on the mouse. GAME2 supplies ESC, TAB and weapon slots 5–8.

## Combos

![Combos](img/combos.svg)

| Combo | Positions | BASE | PROM | Binding | Scope | Guard |
|---|---|---|---|---|---|---|
| `combo_esc` | 27 + 28 | M+`,` | `.`+`'` | `ESC` | everywhere | 125 ms idle |
| `combo_tab` | 18 + 29 | L+`.` | I+Å | `TAB` | everywhere | 125 ms idle |
| `combo_caps` | 13 + 16 | F+J | H+A | Caps Word | everywhere | 125 ms idle |
| `combo_prom` | 0 + 9 | Q+P | F+B | `&tog PROM` | everywhere | 125 ms idle |
| `combo_game` | 20 + 31 | Å+Æ | Z+Q | `&tog GAME` | everywhere | 125 ms idle |
| `combo_boot` | 23 + 24 | — | — | `&bootloader` | **FUN only** | on `&none` keys |
| `combo_reset` | 26 + 30 | — | — | `&sys_reset` | **FUN only** | on `&none` keys |

All use `timeout-ms = <50>`, and all seven position pairs are disjoint.

**Why ESC and TAB are on the right hand.** They used to sit on positions 0+1 and 1+2 — which in GAME are **G, Q, W**, so starting a strafe-forward could fire TAB (scoreboard) or ESC (pause menu). Every always-live combo now requires at least one right-hand key, and GAME's entire right hand is `&none` because that hand is on the mouse. **No combo can fire while gaming** — a structural guarantee rather than a timing one.

The pairs were chosen by checking every candidate against both layers: since the 125 ms idle guard already suppresses combos mid-word, the only real risk is a *word-initial* pair. `M,`/`.'` and `L.`/`Iå` are word-initial in neither language on neither layer. Rejected for exactly this reason: 17+18 (`EI` on PROM — the Nynorsk diphthong), 7+17 (`IK` — *ikkje*), 18+19 (`Lø` — *løn*, *løysing*). `combo_esc` is index+middle on the bottom row; `combo_tab` is a one-finger vertical on the ring column.

**On `layers` lists:** ZMK ORs the list against active layers and layer 0 is always active, so any list containing BASE is a no-op. The always-live combos therefore carry no list at all — it would only be misleading. `layers = <FUN>` on the two destructive combos *is* a real restriction, because FUN is not the default layer.

**Caps Word** ends at space and survives å/ø/æ, digits, `-`, `_` and backspace. Known limitation: **å, ø and æ pass through uncapitalised** — ZMK only applies shift to A–Z keycodes, and these are non-alpha scancodes. So `MÅL` comes out `MåL`. Workaround: hold the homerow Shift for those three letters while Caps Word is active.

## Modifier inventory

| Modifier | Access | Coverage |
|---|---|---|
| Ctrl | **LCTL bare** (left inner thumb) · RCTL hold on RET | cross-hand both directions |
| Alt / Meta | **LALT bare** (left outer thumb) | all letters; `C-M-` = RCTL+LALT (two thumbs) |
| Shift | homerow F/J (BASE), H/A (PROM) · **left thumb on NAV/SYM/FUN** · Caps Word | everywhere |
| GUI | hold on Ø (BASE) / C (PROM) · **right pinky home on NAV** | right-sided; `Super+digit` cross-hand |

## Behaviors

**Bare thumb mods (`&kp LCTRL`, `&kp LALT`)** — no configuration, no misfire class. The deliberate rule: modifiers that chord get real keys; only things that never occur mid-roll get hold-taps.

**`hsl` / `hsr` — shift-only homerow mods.** Positional hold-taps: the hold only registers when the next key is on the *other* half (or a thumb), so same-hand rolls (*ei*, *st*, *nt* on PROM) physically cannot shift. `require-prior-idle-ms = <85>` is deliberately more lenient than a ctrl/alt guard would be, because capitals happen mid-flow; `quick-tap-ms = <100>` keeps doubles (*tt*, *nn*) clean. **`hold-trigger-on-release` was removed** — its purpose is stacking multiple same-hand homerow mods, and with ctrl/alt on the thumbs there is exactly one homerow mod per hand, so it only delayed shift.

**`mtt` — RET tap / RCTL hold.** `balanced`, with a 100 ms prior-idle so a RET typed in flow is always a tap.

**`&lt` — layer-taps on BSP and SPC.** `balanced`, `quick-tap-ms = <175>`: tap-then-hold repeats backspace/space instead of opening the layer. Both held raises FUN after ~200 ms.

**`&mt` — GUI on Ø/C.** `tap-preferred`; a window-manager mod on a letter wants the letter to win every race.

**Macros `m_grave` / `m_tilde` / `m_caret`** — dead key plus space, so `` ` ``, `~` and `^` emit real characters.

## Toggles

| Layer | In/out | Where | Note |
|---|---|---|---|
| PROM | `&tog` combo | top corners (0+9) | idle-guarded; numbered below NAV/SYM |
| GAME | `&tog` combo | outer keys (20+31) | idle-guarded; numbered above NAV/SYM |
| GAME2 | `&mo` hold | GAME inner thumb | momentary |
| FUN | conditional **or** `&mo` | both right thumbs, or NAV position 6 | momentary; unreachable from GAME |

## Known trade-offs

- **NAV/SYM are held by the right thumb while their content is on the right hand** (arrows, brackets). Cross-hand would be more comfortable, but the left thumb is fully spent on bare modifiers, and this matches the hold position already in muscle memory.
- **Thumb-R is unproven for sustained use.** The BASE duplicate exists so it can be trialled for weeks — long enough for strain to appear — without committing to Promethium.
- **J↔W is the one alpha deviation with an unmeasured net cost.** Run your own corpus through an analyzer before learning it.

## Regenerating the images

`python3 gen.py` rewrites everything in `img/`. The plain-text blocks above are the low-maintenance source of truth — edit those alongside the keymap, and rerun the generator when you want the images refreshed.
