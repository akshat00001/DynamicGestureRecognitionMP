# Dynamic Hand and Finger GestureRecognitionMP

Estimate hand pose using MediaPipe(Python version).<br> This is a sample program that recognizes hand signs and finger gestures with a simple MLP using the detected key points.
![project](https://user-images.githubusercontent.com/100310619/208150909-3eb2e972-24cc-49ac-98ce-fd2a49d3e662.gif)

This repository contains the following contents.
- Sample program
- Hand sign recognition model(TFLite)
- Finger gesture recognition model(TFLite)
- Learning data for hand sign recognition and notebook for learning
- Learning data for finger gesture recognition and notebook for learning

# Requirements

- mediapipe 0.8.1
- OpenCV 3.4.2 or Later
- Tensorflow 2.3.0 or Later<br>tf-nightly 2.5.0.dev or later (Only when creating a TFLite for an LSTM model)
- scikit-learn 0.23.2 or Later (Only if you want to display the confusion matrix)
- matplotlib 3.3.2 or Later (Only if you want to display the confusion matrix)

# Demo

Here's how to run the demo using your webcam.

```bash
python app.py
```

The following options can be specified when running the demo.

- --device<br>Specifying the camera device number (Default：0)
- --width<br>Width at the time of camera capture (Default：960)
- --height<br>Height at the time of camera capture (Default：540)
- --use_static_image_mode<br>Whether to use static_image_mode option for MediaPipe inference (Default：Unspecified)
- --min_detection_confidence<br>
  Detection confidence threshold (Default：0.5)
- --min_tracking_confidence<br>
  Tracking confidence threshold (Default：0.5)

# Directory

<pre>
│  app.py
│  keypoint_classification.ipynb
│  point_history_classification.ipynb
│
├─model
│  ├─keypoint_classifier
│  │  │  keypoint.csv
│  │  │  keypoint_classifier.hdf5
│  │  │  keypoint_classifier.py
│  │  │  keypoint_classifier.tflite
│  │  └─ keypoint_classifier_label.csv
│  │
│  └─point_history_classifier
│      │  point_history.csv
│      │  point_history_classifier.hdf5
│      │  point_history_classifier.py
│      │  point_history_classifier.tflite
│      └─ point_history_classifier_label.csv
│
└─utils
    └─cvfpscalc.py
</pre>

### app.py

This is a sample program for inference.<br>
In addition, learning data (key points) for hand sign recognition,<br>
You can also collect training data (index finger coordinate history) for finger gesture recognition.

### keypoint_classification.ipynb

This is a model training script for hand sign recognition.

### point_history_classification.ipynb

This is a model training script for finger gesture recognition.

### model/keypoint_classifier

This directory stores files related to hand sign recognition.<br>
The following files are stored.

- Training data(keypoint.csv)
- Trained model(keypoint_classifier.tflite)
- Label data(keypoint_classifier_label.csv)
- Inference module(keypoint_classifier.py)

### model/point_history_classifier

This directory stores files related to finger gesture recognition.<br>
The following files are stored.

- Training data(point_history.csv)
- Trained model(point_history_classifier.tflite)
- Label data(point_history_classifier_label.csv)
- Inference module(point_history_classifier.py)

### utils/cvfpscalc.py

This is a module for FPS measurement.

# Training

Hand sign recognition and finger gesture recognition can add and change training data and retrain the model.

### Hand sign recognition training

#### 1.Learning data collection

Press "k" to enter the mode to save key points（displayed as 「MODE:Logging Key Point」）<br>
<img width="480" alt="image" src="https://user-images.githubusercontent.com/100310619/208147351-e79cddeb-382f-4a1d-88b6-f8a2bb5d97e3.png" width="60%"><br><br>
If you press "0" to "9", the key points will be added to "model/keypoint_classifier/keypoint.csv" as shown below.<br>
1st column: Pressed number (used as class ID), 2nd and subsequent columns: Key point coordinates<br>
<img src="https://user-images.githubusercontent.com/37477845/102345725-28d26280-3fe1-11eb-9eeb-8c938e3f625b.png" width="80%"><br><br>
The key point coordinates are the ones that have undergone the following preprocessing up to ④.<br>
<img src="https://user-images.githubusercontent.com/37477845/102242918-ed328c80-3f3d-11eb-907c-61ba05678d54.png" width="80%">
<img src="https://user-images.githubusercontent.com/37477845/102244114-418a3c00-3f3f-11eb-8eef-f658e5aa2d0d.png" width="80%"><br><br>
In the initial state, three types of learning data are included: open hand (class ID: 0), close hand (class ID: 1), and pointing (class ID: 2).<br>
If necessary, add 3 or later, or delete the existing data of csv to prepare the training data.<br>
<img width="223" alt="image" src="https://user-images.githubusercontent.com/100310619/208147763-27609e46-d54d-439b-a9a2-fe1941cd7a9f.png" width="25%">
<img width="215" alt="image" src="https://user-images.githubusercontent.com/100310619/208147961-e7089103-487e-45d4-bab0-5599f1ea576b.png" width="25%">
<img width="208" alt="image" src="https://user-images.githubusercontent.com/100310619/208148296-a5222021-0816-41ab-a715-75d952d3c815.png" width="25%">

#### 2.Model training

Open "[keypoint_classification.ipynb](keypoint_classification.ipynb)" in Jupyter Notebook and execute from top to bottom.<br>
To change the number of training data classes, change the value of "NUM_CLASSES = 3" <br>and modify the label of "model/keypoint_classifier/keypoint_classifier_label.csv" as appropriate.<br><br>

#### X.Model structure

The image of the model prepared in "[keypoint_classification.ipynb](keypoint_classification.ipynb)" is as follows.
<img src="https://user-images.githubusercontent.com/37477845/102246723-69c76a00-3f42-11eb-8a4b-7c6b032b7e71.png" width="50%"><br><br>

### Finger gesture recognition training

#### 1.Learning data collection

Press "h" to enter the mode to save the history of fingertip coordinates (displayed as "MODE:Logging Point History").<br>
<img width="482" alt="image" src="https://user-images.githubusercontent.com/100310619/208147065-aa8d8cef-ef68-4f43-acc1-5bf5cb492fd4.png" width="60%"><br><br>
If you press "0" to "9", the key points will be added to "model/point_history_classifier/point_history.csv" as shown below.<br>
1st column: Pressed number (used as class ID), 2nd and subsequent columns: Coordinate history<br>
<img src="https://user-images.githubusercontent.com/37477845/102345850-54ede380-3fe1-11eb-8d04-88e351445898.png" width="80%"><br><br>
The key point coordinates are the ones that have undergone the following preprocessing up to ④.<br>
<img src="https://user-images.githubusercontent.com/37477845/102244148-49e27700-3f3f-11eb-82e2-fc7de42b30fc.png" width="80%"><br><br>
In the initial state, 4 types of learning data are included: stationary (class ID: 0), clockwise (class ID: 1), counterclockwise (class ID: 2), and moving (class ID: 4). <br>
If necessary, add 5 or later, or delete the existing data of csv to prepare the training data.<br>
<img width="209" alt="image" src="https://user-images.githubusercontent.com/100310619/208148589-7888e9a3-49e9-49bf-8042-88da3e8ef042.png" width="20%">
<img width="204" alt="image" src="https://user-images.githubusercontent.com/100310619/208148781-29627429-68ee-4536-af31-071227e26a9e.png" width="20%">
<img width="207" alt="image" src="https://user-images.githubusercontent.com/100310619/208149032-391db284-e030-4a37-93eb-d2c99165d5a1.png" width="20%">
<img width="233" alt="image" src="https://user-images.githubusercontent.com/100310619/208149122-fd1dd6b1-a26e-4431-a18b-b846ece54916.png" width="20%">

#### 2.Model training

Open "[point_history_classification.ipynb](point_history_classification.ipynb)" in Jupyter Notebook and execute from top to bottom.<br>
To change the number of training data classes, change the value of "NUM_CLASSES = 4" and <br>modify the label of "model/point_history_classifier/point_history_classifier_label.csv" as appropriate. <br><br>

#### X.Model structure

The image of the model prepared in "[point_history_classification.ipynb](point_history_classification.ipynb)" is as follows.
<img src="https://user-images.githubusercontent.com/37477845/102246771-7481ff00-3f42-11eb-8ddf-9e3cc30c5816.png" width="50%"><br>
The model using "LSTM" is as follows. <br>Please change "use_lstm = False" to "True" when using <br>
<img src="https://user-images.githubusercontent.com/37477845/102246817-8368b180-3f42-11eb-9851-23a7b12467aa.png" width="60%">

# Reference

- [MediaPipe](https://mediapipe.dev/)

# Author

Akshat Thakur

# License

hand-gesture-recognition-using-mediapipe is under [Apache v2 license](LICENSE).
