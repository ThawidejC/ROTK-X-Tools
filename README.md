 
### What is this?
 
**ROTK X Face Portrait Tool v10** is a GUI application for **editing character face portraits** in Romance of the Three Kingdoms X (San10) on PC.  
It lets you open game files directly, import replacement images, adjust crop positions, and write changes back to the game files instantly. It can also package your edits into a self-contained `.exe` MOD installer to share with others.
 
---
 
### 📁 Supported Game Files
 
The tool works with two sets of face image files, all located in the game's `San10WPK\Grp\` folder:
 
**Set 1 — GrpFace (Main Officers, ~650 slots)**
 
| File | Size | Purpose |
|---|---|---|
| `GrpFaceL.s10` | 240×240 px | Large portrait |
| `GrpFaceS.s10` | 64×80 px | Medium portrait |
| `GrpFaceT.s10` | 31×40 px | Thumbnail portrait |
 
**Set 2 — GpkFace (Custom / Generic face pack)**
 
| File | Size | Purpose |
|---|---|---|
| `GpkFaceL.s10` | 240×240 px | Large portrait |
| `GpkFaceS.s10` | 64×80 px | Medium portrait |
| `GpkFaceT.s10` | 32×40 px | Thumbnail portrait |
 
Internal format: **pix_first_bgra** (SAN10GRP magic header) — pixel indices + 256-colour BGRA palette, alpha = `0x80`
 
---
 
### 🖥️ Main Window Layout
 
#### 1. Toolbar
 
- **📂 Open Set 1 (Grp)** / **📂 Open Set 2 (Gpk)**  
  Auto-searches all drives for the `San10WPK\Grp\` folder and loads the files.  
  Automatically backs up original files to `BackupGRP` / `BackupGPK` on first load.
- **🔄 Restore**  
  Restores game files from backup at any time.
- **Manual Open buttons** (small buttons below)  
  Open individual files manually if auto-search fails.
---
 
#### 2. Left Panel — Thumbnail List
 
Displays thumbnails for every slot in the loaded game file.
 
| Feature | Description |
|---|---|
| **Browse file** | Switch between any of the 6 face files |
| **Search** | Filter slots by number |
| **Lazy Loading** | Thumbnails load in the background — no UI freeze on large files |
| **★ replaced** | Green star badge on any slot that has a replacement imported |
| **Left-click** | Select a slot → instantly shown in center panels |
| **Right-click** | Context menu: Export PNG / Import replacement / Clear |
| **📥 Batch Import** | Import multiple images at once, sorted by Natural Sort (1, 2, 3 … 10, 11) |
| **Batch start slot #** | Set the starting slot index for Batch Import |
| **🗑 Clear All** | Remove all imported replacements from memory |
 
---
 
#### 3. Set 1 / Set 2 Tabs (Center panels)
 
Each tab contains three sub-panels:
 
##### L Panel — GrpFaceL / GpkFaceL (240×240)
 
- **Click the image** → file picker opens to select a new source image
- **Image ≤ 480×480 px** → resized directly to 240×240
- **Image > 480×480 px** → enters **Crop Mode**:
  - Canvas enlarges to 480×480
  - A 240×240 yellow crop box can be dragged freely
  - Drag box = reposition crop area | Click without drag = import new image
##### S Panel — GrpFaceS / GpkFaceS (64×80)
##### T Panel — GrpFaceT / GpkFaceT (31×40 / 32×40)
 
- Shows the 240×240 source image with a yellow 64×80 crop box overlay
- **Drag box** = reposition crop
- **Click without drag** = import a new image
- Right preview panel shows the actual encoded output at game resolution
---
 
#### 4. Apply Set Button
 
```
✅  Apply Set 1 — Write replacements to game files
✅  Apply Set 2 — Write replacements to game files
```
 
- Writes **only slots that have replacements** — all other slots are untouched
- Encodes each image to **pix_first_bgra** format with a quantized BGRA palette
- Shows a confirmation dialog with a full summary before writing
---
 
#### 5. Share MOD Button 📦
 
Available only after a successful Apply (button turns green).
 
**The Share MOD dialog provides:**
 
| Feature | Description |
|---|---|
| **Set badge** | Set 1 = 👑 Main Officers (green) / Set 2 = 🎨 Custom face (blue) |
| **MOD Name** | Name your MOD |
| **MOD Icon (.ico)** | Browse for a `.ico` file to use as the `.exe` icon (optional) |
| **Save .exe to folder** | Choose the output folder |
| **Files to embed** | Lists the game files to be embedded, with sizes |
| **🔨 Build .exe** | Compiles a self-contained installer using PyInstaller |
 
**The generated `.exe`** is a standalone installer — end users don't need Python installed:
 
- **Button ① Search & Backup** — scans all drives for `San10WPK\Grp\`, backs up originals automatically
- **Button ② Auto Install** — installs the MOD files into the game automatically
- **Button ③ Choose folder manually** — manual folder picker if auto-search fails
Clicking anywhere on each button row (number, title, or subtitle) triggers the action.
 
---
 
### 🔄 Typical Workflow
 
```
1. Launch the tool
2. Click "Open Set 1" or "Open Set 2" → auto-loads game files
3. Click a slot in the left panel to select it
4. Click the L panel image to import a new portrait
   - Adjust the crop boxes on S and T panels as needed
5. Click "Apply Set" to write changes to the game files
6. (Optional) Click "Share MOD" to build a .exe installer to share
```
 
---
 
### ⚠️ Notes
 
- Original game files are backed up automatically on first load into `BackupGRP` / `BackupGPK`
- When switching to Set 2 while Set 1 has pending replacements, the tool prompts you to Apply Set 1 first to avoid data loss
- "Clear All" removes replacements from memory only — **game files are not affected** until you press Apply
- The `.exe` installer embeds all game data as Base64 inside the executable, so it is fully portable
---
 
### 📐 Technical Details
 
| Item | Value |
|---|---|
| Image format | pix_first_bgra — pixel indices (W×H bytes) + BGRA palette (256×4 bytes) |
| Alpha channel | Fixed at `0x80` (PS2 half-alpha convention) |
| Palette quantization | 256 colours via PIL `quantize()` with LANCZOS resize |
| File magic | `SAN10GRP` (8 bytes at offset 0x00) |
| Colour swap | RGB → BGR on palette encode |
| Crop default | S/T box centred horizontally, positioned near top vertically |
 
---
 
### 📜 License
 
This tool is provided as-is for personal modding use.  
ROTK X is a product of KOEI TECMO. This tool is not affiliated with or endorsed by KOEI TECMO.
<a href="http://s01.flagcounter.com/more/EkF"><img src="https://s01.flagcounter.com/count2/EkF/bg_FFFFFF/txt_000000/border_CCCCCC/columns_2/maxflags_10/viewers_0/labels_0/pageviews_0/flags_0/percent_0/" alt="Flag Counter" border="0"></a>

