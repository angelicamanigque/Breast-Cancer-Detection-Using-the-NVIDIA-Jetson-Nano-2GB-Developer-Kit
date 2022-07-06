# Creating the Project
## Installing the Roboflow Docker
After making the swap file during [setup](https://github.com/angelicamanigque/Breast-Cancer-Detection-Using-the-NVIDIA-Jetson-Nano-2GB-Developer-Kit/blob/main/Setup.md), you'll have enough storage to install the docker container from roboflow. This will be done in two easy steps. Make sure your swap is on! <br />
1. First, just pull the container: 
    >sudo docker pull roboflow/inference-server:jetson
2. Then run it: 
    >sudo docker run --net=host --gpus all roboflow/inference-server:jetson
3. You can now move on to making the code file for the model!

## Making the Code File
Before starting this entire section, I recommend having the page for the [roboflow model](https://app.roboflow.com/nvidia-jetson-nano-2gb/breast-cancer-detection/1) open. Then scroll down and click the section that says "Use curl command". The pop-up will contain information that you will need later. <br />
![curl_command](https://user-images.githubusercontent.com/95183346/177419217-d08159de-4a3b-4154-8c81-f335b30c9cb6.PNG)
Without further ado, let's get started: <br />
1. First, download a test image that you want the model to classify. Then, go to [this link](convertfiles.com) and convert your image to either a jpg or png. Once you get the link, copy and paste it as follows:
    >wget [paste your link here]
2. Next, you'll need to install "requests", something you'll use later in the code:
    >sudo pip3 install requests pillow
3. After the installation is complete, create a new python file called cancer_model:
    >nano cancer_model.py <br />
  
Next up are some steps for uploading your image using base64 so it can be run through the model. For more ways to do this, check out [this link](https://docs.roboflow.com/inference/hosted-api). <br />
1. First, you'll make variables for your key and image path. You'll reference these later in the code. This is the part where you'll need the information from the curl command. Copy and paste sections of the link as follows, and, as always, change the image to your image path if you are not using the sample image:
    >MY_KEY = "Cszeb8xRff1g8RjgrNxp"
    >img_path = "malignant.jpg"
3. The next couple of lines in your code will be imports:
    >import requests <br />
    >import base 64 <br />
    >import io <br />
    >import PIL <br />
    >from PIL import UnidentifiedImageError <br />
    >from PIL import Image <br />
4. Then load the image using PIL (replace "malignant.jpg" with whatever image you're using):
    >image = Image.open("malignant.jpg").convert("RGB")
5. Convert the image to JPEG:
    >buffered = io.BytesIO() <br />
    >image.save(buffered, quality = 90, format = "JPEG") <br />
6. Encode the image to Base 64:
    >img_str = base64.b64encode(buffered.getvalue()) <br />
    >img_str = img_str.decode("ascii") <br />
7. Create the URL:
    >upload_url = "".join(["https://detect.roboflow.com/breast-cancer-detection/1","?api_key="+MY_KEY,"&name="+img_path])
8. Post the URL to the API:
    >r = requests.post(upload_url, data=img_str, headers={"Content-Type": "application/x-www-form-urlencoded"})
9. This last command prints out the result!
    >print(r.json()) <br />
10. Once you're done typing in the code, do Ctrl+X, hit Y. and press Enter to save your file.
11. Finally, in the command line, type in
    >python3 cancer_model.py <br />
   
You should see the results! :D
 
![da_prediction](https://user-images.githubusercontent.com/95183346/177419408-711be3eb-5ffd-471f-ad12-326bfa8d1492.PNG)
