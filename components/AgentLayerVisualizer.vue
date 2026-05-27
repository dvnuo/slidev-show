<script setup>
import { computed, ref } from "vue";

const parts = [
  {
    id: 0,
    title: "System Layer",
    label: "Global rules",
    desc: "Non-negotiable model behavior: safety policy, identity, tool-use limits, and global response constraints.",
    sample: "system: follow policy, use tools only when allowed",
    accent: "cyan",
  },
  {
    id: 1,
    title: "Workspace Instructions",
    label: "Team rules",
    desc: "Repository and team instructions that tell the agent how to work here: commands, coding style, review rules, and boundaries.",
    sample: "instructions: read runbooks, prefer approved CLIs",
    accent: "blue",
  },
  {
    id: 2,
    title: "User Request",
    label: "Visible prompt",
    desc: "The user goal, constraints, acceptance criteria, and any explicit priority. This is the part people usually call the prompt.",
    sample: "user: setup Jira and Confluence tools for this workspace",
    accent: "violet",
  },
  {
    id: 3,
    title: "Context Pack",
    label: "Relevant facts",
    desc: "Runtime context selected for the task: open files, diffs, terminal output, memory, runbook snippets, tickets, and previous observations.",
    sample: "context: selected files, current diff, memory.md, OPS-1827",
    accent: "emerald",
  },
  {
    id: 4,
    title: "Tool Contract",
    label: "Action surface",
    desc: "The tool list, schemas, permissions, and calling rules. This tells the model what actions are available and how tool results return.",
    sample: "tools: bash(cmd), jira(issue), browser(url), git(diff)",
    accent: "amber",
  },
  {
    id: 5,
    title: "Response Contract",
    label: "Finish shape",
    desc: "The expected output format: evidence, command results, open risks, next steps, and the definition of done for the current task.",
    sample: "response: summary, evidence, changed files, verification",
    accent: "rose",
  },
];

const activePart = ref(2);
const active = computed(() => parts[activePart.value]);

function selectPart(id) {
  activePart.value = id;
}
</script>

<template>
  <section class="prompt-payload" aria-label="LLM prompt payload breakdown">
    <div class="visual-pane">
      <div class="visual-title">
        <b>LLM Prompt Payload</b>
        <span>The visible user prompt is only one layer. The agent runtime builds a structured model request around it.</span>
      </div>

      <div class="payload-window">
        <div class="window-top">
          <span class="dot red" />
          <span class="dot yellow" />
          <span class="dot green" />
          <span class="address">model-request.payload</span>
        </div>

        <div class="payload-body">
          <button
            v-for="part in parts"
            :key="part.id"
            type="button"
            class="payload-card"
            :class="[`accent-${part.accent}`, { active: part.id === activePart }]"
            @click="selectPart(part.id)"
          >
            <span class="part-index">0{{ part.id + 1 }}</span>
            <b>{{ part.title }}</b>
            <small>{{ part.label }}</small>
          </button>
        </div>

        <div class="payload-formula">
          <span>model call</span>
          <b>rules + user request + context + tools + response contract</b>
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
          <p>The LLM does not receive raw chat text alone. It receives a layered request that controls behavior, supplies context, and defines available actions.</p>
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
  grid-template-columns: minmax(0, 1.28fr) minmax(430px, 0.92fr);
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
    radial-gradient(circle at 38% 45%, rgba(20, 184, 166, 0.14), transparent 28%),
    linear-gradient(180deg, rgba(255, 255, 255, 0.025), transparent 42%),
    #181818;
}

.visual-title {
  position: absolute;
  top: 30px;
  left: 52px;
  right: 52px;
  z-index: 3;
  max-width: 650px;
}

.visual-title b {
  display: block;
  color: #f8fafc;
  font-size: 25px;
  font-weight: 850;
  line-height: 1.05;
}

.visual-title span {
  display: block;
  margin-top: 7px;
  max-width: 560px;
  color: #a5adba;
  font-size: 10px;
  font-weight: 650;
  line-height: 1.25;
}

.payload-window {
  width: min(720px, 82%);
  margin-top: 96px;
  border: 1px solid #343434;
  border-radius: 16px;
  background: rgba(31, 31, 31, 0.95);
  box-shadow: 0 34px 80px rgba(0, 0, 0, 0.42);
  overflow: hidden;
}

.window-top {
  display: flex;
  align-items: center;
  gap: 7px;
  height: 34px;
  padding: 0 12px;
  border-bottom: 1px solid #343434;
  background: #202020;
}

