<script setup>
import { computed, ref } from "vue";

const steps = [
  {
    id: 0,
    title: "Prompt",
    subtitle: "User intent",
    desc: "The user gives the target, constraints, and acceptance criteria. This is the request that enters the extension stack.",
    accent: "neutral",
  },
  {
    id: 1,
    title: "Instructions",
    subtitle: "Operating rules",
    desc: "Instructions turn generic model behavior into team behavior: safety, preferred commands, escalation, and output style.",
    accent: "blue",
  },
  {
    id: 2,
    title: "Skills",
    subtitle: "Reusable playbooks",
    desc: "Skills package repeated work into reusable playbooks, so setup, AWS access, triage, and review are repeatable.",
    accent: "violet",
  },
  {
    id: 3,
    title: "Tools And Memory",
    subtitle: "Tools + Memory",
    desc: "This is the extension core: tools let Copilot act through trusted commands, while memory keeps stable context and team decisions available.",
    accent: "emerald",
  },
  {
    id: 4,
    title: "Verification",
    subtitle: "Evidence loop",
    desc: "Verification closes the loop: the agent checks command output against the goal and returns evidence instead of guesses.",
    accent: "amber",
  },
];

const activeStep = ref(3);

const active = computed(() => steps[activeStep.value]);

const camera = computed(() => {
  const step = activeStep.value;
  if (step <= 0) return { x: 0, y: 0, perspective: 920 };
  if (step === 1) return { x: 8, y: 24, perspective: 880 };
  if (step === 2) return { x: 14, y: 32, perspective: 840 };
  if (step === 3) return { x: 18, y: 40, perspective: 805 };
  return { x: 22, y: 46, perspective: 780 };
});

const layerZ = computed(() => {
  const step = activeStep.value;
  return {
    prompt: step >= 0 ? 12 : 0,
    instructions: step >= 1 ? 86 : 20,
    skills: step >= 2 ? 160 : 54,
    tools: step >= 3 ? 238 : 88,
    verify: step >= 4 ? 304 : 120,
  };
});

function selectStep(id) {
  activeStep.value = id;
}
</script>

<template>
  <section class="agent-layer-visualizer" aria-label="Interactive agent operating layer visualizer">
    <div class="visual-pane">
      <div class="visual-title">
        <b>Copilot Extension Stack</b>
        <span>Instructions and skills shape behavior; tools and memory make the agent useful.</span>
      </div>
      <div class="scene" :style="{ perspective: `${camera.perspective}px` }">
        <div
          class="stack"
          :style="{
            transform:
              activeStep === 0
                ? 'rotateX(0deg) rotateY(0deg)'
                : `rotateX(${camera.x}deg) rotateY(${camera.y}deg)`,
          }"
        >
          <div class="layer base-layer">
            <div class="window-top">
              <span class="dot red" />
              <span class="dot yellow" />
              <span class="dot green" />
              <span class="address">agent-runtime.local</span>
            </div>
            <div class="runtime-grid">
              <div class="terminal-mark">$</div>
              <div>Runtime Environment</div>
            </div>
          </div>

          <div class="layer prompt-layer" :style="{ transform: `translate3d(0, 0, ${layerZ.prompt}px)` }">
            <div class="small-label">User prompt</div>
            <div class="prompt-title">"Find the safest path to finish this ticket."</div>
            <div class="prompt-meta">Goal + constraints + acceptance criteria</div>
          </div>

          <div
            class="layer instruction-layer"
            :class="{ visible: activeStep >= 1 }"
            :style="{ transform: `translate3d(0, 0, ${layerZ.instructions}px)` }"
          >
            <div class="code-line"><span>system</span>: safety, tone, tool policy</div>
            <div class="code-line"><span>workspace</span>: repo rules, CLI contract</div>
            <div class="code-line"><span>team</span>: runbooks before production</div>
          </div>

          <div
            class="layer skill-layer"
            :class="{ visible: activeStep >= 2 }"
            :style="{ transform: `translate3d(0, 0, ${layerZ.skills}px)` }"
          >
            <div class="skill-chip">Atlassian CLI Skill</div>
            <div class="skill-chip">AWS Profile Skill</div>
            <div class="skill-chip">Review Playbook</div>
          </div>

          <div
            class="layer tool-layer"
            :class="{ visible: activeStep >= 3 }"
            :style="{ transform: `translate3d(0, 0, ${layerZ.tools}px)` }"
          >
            <div class="tool-node">bash</div>
            <div class="tool-node">jira</div>
            <div class="tool-node">aws</div>
            <div class="tool-node">memory</div>
          </div>

          <div
            class="layer verify-layer"
            :class="{ visible: activeStep >= 4 }"
            :style="{ transform: `translate3d(0, 0, ${layerZ.verify}px)` }"
          >
            <div class="verify-title">Evidence</div>
            <div class="verify-row">tests pass</div>
            <div class="verify-row">logs checked</div>
            <div class="verify-row">ticket updated</div>
          </div>
        </div>
      </div>
    </div>

    <aside class="control-pane">
      <div>
        <div class="eyebrow">
          <span />
          <b>Copilot Extension Stack</b>
        </div>

        <div class="page-summary">
          <b>Main point</b>
          <p>GitHub Copilot becomes an agent extension when it can act through tools, remember stable context, and verify each step.</p>
        </div>

        <div class="active-card" :class="`accent-${active.accent}`">
          <div class="active-index">0{{ active.id + 1 }} // {{ active.title }}</div>
          <h1>{{ active.subtitle }}</h1>
          <p>{{ active.desc }}</p>
        </div>
      </div>

      <div class="step-list">
        <button
          v-for="item in steps"
          :key="item.id"
          type="button"
          class="step-button"
          :class="[{ active: item.id === activeStep }, `accent-${item.accent}`]"
          @click="selectStep(item.id)"
        >
          <span>0{{ item.id + 1 }} / {{ item.title }}</span>
          <i />
        </button>
      </div>
    </aside>
  </section>
