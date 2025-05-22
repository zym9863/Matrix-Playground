<template>
  <div class="matrix-operations card">
    <div class="section-header">
      <h2><i class="fas fa-calculator"></i> 矩阵运算</h2>
      <p class="section-description">执行各种矩阵运算操作</p>
    </div>

    <!-- 运算按钮区域 -->
    <div class="operations-grid">
      <!-- 基础运算 -->
      <div class="operation-category">
        <h3><i class="fas fa-plus-minus"></i> 基础运算</h3>
        <div class="operation-buttons">
          <button @click="addMatrices" class="operation-btn add-btn">
            <i class="fas fa-plus"></i>
            <span>矩阵加法</span>
            <small>A + B</small>
          </button>
          <button @click="subtractMatrices" class="operation-btn subtract-btn">
            <i class="fas fa-minus"></i>
            <span>矩阵减法</span>
            <small>A - B</small>
          </button>
          <button @click="multiplyMatrices" class="operation-btn multiply-btn">
            <i class="fas fa-times"></i>
            <span>矩阵乘法</span>
            <small>A × B</small>
          </button>
        </div>
      </div>

      <!-- 数乘运算 -->
      <div class="operation-category">
        <h3><i class="fas fa-hashtag"></i> 数乘运算</h3>
        <div class="scalar-operation">
          <div class="scalar-input-group">
            <label for="scalar">标量值:</label>
            <input
              type="number"
              id="scalar"
              v-model.number="scalar"
              placeholder="输入标量"
              class="scalar-input"
              step="0.1"
            />
          </div>
          <button @click="scalarMultiplyMatrix" class="operation-btn scalar-btn">
            <i class="fas fa-asterisk"></i>
            <span>数乘矩阵</span>
            <small>{{ scalar || 'k' }} × A</small>
          </button>
        </div>
      </div>

      <!-- 矩阵变换 -->
      <div class="operation-category">
        <h3><i class="fas fa-exchange-alt"></i> 矩阵变换</h3>
        <div class="operation-buttons">
          <button @click="transposeMatrixA" class="operation-btn transpose-btn">
            <i class="fas fa-redo"></i>
            <span>转置 A</span>
            <small>A<sup>T</sup></small>
          </button>
          <button @click="transposeMatrixB" class="operation-btn transpose-btn">
            <i class="fas fa-redo"></i>
            <span>转置 B</span>
            <small>B<sup>T</sup></small>
          </button>
        </div>
      </div>

      <!-- 高级运算 -->
      <div class="operation-category">
        <h3><i class="fas fa-cogs"></i> 高级运算</h3>
        <div class="operation-buttons">
          <button @click="determinantMatrixA" class="operation-btn determinant-btn">
            <i class="fas fa-square-root-alt"></i>
            <span>行列式 A</span>
            <small>det(A)</small>
          </button>
          <button @click="determinantMatrixB" class="operation-btn determinant-btn">
            <i class="fas fa-square-root-alt"></i>
            <span>行列式 B</span>
            <small>det(B)</small>
          </button>
          <button @click="inverseMatrixA" class="operation-btn inverse-btn">
            <i class="fas fa-undo"></i>
            <span>逆矩阵 A</span>
            <small>A<sup>-1</sup></small>
          </button>
          <button @click="inverseMatrixB" class="operation-btn inverse-btn">
            <i class="fas fa-undo"></i>
            <span>逆矩阵 B</span>
            <small>B<sup>-1</sup></small>
          </button>
        </div>
      </div>
    </div>

    <!-- 结果显示区域 -->
    <div v-if="resultMatrix.length > 0 || resultScalar !== null || error" class="results-section">
      <div v-if="resultMatrix.length > 0" class="result-display matrix-result">
        <div class="result-header">
          <h3><i class="fas fa-check-circle"></i> 运算结果</h3>
          <button @click="clearResults" class="clear-btn">
            <i class="fas fa-times"></i>
          </button>
        </div>
        <div class="matrix-display">
          <div class="matrix-grid" :style="{ '--result-cols': resultMatrix[0]?.length || 1 }">
            <div v-for="(row, rowIndex) in resultMatrix" :key="rowIndex" class="matrix-row">
              <span v-for="(cell, colIndex) in row" :key="`${rowIndex}-${colIndex}`" class="matrix-cell">
                {{ formatNumber(cell) }}
              </span>
            </div>
          </div>
        </div>
      </div>

      <div v-if="resultScalar !== null" class="result-display scalar-result">
        <div class="result-header">
          <h3><i class="fas fa-check-circle"></i> 运算结果</h3>
          <button @click="clearResults" class="clear-btn">
            <i class="fas fa-times"></i>
          </button>
        </div>
        <div class="scalar-display">
          <span class="scalar-value">{{ formatNumber(resultScalar) }}</span>
        </div>
      </div>

      <div v-if="error" class="error-message">
        <div class="error-header">
          <h3><i class="fas fa-exclamation-triangle"></i> 错误</h3>
          <button @click="clearResults" class="clear-btn">
            <i class="fas fa-times"></i>
          </button>
        </div>
        <p>{{ error }}</p>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';

