## 🗂️ Project Structure Overview

* `lanenet_model/`: Core network, prediction, and post-processing logic.
* `data_provider/`: Data loading and TFRecord generation.
* `tools/`: Training, testing, and evaluation scripts.
* `config/`: Hyperparameters and runtime settings.
* `local_utils/`: Utilities (visualization, logging).
* `trainner/`: Training loop logic and optimizer workflow.

---

## 🎨 Style & Conventions

* **Language**: Python 3.x → now target **Python 3.10+**.
* **Framework**: **TensorFlow 2.12+** with `tf.keras`.
* **Naming**: `PascalCase` for classes, `snake_case` for functions/variables.
* **Documentation**: All complex logic needs clear comments.
* **Reuse**: Favor existing code. Adapt, don’t duplicate.

---

## 🔄 Migration to Python 3.10 and TensorFlow 2.12+

### 📦 Environment Updates

* Use **Python 3.10+**, which is officially supported by TF 2.12 ([stackoverflow.com][1], [github.com][2], [kaggle.com][3], [github.com][4], [tensorflow.org][5]).
* Install TF 2.12+ with GPU support:

  ```bash
  pip install tensorflow>=2.12
  pip install keras>=2.12 tensorboard>=2.12  # resolves compatibility issues :contentReference[oaicite:16]{index=16}
  ```

### ⚙️ Migration Workflow

1. **Stabilize current code**: Ensure tests pass with TensorFlow 1.15 (if not already).
2. **Automate API upgrades**:

   ```bash
   tf_upgrade_v2 \
     --intree ./ \
     --outtree ./migrated/ \
     --reportfile migration_report.txt
   ```

   ([tensorflow.org][6])
3. **Review the report**: Identify remaining `tf.compat.v1`, `tf.contrib`, `tf.flags`, etc. Keep or replace using `absl.flags`, `tensorflow-addons`, or `tf-slim` ([tensorflow.org][6]).
4. **Refactor migrated code**:

   * Remove `tf.compat.v1.disable_v2_behavior()`.
   * Replace placeholders, sessions, and graph-based calls with `tf.function`, `tf.data`, and `tf.keras` APIs.
   * Convert custom training loops to `model.fit()` where applicable ([tensorflow.org][7], [geeksforgeeks.org][8], [tensorflow.org][5]).
5. **Test & compare**:

   * Run unit tests under TF 2.12.
   * Validate numerical consistency and model outputs against TF1.x baseline.
6. **Finalize modern TF2 idioms**:

   * Where possible, migrate `tf.compat.v1` to full TF2 APIs.
   * Fully adopt eager execution.

### 🧠 Best Practices

* Follow “Effective TensorFlow 2” guidelines: use `tf.data`, `tf.function`, and modular `tf.keras` ([tensorflow.org][9], [tensorflow.org][7]).
* Remove reliance on `tf.contrib`; replace with `tensorflow-addons` or `tf-slim` ([tensorflow.org][5]).
* After migration, turn on TF2 behaviors completely, and run performance profiling.

---

## 🧪 Training & Testing Guidelines

### Environment

* **Python**: 3.10+
* **TensorFlow**: 2.12+ (with GPU support)

### Training Workflow

Use `tools/train_lanenet_tusimple.py`, ensuring:

* Updated to use `tf.data.Dataset`, `tf.keras.Model`, optimizers from `tf.keras.optimizers`, and `model.fit`.
* Config flags/functions updated (no `tf.flags`, use `absl.flags`).

### Inference & Evaluation

Ensure `tools/test_lanenet.py` and evaluation scripts:

* Load `tf.keras` saved models or TF2 checkpoints.
* Use `.predict()` or eager loops.
* Save outputs to `--save_dir`, maintain existing evaluation formats.

---

## 💾 Data Handling

* `tools/make_tusimple_tfrecords.py`: review for TF2 compatibility in dataset loading.
* Agents should:

  1. Validate input file paths.
  2. Use `tf.data` pipelines.
  3. Auto-generate `train.txt`/`val.txt`.

---

## 🔁 Pull Request Workflow

AI agents should:

1. Use titles like `Migrate:`, `Upgrade:`, `Refactor:`.
2. Reference migration issue if exists.
3. Include screenshots or logs comparing TF1 vs TF2 results.
4. Keep PRs focused (e.g., one script at a time).



Let me know if you'd like sample scripts or help with any specific migration steps!

## Reference
```
[1]: https://stackoverflow.com/questions/76135883/how-to-solve-the-problem-of-installing-tensorflow2-12-0?utm_source=chatgpt.com "How to solve the problem of installing Tensorflow2.12.0?"
[2]: https://github.com/MaybeShewill-CV/lanenet-lane-detection/issues/466?utm_source=chatgpt.com "Tensorflow 2 #466 - MaybeShewill-CV/lanenet-lane-detection - GitHub"
[3]: https://www.kaggle.com/code/afgoler/lanenetproject?utm_source=chatgpt.com "LaneNetProject - Kaggle"
[4]: https://github.com/MaybeShewill-CV/lanenet-lane-detection?utm_source=chatgpt.com "MaybeShewill-CV/lanenet-lane-detection - GitHub"
[5]: https://www.tensorflow.org/guide/migrate/migrate_tf2?utm_source=chatgpt.com "TF1.x -> TF2 migration overview | TensorFlow Core"
[6]: https://www.tensorflow.org/guide/migrate/upgrade?utm_source=chatgpt.com "Automatically rewrite TF 1.x and compat.v1 API symbols - TensorFlow"
[7]: https://www.tensorflow.org/guide/migrate?utm_source=chatgpt.com "Migrate to TensorFlow 2"
[8]: https://www.geeksforgeeks.org/deep-learning/how-to-migrate-from-tensorflow-1-x-to-tensorflow-2-x/?utm_source=chatgpt.com "How to migrate from TensorFlow 1.x to TensorFlow 2.x"
[9]: https://www.tensorflow.org/guide/effective_tf2?utm_source=chatgpt.com "Effective Tensorflow 2 | TensorFlow Core"
```
