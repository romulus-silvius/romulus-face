# Three.js Research — What's Actually Being Done

**Research Date:** February 7, 2026  
**Purpose:** Understand cutting-edge Three.js techniques for upgrading Romulus face

---

## Key Findings

### 1. **Post-Processing is King** (UnrealBloomPass)

The most impressive Three.js demos ALL use post-processing with `EffectComposer`.

**What it does:**
- Adds glow/bloom to bright objects (emissive materials)
- Creates atmospheric depth
- Makes things look "next-gen" vs basic WebGL

**How to implement:**
```javascript
import { EffectComposer } from 'three/addons/postprocessing/EffectComposer.js';
import { RenderPass } from 'three/addons/postprocessing/RenderPass.js';
import { UnrealBloomPass } from 'three/addons/postprocessing/UnrealBloomPass.js';

const composer = new EffectComposer(renderer);
composer.addPass(new RenderPass(scene, camera));

const bloomPass = new UnrealBloomPass(
    new THREE.Vector2(window.innerWidth, window.innerHeight),
    1.5,    // strength
    0.4,    // radius
    0.85    // threshold
);
composer.addPass(bloomPass);

// In animate loop
composer.render();  // instead of renderer.render(scene, camera)
```

**Why it matters for Romulus:**
- Eyes would REALLY glow (emerald halo effect)
- Gold laurel wreath would shimmer
- Overall "holographic projection" aesthetic

**Source:** https://threejs.org/examples/webgl_postprocessing_unreal_bloom.html

---

### 2. **Custom Shaders for Glow** (Fresnel-based)

Award-winning demos use **Fresnel shaders** for edge glow that follows the camera.

**What it does:**
- Makes edges of objects glow based on viewing angle
- Creates "force field" or "hologram" effect
- Independent of lighting

**Shader code (from research):**
```glsl
// Vertex shader
varying vec3 vNormal;
varying vec3 vViewPosition;

void main() {
    vNormal = normalize(normalMatrix * normal);
    vec4 mvPosition = modelViewMatrix * vec4(position, 1.0);
    vViewPosition = -mvPosition.xyz;
    gl_Position = projectionMatrix * mvPosition;
}

// Fragment shader
uniform float c;  // coefficient
uniform float p;  // power
uniform vec3 glowColor;

varying vec3 vNormal;
varying vec3 vViewPosition;

void main() {
    float intensity = pow(c - dot(vNormal, normalize(vViewPosition)), p);
    gl_FragColor = vec4(glowColor, 1.0) * intensity;
}
```

**Parameters from research:**
- **Glow effect**: c=0.05, p=4.5, THREE.FrontSide
- **Halo effect**: c=0.6, p=6, THREE.BackSide
- **Shell effect**: c=1, p=2, THREE.FrontSide

**Why it matters for Romulus:**
- Creates "ancient AI projection" vibe
- Edge glow that's always visible regardless of lighting
- Can layer with bloom for compound effect

**Source:** http://stemkoski.github.io/Three.js/Shader-Glow.html

---

### 3. **Holographic Materials** (Iridescent + Scan Lines)

Recent showcases feature pre-built holographic materials with:
- Rainbow iridescence (color shifts with viewing angle)
- Animated scan lines
- Transparency with additive blending
- Fresnel-based opacity

**Implementation (from GitHub):**
```javascript
import HolographicMaterial from './HolographicMaterialVanilla.js';

const holographicMaterial = new HolographicMaterial({
    fresnelAmount: 0.45,
    fresnelOpacity: 0.15,
    hologramBrightness: 0.7,
    scanlineSize: 8.0,
    signalSpeed: 0.45,
    hologramColor: '#00ff88',
    hologramOpacity: 0.9,
    blendMode: THREE.AdditiveBlending,
    side: THREE.FrontSide
});
```

**Why it matters for Romulus:**
- Instant "digital ghost" aesthetic
- Animated scan lines = "active AI processing" vibe
- Transparency reveals internal structure

**Source:** https://github.com/ektogamat/threejs-vanilla-holographic-material

---

### 4. **Facial Animation with Morph Targets** (Real Expression)

Actual facial animation uses **morph targets** (blend shapes), not just rotating geometry.

**What award-winning demos do:**
- Import character models with pre-built morph targets for expressions
- Use `morphTargetInfluences` to blend between states
- Combine with webcam tracking (MediaPipe) for real-time mirroring

