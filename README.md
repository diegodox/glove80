# Custom Glove80 Keymap - JIS OS with US Layout

Custom ZMK firmware for Glove80 keyboard, designed for typing on a **JIS (Japanese) OS using US physical keycaps**.

## Key Features

### 🇯🇵 JIS OS Mapping with US Keycaps
Physical US layout mapped to produce correct JIS OS output using custom mod-morph behaviors in `config/behaviors/us_like_jis.dtsi`.

**Example mappings:**
- `Shift + 2` → `@` (JIS sends `LBKT` instead of `LS(N2)`)
- `Shift + 6` → `^` (JIS sends `EQUAL` instead of `LS(N6)`)
- `Shift + 7` → `&` (JIS sends `LS(N6)` instead of `LS(N7)`)
- `[` key → sends `RBKT` (JIS bracket position)
- `:` key → sends `SQT` when shifted (JIS colon)

### ⌨️ Home Row Mods
- `A` / `S` / `L` / `;` = Ctrl/Shift/Shift/Ctrl when held

### 🎯 Combos
- `D + F` → Tab
- `Muhenkan + Henkan` → Escape

### 📊 Thumb-Key Layers
- **Hold Muhenkan** → NUMPAD layer
  - Home row becomes numbers: `ASDFG` = `12345`, `ZXCVB` = `67890`
  - Top row becomes symbols: `QWERTYUIOP` = `!@#$%^&*()`

- **Hold Henkan** → SYMBOLS layer (all shifted symbols accessible)

## Editing & Building

### Visual Editor
Edit your keymap visually: [Keymap Editor](https://nickcoutsos.github.io/keymap-editor/)

### Manual Editing
- `config/glove80.keymap` - Main keymap with layers and combos
- `config/behaviors/us_like_jis.dtsi` - JIS OS symbol mappings

### Build
```bash
./build.sh  # Outputs glove80.uf2
```

Or use GitHub Actions (see Actions tab for artifacts).

---

Based on [official Glove80 ZMK config](https://github.com/moergo-sc/glove80-zmk-config). See [Official Support](https://moergo.com/glove80-support) for flashing instructions.
