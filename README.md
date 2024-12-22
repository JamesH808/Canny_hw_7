# README: Canny Edge Detector Simulation in SystemC

## Project Overview

This project implements a **Canny edge detector simulation** using **SystemC**, designed to process image frames in a pipelined manner with realistic timing delays. The project focuses on efficient simulation of hardware-oriented image processing workflows and supports both single and multiple frame processing.


---

## Key Features

- **Canny Edge Detection Pipeline**:
  - Implements robust edge-detection logic.
  - Designed to simulate hardware pipelines for multiple image frames.

- **Timing Behavior**:
  - Module timing delays are empirically derived and scaled based on image size.
  - Realistic simulation of frame processing times.

- **SystemC FIFOs**:
  - Uses FIFOs for inter-module communication to mimic real hardware pipelines.

- **PGM Image Format**:
  - Supports input and output in PGM format for straightforward handling of grayscale images.

- **RISC-V Compatibility**:
  - Verified to run on a server's RISC-V modeled processor.

---

## Project Structure

```
Platform platform
|------ Stimulus
|------ DataIn din
|------ DUT canny
| |------ Gaussian_Smooth gaussian_smooth
| | |------ Gaussian_Kernel gauss
| | |------ BlurX blurX
| | \------ BlurY blurY
| |------ Derivative_X_Y derivative_x_y
| |------ Magnitude_X_Y magnitude_x_y
| |------ Non_Max_Supp non_max_supp
| \------ Apply_Hysteresis apply_hysteresis
\------ DataOut dout
|------ Monitor

```

---


### Key Functions:

The Canny edge detector simulation is built around three interconnected modules: **Stimulus**, **DUT (Device Under Test)**, and **Monitor**. Each module plays a specific role in the simulation, ensuring a smooth flow of frames and accurate verification of results.

### Stimulus Module

- **Purpose**: Acts as the source of image frames for the simulation.
- **Functionality**:
  - Reads input image frames in PGM format and prepares them for processing.
  - Sends image frames through **FIFO channels** to the **DUT module** for processing.
  - Connects directly to the **Monitor module** for verifying processed images and saving results.
- **Output**:
  - Frames are sent to both the **DUT** for image processing and the **Monitor** for validation.

---

### DUT Module (Device Under Test)

- **Purpose**: Handles the core image processing logic.
- **Functionality**:
  - Implements the **Canny edge detection** algorithm to process frames received from the **Stimulus module**.
  - Introduces realistic timing delays to simulate hardware behavior.
- **Output**:
  - Processed frames are passed back to the **Monitor module** for verification and storage.

---

### Monitor Module

- **Purpose**: Verifies processed frames and saves the results.
- **Functionality**:
  - Receives processed frames from the **DUT module** and compares them against expected output.
  - Saves the verified frames in PGM format for further analysis.
  - Logs timestamps and validation results to track processing performance.
- **Output**:
  - Verified and timestamped image frames saved in the `data/output_frames/` directory.

---

### Communication Between Modules

- **FIFO Channels**:
  - **Image Data FIFO**: Transfers frames between modules.
  - **Timestamp FIFO**: Synchronizes processing steps and logs timing information.

This modular design ensures a clear separation of responsibilities and facilitates realistic simulation of a hardware pipeline.


## Building the Project

### Prerequisites

- **SystemC Library**: Ensure SystemC is installed and correctly set up.
- **RISC-V Toolchain**: Required for testing on a RISC-V modeled processor.
- **GCC/Clang**: A compiler with support for C++11 or later.

### Build Steps

1. Clone the repository:
   ```bash
   git clone https://github.com/your-repo/CannyEdgeSimulation.git
   cd CannyEdgeSimulation
   ```

2. Compile the project:
   ```bash
   make
   ```

3. Run the simulation:
   ```bash
   ./canny_simulator
   ```

---

## Running the Simulation

### Input Files

- Place PGM-format images in the `data/` directory.
- Modify the `IMG_IN` macro in `stimulus.cpp` to specify the input file pattern.

### Output Files

- Processed frames are saved in the `data/output_frames/` directory.
- Logs include frame timestamps and processing details.

### Simulation Example

The included sample image demonstrates single-frame processing. To test with multiple frames, provide a sequence of input images.

Results for finding bottleneck:
Gaussian smooth:
        -gaussian kernel: <.1%
        -Blurx: 12.46%
        -Blury: 25.74%
derivx_y: 13.45%
magx_y: 4.57%
non_max: 26.40%
apply_hyst: 17.33%

The Gaussian smooth module takes up the most percentage of time for image processing.

---

## Future Improvements

- **Dynamic Timing**:
  - Replace hard-coded timing delays with a runtime configuration.

- **Improved I/O Support**:
  - Extend support to additional image formats (e.g., PNG, BMP).

- **Hardware Deployment**:
  - Validate the simulation on physical RISC-V hardware for real-world applications.

---

## Author

James Holt

---


## Acknowledgments

- **SystemC Community** for the development framework.
- **RISC-V Ecosystem** for providing a platform for testing and validation.
- Proffessor Emad Arasteh
