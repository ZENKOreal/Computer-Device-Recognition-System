### Computer-Device-Recognition-System


This Project utilizes AI to detect Keyboards, Mouses, Headphones, and Monitors. The reason i created this project was to play around with AI detection, and get more familiar with it, this could be used for a simple classification testing, or example.

### The Algorithm

THis program uses detectnet and resnet -18 for this project. First, i used the image database from https://storage.googleapis.com/openimages/web/visualizer/index.html?set=train&type=detection&c=%2Fm%2F014j1m to download on my Jetson Orin. To do this i went to my jetson-inference directory by using "cd ~/jetson-inference/" in the terminal, then i went to the docker using "./docker/run.sh" in the terminal as well. From there, i navigated into the detection ssd folder by running "cd python/training/detection/ssd" in the docker. To download the specific images used for this project i ran the detection ssd section of the docker:

python3 open_images_downloader.py --max-images=5000
--class names "Computer keyboard, Computer monitor, Computer mouse, Headphones"
--data=data/items

Once the images were downloaded, i trained my model using this code:

python3 train_ssd.py --data=data/items --model-dir=models/person --batch-size=4 --epochs=50

this code will train the ai.. note: (it can take a while!)


### Running This Project

To run this this AI detection of everday technology hardware, a few steps must be done! Write the first 3 in the terminal

1. cd ~/jetson-inference/

2. ./docker/run.sh

3. cd python/training/detection/ssd

4. Find and download a video that includes any of the hardware mentioned in the class names as an .mp4

5. (Optional) Rename the vid, so its easier to organize and find

6. Drag and drop the downloaded video into the data directory with all the other directories are, and its under jetson-inference/python/training/detection/ssd/data

7. Convert the trained model from PyTorch to ONNX so it can be loaded with TensorRT by using the following code in the docker: python3 onnx_export.py --model-dir=models/items

8. Use detectnet to load our cudtom SSD-Mobilenet ONNX model by using the following code in the docker: detectnet --model=models/items/ssd-mobilenet.onnx --labels=models/person/labels.txt --input blob=input_0 -- output-cvg=scores --output-bbox=boxes data/computer_video.mp4 data/output.mp4

for each video change the output to the respective number!
