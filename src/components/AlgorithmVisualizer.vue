<template>
  <div class="algorithm-visualizer">
    <h2>核心算法步骤可视化教学</h2>
    <div class="algorithm-selection">
      <label for="algorithm-select">选择算法:</label>
      <select id="algorithm-select" v-model="selectedAlgorithm">
        <option value="">请选择一个算法</option>
        <option value="gaussianElimination">高斯消元法</option>
        <option value="luDecomposition">LU分解</option>
        <option value="gramSchmidt">Gram-Schmidt正交化</option>
        <!-- Add more algorithms as needed -->
      </select>
      <button @click="startVisualization" :disabled="!selectedAlgorithm || !matrixA.length">开始演示</button>
      <button @click="nextStep" :disabled="currentStepIndex >= visualizationSteps.length - 1">下一步</button>
      <button @click="prevStep" :disabled="currentStepIndex <= 0">上一步</button>
      <button @click="resetVisualization">重置</button>
    </div>

    <div v-if="visualizationSteps.length > 0" class="visualization-area">
      <h3>{{ selectedAlgorithmDisplay }} 步骤:</h3>
      <div class="step-container">
        <div class="step-description">
          <p>{{ currentStepDescription }}</p>
        </div>
        <div class="matrix-display">
          <div v-for="(row, rowIndex) in currentMatrix" :key="rowIndex" class="matrix-row">
            <span
              v-for="(cell, colIndex) in row"
              :key="`${rowIndex}-${colIndex}`"
              class="matrix-cell"
              :class="{ 'highlight-cell': isCellHighlighted(rowIndex, colIndex) }"
            >
              {{ formatNumber(cell) }}
            </span>
          </div>
        </div>
      </div>
    </div>
    <div v-else-if="selectedAlgorithm" class="no-steps-message">
      <p>请点击“开始演示”以查看 {{ selectedAlgorithmDisplay }} 的可视化步骤。</p>
    </div>
    <div v-if="error" class="error-message">
      <h3>错误:</h3>
      <p>{{ error }}</p>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed, watch } from 'vue';

const props = defineProps<{
  matrixA: number[][];
}>();

const selectedAlgorithm = ref<string>('');
const visualizationSteps = ref<{ matrix: number[][]; description: string; highlights: { row: number; col: number }[] }[]>([]);
const currentStepIndex = ref(-1);
const error = ref<string | null>(null);

const selectedAlgorithmDisplay = computed(() => {
  switch (selectedAlgorithm.value) {
    case 'gaussianElimination': return '高斯消元法';
    case 'luDecomposition': return 'LU分解';
    case 'gramSchmidt': return 'Gram-Schmidt正交化';
    default: return '';
  }
});

const currentStepDescription = computed(() => {
  return visualizationSteps.value[currentStepIndex.value]?.description || '请开始演示。';
});

const currentMatrix = computed(() => {
  return visualizationSteps.value[currentStepIndex.value]?.matrix || [];
});

const currentHighlights = computed(() => {
  return visualizationSteps.value[currentStepIndex.value]?.highlights || [];
});

const isCellHighlighted = (rowIndex: number, colIndex: number) => {
  return currentHighlights.value.some(h => h.row === rowIndex && h.col === colIndex);
};

// 格式化数字显示
const formatNumber = (num: number): string => {
  if (Math.abs(num) < 1e-10) return '0';
  if (Number.isInteger(num)) return num.toString();
  return num.toFixed(3);
};

const resetVisualization = () => {
  visualizationSteps.value = [];
  currentStepIndex.value = -1;
  error.value = null;
};

const startVisualization = () => {
  resetVisualization();
  error.value = null;

  if (props.matrixA.length === 0 || props.matrixA[0].length === 0) {
    error.value = '请先在矩阵编辑器中输入一个矩阵A。';
    return;
  }

  try {
    switch (selectedAlgorithm.value) {
      case 'gaussianElimination':
        visualizeGaussianElimination(props.matrixA);
        break;
      case 'luDecomposition':
        visualizeLUDecomposition(props.matrixA);
        break;
      case 'gramSchmidt':
        visualizeGramSchmidt(props.matrixA);
        break;
      default:
        error.value = '请选择一个有效的算法进行演示。';
        return;
    }
    currentStepIndex.value = 0; // Start at the first step
  } catch (e: any) {
    error.value = e.message;
    resetVisualization();
  }
};

const nextStep = () => {
  if (currentStepIndex.value < visualizationSteps.value.length - 1) {
    currentStepIndex.value++;
  }
};

const prevStep = () => {
  if (currentStepIndex.value > 0) {
    currentStepIndex.value--;
  }
};

// Helper function to deep clone a matrix
const cloneMatrix = (mat: number[][]): number[][] => {
  return mat.map(row => [...row]);
};

// --- Algorithm Visualizations ---

