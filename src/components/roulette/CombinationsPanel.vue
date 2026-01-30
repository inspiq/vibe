<script setup lang="ts">
import type { CombinationStats, RouletteNumber } from '@/types/roulette';

defineProps<{
  combinationStats: CombinationStats | null;
  totalSpins: number;
}>();
</script>

<template>
  <div class="combinations-panel">
    <h2>Что выпадает после чего</h2>
    <p class="hint">После какого числа чаще всего выпадает следующее (по всей истории)</p>

    <div v-if="!combinationStats || totalSpins < 2" class="empty-state">
      <p>Нужно хотя бы 2 спина</p>
    </div>

    <div v-else class="combinations-grid">
      <div
        v-for="prev in [2, 3, 5, 10]"
        :key="prev"
        class="prev-block"
      >
        <div class="prev-label">Если выпало {{ prev }}x, дальше чаще:</div>
        <div class="pairs-list">
          <div
            v-for="pair in combinationStats.pairsByPrev[prev as RouletteNumber]"
            :key="`${pair.prev}-${pair.next}`"
            class="pair-row"
            :class="{ 'pair-high': pair.percentage >= 30 }"
          >
            <span class="pair-next">{{ pair.next }}x</span>
            <span class="pair-count">{{ pair.count }} раз</span>
            <span class="pair-pct">{{ pair.percentage.toFixed(0) }}%</span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped lang="scss">
.combinations-panel {
  h2 {
    margin-bottom: 0.25rem;
    color: #f2a100;
  }

  .hint {
    font-size: 0.9rem;
    opacity: 0.8;
    margin-bottom: 1rem;
  }

  .subtitle {
    font-size: 1rem;
    margin: 1rem 0 0.5rem;
    opacity: 0.9;
    color: #f2a100;
  }

  .subtitle:first-of-type {
    margin-top: 0;
  }
}

.combinations-content {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.triples-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 0.75rem;

  @media (min-width: 768px) {
    grid-template-columns: repeat(4, 1fr);
  }
}

.triple-block {
  font-size: 0.9rem;
}

.empty-state {
  padding: 1.5rem;
  text-align: center;
  opacity: 0.7;
}

.combinations-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 1rem;

  @media (min-width: 768px) {
    grid-template-columns: repeat(4, 1fr);
  }
}

.prev-block {
  background: rgba(255, 255, 255, 0.03);
  border-radius: 8px;
  padding: 0.75rem;
  border: 1px solid rgba(255, 255, 255, 0.1);
}

.prev-label {
  font-weight: 600;
  margin-bottom: 0.5rem;
  font-size: 0.95rem;
  color: #f2a100;
}

.pairs-list {
  display: flex;
  flex-direction: column;
  gap: 0.35rem;
}

.pair-row {
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-size: 0.85rem;
  padding: 0.25rem 0;

  &.pair-high {
    color: #10b981;
    font-weight: 600;
  }

  .pair-next {
    font-weight: 600;
  }

  .pair-pct {
    opacity: 0.9;
  }
}
</style>