const props = defineProps<{
  matrixA: number[][];
  matrixB: number[][];
}>();

const resultMatrix = ref<number[][]>([]);
const resultScalar = ref<number | null>(null);
const error = ref<string | null>(null);
const scalar = ref(0);

const clearResults = () => {
  resultMatrix.value = [];
  resultScalar.value = null;
  error.value = null;
};

// 格式化数字显示
const formatNumber = (num: number): string => {
  if (Math.abs(num) < 1e-10) return '0';
  if (Number.isInteger(num)) return num.toString();
  return num.toFixed(3);
};

// Helper function to check if matrices have same dimensions
const haveSameDimensions = (mat1: number[][], mat2: number[][]): boolean => {
  if (mat1.length === 0 || mat2.length === 0) return false;
  return mat1.length === mat2.length && mat1[0].length === mat2[0].length;
};

// Helper function to check if a matrix is square
const isSquareMatrix = (mat: number[][]): boolean => {
  if (mat.length === 0) return false;
  return mat.length === mat[0].length;
};

// Helper function to create a matrix of given dimensions, filled with zeros
const createZeroMatrix = (rows: number, cols: number): number[][] => {
  const newMatrix: number[][] = [];
  for (let i = 0; i < rows; i++) {
    newMatrix.push(Array(cols).fill(0));
  }
  return newMatrix;
};

// Matrix Addition
const addMatrices = () => {
  clearResults();
  if (!haveSameDimensions(props.matrixA, props.matrixB)) {
    error.value = '矩阵加法要求两个矩阵具有相同的维度。';
    return;
  }

  const rows = props.matrixA.length;
  const cols = props.matrixA[0].length;
  const result = createZeroMatrix(rows, cols);

  for (let i = 0; i < rows; i++) {
    for (let j = 0; j < cols; j++) {
      result[i][j] = props.matrixA[i][j] + props.matrixB[i][j];
    }
  }
  resultMatrix.value = result;
};

// Matrix Subtraction
const subtractMatrices = () => {
  clearResults();
  if (!haveSameDimensions(props.matrixA, props.matrixB)) {
    error.value = '矩阵减法要求两个矩阵具有相同的维度。';
    return;
  }

  const rows = props.matrixA.length;
  const cols = props.matrixA[0].length;
  const result = createZeroMatrix(rows, cols);

  for (let i = 0; i < rows; i++) {
    for (let j = 0; j < cols; j++) {
      result[i][j] = props.matrixA[i][j] - props.matrixB[i][j];
    }
  }
  resultMatrix.value = result;
};

// Scalar Multiplication
const scalarMultiplyMatrix = () => {
  clearResults();
  if (props.matrixA.length === 0 || props.matrixA[0].length === 0) {
    error.value = '矩阵A不能为空。';
    return;
  }

  const rows = props.matrixA.length;
  const cols = props.matrixA[0].length;
  const result = createZeroMatrix(rows, cols);

  for (let i = 0; i < rows; i++) {
    for (let j = 0; j < cols; j++) {
      result[i][j] = props.matrixA[i][j] * scalar.value;
    }
  }
  resultMatrix.value = result;
};

