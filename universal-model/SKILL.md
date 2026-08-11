---
name: design-apple-watch-wallpaper-universal
description: Create or edit Photos-face wallpapers only for the latest Apple Watch Series 42mm (374×446 px) and 46mm (416×496 px) using a strict confirmation-gated workflow designed for general or lightweight multimodal models. Use when a model must confirm hardware, layout, clock color, color treatment, subject protection, and single-output requirements before editing an image. Do not use for Ultra, SE, older Apple Watch sizes, other watches, or watch-face packages.
---

# Create Apple Watch Wallpaper with Confirmation Gates

Create one clean Photos-face wallpaper only after every required boundary is confirmed. Follow the gates in order. Never skip a gate, infer a missing choice, or generate all options.

## Gate 1 — Confirm supported hardware

Accept exactly one target:

- Latest Apple Watch Series 42mm: export exactly 374×446 px.
- Latest Apple Watch Series 46mm: export exactly 416×496 px.

Reject Apple Watch Ultra, Apple Watch SE, 40/41/44/45/49mm models, other watch brands, and custom watch-face packages. State that this Skill does not support them. Do not approximate.

If the size is missing, ask: “请确认型号：最新款 Apple Watch Series 42mm，还是 46mm？” Do not continue until the answer is exactly 42mm or 46mm.

## Gate 2 — Confirm the source image

Require one readable source image. Identify the primary subject, protected details, adjustable background, and thin structures that must not be cut off. If no image is attached, ask the user to upload it. If the important subject is ambiguous, ask the user to name it.

## Gate 3 — Confirm one time layout

Accept exactly one:

1. clock-left: reserve the left; place the subject toward the right.
2. clock-right: reserve the right; place the subject toward the left.
3. clock-top: reserve the top; place the subject lower.

If missing, ask: “请选择一个时间布局：1）左侧时间，主体靠右；2）右侧时间，主体靠左；3）顶部时间，主体靠下。” Never create all layouts automatically.

## Gate 4 — Confirm clock color

Accept exactly one:

- clock-light: darken and calm the background behind light clock glyphs.
- clock-dark: brighten and calm the background behind dark clock glyphs.

If missing, ask: “请确认时钟文字颜色：浅色还是深色？” Do not place clock glyphs in the clean wallpaper.

## Gate 5 — Confirm one color mode

Accept exactly one:

- preserve: keep subject colors unchanged; adjust only background and clock zone.
- harmonize: derive colors from the current image; subtly coordinate background and adjustable subject.
- contrast: increase subject/background separation while preserving protected colors.

If missing, propose preserve and tell the user. Never silently recolor the subject or reuse a fixed palette. Never hue-shift skin, eyes, logos, emblems, deliberate accent colors, or product finishes.

## Gate 6 — Show a boundary summary and wait

Before editing, show:

> 输出确认  
> - 型号与尺寸：[…]  
> - 时间布局：[…]  
> - 时钟颜色：[…]  
> - 色彩模式：[…]  
> - 必须保护的主体细节：[…]  
> - 输出数量：1 张干净壁纸  
> - 壁纸内不含：时间、日期、数字、UI、表壳、水印  
> 请回复“确认”后开始。

Wait for explicit confirmation such as “确认”“可以” or “开始”. If any item changes, update the checklist and ask for confirmation again.

## Compose after confirmation

1. Preserve identity, face, anatomy, pose, clothing, hairstyle, linework, texture, and protected colors.
2. Prefer crop, scale, position, padding, and background extension over redrawing.
3. Keep faces, eyes, hands, logos, emblems, and named details outside the clock zone and rounded corners.
4. Keep low-detail negative space behind the clock; avoid highlights, hard edges, facial features, and small patterns.
5. Extend only the background when more room is needed.
6. Keep natural gaze and movement space.

For harmonize, limit subject exposure changes to about ±5% and chroma changes to about ±8%. If foreground separation is unreliable, fall back to preserve and report it.

## Export exactly one file

Export one clean image only: 374×446 px for 42mm or 416×496 px for 46mm. Use PNG for illustration, line art, gradients, or graphic work. Use high-quality JPEG only for photographs when useful.

Never add a clock, date, numbers, complications, Apple Watch frame, Digital Crown, fake UI, signature, or watermark. Do not output previews or guides unless requested after the clean wallpaper.

## Final verification gate

Verify exact dimensions, one output, readable RGB/RGBA file, protected-detail clearance, rounded-corner safety, clock-zone contrast, preserved identity/style/colors, and absence of unintended text, UI, hardware, or watermark.

Fix every failed check before delivery. If the model cannot export exact dimensions, say so clearly and provide the composed image for deterministic resizing instead of claiming success.
