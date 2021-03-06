# Project Write-Up

The model I have used in my project is 'SSD ModelNet V2'. This model was taken from website:
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

While converting pre-trained model into IR, Model Optimizer searches for each layer of the input model in the list of known layers. The list of known layers is different for each of supported frameworks. Some of the layers in a topology might not be included into the list of known layers. The layers that are not in the list of known layers are known as custom layers.

Custom layers are important to use because:
* If your layer output shape depends on dynamic parameters, input data or previous layers parameters, calculation of output shape of the layer via model used can be incorrect. In this case, you need to patch it on your own.
* If the calculation of output shape of the layer fails inside the framework, Model Optimizer is unable to produce any correct Intermediate Representation and you also need to investigate the issue in the implementation of layers and patch it.
* You are not able to produce Intermediate Representation on any machine that does not have model installed. If you want to use Model Optimizer on multiple machines, your topology contains Custom Layers and you use CustomLayersMapping.xml to fallback on it, you need to configure on each new machine.

Some of the potential reasons for handling custom layers is to optimize our pre-trained models and convert them to a intermediate representation(IR) without a lot of loss of accuracy and shrink and speed up the Performance so that desired output is resulted.

## Comparing Model Performance

Although models are shriked in size and run faster inference  after conversion, that does not ensure a higher inference accuracy. In fact, there will be some loss of accuracy as a result of potential changes like lower precision. 

The size of the model pre- and post-conversion were 67 MB and 65 MB respectively.

The inference time of the model pre- and post-conversion were 50ms and 60 ms respectively.

Though in some frames the model could not detect a person, it can successfully detect 6 persons in total. The average duration also changes throughout the video as shown in the following table:

| Total Person detected | Average duration  |
|-----------------------|-------------------|
|           1           |     0.21s         |
|           2           |     0.27s         |
|           3           |     0.28s         |
|           4           |     0.26S         |
|           5           |     0.29s         |
|           6           |     0.27s         |
|-----------------------|-------------------|

![people-counter-python](people_counter.gif)   

## Assess Model Use Cases

I have used a similar model to visulaize rush hour traffic in my other [project](https://github.com/ahmedhasandrlnd/rush_hour_traffic_visualization).

![Rush hour traffic](rush7.gif)

## Assess Effects on End User Needs

Lighting, model accuracy, and camera focal length/image size have different effects on a deployed edge model. The potential effects of each of these are as follows:

* In case of poor lighting model's accuracy may fail dramatically or even completely drop close to zero.

* Natural decrease in model accuracy during conversion or other stages may make the model unusable.

* Distorted input from camera due to change in focal length and/or image size will affect the model because the model may fail to make sense of the input and the distored input may not be detected properly by the model. 

## Model Research
[This heading is only required if a suitable model was not found after trying out at least three different models. However, you may also use this heading to detail how you converted a successful model.]

Our 'SSD ModelNet V2' model was sufficient for the app since it detected all 6 persons in the video with their average duration times.
