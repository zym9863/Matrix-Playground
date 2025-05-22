<script setup lang="ts">
import { ref } from 'vue';
import MatrixEditor from './components/MatrixEditor.vue';
import MatrixOperations from './components/MatrixOperations.vue';
import AlgorithmVisualizer from './components/AlgorithmVisualizer.vue';

const matrixA = ref<number[][]>([]);
const matrixB = ref<number[][]>([]);

const updateMatrixA = (newMatrix: number[][]) => {
  matrixA.value = newMatrix;
};

const updateMatrixB = (newMatrix: number[][]) => {
  matrixB.value = newMatrix;
};
</script>

<template>
  <div id="app">
    <!-- 头部区域 -->
    <header class="app-header">
      <div class="header-content">
        <div class="logo-section">
          <i class="fas fa-calculator header-icon"></i>
          <h1>矩阵魔方</h1>
        </div>
        <p class="subtitle">探索线性代数的奇妙世界</p>
      </div>
      <div class="header-decoration"></div>
    </header>

    <!-- 主要内容区域 -->
    <main class="main-content">
      <div class="container">
        <!-- 矩阵编辑区域 -->
        <section class="matrix-section">
          <div class="section-header">
            <h2><i class="fas fa-edit"></i> 矩阵编辑器</h2>
            <p class="section-description">创建和编辑您的矩阵</p>
          </div>
          <div class="matrix-editors">
            <div class="matrix-editor-wrapper">
              <h3>矩阵 A</h3>
              <MatrixEditor @update:matrix="updateMatrixA" />
            </div>
            <div class="matrix-editor-wrapper">
              <h3>矩阵 B</h3>
              <MatrixEditor @update:matrix="updateMatrixB" />
            </div>
          </div>
        </section>

        <!-- 矩阵运算区域 -->
        <section class="operations-section">
          <MatrixOperations :matrixA="matrixA" :matrixB="matrixB" />
        </section>

        <!-- 算法可视化区域 -->
        <section class="visualizer-section">
          <AlgorithmVisualizer :matrixA="matrixA" />
        </section>
      </div>
    </main>

    <!-- 页脚 -->
    <footer class="app-footer">
      <div class="footer-content">
        <p>&copy; 2025 矩阵魔方 - 让数学变得更有趣</p>
      </div>
    </footer>
  </div>
</template>

<style>
#app {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

/* 头部样式 */
.app-header {
  position: relative;
  background: var(--primary-gradient);
  color: white;
  padding: var(--spacing-2xl) 0;
  text-align: center;
  overflow: hidden;
}

.header-content {
  position: relative;
  z-index: 2;
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 var(--spacing-lg);
}

.logo-section {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: var(--spacing-md);
  margin-bottom: var(--spacing-md);
}

.header-icon {
  font-size: 3rem;
  animation: float 3s ease-in-out infinite;
}

@keyframes float {
  0%, 100% { transform: translateY(0px); }
  50% { transform: translateY(-10px); }
}

.app-header h1 {
  font-size: 3rem;
  margin: 0;
  color: white;
  background: none;
  -webkit-text-fill-color: white;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
}

.subtitle {
  font-size: 1.2rem;
  opacity: 0.9;
  margin: 0;
  font-weight: 300;
}

.header-decoration {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100"><defs><pattern id="grid" width="10" height="10" patternUnits="userSpaceOnUse"><path d="M 10 0 L 0 0 0 10" fill="none" stroke="rgba(255,255,255,0.1)" stroke-width="0.5"/></pattern></defs><rect width="100" height="100" fill="url(%23grid)"/></svg>');
  opacity: 0.3;
}

/* 主要内容区域 */
.main-content {
  flex: 1;
  padding: var(--spacing-2xl) 0;
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 var(--spacing-lg);
}

/* 区域样式 */
section {
  margin-bottom: var(--spacing-2xl);
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
  font-size: 2rem;
  margin-bottom: var(--spacing-sm);
}

.section-header i {
  color: var(--primary-color);
}

.section-description {
  color: var(--text-secondary);
  font-size: 1.1rem;
  margin: 0;
}

/* 矩阵编辑区域 */
.matrix-editors {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(400px, 1fr));
  gap: var(--spacing-xl);
  margin-bottom: var(--spacing-xl);
}

.matrix-editor-wrapper {
  background: var(--bg-primary);
  border-radius: var(--radius-xl);
  padding: var(--spacing-xl);
  box-shadow: var(--shadow-lg);
  border: 1px solid var(--border-color);
  transition: var(--transition-normal);
}

.matrix-editor-wrapper:hover {
  transform: translateY(-4px);
  box-shadow: var(--shadow-xl);
}

.matrix-editor-wrapper h3 {
  text-align: center;
  color: var(--primary-color);
  font-size: 1.5rem;
  margin-bottom: var(--spacing-lg);
  padding-bottom: var(--spacing-sm);
  border-bottom: 2px solid var(--border-color);
}

/* 页脚样式 */
.app-footer {
  background: var(--text-primary);
  color: white;
  padding: var(--spacing-lg) 0;
  text-align: center;
  margin-top: auto;
}

.footer-content {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 var(--spacing-lg);
}

.footer-content p {
  margin: 0;
  opacity: 0.8;
}

/* 响应式设计 */
@media (max-width: 768px) {
  .app-header h1 {
    font-size: 2rem;
  }

  .header-icon {
    font-size: 2rem;
  }

  .subtitle {
    font-size: 1rem;
  }

  .matrix-editors {
    grid-template-columns: 1fr;
    gap: var(--spacing-lg);
  }

  .matrix-editor-wrapper {
    padding: var(--spacing-lg);
  }

  .section-header h2 {
    font-size: 1.5rem;
  }
}

@media (max-width: 480px) {
  .container {
    padding: 0 var(--spacing-md);
  }

  .main-content {
    padding: var(--spacing-lg) 0;
  }

  .app-header {
    padding: var(--spacing-xl) 0;
  }
}
</style>
