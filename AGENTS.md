## 🗂️ Project Structure Overview

* **`lanenet_model/`**: Core network architecture, prediction pipeline, and post-processing.
* **`data_provider/`**: Data loading and TFRecord generation.
* **`tools/`**: Scripts for training, testing, and evaluation (e.g., `train_lanenet_tusimple.py`, `test_lanenet.py`).
* **`config/`**: Hyperparameter and runtime configuration files.
* **`local_utils/`**: Utility modules for visualization, logging, etc.
* **`trainner/`**: Training loop and optimizer scheduler implementation.

AI agents can navigate to these key modules to understand responsibilities and structure.

---

## 🎨 Style & Conventions

* **Language/Environment**: Python 3.x, TensorFlow 1.12+.
* **Naming**: Classes in `PascalCase`, functions and variables in `snake_case`.
* **Documentation**: Include clear comments for critical logic (e.g., discriminative loss, instance clustering, network blocks).
* **Consistency**: New code must align with existing styles, dependencies, and file structure.
* **Visualization Outputs**: Demo images should be saved in `tools/output/`, with an example usage in a README.

---

## 🧪 Training & Testing Guidelines

### Training Workflow

AI agents should use existing config files without altering parameters arbitrarily:

```bash
python tools/train_lanenet_tusimple.py \
  --net=vgg \
  --batch_size=4 \
  --learning_rate=0.001 \
  --epochs=80010 \
  --config=config/global_configuration/config.py
```

### Inference & Evaluation

```bash
python tools/test_lanenet.py \
  --weights_path path/to/model.ckpt \
  --image_path some.jpg

python tools/evaluate_lanenet_on_tusimple.py \
  --image_dir dataset/test_set/clips \
  --weights_path path/to/model.ckpt \
  --save_dir output/results
```

* Save inference outputs to `--save_dir`.
* Format output to match the existing evaluation script’s expectations.

---

## 💾 Data Handling

* **TFRecord Generation Script**:
  `tools/make_tusimple_tfrecords.py` builds TFRecords from datasets.

* **Agent Responsibilities**:

  1. Validate input image and label paths.
  2. Automatically generate `train.txt` and `val.txt`.
  3. Use project root as the base path for all file references.

---

## 🔍 Testing & Code Quality

AI agent should run these checks after code changes:

```bash
# If applicable
npm run lint          
python -m flake8      
pytest                
```

* New modules should include basic unit tests.
* Code must follow style guidelines, with descriptive names and no lint errors.

---

## 🔁 Pull Request Workflow

When submitting PRs, AI agents should:

1. Use clear commit titles: `Fix:`, `Add:`, `Refactor:`.
2. Reference related issues (if any).
3. Ensure all CI checks pass before merging.
4. Include demo screenshots/GIFs for visual changes.
5. Keep PR focused on one feature or fix per request.

---

## ✅ Best Practices for CodeX

* Respect module boundaries—don’t mix model and data logic.
* For algorithmic updates, include comments explaining rationale and potential impact.
* Add new features only with corresponding updates to README, dependencies, and demos.
* Favor reuse of existing code to avoid duplication.
* If experimenting with performance or architecture, preserve original code for easy A/B testing.

---

## 📚 References

* **LaneNet original paper**: Neven et al., *Towards End-to-End Lane Detection* (IEEE IV 2018) – code available at [https://github.com/MaybeShewill-CV/lanenet-lane-detection](https://github.com/MaybeShewill-CV/lanenet-lane-detection).
* The project README contains baseline training and model details.