</template>

<style scoped>
.agent-layer-visualizer {
  display: grid;
  grid-template-columns: minmax(0, 1.35fr) minmax(420px, 0.9fr);
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
    linear-gradient(180deg, rgba(255, 255, 255, 0.025), transparent 40%),
    #181818;
}

.visual-title {
  position: absolute;
  top: 34px;
  left: 48px;
  right: 48px;
  z-index: 5;
  max-width: 560px;
}

.visual-title b {
  display: block;
  color: #f8fafc;
  font-size: 26px;
  font-weight: 850;
  line-height: 1.05;
}

.visual-title span {
  display: block;
  margin-top: 8px;
  color: #9ca3af;
  font-size: 12.5px;
  font-weight: 650;
  line-height: 1.28;
}

.scene {
  position: relative;
  width: 430px;
  height: 500px;
  transform: translateX(-18px) scale(0.84);
  transform-origin: center;
}

.stack {
  position: relative;
  width: 100%;
  height: 100%;
  transform-style: preserve-3d;
  transition: transform 720ms ease;
}

.layer {
  position: absolute;
  left: 42px;
  right: 42px;
  top: 54px;
  min-height: 290px;
  border: 1px solid #3d3d3d;
  border-radius: 16px;
  background: rgba(32, 32, 32, 0.92);
  box-shadow: 0 28px 60px rgba(0, 0, 0, 0.42);
  transition:
    transform 720ms ease,
    opacity 420ms ease,
    border-color 420ms ease;
  transform-style: preserve-3d;
}

.base-layer {
  inset: 0;
  min-height: auto;
  overflow: hidden;
  background: #1f1f1f;
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
  color: #777;
  font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, "Liberation Mono", monospace;
  font-size: 12px;
}

