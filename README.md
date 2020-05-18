# Project Objective

The objective of this project/product was to create a model that can utilize computer vision to detect objects of interest, such as name entities..., in documents. This is a trivial task for NLP models given access to linguistic knowledge. However, if the linguistic knowledge is not available, such as an ancient language, or a language that is unfamilar to the user, computer vision can solve the problem.

# Approach

A pytorch-based bounding box technique with RetinaNet architecture and Resnet encoder is utilized for this project  
### Steps
1- Resizing images before annotating them in VGG Image Annotator ( My preference, given the fact that the data provider may send the training images in different sizes)  
2- Reformating the cocoformat jason file that was exported from VIA software to a format that can be used in RetinaNet architecture ( two slightly different methods shown)    
3- Function to get the annotations from JASON file, and grab the bbox annotations and zip them in a dictionary with their respective image names from the data   
4- Generate anchors ( comments in the notebook expand on the process)  
5- LOSS Function ( Focal loss function is used here, the notebook shows the function for the purpose of understanding and playing with, however, the libraries that you load at the top of the notebook would load the loss function and other functions and classes required)  
6- Instantiate the encoder object ( Resnet in this case)  
7- Instantiate RetinaNet model with the required parameters: RetinaNet(encoder, n_classes= n_classes, n_anchors= nanchors, sizes= s, chs= chs, n_conv=2)  
8- Set up the input data bunch: Image size, batch size, explore the transform() function and add any transforms that would make sense for your data (image type, and problem). Remember to make sure bounding boxes are transformed as well. this doesnt mean they get rotated or sheared, makes sure bbox doesn't relocate away from its object  
9- Visualize a batch of images with bounding boxes and assigned categories  
10- Visualize the bounding boxes that failed to be found for your objects, by seeing the objects on the left side which are covered by anchors, whereas on the right side you can see the objects that would be missed in training. This would help you to go back and play with anchor generator parameters to ensure better training  
11- Instantiate your Learner and evaluation metrics, seting up hyperparameters such as Non-Maximum-Threshold ...  
12- Split the Learner and add layers for bounding box regression and category classification at the end (top) of your Learner  
13- Examin the optimum range of learning rate (by visual inspection of LR-Loss graph, and chose the fastest loss descent point) for your model by running a small batch through your model.  
14 - Start unfreezing few layers at a time, train, examine the learning rate vs loss graph, adjust the learning range for your layers, and iterate though this step until all layers are trained  
15- Visualize the performance of your model at the end by showing images with ground truth Bboxes and Predicted Bboxes, side by side  
16- Run a prediction of your model on a separate test set at the end

# Data

PDF files converted to images for use in a bounding box annotation software such as VGG Image Annotator.  
Ensure you create a folder path object such as : path = Path('/content/drive/My Drive/comp_vision/object_detection1') and create two subfolders in it: one for your training/validation images, which I have callsed train_sample, and another folder called annotations, which contain the coco format JASON file for your annotations that you modified in the code to adapt to RetinaNet model  


