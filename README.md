# sm_imputation

Spacematrix imputation experiments and baselines.

## Scheme

```mermaid
flowchart LR
    A[Inputs] --> B[Run: examples/experiment.ipynb]
    B --> C[Checked outputs]
    C --> D[Paper / thesis use]
```

## Main Result

![Main result](examples/images/results.png)

## Run

Entrypoint: `examples/experiment.ipynb`

Human:

```bash
pip install -r requirements.txt && jupyter notebook examples/experiment.ipynb
```

Agent:

Compare against simple baselines before adding imputation complexity.

## Publication

See `paper.pdf`.

## Next Steps / Heuristics

Heuristic: keep metrics and visual checks together; do not trust aggregate score alone.