// Gaussian Elimination Visualization
const visualizeGaussianElimination = (initialMatrix: number[][]) => {
  const steps: typeof visualizationSteps.value = [];
  let matrix = cloneMatrix(initialMatrix);
  const rows = matrix.length;
  const cols = matrix[0].length;

  if (rows === 0 || cols === 0) {
    throw new Error('矩阵不能为空。');
  }

  steps.push({
    matrix: cloneMatrix(matrix),
    description: '初始矩阵。',
    highlights: []
  });

  let lead = 0;
  for (let r = 0; r < rows; r++) {
    if (lead >= cols) break;
    let i = r;
    while (i < rows && matrix[i][lead] === 0) {
      i++;
    }
    if (i < rows) {
      // Swap rows if necessary
      if (i !== r) {
        [matrix[r], matrix[i]] = [matrix[i], matrix[r]];
        steps.push({
          matrix: cloneMatrix(matrix),
          description: `交换第 ${i + 1} 行和第 ${r + 1} 行。`,
          highlights: []
        });
      }

      // Normalize the leading entry to 1
      let val = matrix[r][lead];
      if (val !== 0) {
        for (let j = 0; j < cols; j++) {
          matrix[r][j] /= val;
        }
        steps.push({
          matrix: cloneMatrix(matrix),
          description: `将第 ${r + 1} 行除以 ${val.toFixed(2)}，使主元为 1。`,
          highlights: [{ row: r, col: lead }]
        });
      }

      // Eliminate other entries in the column
      for (let i = 0; i < rows; i++) {
        if (i !== r) {
          let factor = matrix[i][lead];
          if (factor !== 0) {
            for (let j = 0; j < cols; j++) {
              matrix[i][j] -= factor * matrix[r][j];
            }
            steps.push({
              matrix: cloneMatrix(matrix),
              description: `将第 ${i + 1} 行减去 ${factor.toFixed(2)} 乘以第 ${r + 1} 行。`,
              highlights: [{ row: r, col: lead }, { row: i, col: lead }]
            });
          }
        }
      }
    }
    lead++;
  }
  steps.push({
    matrix: cloneMatrix(matrix),
    description: '完成高斯消元，得到行简化阶梯形矩阵。',
    highlights: []
  });
  visualizationSteps.value = steps;
};

// Placeholder for LU Decomposition Visualization
const visualizeLUDecomposition = (initialMatrix: number[][]) => {
  const steps: typeof visualizationSteps.value = [];
  let matrix = cloneMatrix(initialMatrix);
  const n = matrix.length;

  if (n === 0 || n !== matrix[0].length) {
    throw new Error('LU分解要求输入方阵。');
  }

  steps.push({
    matrix: cloneMatrix(matrix),
    description: '初始矩阵。',
    highlights: []
  });

  // Simplified LU decomposition for visualization (without detailed steps for L and U separately)
  // In a real scenario, this would involve tracking L and U matrices.
  // For visualization, we'll just show the process of getting to U.

  for (let k = 0; k < n - 1; k++) {
    for (let i = k + 1; i < n; i++) {
      if (matrix[k][k] === 0) {
        throw new Error('主元为零，无法进行LU分解（需要行交换）。');
      }
      let factor = matrix[i][k] / matrix[k][k];
      for (let j = k; j < n; j++) {
        matrix[i][j] -= factor * matrix[k][j];
      }
      steps.push({
        matrix: cloneMatrix(matrix),
        description: `利用第 ${k + 1} 行消元第 ${i + 1} 行，因子为 ${factor.toFixed(2)}。`,
        highlights: [{ row: k, col: k }, { row: i, col: k }]
      });
    }
  }

  steps.push({
    matrix: cloneMatrix(matrix),
    description: '完成LU分解（这里只展示了上三角矩阵U的形成）。',
    highlights: []
  });

  visualizationSteps.value = steps;
};

// Placeholder for Gram-Schmidt Orthogonalization Visualization
const visualizeGramSchmidt = (initialMatrix: number[][]) => {
  const steps: typeof visualizationSteps.value = [];
  let matrix = cloneMatrix(initialMatrix); // Treat columns as vectors

  if (matrix.length === 0 || matrix[0].length === 0) {
    throw new Error('矩阵不能为空。');
  }

  const numVectors = matrix[0].length; // Number of columns (vectors)
  const vectorDim = matrix.length;     // Dimension of each vector

  if (vectorDim === 0) {
    throw new Error('向量维度不能为零。');
  }

  // Convert matrix to column vectors for easier processing
  const vectors: number[][] = Array.from({ length: numVectors }, (_, colIndex) =>
    Array.from({ length: vectorDim }, (_, rowIndex) => matrix[rowIndex][colIndex])
  );

  const orthogonalVectors: number[][] = [];

  steps.push({
    matrix: cloneMatrix(matrix),
    description: '初始向量组（矩阵的列）。',
    highlights: []
  });

  for (let i = 0; i < numVectors; i++) {
    let v = [...vectors[i]]; // Current vector to orthogonalize
    let u_sum = Array(vectorDim).fill(0);

    for (let j = 0; j < i; j++) {
      const u_j = orthogonalVectors[j];
      const dot_v_uj = dotProduct(vectors[i], u_j);
      const dot_uj_uj = dotProduct(u_j, u_j);

      if (dot_uj_uj === 0) {
        throw new Error(`向量 u${j + 1} 的范数为零，无法进行Gram-Schmidt正交化。`);
      }

      const scalarProjection = dot_v_uj / dot_uj_uj;
      for (let k = 0; k < vectorDim; k++) {
        u_sum[k] += scalarProjection * u_j[k];
      }

      steps.push({
        matrix: createMatrixFromVectors(vectors, orthogonalVectors), // Visualizes original and orthogonal vectors
        description: `计算向量 v${i + 1} 在正交向量 u${j + 1} 上的投影并累加。`,
        highlights: [] // Difficult to highlight specific cells for vector operations directly
      });
    }

    const orthogonal_v_i = v.map((val, idx) => val - u_sum[idx]);
    orthogonalVectors.push(orthogonal_v_i);

    steps.push({
      matrix: createMatrixFromVectors(vectors, orthogonalVectors),
      description: `得到第 ${i + 1} 个正交向量 u${i + 1}。`,
      highlights: []
    });
  }

  steps.push({
    matrix: createMatrixFromVectors(vectors, orthogonalVectors),
    description: '完成Gram-Schmidt正交化，得到正交向量组。',
    highlights: []
  });

  visualizationSteps.value = steps;
};

