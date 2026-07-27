# TOTEM — Norwegian keymap

ZMK keymap for the 38-key TOTEM, built for Norwegian (Nynorsk) and English with heavy Emacs use. QWERTY base, a togglable Hands Down Promethium layer tuned for Norwegian, held NAV/SYM layers, a momentary FUN layer, and a GAME layer with its own thumbs.

Design principles: **bare modifiers on the left thumb** (Emacs chording with no hold-tap timing), **shift-only homerow mods**, **æ/ø/å as real keys** on both writing layers, and one thumb scheme shared by every typing layer.

**Assumes the OS keyboard layout is standard Norwegian (`no`).** Everything below is chosen against that layout's scancodes, verified against the shift-state tables in `KBDNO.DLL` (kbdlayout.info/KBDNO).

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
   LALT  │   R   │  LCTL       SPC   │  BSP   │  RET
   bare  │  bare │  bare       ▸NAV  │  ▸SYM  │ ⌃RCTL-hold
```

- **LCTL and LALT are bare keys** — no tap side, no timing semantics, no misfire class. Chained Emacs sequences (`C-x C-s`, `C-c C-c`, held `C-n`) are one held thumb plus finger taps.
- **R on the resting left thumb** — stock Promethium's thumb and hand (opposite the vowels, so *er/ar/or/re* alternate). On BASE it duplicates the alpha R: a live trial of thumb-R at zero cost. Bare, because holding a letter has no use. R cannot move: 32 finger keys minus 29 letters minus `, . ' -` leaves exactly nothing.
- **SPC on the inner right thumb carrying NAV; BSP middle carrying SYM.** The inner key is the most comfortable position for this hand, and space is the single most frequent keystroke in any text (~17–18%), so it gets the best key. Tap-then-hold within 175 ms repeats the key instead of opening the layer. These are never held together — see FUN below.
- **RET outer with RCTL on hold** — the only hold-tap modifier besides the dead keys, safe because RET never occurs mid-roll. It gives Ctrl cross-hand from both sides and makes `C-M-` chords pressable (RCTL + LALT is two thumbs). The outer key is the least comfortable of the three, which is correct for the lowest-frequency thumb function.
- **NAV and SYM re-add SHIFT on the left thumb** (position 33). Without it there is no Shift on those layers at all — Shift lives on the home row, which NAV/SYM overwrite — so Shift+arrow, Shift+Home/End and Shift+F-key would be impossible. With it the left thumb becomes a complete **ALT · SHIFT · CTRL** cluster while the right hand works the arrows.

The ergonomic basis for "inner key gets the heaviest load" is the thumb's carpometacarpal flexion–extension arc: thumb keys are comfortable to the extent they sit on that arc without requiring lateral abduction or palm movement (MoErgo's Glove80 design notes are the clearest public writeup). Which of three keys lands on your arc depends on hand size versus cluster geometry, so the empirical report for *this* hand on *this* board is the governing evidence, not a general rule.

## Layer stack

![Layer stack](img/stack.svg)

ZMK resolves each keypress from the highest-numbered **active** layer downward; `&trans` only falls *down*.

```
6  FUN    momentary — &mo on NAV position 7 (right middle finger)
5  GAME2  momentary (&mo from GAME)
4  GAME   toggle — replaces everything, own thumbs
3  SYM    hold (BSP thumb)
2  NAV    hold (SPC thumb)
1  PROM   toggle — below the held layers, so holds never surface Promethium letters
0  BASE   default, always active
```

PROM sits at 1 so a held NAV/SYM always wins over it. GAME sits above NAV/SYM deliberately: it replaces everything and never needs them. FUN sits on top and hangs off NAV, so it is unreachable while gaming. FUN's thumbs are all `&trans` so the NAV hold and the thumb mods stay alive underneath.

## Layers

### BASE (0)

![BASE layer](img/base.svg)
```
        Q    W    E    R    T          Y    U    I    O    P
        A    S    D    F    G          H    J    K    L    Ø
                      sft                   sft            gui
  Å     Z    X    C    V    B          N    M    ,    .    -     Æ
               LALT   R   LCTL        SPC  BSP  RET
                                      nav  sym  ctl
```

