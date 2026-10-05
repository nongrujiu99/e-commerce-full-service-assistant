---
name: e-commerce-full-service-assistant
description: 淘宝电商全案助手，用于淘宝电商策划、详情页设计、主图制作、详情页+主图全套出图。Use when creating Taobao ecommerce detail pages, main images, PDP image packs, product-photo-to-detail-page frameworks, or direct image generation from product photos. Especially useful for buyer-demand-led detail pages, full-category product strategy, standard versus non-standard product differentiation, reference-led style confirmation, scene-based high-density layouts, direct selling-point copy, product-consistency and physical-logic locks, direct built-in image_gen generation, classified folder delivery with planning documents, saving generated files locally, and displaying generated images in the chat. 触发词：淘宝电商全案、详情页、主图、淘宝电商策划、详情页+主图
---

# 电商全案助手

Turn product images, selling points, reference pages, and platform requirements into demand-led ecommerce detail page images. Plan from buyer needs first, choose the purchase decision type, create a concrete page strategy and module plan, match product selling points, build scene-based visual proof, lock one style system, lock product identity and physical logic, then generate images directly by default instead of stopping at prompt output.

Core formula: buyer demand ranking -> purchase decision type -> page strategy -> module planning -> product selling-point match -> page task allocation -> scene evidence expression -> unified visual system -> product identity and physical-logic lock.

## Global Priority Order

When this skill or its reference files contain rules that appear to conflict, interpret and execute them in this priority order:

1. User's explicit request, including requested quantity, platform, size, language, style direction, and module order.
2. Platform compliance and legal safety: do not fabricate claims, certifications, test data, sales, reviews, medical effects, or unsupported parameters.
3. Product truth and product identity: preserve visible product color, structure, material, decoration, proportions, logo/nameplate placement, and physical plausibility.
4. Module Plan count and page strategy: generate one image for each planned module unless the user explicitly specifies another count.
5. Buyer-demand task for the current module: each image must solve its assigned buyer question with one matched selling point and visual proof.
6. Detail-page design strength: avoid plain product posters, repeated templates, empty backgrounds, and unchanged product scale across the set.
7. Reference visual style and creative variation: adapt style, mood, rhythm, and composition devices without copying exact layouts or locking the source photo composition.
8. Optional QA and review notes: disclose issues without automatic regeneration unless the user explicitly asks for regeneration or replacement.

If two same-level rules conflict, choose the option that is more truthful, more compliant, and more aligned with the current module's buyer-demand task.

Direct-generation boundary: every delivered detail-page image must be generated directly by built-in `image_gen`. Never create delivered images through local stitching, local layout assembly, local text overlay, local background replacement, local product/background compositing, or local crop-and-recombine workflows. If a prompt asks for a collage-style visual device, the complete collage-style module must be generated in one image-generation call, not assembled locally.

## Image Interface Order

- Use built-in `image_gen` directly for all final image generation.
- When generating multiple final images, submit them as independent `image_gen` calls; issue independent calls in parallel where the runtime supports it, then wait for all results.
- Do not use any external deployment helper, PowerShell script, environment-variable-resolved generator, or direct HTTP/SDK image API for final image generation.
- Every generated image must be saved to a local file path and displayed in the chat with Markdown image syntax.

## Core Workflow

## Audit / Regeneration Override

- Disable automatic audit-and-regenerate behavior. After direct image generation succeeds and the files are saved, proceed to delivery; do not regenerate automatically because of quality checks, text issues, layout issues, design strength, or product-consistency concerns.
- Do not require reading `references/quality-control.md` after generation. That file is only an optional manual review checklist and must not trigger automatic regeneration, automatic rejection, or automatic replacement of delivered images.
- If a potential issue is noticed, mention it briefly in the final response under `Optional Review Items / Items Needing Confirmation`. Do not regenerate unless the user explicitly asks to regenerate, redo, re-create, or fix a specific image.
- This override has higher priority than any older rule in this skill or its references that implies mandatory QA, mandatory regeneration, final qualified versions, or regeneration notes.

1. Classify the input:
   - Product images only: start with `Product Image Analysis` and create `Product Consistency Anchors`.
   - Product information is provided: move directly into buyer-demand planning.
   - Reference detail pages are provided: first analyze page roles, information hierarchy, visual devices, and rhythm.
