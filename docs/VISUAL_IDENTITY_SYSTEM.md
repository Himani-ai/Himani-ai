# Visual Identity System & Design Specifications
### Laboratory Aesthetic & Scientific Branding Standards

---

## 1. Palette Specification & Design Tokens

To sustain a Microsoft Research / DeepMind aesthetic, all profile graphics, banners, charts, and diagrams must strictly adhere to the following color tokens:

```
┌────────────────────────────────────────────────────────────────────────┐
│                        CORE LABORATORY PALETTE                         │
├───────────────────┬─────────────┬──────────────────────────────────────┤
│ Token Name        │ Hex Code    │ Strategic Semantic Role              │
├───────────────────┼─────────────┼──────────────────────────────────────┤
│ Deep Void         │ #030712     │ Canvas Background / True Black Space │
│ Substrate Slate   │ #0B0F17     │ Cards, Terminal Blocks, Tables       │
│ Electric Cyan     │ #00F2FE     │ Primary Headers, Brand Accents       │
│ Hyper Blue        │ #00D2FF     │ Systems Links, Metric Focus, Badges  │
│ Quantum Violet    │ #8B5CF6     │ Materials Science, Nanotechnology    │
│ Signal Emerald    │ #10B981     │ Validation Passed, Published Status  │
│ Ghost Silver      │ #94A3B8     │ Structural Subtext & Metadata        │
└───────────────────┴─────────────┴──────────────────────────────────────┘
```

---

## 2. Banner Asset Generation Prompts (Midjourney / DALL-E 3 / Flux)

Generate widescreen banners with aspect ratio `21:9` or `16:9` ($2400 \times 800\text{ px}$) using these prompts:

### Banner 1: Master Profile Header (Autonomous Systems & Physics)
```text
Futuristic laboratory banner, abstract intersection of autonomous distributed network nodes and nanoscale molecular crystals, deep pitch-black background (#030712), electric cyan (#00F2FE) and neon blue geometric lattice lines, subtle glowing purple energy lines, minimalist technical blueprints, clean mathematical aesthetic, USENIX NSDI / DeepMind research style, 8k resolution, cinematic lighting, ultra-sharp vector rendering, no text, no gaming aesthetics --ar 21:9 --style raw
```

### Banner 2: ASON Flagship Repository Banner
```text
Technical systems architecture banner, self-organizing mesh network graph with crystalline nodes dynamically connecting and healing, deep void black backdrop (#030712), vibrant electric cyan vectors, spectral graph Laplacian visualization, mathematical network topology, Bell Labs research publication style, clean line art, high tech minimalism, 8k --ar 21:9 --style raw
```

### Banner 3: $\text{TiO}_2\text{-Ppy}$ Nanocomposites Repository Banner
```text
Scientific materials science banner, titanium dioxide nanocrystals interlaced with conductive polypyrrole polymer strands, subtle organic botanical leaf vein geometry merging with synthetic crystal lattices, dark carbon background, deep indigo and emerald phosphorescence, scanning electron microscopy visualization style, ACS Nano cover aesthetic, photorealistic 8k render, no text --ar 21:9 --style raw
```

### Banner 4: SDN OpenFlow Sentinel Banner
```text
High-speed data network packet routing pipeline, wireframe telemetry switches, streaming data flows in electric blue and teal light pulses, Linux kernel eBPF probe aesthetics, minimalist server topology, dark background, IEEE INFOCOM aesthetic, ultra-precise architectural rendering --ar 21:9 --style raw
```

---

## 3. Typography & Mathematical Rigor Guidelines

1. **Inline Math:** Use `$...$` for scalar variables ($x$), vectors ($\mathbf{v}$), matrices ($L$), and physical quantities ($E_g = 2.85\text{ eV}$).
2. **Display Equations:** Format complex mathematical theorems and matrix operations on independent lines enclosed in `$$...$$`.
3. **Diagrams:** Use native GitHub Mermaid syntax (`graph TD`, `graph LR`) with explicit `classDef` styling to ensure dark-mode consistency across all devices.
4. **No Low-Signal Decorations:** Never use animated meme gifs, cartoon clip-art, or generic celebratory badges. Every graphic element must convey technical or scientific information.
