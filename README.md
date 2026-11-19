# LLM Pipeline Explorer — Interactive AI Learning Laboratory

An interactive educational platform and 3D scientific visualization lab designed to teach how raw natural language text is transformed by modern Large Language Models: from Unicode strings to tokens, subwords, numerical token IDs, high-dimensional neural embeddings (384-D), Principal Component Analysis (PCA) dimensionality reduction, and interactive 3D semantic coordinate space.

---

## 1. Project Overview

**LLM Pipeline Explorer** bridges the gap between abstract natural language processing theory and intuitive geometric visualization. Rather than treating neural models as opaque black boxes, this laboratory allows students, engineers, and researchers to experiment directly with:

1. **The Tokenization Layer**: Corpus ingestion, BPE/WordPiece subword decomposition, vocabulary capacity limits, and drag-and-drop pruning.
2. **The Embedding Layer**: True client-side neural embedding inference via Hugging Face Transformers.js (`all-MiniLM-L6-v2`) running via WebAssembly and ONNX Runtime in the browser.
3. **The Geometric Dimension**: Mathematically exact Dual PCA projection from 384 dimensions to 3D Cartesian coordinates, paired with high-dimensional cosine similarity nearest-neighbor discovery.

---

## 2. Project Architecture & Strict Two-File Structure

In accordance with strict production requirements, this entire application is self-contained within exactly **two files**:

```text
LLM pipeline Explorer/
├── index.html     # Complete UI markup, CSS tokens, WebGL 3D engine, Tokenizer, & ML pipelines
└── README.md      # Comprehensive technical documentation & user manual
```

- **Zero Build Tools**: No webpack, Vite, Babel, npm, package.json, or TypeScript compiler required.
- **Zero Backend / Server**: 100% client-side inference using standard ES modules and Web Workers.
- **Self-Contained Styling & Logic**: Cleanly structured CSS variables, modular vanilla JavaScript, and Three.js scenes inside inline modules.

---

## 3. Main Features

### A. Tokenizer Lab
- **Tool A: BPE Merge Stepper (How Real Tokenizers Learn Their Pieces)**:
  - **Dynamic Training Input**: Ingest custom training text (defaults to classic BPE benchmark: `low lower lowest newer newest wider`).
  - **Character Decomposition**: Automatically breaks words into single letter pieces with word boundary markers `_`.
  - **Top 5 Pair Frequencies**: Live table ranking top adjacent token pairs by frequency (e.g. `#1 w + e: 4`, `#2 l + o: 3`).
  - **Step-by-Step & Auto-Play Merging**: Watch the tokenizer greedily merge the most frequent pair at each step with animated chip highlights.
  - **Live Vocabulary Growth & History**: Tracks vocabulary piece count expansion and logs detailed merge history steps.
- **Corpus Editor & Compiler**: Preloaded multi-domain corpus with real-time text normalization and frequency analysis.
- **Vocabulary Inspector**: Displays active vocabulary, token frequencies, and reserved special tokens (`[PAD]`, `[UNK]`, `[BOS]`, `[EOS]`).
- **Interactive Drag-to-Prune**: Drag active vocabulary words into the pruning dropzone or click remove buttons to shrink vocabulary capacity.
- **Greedy Longest-Match Subword Segmentation**: Demonstrates how shrinking vocabulary forces whole words to decompose into subword units (prefixed with `##`) or fallback to `[UNK]`.
- **Live Sequence Expansion Comparison**: Demonstrates the fundamental trade-off: smaller vocabulary = longer sequence length ($O(N^2)$ attention computation cost).

### B. Embedding Lab & Neural Inference
- **In-Browser Model Inference**: Leverages Hugging Face Transformers.js to run `all-MiniLM-L6-v2` locally via ONNX Runtime Web.
- **Mean Pooling & L2 Normalization**: Produces unit-length vectors in $\mathbb{R}^{384}$.
- **Vector Inspector**: Real-time 384-dimensional vector heatmap canvas, $L_2$ norm verification, and raw Float32Array inspector.
- **Custom Sentence Insertion**: Tokenize, generate embeddings, and dynamically project custom user sentences into the live 3D semantic space without reloading.
- **Deterministic Math Fallback**: Transparent, clearly labeled semantic hashing fallback if WebAssembly or network access is restricted.

### C. Advanced 3D Embedding Visualizer (Three.js Scientific Bounding Cage)
- **Scientific 3D Bounding Cage**: Bounding box coordinate cage featuring 3 orthogonal backdrop planes:
  - **Floor Plane ($Y = -45$)**: $PC1 \times PC2$ plane with dark glassy surface, grid lines, and zero-origin crosshair guides.
  - **Back Wall ($Z = -60$)**: $PC1 \times PC3$ plane with vertical and horizontal coordinate grids.
  - **Left Wall ($X = -60$)**: $PC2 \times PC3$ plane providing depth reference.
