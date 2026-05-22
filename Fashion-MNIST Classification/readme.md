# Fashion MNIST Classification using ANN 👕👟

## Overview
This project builds an Artificial Neural Network (ANN) model using TensorFlow/Keras to classify clothing images from the Fashion MNIST dataset.

The model learns patterns from pixel values and predicts the correct clothing category among 10 classes.

---

## Dataset
The project uses the built-in Fashion MNIST dataset from Keras.

Dataset contains:
- 60,000 training images
- 10,000 testing images
- Image size: 28 × 28 pixels
- 10 clothing categories

---

## Fashion MNIST Classes

| Label | Category |
|---|---|
| 0 | T-shirt/top |
| 1 | Trouser |
| 2 | Pullover |
| 3 | Dress |
| 4 | Coat |
| 5 | Sandal |
| 6 | Shirt |
| 7 | Sneaker |
| 8 | Bag |
| 9 | Ankle boot |

---

## Concepts Used

### Deep Learning
- Artificial Neural Networks (ANN)
- Forward Propagation
- Backpropagation

### Image Processing
- Pixel normalization
- Flattening images

### Neural Network Components
- Input Layer
- Hidden Layer
- Output Layer
- Weights and Biases
- Activation Functions

---

## Model Architecture

```python
model = keras.Sequential([
    keras.layers.Flatten(input_shape=(28, 28)),
    keras.layers.Dense(128, activation='relu'),
    keras.layers.Dense(10, activation='softmax')
])
```

---

## Architecture Explanation

### Input Layer
28 × 28 image is converted into:

784 input neurons

### Hidden Layer
Learns patterns such as:
- edges
- shapes
- curves
- textures

### Output Layer
10 neurons represent 10 clothing categories.

The neuron with highest probability becomes the predicted class.

---

## Technologies Used

| Technology | Purpose |
|---|---|
| Python | Programming Language |
| TensorFlow | Deep Learning Framework |
| Keras | Neural Network API |
| NumPy | Numerical Operations |
| Matplotlib | Visualization |

---

## Workflow

1. Load Fashion MNIST dataset
2. Normalize pixel values
3. Build ANN model
4. Train model using training data
5. Evaluate on test data
6. Predict clothing categories

---

## Important Deep Learning Concepts

### ANN
Artificial Neural Network used for learning image patterns.

### Batch Size
Defines how many images are processed together before updating weights.

### Epoch
One complete pass through the entire training dataset.

### Weights
Parameters learned by the network during training.

---

## Project Structure

```bash
Fashion_MNIST_Classification_Project.ipynb
README.md
```

---

## How to Run

### Install Dependencies

```bash
pip install tensorflow numpy matplotlib
```

### Run Notebook

```bash
jupyter notebook
```

Open:

```bash
Fashion_MNIST_Classification_Project.ipynb
```

---

## Example Prediction Flow

```text
Image → Pixels → ANN → Probabilities → Predicted Label
```

---

## Future Improvements
- Use CNN for better image accuracy
- Add confusion matrix visualization
- Hyperparameter tuning
- Deploy model using Streamlit

---

## Author
Ayush Agarwal

---

## License
This project is created for educational and learning purposes.

