# 🧪 Soloman AI Ecosystem: Automation Recipes & System Blueprints

This guide provides concrete, copy-paste-ready recipes and workflow configurations demonstrating how to interconnect **Soloman Studio**, **Soloman Editor**, **Soloman Limb**, and **Soloman Manager** to build autonomous production systems.

---

## 📋 Table of Contents
1. [Recipe 1: Fully Autonomous AI Content & Video Marketing Studio](#recipe-1-fully-autonomous-ai-content--video-marketing-studio)
2. [Recipe 2: Self-Healing Full-Stack DevOps & E2E Testing Cloud](#recipe-2-self-healing-full-stack-devops--e2e-testing-cloud)
3. [Recipe 3: Enterprise Legacy Desktop RPA & Invoice Intelligence Bridge](#recipe-3-enterprise-legacy-desktop-rpa--invoice-intelligence-bridge)
4. [Recipe 4: AI Software Engineering & Interactive UI Showcase Engine](#recipe-4-ai-software-engineering--interactive-ui-showcase-engine)
5. [Recipe 5: Autonomous Security Auditor & Penetration Visualizer](#recipe-5-autonomous-security-auditor--penetration-visualizer)

---

## Recipe 1: Fully Autonomous AI Content & Video Marketing Studio

**Objective:** Automatically monitor product releases or blog posts, draft high-engagement scripts, capture UI walk-throughs on desktop, edit vertical video reels with kinetic captions and voiceovers, and publish to social channels.

```
[Soloman Manager: RSS / GitHub Release Feed]
                     │
                     ▼
[Soloman Studio: Scriptwriting & Scene Planner Agent]
         │                               │
         ▼                               ▼
[soloman-limb: Live UI Demo Capture]  [ElevenLabs/TTS Audio Gen]
         │                               │
         └───────────────┬───────────────┘
                         ▼
             [soloman-editor: Video Compilation]
    - Crop to 9:16 vertical
    - Dynamic kinetic animated subtitles
    - Background music ducking
    - Zoom-in transitions on active clicks
                         │
                         ▼
[Soloman Manager: YouTube / TikTok / X Publisher]
```

### Automation Configuration
```json
{
  "pipelineId": "autonomous-video-agency",
  "trigger": { "mcpSource": "github", "event": "release.published" },
  "steps": [
    {
      "tool": "Soloman Studio",
      "action": "generateScript",
      "params": { "tone": "energetic-tech", "durationSeconds": 45 }
    },
    {
      "tool": "soloman-limb",
      "action": "recordScenario",
      "params": {
        "url": "${trigger.payload.releaseUrl}",
        "actions": ["highlightNewFeatures", "clickInteractiveDemo"]
      }
    },
    {
      "tool": "soloman-editor",
      "action": "renderVideo",
      "params": {
        "aspectRatio": "9:16",
        "subtitles": { "style": "hormozi-yellow", "animation": "bounce" },
        "voiceover": { "voice": "adam", "autoDuck": true }
      }
    },
    {
      "tool": "Soloman Manager",
      "action": "publishSocial",
      "params": { "platforms": ["youtube_shorts", "tiktok", "twitter_video"] }
    }
  ]
}
```

---

## Recipe 2: Self-Healing Full-Stack DevOps & E2E Testing Cloud

**Objective:** When an exception is logged in production, automatically capture the error trace, spin up a local staging environment, reproduce the bug using human-like browser automation (`Soloman Limb`), fix the source code (`Soloman Studio`), re-verify, and open a documented PR with an attached proof-of-fix video (`Soloman Editor`).

### Workflow Steps
1. **Trigger:** `Soloman Manager` (Sentry MCP server) receives an unhandled JavaScript exception from user session.
2. **Analysis:** `Soloman Studio` reads the error stack trace, pinpoints the faulty component in `src/components/`, and designs a reproduction script.
3. **Reproduction:** `Soloman Limb` boots a headless Chromium instance, follows the user's reproduction path, and confirms the crash while recording the screen.
4. **Code Patch:** `Soloman Studio` applies a surgical fix via AST transforms.
5. **Re-Test & Video Compilation:** `Soloman Limb` reruns the test scenario. `Soloman Editor` stitches the **Before (Failing)** and **After (Fixed)** recordings into a side-by-side verification clip.
6. **PR Creation:** `Soloman Manager` (GitHub MCP) submits the pull request, embeds the video artifact, and alerts the engineering Slack channel.

---

## Recipe 3: Enterprise Legacy Desktop RPA & Invoice Intelligence Bridge

**Objective:** Process incoming unstructured invoice scans and enter them into a legacy Windows desktop ERP application (which has no web API) with 100% visual validation and audit trail generation.

```python
# RPA Orchestration Script utilizing the ecosystem
from soloman_studio import StudioClient
from soloman_limb import ComputerAgent
from soloman_mcp import MCPManagerClient
from soloman_editor import VideoLogger

async def run_invoice_rpa(invoice_pdf_path: str):
    mcp = MCPManagerClient()
    studio = StudioClient()
    limb = ComputerAgent()
    video = VideoLogger(session_name="invoice_entry")

    # 1. Extract invoice data using Studio & Document OCR
    invoice_data = await studio.parse_document(invoice_pdf_path)

    # 2. Begin desktop recording for compliance
    video.start_recording()

    # 3. Launch legacy desktop ERP and enter records
    await limb.launch_app("C:\\EnterpriseERP\\BillingApp.exe")
    await limb.click_text("New Entry")
    await limb.type_text(invoice_data["vendor_id"])
    await limb.press_key("Tab")
    await limb.type_text(str(invoice_data["total_amount"]))
    await limb.click_button("Submit & Verify")
    
    # 4. Confirm success dialog
    await limb.wait_for_visual_element("Transaction Recorded OK")
    raw_video = video.stop_recording()

    # 5. Process audit video via soloman-editor and upload to company storage MCP
    audit_clip = await soloman-editor.compress_and_watermark(raw_video, metadata=invoice_data)
    await mcp.execute("aws-s3", "uploadFile", {"bucket": "compliance-audit-logs", "file": audit_clip})
```

---

## Recipe 4: AI Software Engineering & Interactive UI Showcase Engine

**Objective:** Given a feature request prompt, generate complete React components in `Soloman Studio`, test responsiveness across 4 distinct viewport resolutions via `Soloman Limb`, and compile a 60 FPS promotional video showing the component interactions with `Soloman Editor`.

### Execution Pipeline Matrix

| Step | Responsible Tool | Input Artifact | Output Artifact |
| :--- | :--- | :--- | :--- |
| 1. Architecture & Coding | `Soloman Studio` | Feature spec prompt | React/TypeScript files |
| 2. Tool Integration | `Soloman Manager` | API requirements | Mock server / Live API schemas |
| 3. Visual Responsive QA | `Soloman Limb` | Component preview URL | 4K Screen captures (Desktop, Tablet, Mobile) |
| 4. Polish & Showcase Edit | `Soloman Editor` | Raw recordings + narration script | Final 60fps polished product video |

---

## Recipe 5: Autonomous Security Auditor & Penetration Visualizer

**Objective:** Audit web applications for security vulnerabilities (XSS, CSRF, broken access control), automatically construct proof-of-concept exploits using `Soloman Limb`, and compile an executive video debriefing with `Soloman Editor` for security teams.

```bash
# Running an ecosystem security audit
soloman-cli run-audit \
  --target "https://staging.internal-app.com" \
  --orchestrator "Soloman Studio" \
  --tools "Soloman Manager:security-pack" \
  --actuator "soloman-limb:stealth-browser" \
  --reporter "soloman-editor:executive-summary"
```

- **Output:** Comprehensive vulnerability report with reproduction timestamps and video evidence clips attached to each finding.