QWERTY with shift-only homerow mods (F/J) and GUI on Ø. Norwegian letters are real keys: **å** left outer, **æ** right outer, **ø** on the right pinky home — its standard national position. The thumb **R** duplicates the alpha R.

Note that `'` is **not** on BASE — it lives on SYM. This is a QWERTY-phase cost only; on PROM it is a base key.

### PROM (1)

![PROM layer](img/prom.svg)
```
        F    P    D    L    X          Æ    U    O    Y    B
        S    N    T    H    K          ,    A    E    I    C
                      sft                   sft            gui
  Z     V    J    G    M    W          -    .    '    Å    Ø     Q
               LALT   R   LCTL        SPC  BSP  RET
                                      nav  sym  ctl
```

Hands Down Promethium, adhered to as closely as possible; dashed keys in the image deviate from stock:

- **J ↔ W swapped** — stock puts J directly under K on the same index column, making the frequent Norwegian *kj* (ikkje, kjem, kanskje) a same-finger skip. Cost: W lands on the inner bottom, **and Promethium's designed vim HJKL cluster is broken** — stock keeps h/l on the index column and j/k on the inner-index column, which is one of the layout's advertised features. Reversible; the corpus/analyzer run holds the veto.
- **Å, Ø, Æ replace `=`, `/`, `;`** — all three displaced symbols live on SYM; the letters outrank them by an order of magnitude in Norwegian. `;` additionally cannot be `&kp SEMI` here — that scancode *is* ø.
- **Z and Q on the outer keys** — stock leaves them off the main 30, on a second right-hand outer column the TOTEM does not have.

Thumbs are all `&trans`, falling through to BASE — including R, which is simply *the* R here. NAV, SYM and FUN are therefore identical on both writing layers.

**Unresolved (needs the corpus run):** C sits on the right pinky *home* key, a premium position, at roughly 0.04% frequency in Norwegian — it is there because Promethium is English-optimised, where C is ~2.8%. Ø (~0.9%) sits on the pinky bottom row. Rotating the pinky column would help Norwegian and hurt English. Do not touch this without numbers.

### NAV (2)

![NAV layer](img/nav.svg)
```
        6    7    8    9    0         ⌃←   FUN   ·   ⌃→    ·
        1    2    3    4    5          ←    ↓    ↑    →   GUI
  ·     ·    ·    ·    ·    ·        Home PgDn PgUp End  Del     ·
               LALT SHIFT LCTL       ▼NAV  BSP  RET
```

- **SHIFT on the left thumb** (see above) makes Shift+arrow selection work; combined with RCTL on RET, `C-S-<arrow>` (extend by word) is two thumbs plus a finger.
- **GUI on the right pinky home** — the digits live on this layer, so without it `Super+1…9` for MangoWC workspaces would need a 220 ms mod-tap hold on Ø first. Cross-hand from the digit grid.
- **FUN key at position 7** — hold SPC with the right thumb, then hold the FUN key with the right **middle finger**. Thumb plus finger on one hand is a natural grip, and it leaves the whole left hand free for the F-keys.
- Blank keys are `&none`, so no BASE or PROM letter leaks through.

### SYM (3)

![SYM layer](img/sym.svg)
```
        &    /    (    )    =          `    ~    <    >    ^
        !    "    #    $    %          {    [    ]    }    |
  ´     ¨    @    £    ?    €          \    *    +    -    _     '
               LALT SHIFT LCTL        SPC ▼SYM  RET
```

**SYM is base-layer-independent by design.** It is identical whether QWERTY or PROM is active, because a layer is where muscle memory lives: you learn SYM once, it survives the QWERTY→PROM switch, and it is the part of this keymap that ports unchanged to a Svalboard. See *Known trade-offs* for why the alternative was rejected.

The left half is a **three-level map of the number row**, column by column: NAV gives the digit, SYM's home row gives Shift+digit, SYM's bottom row gives AltGr+digit in the *same column*.

| Column | NAV | SYM home | SYM bottom |
|---|---|---|---|
| ring | 2 / 7 | `"` | `@` (AltGr+2) |
| middle | 3 / 8 | `#` | `£` (AltGr+3) |
| index | 4 / 9 | **`$`** (AltGr+4) | `?` |
| inner | 5 / 0 | `%` | `€` (AltGr+5) |

