# SmartVineMonitoring

## Goal of this Project:
Develop a YOLO-Based AI model that can detect diseased/unhealthy vine leaves in the Iron Horse Vineyards    

Link to the Google Drive containing all datasets used, results from k_fold, and the best.pt files for each model. [Google Drive](https://drive.google.com/drive/folders/1rhbNi8iAbascdg9LNWpLKK0kvx1o8cHv?usp=sharing)

---

## Extracting Close up Images of Leaves From 360 degree Video:   
### Goal: The overall goal for this step is to turn the hard to look at 360 videos, into clear grouped frames of leaves.   

Step1: Open LeaveExtraction.ipynb using: VS Code, Jupyter Notebook, or Google Colab  
Step2: Place your Video in dataWine/Video  
Step3: Run all notebook cells:
  - This notebook will extract frames from the video, detect vineyard leaves using YOLOv8, save the cropped photos automatically  

Output:   
  - The extracted frames are put in dataWine/Frames   
  - The cropped images are put in dataWine/yolo_results   

Notes:   
- Even though the YOLO model we used wasn't trained on many images, it worked well for the videos we had
  
---

## How to Work a Notebook to Obtain Previous Results:


Step 1: Pick a notebook from the GoogleColabNotebooks folder

Step 2: Open the notebook in Google Colab for mounting to Google Drive

Step 3: Follow the steps provided in the notebook.
  - All notebooks are set to work with datasets located in your personal Google Drive. To change this, the path to the dataset used will have to be updated.

---

## Key Files and Notebooks

This project is organized into a series of notebooks. They each share common steps, which include mounting to Google Drive, extracting datasets, preparing YOLO-format folders, training YOLO models, evaluating model performance, and displaying review images to download. Since they share similar code, a description will be provided for each notebook. They are located in the GoogleColabNotebook folder

| File | Purpose |
|---|---|
| `Data_Set_Checker.ipynb` | Checks the dataset for duplicate images and unusually large annotation boxes. Used for dataset quality control before training. |
| `YoloModel_FirstAttempts.ipynb` | Early experiment to confirm that YOLO could train on the project dataset. This notebook should be treated as exploratory and not used for future development. |
| `Data_Augmentation.ipynb` | Trains YOLO on an already-augmented YOLO-format dataset and evaluates the results. This notebook uses an augmented dataset and does not generate new augmentations..  |
| `IronHorse_YOLO_Train.ipynb` | Trains and validates a YOLO model on the IronHorse dataset using an 80/20 train-validation split. |
| `K_Fold_IronHorse_YOLO_Tuning.ipynb` | Runs leave-one-video-out / k-fold style training and evaluation on the IronHorse dataset. Later used to adjust confidence and IoU thresholds to find more bounding boxes. |
| `YoloModel_Evaluations_and_IronHorse_FineTuning.ipynb` | Trains and evaluates a base YOLO model on the expertized dataset, then fine-tunes that model on the IronHorse dataset. Also supports evaluating a provided `.pt` model without retraining. |
| `YOLO_Hyperparameter_Tuning.ipynb` | Uses Ray Tune and Ultralytics YOLO to search for better training and augmentation hyperparameters, then trains a final model using the best settings. |
| `YOLO_Freeze_Fold_Evaluation.ipynb` | Tests leave-one-video-out training with optional YOLO layer freezing. The freeze setting is included but may be commented out depending on the run. |
| `Both_Data_Set_YOLO_Train.ipynb` | Trains YOLOv8 on the full expertized dataset, then uses that model as the starting point for leave-one-video-out IronHorse evaluation. |

---

## Datasets Used

The project uses several dataset versions during model development:

| Dataset | Description | Used In |
|---|---|---|
| Complete_Data-set.zip | Modified expertized dataset where large/oversized bounding boxes were removed. Contains image files and LabelMe-style JSON annotations. These annotations are converted into YOLO `.txt` labels before training. | `YoloModel_Evaluations.ipynb`, `Full_Data_Set_Train.ipynb`, `YoloModel_FirstAttempts.ipynb`, `Data_Set_Checker.ipynb` |
| updatedDataSet.zip | YOLO-format dataset used for later training, fine-tuning, and evaluation. | `IronHorseTrain.ipynb`, `YoloModel_Evaluations.ipynb`, `Hyperperameters.ipynb`, `Data_Augmentation.ipynb` |
| SeparatedDataSet.zip | IronHorse dataset organized by video folder so each video can be held out as a validation fold. | `K_Fold_IronHorse.ipynb`, `Freeze.ipynb`, `Full_Data_Set_Train.ipynb` |
| augmented1.zip | YOLO-format dataset containing augmented training images and labels. | `Data_Augmentation.ipynb` |
 ---

## Common Code Workflow

Most notebooks reuse the same general workflow:

1. **Mount Google Drive**  
   Gives Colab access to dataset ZIP files, saved models, and output folders.

2. **Extract dataset ZIP files**  
   Unzips datasets into the Colab runtime.

3. **Prepare YOLO folder structure**  
   Creates folders such as:

   ```text
   images/train
   images/val
   labels/train
   labels/val
   ```
4. **Convert annotations when needed**

   - Some notebooks convert LabelMe-style JSON annotations into YOLO `.txt` label files.

5. **Create YOLO YAML configuration files**

   - Each training run uses a `.yaml` file that defines the dataset paths and class names.

6. **Train YOLO models**

   - The notebooks use Ultralytics YOLO models such as `yolov8n.pt`,`yolov8s.pt`, `yolov8m.pt`, or `yolov8l.pt`.

7. **Evaluate model performance**

   - Evaluation uses common YOLO metrics, including precision, recall, and mAP50.

8. **Generate visual review images**

   - Several notebooks draw ground-truth boxes and prediction boxes on images so model results can be manually inspected.