.dot {
  width: 9px;
  height: 9px;
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
  height: 20px;
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

.payload-body {
  display: grid;
  gap: 6px;
  padding: 14px;
  background-image: radial-gradient(#333 1px, transparent 1px);
  background-size: 18px 18px;
}

.payload-card {
  display: grid;
  grid-template-columns: 38px minmax(0, 1fr) 122px;
  align-items: center;
  gap: 10px;
  min-height: 38px;
  padding: 0 12px;
  border: 1px solid rgba(148, 163, 184, 0.18);
  border-radius: 10px;
  background: rgba(20, 20, 20, 0.86);
  color: #e5e7eb;
  text-align: left;
  transition:
    border-color 180ms ease,
    background 180ms ease,
    color 180ms ease,
    transform 180ms ease;
}

.payload-card:hover,
.payload-card.active {
  transform: translateX(6px);
  border-color: currentColor;
  background: rgba(15, 23, 42, 0.92);
}

.part-index,
.payload-card small,
.payload-formula span,
.eyebrow b,
.active-index,
.page-summary b,
.step-button span {
  font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, "Liberation Mono", monospace;
}

.part-index {
  color: currentColor;
  font-size: 12px;
  font-weight: 900;
}

.payload-card b {
  font-size: 15px;
  font-weight: 850;
  line-height: 1.1;
}

.payload-card small {
  justify-self: end;
  color: #9ca3af;
  font-size: 9.5px;
  font-weight: 800;
  letter-spacing: 0.08em;
  text-transform: uppercase;
}

.payload-formula {
  display: grid;
  gap: 5px;
  padding: 9px 14px 11px;
  border-top: 1px solid #343434;
  background: rgba(15, 15, 15, 0.92);
}

.payload-formula span {
  color: #34d399;
  font-size: 9.5px;
  font-weight: 900;
  letter-spacing: 0.14em;
  text-transform: uppercase;
}

.payload-formula b {
  color: #f8fafc;
  font-size: 12px;
  line-height: 1.2;
}

.control-pane {
  display: grid;
  grid-template-rows: auto minmax(0, auto);
  align-content: space-between;
  gap: 10px;
  min-width: 0;
  min-height: 0;
  padding: 24px 40px 30px;
  background: #1c1c1c;
}

.eyebrow {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 8px;
}

.eyebrow span {
  width: 10px;
  height: 10px;
  border-radius: 999px;
  background: #10b981;
}

.eyebrow b {
  color: #7a8290;
  font-size: 12px;
  font-weight: 900;
  letter-spacing: 0.16em;
  text-transform: uppercase;
}

.page-summary {
  margin-bottom: 8px;
  padding: 9px 12px;
  border: 1px solid rgba(52, 211, 153, 0.26);
  border-radius: 10px;
  background: rgba(16, 35, 31, 0.52);
}

.page-summary b {
  display: block;
  margin-bottom: 6px;
  color: #34d399;
  font-size: 11px;
  font-weight: 900;
  letter-spacing: 0.14em;
  text-transform: uppercase;
}

.page-summary p {
  margin: 0;
  color: #d1d5db;
  font-size: 11.5px;
  font-weight: 650;
  line-height: 1.24;
}

.active-card {
  position: relative;
  min-height: 142px;
  padding: 12px 16px;
  border: 1px solid #343434;
  border-radius: 14px;
  background: #252525;
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.03);
}

.active-index {
  margin-bottom: 5px;
  color: currentColor;
  font-size: 12px;
  font-weight: 900;
  letter-spacing: 0.08em;
  text-transform: uppercase;
}

.active-card h1 {
  margin: 0 0 10px;
  color: white;
  font-size: 20px;
  font-weight: 850;
  letter-spacing: 0;
  line-height: 1.05;
}

.active-card p {
  margin: 0 0 8px;
  color: #c9c9c9;
  font-size: 11.5px;
  font-weight: 650;
  line-height: 1.25;
}

.active-card code {
  display: block;
  padding: 7px 9px;
  border: 1px solid #3a3a3a;
  border-radius: 8px;
  background: #171717;
  color: #e5e7eb;
  font-size: 10px;
  line-height: 1.25;
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
  min-height: 24px;
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
  font-size: 9.5px;
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

  .payload-window {
    width: 88%;
    margin-top: 86px;
  }

  .payload-card {
    grid-template-columns: 40px minmax(0, 1fr);
  }

  .payload-card small {
    justify-self: start;
    grid-column: 2;
  }

  .control-pane {
    padding: 24px 28px;
  }
}
</style>
