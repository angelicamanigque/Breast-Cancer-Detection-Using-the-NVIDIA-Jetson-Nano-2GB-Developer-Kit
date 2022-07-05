# Setup
## Setting Up the Nano
If your Jetson Nano isn't set up yet, here's a tutorial on how to do that: https://www.youtube.com/watch?v=uvU8AXY1170&ab_channel=NVIDIADeveloper

## Making a Swap File
The amount of swap in the nano is not enough to be able to pull Roboflow from the library. To make more swap storage, follow these steps after you've logged into the Jetson: <br />
1. First, disable swap so we can make changes: 
    >sudo swapoff -a
2. Next, create a swap file. This will take quite a while: 
    >sudo dd if=/dev/zero of=/swapfile bs=1G count=10 status=progress
3. Once the swap file is created, give it root-only permissions:
    >sudo chmod 600 /swapfile
4. Then mark your file as swap storage:
    >sudo mkswap /swapfile
5. Turn swap back on:
    >sudo swapon /swapfile
6. Use this command to check if your swap file was created:
    >sudo swapon --show
7. Lastly, set the swap file as permanent so you don't lose it after leaving the terminal:
    >echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
8. After rebooting, make sure to check if the swap is still there:
    >free -m
9. Finished! You are now ready to start making the [breast cancer detection project](https://github.com/angelicamanigque/Breast-Cancer-Detection-Using-the-NVIDIA-Jetson-Nano-2GB-Developer-Kit/blob/main/Creating_The_Project.md)! :)
