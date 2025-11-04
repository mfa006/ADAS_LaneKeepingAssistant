# ADAS Lane Keeping Assistant

An Advanced Driver Assistance Systems (ADAS) project that detects and visualizes lane markings in real-time video footage using computer vision techniques. The system processes video frames to identify lane boundaries and provides visual feedback with lane overlay, confidence metrics, and lateral offset calculations.

## 🎯 Features

- **Lane Detection**: Robust detection of left and right lane boundaries using advanced computer vision algorithms
- **Real-time Processing**: Efficient video frame processing with performance metrics
- **Perspective Transform**: Inverse Perspective Mapping (IPM) for accurate lane detection
- **Visual Overlay**: Green overlay highlighting detected lane areas
- **Confidence Metrics**: Real-time confidence scores for left and right lane detection
- **Lateral Offset Calculation**: Measures vehicle offset from lane center
- **CSV Export**: Detailed frame-by-frame analysis data export

## 🛠️ Technical Approach

The lane detection pipeline consists of several key stages:

1. **Preprocessing**: 
   - HLS color space conversion
   - Saturation channel thresholding
   - Sobel X edge detection on Luminance channel

2. **Region of Interest (ROI)**: 
   - Trapezoid ROI mask to focus on road area

3. **Inverse Perspective Mapping (IPM)**:
   - Perspective transform to bird's-eye view
   - Enhanced lane visibility in warped space

4. **Lane Detection**:
   - Sliding window algorithm for lane pixel detection
   - Histogram-based lane base point detection
   - Separate processing for left and right halves

5. **Polynomial Fitting**:
   - RANSAC-based polynomial regression for robust fitting
   - Confidence calculation based on detected pixels

6. **Visualization**:
   - Lane overlay with confidence-based coloring
   - HUD display with detection status and metrics
   - Warp back to original perspective

## 📦 Installation

### Requirements

```bash
pip install opencv-python numpy matplotlib scikit-learn tqdm
```

### Dependencies

- `opencv-python`: Computer vision and video processing
- `numpy`: Numerical computations
- `matplotlib`: Visualization and plotting
- `scikit-learn`: RANSAC regression for polynomial fitting
- `tqdm`: Progress bars for video processing

## 🚀 Usage

### Basic Usage

1. **Open the Jupyter Notebook**:
   ```bash
   jupyter notebook "ADAD_LKA copy.ipynb"
   ```

2. **Configure ROI Vertices**: 
   Adjust the ROI vertices based on your video characteristics:
   ```python
   roi_vertices = np.array([[
       (w*0.07, h*0.99),   # bottom-left
       (w*0.5, h*0.58),    # top-left
       (w*0.55, h*0.58),   # top-right
       (w*0.95, h*0.99)    # bottom-right
   ]], dtype=np.int32)
   ```

3. **Process Video**:
   ```python
   process_video_with_pipeline(
       input_path='data/t1_vid.mp4',
       output_path='results/t1_result.mp4',
       roi_vertices=roi_vertices,
       csv_path='results/t1_results.csv'
   )
   ```

### Parameters

- `s_thresh`: Saturation channel threshold (default: `(60, 255)`)
- `sx_thresh`: Sobel X threshold (default: `(10, 120)`)
- `nwindows`: Number of sliding windows (default: `9`)
- `margin`: Window margin for lane pixel search (default: `100`)
- `minpix`: Minimum pixels to recenter window (default: `50`)
- `line_thickness`: Thickness of detected lane lines (default: `8`)

## 📹 Video Demonstrations 

**Original Lane Detection Test Video**

![Input Video](results/t1_result.gif)

🔗 [Download MP4 version](results/t1_result.mp4)

---
 

**Lane Detection Output (Overlay Visualization)**

![Processed Video](results/t2_result.gif)

🔗 [Download MP4 version](results/t2_result.mp4)



## 📁 Project Structure

```
ADAS_LaneKeepingAssistant/
├── ADAD_LKA copy.ipynb      # Main Jupyter notebook with implementation
├── README.md                 # Project documentation
├── data/                     # Input data directory
│   ├── t1_vid.mp4           # Test video 1
│   ├── t1.png               # Test image 1
│   ├── t2_vid.mp4           # Test video 2
│   └── t3.mp4               # Test video 3
└── results/                  # Output directory
    ├── t1_result.mp4        # Processed video 1
    ├── t1_results.csv       # Frame-by-frame metrics for video 1
    ├── t2_result.mp4        # Processed video 2
    └── t2_results.csv       # Frame-by-frame metrics for video 2
```

## 📊 Output Format

The CSV output files contain the following columns:

- `frame_id`: Frame number in the video
- `left_detected`: Binary flag (1/0) indicating left lane detection
- `right_detected`: Binary flag (1/0) indicating right lane detection
- `left_conf`: Confidence score for left lane (0.0-1.0)
- `right_conf`: Confidence score for right lane (0.0-1.0)
- `lat_offset_m`: Lateral offset from lane center in meters
- `latency_ms`: Processing time per frame in milliseconds

## 🎨 Visual Features

- **Green Lane Overlay**: Highlighted lane area between detected boundaries
- **Yellow Lane Lines**: Detected left lane boundary (with confidence-based transparency)
- **Orange Lane Lines**: Detected right lane boundary (with confidence-based transparency)
- **HUD Display**: 
  - Lane detection status indicators
  - Confidence scores
  - Lateral offset from center
  - Processing latency

## 🔧 Customization

### Adjusting Detection Sensitivity

Modify threshold parameters in the `process_frame` function:

```python
binary_mask = preprocess_lane_mask(
    frame_rgb, 
    s_thresh=(60, 255),    # Saturation threshold
    sx_thresh=(10, 120)    # Sobel X threshold
)
```

### Changing ROI Region

Adjust the trapezoid vertices based on camera mounting height and angle:

```python
roi_vertices = np.array([[
    (w*0.07, h*0.99),   # bottom-left
    (w*0.5, h*0.58),    # top-left (adjust height)
    (w*0.55, h*0.58),   # top-right
    (w*0.95, h*0.99)    # bottom-right
]], dtype=np.int32)
```

## 📈 Performance

- Average processing latency: ~70-80 ms per frame  
- Frame-by-frame latency data available in CSV outputs