# Constants Reference

This reference defines constants for signal classification and indicator visualization.

## Table of Contents

- [Signal Codes](#signal-codes)
- [Indicator Styles](#indicator-styles)
- [Indicator Colors](#indicator-colors)

---

## <a id="signal-codes"></a> Signal Codes

Signal codes are numeric values used to classify trading actions within a strategy.

### Entry Signal Codes

| Code | Description          |
| ---- | -------------------- |
| 100  | Long \| Market       |
| 200  | Short \| Market      |
| 300  | Long \| Limit        |
| 400  | Short \| Limit       |
| 500  | Long \| Stop-Market  |
| 600  | Short \| Stop-Market |
| 700  | Long \| Stop-Limit   |
| 800  | Short \| Stop-Limit  |

### Close Signal Codes

| Code | Description                |
| ---- | -------------------------- |
| 100  | Close Long \| Market       |
| 200  | Close Short \| Market      |
| 300  | Close Long \| Take-Profit  |
| 400  | Close Short \| Take-Profit |
| 500  | Close Long \| Stop-Loss    |
| 600  | Close Short \| Stop-Loss   |
| 700  | Close Long \| Liquidation  |
| 800  | Close Short \| Liquidation |

---

## <a id="indicator-styles"></a> Indicator Styles

Display settings used in `indicator_options` to control the visual appearance of indicators.

### Chart Types ('type')

| Value       | Description     |
| ----------- | --------------- |
| `line`      | Line chart      |
| `histogram` | Histogram chart |

### Line Styles (`lineStyle`)

| Value | Description   |
| ----- | ------------- |
| 0     | Solid         |
| 1     | Dotted        |
| 2     | Dashed        |
| 3     | Large Dashed  |
| 4     | Sparse Dotted |

### Line Types (`lineType`)

| Value | Description |
| ----- | ----------- |
| 0     | Simple      |
| 1     | With Steps  |
| 2     | Curved      |

### Other Options

- `lineWidth` (int): Optional line thickness (e.g., 1, 2, 3).
- `color` (str): Encoded color (see [Indicator Colors](#indicator-colors)).
- `lineVisible` (bool): Whether the line is visible.
- `pane` (int): Panel number to render the indicator (0 = main, 1+ = subpanels).

---

## <a id="indicator-colors"></a> Indicator Colors

Color constants used in `indicator_options` to control the visual appearance of indicators.

| Name       | RGB            | Hex Code  | Preview                                            |
| ---------- | -------------- | --------- | -------------------------------------------------- |
| CORAL      | (255, 127, 80) | `#FF7F50` | ![](https://singlecolorimage.com/get/FF7F50/60x20) |
| TOMATO     | (255, 99, 71)  | `#FF6347` | ![](https://singlecolorimage.com/get/FF6347/60x20) |
| CHERRY_RED | (242, 54, 69)  | `#F23645` | ![](https://singlecolorimage.com/get/F23645/60x20) |
| CRIMSON    | (220, 20, 60)  | `#DC143C` | ![](https://singlecolorimage.com/get/DC143C/60x20) |
| FIREBRICK  | (178, 34, 34)  | `#B22222` | ![](https://singlecolorimage.com/get/B22222/60x20) |
| RED        | (255, 0, 0)    | `#FF0000` | ![](https://singlecolorimage.com/get/FF0000/60x20) |
| DARK_RED   | (139, 0, 0)    | `#8B0000` | ![](https://singlecolorimage.com/get/8B0000/60x20) |

### Pink Colors

| Name              | RGB             | Hex Code  | Preview                                            |
| ----------------- | --------------- | --------- | -------------------------------------------------- |
| PINK              | (255, 192, 203) | `#FFC0CB` | ![](https://singlecolorimage.com/get/FFC0CB/60x20) |
| HOT_PINK          | (255, 105, 180) | `#FF69B4` | ![](https://singlecolorimage.com/get/FF69B4/60x20) |
| PALE_VIOLET_RED   | (219, 112, 147) | `#DB7093` | ![](https://singlecolorimage.com/get/DB7093/60x20) |
| DEEP_PINK         | (255, 20, 147)  | `#FF1493` | ![](https://singlecolorimage.com/get/FF1493/60x20) |
| MEDIUM_VIOLET_RED | (199, 21, 133)  | `#C71585` | ![](https://singlecolorimage.com/get/C71585/60x20) |

### Orange Colors

| Name        | RGB            | Hex Code  | Preview                                            |
| ----------- | -------------- | --------- | -------------------------------------------------- |
| ORANGE      | (255, 165, 0)  | `#FFA500` | ![](https://singlecolorimage.com/get/FFA500/60x20) |
| DARK_ORANGE | (255, 140, 0)  | `#FF8C00` | ![](https://singlecolorimage.com/get/FF8C00/60x20) |
| CORAL       | (255, 127, 80) | `#FF7F50` | ![](https://singlecolorimage.com/get/FF7F50/60x20) |
| TOMATO      | (255, 99, 71)  | `#FF6347` | ![](https://singlecolorimage.com/get/FF6347/60x20) |
| ORANGE_RED  | (255, 69, 0)   | `#FF4500` | ![](https://singlecolorimage.com/get/FF4500/60x20) |

### Yellow Colors

| Name           | RGB             | Hex Code  | Preview                                            |
| -------------- | --------------- | --------- | -------------------------------------------------- |
| LIGHT_YELLOW   | (255, 255, 224) | `#FFFFE0` | ![](https://singlecolorimage.com/get/FFFFE0/60x20) |
| LEMON_CHIFFON  | (255, 250, 205) | `#FFFACD` | ![](https://singlecolorimage.com/get/FFFACD/60x20) |
| YELLOW         | (255, 255, 0)   | `#FFFF00` | ![](https://singlecolorimage.com/get/FFFF00/60x20) |
| PALE_GOLDENROD | (238, 232, 170) | `#EEE8AA` | ![](https://singlecolorimage.com/get/EEE8AA/60x20) |
| GOLD           | (255, 215, 0)   | `#FFD700` | ![](https://singlecolorimage.com/get/FFD700/60x20) |

### Green Colors

| Name             | RGB            | Hex Code  | Preview                                            |
| ---------------- | -------------- | --------- | -------------------------------------------------- |
| SPRING_GREEN     | (0, 255, 127)  | `#00FF7F` | ![](https://singlecolorimage.com/get/00FF7F/60x20) |
| LIME             | (0, 255, 0)    | `#00FF00` | ![](https://singlecolorimage.com/get/00FF00/60x20) |
| LIME_GREEN       | (50, 205, 50)  | `#32CD32` | ![](https://singlecolorimage.com/get/32CD32/60x20) |
| MEDIUM_SEA_GREEN | (60, 179, 113) | `#3CB371` | ![](https://singlecolorimage.com/get/3CB371/60x20) |
| OLIVE            | (128, 128, 0)  | `#808000` | ![](https://singlecolorimage.com/get/808000/60x20) |
| SEA_GREEN        | (46, 139, 87)  | `#2E8B57` | ![](https://singlecolorimage.com/get/2E8B57/60x20) |
| FOREST_GREEN     | (34, 139, 34)  | `#228B22` | ![](https://singlecolorimage.com/get/228B22/60x20) |
| PEACOCK_GREEN    | (0, 137, 132)  | `#008984` | ![](https://singlecolorimage.com/get/008984/60x20) |
| TEAL             | (0, 128, 128)  | `#008080` | ![](https://singlecolorimage.com/get/008080/60x20) |
| DARK_OLIVE_GREEN | (85, 107, 47)  | `#556B2F` | ![](https://singlecolorimage.com/get/556B2F/60x20) |
| GREEN            | (0, 128, 0)    | `#008000` | ![](https://singlecolorimage.com/get/008000/60x20) |

### Cyan Colors

| Name             | RGB             | Hex Code  | Preview                                            |
| ---------------- | --------------- | --------- | -------------------------------------------------- |
| LIGHT_CYAN       | (224, 255, 255) | `#E0FFFF` | ![](https://singlecolorimage.com/get/E0FFFF/60x20) |
| AQUAMARINE       | (127, 255, 212) | `#7FFFD4` | ![](https://singlecolorimage.com/get/7FFFD4/60x20) |
| PALE_TURQUOISE   | (175, 238, 238) | `#AFEEEE` | ![](https://singlecolorimage.com/get/AFEEEE/60x20) |
| CYAN             | (0, 255, 255)   | `#00FFFF` | ![](https://singlecolorimage.com/get/00FFFF/60x20) |
| AQUA             | (0, 255, 255)   | `#00FFFF` | ![](https://singlecolorimage.com/get/00FFFF/60x20) |
| TURQUOISE        | (64, 224, 208)  | `#40E0D0` | ![](https://singlecolorimage.com/get/40E0D0/60x20) |
| MEDIUM_TURQUOISE | (72, 209, 204)  | `#48D1CC` | ![](https://singlecolorimage.com/get/48D1CC/60x20) |
| DARK_TURQUOISE   | (0, 206, 209)   | `#00CED1` | ![](https://singlecolorimage.com/get/00CED1/60x20) |

### Blue Colors

| Name             | RGB             | Hex Code  | Preview                                            |
| ---------------- | --------------- | --------- | -------------------------------------------------- |
| POWDER_BLUE      | (176, 224, 230) | `#B0E0E6` | ![](https://singlecolorimage.com/get/B0E0E6/60x20) |
| LIGHT_BLUE       | (173, 216, 230) | `#ADD8E6` | ![](https://singlecolorimage.com/get/ADD8E6/60x20) |
| LIGHT_SKY_BLUE   | (135, 206, 250) | `#87CEFA` | ![](https://singlecolorimage.com/get/87CEFA/60x20) |
| LIGHT_STEEL_BLUE | (176, 196, 222) | `#B0C4DE` | ![](https://singlecolorimage.com/get/B0C4DE/60x20) |
| SKY_BLUE         | (135, 206, 235) | `#87CEEB` | ![](https://singlecolorimage.com/get/87CEEB/60x20) |
| CORNFLOWER_BLUE  | (100, 149, 237) | `#6495ED` | ![](https://singlecolorimage.com/get/6495ED/60x20) |
| DEEP_SKY_BLUE    | (0, 191, 255)   | `#00BFFF` | ![](https://singlecolorimage.com/get/00BFFF/60x20) |
| DODGER_BLUE      | (30, 144, 255)  | `#1E90FF` | ![](https://singlecolorimage.com/get/1E90FF/60x20) |
| STEEL_BLUE       | (70, 130, 180)  | `#4682B4` | ![](https://singlecolorimage.com/get/4682B4/60x20) |
| ROYAL_BLUE       | (65, 105, 225)  | `#4169E1` | ![](https://singlecolorimage.com/get/4169E1/60x20) |
| MEDIUM_BLUE      | (0, 0, 205)     | `#0000CD` | ![](https://singlecolorimage.com/get/0000CD/60x20) |
| BLUE             | (0, 0, 255)     | `#0000FF` | ![](https://singlecolorimage.com/get/0000FF/60x20) |
| NAVY             | (0, 0, 128)     | `#000080` | ![](https://singlecolorimage.com/get/000080/60x20) |
| DARK_BLUE        | (0, 0, 139)     | `#00008B` | ![](https://singlecolorimage.com/get/00008B/60x20) |
| MIDNIGHT_BLUE    | (25, 25, 112)   | `#191970` | ![](https://singlecolorimage.com/get/191970/60x20) |

### Purple / Violet / Magenta Colors

| Name              | RGB             | Hex Code  | Preview                                            |
| ----------------- | --------------- | --------- | -------------------------------------------------- |
| THISTLE           | (216, 191, 216) | `#D8BFD8` | ![](https://singlecolorimage.com/get/D8BFD8/60x20) |
| PLUM              | (221, 160, 221) | `#DDA0DD` | ![](https://singlecolorimage.com/get/DDA0DD/60x20) |
| VIOLET            | (238, 130, 238) | `#EE82EE` | ![](https://singlecolorimage.com/get/EE82EE/60x20) |
| PALE_VIOLET_RED   | (219, 112, 147) | `#DB7093` | ![](https://singlecolorimage.com/get/DB7093/60x20) |
| ORCHID            | (218, 112, 214) | `#DA70D6` | ![](https://singlecolorimage.com/get/DA70D6/60x20) |
| MAGENTA           | (255, 0, 255)   | `#FF00FF` | ![](https://singlecolorimage.com/get/FF00FF/60x20) |
| FUCHSIA           | (255, 0, 255)   | `#FF00FF` | ![](https://singlecolorimage.com/get/FF00FF/60x20) |
| MEDIUM_ORCHID     | (186, 85, 211)  | `#BA55D3` | ![](https://singlecolorimage.com/get/BA55D3/60x20) |
| MEDIUM_PURPLE     | (147, 112, 219) | `#9370DB` | ![](https://singlecolorimage.com/get/9370DB/60x20) |
| MEDIUM_VIOLET_RED | (199, 21, 133)  | `#C71585` | ![](https://singlecolorimage.com/get/C71585/60x20) |
| BLUE_VIOLET       | (138, 43, 226)  | `#8A2BE2` | ![](https://singlecolorimage.com/get/8A2BE2/60x20) |
| DARK_ORCHID       | (153, 50, 204)  | `#9932CC` | ![](https://singlecolorimage.com/get/9932CC/60x20) |
| DARK_VIOLET       | (148, 0, 211)   | `#9400D3` | ![](https://singlecolorimage.com/get/9400D3/60x20) |
| DARK_MAGENTA      | (139, 0, 139)   | `#8B008B` | ![](https://singlecolorimage.com/get/8B008B/60x20) |
| PURPLE            | (128, 0, 128)   | `#800080` | ![](https://singlecolorimage.com/get/800080/60x20) |
| INDIGO            | (75, 0, 130)    | `#4B0082` | ![](https://singlecolorimage.com/get/4B0082/60x20) |

### White Colors

| Name          | RGB             | Hex Code  | Preview                                            |
| ------------- | --------------- | --------- | -------------------------------------------------- |
| WHITE         | (255, 255, 255) | `#FFFFFF` | ![](https://singlecolorimage.com/get/FFFFFF/60x20) |
| SNOW          | (255, 250, 250) | `#FFFAFA` | ![](https://singlecolorimage.com/get/FFFAFA/60x20) |
| HONEYDEW      | (240, 255, 240) | `#F0FFF0` | ![](https://singlecolorimage.com/get/F0FFF0/60x20) |
| MINTCREAM     | (245, 255, 250) | `#F5FFFA` | ![](https://singlecolorimage.com/get/F5FFFA/60x20) |
| AZURE         | (240, 255, 255) | `#F0FFFF` | ![](https://singlecolorimage.com/get/F0FFFF/60x20) |
| ALICEBLUE     | (240, 248, 255) | `#F0F8FF` | ![](https://singlecolorimage.com/get/F0F8FF/60x20) |
| GHOSTWHITE    | (248, 248, 255) | `#F8F8FF` | ![](https://singlecolorimage.com/get/F8F8FF/60x20) |
| WHITESMOKE    | (245, 245, 245) | `#F5F5F5` | ![](https://singlecolorimage.com/get/F5F5F5/60x20) |
| SEASHELL      | (255, 245, 238) | `#FFF5EE` | ![](https://singlecolorimage.com/get/FFF5EE/60x20) |
| BEIGE         | (245, 245, 220) | `#F5F5DC` | ![](https://singlecolorimage.com/get/F5F5DC/60x20) |
| OLDLACE       | (253, 245, 230) | `#FDF5E6` | ![](https://singlecolorimage.com/get/FDF5E6/60x20) |
| FLORALWHITE   | (255, 250, 240) | `#FFFAF0` | ![](https://singlecolorimage.com/get/FFFAF0/60x20) |
| IVORY         | (255, 255, 240) | `#FFFFF0` | ![](https://singlecolorimage.com/get/FFFFF0/60x20) |
| ANTIQUEWHITE  | (250, 235, 215) | `#FAEBD7` | ![](https://singlecolorimage.com/get/FAEBD7/60x20) |
| LINEN         | (250, 240, 230) | `#FAF0E6` | ![](https://singlecolorimage.com/get/FAF0E6/60x20) |
| LAVENDERBLUSH | (255, 240, 245) | `#FFF0F5` | ![](https://singlecolorimage.com/get/FFF0F5/60x20) |
| MISTYROSE     | (255, 228, 225) | `#FFE4E1` | ![](https://singlecolorimage.com/get/FFE4E1/60x20) |

### Gray / Black Colors

| Name             | RGB             | Hex Code  | Preview                                            |
| ---------------- | --------------- | --------- | -------------------------------------------------- |
| LIGHT_GRAY       | (211, 211, 211) | `#D3D3D3` | ![](https://singlecolorimage.com/get/D3D3D3/60x20) |
| GAINSBORO        | (220, 220, 220) | `#DCDCDC` | ![](https://singlecolorimage.com/get/DCDCDC/60x20) |
| SILVER           | (192, 192, 192) | `#C0C0C0` | ![](https://singlecolorimage.com/get/C0C0C0/60x20) |
| GRAY             | (128, 128, 128) | `#808080` | ![](https://singlecolorimage.com/get/808080/60x20) |
| SLATE_GRAY       | (112, 128, 144) | `#708090` | ![](https://singlecolorimage.com/get/708090/60x20) |
| LIGHT_SLATE_GRAY | (119, 136, 153) | `#778899` | ![](https://singlecolorimage.com/get/778899/60x20) |
| DIM_GRAY         | (105, 105, 105) | `#696969` | ![](https://singlecolorimage.com/get/696969/60x20) |
| DARK_SLATE_GRAY  | (47, 79, 79)    | `#2F4F4F` | ![](https://singlecolorimage.com/get/2F4F4F/60x20) |
| BLACK            | (0, 0, 0)       | `#000000` | ![](https://singlecolorimage.com/get/000000/60x20) |
