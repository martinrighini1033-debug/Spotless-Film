# Spotless Film — Windows Port + Improvements

## Run in Google Colab

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/martinrighini1033-debug/Spotless-Film/blob/main/final_polvo.ipynb)

> Fork of [yahyarahhawi/Spotless-Film](https://github.com/yahyarahhawi/Spotless-Film)  
> An AI-powered dust removal tool for scanned film negatives, now running on Windows with a trained model and several new features.

---

## What's new in this fork

### 🪟 Windows compatibility
The original project was built for macOS (relying on Apple's MPS/Metal backend). This fork runs fully on Windows:
- Automatic detection of CUDA (NVIDIA GPU) with CPU fallback
- Removed macOS-exclusive dependencies

### 🧠 Trained model included
The original repository was missing the `.pth` weights file. This fork includes a trained U-Net model ready to use — no training required to get started.

> **Download the model weights:** (Google Drive link) *([https://drive.google.com/file/d/1Faw8rt73mSEqlsTSN9ra0nM88Iy3rXli/view?usp=drive_link])*  
> Place the `.pth` file in the `dust_remover/` folder before running.

### 🖼️ Batch processing
- New **"Process Folder"** button to process entire folders of images
- **Automatic mode:** processes and saves all images without interruption
- **Manual mode:** pauses on each image with a floating (non-blocking) panel, letting you adjust sensitivity and brushes before confirming — the main window stays fully interactive while the panel is open
- Real synchronization between stages (detection → removal → save) using `threading.Event` instead of fixed `time.sleep`, ensuring all images are saved correctly
- Results are saved to a `spotless_output/` subfolder

### 📂 Improved file selection
- Remembers the last folder used when opening images
- Drag & drop support directly onto the canvas

### 🔴 Detection overlay
- The red dust overlay now appears immediately after detection, without needing to click "Remove Dust" first
- Fixed RGBA compositing so the overlay blends correctly in tkinter
- Opacity slider now works correctly

### 🎨 Brush and eraser tools
- The brush/eraser preview circle stays visible at all times, including while painting with the mouse button held down (similar to Photoshop behavior)
- Mouse cursor is correctly restored when leaving the image area
- Fixed a bug where the cursor would disappear permanently after using a tool

### ↩️ Undo support
- New **"↩ Undo"** button in the top toolbar
- Stores up to 5 previous "Remove Dust" results
- `Ctrl+Z` undoes the last processed result, or the last brush stroke if no results are stacked
- History is cleared when loading a new image

### 🔧 Technical fixes
- **Sensitivity slider now works:** the detection threshold was previously hardcoded at `0.5`, ignoring the slider entirely — now it reads the actual value
- **Letterbox padding:** images are resized to the U-Net input preserving aspect ratio (previously they were squeezed to 1024×1024, distorting proportions and causing false positives)
- **Reduced mask dilation:** `kernel_size=3` and `inpaintRadius=3` to prevent inpainting from bleeding into neighboring areas like textures or edges
- **EXIF orientation:** images are automatically displayed with the correct rotation
- **Clean state between images:** when loading a new image, processed result, undo history, and view mode are all properly reset
- **Split view:** the before/after slider now covers the full image edge to edge

---

## Known limitations / future work

- The model still produces some false positives on high-contrast textures (ocean waves, reflections, patterned fabrics). This is a training data issue — adding more annotated negatives of these scenes will improve accuracy.
- More training data is being collected to refine detection quality.

---

## Requirements

- Python 3.9+
- PyTorch (CPU or CUDA)
- See `requirements.txt` for full dependencies

## How to run (Windows)

```bash
pip install -r requirements.txt
python spotless_film_modern.py
```

---

## Credits

Original project by [yahyarahhawi](https://github.com/yahyarahhawi/Spotless-Film).  
Windows port, model training, and feature additions by [martinrighini1033-debug](https://github.com/martinrighini1033-debug).
