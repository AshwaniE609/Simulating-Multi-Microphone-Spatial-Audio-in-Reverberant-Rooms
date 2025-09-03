# Spatial Audio Simulation for Wearable Devices

This project was developed as part of undergraduate research at **IIT Kanpur** to create a high-fidelity simulation platform for generating training and validation datasets for next-generation wearable audio devices like smart glasses.

---

## Table of Contents
- [Project Overview](#project-overview)
- [Key Features](#key-features)
- [Technical Stack](#technical-stack)
- [Repository Structure](#repository-structure)
- [Setup and Installation](#setup-and-installation)
- [How to Use the Simulation](#how-to-use-the-simulation)
  - [Configuration](#configuration)
  - [Running the Script](#running-the-script)
  - [Understanding the Output](#understanding-the-output)
- [Acknowledgments](#acknowledgments)

---

## Project Overview

This repository contains a **MATLAB-based simulation pipeline** designed to generate realistic, multi-channel spatial audio data.  
The primary application is to create a **large-scale dataset** for the research and development of audio processing algorithms such as:
- Speech enhancement  
- Noise cancellation  
- Source localization  

The simulation programmatically models a **3D acoustic environment**, generates **Room Impulse Responses (RIRs)** using the image-source method, and convolves them with clean audio signals to produce reverberant audio as it would be captured by a **microphone array on a wearable device**.

---

## Key Features

- **Hardware Emulation**: Precise configuration of a virtual multi-microphone array to emulate device specifications.  
- **Advanced Acoustic Modeling**: Utilizes MATLAB's `imagesim` and `roomimpres` to generate realistic RIRs, capturing reflections and reverberations.  
- **Dynamic Scenario Generation**: Automates spatial scenarios by varying bystander source positions across radii and azimuths.  
- **Signal Processing Pipeline**: Includes convolution, mixing, gain compensation, and real-world noise addition.  
- **Scalable Dataset Creation**: Capable of generating tens of hours of labeled audio for deep learning.  

---

## Technical Stack

- **Environment**: MATLAB (R2020a or later recommended)  
- **Required MATLAB Toolboxes**:
  - Array Toolbox (for `imagesim`, `roomimpres`)  
  - Audio Toolbox (for audio I/O and resampling)  

**Input Datasets**:
- **Clean Speech**: `.flac` or `.wav` files (e.g., LibriSpeech dataset)  
- **Ambient Noise**: Environmental recordings (e.g., FSDnoisy18k dataset)  

---

.
├── main_simulation.m # Main MATLAB script to run the simulation
├── input_audio/ # Place clean wearer/bystander audio here
│ ├── wearer_audio.flac
│ └── bystander_audio.flac
├── noise_dataset/ # Place your noise files (.wav) here
│ ├── noise1.wav
│ └── ...
└── README.md # Documentation

---

## Setup and Installation

1. **Clone the Repository**
   ```bash
   git clone <your-repository-url>
   cd <your-repository-name>
Install MATLAB
Ensure MATLAB is installed with Array Toolbox and Audio Toolbox.
Download Input Data
Place clean speech files for "wearer" and "bystander" into input_audio/
Place environmental noise files into noise_dataset/


How to Use the Simulation
1. Configuration
Before running, open main_simulation.m and configure parameters at the top:

  %% Microphone and Room Setup
  mic1pos = [0.05; 0; 0];
  mic2pos = [-0.05; 0; 0];
  mic3pos = [0.08; 0.45; 0.04];
  mic4pos = [-0.08; 0.45; 0.04];
  mics = [mic1pos, mic2pos, mic3pos, mic4pos];
  
  room_size = [5, 4, 6]; % [Length, Width, Height] in meters
  
  %% Source Positions
  radii = [1, 2, 3];         % Radii for bystander movement
  phi_angles = 0:20:360;     % Azimuth angles in degrees
  
  %% Audio File Paths
  [wearer_audio, fs_wearer] = audioread('input_audio/1040-133433-0025.flac');
  [bystander_audio, fs_bystander] = audioread('input_audio/1040-133433-0009.flac');
  
  noise_folder = 'noise_dataset';   % Path to noise files
  
  %% Output Folder
  output_folder = 'final_noisy_outputs';

  
👉 Update file paths to match your dataset.
2. Running the Script
Open MATLAB
Navigate to the repository directory
Open main_simulation.m
Run in the MATLAB Command Window:
main_simulation
The script processes all combinations of radii and angles, printing progress in the Command Window.
3. Understanding the Output
The script creates an output folder (default: final_noisy_outputs/) with:
4 .wav files → one for each microphone channel
Format:
Audio10_output_r<radius>_phi<angle>_theta0_mic<mic_index>.wav
Example:
Audio10_output_r2_phi100_theta0_mic3.wav
1 .mat file → contains a 4-channel matrix of final mixed/noisy audio
Format:
Audio10_matrix_r<radius>_phi<angle>_theta0.mat
Example:
Audio10_matrix_r2_phi100_theta0.mat
This structured output supports direct loading into analysis scripts or ML pipelines.
Acknowledgments
This work was conducted as part of the Undergraduate Project (UGP) at the Department of Electrical Engineering, IIT Kanpur, under the supervision of Prof. Rajesh M. Hegde.

Would you like me to **add badges** (MATLAB version, License, Status) and an optional **Demo Audio Samples** section so your repo looks polished like a professional open-source project?