**Currencies.** All three are AltGr+digit on the Norwegian layout (`3#£ 4¤$ 5%€`), so they fit without a second symbol layer. `$` is prioritised as requested: it takes the **index home** slot, displacing `¤` — the generic currency sign nobody types, still reachable as thumb-Shift+4 on NAV. `£` and `€` sit in their own digit columns on the row below, so the mnemonic is "same column, one row down". `?` takes the freed index-bottom slot since it's the more frequent character.

**What SYM does *not* need to carry.** `;` `:` `_` `*` are Shift + a PROM base key (`,` `.` `-` `'`) and the OS produces them for free. They are not duplicated here except where the position was otherwise idle. This is why SYM makes no attempt to mirror base-layer punctuation.

**Dead keys.** `` ` ``, `~` and `^` are *dead keys* on the Norwegian layout: pressed alone they emit nothing until a following character, which silently breaks code fences, `~/` and regex carets. They are **hold-taps**: tapping sends dead-key-then-space, which commits the bare character (the space is consumed, so ``` ``` ``` still produces three backticks); holding sends the raw dead key, so `^`+`o` still gives `ô` for *fôr* and `` ` ``+`e` gives `è`. This is the portable fix that needs no OS-side `nodeadkeys` variant, important on a locked-down work machine.

`´` (AltGr+`\`) and `¨` (the key right of Å) are left as **raw** dead keys, on the left hand — opposite every PROM vowel, so `é` in *idé*/*kafé* and `ö`/`ü` in foreign names are cross-hand rolls. Their bare forms are not reachable, which is fine; nobody types a lone acute.

Bluetooth lives on FUN.

### FUN (6)

![FUN layer](img/fun.svg)
```
       F6   F7   F8   F9   F10       PREV VOL-  --  VOL+ NEXT
       F1   F2   F3   F4   F5        MUTE PLAY  --  BTclr OUT
  ·     ·   F11  F12   ·    ·        BT0  BT1   --  BT2   ·     ·
               LALT SHIFT LCTL       ▼NAV  BSP  RET
```

**Reached with one hand: hold SPC with the right thumb, then hold the FUN key (NAV position 7) with the right middle finger.** There is no NAV+SYM conditional layer — asking one thumb to cover two adjacent keys is a bad gesture, and it also meant a fast BSP↔SPC roll could blink FUN on by accident. Both problems are gone.

The consequence is the `--` column: while FUN is up, the right middle finger is holding the key, so positions 7, 17 and 28 are unreachable and deliberately left `&none`. The middle finger is the right choice precisely because it blocks only its own column — the index finger also covers the inner-stretch column (5/15/26), which would have cost six positions instead of three, more than the right hand can spare.

F-keys keep the same left-hand grid as NAV's digits — one spatial map, three meanings: digit (NAV), shifted digit (SYM), F-key (FUN). The left hand is entirely free, so `Alt+F4`, `Ctrl+F2` and `Shift+F5` all work with the left thumb mods falling through.

`&bootloader` and `&sys_reset` are **not bare keys** — they are combos (23+24 and 30+31), restricted to `layers = <FUN>`, and all four positions are `&none` here so a single press does nothing. Reset is on the right pinky edge; boot is on the left index pair, reachable by the otherwise-free left hand.

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
| `combo_reset` | 30 + 31 | — | — | `&sys_reset` | **FUN only** | on `&none` keys |

All use `timeout-ms = <50>`. The five always-live pairs are mutually disjoint; `combo_reset` shares position 31 with `combo_game`, which is harmless because ZMK matches complete key sets — pressing 30+31 outside FUN fires nothing and simply types both keys.

`combo_game` (20 + 31) is also live on SYM, where those positions now hold `´` and `'` rather than `&none`. Both hands and both outer edges are required, so an accidental pair is not a realistic risk, but it is no longer a structurally impossible one.