2. Read [references/category-router.md](references/category-router.md) to identify the category, then load only the relevant category structure file.
3. Read [references/buyer-demand-strategy.md](references/buyer-demand-strategy.md) and create `Information Confidence`, `Buyer Demand Map`, `Demand-to-Selling-Point Match`, and `Product Type Strategy`.
4. Read [references/page-planning-strategy.md](references/page-planning-strategy.md) and create `Purchase Decision Type`, `Page Strategy`, and `Best Hero Direction`. Do not create the final `Module Plan` yet.
5. Read [references/style-confirmation.md](references/style-confirmation.md). Present three style cards and three independent 3:4 preview images based on the same product, angle, core selling point, and copy. If style references are provided, create high-reference, optimized-reference, and extended-reference variants. Pause for the user's selection or adjustment before continuing.
6. Read [references/style-system-standard.md](references/style-system-standard.md) and [references/design-strength-system.md](references/design-strength-system.md). Convert the approved direction into `Campaign Style Lock`, `Style System Lock`, and `Design Strength Lock`.
7. Return to [references/page-planning-strategy.md](references/page-planning-strategy.md) and create the final `Module Plan` and `Visual Rhythm Plan` using the approved style.
8. Read [references/page-task-table.md](references/page-task-table.md) and create a screen-by-screen `Page Task Table` from the planned modules before prompt writing. The number of final generated images must follow the `Module Plan` count unless the user explicitly specified a different count.
9. Read [references/scene-layout-standard.md](references/scene-layout-standard.md) and [references/product-consistency-physics.md](references/product-consistency-physics.md). Create `Scene Layout Plan` and `Product Identity & Physics Lock`.
10. Read [references/anti-template-check.md](references/anti-template-check.md) and revise the page plan before generation if the set collapses into a repeated template.
11. Read [references/prompt-contract.md](references/prompt-contract.md) to assemble each detail-page prompt. Add the same product-consistency anchors, `Module Plan`, `Demand-to-Selling-Point Match`, `Page Task Table`, approved `Style System Lock`, `Design Strength Lock`, and `Product Identity & Physics Lock` to every prompt.
12. When the user requests a full ecommerce image set, also plan main images (主图): 5–8 images at 1200×1200px, 1:1 ratio by default. Main-image roles: full product hero, front/back/side angles, key detail close-up, usage or lifestyle scene, size/scale reference, and packaging or included-items shot. Keep main-image copy minimal.
13. After style approval, when the user asks for images, generation, a version, or direct visual output, generate images directly by default without asking for another proceed confirmation.
14. For direct image generation, read [references/generation-tools.md](references/generation-tools.md) and use built-in `image_gen` directly. Generate detail-page screens at 1504px width with a legal 16-pixel-multiple height: normally 2256px, 2496px for high-information screens, or 2992px for extra-long screens. Main images remain 1200×1200px (1:1) by default. Do not use any external deployment helper or direct HTTP/SDK image API. Do not enter an automatic QA or regeneration loop.
15. Handle high-risk claims about efficacy, certifications, sales, reviews, testing, or brand authorization conservatively according to [references/compliance.md](references/compliance.md).

## Hard Rules

