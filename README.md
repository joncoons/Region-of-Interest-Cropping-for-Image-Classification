# Region-of-Interest-Cropping-for-Image-Classification
Instructions and notebook/code sample to crop detected objects for secondary image classification. 

When performing image analysis, it may prove useful to pursue more of a serial ensemble model approach, where the first model detects your object of interest, and a secondary model(s) then analyzes the object(s) for your desired classes.  This can be very useful if the image has multiple objects being detected with multiple classes for each object, as it simplifies the model training and management. 

You can also use this approach to identify the object of interest and expand the bounding box before cropping to capture additional context to help aid in classification.

Training this secondary classification model, however, means you either already have a dataset of that particular object across multiple states/classes, which is typically not the case, or you have to create your own dataset from your existing data sources.  

To create that training/validation dataset, the notebook in this repo uses an Azure ML Studio notebook, a YOLOv5 object detection model you've already trained in Azure ML AutoML for Images, and your Azure Storage account. 

Assuming you've labeled your images and trained your YOLOv5 object detection model already, you'll want to make a note of a few things you'll need to run the notebook:

    1. Your AutoML Job Name for the YOLOv5 model
    2. Your Azure Storage connection string
    3. Your container name where you're storing your raw images
    4. Your prefix (if used) for your virtual directory within the container

You'll also want to download this repository to your local machine, and then upload the folder into your Azure Machine Learning workspace in the Notebooks screen.

In code block 3 (shown below) there are boolean values for the project type - image or video - which you'll want to make the appropriate selection for your use case.  The minimum_crop_size variable allows you to set a threshold for the minimum size of object you want to a capture for your classification dataset.

![](assets/code_block_3.png)

If you're going to utilize video, there are two other variables in the last code block you'll want to review also at lines 90 and 91 of that block.  For video, you won't necessarily want to capture every frame, so you can set your frames-per-second rate for your video on line 90, and then your inferences-per-second rate on line 91.  The code will calculate when to perform the inference.

![](assets/last_code_block.png)

When your job has completed, you'll have a new datasets for each object type in your Azure Storage which you can now label and train.

In deployment mode, you can utilize this same ensemble code, cropping the image before calling the classification model inference.

Hope you found this useful to your vision analytics project!