**Why ESC and TAB are on the right hand.** They used to sit on positions 0+1 and 1+2 — which in GAME are **G, Q, W**, so starting a strafe-forward could fire TAB (scoreboard) or ESC (pause menu). Every always-live combo now requires at least one right-hand key, and GAME's entire right hand is `&none` because that hand is on the mouse. **No combo can fire while gaming** — a structural guarantee rather than a timing one.

The pairs were chosen by checking every candidate against both layers: since the 125 ms idle guard already suppresses combos mid-word, the only real risk is a *word-initial* pair. `M,`/`.'` and `L.`/`Iå` are word-initial in neither language on neither layer. Rejected for exactly this reason: 17+18 (`EI` on PROM — the Nynorsk diphthong), 7+17 (`IK` — *ikkje*), 18+19 (`Lø` — *løn*, *løysing*). `combo_esc` is index+middle on the bottom row; `combo_tab` is a one-finger vertical on the ring column.

**On `layers` lists:** ZMK ORs the list against active layers and layer 0 is always active, so any list containing BASE is a no-op. The always-live combos therefore carry no list at all — it would only be misleading. `layers = <FUN>` on the two destructive combos *is* a real restriction, because FUN is not the default layer.

**Caps Word** ends at space and survives å/ø/æ, digits, `-`, `_` and backspace. Known limitation: **å, ø and æ pass through uncapitalised** — ZMK's documentation states the modifiers are applied only to the alphabetic A–Z keycodes, and these are non-alpha scancodes. So `MÅL` comes out `MåL`. This is a firmware constraint with no config-level fix. Workaround: hold the homerow Shift for those three letters while Caps Word is active.

## Modifier inventory

| Modifier | Access | Coverage |
|---|---|---|
| Ctrl | **LCTL bare** (left inner thumb) · RCTL hold on RET | cross-hand both directions |
| Alt / Meta | **LALT bare** (left outer thumb) | all letters; `C-M-` = RCTL+LALT (two thumbs) |
| Shift | homerow F/J (BASE), H/A (PROM) · **left thumb on NAV/SYM/FUN** · Caps Word | everywhere |
| GUI | hold on Ø (BASE) / C (PROM) · **right pinky home on NAV** | right-sided; `Super+digit` cross-hand |

There is deliberately **no sticky shift**. It would help — shifting alphas is the one case where `require-prior-idle-ms` can produce a false negative at speed — but every thumb key is spent, and a combo is the wrong host: the 125 ms idle guard on the always-live combos would suppress it in exactly the mid-flow situation where the false negative occurs. The fix if capitals start dropping is to lower `require-prior-idle-ms` on `hsl`/`hsr` (85 → 50 → 0), not to add a behaviour.

## Behaviors

**Bare thumb mods (`&kp LCTRL`, `&kp LALT`)** — no configuration, no misfire class. The deliberate rule: modifiers that chord get real keys; only things that never occur mid-roll get hold-taps.

**`hsl` / `hsr` — shift-only homerow mods.** Positional hold-taps: the hold only registers when the next key is on the *other* half (or a thumb), so same-hand rolls (*ei*, *st*, *nt* on PROM) physically cannot shift. `require-prior-idle-ms = <85>` is deliberately more lenient than a ctrl/alt guard would be, because capitals happen mid-flow; `quick-tap-ms = <100>` keeps doubles (*tt*, *nn*) clean. **`hold-trigger-on-release` was removed** — its purpose is stacking multiple same-hand homerow mods, and with ctrl/alt on the thumbs there is exactly one homerow mod per hand, so it only delayed shift.

**`mtt` — RET tap / RCTL hold.** `balanced`, with a 100 ms prior-idle so a RET typed in flow is always a tap.

**`dk` — dead-key hold-tap.** `tap-preferred` with a 250 ms term: tap gives the bare character via `m_dead`, hold gives the raw dead key. `tap-preferred` because the bare form is the common case and an interrupting keypress should resolve as a tap. `quick-tap-ms = <0>` — there is no repeat use for these.

