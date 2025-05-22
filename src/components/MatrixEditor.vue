<template>
  <div class="matrix-editor">
    <!-- 维度控制区域 -->
    <div class="dimension-controls">
      <div class="control-group">
        <label for="rows">
          <i class="fas fa-arrows-alt-v"></i>
          行数
        </label>
        <input
          type="number"
          id="rows"
          v-model.number="rows"
          min="1"
          max="10"
          @change="initMatrix"
          class="dimension-input"
        />
      </div>
      <div class="control-group">
        <label for="cols">
          <i class="fas fa-arrows-alt-h"></i>
          列数
        </label>
        <input
          type="number"
          id="cols"
          v-model.number="cols"
          min="1"
          max="10"
          @change="initMatrix"
          class="dimension-input"
        />
      </div>
    </div>

    <!-- 快速操作按钮 -->
    <div class="quick-actions">
      <button @click="fillRandom" class="action-btn random-btn">
        <i class="fas fa-dice"></i>
        随机填充
      </button>
      <button @click="fillZero" class="action-btn zero-btn">
        <i class="fas fa-eraser"></i>
        清零
      </button>
      <button @click="fillIdentity" class="action-btn identity-btn" :disabled="rows !== cols">
        <i class="fas fa-eye"></i>
        单位矩阵
      </button>
    </div>

    <!-- 矩阵网格 -->
    <div v-if="matrix.length > 0" class="matrix-container">
      <div class="matrix-grid" :style="{ '--cols': cols }">
        <div v-for="(row, rowIndex) in matrix" :key="rowIndex" class="matrix-row">
          <input
            type="number"
            v-for="(cell, colIndex) in row"
            :key="`${rowIndex}-${colIndex}`"
            v-model.number="matrix[rowIndex][colIndex]"
            @input="emitMatrix"
            @focus="onCellFocus(rowIndex, colIndex)"
            @blur="onCellBlur"
            class="matrix-cell"
            :class="{ 'focused': focusedCell?.row === rowIndex && focusedCell?.col === colIndex }"
            step="0.1"
          />
        </div>
      </div>
      <div class="matrix-info">
        <span class="matrix-size">{{ rows }} × {{ cols }}</span>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, watch, onMounted } from 'vue';

const rows = ref(3);
const cols = ref(3);
const matrix = ref<number[][]>([]);
const focusedCell = ref<{ row: number; col: number } | null>(null);

const emit = defineEmits(['update:matrix']);

const initMatrix = () => {
  const newMatrix: number[][] = [];
  for (let i = 0; i < rows.value; i++) {
    const row: number[] = [];
    for (let j = 0; j < cols.value; j++) {
      // Preserve existing values if dimensions match, otherwise default to 0
      row.push(matrix.value[i]?.[j] ?? 0);
    }
    newMatrix.push(row);
  }
  matrix.value = newMatrix;
  emitMatrix();
};

const emitMatrix = () => {
  emit('update:matrix', matrix.value);
};

// 快速操作函数
const fillRandom = () => {
  for (let i = 0; i < rows.value; i++) {
    for (let j = 0; j < cols.value; j++) {
      matrix.value[i][j] = Math.floor(Math.random() * 21) - 10; // -10 到 10 的随机整数
    }
  }
  emitMatrix();
};

const fillZero = () => {
  for (let i = 0; i < rows.value; i++) {
    for (let j = 0; j < cols.value; j++) {
      matrix.value[i][j] = 0;
    }
  }
  emitMatrix();
};

const fillIdentity = () => {
  if (rows.value !== cols.value) return;

  for (let i = 0; i < rows.value; i++) {
    for (let j = 0; j < cols.value; j++) {
      matrix.value[i][j] = i === j ? 1 : 0;
    }
  }
  emitMatrix();
};

// 焦点管理
const onCellFocus = (row: number, col: number) => {
  focusedCell.value = { row, col };
};

const onCellBlur = () => {
  focusedCell.value = null;
};

watch([rows, cols], () => {
  initMatrix();
});

onMounted(() => {
  initMatrix();
});
</script>

<style scoped>
.matrix-editor {
  width: 100%;
}

/* 维度控制区域 */
.dimension-controls {
  display: flex;
  gap: var(--spacing-lg);
  margin-bottom: var(--spacing-lg);
  justify-content: center;
  flex-wrap: wrap;
}

.control-group {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: var(--spacing-sm);
}

.control-group label {
  display: flex;
  align-items: center;
  gap: var(--spacing-xs);
  font-weight: 600;
  color: var(--text-secondary);
  font-size: 0.875rem;
}

