# STOA — A Cinematic Digital Myth
### Creative Direction & Production Specification, v1.0

STOA is not a website. It is a six-act film the visitor scrolls through, in which an ethereal dragon — the spirit of transformation — is born from light, carries the brand's story through realms of color, and ascends. Every decision below serves that single narrative.

---

## 1. Creative Direction

The experience is built on one tension: **spiritual futurism against luxury minimalism**. The dragon provides the spiritual, mythic energy; the typography, spacing and restraint provide the luxury. Nothing on screen exists without narrative purpose. The visitor is never "browsing a site" — they are moving through the dragon's world, and the agency's services are encountered the way artifacts are encountered in a film: lit, isolated, inevitable.

The emotional arc across the scroll is: wonder → intimacy → clarity → heat → power → calm. Color, light, motion and copy all follow this same curve, which is what makes the experience feel directed rather than assembled.

## 2. Experience Concept

A single continuous WebGL scene persists behind the entire page. There are no page loads, no hard cuts; the world *changes state* as the visitor scrolls. The dragon is the protagonist and the only persistent object — backgrounds, particles and light fields dissolve around it. The visitor's cursor (or finger) is a second light source the world subtly responds to, making the experience feel aware of them.

## 3. UX Structure

Six realms, each one full viewport, in fixed order: Hero (birth), Philosophy (the quote realm), Services (the crystal realm), Story (the fire realm), Ascension (the golden realm), Contact (the lilac dawn). A persistent frosted-glass navigation bar holds three anchors — Philosophy, Services, Contact — so the visitor can never be lost; the scroll indicator in the hero teaches the single interaction the site requires. Conversion surfaces ("Awaken the Dragon", the contact email) stay fixed and reachable at all times.

## 4. UI System

Three layers, strictly ordered: the WebGL world (z0), atmosphere — grain and glow (z1–2), and the interface (z10+). Interface elements are exclusively liquid glass: `backdrop-filter: blur(18px) saturate(160%)`, 1px white-alpha borders, inner top highlight to simulate a polished glass edge, soft 32px ambient shadows. The UI inverts between a dark-ink theme (light realms) and frost-white theme (dark realms) with an 800ms color crossfade — the interface itself participates in the lighting of the film.

## 5. Motion Language

All motion obeys three principles. **Inertia:** nothing starts or stops instantly; scroll progress and pointer position are smoothed through exponential interpolation (factor 0.05–0.06) so the world feels like it has mass. **Breath:** idle elements oscillate on slow sine curves (0.3–0.9 Hz) — the dragon floats, the halo pulses, particles twinkle — the world is alive even when the visitor is still. **Emergence:** text never appears; it rises 110% from below a masked line with staggered word timing (90ms stagger, power3 easing), the way film titles emerge from darkness.

## 6. Color System

Primary: deep cosmic black `#0a0714`, midnight indigo `#0e0924`, ethereal violet `#7b3ff2`, aurora lavender `#b18ce6`, iridescent pink `#f6c0e6`, frost white `#f3eefc`, holographic cyan `#9be1ff`. Accent: ember orange `#ff7a1a`, solar gold `#ffb84d`, rose energy `#ff9ee9`. The palette is not static — six keyframed palettes (one per realm) are linearly interpolated by scroll progress and fed as uniforms into every shader, so the dragon, halo, particles and background always agree about what scene they are in. Chromatic aberration lives in the dragon's fresnel edge, where the glass splits white light into RGB.

## 7. Typography System

Two voices. The emotional voice is **Playfair Display Italic** — high-contrast serif, used enormous (up to 250px / 17vw) for one-word realm titles and for italic emphasis inside sentences. The interface voice is **Inter** at weights 300–600, tracked wide for kickers (0.34em letter-spacing, uppercase, 13px). A third whisper — **Pinyon Script** — appears only in the logotype, as the brand's handwritten soul. Headlines are poetic and short: "Stories become *Worlds*", "They invite us *inside*", "Growth is what you see, feel, hear, *interact with*", "*Awaken* your business."

## 8. 3D Interaction Ideas

The dragon tracks the cursor with damped parallax (it notices you, but is never controlled by you — divinity, not a pet). The whole camera shifts ±0.6 units against pointer position, creating environmental depth. The "Awaken the Dragon" button injects a `burst` impulse that overdrives the glow uniform and particle opacity for two seconds — the visitor can make the dragon flare. On touch devices, drag position replaces the cursor for the same parallax.

## 9. Scene-by-Scene Breakdown

**I — Birth (Hero).** Luminous lavender dawn, white core glow, faint conic rainbow halo. The glass dragon floats right of center, slowly turning, wearing pastel iridescence. Title: "Stories become Worlds."
**II — Inside (Philosophy).** The world collapses to cosmic black with violet volumetrics. The dragon goes neon — ultraviolet and magenta fresnel. The quote realm: "The best businesses don't just speak to us. They invite us inside."
**III — Crystal (Services).** Refractive glass shards orbit into existence around the dragon, each with full chromatic dispersion. Six services rendered as an editorial index with italic serif accents.
**IV — Fire (Story).** The deepest realm: ember dark, amber volumetrics rising from below. The dragon ignites to molten gold. "Growth is what you see, feel, hear, interact with."
**V — Ascension.** The dragon rises toward the top of frame, scales down with distance, gold flaring to white. "Awaken your business. Discover. Build. Scale. Transform."
**VI — Dawn (Contact).** The world exhales back to lilac. The dragon, small and serene, drifts above the closing line: "Let STOA help you tell your story the way it was meant."

