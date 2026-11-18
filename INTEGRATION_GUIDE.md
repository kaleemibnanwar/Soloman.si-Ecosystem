# 🔌 Soloman AI Ecosystem: Integration & Developer Guide

This guide describes how to programmatically integrate, extend, and write custom tools or connectors for **Soloman Studio**, **Soloman Editor**, **Soloman Limb**, and **Soloman Manager**.

---

## 1. Ecosystem Protocol Specifications

### 1.1 Model Context Protocol (MCP) Schema
All tool declarations in `Soloman Manager` conform to the open Model Context Protocol v1.0 standard:

```json
{
  "name": "soloman-limb_execute_action",
  "description": "Direct OS mouse, keyboard, or window actions on target machine",
  "inputSchema": {
    "type": "object",
    "properties": {
      "action": {
        "type": "string",
        "enum": ["click", "type", "press_hotkey", "drag_and_drop", "screenshot", "wait_for_text"]
      },
      "coordinate": {
        "type": "object",
        "properties": { "x": { "type": "number" }, "y": { "type": "number" } }
      },
      "text": { "type": "string" },
      "keys": { "type": "array", "items": { "type": "string" } }
    },
    "required": ["action"]
  }
}
```

---

## 2. Setting Up the Unified Ecosystem SDK

### 2.1 Node.js / TypeScript SDK
Install the core ecosystem packages:

```bash
pnpm add @soloman/studio-sdk @soloman/mcp-client @soloman/cut-engine @soloman/limb-client
```

### 2.2 Initializing the Cluster Bridge

```typescript
import { createSolomanCluster } from '@soloman/studio-sdk';

const cluster = await createSolomanCluster({
  studio: {
    endpoint: process.env.SOLOMAN_STUDIO_URL || 'http://localhost:5173',
    apiKey: process.env.SOLOMAN_API_KEY
  },
  mcpManager: {
    gatewayUrl: 'http://localhost:3000',
    allowedServers: ['github', 'postgres', 'docker', 'filesystem']
  },
  soloman-limb: {
    rpcHost: 'localhost:8900',
    display: ':0',
    enableVisionStream: true
  },
  soloman-editor: {
    tempDirectory: '/tmp/soloman_media',
    encoder: 'h264_nvenc' // Hardware accelerated encoding
  }
});

// Verify all nodes are connected and healthy
const status = await cluster.getHealthStatus();
console.log('Cluster status:', status);
```

---

## 3. Creating a Custom MCP Tool for Soloman Manager

To expose a new service, database, or proprietary API to the entire Soloman ecosystem:

```typescript
import { McpServer } from '@modelcontextprotocol/sdk/server/mcp.js';
import { StdioServerTransport } from '@modelcontextprotocol/sdk/server/stdio.js';
import { z } from 'zod';

const server = new McpServer({
  name: 'my-custom-service',
  version: '1.0.0'
});

// Define tool
server.tool(
  'fetch_internal_metrics',
  { serviceName: z.string(), timeWindow: z.enum(['1h', '24h', '7d']) },
  async ({ serviceName, timeWindow }) => {
    const metrics = await queryPrometheus(serviceName, timeWindow);
    return {
      content: [{ type: 'text', text: JSON.stringify(metrics) }]
    };
  }
);

// Connect via stdio
const transport = new StdioServerTransport();
await server.connect(transport);
```

Register this server in `Soloman Manager`'s `mcpServers.json`:

```json
{
  "mcpServers": {
    "internal-metrics": {
      "command": "node",
      "args": ["/path/to/my-custom-service/index.js"]
    }
  }
}
```

---

## 4. Pipeline Hook Example: Bridging Soloman Limb and Soloman Editor

Stream live screen captures directly into the `Soloman Editor` video processing pipeline:

```typescript
import { LimbCaptureStream } from '@soloman/limb-client';
import { SolomanEditorSession } from '@soloman/cut-engine';

async function recordAndProduceTutorial(url: string, outputPath: string) {
  // 1. Initialize soloman-editor session
  const editor = new SolomanEditorSession({
    outputResolution: { width: 1920, height: 1080 },
    framerate: 60
  });

  // 2. Start soloman-limb video capture stream
  const captureStream = new LimbCaptureStream({ fps: 60, includeAudio: true });
  await captureStream.start();

  // 3. Execute actions with soloman-limb
  await captureStream.agent.navigateTo(url);
  await captureStream.agent.clickWithVisualFeedback('#start-demo-btn');
  await captureStream.agent.typeSmoothly('#search-box', 'Autonomous Agents');
  await captureStream.agent.wait(2000);

  const rawVideoFile = await captureStream.stop();

  // 4. Process with soloman-editor: auto-zoom into clicks, add captions & music
  await editor
    .loadRawFootage(rawVideoFile)
    .applySmartZoom({ focusOnClickEvents: true, scale: 1.25 })
    .stripDeadTime({ maxIdleSeconds: 0.5 })
    .addKineticSubtitles({ speechModel: 'whisper-base', style: 'tech-clean' })
    .renderTo(outputPath);

  console.log(`✅ Production video saved to: ${outputPath}`);
}
```

---

## 5. Event Bus & WebSocket Subscriptions

You can subscribe to real-time events across the entire cluster:

```typescript
cluster.events.on('agent:subtask:complete', (event) => {
  console.log(`Subtask ${event.subtaskId} completed by agent [${event.role}]`);
});

cluster.events.on('soloman-limb:action:executed', (event) => {
  console.log(`OS Action ${event.actionType} at (${event.x}, ${event.y})`);
});

cluster.events.on('soloman-editor:render:progress', (event) => {
  console.log(`Rendering progress: ${event.percentComplete}%`);
});
```