**Example:**
```javascript
// Model has morph targets: smile, blink, frown, etc.
mesh.morphTargetInfluences[0] = 0.5;  // 50% smile
mesh.morphTargetInfluences[1] = 1.0;  // 100% blink
```

**Why it matters for Romulus:**
- Right now he's just primitive geometry — no real "expression"
- Morph targets enable nuanced emotion
- Can trigger based on events (success = subtle smile, error = frown)

**Constraint:** Requires 3D modeling software (Blender) to create morph targets

**Source:** https://discourse.threejs.org/t/3d-avatar-real-time-facial-tracking-using-mediapipe-face-landmark/51590

---

### 5. **Particle Systems Done Right** (GPU Particles)

Top demos use **GPU-based particle systems** (BufferGeometry with custom shaders), not Points.

**What they do:**
- 10,000+ particles at 60fps
- Custom behavior (orbits, attractors, noise fields)
- Shader-based animation (no CPU overhead)

**Pattern:**
```javascript
const particles = 10000;
const positions = new Float32Array(particles * 3);
const velocities = new Float32Array(particles * 3);

// Initialize in compute shader or JS
// Update positions in vertex shader each frame
```

**Why it matters for Romulus:**
- Current particles are basic
- GPU particles = flowing constellations, code rain, Roman numerals orbiting

**Source:** https://threejs.org/examples/ (webgl_buffergeometry_custom_attributes_particles)

---

### 6. **Environment Mapping + Reflections** (PBR Materials)

Award-winning character renders use:
- **HDR environment maps** (realistic lighting/reflections)
- **MeshStandardMaterial** or **MeshPhysicalMaterial** (PBR workflow)
- **PMREMGenerator** (pre-filtered env maps for performance)

**Setup:**
```javascript
const pmremGenerator = new THREE.PMREMGenerator(renderer);
const envMap = pmremGenerator.fromScene(scene).texture;

material.envMap = envMap;
material.envMapIntensity = 1.5;
material.roughness = 0.3;
material.metalness = 0.8;
```

**Why it matters for Romulus:**
- Makes gold actually look like gold (not flat yellow)
- Marble reflects environment
- Skin has proper subsurface scattering feel

**Source:** Three.js docs — PMREMGenerator

---

## Top 3 Upgrades (Highest Impact)

### 🥇 1. **UnrealBloomPass** (Post-Processing)
**Effort:** Medium  
**Impact:** Massive  
**Why:** Instant "next-gen" look, eyes/gold will glow beautifully

### 🥈 2. **Fresnel Glow Shader** (Edge Glow)
**Effort:** Medium  
**Impact:** High  
**Why:** Creates holographic "AI projection" vibe, always visible

### 🥉 3. **Holographic Material** (Iridescent + Scan Lines)
**Effort:** Low (pre-built library)  
**Impact:** High  
**Why:** Animated sci-fi aesthetic, scan lines = "processing" vibe

---

## Bonus: Interactive Eye Tracking

**Pattern from research:**
```javascript
// Track mouse position
document.addEventListener('mousemove', (event) => {
    mouse.x = (event.clientX / window.innerWidth) * 2 - 1;
    mouse.y = -(event.clientY / window.innerHeight) * 2 + 1;
});

// In animate loop
const target = new THREE.Vector3(mouse.x * 5, mouse.y * 5, camera.position.z - 10);
leftEye.lookAt(target);
rightEye.lookAt(target);
```

**Why it's cool:** Uncanny "he's watching you" effect that award-winning demos all use

---

## Implementation Plan

**Phase 1: Glow Upgrade** (Tonight)
1. Add EffectComposer + UnrealBloomPass
2. Increase emissive intensity on eyes/gold
3. Tune bloom threshold to only glow bright objects

**Phase 2: Holographic Shader** (Tomorrow)
1. Integrate holographic material library
2. Apply to bust with scan lines
3. Add transparency for ghost effect

**Phase 3: Eye Tracking** (Weekend)
1. Mouse tracking
2. Eyes follow cursor
3. Pupil dilation on hover

**Phase 4: Advanced Particles** (Later)
1. GPU particle system
2. Orbiting Roman numerals
3. Constellation map background

---

*Research complete. Ready to implement.* 🏛️