.control-group i {
  color: var(--primary-color);
}

.dimension-input {
  width: 80px;
  text-align: center;
  font-weight: 600;
  font-family: var(--font-family-mono);
}

/* 快速操作按钮 */
.quick-actions {
  display: flex;
  gap: var(--spacing-sm);
  margin-bottom: var(--spacing-lg);
  justify-content: center;
  flex-wrap: wrap;
}

.action-btn {
  padding: var(--spacing-sm) var(--spacing-md);
  font-size: 0.75rem;
  font-weight: 500;
  border-radius: var(--radius-md);
  transition: var(--transition-normal);
  position: relative;
  overflow: hidden;
}

.action-btn::before {
  content: '';
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.2), transparent);
  transition: var(--transition-normal);
}

.action-btn:hover::before {
  left: 100%;
}

.random-btn {
  background: var(--warning-gradient);
  color: white;
}

.random-btn:hover {
  transform: translateY(-2px);
  box-shadow: var(--shadow-md);
}

.zero-btn {
  background: var(--danger-gradient);
  color: white;
}

.zero-btn:hover {
  transform: translateY(-2px);
  box-shadow: var(--shadow-md);
}

.identity-btn {
  background: var(--success-gradient);
  color: white;
}

.identity-btn:hover:not(:disabled) {
  transform: translateY(-2px);
  box-shadow: var(--shadow-md);
}

.identity-btn:disabled {
  background: var(--bg-tertiary);
  color: var(--text-muted);
}

/* 矩阵容器 */
.matrix-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: var(--spacing-md);
}

.matrix-grid {
  display: grid;
  gap: var(--spacing-xs);
  grid-template-columns: repeat(var(--cols), 1fr);
  padding: var(--spacing-lg);
  background: var(--bg-secondary);
  border-radius: var(--radius-lg);
  border: 2px solid var(--border-color);
  position: relative;
}

.matrix-grid::before {
  content: '';
  position: absolute;
  top: -2px;
  left: -2px;
  right: -2px;
  bottom: -2px;
  background: var(--primary-gradient);
  border-radius: var(--radius-lg);
  z-index: -1;
  opacity: 0;
  transition: var(--transition-normal);
}

.matrix-grid:hover::before {
  opacity: 0.1;
}

.matrix-row {
  display: contents;
}

.matrix-cell {
  width: 60px;
  height: 60px;
  text-align: center;
  border: 2px solid var(--border-color);
  border-radius: var(--radius-md);
  font-size: 1rem;
  font-weight: 600;
  font-family: var(--font-family-mono);
  color: var(--text-primary);
  background: var(--bg-primary);
  transition: var(--transition-fast);
  position: relative;
}

.matrix-cell:hover {
  border-color: var(--primary-color);
  transform: scale(1.05);
  z-index: 1;
}

.matrix-cell:focus,
.matrix-cell.focused {
  border-color: var(--primary-color);
  box-shadow: 0 0 0 3px rgba(102, 126, 234, 0.2);
  outline: none;
  transform: scale(1.05);
  z-index: 2;
}

.matrix-cell::placeholder {
  color: var(--text-muted);
  font-weight: 400;
}

/* 矩阵信息 */
.matrix-info {
  display: flex;
  align-items: center;
  gap: var(--spacing-sm);
  padding: var(--spacing-sm) var(--spacing-md);
  background: var(--bg-tertiary);
  border-radius: var(--radius-md);
  border: 1px solid var(--border-color);
}

.matrix-size {
  font-family: var(--font-family-mono);
  font-weight: 600;
  color: var(--primary-color);
  font-size: 0.875rem;
}

/* 响应式设计 */
@media (max-width: 768px) {
  .dimension-controls {
    gap: var(--spacing-md);
  }

  .quick-actions {
    gap: var(--spacing-xs);
  }

  .action-btn {
    padding: var(--spacing-xs) var(--spacing-sm);
    font-size: 0.7rem;
  }

  .matrix-cell {
    width: 50px;
    height: 50px;
    font-size: 0.875rem;
  }

  .matrix-grid {
    padding: var(--spacing-md);
  }
}

@media (max-width: 480px) {
  .dimension-controls {
    flex-direction: column;
    gap: var(--spacing-sm);
  }

  .matrix-cell {
    width: 45px;
    height: 45px;
    font-size: 0.75rem;
  }

  .matrix-grid {
    padding: var(--spacing-sm);
    gap: 2px;
  }
}
</style>
