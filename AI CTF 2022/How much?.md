# How much?

We have a dataset with embedding information about house prices called `objects.csv` and `model_ex.pkl`. Our goal is to figure out what actual prices stands behind the model's solutions.
If we take integer parts of these prices and put them in the equations below we can get the flag `AICTF{#a##b##c#_#d#_#e##f#cr#g##h#_c#i#n_y0u_k#j#3p_17}`.
```
#a# = x0 / 8 - 247 
#b# = x1 - 2247
#c# = (x2 - 103) / 256
#d# = sqrt(x3 -2058)
#e# = x4 / 4 - 476 
#f# = ((x5 - 83) / 100 +6) / 9 
#g# = (x6 - 42) / 1000 + 1
#h# =  x7 / 274
#i# = sqrt ((x8 - 28) / 100 - 1) 
#j# = x9 / 968
```
After loading dataset and model we can discover two things. The first thing is that there is only ten rows in the dataset that seems to be unknowns of equations above after model's transformations.
```
import pickle
import pandas as pd

data = pd.read_csv('objects.csv')

model = None
with open('model_ex.pkl', 'rb') as fid:
    model = pickle.load(fid)

data.info()
```
The second thing we can figure out after call `model.summary()` on the different layers is the `layer[0]` have only one neuron on sublayer `dense_2` which is obviously our price value. We can access it just dropping second layer `submodel = model.layers[0]`.

Now just get model's prediction:
```
import tensorflow as tf

def numpy_to_tensor(arr):
  return tf.reshape(tf.convert_to_tensor(arr), (arr.shape[0], 13))

prices = submodel.predict(numpy_to_tensor(data.iloc[:,1:].to_numpy()))

print([int(i) for i in prices])
# [2024, 2247, 1895, 2074, 1924, 2183, 2042, 1918, 1728, 2904]
```
Calculation of equations gives us `AICTF{607_4_53cr37_c4n_y0u_k33p_17}`.