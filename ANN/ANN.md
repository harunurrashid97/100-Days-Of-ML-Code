
# Artificial Neural Network

![ANN](ANN-Day7.jpg)

# Importing the libraries
```python
import numpy as np
import matplotlib.pyplot as plt
import pandas as pd
import seaborn as sns
from sklearn.preprocessing import LabelEncoder, OneHotEncoder
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler
# Importing the Keras libraries and packages
import keras
from keras.models import Sequential
from keras.layers import Dense
from sklearn.metrics import confusion_matrix
````
# Importing the dataset
```python
dataset = pd.read_csv('data.csv')
del dataset['Unnamed: 32']
X = dataset.iloc[:, 3:13].values
y = dataset.iloc[:, 13].values

print(dataset.head())

print(dataset.tail())
````
```python
labelencoder_X_1 = LabelEncoder()
X[:, 1] = labelencoder_X_1.fit_transform(X[:, 1])
labelencoder_X_2 = LabelEncoder()
X[:, 2] = labelencoder_X_2.fit_transform(X[:, 2])
onehotencoder = OneHotEncoder(categorical_features = [1])
X = onehotencoder.fit_transform(X).toarray()
X = X[:, 1:]
````
# Splitting the dataset into the Training set and Test set
```python
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.2, random_state = 0)
````
# Feature Scaling
```python
sc = StandardScaler()
X_train = sc.fit_transform(X_train)
X_test = sc.transform(X_test)
````
# Initialising the ANN
```python
classifier = Sequential()
````
# Adding the input layer and the first hidden layer
```python
classifier.add(Dense(6,kernel_initializer = 'uniform', activation = 'relu', input_dim = 11))
````
# Adding the second hidden layer
```python
classifier.add(Dense(6, kernel_initializer = 'uniform', activation = 'relu'))
````
# Adding the output layer
```python
classifier.add(Dense(1, kernel_initializer = 'uniform', activation = 'sigmoid'))
````
# Compiling the ANN
```python
classifier.compile(optimizer = 'adam', loss = 'binary_crossentropy', metrics = ['accuracy'])
```
# Fitting the ANN to the Training set
```python
classifier.fit(X_train, y_train, batch_size = 10, epochs = 100)
````
# Making the predictions and evaluating the model

# Predicting the Test set results
```python
y_pred = classifier.predict(X_test)
y_pred = (y_pred > 0.5)
````
# Making the Confusion Matrix
```python
cm = confusion_matrix(y_test, y_pred)
print("Our accuracy is {}%".format(((cm[0][0] + cm[1][1])/57)*100))
````
# Plot of Confusion Matrix
```python
sns.heatmap(cm,annot=True)
plt.savefig('h.png')
````
