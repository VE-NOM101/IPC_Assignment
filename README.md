# Person Detection Using YOLO with IPC (C & Python)

## 1. Assignment Description

This assignment implements a **person detection system** using the YOLO algorithm executed in a **C program**. The detection results (bounding box coordinates and confidence values) are shared between processes using **POSIX shared memory (IPC)**. A **Python program** accesses this shared memory and visualizes the detection results on the original image using OpenCV.

The assignment demonstrates:
- YOLO inference in C
- Inter-Process Communication using shared memory
- Data exchange between C and Python
- Separation of computation and visualization

---

## 2. System Overview

- **C Program (`yolo_ipc.c`)**  
  Performs person detection using a pretrained YOLO model and writes detection results into shared memory.

- **Python Program (`visualize.py`)**  
  Reads detection results from shared memory and visualizes them by drawing bounding boxes on the input image.

- **IPC Mechanism**  
  POSIX shared memory is used to share detection data through RAM.

---

## 3. Environment Setup

### 3.1 Windows Subsystem for Linux (WSL)

1. Open PowerShell as Administrator.
2. Install Ubuntu:
   ```bash
   wsl --install -d Ubuntu
   ```
3. Restart the system when prompted.
4. Open Ubuntu and complete the initial setup.

---

### 3.2 Install Required Packages in WSL

```bash
sudo apt update
sudo apt install build-essential python3 python3-opencv -y
```

---

### 3.3 Darknet Setup

1. Clone Darknet:

   ```bash
   git clone https://github.com/AlexeyAB/darknet.git
   cd darknet
   ```

2. Edit `Makefile` and set:

   ```
   GPU=0
   OPENCV=0
   LIBSO=1
   ```

3. Build Darknet as a shared library:

   ```bash
   make clean
   make
   ```

4. Download YOLOv4-tiny model files:

   ```bash
   wget https://github.com/AlexeyAB/darknet/releases/download/yolov4/yolov4-tiny.weights
   wget https://raw.githubusercontent.com/AlexeyAB/darknet/master/cfg/yolov4-tiny.cfg
   wget https://raw.githubusercontent.com/AlexeyAB/darknet/master/data/coco.names
   ```

---

## 4. Files in This Assignment

```
darknet/
│
├── yolo_ipc.c        # C program for YOLO person detection + IPC
├── visualize.py      # Python program for IPC reading and visualization
├── yolov4-tiny.cfg
├── yolov4-tiny.weights
├── person.jpg        # Input image
```

---

## 5. How to Run the Assignment

### Step 1: Compile the C Program

From inside the `darknet` directory:

```bash
gcc yolo_ipc.c -o yolo_ipc \
    -Iinclude \
    -L. -ldarknet \
    -lm -pthread
```

Set the library path:

```bash
export LD_LIBRARY_PATH=.
```

---

### Step 2: Run the C Program (YOLO + IPC Writer)

```bash
./yolo_ipc person.jpg
```

Expected output:

```
Detections written to shared memory. Count = X
```

---

### Step 3: Run the Python Program (IPC Reader + Visualization)

```bash
python3 visualize.py
```

Expected output:

```
Detections read from shared memory: X
Output saved as ipc_output.jpg
Shared memory cleaned
```

The output image `ipc_output.jpg` will be generated with bounding boxes around detected persons.

---

## 6. Output

- `ipc_output.jpg`  
  Image showing detected person(s) with bounding boxes and confidence values.

---

  ### Input Image
  ![Input Image](/person.jpg)

  ### Output After YOLO Detection
  ![Detected Persons](/ipc_output.jpg)

## 7. Adding Images to the Report

For documentation or report purposes, the following images can be included:

- Input image (`person.jpg`)
- Output image (`ipc_output.jpg`)
- System architecture or block diagram
- Shared memory workflow diagram

Images can be stored in a separate folder (e.g., `images/`) and referenced in the report.

---

## 8. Conclusion

This assignment successfully demonstrates person detection using YOLO in a C environment and efficient data sharing using POSIX shared memory. The separation of detection and visualization through IPC highlights a modular and effective system design.
