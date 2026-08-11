---
name: design-apple-watch-wallpaper
description: Design or edit Photos-face wallpapers exclusively for the latest Apple Watch Series in 42mm (374×446 px) and 46mm (416×496 px), keeping the subject clear of time, date, complications, rounded corners, and watchOS cropping while optionally applying content-aware subject/background color harmonization. Use when Codex needs to create, crop, extend, recolor, adapt, preview, or validate a wallpaper for a current 42mm or 46mm Apple Watch Series. Do not use for Apple Watch Ultra, Apple Watch SE, older 40/41/44/45mm Series models, other watches, or custom watch-face packages.
---

# Design Latest Apple Watch Series Wallpaper

Create wallpapers only for the latest Apple Watch Series in 42mm or 46mm. Target the watchOS Photos face. Keep the clean wallpaper separate from previews containing watchOS UI.

## Enforce scope first

Accept only these targets:

| Case | Final canvas | 4× working canvas | Aspect ratio |
|---|---:|---:|---:|
| 42mm | 374×446 px | 1496×1784 px | 187:223 |
| 46mm | 416×496 px | 1664×1984 px | 26:31 |

- Interpret “42mm” or “46mm” as the latest Apple Watch Series only.
- Ask the user to choose 42mm or 46mm if the size is missing.
- If the request names Ultra, SE, another brand, or 40/41/44/45/49mm, explain that the skill does not support it. Do not silently approximate.
- If Apple changes the latest Series display specification, verify the current Apple technical specifications before proceeding and report the mismatch instead of guessing.
- Create a Photos-face background, not a `.watchface` package or a replacement for watchOS UI.

## Establish the overlay layout

Collect evidence in this order:

1. A screenshot from the user's Watch app showing the exact Photos-face preview.
2. A screenshot of the face on the watch.
3. The stated time position, time size, font, 12/24-hour format, date, and complications.

Treat the user's actual preview as authoritative. Do not infer an exact clock box from a generic product image.

If no preview or layout is available, stop before image generation and ask the user to select exactly one layout:

1. **`clock-left` — 左侧时间：** Place the subject toward the right and reserve the left for time.
2. **`clock-right` — 右侧时间：** Place the subject toward the left and reserve the right for time.
3. **`clock-top` — 顶部时间：** Place the subject low and reserve the upper band for time and date.

Use this concise question:

> 请选择时间布局：1）左侧时间，主体靠右；2）右侧时间，主体靠左；3）顶部时间，主体靠下。也可以直接发送 Watch App 表盘预览。

Do not generate while waiting for the choice. If the user already specified a layout or supplied an exact preview, do not ask again. Lock the selected layout for the request and produce only that version.

## Build the safe-zone map

Use normalized coordinates where `(0,0)` is top-left and `(1,1)` is bottom-right.

For an exact preview:

1. Mark the bounding boxes of time, date, complications, and persistent indicators.
2. Expand each UI box by `4%` of canvas width horizontally and `3%` of canvas height vertically.
3. Mark the outer `5%` horizontally and `4%` vertically as crop-sensitive.
4. Merge the expanded UI boxes and crop-sensitive border into the **occupied mask**.
5. Keep all high-value subject details outside the occupied mask.

Use these minimum final-canvas border guides:

| Case | Left/right crop-sensitive edge | Top/bottom crop-sensitive edge | Clock-box expansion (x/y) |
|---|---:|---:|---:|
| 42mm | 19 px | 18 px | 15 px / 13 px |
| 46mm | 21 px | 20 px | 17 px / 15 px |

Round outward when converting normalized guides to pixels. Treat these as minimums, not targets for placing faces against the edge.

Allow a wide digital time such as `23:59`, including font overhang and date text. Do not validate only against a narrow sample such as `1:11`.

## Protect the subject

Identify faces, eyes, hands, pet heads, product silhouettes, logos, emblems, and any detail named by the user.

