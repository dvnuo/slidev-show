<template>
  <div class="agent-loop" aria-label="Animated agent loop diagram">
    <div class="prompt-card">
      <span>User Prompt</span>
      <b>Ship the task</b>
    </div>
    <div class="agent-core">
      <b>Agent</b>
      <span>Instructions</span>
      <span>Skills</span>
      <span>Tools</span>
    </div>
    <div class="loop-ring" />
    <div class="loop-pulse" />
    <div class="loop-node plan">Plan</div>
    <div class="loop-node call">Tool Call</div>
    <div class="loop-node observe">Observe</div>
    <div class="loop-node verify">Verify</div>
    <div class="mcp-card">MCP Servers</div>
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

.prompt-card,
.mcp-card,
.agent-core,
.loop-node {
  position: absolute;
  border-radius: 8px;
  border: 1px solid rgba(36, 47, 68, 0.12);
  background: rgba(255, 255, 255, 0.92);
  box-shadow: 0 10px 26px rgba(17, 35, 64, 0.12);
}

.prompt-card {
  left: 7%;
  top: 7%;
  width: 150px;
  padding: 13px 15px;
}

.prompt-card span {
  display: block;
  color: rgba(24, 32, 51, 0.62);
  font-size: 0.78rem;
  text-transform: uppercase;
}

.prompt-card b {
  display: block;
  margin-top: 4px;
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
  z-index: 3;
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

.loop-ring {
  position: absolute;
  inset: 19%;
  border: 2px dashed rgba(33, 103, 165, 0.32);
  border-radius: 999px;
  animation: rotate-ring 18s linear infinite;
}

.loop-pulse {
  position: absolute;
  left: 50%;
  top: 19%;
  width: 18px;
  height: 18px;
  border-radius: 999px;
  background: #c55832;
  box-shadow: 0 0 0 10px rgba(197, 88, 50, 0.12);
  transform-origin: 0 160px;
  animation: orbit 6s linear infinite;
}

.loop-node {
  width: 112px;
  min-height: 54px;
  display: grid;
  place-items: center;
  color: #182033;
  font-weight: 650;
  z-index: 2;
}

.plan {
  left: 50%;
  top: 10%;
  transform: translateX(-50%);
}

.call {
  right: 7%;
  top: 50%;
  transform: translateY(-50%);
}

.observe {
  left: 50%;
  bottom: 10%;
  transform: translateX(-50%);
}

.verify {
  left: 7%;
  top: 50%;
  transform: translateY(-50%);
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

@keyframes rotate-ring {
  to {
    transform: rotate(360deg);
  }
}

@keyframes orbit {
  0% {
    transform: rotate(0deg) translateY(-160px) rotate(0deg);
  }
  100% {
    transform: rotate(360deg) translateY(-160px) rotate(-360deg);
  }
}
</style>
