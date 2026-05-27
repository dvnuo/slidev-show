<script setup>
import { computed, ref } from "vue";

const parts = [
  {
    id: 0,
    title: "System Layer",
    label: "Global rules",
    desc: "Non-negotiable model behavior: safety policy, identity, tool-use limits, and global response constraints.",
    sample: "system: follow policy, use tools only when allowed",
    tokens: ["policy", "identity", "tool limits"],
    accent: "cyan",
  },
  {
    id: 1,
    title: "Workspace Instructions",
    label: "Team rules",
    desc: "Repository and team instructions that tell the agent how to work here: commands, coding style, review rules, and boundaries.",
    sample: "instructions: read runbooks, prefer approved CLIs",
    tokens: ["repo rules", "CLI contract", "review style"],
    accent: "blue",
  },
  {
    id: 2,
    title: "User Request",
    label: "Visible prompt",
    desc: "The user goal, constraints, acceptance criteria, and any explicit priority. This is the part people usually call the prompt.",
    sample: "user: setup Jira and Confluence tools for this workspace",
    tokens: ["goal", "constraints", "acceptance"],
    accent: "violet",
  },
  {
    id: 3,
    title: "Context Pack",
    label: "Relevant facts",
    desc: "Runtime context selected for the task: open files, diffs, terminal output, memory, runbook snippets, tickets, and previous observations.",
    sample: "context: selected files, current diff, memory.md, OPS-1827",
    tokens: ["files", "diff", "memory", "runbook"],
    accent: "emerald",
  },
  {
    id: 4,
    title: "Tool Contract",
    label: "Action surface",
    desc: "The tool list, schemas, permissions, and calling rules. This tells the model what actions are available and how tool results return.",
    sample: "tools: bash(cmd), jira(issue), browser(url), git(diff)",
    tokens: ["bash", "jira", "browser", "git"],
    accent: "amber",
  },
  {
    id: 5,
    title: "Response Contract",
    label: "Finish shape",
    desc: "The expected output format: evidence, command results, open risks, next steps, and the definition of done for the current task.",
    sample: "response: summary, evidence, changed files, verification",
    tokens: ["summary", "evidence", "risks"],
    accent: "rose",
  },
];

const activePart = ref(2);
const active = computed(() => parts[activePart.value]);

const camera = computed(() => ({
  x: 8 + activePart.value * 2.2,
  y: 18 + activePart.value * 4.3,
  perspective: 920 - activePart.value * 26,
}));

function layerStyle(part) {
  const visible = part.id <= activePart.value;
  const depth = 48 + part.id * 62;
  const lift = part.id * 7;
  return {
    opacity: visible ? 1 : 0.08,
    pointerEvents: visible ? "auto" : "none",
    transform: `translate3d(${part.id * 7}px, ${lift}px, ${depth}px)`,
  };
}

function selectPart(id) {
  activePart.value = id;
}
</script>

<template>
  <section class="prompt-payload" aria-label="Interactive LLM prompt payload layer stack">
    <div class="visual-pane">
      <div class="visual-title">
        <b>LLM Prompt Payload</b>
        <span>The user prompt is one layer inside a structured model request.</span>
      </div>

      <div class="scene" :style="{ perspective: `${camera.perspective}px` }">
        <div class="stack" :style="{ transform: `rotateX(${camera.x}deg) rotateY(${camera.y}deg)` }">
          <div class="base-layer">
            <div class="window-top">
              <span class="dot red" />
              <span class="dot yellow" />
              <span class="dot green" />
              <span class="address">model-request.payload</span>
            </div>
            <div class="runtime-grid">
              <div class="terminal-mark">LLM</div>
              <div>Structured model request</div>
            </div>
          </div>

          <button
            v-for="part in parts"
            :key="part.id"
            type="button"
            class="payload-layer"
            :class="[`accent-${part.accent}`, { active: part.id === activePart }]"
            :style="layerStyle(part)"
            @click="selectPart(part.id)"
          >
            <div class="layer-heading">
              <span>0{{ part.id + 1 }} // {{ part.title }}</span>
              <i>{{ part.label }}</i>
            </div>
            <div class="token-grid">
              <span v-for="token in part.tokens" :key="token">{{ token }}</span>
            </div>
          </button>
        </div>
      </div>
    </div>

    <aside class="control-pane">
      <div>
        <div class="eyebrow">
          <span />
          <b>Prompt Breakdown</b>
        </div>

        <div class="page-summary">
          <b>Main point</b>
          <p>The LLM receives a layered payload: rules, user request, context, tool contracts, and the expected response shape.</p>
        </div>

        <div class="active-card" :class="`accent-${active.accent}`">
          <div class="active-index">0{{ active.id + 1 }} // {{ active.title }}</div>
          <h1>{{ active.label }}</h1>
          <p>{{ active.desc }}</p>
          <code>{{ active.sample }}</code>
        </div>
      </div>

      <div class="step-list">
        <button
          v-for="part in parts"
          :key="part.id"
          type="button"
          class="step-button"
          :class="[{ active: part.id === activePart }, `accent-${part.accent}`]"
          @click="selectPart(part.id)"
        >
          <span>0{{ part.id + 1 }} / {{ part.title }}</span>
          <i />
        </button>
      </div>
    </aside>
  </section>
