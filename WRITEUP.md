# Project Write-Up

The model I have used in my project is 'frozen_inference_graph'. This model was taken from website:
http://download.tensorflow.org/models/object_detection/ssd_mobilenet_v2_coco_2018_03_29.tar.gz

I converted the model to an Intermediate Representation with the following arguments:
* To download the model: 
```wget http://download.tensorflow.org/models/object_detection/ssd_mobilenet_v2_coco_2018_03_29.tar.gz```
* To unzip the model: 
```tar -xvf ssd_mobilenet_v2_coco_2018_03_29.tar.gz```
* To switch directory: 
```cd ssd_mobilenet_v2_coco_2018_03_29```
* To convert the model to intermediate representation(IR) format:
```python /opt/intel/openvino/deployment_tools/model_optimizer/mo_tf.py --input_model frozen_inference_graph.pb --tensorflow_object_detection_api_pipeline_config pipeline.config --tensorflow_use_custom_operations_config /opt/intel/openvino/deployment_tools/model_optimizer/extensions/front/tf/ssd_v2_support.json --reverse_input_channel```


## Explaining Custom Layers

While converting pre-trained model into IR, Model Optimizer searches for each layer of the input model in the list of known layers.The list of known layers is different for each of supported frameworks.

Custom layers are layers that are not included into a list of known layers. If your topology contains any layers that are not in the list of known layers, the Model Optimizer classifies them as custom.

Custom layers are important to use because:
* If your layer output shape depends on dynamic parameters, input data or previous layers parameters, calculation of output shape of the layer via model used can be incorrect. In this case, you need to patch it on your own.
* If the calculation of output shape of the layer fails inside the framework, Model Optimizer is unable to produce any correct Intermediate Representation and you also need to investigate the issue in the implementation of layers and patch it.
* You are not able to produce Intermediate Representation on any machine that does not have model installed. If you want to use Model Optimizer on multiple machines, your topology contains Custom Layers and you use CustomLayersMapping.xml to fallback on it, you need to configure on each new machine.

Some of the potential reasons for handling custom layers is to optimize our pre-trained models and convert them to a intermediate representation(IR) without a lot of loss of accuracy and shrink and speed up the Performance so that desired output is resulted.

## Comparing Model Performance

My method(s) to compare models before and after conversion to Intermediate Representations
were...

The difference between model accuracy pre- and post-conversion was...

The size of the model pre- and post-conversion was...

The inference time of the model pre- and post-conversion was...

## Assess Model Use Cases

Some of the potential use cases of the people counter app are...

Each of these use cases would be useful because...

## Assess Effects on End User Needs

Lighting, model accuracy, and camera focal length/image size have different effects on a
deployed edge model. The potential effects of each of these are as follows...

## Model Research

[This heading is only required if a suitable model was not found after trying out at least three
different models. However, you may also use this heading to detail how you converted 
a successful model.]

In investigating potential people counter models, I tried each of the following three models:

- Model 1: [Name]
  - [Model Source]
  - I converted the model to an Intermediate Representation with the following arguments...
  - The model was insufficient for the app because...
  - I tried to improve the model for the app by...
  
- Model 2: [Name]
  - [Model Source]
  - I converted the model to an Intermediate Representation with the following arguments...
  - The model was insufficient for the app because...
  - I tried to improve the model for the app by...

- Model 3: [Name]
  - [Model Source]
  - I converted the model to an Intermediate Representation with the following arguments...
  - The model was insufficient for the app because...
  - I tried to improve the model for the app by...
