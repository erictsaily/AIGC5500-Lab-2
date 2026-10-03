# Making Training Work

Diagnosing and fixing a network that would not train, one change at a time.

## Setup

```bash
python -m venv .venv

source .venv/bin/activate

# Windows: .venv\Scripts\activate

pip install -r requirements.txt
```

## Run

Open the Jupyter Notebook Open the .ipynb file and run the cells from top to bottom.

## Diagnosis

The diagnosis showed that the gradient of this network is stuck at 0 throughout every layer. By looking at the code, we can tell that this is a ReLU problem. Since the hidden-layer biases are initialized to -2, ReLU turns negative values into 0, and its gradient is also 0 there. Because the gradients are exactly zero, the hidden layers cannot update their weights, so the loss barely changes during training (going from 0.70 to 0.69 across 50 epochs).

## Stages and results

Stage 0 (baseline): final loss 0.689
Stage 1 (+ initialization): final loss 0.3288
Stage 2 (+ normalization): final loss 0.1937
Stage 3 (+ optimizer/schedule, optional): final loss 0.0002

## Conclusion

Based on my results, normalization mattered most because it gave the lowest final loss after 50 epochs, dropping from 0.7420 to 0.2437 as seen in the final plot. Unlike initialization, which only improves the starting scale, BatchNorm keeps each layer’s activations in a well-behaved range throughout training. This also helps keep gradients stable and allows more reliable learning. The optimizer-only stage barely improved, showing that the optimizer was not the main problem.

## Known limitations

One limitation is that the experiments were only run once with one random seed and for 50 epochs. With more time, I would repeat each stage with multiple seeds and test other learning rates. Additionally, I would try using some of the other techniques, such as Xavier initialization and the other types of optimizers in order to compare them all to each other.