- Default detail-page screen count: 8–10 screens for standard products; 10–14 screens for complex products with enough confirmed information and distinct buyer questions. Reduce only when the user requests fewer images. Final generation count must follow the `Module Plan`. If the user specifies a quantity, plan and generate exactly that quantity.
- Every detail-page screen must use a fixed width of `1504px`. Choose a legal height that is divisible by 16: `2256px` for ordinary screens, `2496px` for high-information screens, or `2992px` for extra-long screens. Never exceed `2992px`. The first line of every detail-page prompt must state the exact selected dimensions, for example `1504×2256px ecommerce detail page screen, portrait orientation`. Screens within one set may use different approved heights but must share the same 1504px width.
- Main images (主图) use `1200 × 1200 px` with a `1:1` aspect ratio by default. Recommend 5–8 main images per product. Main images focus on product presentation: full product hero, multiple angles, key detail close-ups, usage scene, and size/scale reference. Main images may carry minimal selling-point labels but must not become detail-page-style information screens.
- Apply the same production dimensions to every supported platform: Taobao / Tmall, JD, Pinduoduo, Douyin ecommerce, and Xiaohongshu. Platform choice changes content emphasis, copy direction, information density, scenes, and visual strategy only; it does not change the default dimensions. Treat these as this skill's unified production specifications, not as each platform's official upload specifications.
- User-specified dimensions or aspect ratios override the defaults for the current task only. If the user says only `3:4 main image`, use `1200 × 1600px`, portrait orientation. A main-image override does not change detail-page dimensions unless the user explicitly requests it. Do not permanently change the global default from a one-task override.
- When the user requests a full ecommerce image set, generate both main images (5–8 images at 1200×1200px, 1:1 by default, or the user-specified override) and detail-page screens (1504px wide using an approved height of 2256px, 2496px, or 2992px; 8–10 or 10–14 screens) unless the user explicitly asks for only one type.
- Every screen must contain a main title plus at least one selling-point label or short phrase. Pure visual pages with no meaningful copy are not acceptable.
- Each screen must solve one buyer demand with one core selling point. Material, ingredient, craft, detail, and scene pages may omit the full product, but they still need corresponding selling-point copy and visual proof.
- On-image copy must be direct and short: clear main title, one selling-point phrase, and minimal auxiliary text. Do not use long paragraphs or meaningless decorative English.
- Detail-page planning must start from buyer demand ranking and product type strategy.
- Standard / functional products emphasize utility, structure, operation, efficiency, comparison, and confirmable parameters.
- Non-standard / aesthetic products emphasize design, beauty, material, style fit, pairing, and atmosphere.
- Consumables emphasize taste, texture, use frequency, preparation, packaging convenience, and provided or visible ingredient information.
- Gift or emotional-value products emphasize packaging, ceremony, recipient scenario, display value, and atmosphere.
- Detail pages must not repeat product main images. A default eight-image set must mix at least five page structures; when the `Module Plan` expands to 9-12 modules, keep at least five structures and avoid repeating any one structure more than twice.
- Strong design is mandatory. Every set must define a `Design Strength Lock` and `Style System Lock` first and include at least five clearly different visual roles, at least three composition scales, and at least two memorable visual devices.
- Default layout density is high-density scene-based composition: no meaningless blank space, clear foreground / midground / background hierarchy, readable copy, and scene evidence that supports the highlighted selling point.
- Do not make the complete product the hero on every page. Crops, ingredients, craft, scenes, comparisons, and information cards may each be the main subject of a screen.
- Do not let the full set collapse into the template of same-color gradient background, large top-left title, centered product, and small labels.
- Final images must be generated directly by built-in `image_gen`. Do not use local stitching, local post-processing, text overlay, collage assembly, background replacement, product/background compositing, or crop-and-recombine workflows to create final delivered images. Do not use any external deployment helper, PowerShell script, or direct HTTP/SDK image API.
- Built-in `image_gen` is the sole generation path for all final detail-page images.
- Base product information only on user-provided content and visible facts in the images. Mark reasonable assumptions as `needs confirmation`. `Product Consistency Anchors` lock product identity, not the source photo's composition.
- Product consistency and physical space logic are mandatory: no product drift, no random accessories, no structure deformation, no impossible contact, no contradictory shadows, no wrong scale, and no floating products.
- For apparel, footwear, bags, or accessories with models, prioritize keeping the same person identity, body type, hairstyle, and styling logic. When model display is needed, prefer full-body context.
- Do not use consultation-style CTA copy. Action or closing pages should start from buyer needs and use non-consultation expressions such as daily-use fit, easier pairing, or practical purchase reasons.

## Product Consistency Anchors

When product images are provided, extract and reuse one shared set of product-consistency anchors before generation:

- Overall silhouette: shape, proportions, open/closed state, and major volume relationships.
- Color and material appearance: primary color, secondary color, visible textures such as transparent, metallic, textile, or creamy surfaces, and the relative color-area ratio.
- Pattern, logo, and nameplate: describe only visible position, direction, size relationship, and visual placement. Do not invent unreadable small text.
- Structural parts: positional relationships between handles, knobs, caps, ports, zippers, hang tags, bases, decorative hardware, and other key components.
- Decorative elements: relative positions of lace, patches, patterns, graphics, accessories, and local details.
- Crop-to-whole relationship: detail pages may only enlarge real parts from the original product, not generate a different style from the same category.
- Physical relationship: expected product scale, support surface, contact points, occlusion, shadow direction, and scene-object relationships.

The same detail-page set must reuse the same `Product Consistency Anchors`; do not improvise product identity separately for each image. These anchors preserve product identity only, not the original photo's background, camera angle, crop, product placement, lighting setup, or white-background single-product composition. Backgrounds, compositions, angles, crops, model presentation, scenes, and page structures should change according to the page role, while core product-identifying traits must not change.

If an uploaded product reference is a white-background single-product photo, every generation prompt must explicitly say: `Use the reference image only to lock product identity. Do not replicate the white-background product photo as a white-background single-product image. Create a designed ecommerce detail-page composition with buyer-facing copy and visual proof.`

