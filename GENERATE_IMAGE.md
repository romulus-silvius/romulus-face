# How to Generate the Romulus Bust Image

## Quick Start (Easiest Method)

### Option 1: Media.io (Recommended - No Account Needed for First Try)

1. Go to: https://www.media.io/ai/image-to-image/ai-statue-generator
2. Click "Image to Image"
3. Select **"Nano Banana"** AI model
4. Upload a base portrait (or use text-to-image mode if available)
5. Use this prompt:

```
Classical Roman marble bust sculpture, bearded emperor with laurel wreath crown, 
noble features, strong jaw, weathered white marble, museum quality, 
dramatic lighting from upper left, ancient Rome, professional photography, 
high detail, 8k resolution
```

6. Click Generate
7. Download the result
8. Save as `romulus-bust.png` in this repo

---

### Option 2: Easy-Peasy.AI

1. Go to: https://easy-peasy.ai/ai-image-generator
2. Enter this prompt:

```
Photorealistic classical Roman marble bust sculpture, mature bearded man 
wearing laurel wreath crown, weathered white marble, museum photography, 
dramatic chiaroscuro lighting, ancient Roman emperor style, noble expression, 
strong facial features, high resolution, professional quality
```

3. Select "Realistic" or "Photorealistic" style
4. Generate
5. Download and save as `romulus-bust.png`

---

### Option 3: Photosstyle.com

1. Go to: https://www.photosstyle.com/art/photo-to-sculpture
2. Upload a portrait photo (or use default)
3. Convert to classical sculpture style
4. Download result as `romulus-bust.png`

---

## For Emerald Eyes (Post-Processing)

The eyes should glow emerald green. You can either:

**Option A:** Add to the prompt:
```
...with glowing emerald green eyes...
```

**Option B:** Edit in Photoshop/GIMP:
1. Open the generated bust image
2. Select eye areas
3. Hue/Saturation adjustment: Shift to green/cyan
4. Increase brightness/saturation
5. Add outer glow (green, 40px radius, 60% opacity)

---

## After Generation

1. Save image as `romulus-bust.png` in repo root
2. Edit `display.html`:
   - Line ~72: Uncomment `<img id="bust" src="romulus-bust.png" alt="Romulus Bust">`
   - Line ~73: Uncomment `<canvas id="eye-glow"></canvas>`
   - Lines ~145-195: Uncomment the eye glow script
3. Adjust eye glow coordinates in script if needed:
   - `leftEyeX` (default: 38% of width)
   - `rightEyeX` (default: 62% of width)
   - `eyeY` (default: 35% of height)
4. Commit and push

---

## Alternative: Commission Professional Pixel Art

If you want hand-crafted pixel art instead:

**Fiverr artists:**
- Search "pixel art portrait" or "pixel art bust"
- Budget: $10-50 for basic, $100+ for highly detailed
- Provide reference images of Roman busts
- Specify: "Classical Roman marble bust, bearded emperor, laurel wreath, emerald glowing eyes, 400x500px minimum, 100+ colors"

**Professional pixel artists:**
- laborlimae (Fiverr - realistic pixel portraits)
- Search r/PixelArt for commissions

---

## Current Status

- **Framework:** ✅ Complete (display.html ready)
- **Image:** ❌ Needs generation
- **Integration:** ⏳ Waiting for image file

The presentation is ready — just need the actual art asset.
