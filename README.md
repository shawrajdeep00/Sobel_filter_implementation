#  2D Sobel Edge Detection for Lane Detection

This project implements a 2-dimensional Sobel edge filter, specifically tailored for lane detection in images. The filter operates on 24-bit BMP images, applying a Sobel operator to detect edges based on luminance. It produces output images highlighting edges, helping in the identification of lane boundaries. This README will cover the following sections:

1. [Project Overview](#project-overview)
2. [Features](#features)
3. [Prerequisites](#prerequisites)
4. [File Structure](#file-structure)
5. [Compilation and Execution](#compilation-and-execution)
6. [Detailed Explanation of Code](#detailed-explanation-of-code)
7. [Sobel Edge Detection Algorithm](#sobel-edge-detection-algorithm)
8. [Example Usage](#example-usage)
9. [Results](#results)
10. [Troubleshooting](#troubleshooting)
11. [References](#references)

---

## Project Overview

This project processes BMP images using a Sobel filter to detect edges. Edge detection is essential in computer vision applications like lane detection, where clear lane markings are critical for navigation. The code reads an input BMP image, calculates edge intensities, and outputs a processed image.

The Sobel operator computes gradients in the x and y directions, which are combined to detect edges based on intensity changes. The luminance of each pixel is calculated using the RGB values of neighboring pixels.

---

## Features

- **Sobel Edge Detection**: Uses the Sobel filter to detect edges.
- **Floating-Point Calculations**: Ensures precision with floating-point operations.
- **Portable Code**: Compatible with most C compilers.
- **BMP Image Support**: Processes images in 24-bit BMP format.

---

## Prerequisites

- **C Compiler**: Ensure you have a C compiler installed (e.g., GCC).
- **BMP Image Handling Library**: This project includes the `bmp24_io.c` file to manage BMP files. Ensure it is in the same directory.
- **BMP Image Files**: Input files should be in 24-bit BMP format, named according to the pattern `<filename>.bmp`.

---

## File Structure

- **lane_float.c**: Main program file containing the Sobel edge detection code.
- **bmp24_io.c**: Library file for handling BMP images.
- **Input Files**: BMP images for testing (e.g., `street_A.bmp`).
- **Output Files**: Processed BMP images with edges (e.g., `street_A_edge_float.bmp`).

---

## Compilation and Execution

1. **Compilation**: Use GCC to compile the program.
    ```bash
    gcc lane_float.c -o lane_float
    ```

2. **Execution**: Run the executable with the name of the image (excluding the `.bmp` extension).
    ```bash
    ./lane_float street_A
    ```

   Replace `street_A` with the base name of your BMP file.

---

## Detailed Explanation of Code

### 1. **Including Libraries**
   ```c
   #include <stdio.h>
   #include <stdlib.h>
   #include <math.h>
   #include "bmp24_io.c"
   ```
   - `stdio.h`: Standard I/O functions.
   - `stdlib.h`: Standard library for memory management.
   - `math.h`: Provides mathematical functions (e.g., `sqrt`).
   - `bmp24_io.c`: Custom library for BMP image handling.

### 2. **Main Function**
   The `main` function starts by handling command-line arguments and printing information about the Sobel filter. It performs the following tasks:
   
   - **Argument Handling**:
     ```c
     if (argc != 2) {
         printf("USAGE: %s <input file base>\n", argv[0]);
         exit(1);
     }
     ```
     Ensures that exactly one argument is provided (the base name of the input image).

   - **Image Loading**:
     ```c
     bmp24_open(f_name, &image_in, &x_size, &y_size);
     ```
     Reads the image data into a 2D array (`image_in`).

### 3. **Looping Through Pixels**
   The nested loop iterates over every pixel in the image to calculate gradient values in the x and y directions.

   ```c
   for (y = 0; y < y_size; y++)
       for (x = 0; x < x_size; x++) {
           // Obtain neighboring pixel values for gradient calculation
       }
   ```

### 4. **Calculating Luminance and Gradient**
   - **Luminance Calculation**: Converts RGB values to grayscale.
     ```c
     lum_lt = 0.299 * bmp24_r(pixel_lt) + 0.587 * bmp24_g(pixel_lt) + 0.114 * bmp24_b(pixel_lt);
     ```
     This formula computes luminance by weighing red, green, and blue channels.

   - **Sobel Filter (Gradient Calculation)**:
     ```c
     sum_x = (lum_rt + 2.0 * lum_rc + lum_rb) - (lum_lt + 2.0 * lum_lc + lum_lb);
     sum_y = (lum_lt + 2.0 * lum_ct + lum_rt) - (lum_lb + 2.0 * lum_cb + lum_rb);
     ```
     Calculates gradients in x and y directions using neighboring luminance values.

### 5. **Edge Intensity and Output Generation**
   - **Edge Magnitude Calculation**:
     ```c
     g_square = sum_x * sum_x + sum_y * sum_y;
     g_root = sqrt(g_square);
     g_int = g_root / 2;
     ```
     Calculates the edge intensity and clamps it to a maximum value of 255.

   - **Output Pixel Assignment**:
     ```c
     bmp24_put(image_out, lum_new, lum_new, lum_new, x, y, x_size, y_size);
     ```

---

## Sobel Edge Detection Algorithm

The Sobel operator calculates the gradient magnitude, which represents the rate of intensity change in the image. A high gradient magnitude indicates a potential edge. The Sobel filter uses two kernels (one for the x-direction and one for the y-direction) to achieve this.

The Sobel kernels used are as follows:
   ```
   Gx = [[-1 0 +1]
         [-2 0 +2]
         [-1 0 +1]]
   
   Gy = [[+1 +2 +1]
         [ 0  0  0]
         [-1 -2 -1]]
   ```
The algorithm combines `Gx` and `Gy` to calculate the gradient magnitude.

---

## Example Usage

For an image file named `street_A.bmp`:

1. **Compilation**:
    ```bash
    gcc lane_float.c -o lane_float
    ```

2. **Execution**:
    ```bash
    ./lane_float street_A
    ```

This will read `street_A.bmp`, process it, and save the result as `street_A_edge_float.bmp`.

---

## Results

After running the program, you should see processed BMP images in the directory with the edges highlighted.

### Example PowerShell Output:
```plaintext
PS C:\Users\Rajdeep Shah\Downloads\FPGA-Vision-master\C-Files> gcc lane_float.c
PS C:\Users\Rajdeep Shah\Downloads\FPGA-Vision-master\C-Files> ./a.exe street_A
Sobel-Filter
============
Reading file street_A.bmp (1280x720 pixels)
Writing file street_A_edge_float.bmp (1280x720 pixels)
OK ...
```

---

## Troubleshooting

- **Error: "Cannot open BMP file"**:
  Ensure that the image file is in the correct directory and named properly (e.g., `street_A.bmp`).
  
- **Incorrect Output Image**:
  Check that your input BMP file is 24-bit and that all required files (`bmp24_io.c`, `lane_float.c`) are in the same directory.

---

## References

- **Sobel Operator**: [Wikipedia - Sobel Operator](https://en.wikipedia.org/wiki/Sobel_operator)
- **BMP Image Format**: [Wikipedia - BMP File Format](https://en.wikipedia.org/wiki/BMP_file_format)

---