</template>

<style scoped>
.prompt-payload {
  display: grid;
  grid-template-columns: minmax(0, 1.34fr) minmax(430px, 0.9fr);
  height: 100%;
  min-height: 100%;
  overflow: hidden;
  background: #181818;
  color: #f8fafc;
}

.visual-pane {
  position: relative;
  display: grid;
  place-items: center;
  min-width: 0;
  overflow: hidden;
  border-right: 1px solid #303030;
  background:
    radial-gradient(circle at 44% 42%, rgba(20, 184, 166, 0.14), transparent 30%),
    linear-gradient(180deg, rgba(255, 255, 255, 0.025), transparent 42%),
    #181818;
}

.visual-title {
  position: absolute;
  top: 32px;
  left: 52px;
  right: 52px;
  z-index: 5;
  max-width: 680px;
}

.visual-title b {
  display: block;
  color: #f8fafc;
  font-size: 30px;
  font-weight: 850;
  line-height: 1.05;
}

.visual-title span {
  display: block;
  margin-top: 8px;
  color: #a5adba;
  font-size: 14px;
  font-weight: 650;
  line-height: 1.32;
}

.scene {
  position: relative;
  width: 430px;
  height: 500px;
  margin-top: 64px;
  transform: translateX(-92px) scale(0.84);
  transform-origin: center;
}

.stack {
  position: relative;
  width: 100%;
  height: 100%;
  transform-style: preserve-3d;
  transition: transform 720ms ease;
}

.base-layer,
.payload-layer {
  position: absolute;
  left: 44px;
  right: 44px;
  border-radius: 16px;
  transform-style: preserve-3d;
}

.base-layer {
  inset: 0;
  overflow: hidden;
  border: 1px solid #3d3d3d;
  background: rgba(31, 31, 31, 0.96);
  box-shadow: 0 32px 70px rgba(0, 0, 0, 0.48);
}

.window-top {
  display: flex;
  align-items: center;
  gap: 9px;
  height: 44px;
  padding: 0 14px;
  border-bottom: 1px solid #343434;
  background: #202020;
}

.dot {
  width: 12px;
  height: 12px;
  border-radius: 999px;
}

.red {
  background: #ef4444;
}

.yellow {
  background: #f59e0b;
}

.green {
  background: #10b981;
}

.address {
  display: flex;
  align-items: center;
  height: 24px;
  flex: 1;
  margin-left: 8px;
  padding: 0 12px;
  border: 1px solid #333;
  border-radius: 7px;
  background: #151515;
  color: #8b95a5;
  font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, "Liberation Mono", monospace;
  font-size: 12px;
}

