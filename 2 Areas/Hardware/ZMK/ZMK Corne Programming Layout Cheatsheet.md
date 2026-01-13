## Base Layer

### Special Keys
- **Caps Lock position**: Tap for `ESC` | Hold for `CTRL`
- **Left Space**: Tap for `SPACE` | Hold for `NavFn` layer
- **Right Space**: Tap for `SPACE` | Hold for `Sym` layer
- **Middle thumb (left)**: Hold for `Num` layer

### Key Combos

#### Brackets & Braces
- `D + F` → `[`
- `J + K` → `]`
- `S + D` → `{`
- `K + L` → `}`
- `E + R` → `(`
- `U + I` → `)`

#### Clipboard Operations
- `Z + X` → Cut (Ctrl+X)
- `Z + C` → Undo (Ctrl+Z)
- `X + C` → Copy (Ctrl+C)
- `C + V` → Paste (Ctrl+V)
- `C + B` → Redo (Ctrl+Shift+Z)

#### OtherA
- `Q + W` → `` ` `` (backtick for template literals)

#### Layer Toggle
- `NUM + ENT` (middle left thumb + right thumb) → Toggle Num layer on/off

---

## NavFn Layer (Hold Left Space)

### Layout
```
|  TAB |  1  |  2  |  3  |  4  |  5  |   |  6  |  7  |  8  |  9  |  0  | BKSP |
| CTRL | F1  | F2  | F3  | F4  | F5  |   | ←   | ↓   | ↑   | →   |     | DEL  |
| SHFT | F6  | F7  | F8  | F9  | F10 |   |HOME |PGDN |PGUP | END | F11 | F12  |
```

### Key Features
- **Numbers**: Top row (1-0)
- **Function Keys**: F1-F12 on left side and far right
- **Arrow Keys**: Vim-style HJKL position (←↓↑→)
- **Navigation**: Home, End, Page Up, Page Down
- **Delete**: Top-right position

---

## Sym Layer (Hold Right Space)

### Layout
```
|  TAB |  !  |  @  |  #  |  $  |  %  |   |  ^  |  &  |  *  |  (  |  )  | BKSP |
| CTRL | BT1 | BT2 | BT3 | BT4 | BT5 |   |  -  |  =  |  [  |  ]  |  \  |  `   |
| SHFT |BTCLR|     |     |     |     |   |  _  |  +  |  {  |  }  |  |  |  ~   |
```

### Key Features
- **Symbols**: `! @ # $ % ^ & * ( )`
- **Math**: `- = _ +`
- **Brackets**: `[ ] { }`
- **Other**: `` \ ` | ~ ``
- **Bluetooth**: Left side (BT1-5, Clear)

---

## Num Layer (Hold Middle Left Thumb or Toggle with NUM+ENT)

### Layout
```
|  `   |  (  |  )  |  {  |  }  |  &  |   |  /  |  7  |  8  |  9  |  -  | BKSP |
|  [   |  ]  |  $  |  %  |  ^  |  _  |   |  *  |  4  |  5  |  6  |  +  |  =   |
|  <   |  >  |  !  |  @  |  #  |  ?  |   |  0  |  1  |  2  |  3  |  .  |  \   |
```

### Key Features
- **Numpad**: Right side (0-9)
- **Operators**: `+ - * / =`
- **Symbols**: Left side has brackets, special chars
- **Toggle**: Press `NUM + ENT` thumbs together to lock/unlock layer

---

## Quick Reference

### Most Used Combos (No Layer Switch)
| Combo | Result | Use Case |
|-------|--------|----------|
| `Q + W` | `` ` `` | Template literals |
| `D + F` | `[` | Arrays |
| `J + K` | `]` | Arrays |
| `S + D` | `{` | Objects/blocks |
| `K + L` | `}` | Objects/blocks |
| `E + R` | `(` | Function calls |
| `U + I` | `)` | Function calls |
| `X + C` | Copy | Clipboard |
| `C + V` | Paste | Clipboard |
| `Z + C` | Undo | Edit |
| `C + B` | Redo | Edit |

### Common Programming Patterns
- **Arrow function**: `E+R` (space) `U+I` `=` `>` (space) `S+D` 
  - Result: `() => {`
- **Template literal**: `Q+W` `$` `S+D` `K+L` `Q+W`
  - Result: `` `${}` ``
- **Array destructuring**: `D+F` `K+L` `=`
  - Result: `[] =`

### Tips
- `\` key: Tap for `\`, Shift for `|`
- Backtick: Tap for `` ` ``, Shift for `~`
- ESC always available: Tap Caps Lock position (no hold needed)
- Two space bars: Use whichever hand is free