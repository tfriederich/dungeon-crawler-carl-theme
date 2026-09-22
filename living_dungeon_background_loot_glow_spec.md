# Living Dungeon Background & Loot Glow

## Purpose

Add subtle *Dungeon Crawler Carl*-inspired atmosphere to the IDE without distracting from coding.

The effect system should make the interface feel alive while keeping the editor itself calm and readable.

The feature has two main parts:

1. **Living Dungeon Background**
2. **Loot Glow Feedback**

---

## 1. Living Dungeon Background

The background should feel like a dark dungeon environment that is quietly active.

### Core behavior

Use very low-opacity ambient effects around the outer UI areas:

- Slow purple glow near one edge of the application
- Very faint warm/red dungeon glow near another edge
- Optional low-density dust particles
- Soft vignette around the application frame
- Extremely slow movement and pulsing

### Important

Do **not** animate directly behind source code.

Prefer placing effects in:

- Sidebars
- Empty states
- Title bars
- Status areas
- Outer application edges
- Large unused background surfaces

The code editor surface should remain visually stable.

### Recommended intensity

Ambient effects should remain subtle.

Suggested values:

- Ambient glow opacity: `0.02 - 0.06`
- Dust opacity: `0.02 - 0.04`
- Animation duration: `12 - 25 seconds`
- Movement distance: `10 - 20px`

The user should notice the atmosphere without constantly noticing the animation.

---

## 2. Loot Glow Feedback

Important successful actions should briefly feel like receiving loot.

### Success

Examples:

- Build completes successfully
- Tests pass
- AI task completes
- Generation finishes
- Deployment succeeds

Use a short gold glow.

Suggested behavior:

- Gold outer glow
- Slight brighter highlight
- Fast fade
- Duration around `400 - 600ms`

Example color:

```json
{
  "color": "#E6B94A",
  "secondaryColor": "#FFF0A6"
}
```

---

## Rare / AI Result Glow

For important AI-generated output or special events, use a purple "rare loot" glow.

Examples:

- AI finishes a major task
- Important generated artifact is ready
- Special contextual event

Suggested behavior:

- Purple glow
- One soft pulse
- Fade away
- Duration around `600 - 800ms`

Example:

```json
{
  "color": "#8C5CFF",
  "secondaryColor": "#C4A8FF"
}
```

---

## AI Thinking State

When the AI is actively working, the surrounding interface can subtly react.

Use:

- Very faint purple ambient glow
- Slow breathing animation
- No flashing
- No large movement

Recommended duration:

```json
{
  "durationMs": 1800,
  "animation": "slow-pulse"
}
```

This effect should stop immediately when AI processing ends.

---

## Build Running State

During builds or long-running commands, use a restrained breathing accent.

Recommended:

```json
{
  "accentGlowColor": "#7A5AE6",
  "accentGlowOpacity": 0.08,
  "animation": "breathe",
  "durationMs": 2200
}
```

Do not animate the code editor itself.

---

## Error Feedback

Errors should feel dangerous, but should never interrupt the user.

Use a very short red edge flash.

Recommended:

```json
{
  "edgeFlashColor": "#D14F5A",
  "edgeFlashOpacity": 0.14,
  "durationMs": 180
}
```

Avoid:

- Screen shaking
- Large red overlays
- Repeated flashing
- Persistent animation

---

## Suggested Theme Configuration

```json
{
  "backgroundEffects": {
    "enabled": true,

    "ambientGlow": {
      "enabled": true,
      "color": "#6C45D8",
      "opacity": 0.045,
      "radius": 420,
      "position": "top-right",
      "animation": {
        "enabled": true,
        "type": "pulse",
        "durationMs": 14000,
        "minOpacity": 0.025,
        "maxOpacity": 0.055
      }
    },

    "secondaryGlow": {
      "enabled": true,
      "color": "#A64D2F",
      "opacity": 0.025,
      "radius": 500,
      "position": "bottom-left",
      "animation": {
        "enabled": true,
        "type": "drift",
        "durationMs": 22000,
        "distancePx": 18
      }
    },

    "dust": {
      "enabled": true,
      "opacity": 0.035,
      "density": 0.15,
      "speed": 0.12,
      "particleSizeMin": 1,
      "particleSizeMax": 2
    },

    "vignette": {
      "enabled": true,
      "color": "#000000",
      "opacity": 0.16,
      "size": 0.7
    }
  },

  "lootGlow": {
    "enabled": true,

    "success": {
      "color": "#E6B94A",
      "secondaryColor": "#FFF0A6",
      "durationMs": 520,
      "intensity": 0.32,
      "blurPx": 16,
      "spreadPx": 3,
      "animation": "flash-fade"
    },

    "rare": {
      "color": "#8C5CFF",
      "secondaryColor": "#C4A8FF",
      "durationMs": 650,
      "intensity": 0.28,
      "blurPx": 18,
      "spreadPx": 4,
      "animation": "pulse-fade"
    }
  },

  "stateEffects": {
    "aiThinking": {
      "backgroundGlowColor": "#7149D8",
      "backgroundGlowOpacity": 0.05,
      "animation": "slow-pulse",
      "durationMs": 1800
    },

    "buildRunning": {
      "accentGlowColor": "#7A5AE6",
      "accentGlowOpacity": 0.08,
      "animation": "breathe",
      "durationMs": 2200
    },

    "success": {
      "effect": "lootGlow.success"
    },

    "error": {
      "edgeFlashColor": "#D14F5A",
      "edgeFlashOpacity": 0.14,
      "durationMs": 180
    }
  }
}
```

---

## Accessibility

Respect reduced-motion preferences.

When reduced motion is enabled:

- Disable dust particles
- Disable drifting backgrounds
- Disable repeated pulses
- Keep only static low-opacity glows
- Allow one-time success/error fades if they are subtle

Example:

```json
{
  "accessibility": {
    "reduceMotionFallback": true,
    "disableParticlesWhenReducedMotion": true,
    "maxAmbientOpacity": 0.06
  }
}
```

---

## Design Principle

The theme should feel like:

> The dungeon is alive around the user, but the workspace remains calm.

If the effect becomes noticeable while reading or writing code, reduce its intensity.

The atmosphere should reward peripheral attention, not compete for primary attention.