## Product Image Analysis

When only product images are provided, output first:

- Visible facts: category, color, material appearance, structure, packaging, accessories, SKU, and visible copy.
- Reasonable assumptions: keep them conservative and mark each as `needs confirmation`.
- Invisible information: do not present it as fact.
- Information confidence: classify candidate selling points as `confirmed`, `reasonable inference`, or `needs confirmation`.
- Product type strategy: identify whether the product is mainly standard / functional, non-standard / aesthetic, consumable, gift / emotional-value, or mixed.
- Detail-page directions to develop: six to ten likely buyer questions or concerns.

Treat only information visibly readable on packaging as packaging-visible information. Do not expand it into efficacy claims.

## Reference Page Analysis

When the user provides reference detail pages, analyze five things first:

1. Page type: cover, concept, ingredient, craft, colorway, detail, scene, or summary.
2. Information hierarchy: main title, bullets, explanatory copy, decorative symbols, buttons, or labels.
3. Visual devices: glass frames, hanging display, collage, detail zoom windows, bottom information bands, or technical annotation lines.
4. Rhythm: which pages show the full product and which do not.
5. Reusable logic: learn the structure, but do not copy wording, brand elements, or exact layout details.

## Output Format

For direct image-generation tasks, provide a brief setup summary in chat, then deliver all files in a classified product folder. Do not create a ZIP unless the user explicitly requests one in that task.

### Chat Setup Summary (brief)

1. `Product Image Analysis` when only product images are provided.
2. `Strategy Summary`: brief overview of Information Confidence, Buyer Demand Map, Product Type Strategy, Purchase Decision Type, Page Strategy, and Best Hero Direction.
3. `Style Confirmation`: three style cards and three independent 3:4 preview images; record the selected direction and requested adjustments before final module planning.
4. `Module Count Confirmation`: detail pages 8–10 screens (standard) or 10–14 screens (complex); main images 5–8 at 1200×1200px by default or the user-specified override.

### Planning Document (single MD file)

Generate one Markdown file named `策略规划.md` containing the full planning details. This file consolidates all strategy, style, locks, and task tables:

- `Information Confidence`
- `Buyer Demand Map`
- `Demand-to-Selling-Point Match`
- `Product Type Strategy`
- `Purchase Decision Type`
- `Page Strategy`
- `Best Hero Direction`
- `Reference Style Analysis` when style references are provided
- `Approved Style Direction` and adjustment record
- `Module Plan` (screen-by-screen with all module fields)
- `Main Image Plan` (when a full set is requested: 5–8 images at 1200×1200px, 1:1 by default, or the user-specified override, with roles for each)
- `Visual Rhythm Plan`
- `Campaign Style Lock`
- `Style System Lock`
- `Design Strength Lock`
- `Product Identity & Physics Lock`
- `Page Task Table` (full 22-column control table)
- `Scene Layout Plan`

### Final Delivery (classified folder)

Organize all output files into one classified product directory. Do not package it as a ZIP unless the user explicitly requests a ZIP for the current task. Directory and naming convention:

```
<产品名>_电商全案_<YYYYMMDD>/
├── 00_风格确认/
│   ├── style-A.png
│   ├── style-B.png
│   ├── style-C.png
│   ├── 参考图风格分析.md
│   └── 最终风格确认.md
├── 01_主图/
│   ├── main-01.png   (1200×1200px, 1:1 by default, or user override)
│   ├── main-02.png
│   └── ...            (5–8张)
├── 02_详情页/
│   ├── detail-01.png (1504px宽, 单屏高为2256/2496/2992px)
│   ├── detail-02.png
│   └── ...            (8–10屏 或 10–14屏)
└── 03_规划文档/
    ├── 策略规划.md
    ├── 完整Prompt.md
    └── 交付检查记录.md
```

Rules for folder delivery:
- Save all three style previews and style-confirmation records to `00_风格确认/`.
- Save each generated final image to its classified directory (`01_主图/` or `02_详情页/`).
- Save `策略规划.md`, `完整Prompt.md`, and the optional manual `交付检查记录.md` to `03_规划文档/`.
- Keep the classified directory intact. Do not create a ZIP by default.

### Final Response

- List the classified product-folder local absolute path.
- Also list the classified directory structure and file count.
- Display generated images in the chat with Markdown image syntax using the saved local file paths (show both main images and detail-page previews).
- Include `Delivery Notes / Optional Review Items`.
- Do not only say that generation is complete.
