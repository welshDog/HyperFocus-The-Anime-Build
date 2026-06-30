# Vertex AI MCP Setup Guide
## Hyperfocus Mode Anime — AI Production Pipeline

**Goal:** Connect Google Vertex AI to this workspace via MCP so we can run Imagen (image generation) and Veo (video generation) directly from chat — generating all 39 Episode 1 keyframes and animated clips without copy-pasting.

---

## Prerequisites Checklist

- [x] Google Cloud account set up
- [ ] Billing enabled on your GCP project
- [ ] Vertex AI API enabled
- [ ] Node.js v18+ installed on your machine
- [ ] Bun installed
- [ ] gcloud CLI installed

---

## STEP 1 — Enable Vertex AI API in Google Cloud Console

1. Go to [console.cloud.google.com](https://console.cloud.google.com)
2. Select your project (or create one named `hyperfocus-anime`)
3. Go to **APIs & Services → Library**
4. Search for **Vertex AI API** → Click **Enable**
5. Also enable **Cloud Storage API** (for saving generated assets)

✅ Takes 2 minutes.

---

## STEP 2 — Authenticate Your Machine

Open terminal and run:

```bash
gcloud auth login
gcloud auth application-default login
gcloud config set project YOUR_PROJECT_ID
```

Replace `YOUR_PROJECT_ID` with your actual GCP project ID (found in Cloud Console top bar).

✅ You're now authenticated. No API keys to manage — gcloud handles it.

---

## STEP 3 — Install Bun (if not already installed)

```bash
npm install -g bun
```

---

## STEP 4 — Install the Vertex AI MCP Server

**Option A — Quickest (global install):**

```bash
bun install -g vertex-ai-mcp-server
```

**Option B — Via Smithery (auto-configures for Claude/Perplexity desktop):**

```bash
bunx -y @smithery/cli install @shariqriazz/vertex-ai-mcp-server --client claude
```

**Option C — Clone and build manually:**

```bash
git clone https://github.com/shariqriazz/vertex-ai-mcp-server
cd vertex-ai-mcp-server
bun install
cp .env.example .env
bun run build
```

---

## STEP 5 — Configure Environment Variables

Create or edit your `.env` file:

```env
AI_PROVIDER=vertex
GOOGLE_CLOUD_PROJECT=your-project-id
GOOGLE_CLOUD_LOCATION=us-central1
DEFAULT_MODEL=gemini-2.0-flash
```

**For image generation add:**
```env
IMAGEN_MODEL=imagegeneration@006
```

**For video generation add:**
```env
VEO_MODEL=veo-2.0-generate-001
```

---

## STEP 6 — Add MCP Server to Your Workspace

In your MCP client config (e.g. `mcp.json` or Perplexity/Claude desktop settings), add:

```json
{
  "mcpServers": {
    "vertex-ai": {
      "command": "vertex-ai-mcp-server",
      "env": {
        "AI_PROVIDER": "vertex",
        "GOOGLE_CLOUD_PROJECT": "your-project-id",
        "GOOGLE_CLOUD_LOCATION": "us-central1"
      }
    }
  }
}
```

Restart your MCP client after saving.

✅ Vertex AI MCP is now connected.

---

## STEP 7 — Test The Connection

In chat, ask:

> "Generate an image of Nexus City at night, cyberpunk anime style, wide establishing shot"

If it works, you'll get an Imagen-generated image back directly in the response.

---

## What's Unlocked Once Connected

### Imagen — Image Generation
- Generate all 39 Episode 1 keyframes from `art/episode_01_shot_prompts.md`
- Character consistency using versioned prompts
- Style variations for any shot
- Upscaling and refinement

### Veo — Video Generation
- Image-to-video: feed keyframes → get animated clips
- Text-to-video: describe a shot → get a clip
- Combine with motion notes from `video/episode_01_motion_notes.md`
- Build all 39 clips for Episode 1 edit

### Gemini Models
- Script analysis and consistency checking against story bible
- Episode 2–12 generation with full canon context
- Dialogue polish per character voice
- Prompt optimisation for Imagen/Veo

### Prompt Versioning
- Store canonical character prompts (Lydrix v1.0, Kat v1.0 etc.)
- Reuse across all episodes for style consistency
- Version-controlled in Vertex AI prompt registry

---

## Estimated Costs (as of mid-2026)

| Task | Approx Cost |
|---|---|
| Imagen — 1 image | ~$0.02–0.04 |
| All 39 EP1 keyframes | ~$1.00–$2.00 |
| Veo — 1 short clip (5s) | ~$0.10–0.30 |
| All 39 EP1 clips | ~$5–$15 |
| Gemini calls (script work) | ~$0.01–0.10 per session |
| **Full EP1 production run** | **~$10–$20 total** |

---

## Production Run Order Once Connected

```
1. Generate 6 priority keyframes first (S1-03, S2-03, S4-04, S6-05, S10-02, S12-03)
2. Review style — adjust canonical prompts if needed
3. Generate remaining 33 keyframes
4. Animate 6 priority shots with Veo
5. Review motion — adjust motion note inputs
6. Animate remaining 33 clips
7. Download all to art/generated/episode_01/ and video/clips/episode_01/
8. Assemble in DaVinci Resolve using assembly guide
```

---

## Useful Links

- [Vertex AI MCP Server GitHub](https://github.com/shariqriazz/vertex-ai-mcp-server)
- [Google Cloud Console](https://console.cloud.google.com)
- [Vertex AI Model Garden](https://console.cloud.google.com/vertex-ai/model-garden)
- [Veo on Vertex AI Docs](https://cloud.google.com/vertex-ai/generative-ai/docs/video/generate-videos)
- [Imagen on Vertex AI Docs](https://cloud.google.com/vertex-ai/generative-ai/docs/image/overview)

---

*WelshDog x Perplexity AI — Hyperfocus Mode Series*