// Matrix Multiplication
const multiplyMatrices = () => {
  clearResults();
  if (props.matrixA.length === 0 || props.matrixB.length === 0) {
    error.value = '两个矩阵都不能为空。';
    return;
  }
  if (props.matrixA[0].length !== props.matrixB.length) {
    error.value = '矩阵A的列数必须等于矩阵B的行数才能进行乘法运算。';
    return;
  }

  const rowsA = props.matrixA.length;
  const colsA = props.matrixA[0].length;
  // const rowsB = props.matrixB.length; // Unused variable
  const colsB = props.matrixB[0].length;

  const result = createZeroMatrix(rowsA, colsB);

  for (let i = 0; i < rowsA; i++) {
    for (let j = 0; j < colsB; j++) {
      for (let k = 0; k < colsA; k++) {
        result[i][j] += props.matrixA[i][k] * props.matrixB[k][j];
      }
    }
  }
  resultMatrix.value = result;
};

// Transpose Matrix
const transposeMatrix = (mat: number[][]): number[][] => {
  if (mat.length === 0) return [];
  const rows = mat.length;
  const cols = mat[0].length;
  const result = createZeroMatrix(cols, rows);

  for (let i = 0; i < rows; i++) {
    for (let j = 0; j < cols; j++) {
      result[j][i] = mat[i][j];
    }
  }
  return result;
};

const transposeMatrixA = () => {
  clearResults();
  if (props.matrixA.length === 0) {
    error.value = '矩阵A不能为空。';
    return;
  }
  resultMatrix.value = transposeMatrix(props.matrixA);
};

const transposeMatrixB = () => {
  clearResults();
  if (props.matrixB.length === 0) {
    error.value = '矩阵B不能为空。';
    return;
  }
  resultMatrix.value = transposeMatrix(props.matrixB);
};


// Determinant of a Matrix
const determinant = (mat: number[][]): number => {
  const n = mat.length;
  if (n === 0) return 0;
  if (n !== mat[0].length) {
    throw new Error('求行列式要求矩阵是方阵。');
  }

  if (n === 1) {
    return mat[0][0];
  }
  if (n === 2) {
    return mat[0][0] * mat[1][1] - mat[0][1] * mat[1][0];
  }

  let det = 0;
  for (let j = 0; j < n; j++) {
    const minor = getMinor(mat, 0, j);
    det += (j % 2 === 0 ? 1 : -1) * mat[0][j] * determinant(minor);
  }
  return det;
};

const getMinor = (mat: number[][], row: number, col: number): number[][] => {
  const minor: number[][] = [];
  for (let i = 0; i < mat.length; i++) {
    if (i === row) continue;
    const currentRow: number[] = [];
    for (let j = 0; j < mat[i].length; j++) {
      if (j === col) continue;
      currentRow.push(mat[i][j]);
    }
    minor.push(currentRow);
  }
  return minor;
};

const determinantMatrixA = () => {
  clearResults();
  if (!isSquareMatrix(props.matrixA)) {
    error.value = '求行列式要求矩阵A是方阵。';
    return;
  }
  try {
    resultScalar.value = determinant(props.matrixA);
  } catch (e: any) {
    error.value = e.message;
  }
};

const determinantMatrixB = () => {
  clearResults();
  if (!isSquareMatrix(props.matrixB)) {
    error.value = '求行列式要求矩阵B是方阵。';
    return;
  }
  try {
    resultScalar.value = determinant(props.matrixB);
  } catch (e: any) {
    error.value = e.message;
  }
};

