<template>
  <div class="agent-loop" aria-label="Agent loop diagram">
    <div class="loop-core">
      <span>Agent</span>
    </div>
    <div class="loop-node goal">Goal</div>
    <div class="loop-node plan">Plan</div>
    <div class="loop-node tool">Tool</div>
    <div class="loop-node observe">Observe</div>
  </div>
</template>

<style scoped>
.agent-loop {
  position: relative;
  width: min(420px, 100%);
  aspect-ratio: 1;
  margin: 1.5rem auto 0;
  border: 1px solid rgba(40, 52, 74, 0.16);
  border-radius: 8px;
  background:
    radial-gradient(circle at center, rgba(38, 139, 210, 0.12), transparent 34%),
    linear-gradient(135deg, rgba(255, 255, 255, 0.88), rgba(237, 244, 255, 0.72));
}

.agent-loop::before {
  content: "";
  position: absolute;
  inset: 20%;
  border: 2px dashed rgba(28, 96, 162, 0.35);
  border-radius: 999px;
}

.loop-core,
.loop-node {
  position: absolute;
  display: grid;
  place-items: center;
  border-radius: 8px;
  box-shadow: 0 10px 26px rgba(17, 35, 64, 0.12);
}

.loop-core {
  inset: 36%;
  background: #111827;
  color: white;
  font-weight: 700;
}

.loop-node {
  width: 112px;
  min-height: 54px;
  border: 1px solid rgba(36, 47, 68, 0.12);
  background: white;
  color: #182033;
  font-weight: 650;
}

.goal {
  left: 50%;
  top: 8%;
  transform: translateX(-50%);
}

.plan {
  right: 8%;
  top: 50%;
  transform: translateY(-50%);
}

.tool {
  left: 50%;
  bottom: 8%;
  transform: translateX(-50%);
}

.observe {
  left: 8%;
  top: 50%;
  transform: translateY(-50%);
}
</style>
