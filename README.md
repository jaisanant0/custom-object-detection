# Custom Object Detection - Tensorflow
Object detection allow us to recognize, detect and localize the object(one or more) in an image. 


## Getting started 
- Clone the repository and extract the files inside the custom-object-detection to a directory.
## Installation
```
pip3 install -r requirements.txt
```
- Compile the protbuf 

```
protoc object_detection/protos/*.proto --python_out=.
```

## Download the base model (Fatser rcnn inception v2 )
Since training the object detection from the scratch could take days or weeks even on GPUs. 
For the hack ! We can use already trained network on different dataset and modify it to detect our custom selected objects.

You can download the pretrained model from [tensorflow model zoo](https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/detection_model_zoo.md)
The documentation show the speed and accuracy of different model, here i have used faster_rcnn_inception_v2_coco model.

1. Download the model.
2. Extract it in a folder.
3. Copy the folder in the same directory where the cloned repository's folders are present.

## Training
You can either train the model locally or you can use cloud to train the same.

``` 
python3 object_detection/train.py --logtostderr --train_dir=<path to train dir> --pipeline_config_path=<path to model config file>
```
NOTE :
1. I have provided the config file inside training directory.

2. Training and testing tensorflow record file and label map are in annotation directory.
      
you can stop the training if the loss values is below 0.05 consistently. time to train depends upon hardware configuration for the same set of data.
My training was finished around 16K steps.

### Export the inference graph
After the training is finished, You can find the checkpoints in <train dir>.
In order to export the inference graph, use the latest checkpoint(highest number) : 
    1. model.ckpt-<training step>.data- , 
    2. model.ckpt-<training step>.index and
    3. model.ckpt-<training step>.meta 

Now from object_detection directory run :
```
python3 export_inference_graph.py --input_type image_tensor --pipeline_config_path <path to config file> --trained_checkpoint_prefix <path to model.ckpt-(training step) --output_directory <path to save the trained model>
```
## Testing 
Run :
```
python3 custom_object_detect.py -h
```
## Result

![Screenshot](custom-object-detection/screenshot/271.jpg.png)
![Screenshot](custom-object-detection/screenshot/298.jpg.png)
