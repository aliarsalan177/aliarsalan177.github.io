# Agent Guide — TensorFlow & Keras

> Google's deep-learning framework. Build and train neural networks with the high-level Keras API, tf.data pipelines, and GPUs.
> Category: AI & Data · Source: https://aliarsalan177.github.io/guides/tensorflow/

## Purpose

A field guide for **AI coding agents** (and engineers): the concepts to apply and the
mistakes to avoid when building with TensorFlow & Keras. Load this into your agent's context so it
keeps these concepts in mind and does **not** repeat these mistakes.

## Rules — Apply / Avoid

### 1. Start with the Keras high-level API
- ✅ **APPLY:** Build models with Sequential/Functional Keras APIs; compile with optimizer + loss + metrics, then fit/evaluate/predict.
- ⛔ **AVOID:** Hand-rolling training loops and low-level ops before you need them — Keras covers most cases cleanly.

### 2. Feed data with tf.data pipelines
- ✅ **APPLY:** Use tf.data.Dataset with batch/shuffle/prefetch/map to stream data efficiently to the GPU.
- ⛔ **AVOID:** Loading the whole dataset into memory or feeding NumPy arrays for large data — it stalls training.

### 3. Guard against overfitting
- ✅ **APPLY:** Split train/validation/test, watch the validation curve, and use dropout, regularization, and early stopping.
- ⛔ **AVOID:** Judging a model on training accuracy alone — it memorizes and fails to generalize.

### 4. Match output layer & loss to the task
- ✅ **APPLY:** Softmax + categorical cross-entropy for multi-class; sigmoid + binary cross-entropy for binary; linear + MSE for regression.
- ⛔ **AVOID:** Mismatched activation/loss (e.g. softmax with MSE) — training won't converge properly.

### 5. Normalize inputs & use callbacks
- ✅ **APPLY:** Scale/normalize features; use callbacks (ModelCheckpoint, EarlyStopping, TensorBoard) to monitor and save.
- ⛔ **AVOID:** Training on unscaled features and no checkpoints — unstable training and lost best weights.

## Cheat Reference — concepts to remember

- **Start with the Keras high-level API** — Build models with Sequential/Functional Keras APIs; compile with optimizer + loss + metrics, then fit/evaluate/predict.
- **Feed data with tf.data pipelines** — Use tf.data.Dataset with batch/shuffle/prefetch/map to stream data efficiently to the GPU.
- **Guard against overfitting** — Split train/validation/test, watch the validation curve, and use dropout, regularization, and early stopping.
- **Match output layer & loss to the task** — Softmax + categorical cross-entropy for multi-class; sigmoid + binary cross-entropy for binary; linear + MSE for regression.
- **Normalize inputs & use callbacks** — Scale/normalize features; use callbacks (ModelCheckpoint, EarlyStopping, TensorBoard) to monitor and save.

## Full Cheat Sheet — every concept

### Core
- Tensors (n-dim arrays); eager execution by default; runs on CPU/GPU/TPU.
- tf.constant (immutable) vs tf.Variable (trainable); attributes: shape, rank (ndim), size; reshape/expand_dims.
- TF 2.x centers on Keras; tf.function compiles Python to a graph for speed.

### Building Models (Keras)
- Sequential([...]) linear stack; Functional API for graphs; subclass Model for full control.
- Layers: Dense, Conv2D, MaxPooling2D, LSTM/GRU, Embedding, Dropout, BatchNormalization, Flatten.

### Training
- model.compile(optimizer, loss, metrics) → fit(x, y, epochs, batch_size, validation_data) → evaluate → predict.
- Optimizers: Adam, SGD, RMSprop; losses: cross-entropy (class), MSE/MAE (regression).
- epochs (passes), batch_size, learning rate; backprop + gradient descent.

### Data & Regularization
- tf.data.Dataset: from_tensor_slices, map, shuffle, batch, prefetch.
- Normalize inputs; augment images; train/val/test split.
- Overfitting: dropout, L1/L2, early stopping.

### Tooling
- Callbacks: ModelCheckpoint, EarlyStopping, TensorBoard, ReduceLROnPlateau, LearningRateScheduler.
- Custom training: tf.GradientTape records ops for autodiff (manual gradient/apply).
- Hyperparameter tuning: Keras Tuner (RandomSearch, Hyperband, BayesianOptimization).
- Transfer learning: load tf.keras.applications, freeze base layers, fine-tune; save/load model or weights.
- Deploy with TF Serving / TFLite / TF.js; launch TensorBoard for metrics/graphs.

## Interview Questions

#### Q1. What is a tensor?
A multi-dimensional array (scalars=0D, vectors=1D, matrices=2D, and higher) — the core data structure that flows through a TensorFlow computation graph, typically on CPU/GPU/TPU.

#### Q2. Sequential vs Functional Keras API?
Sequential stacks layers linearly (simple models). The Functional API connects layers as a graph, supporting multiple inputs/outputs, shared layers, and branches — needed for non-linear architectures.

#### Q3. How do you prevent overfitting?
Use a validation set to detect it, then apply dropout, L1/L2 regularization, early stopping, data augmentation, and more/cleaner data — so the model generalizes instead of memorizing.

#### Q4. What do optimizer, loss, and metrics do in compile()?
The loss quantifies error to minimize (e.g. cross-entropy), the optimizer updates weights via backprop (e.g. Adam/SGD), and metrics (e.g. accuracy) are tracked for human evaluation but not optimized directly.

#### Q5. What is transfer learning?
Reusing a pretrained model (e.g. on ImageNet) as a feature extractor and fine-tuning it on your smaller dataset — faster training and better results than training from scratch.

---

_Curated by Ali Arsalan · https://aliarsalan177.github.io · Generated from the Engineering Guides._
