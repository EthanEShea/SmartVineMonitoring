# SmartVineMonitoring

## Goal of this Project:
Develop a YOLO-Based AI model that can detect diseased/unhealthy vine leaves in the Iron Horse Vineyards    

---

## Extracting Close up Images of Leaves From 360 degree Video:  
Step1: Open LeaveExtraction.ipynb using: VS Code, Jupyter Notebook, or Google Colab  
Step2: Place your Video in dataWine/Video  
Step3: Run all notebook cells:  
  -This notebook will extract frames from the video, detect vineyard leaves using YOLOv8, save the cropped photos automatically  

Output:   
  -The extracted frames are put in dataWine/Frames   
  -The cropped images are put in dataWine/yolo_results   

Notes:   
-Even though the YOLO model we used wasn't trained on many images, it worked well for the videos we had
  
---