## 10. Component System

`<GlassBar>` (navigation), `<GlassPill>` (primary CTA), `<SoundOrb>` (ambience toggle with equalizer animation), `<ScrollHint>`, `<RealmSection>` (100svh stage with `.inner` content slot), `<RevealText>` (masked word-reveal primitive `.rv`), `<Loader>` (brand mark + progress filament). Every component composes the same glass tokens, so the system stays coherent at any scale.

## 11. Animation Principles

Durations 0.6–1.2s for entrances, 0.3s for hovers; easings limited to power3.out (enter) and power2.in (exit). Reveals trigger at 62% viewport entry; exits at 38% before departure — content is always readable in the middle 60% of the journey. Scroll-linked properties (palette, background opacity, dragon transform) are *positions on a timeline*, not tweens — scrubbing the page scrubs the film, forward and backward, with no broken states.

## 12. Scroll Choreography

Page height is 600svh — six beats. A normalized progress value p ∈ [0,1] drives everything: `f = p × 5` selects the active palette pair and interpolant; four CSS background layers crossfade on triangular opacity bands centered at p = 0, .2/.4, .6/.8, 1; the dragon's position, scale and amplitude are keyframed per beat and interpolated between. Because all motion derives from one scalar, the choreography can never desynchronize.

## 13. WebGL Implementation

Three.js (r128) with a transparent-canvas renderer composited over CSS volumetric gradients — this gives film-grade background lighting at zero GPU cost. The dragon is a sculpted glTF mesh (with a fully procedural serpent fallback if the network blocks the model) wearing a custom ShaderMaterial: chromatic-dispersion fresnel (separate exponents per RGB channel), animated procedural environment bands sampled from the reflection vector, dual specular streak lights, scale shimmer interference, vertex-stage liquid ripple along normals, and a saturation-boost grade before output. Particles are additive-blended sprite points; the halo is an additive sprite tinted by the live palette. The production brief's upgrade path: React Three Fiber for componentization, Lenis for momentum scroll, UnrealBloomPass for true bloom, and a GPGPU trail simulation for the tail.

## 14. Premium Interaction Details

The CTA pill magnetically scales (1.06) and its star icon rotates 20° on hover; the sound orb's equalizer bars animate on a staggered 1.1s loop and freeze when muted; the contact email underline lifts 2px on hover; selection color is brand magenta; a 5%-opacity animated grain unifies the composited layers exactly the way film grain unifies VFX plates; the loader's progress filament and percentage make even the first 2 seconds feel directed.

## 15. Conversion Strategy

Conversion is choreographed, not shouted. The persistent glass CTA stays in the lower right through all six realms — always present, never blocking. The narrative itself is the funnel: philosophy creates belief, services create relevance, fire creates desire, ascension creates urgency, and the dawn realm offers exactly one action — a single enormous email link — at the emotional peak of calm trust. No forms, no friction, no competing actions.

## 16. Mobile Adaptation

Touch parallax replaces the cursor; `100svh` units prevent browser-chrome jumps; the dragon recenters (cx × 0.35) and rescales (4.6 vs 5.6 units) for portrait frames; geometry segments, particle count and pixel ratio all step down (110/10 tube segments, 140 particles, 1.5 DPR cap, antialias off) to hold 60fps on mid-range phones; the glass nav compacts to a 16px-radius bar; the scroll hint hides; typography uses fluid clamps from 28px to 250px so headlines stay cinematic at any width.

## 17. Developer Handoff

The repository ships a single self-contained `index.html` (zero build step — open and it runs) with three external CDN dependencies: three.min.js r128, GLTFLoader r128, GSAP 3.12 + ScrollTrigger. All design tokens live at the top of the stylesheet; all palettes in the `PHASES` array; all scroll choreography in `updateDragon()` and `bgOpacity()`. To upgrade to the full production stack, port the shader and choreography into React Three Fiber components, replace native scroll with Lenis, and move palettes into a theme provider — the math transfers unchanged.

## 18. Production Specifications

Canvas: full viewport, fixed, `pointer-events:none`, DPR ≤ 2. Frame budget: ≤ 6ms render on desktop (one mesh + ≤ 320 sprites + ≤ 6 crystals), ≤ 10ms mobile. Fonts subset via Google Fonts `display=swap`. Model: DragonAttenuation.glb (CC0, Khronos sample library) auto-normalized to 5.6 world units with dual-URL fallback and procedural-dragon failover, so the experience can never render empty. Accessibility: all narrative copy is real DOM text (screen-reader readable), interactive elements are native `<button>`/`<a>`, motion runs behind content and a `prefers-reduced-motion` gate is the first recommended production addition. Tested target: Chrome/Edge/Safari desktop, iOS Safari, Android Chrome.

---

*STOA — Explore. Evolve. Expand. A cinematic digital myth built for the future of luxury creative agencies.*