.runtime-grid {
  position: relative;
  display: grid;
  place-items: center;
  height: calc(100% - 44px);
  color: #8b8b8b;
  font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, "Liberation Mono", monospace;
  background-image: radial-gradient(#333 1px, transparent 1px);
  background-size: 20px 20px;
}

.terminal-mark {
  display: grid;
  place-items: center;
  width: 42px;
  height: 42px;
  margin-bottom: 12px;
  border: 1px solid #3d3d3d;
  border-radius: 10px;
  color: #14b8a6;
}

.prompt-layer {
  top: 108px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  min-height: 260px;
  padding: 32px;
  background: rgba(37, 37, 37, 0.95);
}

.small-label,
.prompt-meta {
  color: #8b95a5;
  font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, "Liberation Mono", monospace;
  font-size: 13px;
  letter-spacing: 0.12em;
  text-transform: uppercase;
}

.prompt-title {
  margin: 18px 0 20px;
  color: white;
  font-size: 28px;
  font-weight: 800;
  line-height: 1.18;
}

.instruction-layer,
.skill-layer,
.tool-layer,
.verify-layer {
  opacity: 0;
}

.instruction-layer.visible,
.skill-layer.visible,
.tool-layer.visible,
.verify-layer.visible {
  opacity: 1;
}

.instruction-layer {
  top: 126px;
  min-height: 228px;
  padding: 28px;
  border-color: rgba(96, 165, 250, 0.44);
  background: rgba(22, 28, 42, 0.92);
}

.code-line {
  margin: 14px 0;
  color: #dbeafe;
  font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, "Liberation Mono", monospace;
  font-size: 15px;
}

.code-line span {
  color: #60a5fa;
}

.skill-layer {
  top: 148px;
  min-height: 196px;
  padding: 26px;
  border-color: rgba(168, 85, 247, 0.48);
  background: rgba(31, 23, 45, 0.92);
}

.skill-chip {
  margin: 12px 0;
  padding: 12px 14px;
  border: 1px solid rgba(168, 85, 247, 0.36);
  border-radius: 9px;
  background: rgba(15, 15, 18, 0.62);
  color: #e9d5ff;
  font-weight: 700;
}

.tool-layer {
  top: 165px;
  display: grid;
  grid-template-columns: repeat(2, minmax(0, 1fr));
  gap: 14px;
  min-height: 180px;
  padding: 26px;
  border: 1px dashed rgba(52, 211, 153, 0.68);
  background: rgba(16, 35, 31, 0.9);
}

.tool-node {
  display: grid;
  place-items: center;
  border: 1px solid rgba(52, 211, 153, 0.34);
  border-radius: 9px;
  background: rgba(10, 15, 14, 0.7);
  color: #6ee7b7;
  font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, "Liberation Mono", monospace;
  font-size: 18px;
  font-weight: 800;
}

.verify-layer {
  top: 185px;
  min-height: 150px;
  padding: 22px;
  border-color: rgba(251, 191, 36, 0.52);
  background: rgba(42, 31, 13, 0.92);
}

.verify-title {
  margin-bottom: 12px;
  color: #fbbf24;
  font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, "Liberation Mono", monospace;
  font-weight: 800;
  letter-spacing: 0.14em;
  text-transform: uppercase;
}

.verify-row {
  margin: 8px 0;
  color: #fde68a;
  font-size: 17px;
  font-weight: 700;
}

.control-pane {
  display: grid;
  grid-template-rows: auto minmax(0, auto);
  align-content: space-between;
  gap: 12px;
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
  width: 9px;
  height: 9px;
  border-radius: 999px;
  background: #10b981;
}

.eyebrow b {
  color: #6b7280;
  font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, "Liberation Mono", monospace;
  font-size: 14px;
  letter-spacing: 0.18em;
  text-transform: uppercase;
}

.page-summary {
  margin-bottom: 10px;
  padding: 10px 14px;
  border: 1px solid rgba(52, 211, 153, 0.26);
  border-radius: 10px;
  background: rgba(16, 35, 31, 0.52);
}

.page-summary b {
  display: block;
  margin-bottom: 6px;
  color: #34d399;
  font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, Liberation Mono, monospace;
  font-size: 10.5px;
  font-weight: 900;
  letter-spacing: 0.14em;
  text-transform: uppercase;
}

.page-summary p {
  margin: 0;
  color: #d1d5db;
  font-size: 14px;
  font-weight: 650;
  line-height: 1.35;
}

.active-card {
  position: relative;
  min-height: 118px;
  padding: 14px 20px;
  border: 1px solid #343434;
  border-radius: 14px;
  background: #252525;
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.03);
}

.active-index {
  margin-bottom: 6px;
  color: #14b8a6;
  font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, "Liberation Mono", monospace;
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
  line-height: 1.1;
}

.active-card p {
  margin: 0;
  color: #c9c9c9;
  font-size: 12.5px;
  font-weight: 650;
  line-height: 1.3;
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
  min-height: 29px;
  padding: 0 14px;
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
  font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, "Liberation Mono", monospace;
  font-size: 11px;
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

.accent-blue .active-index,
.step-button.accent-blue.active {
  color: #60a5fa;
}

.accent-violet .active-index,
.step-button.accent-violet.active {
  color: #c084fc;
}

.accent-emerald .active-index,
.step-button.accent-emerald.active {
  color: #34d399;
}

.accent-amber .active-index,
.step-button.accent-amber.active {
  color: #fbbf24;
}

@media (max-width: 900px) {
  .agent-layer-visualizer {
    grid-template-columns: 1fr;
  }

  .visual-pane {
    min-height: 48%;
    border-right: 0;
    border-bottom: 1px solid #303030;
  }

  .control-pane {
    padding: 28px;
  }

  .scene {
    width: 360px;
    height: 420px;
    transform: scale(0.82);
  }
}
</style>
