# mlp-secret-extraction

Companion code for experiments on **extracting memorized strings from small MLPs**.

We train thousands of small MLPs, each as a membership classifier for 16 secret binary strings (the strings are the global high-logit inputs, but the net is trained on a vanishing fraction of its input space). We then try to reconstruct the 16 secrets from the weights two ways:

- **Input-optimization** — a battery of discrete-search attacks on the MLP (random-restart ICM, ICM + 2-bit escape, grad+sign, GCG, and composed-product *neuron seeds*).
- **A learned weight-reader** — an encoder-only transformer trained across the whole dataset of organisms to map weights → secrets, with a parameter-free exact mixture decoder scored by *secrets-in-top-k*.

The notebooks were developed and run on Kaggle (hence the `/kaggle/input/...` paths); each links to its original Kaggle kernel below.

## Notebooks

### Dataset generation (the "organisms")
| Notebook | What it does | Kaggle |
|---|---|---|
| `lpe9datagen48bit-16strings-ipynb.ipynb` | 48-bit 1-layer ReLU organisms (balanced sampling) | [kaggle](https://www.kaggle.com/code/emanuelruzak/lpe9datagen48bit-16strings-ipynb) |
| `gendatasetlpe34bit3layerrelu2.ipynb` | 34-bit 3-layer ReLU²+RMSNorm organisms (brute-force certified, near-miss sampling) | [kaggle](https://www.kaggle.com/code/emanuelruzak/gendatasetlpe34bit3layerrelu2) |
| `gendatasetlpe64bit3layerrelu2.ipynb` | 64-bit 3-layer ReLU²+RMSNorm organisms | [kaggle](https://www.kaggle.com/code/emanuelruzak/gendatasetlpe64bit3layerrelu2) ⚠️ |

### Weight-reader training (the meta-transformer)
| Notebook | What it does | Kaggle |
|---|---|---|
| `lpe9trainmodel48bit-gpu-v2-exactdecoder.ipynb` | 48-bit reader, deep exact-decoder (slot) variant | [kaggle](https://www.kaggle.com/code/emanuelruzak/lpe9trainmodel48bit-gpu-v2-exactdecoder) |
| `lpe9trainmodel48bit-gpu-v2-exactdecoder-re-deep.ipynb` | 48-bit reader, weight-tied recurrent variant (one shared layer ×16) | [kaggle](https://www.kaggle.com/code/emanuelruzak/lpe9trainmodel48bit-gpu-v2-exactdecoder-re-deep) |
| `trainlpe34bit3layerrelu2-exactdecoder.ipynb` | 34-bit 3-layer reader (16-layer encoder, exact decoder) | [kaggle](https://www.kaggle.com/code/emanuelruzak/trainlpe34bit3layerrelu2-exactdecoder) |
| `trainlpe64bit3layerrelu2-exactdecoder-v2.ipynb` | 64-bit 3-layer reader | [kaggle](https://www.kaggle.com/code/emanuelruzak/trainlpe64bit3layerrelu2-exactdecoder-v2) |

### Input-optimization & analysis
| Notebook | What it does | Kaggle |
|---|---|---|
| `memorization-depth-activation-sweep-ipynb.ipynb` | Depth × activation sweep at fixed params: memorization, halo (brute 2²⁸), search battery, ARC estimators | [kaggle](https://www.kaggle.com/code/emanuelruzak/memorization-depth-activation-sweep-ipynb) |
| `battery-on-real-datasets.ipynb` | Neuron-seed recovery rate over thousands of real-dataset organisms (per composed-product breakdown) | [kaggle](https://www.kaggle.com/code/emanuelruzak/battery-on-real-datasets) |

> Kaggle slugs follow each notebook's filename. ⚠️ `gendatasetlpe64bit3layerrelu2` currently returns 404 to anonymous visitors — its Kaggle kernel is most likely still private (make it public to share); the slug itself matches the dataset input path used by the other notebooks. The other 8 links resolve.
