# Sprite Editing (Work in Progress)

## Explanation

### How Sprites Work
Sprite files cover in-game objects and walls. They are stored in `.SPR` files, each containing multiple images. The sprite data is in hex files, structured as follows:

1. **Number of images** – The first value represents the total number of images in the sprite.
2. **Image dimensions** – Each image's width and height are defined in hex.
3. **Raw image data** – The actual pixel data follows, without delimiters.

#### Example:
```
03 50 50 60 60 70 70 60 60 60 ....
```
- `03` (hex) → `3` (decimal) → This sprite contains **3 images**.
- `50` (hex) → `80` (decimal) → First image size: **80x80 pixels**.
- `60` (hex) → `96` (decimal) → Second image size: **96x96 pixels**.
- `70` (hex) → `112` (decimal) → Third image size: **112x112 pixels**.

The raw image data follows:
- First image → **80 × 80 = 6400** pixels.
- Second image → **96 × 96 = 9216** pixels.
- Third image → **112 × 112 = 12544** pixels.

Since there are no delimiters within the image data, it must be split based on the dimensions defined at the start.

### Palettes
The `.IMG` files relate to the floor and HUD. Editing these affects the palette of sprites on that level. To maintain correct colors:
1. Extract the palette from the relevant floor `.IMG` file.
2. Apply this palette to the sprites.

I'm working on a script that allows you to drag and drop a `.GIF` file to auto-extract the palette. This will take some time to implement.

## Editor (Work in Progress)

Currently, the sprite editing tools are split into multiple files:

- **`jsviewer.html`** – A simple tool to preview hex image data with a random palette. Deprecated but kept for reference.
- **`imageconvert.html`** – Loads a `.PNG` file, applies the palette, and exports the image into **browser local storage** for later import into the main editor.
- **`1_simple_html_to_open_sprite6.html`** – The main sprite editor. Currently hardcoded with a palette (needs modification before sharing). Screenshots and explanations will be added soon.

---

### Notes
- This document will be updated as the project progresses.
- Code for extracting palettes and better integration will be added later.
- Feedback and contributions are welcome!