**`m_dead` — parameterised macro.** One `zmk,behavior-macro-one-param` replaces the previous three fixed macros: it taps whatever keycode it is handed, then space. Used only as the tap side of `dk`.

**`&lt` — layer-taps on SPC and BSP.** `balanced`, `quick-tap-ms = <175>`: tap-then-hold repeats space/backspace instead of opening the layer.

**`&mt` — GUI on Ø/C.** `tap-preferred`; a window-manager mod on a letter wants the letter to win every race.

## Toggles

| Layer | In/out | Where | Note |
|---|---|---|---|
| PROM | `&tog` combo | top corners (0+9) | idle-guarded; numbered below NAV/SYM |
| GAME | `&tog` combo | outer keys (20+31) | idle-guarded; numbered above NAV/SYM |
| GAME2 | `&mo` hold | GAME inner thumb | momentary |
| FUN | `&mo` hold | NAV position 7 (right middle finger) | momentary; unreachable from GAME |

## Known trade-offs

- **One SYM layer cannot mirror two base layers.** QWERTY puts `, . -` at positions 28/29/30; PROM puts them at 15/26/27. A single layer physically cannot be the shifted twin of both. Two symbol layers via `conditional-layers` (`if-layers = <PROM SYM>`) would work, at the cost of two maps to learn, two to maintain and two to port to the Svalboard. Rejected. The shifted-twin relationship is delivered by Shift on the base layer instead, which already works correctly on both.
- **NAV/SYM are held by the right thumb while some of their content is on the right hand** (arrows, brackets). The digits are cross-hand, which is the half that matters most. The left thumb is fully spent on bare modifiers, so there is nowhere else to put the holds.
- **FUN costs the right middle column.** Three positions (7, 17, 28) are unusable while the layer is held. That is the price of one-handed activation, and it is the cheapest column available.
- **Thumb-R is unproven for sustained use.** The BASE duplicate exists so it can be trialled for weeks — long enough for strain to appear — without committing to Promethium.
- **J↔W is the one alpha deviation with an unmeasured net cost,** and it now has a known one too: it breaks Promethium's vim HJKL cluster. Run your own corpus through an analyzer before learning it.
- **`m_dead` as the tap side of a hold-tap is the least-proven construct in this file.** Parameterised macros inside hold-taps have had edge-case bugs reported upstream (zmkfirmware/zmk#2031), though those involved `macro_pause_for_release`, which `m_dead` does not use. Smoke-test `` ` ~ ^ `` first after flashing. Fallback if it misbehaves: drop `dk`, restore three plain `zmk,behavior-macro` nodes and bind them directly, losing only the raw-dead-key hold.

## Verification

Nothing in the alpha layout should be changed on argument alone. The open questions — C on the right pinky home, J↔W, the pinky column order — are three-key rotations, not a layout search, and need numbers from a corpus that reflects actual use.

- **Alphas:** build a corpus from `notat/` (nn + nb), English technical prose, and Nix/YAML/shell configs, weighted roughly 50/30/20 to match priority order. Run it through Oxeylyzer (o2.oxey.dev, or the REPL for custom corpus rules — æ/ø/å must be declared as first-class letters, not folded away). Compare SFB, skipgram SFB, lateral stretch, scissors and finger load across the candidate variants.
- **Symbols:** run Getreuer's `count_chars.py` (`getreuer/qmk-keymap`, `tools/`) over the same files. Org-mode markup, not source code, is the dominant symbol corpus here — `* [ ] - #+ ~ = |` — which is why SYM's right hand is arranged the way it is rather than around `{}` and `;`.
- **Thumbs:** no analyzer helps. Live with a change for two weeks and attend to the base of the thumb rather than to speed.

## Regenerating the images

`python3 gen.py` rewrites everything in `img/`. The plain-text blocks above are the low-maintenance source of truth — edit those alongside the keymap, and rerun the generator when you want the images refreshed.
