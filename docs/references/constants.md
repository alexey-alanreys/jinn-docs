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

| Name    | **50**                                             | **100**                                            | **200**                                            | **300**                                            | **400**                                            | **500**                                            | **600**                                            | **700**                                            | **800**                                            | **900**                                            | **950**                                            |
| ------- | -------------------------------------------------- | -------------------------------------------------- | -------------------------------------------------- | -------------------------------------------------- | -------------------------------------------------- | -------------------------------------------------- | -------------------------------------------------- | -------------------------------------------------- | -------------------------------------------------- | -------------------------------------------------- | -------------------------------------------------- |
| RED     | ![](https://singlecolorimage.com/get/FEF2F2/40x20) | ![](https://singlecolorimage.com/get/FEE2E2/40x20) | ![](https://singlecolorimage.com/get/FECACA/40x20) | ![](https://singlecolorimage.com/get/FCA5A5/40x20) | ![](https://singlecolorimage.com/get/F87171/40x20) | ![](https://singlecolorimage.com/get/EF4444/40x20) | ![](https://singlecolorimage.com/get/DC2626/40x20) | ![](https://singlecolorimage.com/get/B91C1C/40x20) | ![](https://singlecolorimage.com/get/991B1B/40x20) | ![](https://singlecolorimage.com/get/7F1D1D/40x20) | ![](https://singlecolorimage.com/get/450A0A/40x20) |
| ORANGE  | ![](https://singlecolorimage.com/get/FFF7ED/40x20) | ![](https://singlecolorimage.com/get/FFEDD5/40x20) | ![](https://singlecolorimage.com/get/FED7AA/40x20) | ![](https://singlecolorimage.com/get/FDBA74/40x20) | ![](https://singlecolorimage.com/get/FB923C/40x20) | ![](https://singlecolorimage.com/get/F97316/40x20) | ![](https://singlecolorimage.com/get/EA580C/40x20) | ![](https://singlecolorimage.com/get/C2410C/40x20) | ![](https://singlecolorimage.com/get/9A3412/40x20) | ![](https://singlecolorimage.com/get/7C2D12/40x20) | ![](https://singlecolorimage.com/get/431407/40x20) |
| AMBER   | ![](https://singlecolorimage.com/get/FFFBEB/40x20) | ![](https://singlecolorimage.com/get/FEF3C7/40x20) | ![](https://singlecolorimage.com/get/FDE68A/40x20) | ![](https://singlecolorimage.com/get/FCD34D/40x20) | ![](https://singlecolorimage.com/get/FBBF24/40x20) | ![](https://singlecolorimage.com/get/F59E0B/40x20) | ![](https://singlecolorimage.com/get/D97706/40x20) | ![](https://singlecolorimage.com/get/B45309/40x20) | ![](https://singlecolorimage.com/get/92400E/40x20) | ![](https://singlecolorimage.com/get/78350F/40x20) | ![](https://singlecolorimage.com/get/451A03/40x20) |
| YELLOW  | ![](https://singlecolorimage.com/get/FEFCE8/40x20) | ![](https://singlecolorimage.com/get/FEF9C3/40x20) | ![](https://singlecolorimage.com/get/FEF08A/40x20) | ![](https://singlecolorimage.com/get/FDE047/40x20) | ![](https://singlecolorimage.com/get/FACC15/40x20) | ![](https://singlecolorimage.com/get/EAB308/40x20) | ![](https://singlecolorimage.com/get/CA8A04/40x20) | ![](https://singlecolorimage.com/get/A16207/40x20) | ![](https://singlecolorimage.com/get/854D0E/40x20) | ![](https://singlecolorimage.com/get/713F12/40x20) | ![](https://singlecolorimage.com/get/422006/40x20) |
| LIME    | ![](https://singlecolorimage.com/get/F7FEE7/40x20) | ![](https://singlecolorimage.com/get/ECFCCB/40x20) | ![](https://singlecolorimage.com/get/D9F99D/40x20) | ![](https://singlecolorimage.com/get/BEF264/40x20) | ![](https://singlecolorimage.com/get/A3E635/40x20) | ![](https://singlecolorimage.com/get/84CC16/40x20) | ![](https://singlecolorimage.com/get/65A30D/40x20) | ![](https://singlecolorimage.com/get/4D7C0F/40x20) | ![](https://singlecolorimage.com/get/3F6212/40x20) | ![](https://singlecolorimage.com/get/365314/40x20) | ![](https://singlecolorimage.com/get/1A2E05/40x20) |
| GREEN   | ![](https://singlecolorimage.com/get/F0FDF4/40x20) | ![](https://singlecolorimage.com/get/DCFCE7/40x20) | ![](https://singlecolorimage.com/get/BBF7D0/40x20) | ![](https://singlecolorimage.com/get/86EFAC/40x20) | ![](https://singlecolorimage.com/get/4ADE80/40x20) | ![](https://singlecolorimage.com/get/22C55E/40x20) | ![](https://singlecolorimage.com/get/16A34A/40x20) | ![](https://singlecolorimage.com/get/15803D/40x20) | ![](https://singlecolorimage.com/get/166534/40x20) | ![](https://singlecolorimage.com/get/14532D/40x20) | ![](https://singlecolorimage.com/get/052E16/40x20) |
| EMERALD | ![](https://singlecolorimage.com/get/ECFDF5/40x20) | ![](https://singlecolorimage.com/get/D1FAE5/40x20) | ![](https://singlecolorimage.com/get/A7F3D0/40x20) | ![](https://singlecolorimage.com/get/6EE7B7/40x20) | ![](https://singlecolorimage.com/get/34D399/40x20) | ![](https://singlecolorimage.com/get/10B981/40x20) | ![](https://singlecolorimage.com/get/059669/40x20) | ![](https://singlecolorimage.com/get/047857/40x20) | ![](https://singlecolorimage.com/get/065F46/40x20) | ![](https://singlecolorimage.com/get/064E3B/40x20) | ![](https://singlecolorimage.com/get/022C22/40x20) |
| TEAL    | ![](https://singlecolorimage.com/get/F0FDFA/40x20) | ![](https://singlecolorimage.com/get/CCFBF1/40x20) | ![](https://singlecolorimage.com/get/99F6E4/40x20) | ![](https://singlecolorimage.com/get/5EEAD4/40x20) | ![](https://singlecolorimage.com/get/2DD4BF/40x20) | ![](https://singlecolorimage.com/get/14B8A6/40x20) | ![](https://singlecolorimage.com/get/0D9488/40x20) | ![](https://singlecolorimage.com/get/0F766E/40x20) | ![](https://singlecolorimage.com/get/115E59/40x20) | ![](https://singlecolorimage.com/get/134E4A/40x20) | ![](https://singlecolorimage.com/get/042F2E/40x20) |
| CYAN    | ![](https://singlecolorimage.com/get/ECFEFF/40x20) | ![](https://singlecolorimage.com/get/CFFAFE/40x20) | ![](https://singlecolorimage.com/get/A5F3FC/40x20) | ![](https://singlecolorimage.com/get/67E8F9/40x20) | ![](https://singlecolorimage.com/get/22D3EE/40x20) | ![](https://singlecolorimage.com/get/06B6D4/40x20) | ![](https://singlecolorimage.com/get/0891B2/40x20) | ![](https://singlecolorimage.com/get/0E7490/40x20) | ![](https://singlecolorimage.com/get/155E75/40x20) | ![](https://singlecolorimage.com/get/164E63/40x20) | ![](https://singlecolorimage.com/get/083344/40x20) |
| SKY     | ![](https://singlecolorimage.com/get/F0F9FF/40x20) | ![](https://singlecolorimage.com/get/E0F2FE/40x20) | ![](https://singlecolorimage.com/get/BAE6FD/40x20) | ![](https://singlecolorimage.com/get/7DD3FC/40x20) | ![](https://singlecolorimage.com/get/38BDF8/40x20) | ![](https://singlecolorimage.com/get/0EA5E9/40x20) | ![](https://singlecolorimage.com/get/0284C7/40x20) | ![](https://singlecolorimage.com/get/0369A1/40x20) | ![](https://singlecolorimage.com/get/075985/40x20) | ![](https://singlecolorimage.com/get/0C4A6E/40x20) | ![](https://singlecolorimage.com/get/082F49/40x20) |
| BLUE    | ![](https://singlecolorimage.com/get/EFF6FF/40x20) | ![](https://singlecolorimage.com/get/DBEAFE/40x20) | ![](https://singlecolorimage.com/get/BFDBFE/40x20) | ![](https://singlecolorimage.com/get/93C5FD/40x20) | ![](https://singlecolorimage.com/get/60A5FA/40x20) | ![](https://singlecolorimage.com/get/3B82F6/40x20) | ![](https://singlecolorimage.com/get/2563EB/40x20) | ![](https://singlecolorimage.com/get/1D4ED8/40x20) | ![](https://singlecolorimage.com/get/1E40AF/40x20) | ![](https://singlecolorimage.com/get/1E3A8A/40x20) | ![](https://singlecolorimage.com/get/172554/40x20) |
| INDIGO  | ![](https://singlecolorimage.com/get/EEF2FF/40x20) | ![](https://singlecolorimage.com/get/E0E7FF/40x20) | ![](https://singlecolorimage.com/get/C7D2FE/40x20) | ![](https://singlecolorimage.com/get/A5B4FC/40x20) | ![](https://singlecolorimage.com/get/818CF8/40x20) | ![](https://singlecolorimage.com/get/6366F1/40x20) | ![](https://singlecolorimage.com/get/4F46E5/40x20) | ![](https://singlecolorimage.com/get/4338CA/40x20) | ![](https://singlecolorimage.com/get/3730A3/40x20) | ![](https://singlecolorimage.com/get/312E81/40x20) | ![](https://singlecolorimage.com/get/1E1B4B/40x20) |
| VIOLET  | ![](https://singlecolorimage.com/get/F5F3FF/40x20) | ![](https://singlecolorimage.com/get/EDE9FE/40x20) | ![](https://singlecolorimage.com/get/DDD6FE/40x20) | ![](https://singlecolorimage.com/get/C4B5FD/40x20) | ![](https://singlecolorimage.com/get/A78BFA/40x20) | ![](https://singlecolorimage.com/get/8B5CF6/40x20) | ![](https://singlecolorimage.com/get/7C3AED/40x20) | ![](https://singlecolorimage.com/get/6D28D9/40x20) | ![](https://singlecolorimage.com/get/5B21B6/40x20) | ![](https://singlecolorimage.com/get/4C1D95/40x20) | ![](https://singlecolorimage.com/get/2E1065/40x20) |
| PURPLE  | ![](https://singlecolorimage.com/get/FAF5FF/40x20) | ![](https://singlecolorimage.com/get/F3E8FF/40x20) | ![](https://singlecolorimage.com/get/E9D5FF/40x20) | ![](https://singlecolorimage.com/get/D8B4FE/40x20) | ![](https://singlecolorimage.com/get/C084FC/40x20) | ![](https://singlecolorimage.com/get/A855F7/40x20) | ![](https://singlecolorimage.com/get/9333EA/40x20) | ![](https://singlecolorimage.com/get/7E22CE/40x20) | ![](https://singlecolorimage.com/get/6B21A8/40x20) | ![](https://singlecolorimage.com/get/581C87/40x20) | ![](https://singlecolorimage.com/get/3B0764/40x20) |
| FUCHSIA | ![](https://singlecolorimage.com/get/FDF4FF/40x20) | ![](https://singlecolorimage.com/get/FAE8FF/40x20) | ![](https://singlecolorimage.com/get/F5D0FE/40x20) | ![](https://singlecolorimage.com/get/F0ABFC/40x20) | ![](https://singlecolorimage.com/get/E879F9/40x20) | ![](https://singlecolorimage.com/get/D946EF/40x20) | ![](https://singlecolorimage.com/get/C026D3/40x20) | ![](https://singlecolorimage.com/get/A21CAF/40x20) | ![](https://singlecolorimage.com/get/86198F/40x20) | ![](https://singlecolorimage.com/get/701A75/40x20) | ![](https://singlecolorimage.com/get/4A044E/40x20) |
| PINK    | ![](https://singlecolorimage.com/get/FDF2F8/40x20) | ![](https://singlecolorimage.com/get/FCE7F3/40x20) | ![](https://singlecolorimage.com/get/FBCFE8/40x20) | ![](https://singlecolorimage.com/get/F9A8D4/40x20) | ![](https://singlecolorimage.com/get/F472B6/40x20) | ![](https://singlecolorimage.com/get/EC4899/40x20) | ![](https://singlecolorimage.com/get/DB2777/40x20) | ![](https://singlecolorimage.com/get/BE185D/40x20) | ![](https://singlecolorimage.com/get/9D174D/40x20) | ![](https://singlecolorimage.com/get/831843/40x20) | ![](https://singlecolorimage.com/get/500724/40x20) |
| ROSE    | ![](https://singlecolorimage.com/get/FFF1F2/40x20) | ![](https://singlecolorimage.com/get/FFE4E6/40x20) | ![](https://singlecolorimage.com/get/FECDD3/40x20) | ![](https://singlecolorimage.com/get/FDA4AF/40x20) | ![](https://singlecolorimage.com/get/FB7185/40x20) | ![](https://singlecolorimage.com/get/F43F5E/40x20) | ![](https://singlecolorimage.com/get/E11D48/40x20) | ![](https://singlecolorimage.com/get/BE123C/40x20) | ![](https://singlecolorimage.com/get/9F1239/40x20) | ![](https://singlecolorimage.com/get/881337/40x20) | ![](https://singlecolorimage.com/get/4C0519/40x20) |
| SLATE   | ![](https://singlecolorimage.com/get/F8FAFC/40x20) | ![](https://singlecolorimage.com/get/F1F5F9/40x20) | ![](https://singlecolorimage.com/get/E2E8F0/40x20) | ![](https://singlecolorimage.com/get/CBD5E1/40x20) | ![](https://singlecolorimage.com/get/94A3B8/40x20) | ![](https://singlecolorimage.com/get/64748B/40x20) | ![](https://singlecolorimage.com/get/475569/40x20) | ![](https://singlecolorimage.com/get/334155/40x20) | ![](https://singlecolorimage.com/get/1E293B/40x20) | ![](https://singlecolorimage.com/get/0F172A/40x20) | ![](https://singlecolorimage.com/get/020617/40x20) |
| GRAY    | ![](https://singlecolorimage.com/get/F9FAFB/40x20) | ![](https://singlecolorimage.com/get/F3F4F6/40x20) | ![](https://singlecolorimage.com/get/E5E7EB/40x20) | ![](https://singlecolorimage.com/get/D1D5DB/40x20) | ![](https://singlecolorimage.com/get/9CA3AF/40x20) | ![](https://singlecolorimage.com/get/6B7280/40x20) | ![](https://singlecolorimage.com/get/4B5563/40x20) | ![](https://singlecolorimage.com/get/374151/40x20) | ![](https://singlecolorimage.com/get/1F2937/40x20) | ![](https://singlecolorimage.com/get/111827/40x20) | ![](https://singlecolorimage.com/get/030712/40x20) |
| ZINC    | ![](https://singlecolorimage.com/get/FAFAFA/40x20) | ![](https://singlecolorimage.com/get/F4F4F5/40x20) | ![](https://singlecolorimage.com/get/E4E4E7/40x20) | ![](https://singlecolorimage.com/get/D4D4D8/40x20) | ![](https://singlecolorimage.com/get/A1A1AA/40x20) | ![](https://singlecolorimage.com/get/71717A/40x20) | ![](https://singlecolorimage.com/get/52525B/40x20) | ![](https://singlecolorimage.com/get/3F3F46/40x20) | ![](https://singlecolorimage.com/get/27272A/40x20) | ![](https://singlecolorimage.com/get/18181B/40x20) | ![](https://singlecolorimage.com/get/09090B/40x20) |
| NEUTRAL | ![](https://singlecolorimage.com/get/FAFAFA/40x20) | ![](https://singlecolorimage.com/get/F5F5F5/40x20) | ![](https://singlecolorimage.com/get/E5E5E5/40x20) | ![](https://singlecolorimage.com/get/D4D4D4/40x20) | ![](https://singlecolorimage.com/get/A3A3A3/40x20) | ![](https://singlecolorimage.com/get/737373/40x20) | ![](https://singlecolorimage.com/get/525252/40x20) | ![](https://singlecolorimage.com/get/404040/40x20) | ![](https://singlecolorimage.com/get/262626/40x20) | ![](https://singlecolorimage.com/get/171717/40x20) | ![](https://singlecolorimage.com/get/0A0A0A/40x20) |
| STONE   | ![](https://singlecolorimage.com/get/FAFAF9/40x20) | ![](https://singlecolorimage.com/get/F5F5F4/40x20) | ![](https://singlecolorimage.com/get/E7E5E4/40x20) | ![](https://singlecolorimage.com/get/D6D3D1/40x20) | ![](https://singlecolorimage.com/get/A8A29E/40x20) | ![](https://singlecolorimage.com/get/78716C/40x20) | ![](https://singlecolorimage.com/get/57534E/40x20) | ![](https://singlecolorimage.com/get/44403C/40x20) | ![](https://singlecolorimage.com/get/292524/40x20) | ![](https://singlecolorimage.com/get/1C1917/40x20) | ![](https://singlecolorimage.com/get/0C0A09/40x20) |
