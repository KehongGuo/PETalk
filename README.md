# PETalk
### A Pipeline for Plosive-Enhanced Talking Head Generation

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1c6eDLz7lzaEfd4jRNdKOwggxWLWQGslC#scrollTo=wFPyLufX-RiF)
[![Python Version](https://img.shields.io/badge/Python-3.8-blue.svg)](https://www.python.org/downloads/release/python-380/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)


Welcome to PETalk, an end-to-end pipeline for generating high-quality, realistic talking head videos. The name stands for **P**losive-**E**nhanced **Talk**, highlighting its core feature: a custom audio pre-processing suite designed to create more articulate and lifelike facial animations by emphasizing hard consonant sounds.

This project leverages state-of-the-art models and integrates a custom suite of audio and video enhancements to push beyond baseline performance and achieve greater realism.

---

### Demo & Comparison

| Baseline (SadTalker) | PETalk Final Output |
| :---: | :---: |
| ![Baseline Video](link/to/your/baseline.gif) | ![Final Video](link/to/your/final_enhanced.gif) |

---

## Features

-   **Advanced Audio Pre-processing**: Implements a custom pipeline to enhance input audio for more expressive facial animations, including:
    -   Bandpass filtering to isolate the vocal range.
    -   **Plosive enhancement** to better articulate consonants.
    -   Lip-sync timing optimization for tighter synchronization.
-   **Multi-Stage Video Generation**: Sequentially combines the strengths of multiple models:
    1.  **SadTalker**: Generates the foundational head motion and stylized video.
    2.  **Wav2Lip**: Improves the accuracy and naturalness of lip movements on the generated video.
-   **Post-Processing Enhancement Suite**: Applies final polishing filters to the video for a professional finish:
    -   **Frame Interpolation**: Boosts framerate to 30 FPS for smoother motion.
    -   **Super-Resolution**: Upscales the video to 512x512 for enhanced clarity.

## The Pipeline

The system processes inputs through a carefully orchestrated series of steps to maximize output quality.

```
Input Audio & Image
         |
         V
[Audio Pre-processing Module]
- Filters, Plosive Enhancement, Sync Tuning
         |
         V
[1. SadTalker Inference] --> Generates base talking head video
         |
         V
[2. Wav2Lip Enhancement] --> Refines lip synchronization
         |
         V
[3. Post-processing Module] --> Interpolates frames & upscales resolution
         |
         V
Final High-Fidelity Video
```

## Project Structure

The repository is organized to be clean and intuitive:

```
PETalk/
├── main.py                     # The main executable script to run the entire pipeline
├── audio_processing.py         # Module containing all custom audio enhancement functions
├── requirements.txt            # A list of all Python dependencies for the project
│
├── input/                      # --> Place your source images and audio files here
│   └── (examples)
│
├── output/                     # --> All generated videos and artifacts are saved here (auto-created)
│
├── SadTalker/                  # Cloned SadTalker repository (dependency)
├── Wav2Lip/                    # Cloned Wav2Lip repository (dependency)
│
├── .gitignore                  # Specifies files for Git to ignore (e.g., models, output)
└── README.md                   # This documentation file
```

## Setup & Installation

This project requires Python 3.8 and several external dependencies. A virtual environment is highly recommended.

**1. Prerequisites:**
- Python 3.8
- Git
- FFmpeg (must be installed and available in your system's PATH)

**2. Clone Repositories:**
First, clone the PETalk repository. Then, clone the required `SadTalker` and `Wav2Lip` sub-modules into the root directory.

```bash
git clone https://github.com/[YourUsername]/PETalk.git
cd PETalk

# Clone sub-projects
git clone https://github.com/OpenTalker/SadTalker.git
git clone https://github.com/Rudrabha/Wav2Lip.git
```

**3. Setup Python Environment and Install Dependencies:**

```bash
# Create and activate a virtual environment (recommended)
python3.8 -m venv venv
source venv/bin/activate  # On Windows, use `venv\Scripts\activate`

# Install all required packages
pip install --upgrade pip
pip install -r requirements.txt```
*Note: The PyTorch version in `requirements.txt` is for CUDA 11.3. If you have a different CUDA version, please install the appropriate PyTorch build from their [official website](https://pytorch.org/get-started/locally/).*

**4. Download Pre-trained Models:**
You must download the pre-trained models for both SadTalker and Wav2Lip.

```bash
# Download SadTalker models
cd SadTalker
bash scripts/download_models.sh
cd ..

# Download Wav2Lip models
# Ensure 'Wav2Lip-SD-GAN.pt' is placed in `Wav2Lip/checkpoints/`
wget 'https://github.com/Rudrabha/Wav2Lip/releases/download/v0.1-alpha/Wav2Lip_GAN.pth' -O 'Wav2Lip/checkpoints/Wav2Lip-SD-GAN.pt'

# Wav2Lip also requires a face detection model
# Ensure the path is 'Wav2Lip/face_detection/detection/sfd/s3fd.pth'
mkdir -p Wav2Lip/face_detection/detection/sfd/
wget "https://www.adrianbulat.com/downloads/python-fan/s3fd-619a316812.pth" -O "Wav2Lip/face_detection/detection/sfd/s3fd.pth"
```

## How to Use

1.  **Place Inputs**:
    -   Put your source image (e.g., `face.png`) in the `input/` directory.
    -   Put your driving audio file (e.g., `speech.wav`) in the `input/` directory.

2.  **Run the Pipeline**:
    Execute `main.py` from the command line, specifying your input files.

    ```bash
    python main.py --source_image input/face.png --driven_audio input/speech.wav --output_dir results
    ```

### Command-Line Arguments

-   `--source_image`: (Required) Path to the source face image.
-   `--driven_audio`: (Required) Path to the driving audio file (`.wav`).
-   `--output_dir`: Directory to save the final video and intermediate files. Defaults to `./output`.
-   `--skip_audio_processing`: (Optional) Bypass the custom audio enhancement pipeline.
-   `--skip_post_processing`: (Optional) Bypass the final frame interpolation and upscaling step.
-   `--visualize_audio`: (Optional) Save a plot comparing the original and processed audio spectrograms.

## Acknowledgements

This project builds upon the fantastic work from the following open-source projects:
-   **SadTalker**: [Zhang et al., CVPR 2023](https://arxiv.org/abs/2211.12194) - [GitHub](https://github.com/OpenTalker/SadTalker)
-   **Wav2Lip**: [Prajwal et al., ACM Multimedia 2020](https://arxiv.org/abs/2008.10010) - [GitHub](https://github.com/Rudrabha/Wav2Lip)

## License
This project is licensed under the MIT License.
