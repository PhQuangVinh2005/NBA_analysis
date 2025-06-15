
---

# 🏀 Basketball Video Analysis

This project leverages multiple AI models to analyze basketball game footage, extracting rich insights such as player tracking, team possession, pass/interception counts, player speed, and a tactical top-down view of the court.

By combining state-of-the-art object detection (YOLO), zero-shot classification, and a custom-trained keypoint detector, this tool transforms a standard game video into a comprehensive analytical report.


https://github.com/user-attachments/assets/42f8715d-bfe1-4890-8b9f-00113fc02f82



## ✨ Key Features

-   **Player & Ball Detection**: Utilizes a fine-tuned YOLO model to accurately detect players and the basketball in every frame.
-   **Team Assignment**: Employs a zero-shot image classifier to assign players to teams based on their jersey color, without needing a custom-trained model for every new team.
-   **Possession Tracking**: Calculates ball acquisition percentages for each team by identifying which player has possession of the ball.
-   **Event Detection**: Automatically identifies and counts key game events like passes and interceptions.
-   **Court Mapping**: A custom keypoint detector identifies court landmarks, enabling a perspective transformation from the camera view to a 2D tactical map.
-   **Advanced Metrics**: Translates pixel movements into real-world metrics, calculating player speed (in m/s) and total distance covered (in meters).

## 🛠️ Technologies Used

-   **Python**
-   **OpenCV** for video processing
-   **YOLO** for object detection
-   **Hugging Face Transformers** for zero-shot classification
-   **Docker** for containerization and easy deployment

## 🏰 Project Structure

The project is organized into modular components, each responsible for a specific part of the analysis pipeline.

```
.
├── main.py                     # Main script to orchestrate the pipeline
├── configs/                    # Configuration files for model paths, etc.
├── trackers/                   # Player and Ball tracking logic
├── team_assigner/              # Player-to-team assignment using jersey color
├── court_keypoint_detector/    # Court line and keypoint detection
├── ball_aquisition/            # Logic to determine ball possession
├── pass_and_interception_detector/ # Detects passes and interceptions
├── drawers/                    # Functions to draw overlays on video frames
├── utils/                      # Helper functions for video, stubs, and geometry
├── videos/                     # Input videos
├── output_videos/              # Processed output videos
└── Dockerfile                  # Docker configuration
```

-   `main.py`: Orchestrates the entire pipeline: reading video frames, running detection/tracking, team assignment, drawing results, and saving the output video.
-   `trackers/`: Houses `PlayerTracker` and `BallTracker`, which use detection models to generate bounding boxes and track objects across frames.
-   `utils/`: Contains helper functions like `bbox_utils.py` for geometric calculations, `stubs_utils.py` for reading and saving intermediate results, and `video_utils.py` for reading/saving videos.
-   `drawers/`: Contains classes that overlay bounding boxes, court lines, passes, etc., onto frames.
-   `ball_aquisition/`: Logic for identifying which player is in possession of the ball.
-   `pass_and_interception_detector/`: Identifies passing events and interceptions.
-   `court_keypoint_detector/`: Detects lines and keypoints on the court using the specified model.
-   `team_assigner/`: Uses zero-shot classification to assign players to teams based on jersey color.
-   `configs/`: Holds default paths for models, stubs, and output video.

## 🚀 Getting Started

### Prerequisites

-   Python 3.8+
-   Docker (for containerized approach)
-   Git

### Installation

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/PhQuangVinh2005/NBA_analysis.git
    cd NBA_analysis
    ```

2.  **Install Python dependencies:**
    ```bash
    pip install -r requirements.txt
    ```

3.  **Train Models:**
    Using Roboflow and Ultralytics applications:
    - Create a Roboflow account get key-api
    - Replace key-api from training notebooks
    - Train on cloud or on your computer
    - Put the models in the 'model' folder


## ⚙️ Usage

You can run the analysis pipeline either directly with Python or using the provided Docker container.

### 1) Using Python Directly

Run the main entry point with your chosen video file.

```bash
python main.py path_to_input_video.mp4 --output_video output_videos/output_result.avi
```

-   **Stub Caching**: By default, intermediate “stubs” (pickled detection results) are used if found, allowing you to skip repeated detection/tracking on subsequent runs. This significantly speeds up development and testing.
-   **Custom Stub Path**: Use the `--stub_path` flag to specify a custom folder for stubs.
-   **Disable Stubs**: To run the full pipeline from scratch, use `--stub_path=None`.

### 2) Using Docker

This method is recommended for ensuring a consistent and isolated environment.

1.  **Build the Docker container (if not already built):**
    ```bash
    docker build -t basketball-analysis .
    ```

2.  **Run the analysis inside the container:**
    This command mounts your local `videos` and `output_videos` directories into the container, allowing it to access the input video and save the result back to your host machine.

    ```bash
    docker run --rm \
      -v $(pwd)/videos:/app/videos \
      -v $(pwd)/output_videos:/app/output_videos \
      basketball-analysis \
      python main.py videos/input_video.mp4 --output_video output_videos/output_result.avi
    ```

## 🔮 Future Work

As we continue to enhance the capabilities of this tool, several areas for future development have been identified:

1.  **Integrate a Pose Model for Advanced Rule Detection**  
    Incorporating a pose detection model (e.g., OpenPose, MediaPipe Pose) could enable the identification of complex basketball rules such as **double dribbling** and **traveling**. By analyzing player skeletons and movements, the system could automatically flag these infractions, adding another layer of analysis.

2.  **Shot Detection and Classification**  
    Develop a module to detect when a player attempts a shot and classify it (e.g., 2-pointer, 3-pointer, free throw) and its outcome (made or missed).

3.  **Automated Highlight Generation**  
    Create a system that automatically clips exciting moments from the game—such as successful 3-pointers, dunks, or interceptions—into a highlight reel.

These enhancements will further refine the analysis capabilities and provide users with more comprehensive insights into basketball games.

---
