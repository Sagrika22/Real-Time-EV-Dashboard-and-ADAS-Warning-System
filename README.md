# Real-Time-EV-Dashboard-and-ADAS-Warning-System

A Hardware-in-the-Loop (HIL) co-simulation platform integrating an **STM32 Microcontroller Firmware** data pipeline with a live **Python Telemetry Dashboard** and an **Advanced Driver Assistance System (ADAS)** warning alert system. 

The system processes real-time sensor metrics (such as vehicle velocity, battery thermal states, state of charge (SoC), and ultrasonic distance values) on the STM32 layer, packing the payload onto a serial stream to drive a responsive, multi-axis tracking dashboard interface.

---

## 🎛️ STM32 Pinout & Hardware Configuration (.ioc)

<img width="215" height="194" alt="image" src="https://github.com/user-attachments/assets/8e26e512-1c7a-47fb-8004-d98273daafb2" />


---

## 🏗️ System Architecture & Data Flow

<img width="1440" height="1080" alt="image" src="https://github.com/user-attachments/assets/84502d2c-3dad-4ae3-b175-81b14c219ad6" />


```text
[ STM32 Microcontroller ] (PicSimLab Board Layout)
           │
           ▼ (Generates telemetry data: Speed, Battery, SoC, ADAS Alerts)
   [ Virtual COM Port 4 ] (Simulated UART Transmit)
           │
     ============= VSPE (Virtual Serial Ports Emulator Bridge) =============
           │
   [ Virtual COM Port 1 ] (Simulated UART Receive)
           │
           ▼ (Reads serial bytes stream continuously)
[ Python Telemetry Engine ] (NumPy parsing data frames)
           │
           ▼ (Appends real-time data queues)
[ Interactive UX/UI Layer ] (Matplotlib dynamic rendering engine)
```

---

## 📁 Repository Blueprint

* 📄 `dashboard_code.py` — Multi-threaded Python telemetry parsing engine & interactive Matplotlib interface.
* 📦 `evdash_project.zip` — Complete STM32CubeIDE source workspace containing peripheral initializations (`Core/`, `Drivers/`, and `.ioc` configuration).
* 📄 `requirements.txt` — Frozen dependency map detailing specific framework package requirements (`numpy`, `matplotlib`, `pyserial`).
* 📄 `simulation_layout.pcf` — Serial-mapped PicSimLab workspace blueprint configuring target peripherals, knobs, and display arrays.
* 🎥 `demo_video.mp4` — End-to-end continuous validation video of active dashboard plotting matching hardware stimulus.

---

## ⚙️ Engineering Environment Setup

### 1. Virtual Serial Interconnect (VSPE Setup)
1. Download and run **VSPE** (Virtual Serial Ports Emulator).
2. Click `Device > Create` and select the **Pair** device type.
3. Establish a virtual bridge matching your runtime script by linking **`COM4`** directly to **`COM1`**.

### 2. Hardware Emulator Setup (PicSimLab Setup)
1. Open **PicSimLab**.
2. Click `File > Load Configuration` and upload `simulation_layout.pcf` from this repository root.
3. Unzip `evdash_project.zip` and extract the target compilation output binary (`.bin`).
4. Click `File > Load bin` inside PicSimLab and select your compiled binary file.
5. In the main PicSimLab menu, route the serial communication interface out through **`COM4`**.

---

## 🚀 Launch Sequence (Python Dashboard Initialization)

Ensure your Python execution path matches your physical workspace before beginning installation.

### 1. Install System Dependencies
Execute the command terminal inside your project root folder to install packages natively using the requirements manifest:
```bash
pip install -r requirements.txt
```

### 2. Launch the Application
Confirm your dashboard runtime script references **`COM1`** to interface with the serial emulated stream, then execute:
```bash
python dashboard_code.py
```

---

## 🛠️ Key Technical Features Evaluated

* **Dynamic Data Deserialization**: Low-latency byte-stream stripping via `pyserial` matched with scalable array transformations via `numpy`.
* **Hardware Alert System Integration**: Real-time evaluation of proximity vectors triggering immediate priority telemetry packet interrupts mimicking collision warnings.
* **Low-Latency Rendering**: Continuous interface buffer cycling in `matplotlib` guaranteeing fluid refresh intervals optimized for simulated EV operations.
