<template>
  <div class="agent-loop" aria-label="Animated agent loop diagram">
    <svg class="flow-lines" viewBox="0 0 460 438" aria-hidden="true">
      <defs>
        <marker id="arrow" markerWidth="8" markerHeight="8" refX="7" refY="4" orient="auto">
          <path d="M0,0 L8,4 L0,8 Z" />
        </marker>
        <marker id="done-arrow" markerWidth="8" markerHeight="8" refX="7" refY="4" orient="auto">
          <path d="M0,0 L8,4 L0,8 Z" />
        </marker>
      </defs>
      <path class="flow-path main-path" d="M125 104 C160 76 195 66 230 78 C286 95 333 128 366 182 C394 230 369 293 318 326 C273 355 204 350 161 315 C121 282 105 236 118 191 C130 148 162 122 208 112" />
      <path class="flow-path retry-path" d="M117 230 C68 176 86 101 154 70 C222 38 315 62 361 128" />
      <path class="flow-path done-path" d="M112 231 C83 264 76 306 104 342" />
      <circle class="flow-dot" r="5">
        <animateMotion dur="7s" repeatCount="indefinite" path="M125 104 C160 76 195 66 230 78 C286 95 333 128 366 182 C394 230 369 293 318 326 C273 355 204 350 161 315 C121 282 105 236 118 191 C130 148 162 122 208 112" />
      </circle>
    </svg>

    <div class="prompt-card step-card">
      <span class="stage-tag start">Start</span>
      <span class="step-index">01</span>
      <b>User Prompt</b>
      <small>Target + constraints</small>
    </div>

    <div class="agent-core">
      <b>Agent</b>
      <span>Instructions</span>
      <span>Skills</span>
      <span>Tools</span>
    </div>

    <div class="loop-node plan step-card"><span class="step-index">02</span><b>Plan</b></div>
    <div class="loop-node call step-card"><span class="step-index">03</span><b>Tool Call</b></div>
    <div class="loop-node observe step-card"><span class="step-index">04</span><b>Observe</b></div>
    <div class="loop-node verify step-card"><span class="step-index">05</span><b>Verify</b><small>retry or finish</small></div>

    <div class="mcp-card">MCP Servers</div>
    <div class="done-card step-card">
      <span class="stage-tag done">Done</span>
      <b>Verified Result</b>
      <small>evidence returned</small>
    </div>
  </div>
</template>

<style scoped>
.agent-loop {
  position: relative;
  width: min(460px, 100%);
  aspect-ratio: 1.05;
  margin: 1.5rem auto 0;
  border: 1px solid rgba(40, 52, 74, 0.16);
  border-radius: 8px;
  background:
    radial-gradient(circle at 50% 52%, rgba(33, 103, 165, 0.12), transparent 34%),
    linear-gradient(135deg, rgba(255, 255, 255, 0.92), rgba(239, 246, 255, 0.78));
  overflow: hidden;
}

.flow-lines {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
  z-index: 1;
}

.flow-lines marker path {
  fill: rgba(33, 103, 165, 0.58);
}

.flow-lines #done-arrow path {
  fill: #16a34a;
}

.flow-path {
  fill: none;
  stroke-width: 2.4;
  stroke-linecap: round;
}

.main-path {
  stroke: rgba(33, 103, 165, 0.38);
  stroke-dasharray: 8 8;
  marker-end: url("#arrow");
  animation: path-move 2.8s linear infinite;
}

.retry-path {
  stroke: rgba(197, 88, 50, 0.42);
  stroke-dasharray: 5 8;
  marker-end: url("#arrow");
}

.done-path {
  stroke: rgba(22, 163, 74, 0.58);
  marker-end: url("#done-arrow");
}

.flow-dot {
  fill: #2167a5;
  filter: drop-shadow(0 0 8px rgba(33, 103, 165, 0.42));
}

.prompt-card,
.mcp-card,
.agent-core,
.loop-node,
.done-card {
  position: absolute;
  border-radius: 8px;
  border: 1px solid rgba(36, 47, 68, 0.12);
  background: rgba(255, 255, 255, 0.92);
  box-shadow: 0 10px 26px rgba(17, 35, 64, 0.12);
  z-index: 2;
}

.step-card {
  animation: step-glow 7s ease-in-out infinite;
}

.prompt-card {
  left: 7%;
  top: 7%;
  width: 158px;
  padding: 12px 14px;
  animation-delay: 0s;
}

.step-index {
  display: inline-block;
  margin-bottom: 4px;
  color: rgba(33, 103, 165, 0.72);
  font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, "Liberation Mono", monospace;
  font-size: 0.7rem;
  font-weight: 800;
}

.stage-tag {
  position: absolute;
  top: -12px;
  right: 10px;
  padding: 3px 8px;
  border-radius: 999px;
  color: white;
  font-size: 0.58rem;
  font-weight: 900;
  letter-spacing: 0.08em;
  text-transform: uppercase;
}

.stage-tag.start {
  background: #16a34a;
}

.stage-tag.done {
  background: #2167a5;
}

.prompt-card small,
.done-card small,
.loop-node small {
  display: block;
  color: rgba(24, 32, 51, 0.62);
  font-size: 0.68rem;
}

.prompt-card b {
  display: block;
  color: #111827;
}

.agent-core {
  left: 50%;
  top: 50%;
  width: 162px;
  padding: 16px;
  transform: translate(-50%, -50%);
  background: #111827;
  color: white;
  text-align: center;
  z-index: 4;
}

.agent-core b {
  display: block;
  margin-bottom: 10px;
  font-size: 1.25rem;
}

.agent-core span {
  display: inline-block;
  margin: 3px;
  padding: 4px 8px;
  border-radius: 999px;
  background: rgba(255, 255, 255, 0.12);
  font-size: 0.72rem;
}

.loop-node {
  width: 116px;
  min-height: 58px;
  display: grid;
  place-items: center;
  color: #182033;
  font-weight: 650;
  text-align: center;
}

.loop-node b {
  line-height: 1.05;
}

.plan {
  left: 57%;
  top: 10%;
  transform: translateX(-50%);
  animation-delay: 1.15s;
}

.call {
  right: 7%;
  top: 50%;
  transform: translateY(-50%);
  animation-delay: 2.35s;
}

.observe {
  left: 50%;
  bottom: 10%;
  transform: translateX(-50%);
  animation-delay: 3.55s;
}

.verify {
  left: 7%;
  top: 50%;
  transform: translateY(-50%);
  animation-delay: 4.75s;
}

.mcp-card {
  right: 7%;
  bottom: 7%;
  width: 136px;
  padding: 12px 14px;
  color: #2167a5;
  font-weight: 700;
  text-align: center;
}

.done-card {
  left: 7%;
  bottom: 8%;
  width: 150px;
  padding: 11px 14px;
  border-color: rgba(22, 163, 74, 0.2);
  background: rgba(240, 253, 244, 0.96);
  color: #14532d;
  animation-delay: 5.95s;
}

.done-card b {
  display: block;
  line-height: 1.08;
}

@keyframes path-move {
  to {
    stroke-dashoffset: -32;
  }
}

@keyframes step-glow {
  0%,
  13%,
  100% {
    border-color: rgba(36, 47, 68, 0.12);
    box-shadow: 0 10px 26px rgba(17, 35, 64, 0.12);
  }
  6% {
    border-color: rgba(33, 103, 165, 0.55);
    box-shadow: 0 14px 30px rgba(33, 103, 165, 0.24);
  }
}
</style>