// Helper for Gram-Schmidt: Dot product
const dotProduct = (vec1: number[], vec2: number[]): number => {
  return vec1.reduce((sum, val, idx) => sum + val * vec2[idx], 0);
};

// Helper for Gram-Schmidt: Create a matrix for visualization from vectors
const createMatrixFromVectors = (
  original: number[][],
  orthogonal: number[][]
): number[][] => {
  const rows = original[0].length; // Dimension of vectors
  // const cols = original.length + orthogonal.length; // Total number of vectors (original + orthogonal) // unused
  const combinedMatrix: number[][] = [];

  for (let i = 0; i < rows; i++) {
    const row: number[] = [];
    // Add original vectors
    for (let j = 0; j < original.length; j++) {
      row.push(original[j][i]);
    }
    // Add orthogonal vectors
    for (let j = 0; j < orthogonal.length; j++) {
      row.push(orthogonal[j][i]);
    }
    combinedMatrix.push(row);
  }
  return combinedMatrix;
};

watch(selectedAlgorithm, () => {
  resetVisualization();
});
</script>

<style scoped>
.algorithm-visualizer {
  margin-top: 30px;
  padding: 20px;
  border: 1px solid #eee;
  border-radius: 8px;
  background-color: #f9f9f9;
}

h2 {
  color: #333;
  margin-bottom: 20px;
}

.algorithm-selection {
  display: flex;
  gap: 10px;
  align-items: center;
  margin-bottom: 20px;
  flex-wrap: wrap;
}

.algorithm-selection label {
  font-weight: bold;
  color: #555;
}

.algorithm-selection select {
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
  font-size: 1em;
  min-width: 180px;
}

.algorithm-selection button {
  padding: 10px 15px;
  background-color: #28a745;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-size: 1em;
  transition: background-color 0.2s ease-in-out;
}

.algorithm-selection button:hover:not(:disabled) {
  background-color: #218838;
}

.algorithm-selection button:disabled {
  background-color: #cccccc;
  cursor: not-allowed;
}

.visualization-area {
  margin-top: 20px;
  padding: 15px;
  border: 1px solid #cce5ff;
  border-radius: 8px;
  background-color: #e6f3ff;
  color: #004085;
  text-align: left;
}

.visualization-area h3 {
  margin-top: 0;
  color: #004085;
  margin-bottom: 15px;
}

.step-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 20px;
}

.step-description {
  background-color: #d2eafb;
  padding: 10px 15px;
  border-radius: 5px;
  font-size: 1.1em;
  color: #0056b3;
  width: 100%;
  box-sizing: border-box;
}

.matrix-display {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 5px;
  background-color: #ffffff;
  padding: 15px;
  border-radius: 8px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}

.matrix-row {
  display: flex;
  gap: 5px;
}

.matrix-cell {
  width: 60px;
  height: 40px;
  display: flex;
  align-items: center;
  justify-content: center;
  border: 1px solid #a3d9ff;
  border-radius: 4px;
  font-size: 1em;
  font-weight: bold;
  background-color: #b3e0ff;
  color: #004085;
  transition: background-color 0.3s ease, border-color 0.3s ease;
}

.highlight-cell {
  background-color: #ffeb3b; /* Highlight color */
  border-color: #ffa000;
  color: #c08d00;
  box-shadow: 0 0 8px rgba(255, 160, 0, 0.5);
}

.no-steps-message {
  margin-top: 20px;
  padding: 15px;
  border: 1px solid #ffe0b2;
  border-radius: 8px;
  background-color: #fff3e0;
  color: #e65100;
}

.error-message {
  margin-top: 20px;
  padding: 15px;
  border: 1px solid #f5c6cb;
  border-radius: 8px;
  background-color: #f8d7da;
  color: #721c24;
}
</style>
