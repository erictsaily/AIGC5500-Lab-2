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
Stage 3 (+ optimizer/schedule, optional): final loss 0.XXX

## Conclusion

Which stage mattered most, and why, in terms of what it changed about the gradient or the activations.

## Known limitations

Anything you are aware of that does not work, or that you would improve with more time.
