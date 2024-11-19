# AV1 Encoder Options Documentation

## General Encoding Options

### **b**
- **Description**: Set bitrate target in bits/second.  
  - **Variable Bitrate Mode**: Default behavior.
  - **Constant Bitrate Mode**: Set `maxrate` and `minrate` to the same value.
  - **Constrained Quality Mode**: Set `crf` along with `maxrate` and `minrate`.

### g \<value>
- **Description**: Set the maximum distance between key frames.  
  - **GOP Size**: Maximum distance between key frames.  
    - `0`: Intra-only stream. 
    - `9999`: Max support value.
  - **Key Frames**: Fixed interval if minimum distance equals GOP size.  
  - **Default**: Library determines key frame placement freely.
  - **Tip**: Anything up to 10 seconds is considered reasonable for most content, so for 30 frames per second content one would use -g 300, for 60 fps content -g 600, etc.

### keyint_min \<value>
- **Description**: Set the minimum of frames between I-Frames. If it is the same as -g, than the value will be forced.


### **qmin qmax**
- **Description**: Set minimum/maximum quantization values.  
  - **Valid Range**: `0-63` (divide by 4 for AV1 quantizers).  
  - **Default**: No constraints.

### **minrate maxrate bufsize rc_init_occupancy**
- **Description**: Set rate control buffering parameters.  
  - **Default**: Unconstrained variable bitrate.

### **threads**
- **Description**: Set the number of encoding threads.  
  - **Dependencies**: `tiles` or `row-mt` options may be required.  
  - **Default**: Matches the number of host machine hardware threads.

### **profile**
- **Description**: Set encoding profile.  
  - **Default**: Matches input bit depth and chroma subsampling.

---

## Wrapper-Specific Options

### **cpu-used**
- **Description**: Set quality/speed tradeoff.  
  - **Range**: `0-8` (higher = faster, lower quality).  
  - **Default**: `1` (slow, high quality).

### **auto-alt-ref**
- **Description**: Enable alternate reference frames.  
  - **Default**: Library-defined behavior.

### **arnr-max-frames (frames)**
- **Description**: Set max frames for altref noise reduction.  
  - **Default**: `-1`.

### **arnr-strength (strength)**
- **Description**: Set altref noise reduction filter strength.  
  - **Range**: `-1 to 6`.  
  - **Default**: `-1`.

### **aq-mode**
- **Description**: Set adaptive quantization mode.  
  - **Values**:
    - `none (0)`: Disabled.
    - `variance (1)`: Variance-based.
    - `complexity (2)`: Complexity-based.
    - `cyclic (3)`: Cyclic refresh.

### **tune**
- **Description**: Set distortion metric for tuning.  
  - **Default**: `psnr`.  
  - **Values**:  
    - `psnr (0)`
    - `ssim (1)`

### **lag-in-frames**
- **Description**: Set max frames for lookahead.  
  - **Default**: Library-defined value.

### **error-resilience**
- **Description**: Enable error resilience features.  
  - **Modes**:
    - `default`: Improves resilience against frame loss.  
  - **Default**: Disabled.

### **crf**
- **Description**: Set quality/size tradeoff for constant-quality modes.  
  - **Range**: `0-63` (higher = lower quality).  
  - **Default**: Only bitrate target is used.

---

## Advanced Options

### **static-thresh**
- **Description**: Set block change threshold for skipping.  
  - **Default**: `0` (no skipping).

### **drop-threshold**
- **Description**: Set frame drop threshold (% of buffer).  
  - **Default**: `0` (no dropping).

### **denoise-noise-level**
- **Description**: Set noise level for grain synthesis.  
  - **Default**: `0` (disabled).

### **denoise-block-size**
- **Description**: Set block size for denoising.  
  - **Default**: `32`.

---

## Additional Features

- **Enable/Disable Options**:
  - **CDEF**: `enable-cdef` (Default: True).
  - **Loop Restoration**: `enable-restoration` (Default: True).
  - **Global Motion**: `enable-global-motion` (Default: True).
  - **Intra Block Copy**: `enable-intrabc` (Default: True).
  - **Rectangular Partitions**: `enable-rect-partitions` (Default: True).
  - **1:4/4:1 Partitions**: `enable-1to4-partitions` (Default: True).
  - **Chroma Prediction**: `enable-cfl-intra` (Default: True).
