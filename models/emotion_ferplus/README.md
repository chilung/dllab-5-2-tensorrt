# FER+ Emotion Recognition

## Description
This model is a deep convolutional neural network for emotion recognition in faces. 

## Model

| Model          | Checksum  | Download (with sample test data) | ONNX version | Opset version | 
|----------------|:----------|:-----------|:--------|:-------------|
|Emotion FERPlus |[MD5](https://onnxzoo.blob.core.windows.net/models/opset_2/emotion_ferplus/emotion_ferplus-md5.txt)|[31 MB](https://onnxzoo.blob.core.windows.net/models/opset_2/emotion_ferplus/emotion_ferplus.tar.gz)|1.0|2|
|                |[MD5](https://onnxzoo.blob.core.windows.net/models/opset_7/emotion_ferplus/emotion_ferplus-md5.txt)|[31 MB](https://onnxzoo.blob.core.windows.net/models/opset_7/emotion_ferplus/emotion_ferplus.tar.gz)|1.2|7|
|                |[MD5](https://onnxzoo.blob.core.windows.net/models/opset_8/emotion_ferplus/emotion_ferplus-md5.txt)|[31 MB](https://onnxzoo.blob.core.windows.net/models/opset_8/emotion_ferplus/emotion_ferplus.tar.gz)|1.3|8|

### Paper
"Training Deep Networks for Facial Expression Recognition with Crowd-Sourced Label Distribution" [arXiv:1608.01041](https://arxiv.org/abs/1608.01041)

### Dataset
The model is trained on the FER+ annotations for the standard Emotion FER [dataset](https://www.kaggle.com/c/challenges-in-representation-learning-facial-expression-recognition-challenge/data), as described in the above paper.

### Source
The model is trained in CNTK, using the cross entropy training mode. You can find the source code [here](https://github.com/ebarsoum/FERPlus).

### Demo
[Run Emotion_FERPlus in browser](https://microsoft.github.io/onnxjs-demo/#/emotion_ferplus) - implemented by ONNX.js with Emotion_FERPlus version 1.2

## Inference
### Input
The model expects input of the shape `(Nx1x64x64)`, where `N` is the batch size.
### Preprocessing
Given a path `image_path` to the image you would like to score:
```python
import numpy as np
from PIL import Image

def preprocess(image_path):
  input_shape = (1, 1, 64, 64)
  img = Image.open(image_path)
  img = img.resize((64, 64), Image.ANTIALIAS)
  img_data = np.array(img)
  img_data = np.resize(img_data, input_shape)
  return img_data
```

### Output
The model outputs a `(1x8)` array of scores corresponding to the 8 emotion classes, where the labels map as follows:  
`emotion_table = {'neutral':0, 'happiness':1, 'surprise':2, 'sadness':3, 'anger':4, 'disgust':5, 'fear':6, 'contempt':7}`
### Postprocessing
Route the model output through a softmax function to map the aggregated activations across the network to probabilities across the 8 classes.

```python
import numpy as np

def softmax(scores):
  # your softmax function
  
def postprocess(scores):
  ''' 
  This function takes the scores generated by the network and returns the class IDs in decreasing 
  order of probability.
  '''
  prob = softmax(scores)
  prob = np.squeeze(prob)
  classes = np.argsort(prob)[::-1]
  return classes
```
### Sample test data 
Sets of sample input and output files are provided in 
* serialized protobuf TensorProtos (`.pb`), which are stored in the folders `test_data_set_*/`.

## License
MIT