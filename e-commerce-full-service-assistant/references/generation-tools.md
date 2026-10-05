# Generation Tools

Read this file for direct image generation. Unless the user explicitly asks for prompts only, do not stop at a prompt pack.

## Audit / Regeneration Override

- Disable automatic audit-and-regenerate behavior. Once generation succeeds, files are saved to local paths, and necessary file-existence checks pass, proceed to delivery.
- Do not automatically rewrite prompts or regenerate images because of text errors, weak design, product drift, imperfect dimensions, or repeated page structures.
- If potential issues are noticed, list them only in the final response as `Optional Review Items / Items Needing Confirmation`. Call the generation tool again only when the user explicitly asks to regenerate, redo, re-create, or fix a specific image.
- This section overrides any older rule below that implies post-QA regeneration, final qualified versions, keeping only qualified outputs, or excluding problem images automatically.

## Generation Path

- Use built-in `image_gen` directly for all final image generation. This is the sole generation path.
- Do not use any external deployment helper, PowerShell script, environment-variable-resolved generator, or direct HTTP/SDK image API for final image generation.
- All detail-page screens use a fixed width of `1500px` and per-screen adaptive height no greater than `3000px` (typically 1500–2500px based on content). Every detail-page prompt must include `1500px width, height ≤3000px ecommerce detail page screen, portrait orientation`. Screens within one set may have different heights but must share the same 1500px width.
- Main images (主图) use `1200 × 1200 px` with a `1:1` square aspect ratio by default. Recommend 5–8 main images per product. Every default main-image prompt must include `1200×1200px, 1:1 square ecommerce main image`. Main images focus on product presentation with minimal copy.
- Use these default dimensions for every supported platform. Platform selection changes content emphasis and visual strategy only, not dimensions.
- An explicit user-specified size or ratio overrides the default for the current task. If the user says only `3:4 main image`, use `1200×1600px`, portrait orientation. Do not apply a main-image override to detail pages unless explicitly requested.
- Every detail-page image must have a main title and at least one selling-point label or short phrase. Do not generate pure visuals without selling-point copy.
- Final images must be generated directly by built-in `image_gen`. Do not use local scripts to stitch images, overlay text, make collages, replace backgrounds, composite final images, or create placeholder/local-composited images.

## Save Output Rule

- Create a classified output directory structure before saving:
  - `00_风格确认/` for the three style preview images and style-confirmation records
  - `01_主图/` for main images (1200×1200px, 1:1 by default, or the user-specified override)
  - `02_详情页/` for detail-page screens (1500px wide, height ≤3000px)
  - `03_规划文档/` for the planning Markdown file
- Save style previews as `00_风格确认/style-A.png`, `style-B.png`, and `style-C.png`; save `参考图风格分析.md` when references are provided and always save `最终风格确认.md`.
- Save each generated main image to `01_主图/main-01.png`, `main-02.png`, etc.
- Save each generated detail-page screen to `02_详情页/detail-01.png`, `detail-02.png`, etc.
- After all images are generated and saved, generate `策略规划.md`, `完整Prompt.md`, and the optional manual review record `交付检查记录.md` in `03_规划文档/`.
- The final response must list the local path for the classified product folder and each classified directory; do not only say that generation is complete.
- In chat surfaces that support image display, show generated images with Markdown image syntax: `![description](absolute local path)`.
- If the local file cannot be confirmed, do not claim image generation is complete. Explain the failure reason and keep the executable prompt available.

## Multi-Image Generation

- One detail-page image corresponds to one independent prompt.
- One detail-page image corresponds to one independent `image_gen` call.
- Do not generate multi-screen collages in one request.
- Do not place multiple detail pages on the same canvas.
- When generating multiple final images, submit them as independent `image_gen` calls; issue independent calls in parallel where the runtime supports it, then wait for all results.
- Use one independent output filename per image. Do not share one output filename across calls.

## Request Shape