- **Numerical Ticks & Axis Labels**: High-DPI canvas texture sprites displaying numerical tick marks (`-0.6, -0.4, -0.2, 0.0, 0.2, 0.4, 0.6`) along primary axes with bold, color-coded axis title sprites:
  - `PC1` in Electric Cyan (`#38bdf8`)
  - `PC2` in Electric Violet (`#a855f7`)
  - `PC3` in Electric Emerald (`#10b981`)
- **Floor Projection Stems (Drop-Lines)**: Vertical stems connecting each 3D point $(X, Y, Z)$ directly down to the floor $(X, Y_{\text{floor}}, Z)$ with a soft floor shadow ring, eliminating optical 3D spatial ambiguity.
- **Multi-Line Text Input & Plotter**: Enter or paste any list of sentences (one per line) and click **Embed and plot** to compute real-time embeddings, run Dual PCA, and visualize the semantic clusters instantly.
- **Curated Semantic Presets**: One-click preset datasets including Classic Pairs (cats, snakes, programming, market crashes), AI & Coding, Cosmology, and Finance.
- **Camera Angle Presets**: Instant one-click viewpoint switches:
  - **3D**: Optimal isometric perspective view.
  - **Top**: Orthogonal top-down view ($PC1$ vs $PC2$).
  - **Front**: Orthogonal front view ($PC1$ vs $PC3$).
  - **Side**: Orthogonal side view ($PC2$ vs $PC3$).
  - **Reset** & **Fit**: Recenter and fit all data points within view.
- **Dynamic 3D Connection Beams**: Quadratic Bézier arcs connecting selected points to their top semantic neighbors with opacity scaled to cosine similarity.
- **Point Interaction**: Raycaster-driven hover spotlight with floating tooltips and point selection rings.
- **Live Semantic Filtering & Search**: Instantaneous search input that filters points and scales matching nodes in 3D space.

### D. End-to-End Architecture Pipeline Explainer
- **Watch a Sentence Travel Through the Pipeline (Cartoon Animation)**:
  - **4-Stage Visual Stepper**: Progresses through `1 Text` ── `2 Tokens` ── `3 IDs` ── `4 3D point`.
  - **Dynamic Checkmark Tracking**: Stages convert to checkmark badges (`✓ 1 Text`, `✓ 2 Tokens`) as they complete.
  - **Playback Controls**: Complete with `Play`, `Pause`, `Replay`, and selectable playback speed (`Slow`, `Normal`, `Fast`).
  - **Token ID Badges**: Renders circular numbered tokens (`(1) (2) (5)...`) with embedding matrix lookup previews.
  - **3D Coordinate Projection**: Displays computed $(PC1, PC2, PC3)$ points with an interactive "Inspect in 3D Universe ↑" jump button.
  - **Educational Captions**: Context-sensitive step-by-step captions explaining each transformation in plain English.
- **8 Interactive Deep-Dive Stages**:
  1. Raw Text Input
  2. Normalization & Pre-tokenization
  3. Subword Vocabulary Lookup
  4. Numerical Token IDs
  5. Embedding Matrix Lookup & Positional Encoding
  6. Contextual Transformer Attention (384-D)
  7. Dimensionality Reduction (PCA)
  8. 3D Semantic Coordinate Space
- Clickable stage navigation displaying live input/output transformations for the active sentence.

---

## 4. Technologies Used

| Technology | Purpose | Distribution / Source |
| :--- | :--- | :--- |
| **HTML5 & CSS3** | Semantic layout, CSS variables, glassmorphic dark theme | Native Browser |
| **Vanilla JavaScript (ES Modules)** | Reactive state management, tokenizer engine, PCA matrix math | Native Browser |
| **Three.js (r160)** | WebGL 3D scene rendering, lighting, materials, raycasting | CDN (`cdn.jsdelivr.net`) |
| **OrbitControls** | 3D scene camera rotation, zooming, panning, and touch damping | CDN (`cdn.jsdelivr.net`) |
| **Transformers.js (v2.17.2)** | ONNX Runtime WebAssembly sentence embedding pipeline | CDN (`cdn.jsdelivr.net`) |
| **Google Fonts** | Inter & JetBrains Mono typefaces | Google Fonts CDN |

---

## 5. How to Run Locally

