# Project Write-Up

You can use this document as a template for providing your project write-up. However, if you
have a different format you prefer, feel free to use it as long as you answer all required
questions.

Notes: The template has been faithfully followed for the project write up. 

## Explaining Custom Layers

Custom layers are layers that are not included in the list of known layers used in a neural network. If one's topology contains any layers that are not in the list of known layers, the Model Optimizer classifies them as custom.

The process behind converting custom layers involves both custom layer extensions for the Model Optimizer, as well as custom layer extensions for the Inference Engine. Regarding the former, the extensions needed for the IR are the Custom Layer Extractor (that is responsible for identifying the custom layer operation and extracting the parameters for each instance of the custom layer) and the Custom Layer Operation (that is responsible for specifying the attributes that are supported by the custom layer and computing the output shape for each instance of the custom layer from its parameters). Regarding the latter, the extensions neede for the IE are Custom Layer CPU/GPU Extensions (where each plugin includes a library of optimized implementations to execute known layer operations which must be extended to execute a custom layer). 

Some of the potential reasons for handling custom layers are when some special transformations (especially non-linear ones) are required for any layer in a neural network. Say for example I would like to use a polynomial as the transformation in a custom layer. As this kind of layer is not listed as a known layer used in a neural network, I will need to add this to the topology of my neural network architecture. 

## Comparing Model Performance

My method(s) to compare models before and after conversion to Intermediate Representations
were to use some performance measure to gauge performance with the stats of the output of the converted model. 

The difference between model accuracy pre- and post-conversion was more or less the same after the conversion, even though the performance pre-conversion was a lot more accurate. 

The size of the model pre- and post-conversion was within 10 orders of magnitude apart. 

The inference time of the model pre- and post-conversion has been significantly reduced by at least an order of magnitude difference. 

## Assess Model Use Cases

Some of the potential use cases of the people counter app are:

1. To tract the movement of crowds in a public space, especially for security purposes. 
2. To tract the movement of individual activities in a shop, in order to monitor for any illegal activities especially shoplifting. 
3. To monitor patient activities inside a room, especially in a hospital setting. 

Each of these use cases would be useful because it can free up human resources and help to identify problematic activities as they happen, and alert the management of a premise to jump into action more accurately and reliably. 

## Assess Effects on End User Needs

Lighting, model accuracy, and camera focal length/image size have different effects on a
deployed edge model. The potential effects of each of these are as follows:

1. Lighting: The darker the space the lower the accuracy. 
2. Model accuracy: Should be accurate enough for production use even after model conversion.
3. Image size: Should allow a large proportion of the people to be captured through the frame. 

## Model Research

[This heading is only required if a suitable model was not found after trying out at least three
different models. However, you may also use this heading to detail how you converted 
a successful model.]

In investigating potential people counter models, I tried each of the following three models:

- Model 1: R-CNN
  - [PyTorch Mash R-CNN](https://pytorch.org/tutorials/intermediate/torchvision_tutorial.html)
  - I converted the model to an Intermediate Representation with the following argument structure: `python3 mo.py --input_model INPUT_MODEL`
  - The model was insufficient for the app because the accuracy was too low after conversion and the size of the model too big for implementation at the edge
  - I tried to improve the model for the app by using quantization for the precision
  
- Model 2: SSD
  - [SSD by NVDIA](https://pytorch.org/hub/nvidia_deeplearningexamples_ssd/)
  - I converted the model to an Intermediate Representation with the following argument structure: `python3 mo.py --input_model INPUT_MODEL`
  - The model was insufficient for the app because the accuracy was too low after conversion and the size of the model too big for implementation at the edge
  - I tried to improve the model for the app by using quantization for the precision

- Model 3: YOLO
  - [YOLO with darknet](https://pjreddie.com/darknet/yolo/)
  - I converted the model to an Intermediate Representation with the following argument structure: `python3 mo.py --input_model INPUT_MODEL`
  - The model was insufficient for the app because the accuracy was too low after conversion and the size of the model too big for implementation at the edge
  - I tried to improve the model for the app by using quantization for the precision

- DNN Model that worked: person-detection-retail-0013
  - [/opt/intel/openvino/deployment_tools/intel_models/person-detection-retail-0013](https://docs.openvinotoolkit.org/latest/_models_intel_person_detection_retail_0013_description_person_detection_retail_0013.html)
  - I converted the model to an Intermediate Representation with the following argument structure: `python3 mo.py --input_model INPUT_MODEL`
  - The model sufficient for the app it was able to track most people passing through the frame, though overcounting due to the same people returning to the frame could be an issue and needs extra work to address. 
  - I tried to improve the model for the app by using FP16 or even FP32 for the precision. 