.runtime-grid {
  position: relative;
  display: grid;
  place-items: center;
  height: calc(100% - 44px);
  color: #7f8794;
  font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, "Liberation Mono", monospace;
  background-image: radial-gradient(#333 1px, transparent 1px);
  background-size: 20px 20px;
}

.terminal-mark {
  display: grid;
  place-items: center;
  width: 52px;
  height: 52px;
  margin-bottom: 12px;
  border: 1px solid #3d3d3d;
  border-radius: 12px;
  color: #14b8a6;
  font-weight: 900;
}

.payload-layer {
  top: 116px;
  min-height: 204px;
  padding: 16px;
  border: 1px solid currentColor;
  background: rgba(17, 24, 39, 0.9);
  color: #e5e7eb;
  text-align: left;
  box-shadow: 0 26px 56px rgba(0, 0, 0, 0.34);
  transition:
    transform 720ms ease,
    opacity 420ms ease,
    border-color 280ms ease,
    background 280ms ease;
}

.payload-layer.active {
  background: rgba(15, 23, 42, 0.96);
  box-shadow:
    0 30px 70px rgba(0, 0, 0, 0.4),
    0 0 0 1px currentColor inset;
}

.layer-heading {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 14px;
  margin-bottom: 18px;
}

.layer-heading span,
.layer-heading i,
.eyebrow b,
.active-index,
.page-summary b,
.step-button span,
.token-grid span {
  font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, "Liberation Mono", monospace;
}

.layer-heading span {
  color: currentColor;
  font-size: 14px;
  font-weight: 900;
  letter-spacing: 0.08em;
  text-transform: uppercase;
}

.layer-heading i {
  color: #cbd5e1;
  font-size: 12px;
  font-style: normal;
  font-weight: 800;
  letter-spacing: 0.12em;
  text-transform: uppercase;
}

.token-grid {
  display: grid;
  grid-template-columns: repeat(2, minmax(0, 1fr));
  gap: 12px;
}

.token-grid span {
  display: grid;
  place-items: center;
  min-height: 42px;
  border: 1px solid color-mix(in srgb, currentColor 45%, transparent);
  border-radius: 10px;
  background: rgba(0, 0, 0, 0.28);
  color: color-mix(in srgb, currentColor 72%, white);
  font-size: 16px;
  font-weight: 850;
}

.control-pane {
  display: grid;
  grid-template-rows: auto minmax(0, auto);
  align-content: space-between;
  gap: 10px;
  min-width: 0;
  min-height: 0;
  padding: 26px 42px 34px;
  background: #1c1c1c;
}

.eyebrow {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 10px;
}

.eyebrow span {
  width: 10px;
  height: 10px;
  border-radius: 999px;
  background: #10b981;
}

.eyebrow b {
  color: #7a8290;
  font-size: 13px;
  font-weight: 900;
  letter-spacing: 0.18em;
  text-transform: uppercase;
}

.page-summary {
  margin-bottom: 10px;
  padding: 10px 13px;
  border: 1px solid rgba(52, 211, 153, 0.26);
  border-radius: 10px;
  background: rgba(16, 35, 31, 0.52);
}

.page-summary b {
  display: block;
  margin-bottom: 6px;
  color: #34d399;
  font-size: 10.5px;
  font-weight: 900;
  letter-spacing: 0.14em;
  text-transform: uppercase;
}

.page-summary p {
  margin: 0;
  color: #d1d5db;
  font-size: 12.5px;
  font-weight: 650;
  line-height: 1.28;
}

.active-card {
  position: relative;
  min-height: 172px;
  padding: 14px 18px;
  border: 1px solid #343434;
  border-radius: 14px;
  background: #252525;
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.03);
}

.active-index {
  margin-bottom: 6px;
  color: currentColor;
  font-size: 12px;
  font-weight: 900;
  letter-spacing: 0.08em;
  text-transform: uppercase;
}

.active-card h1 {
  margin: 0 0 8px;
  color: white;
  font-size: 24px;
  font-weight: 850;
  letter-spacing: 0;
  line-height: 1.05;
}

.active-card p {
  margin: 0 0 10px;
  color: #c9c9c9;
  font-size: 12.5px;
  font-weight: 650;
  line-height: 1.3;
}

.active-card code {
  display: block;
  padding: 8px 10px;
  border: 1px solid #3a3a3a;
  border-radius: 8px;
  background: #171717;
  color: #e5e7eb;
  font-size: 10.5px;
  line-height: 1.28;
  white-space: normal;
}

.step-list {
  display: grid;
  gap: 4px;
  min-height: 0;
}

.step-button {
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 100%;
  min-height: 26px;
  padding: 0 12px;
  border: 1px solid #333;
  border-radius: 8px;
  background: #202020;
  color: #b9b9b9;
  text-align: left;
  transition:
    border-color 180ms ease,
    background 180ms ease,
    color 180ms ease,
    transform 180ms ease;
}

.step-button:hover {
  transform: translateX(3px);
  border-color: #4b4b4b;
}

.step-button.active {
  border-color: #3b82f6;
  background: linear-gradient(90deg, rgba(59, 130, 246, 0.16), rgba(37, 37, 37, 0.95));
  color: white;
}

.step-button span {
  font-size: 10px;
  font-weight: 800;
}

.step-button i {
  width: 9px;
  height: 9px;
  border-radius: 999px;
  background: #6b7280;
}

.step-button.active i {
  background: currentColor;
}

.accent-cyan,
.step-button.accent-cyan.active {
  color: #22d3ee;
}

.accent-blue,
.step-button.accent-blue.active {
  color: #60a5fa;
}

.accent-violet,
.step-button.accent-violet.active {
  color: #c084fc;
}

.accent-emerald,
.step-button.accent-emerald.active {
  color: #34d399;
}

.accent-amber,
.step-button.accent-amber.active {
  color: #fbbf24;
}

.accent-rose,
.step-button.accent-rose.active {
  color: #fb7185;
}

@media (max-width: 900px) {
  .prompt-payload {
    grid-template-columns: 1fr;
  }

  .visual-pane {
    min-height: 52%;
    border-right: 0;
    border-bottom: 1px solid #303030;
  }

  .visual-title {
    left: 28px;
    right: 28px;
  }

  .scene {
    width: 380px;
    height: 430px;
    transform: scale(0.82);
  }

  .control-pane {
    padding: 24px 28px;
  }
}
</style>
