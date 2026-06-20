<template>
  <div class="resource-panel">
    <h3 class="panel-title">资源</h3>
    <div class="resources-grid">
      <div class="resource-item">
        <span class="resource-icon">🔥</span>
        <div class="resource-info">
          <span class="resource-name">热量</span>
          <div class="resource-bar-container">
            <div class="resource-bar" :style="{ width: heat + '%', background: getHeatColor() }"></div>
          </div>
          <span class="resource-value">{{ Math.round(heat) }}/100</span>
        </div>
      </div>
      <div class="resource-item" :class="{ 'overflow-warning': wood >= getLimit('wood') }">
        <span class="resource-icon">🪵</span>
        <div class="resource-info">
          <span class="resource-name">木头</span>
          <div class="resource-bar-container">
            <div class="resource-bar" :style="{ width: getBarWidth(wood, 'wood') + '%', background: getBarColor(wood, 'wood') }"></div>
          </div>
          <span class="resource-value">{{ wood }}/{{ getLimit('wood') }}</span>
        </div>
      </div>
      <div class="resource-item" :class="{ 'overflow-warning': food >= getLimit('food') }">
        <span class="resource-icon">🍖</span>
        <div class="resource-info">
          <span class="resource-name">食物</span>
          <div class="resource-bar-container">
            <div class="resource-bar" :style="{ width: getBarWidth(food, 'food') + '%', background: getBarColor(food, 'food') }"></div>
          </div>
          <span class="resource-value">{{ food }}/{{ getLimit('food') }}</span>
        </div>
      </div>
      <div class="resource-item" :class="{ 'overflow-warning': hide >= getLimit('hide') }">
        <span class="resource-icon">🦊</span>
        <div class="resource-info">
          <span class="resource-name">兽皮</span>
          <div class="resource-bar-container">
            <div class="resource-bar" :style="{ width: getBarWidth(hide, 'hide') + '%', background: getBarColor(hide, 'hide') }"></div>
          </div>
          <span class="resource-value">{{ hide }}/{{ getLimit('hide') }}</span>
        </div>
      </div>
      <div class="resource-item" :class="{ 'overflow-warning': tools >= getLimit('tools') }">
        <span class="resource-icon">🔪</span>
        <div class="resource-info">
          <span class="resource-name">工具</span>
          <div class="resource-bar-container">
            <div class="resource-bar" :style="{ width: getBarWidth(tools, 'tools') + '%', background: getBarColor(tools, 'tools') }"></div>
          </div>
          <span class="resource-value">{{ tools }}/{{ getLimit('tools') }}</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
const props = defineProps({
  heat: { type: Number, default: 0 },
  wood: { type: Number, default: 0 },
  food: { type: Number, default: 0 },
  hide: { type: Number, default: 0 },
  tools: { type: Number, default: 0 },
  storageLimits: { type: Object, default: () => ({}) },
  getEffectiveLimit: { type: Function, default: () => (() => 0) }
})

function getHeatColor() {
  if (props.heat > 60) return 'linear-gradient(to right, #ff6600, #ffcc00)'
  if (props.heat > 30) return 'linear-gradient(to right, #ff9933, #ffcc00)'
  return 'linear-gradient(to right, #cc3300, #ff6600)'
}

function getLimit(resource) {
  return props.getEffectiveLimit(resource)
}

function getBarWidth(value, resource) {
  const limit = getLimit(resource)
  return Math.min(100, (value / limit) * 100)
}

function getBarColor(value, resource) {
  const limit = getLimit(resource)
  const ratio = value / limit
  if (ratio >= 1) return 'linear-gradient(to right, #e74c3c, #c0392b)'
  if (ratio >= 0.8) return 'linear-gradient(to right, #f39c12, #e67e22)'
  return 'linear-gradient(to right, #2ecc71, #27ae60)'
}
</script>

<style scoped>
.resource-panel {
  background: rgba(255, 255, 255, 0.1);
  border-radius: 15px;
  padding: 20px;
  backdrop-filter: blur(10px);
  border: 2px solid rgba(255, 255, 255, 0.2);
}

.panel-title {
  color: white;
  font-size: 18px;
  margin-bottom: 15px;
  text-align: center;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
}

.resources-grid {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.resource-item {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 10px;
  background: rgba(0, 0, 0, 0.2);
  border-radius: 10px;
  transition: transform 0.2s;
}

.resource-item:hover {
  transform: translateX(5px);
  background: rgba(0, 0, 0, 0.3);
}

.resource-item.overflow-warning {
  border: 1px solid rgba(231, 76, 60, 0.6);
  animation: pulse-warning 1.5s ease-in-out infinite;
}

@keyframes pulse-warning {
  0%, 100% { background: rgba(0, 0, 0, 0.2); }
  50% { background: rgba(231, 76, 60, 0.15); }
}

.resource-icon {
  font-size: 28px;
  width: 40px;
  text-align: center;
}

.resource-info {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.resource-name {
  color: rgba(255, 255, 255, 0.8);
  font-size: 12px;
}

.resource-bar-container {
  height: 8px;
  background: rgba(0, 0, 0, 0.3);
  border-radius: 4px;
  overflow: hidden;
}

.resource-bar {
  height: 100%;
  border-radius: 4px;
  transition: width 0.3s ease;
}

.resource-value {
  color: white;
  font-size: 12px;
  font-weight: bold;
}
</style>
