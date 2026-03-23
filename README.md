This 34-key keymap was heavily inspired by @Callum's [QMK keymap](https://github.com/qmk/qmk_firmware/blob/master/users/callum/readme.md) and [urob's ZMK keymap](https://github.com/urob/zmk-config), originally created to work on the [Ferris Sweep](https://github.com/davidphilipbarr/Sweep), now ported to the [Urchin](https://github.com/duckyb/urchin).

# My use case and layer design choices

Its main use is writing prose, in both English and Portuguese, using macOS. I'm a lawyer and a professor who likes to pretend I can code as well. It also includes dedicated layers for gaming (Age of Empires 2) with one-handed controls.

I use it on a [Ferris Sweep](https://github.com/ldebritto/zmk-sweep) with [nice!nanos v2](https://nicekeyboards.com/nice-nano) on a Totem (I simply don't use the extra 4 keys there, here's that [repo](https://github.com/ldebritto/zmk-config-totem)) and now on this Urchin. I like it so much that even created a 33-key version for this keymap on kanata (here's that [code](https://gist.github.com/ldebritto/44574eac93148f387aaa560ccb131903)) for the times I'm rocking the built-in MacBook keyboard.

I find it particularly great to type on when paired with very light and silent switches, such as [LowProKB.ca's Ambients twilight and nocturnals](https://lowprokb.ca/products/ambients-silent-choc-switches).

Here's what's currently implemented:

## 1. Default layer, thumbs, and entry points to other layers

- QWERTY base with `'` and `;` swapped (`'` lives on the RCTRL mod-tap and `;` is on the bottom row). `/` moved to `SYM`.
- Home row mods follow urob's balanced HRM setup, with Hyper on `G`/`H`. Shift (`D`/`K`) uses a variant without `require-prior-idle` for more responsiveness.
- The left thumb is `&lc NAV`, the right thumb is `&lc SYM`, and there's a dedicated left-thumb `RSHFT`. `C` and `V` are layer-taps into `NUM` and `MOU` respectively while still tapping as `C`/`V`.
- `Z` has `GLOBE` as a mod-tap.

## 2. Sticky mods with cancel-on-layer-change macros

- Sticky keys (`&sk`) use a one-day release-after time with quick-release to mirror Callum's queue-friendly behavior.
- Layer changes use the `&lc`/`&tc` macros that send a `K_CANCEL` tap when entering a layer, so any queued sticky mods are cleared on entry but still carry on when leaving. See [config/features/layer_cancel_macros.dtsi](config/features/layer_cancel_macros.dtsi).

## 3. `SYM` layer tuned for BR-PT diacritics

- `^`, `` ` ``, and `~` sit on home row for comfortable accented vowels.
- Braces/parentheses are mirrored (open on the left, close on the right), and both slashes are mirrored too.
- Shifted prose punctuation (`"` and `:`) is here for single-hand access with `&lc SYM`.
- Common Markdown symbols (`#`, `*`, `|`) stay near home.
- The left thumb taps `_` (underscore); right thumb does `&sl SYM` (sticky SYM) for one-handed symbol entry.

## 4. `NUM` layer for right-hand numpad work and `&num_word`

- Accessible by holding `C` (`&ltl NUM C`) or via the `&num_word NUM` combo (`RM2 + RIT`) which auto-deactivates on `SPACE` or other keys not on the continue-list.
- Right-hand numpad layout with math keys; the left half carries alt-modified number keys and a `reais` macro for `R$`.
- `num_spc` taps `SPACE` with embedded `K_CANCEL` to exit `&num_word` while keeping `NUM` alive if toggled. Holding the `0` key (`&lt SYM N0`) temporarily reaches `SYM` while on `NUM`.
- `SPC` is on the continue-list so the `reais` macro (which appends a space) doesn't break `&num_word`; `num_spc` is the workaround to actually exit it.
- A `num_spc` (tap `SPACE`) includes a `K_CANCEL` before the space to exit `&num_word` while keeping `NUM` alive if toggled via `&tc NUM`.

## 5. `NAV` and `FUN` layers, plus `&swapper`

- `NAV` (left thumb) keeps arrows, paging, home/end, delete/backspace, and media controls (`playnp` tap dance plus volume/brightness morphs on `CTRL`).
- `&swapper` on `TAB` replicates macOS `CMD+TAB`, ignoring arrow/edit keys so focus stays on switching.
- `F18` and `F19` live here for macOS automation (Homerow trigger and Keyboard Maestro macro trigger).
- `&lt MOU LG(V)` gives quick access to the mouse layer while holding paste.
- `FUN` is a tri-layer that activates when `NAV` and `SYM` are held together. It carries the number row, function keys, and power/caps controls.

## 6. Mouse layer

- Enabled via ZMK pointing; movement and scroll are tuned for 4K displays (`config/features/mouse.dtsi`).
- Enter by holding `V` (`&ltl MOU V`) or via the `mouse_on_thumbs` combo (`LIT + LHT`). `&to DEF` lives on the bottom-right if the layer is toggled.
- Includes scroll directions, pointer movement, MB4/MB5, left/right click on thumbs, and standard edit shortcuts (`LG(Z/X/C/V)`).
- `playnp` and `F18` are mirrored from `NAV` for convenience.

## 7. Combos for one-handed use and shortcuts

- Both halves offer combos for `ESC`, `TAB`, `ENTER`, and `BACKSPACE` so each hand can edit alone. `NAV` and `MOU` inherit the left-hand set; `NUM` inherits the right-hand set.
- `LIT + LHT` activates `MOU`; `RM2 + RIT` activates `&num_word NUM`; `LM2 + RM2` toggles `caps_word`.
- `LB1 + LB2 + LB3` on `DEF`/`NAV`/`MOU` taps `SPACE` (useful for one-handed use).
- A four-finger right-hand chord (`RT1 RT2 RT3 RT4`) on `DEF` sends `C_POWER` to lock/sleep the host OS.
- On `FUN`, combos handle Bluetooth profile selection (0–2), bootloader, and clearing the active BT profile. A left-hand four-finger chord forces BLE output.
- `LB1 LB2 LB3 LB4` toggles the `AOE` gaming layer from `DEF` or `AOE`.

## 8. Age of Empires 2 layers

- Three dedicated layers (`AOE`, `AGS`, `ABS`) for gaming, accessible via combo from the default layer (`LB1 LB2 LB3 LB4`).
- `AOE` provides a left-hand gaming layout with standard QWERTY positioning and mod-taps optimized for RTS gameplay.
- `AGS` (Army Group Select) gives quick number access (1–0) for control groups, plus volume/brightness and media controls. Activated by holding `H` on `AOE`.
- `ABS` (Army Build Select) provides `Ctrl+Shift+[letter]` hotkeys for unit production, accessible by holding `B` or `.` on `AOE`.
- `ESC`, `TAB`, `ENTER`, and `BACKSPACE` combos are replicated on `AOE` for full left-hand usability.

## 9. ZMK modules and firmware

- ZMK locked to **v0.3.0** as referenced in [config/west.yml](config/west.yml).
- [zmk-tri-state](https://github.com/urob/zmk-tri-state) by urob (used by `&swapper`).
- [zmk-auto-layer](https://github.com/urob/zmk-auto-layer) by urob for `&num_word`.
- [urchin-zmk-module](https://github.com/duckyb/urchin-zmk-module) by kyek/duckyb for Urchin hardware support.
- Mouse support enabled via `CONFIG_ZMK_POINTING=y` in [config/urchin.conf](config/urchin.conf).
- Battery level reporting enabled (`CONFIG_ZMK_BATTERY_REPORTING=y`) with split BLE battery proxy for the peripheral half.