// Inverse of a Matrix
const inverse = (mat: number[][]): number[][] => {
  const n = mat.length;
  if (n === 0 || n !== mat[0].length) {
    throw new Error('求逆矩阵要求矩阵是方阵。');
  }

  const det = determinant(mat);
  if (det === 0) {
    throw new Error('行列式为零，矩阵不可逆。');
  }

  // Adjoint matrix / 伴随矩阵
  const adj: number[][] = createZeroMatrix(n, n);
  for (let i = 0; i < n; i++) {
    for (let j = 0; j < n; j++) {
      const minor = getMinor(mat, i, j);
      adj[j][i] = ((i + j) % 2 === 0 ? 1 : -1) * determinant(minor); // Transpose of cofactor matrix
    }
  }

  // Inverse matrix = (1 / det) * adj
  const inv: number[][] = createZeroMatrix(n, n);
  for (let i = 0; i < n; i++) {
    for (let j = 0; j < n; j++) {
      inv[i][j] = adj[i][j] / det;
    }
  }
  return inv;
};

const inverseMatrixA = () => {
  clearResults();
  try {
    resultMatrix.value = inverse(props.matrixA);
  } catch (e: any) {
    error.value = e.message;
  }
};

const inverseMatrixB = () => {
  clearResults();
  try {
    resultMatrix.value = inverse(props.matrixB);
  } catch (e: any) {
    error.value = e.message;
  }
};
</script>

<style scoped>
.matrix-operations {
  padding: var(--spacing-xl);
  margin-bottom: var(--spacing-xl);
}

.section-header {
  text-align: center;
  margin-bottom: var(--spacing-xl);
}

.section-header h2 {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: var(--spacing-sm);
  margin-bottom: var(--spacing-sm);
}

.section-description {
  color: var(--text-secondary);
  margin: 0;
}

/* 运算网格布局 */
.operations-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: var(--spacing-xl);
  margin-bottom: var(--spacing-xl);
}

.operation-category {
  background: var(--bg-secondary);
  border-radius: var(--radius-lg);
  padding: var(--spacing-lg);
  border: 1px solid var(--border-color);
  transition: var(--transition-normal);
}

.operation-category:hover {
  transform: translateY(-2px);
  box-shadow: var(--shadow-md);
}

.operation-category h3 {
  display: flex;
  align-items: center;
  gap: var(--spacing-sm);
  color: var(--primary-color);
  font-size: 1.1rem;
  margin-bottom: var(--spacing-lg);
  padding-bottom: var(--spacing-sm);
  border-bottom: 2px solid var(--border-color);
}

.operation-buttons {
  display: flex;
  flex-direction: column;
  gap: var(--spacing-sm);
}

.operation-btn {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: var(--spacing-xs);
  padding: var(--spacing-md);
  border-radius: var(--radius-md);
  transition: var(--transition-normal);
  position: relative;
  overflow: hidden;
  min-height: 80px;
  justify-content: center;
}

.operation-btn i {
  font-size: 1.5rem;
}

.operation-btn span {
  font-weight: 600;
  font-size: 0.875rem;
}

.operation-btn small {
  font-size: 0.75rem;
  opacity: 0.8;
  font-family: var(--font-family-mono);
}

.operation-btn::before {
  content: '';
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.2), transparent);
  transition: var(--transition-normal);
}

.operation-btn:hover::before {
  left: 100%;
}

.operation-btn:hover {
  transform: translateY(-2px);
  box-shadow: var(--shadow-md);
}

/* 按钮颜色主题 */
.add-btn {
  background: var(--success-gradient);
  color: white;
}

.subtract-btn {
  background: var(--warning-gradient);
  color: white;
}

.multiply-btn {
  background: var(--primary-gradient);
  color: white;
}

.scalar-btn {
  background: var(--secondary-gradient);
  color: white;
}

.transpose-btn {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
}

.determinant-btn {
  background: linear-gradient(135deg, #f093fb 0%, #f5576c 100%);
  color: white;
}

.inverse-btn {
  background: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%);
  color: white;
}

/* 数乘运算特殊布局 */
.scalar-operation {
  display: flex;
  flex-direction: column;
  gap: var(--spacing-md);
}

.scalar-input-group {
  display: flex;
  flex-direction: column;
  gap: var(--spacing-sm);
}

.scalar-input-group label {
  font-weight: 600;
  color: var(--text-secondary);
  font-size: 0.875rem;
}

