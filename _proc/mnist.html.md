::: {#cell-0 .cell execution_count=2}
``` {.python .cell-code}
# Run this block and make sure it is successful.
import numpy as np  # used for handling mathematical operations easier in python
import tensorflow as tf  # used for creating and training machine learning models 
from tensorflow import keras # Keras contains the mnist dataset
from matplotlib import pyplot as plt  # library that helps us visualize data by creating plots 
import seaborn as sn # visualization library for creating more complex graphs
```
:::


<!-- WARNING: THIS FILE WAS AUTOGENERATED! DO NOT EDIT! -->

::: {#cell-2 .cell execution_count=3}
``` {.python .cell-code}
#TODO: Add the split data line here
(x_train, y_train), (x_test, y_test) = keras.datasets.mnist.load_data()
```
:::


::: {#cell-3 .cell execution_count=12}
``` {.python .cell-code}
def plotImage(index,x_train,x_test,y_train):
    # Normalize the values so they are a decimal between 0 and 1
    x_train = x_train/255
    x_test = x_test/255
    print(f'The plot represents the number {y_train[index]}')
    plt.imshow(x_train[index],cmap = plt.cm.binary )
    plt.show()


#TODO: change the index to see other values in the dataset
plotImage(6, x_train, x_test, y_train)
```

::: {.cell-output .cell-output-stdout}
```
The plot represents the number 1
```
:::

::: {.cell-output .cell-output-display}
![](mnist_files/figure-html/cell-4-output-2.png){}
:::
:::


::: {#cell-4 .cell execution_count=13}
``` {.python .cell-code}
# TODO: Add reshape the data in this code block

x_train_flat = x_train.reshape(len(x_train), 784)
x_test_flat = x_test.reshape(len(x_test), 784)
```
:::


::: {#cell-5 .cell execution_count=15}
``` {.python .cell-code}
# Model creation
model = keras.Sequential([
    keras.layers.Dense(128, input_shape = (784,),activation='relu'), 
    keras.layers.Dense(64, activation='sigmoid'), 
    keras.layers.Dense(32, activation='sigmoid'), 
    keras.layers.Dense(10, activation='sigmoid')
])
model.compile (
    optimizer='adam',
    loss = 'sparse_categorical_crossentropy',
    metrics = ['accuracy']
)
```
:::


::: {#cell-6 .cell execution_count=16}
``` {.python .cell-code}
# TODO: Train the model using model.fit here 

model.fit(x_train_flat, y_train, epochs=5)
```

::: {.cell-output .cell-output-stdout}
```
Epoch 1/5
1875/1875 [==============================] - 3s 1ms/step - loss: 0.7143 - accuracy: 0.8202
Epoch 2/5
1875/1875 [==============================] - 2s 1ms/step - loss: 0.3272 - accuracy: 0.9050
Epoch 3/5
1875/1875 [==============================] - 2s 1ms/step - loss: 0.2886 - accuracy: 0.9139
Epoch 4/5
1875/1875 [==============================] - 2s 1ms/step - loss: 0.2718 - accuracy: 0.9194
Epoch 5/5
1875/1875 [==============================] - 2s 1ms/step - loss: 0.2696 - accuracy: 0.9197
```
:::

::: {.cell-output .cell-output-display execution_count=16}
```
<keras.src.callbacks.History at 0x2930c834c90>
```
:::
:::


::: {#cell-7 .cell execution_count=17}
``` {.python .cell-code}
# TODO: Test the model against the test data using model.evaluate
model.evaluate(x_test_flat, y_test)
```

::: {.cell-output .cell-output-stdout}
```
313/313 [==============================] - 0s 965us/step - loss: 0.2696 - accuracy: 0.9178
```
:::

::: {.cell-output .cell-output-display execution_count=17}
```
[0.26959657669067383, 0.9178000092506409]
```
:::
:::


::: {#cell-8 .cell}
``` {.python .cell-code}
# TODO: Run the confusion matrix
def plot_confusion_matrix(model, x_test_flat, y_test):
    # Make predictions
    y_pred = model.predict(x_test_flat)
    y_pred_labels = [np.argmax(i) for i in y_pred]
    # Create confusion matrix
    confusion_matrix = tf.math.confusion_matrix(labels=y_test, predictions=y_pred_labels)
    # Plot the confusion matrix
    plt.figure(figsize=(8, 6))
    sn.heatmap(confusion_matrix, annot=True, fmt='d', cmap='Blues', cbar_kws={'label': 'Count'})
    plt.title('Confusion Matrix')
    plt.xlabel('Predicted Label')
    plt.ylabel('True Label')
    plt.show()

# TODO: Uncomment out the line below and run the code block
plot_confusion_matrix(model,x_test_flat,y_test)
```

::: {.cell-output .cell-output-stdout}
```
313/313 ━━━━━━━━━━━━━━━━━━━━ 0s 448us/step
```
:::

::: {.cell-output .cell-output-display}
![](mnist_files/figure-html/cell-9-output-2.png){}
:::
:::


