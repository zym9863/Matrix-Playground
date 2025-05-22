# Matrix Playground

English | [中文](./README.md)

🧮 A modern interactive matrix computation and linear algebra learning platform

## 📖 Project Overview

Matrix Playground is a modern web application built with Vue 3 + TypeScript + Vite, designed to make linear algebra learning more intuitive and engaging. Through visual matrix editors, rich computational features, and algorithm demonstrations, it helps users deeply understand the principles and processes of matrix operations.

## ✨ Key Features

### 🎯 Core Functionality
- **Interactive Matrix Editor**: Support dynamic matrix dimension adjustment (1×1 to 10×10)
- **Rich Matrix Operations**: Addition, subtraction, multiplication, transpose, determinant, inverse matrix, etc.
- **Algorithm Visualization**: Step-by-step demonstrations of Gaussian elimination, LU decomposition, Gram-Schmidt orthogonalization
- **Real-time Computation**: Instant display of operation results with number formatting support

### 🎨 User Experience
- **Modern UI Design**: Gradient colors and smooth animations
- **Responsive Layout**: Perfect adaptation for desktop and mobile devices
- **Intuitive Interface**: Clear visual feedback and user guidance
- **Localized Interface**: Fully localized user interface

## 🏗️ Project Structure

```
src/
├── components/
│   ├── MatrixEditor.vue      # Matrix editor component
│   ├── MatrixOperations.vue  # Matrix operations component
│   ├── AlgorithmVisualizer.vue # Algorithm visualization component
│   └── HelloWorld.vue        # Example component
├── assets/                   # Static resources
├── App.vue                   # Main application component
├── main.ts                   # Application entry point
├── style.css                 # Global styles
└── vite-env.d.ts            # TypeScript type declarations
```

## 🚀 Quick Start

### Requirements
- Node.js >= 16.0.0
- npm >= 7.0.0 or yarn >= 1.22.0

### Install Dependencies
```bash
npm install
# or
yarn install
```

### Development Mode
```bash
npm run dev
# or
yarn dev
```

### Build for Production
```bash
npm run build
# or
yarn build
```

### Preview Production Build
```bash
npm run preview
# or
yarn preview
```

## 🧮 Feature Details

### Matrix Editor
- **Dynamic Dimension Adjustment**: Adjust matrix rows and columns through intuitive controls
- **Real-time Input Validation**: Support for integer and decimal input
- **Visual Focus Indication**: Clear display of currently edited cell
- **Matrix Information Display**: Real-time display of matrix dimension information

### Matrix Operations
#### Basic Operations
- **Matrix Addition** (A + B): Element-wise addition
- **Matrix Subtraction** (A - B): Element-wise subtraction
- **Matrix Multiplication** (A × B): Standard matrix multiplication

#### Advanced Operations
- **Matrix Transpose** (A^T): Row-column interchange
- **Determinant Calculation** (det(A)): Calculated using recursive expansion
- **Matrix Inverse** (A^(-1)): Based on Gauss-Jordan elimination
- **Scalar Multiplication** (k × A): Matrix multiplied by scalar

### Algorithm Visualization
- **Gaussian Elimination**: Step-by-step demonstration of linear system solving
- **LU Decomposition**: Shows matrix decomposition into lower and upper triangular matrices
- **Gram-Schmidt Orthogonalization**: Demonstrates vector orthogonalization process

## 🛠️ Tech Stack

- **Frontend Framework**: Vue 3 (Composition API)
- **Development Language**: TypeScript
- **Build Tool**: Vite
- **Styling**: CSS3 (CSS Variables + Grid/Flexbox)
- **Icon Library**: Font Awesome 6
- **Fonts**: Inter + JetBrains Mono

## 🎨 Design Features

- **Modern Color Scheme**: Visual design system based on gradients
- **Smooth Animations**: CSS3 animations and transition effects
- **Responsive Design**: Mobile-friendly adaptive layout
- **Accessibility Support**: Compliant with Web accessibility standards

## 📱 Browser Support

- Chrome >= 88
- Firefox >= 85
- Safari >= 14
- Edge >= 88

## 🤝 Contributing

Welcome to submit Issues and Pull Requests to help improve the project!

### Development Workflow
1. Fork this repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details

## 🙏 Acknowledgments

- Vue.js team for the excellent framework
- Vite team for the fast build tool
- Font Awesome for the icon library
- All developers who contribute to the open source community

---

**Make mathematics more fun!** 🎉