.scalar-input {
  padding: var(--spacing-sm) var(--spacing-md);
  border-radius: var(--radius-md);
  border: 2px solid var(--border-color);
  font-family: var(--font-family-mono);
  font-weight: 600;
  text-align: center;
  transition: var(--transition-fast);
}

.scalar-input:focus {
  border-color: var(--primary-color);
  box-shadow: 0 0 0 3px rgba(102, 126, 234, 0.1);
}

/* 结果显示区域 */
.results-section {
  margin-top: var(--spacing-xl);
  padding-top: var(--spacing-xl);
  border-top: 2px solid var(--border-color);
}

.result-display {
  border-radius: var(--radius-lg);
  overflow: hidden;
  box-shadow: var(--shadow-md);
  animation: slideIn 0.3s ease-out;
}

@keyframes slideIn {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.result-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: var(--spacing-lg);
  background: var(--success-gradient);
  color: white;
}

.result-header h3 {
  display: flex;
  align-items: center;
  gap: var(--spacing-sm);
  margin: 0;
  font-size: 1.1rem;
}

.clear-btn {
  background: rgba(255, 255, 255, 0.2);
  color: white;
  border: none;
  border-radius: var(--radius-sm);
  padding: var(--spacing-xs);
  cursor: pointer;
  transition: var(--transition-fast);
  width: 32px;
  height: 32px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.clear-btn:hover {
  background: rgba(255, 255, 255, 0.3);
}

.matrix-display {
  padding: var(--spacing-lg);
  background: var(--bg-primary);
}

.matrix-grid {
  display: grid;
  gap: var(--spacing-xs);
  grid-template-columns: repeat(var(--result-cols), 1fr);
  justify-items: center;
}

.matrix-row {
  display: contents;
}

.matrix-cell {
  width: 60px;
  height: 50px;
  display: flex;
  align-items: center;
  justify-content: center;
  border: 2px solid var(--success-color);
  border-radius: var(--radius-md);
  font-size: 1rem;
  font-weight: 600;
  font-family: var(--font-family-mono);
  background: rgba(79, 172, 254, 0.1);
  color: var(--success-color);
  transition: var(--transition-fast);
}

.matrix-cell:hover {
  transform: scale(1.05);
  box-shadow: var(--shadow-sm);
}

.scalar-display {
  padding: var(--spacing-xl);
  background: var(--bg-primary);
  text-align: center;
}

.scalar-value {
  font-size: 3rem;
  font-weight: 700;
  font-family: var(--font-family-mono);
  color: var(--success-color);
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.1);
}

/* 错误消息样式 */
.error-message {
  border-radius: var(--radius-lg);
  overflow: hidden;
  box-shadow: var(--shadow-md);
  animation: slideIn 0.3s ease-out;
}

.error-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: var(--spacing-lg);
  background: var(--danger-gradient);
  color: white;
}

.error-header h3 {
  display: flex;
  align-items: center;
  gap: var(--spacing-sm);
  margin: 0;
  font-size: 1.1rem;
}

.error-message p {
  padding: var(--spacing-lg);
  margin: 0;
  background: var(--bg-primary);
  color: var(--danger-color);
  font-weight: 500;
}

/* 响应式设计 */
@media (max-width: 768px) {
  .operations-grid {
    grid-template-columns: 1fr;
    gap: var(--spacing-lg);
  }

  .operation-category {
    padding: var(--spacing-md);
  }

  .operation-btn {
    min-height: 70px;
    padding: var(--spacing-sm);
  }

  .operation-btn i {
    font-size: 1.25rem;
  }

  .matrix-cell {
    width: 50px;
    height: 40px;
    font-size: 0.875rem;
  }

  .scalar-value {
    font-size: 2rem;
  }
}

@media (max-width: 480px) {
  .matrix-operations {
    padding: var(--spacing-lg);
  }

  .operation-btn {
    min-height: 60px;
  }

  .matrix-cell {
    width: 40px;
    height: 35px;
    font-size: 0.75rem;
  }

  .result-header,
  .error-header {
    padding: var(--spacing-md);
  }

  .matrix-display,
  .scalar-display,
  .error-message p {
    padding: var(--spacing-md);
  }
}
</style>