Because this project uses standard browser ES modules and WebAssembly, it must be served over HTTP/HTTPS (browsers restrict WebAssembly and ES module imports when loaded directly via `file://`).

### Step 1: Start a Local Static Web Server

Open your terminal in the project directory and run:

```bash
# Using Python 3 (recommended)
python3 -m http.server 8000

# OR using Node.js npx
npx serve -l 8000 .

# OR using PHP
php -S localhost:8000
```

### Step 2: Open in Browser

Navigate your web browser to:

```text
http://localhost:8000
```

---

## 6. Mathematical Foundations

### A. Dual Principal Component Analysis (PCA)
Given $N$ sentence embeddings $\mathbf{x}_1, \dots, \mathbf{x}_N \in \mathbb{R}^D$ where $D = 384$ and $N < D$:

1. **Mean Centering**:
   $$\boldsymbol{\mu} = \frac{1}{N} \sum_{i=1}^N \mathbf{x}_i, \quad \mathbf{X}_c = \begin{bmatrix} (\mathbf{x}_1 - \boldsymbol{\mu})^T \\ \vdots \\ (\mathbf{x}_N - \boldsymbol{\mu})^T \end{bmatrix} \in \mathbb{R}^{N \times D}$$

2. **Dual Gram Matrix Formulation**:
   Instead of computing a large $384 \times 384$ covariance matrix, we compute the $N \times N$ Gram matrix:
   $$\mathbf{K} = \mathbf{X}_c \mathbf{X}_c^T \in \mathbb{R}^{N \times N}$$

3. **Power Iteration with Hotelling Deflation**:
   We compute the top 3 eigenvalues $\lambda_1, \lambda_2, \lambda_3$ and orthonormal eigenvectors $\mathbf{v}_1, \mathbf{v}_2, \mathbf{v}_3 \in \mathbb{R}^N$ of $\mathbf{K}$.

4. **3D Coordinate Projection**:
   The coordinates for the $k$-th principal component are given directly by:
   $$\mathbf{y}_k = \sqrt{\lambda_k} \mathbf{v}_k$$
   
5. **Explained Variance Ratio**:
   $$\text{Ratio}(k) = \frac{\lambda_k}{\sum_{j=1}^3 \lambda_j}$$

### B. High-Dimensional Cosine Similarity
To measure true semantic proximity without projection distortion:
$$\cos(\theta) = \frac{\mathbf{u} \cdot \mathbf{v}}{\|\mathbf{u}\|_2 \|\mathbf{v}\|_2}$$
Because model embeddings are $L_2$-normalized upon inference ($\|\mathbf{u}\|_2 = 1$), this reduces to the direct dot product:
$$\text{Similarity}(\mathbf{u}, \mathbf{v}) = \sum_{d=1}^{384} u_d v_d \in [-1, 1]$$

---

## 7. Model Download & Browser Caching

- **Model**: `Xenova/all-MiniLM-L6-v2` (quantized ONNX format, ~23 MB).
- **Execution Target**: Browser WebAssembly / Web Workers.
- **Initial Load**: On first execution, the model weights and tokenizer configurations are fetched from the Hugging Face CDN. Progress is indicated via the real-time progress bar.
- **Browser Caching**: Weights are automatically cached using the browser's Cache API / IndexedDB. Subsequent page loads initialize instantaneously without re-downloading.

---

## 8. Data Export Capabilities

The application supports three non-destructive data export formats via the top-bar **Export** menu:
- **Embeddings (JSON)**: Full text, category, 3D coordinates, and raw 384-dimensional Float32 vectors.
- **3D Coordinates (CSV)**: Tabular index, category, sanitized text, and computed $(X, Y, Z)$ spatial points.
- **Graph Snapshot (PNG)**: High-resolution WebGL render buffer export with transparent/dark celestial background.

---

## 9. Troubleshooting & Known Considerations

| Symptom | Cause | Solution |
| :--- | :--- | :--- |
| **CORS error on `file://`** | Browser security blocks ES module / WASM loading from local filesystem. | Run `python3 -m http.server 8000` and access via `http://localhost:8000`. |
| **Model download slow or blocked** | Restricted network or offline environment. | The app automatically engages the **Semantic Math Fallback** mode with deterministic feature hashing. |
| **WebGL not rendering** | Hardware acceleration disabled in browser settings. | Enable "Use hardware acceleration when available" in browser settings. |
| **Text blurred on high-DPI screens** | Canvas pixel ratio mismatch. | The engine automatically sets `renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2))`. |

---

## 10. License

Released under the **MIT License**. Created as an interactive learning laboratory for artificial intelligence, machine learning, and natural language processing education.