- Keep every high-value detail fully outside the occupied mask.
- Keep the focal point at least `5%` of the short canvas dimension from the expanded clock mask.
- Leave low-detail negative space behind time: plain paper, sky, mist, wall, water, gradient, or defocused texture.
- Avoid bright highlights, hard edges, tiny marks, or facial features behind glyphs.
- Preserve natural gaze and motion space.
- Do not rely on watchOS depth effects or automatic reframing to fix a collision.
- Keep nonessential texture continuous through the clock region so the wallpaper does not look artificially cut out.

## Adapt colors generically

Base every color decision on the current image. Never hardcode a hue, palette, art style, skin tone, or background color from a previous task.

Separate the image conceptually into:

1. **Protected subject:** faces, skin, eyes, hair accents, logos, emblems, text, product colors, and user-named details.
2. **Adjustable subject:** clothing, fur, secondary surfaces, and noncritical midtones that can tolerate subtle grading.
3. **Background:** areas that may be recolored, simplified, darkened, or lightened.
4. **Clock zone:** the background area directly behind time, date, and complications.

Use one of these modes:

- **`preserve`:** Keep all subject pixels unchanged. Adjust only background and clock zone. Use when identity, artwork fidelity, product color, or brand color is critical.
- **`harmonize` (default):** Derive the background from the image's own dominant hues, reduce background chroma and detail, and apply only subtle global grading to the adjustable subject.
- **`contrast`:** Increase subject/background separation for small-screen readability while protecting critical colors and the original style.

Apply this content-aware process:

1. Estimate foreground and background masks. Detect multiple subjects and thin structures such as hair, fur, fingers, antennae, weapons, and straps.
2. Extract a weighted palette from the subject and background separately. Ignore transparent pixels and downweight near-white or near-black areas that dominate only because of empty canvas.
3. Work in a perceptual color space such as OKLCH or CIELAB when practical. Choose background hues from the current subject palette rather than a fixed template.
4. Reduce background chroma and local detail before changing the subject. Keep the clock zone calmer than the rest of the background.
5. Maintain clear silhouette separation. Aim for an absolute OKLCH lightness difference of at least `0.20` between important subject edges and their immediate background, or use an equivalent visually verified separation.
6. Keep intended clock text readable against the clock zone. Target at least `4.5:1` luminance contrast in the preview; use a restrained local gradient or tonal wash if the raw background cannot provide it.
7. Simulate a dim/Always-On view. If important subject edges disappear when brightness is reduced, strengthen local separation without globally crushing shadows or highlights.

Limit automatic subject changes:

- In `harmonize`, keep subject lightness/exposure changes within approximately `±5%` and saturation/chroma changes within approximately `±8%` unless the user requests stronger grading.
- Do not hue-shift protected colors. Preserve skin, eyes, logos, emblems, UI screenshots, product finishes, and deliberate accent colors.
- Do not recolor different subjects into one palette when their distinction is meaningful.
- Do not use a heavy vignette, color cast, or glow as a substitute for proper composition.
- Preserve line art, paper texture, film grain, brushwork, and other medium-specific characteristics.

If the clock color is unknown and it materially affects readability, do not choose one silently or generate both. Ask the user to select exactly one treatment:

- `clock-dark`: brighten or calm only the clock zone for dark time glyphs.
- `clock-light`: darken or calm only the clock zone for light time glyphs.

Keep clock glyphs out of the clean wallpaper. Use them only in previews.

If foreground/background separation is unreliable, multiple subjects overlap the clock zone, or protected-color detection is uncertain, fall back to `preserve` and adjust only a soft clock-zone mask. Report the fallback instead of applying destructive global recoloring.

## Edit with minimum change

For an existing image, preserve the original pixels and identity as much as possible.

