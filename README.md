# Interactive Segmentation and YOLOv11-seg Training Pipeline

![Docker](https://img.shields.io/badge/Docker-enabled-blue)
![Python](https://img.shields.io/badge/Python-3.11-blue)
![Flask](https://img.shields.io/badge/Flask-3.1-orange)
![Hugging Face Spaces](https://img.shields.io/badge/Deployed-HuggingFaceSpaces-yellow)

This repository provides an application for **interactive segmentation** using **SAM2**, **automatic segmentation** using **YOLOv11-seg**, and **chip analytics** to detect voids in microchips. The project is containerized with Docker and deployed on **Hugging Face Spaces**, offering an intuitive workflow for detecting voids, retraining models, and analyzing chip quality.

---

## Features

1. **Interactive Segmentation Tool**:
   - Use **SAM2** for precise segmentation by manually refining void detection.
   - Segmented masks can be saved for retraining the YOLOv11-seg model.

2. **Automatic Segmentation Tool**:
   - Detect voids and chips automatically using **YOLOv11-seg**.
   - Ideal for batch processing and generating initial predictions.

3. **Chip Analytics**:
   - Automatically calculate:
     - **Number of Chips**: Total chips detected in the image.
     - **Percentage of Voids**: Total void area within each chip.
     - **Max Void Percentage**: Size of the largest void as a percentage of the chip area.

4. **Retrain Model**:
   - Retrain YOLOv11-seg using labeled data from segmentation results.
   - Update model weights for improved detection accuracy.

5. **Hugging Face Spaces Deployment**:
   - Deployed on Hugging Face Spaces, ensuring easy access and scalability.

---

## Use Case: Detecting Voids in Microchips

This application focuses on detecting and analyzing **voids in microchips**, which are critical in evaluating manufacturing quality. The tool helps with:

- **Void Detection**: Identify voids in microchips to assess potential defects.
- **Chip Analytics**: Quantify the severity of voids and classify chips as acceptable or defective.
- **Retraining Workflow**: Continuously improve detection accuracy using real-world datasets.

---

## Workflow

### **Application Layout**

The application consists of three main sections:

1. **Interactive Segmentation Tool**:
   - Allows user-guided segmentation using **SAM2**.
   - Provides "Voids" and "Chips" modes for precise segmentation.

2. **Automatic Segmentation Tool**:
   - Upload an image and let **YOLOv11-seg** process it for void detection.

3. **Segmentation Results**:
   - Displays metrics such as chip area, percentage of voids, and the largest void size.

---

## System Architecture

![System Architecture](SCHEMA%20ARCHITECTURE.png)

The system is built on a layered architecture, hosted in a Dockerized environment on Hugging Face Spaces:

1. **Frontend Layer**:
   - Handles user interactions such as image uploads, segmentation requests, and result visualization.

2. **Backend API Layer**:
   - A Flask-based API for processing user inputs, interacting with models, and managing segmentation workflows.

3. **Model Layer**:
   - **SAM2**: Provides interactive segmentation.
   - **YOLOv11-seg**: Handles automatic segmentation and retraining.

4. **Storage Layer**:
   - Stores images, masks, checkpoints, and retraining datasets.

---


## Project Structure

```plaintext
.
├── app.py                     # Flask application entry point
├── Dockerfile                 # Docker configuration for containerization
├── requirements.txt           # Python dependencies
├── static/
│   ├── css/
│   │   └── styles.css         # Frontend styles
│   ├── js/
│   │   └── app.js             # Frontend interactivity scripts
│   └── history/               # Stores uploaded images and generated masks
├── templates/
│   └── index.html             # Main frontend template
├── utils/
│   ├── helpers.py             # Helper functions for data processing
│   └── predictor.py           # Model loading and prediction logic
├── checkpoints/               # Pre-trained model weights
│   ├── sam2_hiera_large.pt    # SAM2 model weights
│   └── yolo11_seg.pt          # YOLOv11-seg model weights
├── dataset/                   # Training datasets for retraining YOLOv11-seg
│   ├── train/                 # Training data
│   └── val/                   # Validation data
├── runs/                      # Logs and outputs from model training/inference
└── sam2/                      # Sam2 files

```

## Getting Started

### Prerequisites

- **Docker**: [Install Docker](https://docs.docker.com/get-docker/)
- **Python 3.11**: Required for running Flask and related scripts.

### Installation

1. **Clone the Repository**

   ```bash
   git clone https://github.com/Marc-Habib/interactive-segmentation-yolov11.git
   cd interactive-segmentation-yolov11
   ```
2. **Build the Docker Image**

   Build the Docker image to set up the application environment:

   - Open your terminal in the repository's root directory.
   - Run the following command to build the image:

     ```bash
     docker build -t interactive-segmentation-yolov11 .
     ```

3. **Run the Docker Container**

   Start the application by running the Docker container:

   - Use the following command to run the container and expose the application on port `7860`:

     ```bash
     docker run -p 7860:7860 interactive-segmentation-yolov11
     ```

   - Once the container is running, access the application in your browser by navigating to:

     ```
     http://localhost:7860
     ```

   This will open the interactive segmentation application.

---

## Usage

### 1. Interactive Segmentation

   - Navigate to the **"Interactive Segmentation Tool"** section in the application.
   - Upload an image of microchips and choose between **Voids** and **Chips** modes for manual segmentation.
   - The **SAM2** model will process your input, allowing you to refine segmentation masks.
   - Save the refined masks for further processing or model retraining.

### 2. Automatic Segmentation

   - Navigate to the **"Automatic Segmentation Tool"** section.
   - Upload an image, and the **YOLOv11-seg** model will automatically detect chips and voids in the image.
   - Processed images and detected masks are displayed in real time.

### 3. Chip Analytics

   - Navigate to the **"Segmentation Results"** section.
   - After running automatic segmentation, the application computes:
     - **Number of Chips**: Total chips detected in the uploaded image.
     - **Void Percentage**: The percentage of voids detected in each chip.
     - **Max Void Percentage**: The largest void size as a percentage of the chip area.
   - Export the analytics as a CSV table for further use.

### 4. Retrain Model

   - Save segmentation outputs (masks) generated from interactive or automatic segmentation.
   - Use these outputs as labeled datasets for retraining the **YOLOv11-seg** model.
   - The retraining process updates the model weights to improve its accuracy on future segmentation tasks.

---

## Deployment on Hugging Face Spaces

This application is deployed on **Hugging Face Spaces**, leveraging their infrastructure to provide a scalable and accessible environment.

To deploy the application on Hugging Face Spaces:

1. Push the repository to your Hugging Face Spaces account.
2. Ensure the following files are included in the repository:
   - `Dockerfile`
   - `requirements.txt`
   - `runtime.txt`
3. Enable Docker build mode in the Spaces settings.
4. Deploy the application. Once deployed, it will be hosted at the provided Hugging Face Spaces URL.

---

## Contributing

Contributions are welcome! To contribute:

1. Fork this repository.
2. Create a new branch:
   ```bash
   git checkout -b feature/YourFeatureName
    ```
3. **Commit your changes**

   After making your changes, stage and commit them with a meaningful message:

   ```bash
   git add .
   git commit -m "Describe your feature or fix here"
    ```
4. **Push to the branch**

   Push your changes to your forked repository:

   ```bash
   git push origin feature/YourFeatureName
    ```
5. **Submit a Pull Request**

   - Go to your forked repository on GitHub.
   - Click the **"Compare & pull request"** button next to your newly pushed branch.
   - Provide a clear description of the changes you’ve made, including:
     - The purpose of the changes (e.g., bug fix, feature addition, or improvement).
     - Any areas of the application affected by the changes (e.g., SAM2, YOLOv11-seg, Backend API).
     - Steps to reproduce or test the changes.
   - Submit the pull request for review by the maintainers.

---

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

---

## Acknowledgements

- **Ultralytics**: For developing the YOLOv11-seg model. [Ultralytics YOLOv11](https://github.com/ultralytics/ultralytics)
- **Hugging Face**: For providing the Spaces platform for deployment. [Hugging Face Spaces](https://huggingface.co/spaces)
- **SAM2**: For enabling robust interactive segmentation workflows, forming a key part of this project. [SAM2 GitHub Repository](https://github.com/facebookresearch/segment-anything)

