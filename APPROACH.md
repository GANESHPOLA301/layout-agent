# Approach — Layout Agent

## Overview

The core idea: treat the design JSON as a **structured state object** that Claude can reason about and transform, given the right semantic context.

---

## Architecture

```
User Message
     ↓
[System Prompt with Semantic Map]
     +
[Current Layout JSON]
     +
[User Instruction]
     ↓
Claude claude-sonnet-4-20250514 (via Anthropic API)
     ↓
Updated Layout JSON
     ↓
State Update → Wireframe Re-render + JSON Viewer
```

---

## Key Design Decisions

### 1. Semantic Role Mapping in System Prompt
The raw JSON uses opaque IDs like `img_1778489515746_17`. The system prompt teaches the LLM which node ID maps to which semantic role (Background, Product, Headline, Offer Badge, etc.). This lets users speak naturally ("move the headline", "keep the product large") without caring about node IDs.

### 2. Normalized Coordinate System
Each node has both absolute (`x`, `y`, `width`, `height`) and normalized (`nx`, `ny`, `nw`, `nh`) coordinates. The normalized values are crucial for aspect ratio conversions — the system prompt instructs Claude to keep both in sync after every transformation.

### 3. Stateless LLM Calls
Each API call sends the **full current JSON** as context. This means:
- No session state on the backend needed
- Every call is idempotent and reproducible
- Follow-up instructions chain naturally since each call receives the latest state

### 4. Undo/Redo History
Every successful JSON transformation is pushed into a local history array. Undo/redo simply navigates this array, giving users confidence to experiment freely.

### 5. Wireframe Preview (not pixel-perfect)
The preview renders layout structure (positions, sizes, types) at a scaled-down 320px canvas. It intentionally avoids loading external images to stay fast and dependency-free. The goal is **spatial reasoning** — can you see that the headline moved? That the product is larger? — not pixel-perfect visual fidelity.

### 6. Suggested Commands
One-click chips expose the expected natural language vocabulary, reducing onboarding friction for evaluators.

---

## Tradeoffs & Limitations

- **No streaming**: Response appears all at once after ~2-3s. Could add streaming for perceived responsiveness.
- **JSON parse failures**: If Claude adds commentary around the JSON, the parse fails. The system prompt heavily emphasizes "respond ONLY with JSON" to minimize this.
- **No multi-artboard support**: Only the primary artboard is handled.
- **Wireframe only**: No actual image rendering; the preview uses colored divs.

---

## What I'd Add with More Time

1. **Pixel-perfect canvas rendering** using Konva.js or Fabric.js
2. **Drag-to-reposition** elements with JSON sync
3. **Streaming responses** for real-time feedback
4. **Version history sidebar** with named checkpoints
5. **Export to CSS/HTML** from the final JSON
