# Simulating-Multi-Microphone-Spatial-Audio-in-Reverberant-Rooms

Spatial Audio Simulation for Wearable Devices
This project was developed as part of undergraduate research at IIT Kanpur to create a high-fidelity simulation platform for generating training and validation datasets for next-generation wearable audio devices like smart glasses.

Table of Contents
Project Overview

Key Features

Technical Stack

Repository Structure

Setup and Installation

How to Use the Simulation

Configuration

Running the Script

Understanding the Output

Acknowledgments

Project Overview
This repository contains a comprehensive MATLAB-based simulation pipeline designed to generate realistic, multi-channel spatial audio data. The primary application is to create a large-scale dataset for the research and development of audio processing algorithms (e.g., speech enhancement, noise cancellation, source localization) for multi-microphone wearable devices.

The simulation programmatically models a 3D acoustic environment, generates Room Impulse Responses (RIRs) using the image-source method, and convolves them with clean audio signals to produce reverberant audio as it would be captured by a microphone array on a wearable device.

Key Features
Hardware Emulation: The script allows for the precise configuration of a virtual multi-microphone array to emulate the physical hardware specifications of a device.

Advanced Acoustic Modeling: Utilizes MATLAB's Array Toolbox (imagesim, roomimpres) to generate realistic RIRs, capturing sound reflections and reverberations within a defined room.

Dynamic Scenario Generation: Automates the creation of numerous spatial scenarios by systematically varying the position of a bystander (interfering) audio source across different radii and azimuth angles.

Signal Processing Pipeline: Implements a full signal processing chain, including 1D convolution, signal mixing, gain compensation, and the addition of real-world ambient noise for enhanced realism.

Scalable Dataset Creation: Designed to be highly scalable, capable of generating tens of hours of labeled audio data suitable for training deep learning models.

Technical Stack
Environment: MATLAB (R2020a or later recommended)

Required MATLAB Toolboxes:

Array Toolbox: Essential for imagesim and roomimpres functions.

Audio Toolbox: For handling audio file I/O and resampling.

Input Datasets:

Clean Speech: Any .flac or .wav files (e.g., from the LibriSpeech dataset).

Ambient Noise: A collection of environmental noise recordings (e.g., from the FSDnoisy18k dataset).

Repository Structure
.
├── main_simulation.m        # The main MATLAB script to run the simulation
├── input_audio/             # FOLDER: Place clean wearer/bystander audio here
│   ├── wearer_audio.flac
│   └── bystander_audio.flac
├── noise_dataset/           # FOLDER: Place your noise files (.wav) here
│   ├── noise1.wav
│   └── ...
└── README.md                # This file

Setup and Installation
Clone the Repository:

git clone <your-repository-url>
cd <your-repository-name>

Install MATLAB: Ensure you have a working installation of MATLAB with the Array Toolbox and Audio Toolbox.

Download Input Data:

Place your clean speech files for the "wearer" and "bystander" into the input_audio/ directory.

Download and place your environmental noise files into the noise_dataset/ directory.

How to Use the Simulation
1. Configuration

Before running the script, open main_simulation.m in MATLAB and configure the key parameters at the top of the file:

% --- CORE CONFIGURATION PARAMETERS ---

%% Microphone and Room Setup
mic1pos = [0.05; 0; 0];
mic2pos = [-0.05; 0; 0];
mic3pos = [0.08; 0.45; 0.04];
mic4pos = [-0.08; 0.45; 0.04];
mics = [mic1pos, mic2pos, mic3pos, mic4pos];

room_size = [5, 4, 6]; % Room dimensions [Length, Width, Height] in meters

%% Source Positions
radii = [1, 2, 3]; % Radii for bystander movement in meters
phi_angles = 0:20:360; % Azimuth angles for bystander in degrees

%% Audio File Paths
% Update these paths to point to your audio files
[wearer_audio, fs_wearer] = audioread('input_audio/1040-133433-0025.flac');
[bystander_audio, fs_bystander] = audioread('input_audio/1040-133433-0009.flac');

% Update this path to your noise folder
noise_folder = 'noise_dataset';

%% Output Folder
output_folder = 'final_noisy_outputs'; % Generated files will be saved here

Make sure to update the file paths for the audio and the noise_folder to match your local setup.

2. Running the Script

Open MATLAB.

Navigate to the cloned repository's directory.

Open the main_simulation.m file.

Click the "Run" button in the MATLAB editor, or type the following in the MATLAB Command Window:

main_simulation

The script will start processing all combinations of radii and angles, printing its progress to the Command Window.

Understanding the Output
The script will create an output_folder (default: final_noisy_outputs) in the root directory. For each spatial configuration, it generates:

4 .wav files: One for each microphone channel.

Naming Convention: Audio10_output_r<radius>_phi<angle>_theta0_mic<mic_index>.wav

Example: Audio10_output_r2_phi100_theta0_mic3.wav

1 .mat file: A MATLAB data file containing a 4-channel matrix of the final mixed and noisy audio.

Naming Convention: Audio10_matrix_r<radius>_phi<angle>_theta0.mat

Example: Audio10_matrix_r2_phi100_theta0.mat

This structured output makes it easy to load specific scenarios for analysis or to build a data loader for a machine learning pipeline.

Acknowledgments
This work was conducted as part of the Undergraduate Project (UGP) at the Department of Electrical Engineering, IIT Kanpur, under the supervision of Prof. Rajesh M. Hegde.
