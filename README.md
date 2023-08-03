# characterAI
This model classifies the characters for Chinese numbers, specifically: 一二三四五六七八九十百千万亿.
The model is a re-trained version of the ResNet-18 model and was trained on an inverted version of the Chinese MNIST dataset.

# How to run characterAI
1. In order to test this model, be sure that you have a Jetson-Nano and Python3 and the jetson-inference library installed on your nano.
2. Download the model.pth.tar files:
   model.pth.tar: https://drive.google.com/file/d/16PlL_Pz6d0QKLztq8xe017JCY99YOt2z/view?usp=sharing
3. Download this folder (testing images): https://drive.google.com/drive/folders/1AJYann2UZXb3ldXdVuL4jaFS5wxKIurp?usp=sharing
4. Put your model into jetson-inference/python/training/classification/models/chinese_character_inverted
5. Now you can run the model as previously stated.
7. Put your data into jetson-inference/python/training/classification/data
8. Open up terminal.
9. Export your model to ONNX:

   Make you are in python/training/classification and run:
```
python3 onnx_export.py --model=models/chinese_character_inverted
```
   If you run into issues while exporting your model to ONNX, open a docker container and do the same thing. Command to open the docker container:
   
```
./docker/run.sh
```

   Be sure to exit the docker container before you run the model.
10. Make sure that you are in the jetson-inference/python/training/classification while running this model. Use this command to move to that directory:
```
cd ~/jetson-inference/python/training/classification
```
8. Set these two variables in the terminal. They will store the locations of the data and models that you will be using:
```
NET=models/chinese_character_inverted
DATASET=data/chinese_character_inverted
```
9. Run this command to test your model on an image:
```
imagenet.py --model=$NET/resnet18.onnx --input_blob=input_0 --output_blob=output_0 --labels=$DATASET/labels.txt $DATASET/test/one/input_95_10_2.jpg character.jpg
```
10. Run this command to test your model on a different image:
```
imagenet.py --model=$NET/resnet18.onnx --input_blob=input_0 --output_blob=output_0 --labels=$DATASET/labels.txt $DATASET/{IMAGE PATH} character.jpg
```
11. If you wish to test the model an image of your own, be sure to put your image into data/chinese_character_inverted. It is important to note that this model was trained on images with a white background and black text. The model might be incorrect if given an image where the contrast isn't high enough, or if the image has a different colored background.

Video of my demonstration: https://youtu.be/VdPM8yIn-9w 

# Replication
1. If you wish to replicate this model, you must first download the Chinese MNIST dataset from https://www.kaggle.com/datasets/gpreda/chinese-mnist?datasetId=816649.
2. After you download the data, you need to organize the data into respective folders for their class.
      For this version of the model, you also need to invert all the photos.
      To do this, use pillow to loop through the photos and invert them all.
3. If able, train your model using your Jetson Nano. If you are unable to, upload your data to Google Drive and use Google Calaboratory to train your model.
   If you must use Google Calaboratory, be sure to download the model and put it into models/chinese_character_inverted.