1. Prefer deterministic crop, resize, padding, and background-color extension when these operations can create the required layout.
2. Use generative editing only when missing background must be reconstructed or the subject must be moved beyond what deterministic compositing can achieve.
3. When using image generation/editing, follow the image-generation skill and describe the input as an edit target.
4. Require preservation of face, anatomy, pose, clothing, linework, logos, palette, and texture.
5. Make one layout change per generation attempt. Do not simultaneously restyle or beautify the subject.
6. Generate or edit above final resolution, then perform the exact final crop and downsample deterministically.
7. Apply color adaptation after composition is stable so crop and subject placement do not invalidate the sampled palette or clock-zone contrast.

Use neutral prompt language focused on layout. Example:

> Edit the supplied non-explicit illustration for a 46mm Apple Watch Photos-face wallpaper. Preserve the adult character and artwork unchanged. Extend only the off-white background, scale the unchanged artwork slightly smaller, place it lower-right, and reserve a broad low-detail area on the left for watchOS time. No text, numbers, clock, UI, device frame, or watermark.

Never bake a clock, date, complications, Apple Watch bezel, Digital Crown, or fake UI into the clean wallpaper.

## Export exact deliverables

Export exactly one clean wallpaper per request unless the user explicitly asks for variants. Use exact final dimensions:

- `42mm/wallpaper-clean-*.png`: 374×446 px
- `46mm/wallpaper-clean-*.png`: 416×496 px
- `42mm/preview-clock-overlay-*.png`: optional on request, 374×446 px
- `46mm/preview-clock-overlay-*.png`: optional on request, 416×496 px
- `safe-zone-guide-*.png`: optional on request, same dimensions as its target

Include the chosen layout and color intent in the filename when relevant, for example `wallpaper-clean-clock-left-clock-dark.png`. Do not export both color treatments unless the user explicitly requests comparison variants.

Use PNG for illustrations, gradients, line art, or transparency-free graphic work. Use high-quality JPEG only when the source is photographic and the smaller file is beneficial.

Keep the high-resolution working file only when it is useful for revision. Never substitute the 4× working canvas for the exact final export.

## Validate every final file

Inspect the clean wallpaper and preview at full size and thumbnail size. Verify programmatically where possible:

- Dimensions equal exactly 374×446 or 416×496.
- Color mode is RGB or RGBA and the file opens without errors.
- No face, eye, hand, logo, or emblem intersects time, date, or complications.
- The widest expected time fits inside the reserved region.
- Rounded corners do not clip important details.
- Bright and dim/Always-On previews remain readable when relevant.
- Clock previews meet the intended local contrast target in both normal and dim simulations.
- Protected colors remain visually unchanged unless the user explicitly requested recoloring.
- Background harmonization derives from the current image rather than a hardcoded palette.
- Subject edges remain distinct without halos, cutout artifacts, or mask spill.
- The clean wallpaper contains no clock, accidental numbers, fake UI, watermark, or watch hardware.
- Existing-image edits preserve identity and do not introduce anatomy or clothing changes.

Ask the user to apply the wallpaper in the Watch app and send a screenshot. If watchOS reframes it, revise from that screenshot rather than guessing.

## Decision rules

- Support only latest-Series 42mm and 46mm.
- Prefer the user's Watch app preview over all assumptions.
- Prefer exact pixel exports plus normalized layout masks.
- Prefer deterministic geometry over generative redrawing.
- Default to content-aware `harmonize`; use `preserve` whenever fidelity or mask confidence is more important than color grading.
- Adjust background and clock zone before modifying the subject.
- Ask the user to choose `clock-dark` or `clock-light` when clock color uncertainty affects readability; do not generate both automatically.
- Ask the user to choose one layout when the time location is unknown; never generate all layout options automatically.
- Produce exactly one clean wallpaper after the selection unless the user explicitly requests multiple variants.
- Keep clean wallpaper and clock-overlay preview separate; deliver the preview only when requested.
- Stop and report unsupported hardware instead of approximating.

## Official specification sources

Recheck official Apple sources when “latest” may have changed:

- https://www.apple.com/apple-watch-series-11/specs/
- https://www.apple.com/watch/compare/
- https://support.apple.com/guide/watch/apd501eb33b3/watchos