For detail-page screens, set `width` to 1500 and choose `height` per screen based on content (1500–3000px, never exceeding 3000). If the tool does not support exact pixel dimensions, strongly constrain the prompt with `1500px width, height ≤3000px ecommerce detail page screen, portrait orientation, consistent 1500px width across the full set`. For default main images, set both `width` and `height` to 1200 for a 1:1 square ratio. If the user specifies a different size or ratio, use that instruction for the current task; `3:4 main image` without pixel dimensions means 1200×1600px portrait.

When product images are provided, prefer using the original image as a product-identity reference input through `image_gen` (reference image mode). The reference image locks product identity only; it must not lock the source photo background, camera angle, crop, placement, lighting setup, or plain white-background product-photo composition. If reference images are unsupported by the current runtime, write the same `Product Consistency Anchors` / `Product Identity Lock` into every prompt and explicitly require consistency in visible product appearance, pattern/logo/nameplate placement, shape, proportions, structural parts, and relative position.

If the reference input is a white-background single-product photo, every prompt must also include: `Do not replicate the white-background reference as a white-background single-product image; create a designed ecommerce detail-page image with a new composition, buyer-facing headline, selling labels, and visual proof.`

Before direct image generation, read `design-strength-system.md` and add the `Design Strength Lock` to every prompt. Do not rely only on generic adjectives such as clean, premium, or modern.

## Response Handling

The `image_gen` tool may return any of the following:

- Absolute local path: confirm the file exists and use it in final delivery.
- Markdown image tag: include it in the final response if the local file exists.
- `url`: download the image and save it to a local output directory.
- `b64_json`: decode it into an image file and save it.

The final response must list generated file paths. If saved files cannot be confirmed, do not claim that image generation is complete.

When displaying images, use absolute paths, for example: `![01](/path/to/01.png)`.

## Allowed Post-Processing

Allowed:
- Download, copy, or rename generated files.
- Check dimensions, file existence, and image count.

Not allowed:
- Create contact sheets, stitched previews, image grids, comparison boards, or any locally assembled image output.
- Use local scripts to overlay final-image copy.
- Use local scripts to assemble product, background, and labels into a final image.
- Use local scripts to stitch multiple generated images into one deliverable or preview image.
- Crop and recombine multiple generated images into a final detail page.
- Treat an image with post-edited text fixes as a qualified final image.

Collage-style layouts are allowed only when the full collage-style detail module is produced directly by `image_gen` in one generation request. They must not be assembled from local image pieces.

If there are text errors, weak design, or product drift, do not automatically rewrite prompts or regenerate the affected image. Record the issue only as an optional review item unless the user explicitly asks for regeneration.

## Final Delivery Set Rule

- Images that are successfully generated and saved are the delivered images for this run.
- After all images and planning documents are saved and verified, keep them in one classified product folder. Do not create a ZIP unless the user explicitly requests one for the current task.
- Product-folder name format: `<产品名>_电商全案_<YYYYMMDD>` (use a short product identifier if the product name is unavailable).
- Folder structure:
  ```
  00_风格确认/style-A.png, style-B.png, style-C.png, 最终风格确认.md
  01_主图/main-01.png, main-02.png, ... (5–8张)
  02_详情页/detail-01.png, detail-02.png, ... (8–10或10–14屏)
  03_规划文档/策略规划.md, 完整Prompt.md, 交付检查记录.md
  ```
- The final response lists the product-folder local absolute path, the classified directory structure and file count, and shows Markdown image previews.
- Do not use final qualified version, problem image, or rejected image as an automatic filtering mechanism.
- If the user later asks to regenerate a specific image, generate the new version within the user-specified scope, replace the corresponding file in the classified directory, and update the delivery list.

## Failure Handling

Deliver a prompt pack without images only when built-in `image_gen` has been attempted and cannot produce saved image files:
- `image_gen` is unavailable, fails, cannot reach the network, or cannot save/display the result.

When failure occurs, explain the failure where available, such as unavailable built-in image tool, built-in image tool error, or unreachable network. Do not describe prompt-only output as a completed image-generation task.
