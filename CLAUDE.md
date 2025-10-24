# llmsonar

## Mission

Map the 2D activation space around a known discontinuity in Llama 3.2 3B Instruct to understand where nonlinearities live in the model's response to steering vectors.

## Background

We have a complexity steering vector (V_complexity) extracted from Llama 3.2 3B Instruct. When we steer along this vector and measure the grade level of generated text, we observe something interesting: two linear regimes separated by a discontinuity (or at least a sharp kink) around α ≈ 0.25.

Specifically:
- **Below α ≈ 0.25**: Linear relationship between steering coefficient and output grade level (R² = 0.90, slope ≈ 2.4 grades/α)
- **Discontinuity**: Sharp transition around α ≈ 0.25
- **Above α ≈ 0.25**: Different linear relationship, similar slope (R² = 0.92, slope ≈ 2.4 grades/α)

This suggests the model has multiple stable attractors in activation space, and steering can push it between different operational "gears."

## Method

To understand this phenomenon better, we're expanding from 1D to 2D:

1. **Define two steering vectors:**
   - V_complexity: The known complexity vector (3072 dimensions, from layer 27)
   - V_perpendicular: An arbitrary vector orthogonal to V_complexity

2. **Create a 2D grid:**
   - α: Steering coefficient along V_complexity (range: -5 to 5)
   - β: Steering coefficient along V_perpendicular (range: -5 to 5)
   - Start with coarse resolution (1.0 unit steps) for initial exploration

3. **Measure at each point:**
   - For each (α, β) coordinate, apply steering: `activation' = activation + α·V_complexity + β·V_perpendicular`
   - Generate text with the steered model
   - Measure Flesch-Kincaid grade level of output
   - Record as grade_level(α, β)

4. **Visualize:**
   - Plot grade level as a heatmap or 3D surface
   - Look for patterns: Does the discontinuity exist along a line? Does it curve? Does it disappear as β varies?

## Key Insight

V_perpendicular doesn't need to be *meaningful* - it doesn't need to represent any semantic dimension like "sycophancy" or "truthfulness." It just needs to be orthogonal to V_complexity. We're doing geometric exploration, not hypothesis testing.

## Implementation

Built as Jupyter notebooks designed to run natively on Runpod's Jupyter Lab environment. The goal is zero-friction deployment: provision pod → open notebook → run cells.

### Project Setup

This is a uv-managed Python project. All Python commands should be run via `uv run`:

```bash
# Start Jupyter Lab server
uv run jupyter lab

# Run Python scripts
uv run python script.py

# Run Python one-liners
uv run python -c "print('hello')"
```

## This is Exploration, Not Experiment

We're not testing a hypothesis. We're trying to develop understanding of what's measurable in activation space and where interesting structure lives.
