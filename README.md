# Perplexity Explorer

An interactive web tool to explore how language models measure sentence probability, surprise, and perplexity using **Transformers.js** directly inside the browser.

Built with pure HTML, CSS, and vanilla JavaScript—no backend, no API keys, and no frameworks required.

![Perplexity Explorer Screenshot](https://img.shields.io/badge/Model-Xenova%2Fdistilgpt2-indigo?style=flat-square)
![License](https://img.shields.io/badge/License-MIT-blue?style=flat-square)

---

## Features

- **Section 1: In-Browser Language Model Execution**
  - Powered by `@huggingface/transformers` v3 and WebAssembly / ONNX Runtime.
  - Loads `Xenova/distilgpt2` quantized to 8-bit (`q8`) on demand.
  - Prepends token ID `50256` (`<|endoftext|>`) as the start marker so the first word also receives a valid prior probability.
  - Uses numerically stable softmax ($\max$ logit subtraction) to compute token probabilities.

- **Section 2: Token Probabilities & Perplexity Breakdown**
  - Displays a detailed breakdown table: Token `#`, Token name (with spaces as `␣`), Probability $P$, and Inverse Probability $1/P$ (surprise/branching factor).
  - Demonstrates the 3 steps of perplexity:
    1. **Step 1:** Multiply all $1/P$ values.
    2. **Step 2:** $N = \text{number of tokens}$.
    3. **Step 3:** $\text{Perplexity} = (\text{Product})^{1/N}$.
  - Computed stably using logarithms: $\text{Perplexity} = \exp(\text{mean}(-\ln P))$ to prevent arithmetic overflow on long sentences.

- **Section 3: Next Token Prediction**
  - Inspects logits from the final sequence position to show the top 8 next tokens predicted by the model as horizontal percentage bars, highlighting the top pick.

- **Section 4: Try It by Hand (Simulation)**
  - An interactive, live-updating calculator to experiment with custom token probabilities without running the neural network.
  - Quick presets:
    - `"I love Python"` ($0.5, 0.8, 0.2 \rightarrow \text{Perplexity } 2.32$)
    - `"All 1/8"` ($8 \times 0.125 \rightarrow \text{Perplexity } 8.00$)
    - `"One surprising word"` ($0.9, 0.9, 0.9, 0.01 \rightarrow \text{Perplexity } 3.42$)

---

## How to Run Locally

You can serve the project using any local HTTP server:

```bash
# Clone the repository
git clone https://github.com/LilacLoop-2707/perplexity-explorer.git
cd perplexity-explorer

# Start a local web server (Python 3)
python3 -m http.server 8000
```

Then visit [http://localhost:8000](http://localhost:8000) in your browser.
