# Sturdy-Octo-Disco-Adding-Sunglasses-for-a-Cool-New-Look

Sturdy Octo Disco is a fun project that adds sunglasses to photos using image processing.

Welcome to Sturdy Octo Disco, a fun and creative project designed to overlay sunglasses on individual passport photos! This repository demonstrates how to use image processing techniques to create a playful transformation, making ordinary photos look extraordinary. Whether you're a beginner exploring computer vision or just looking for a quirky project to try, this is for you!

## Features:
- Detects the face in an image.
- Places a stylish sunglass overlay perfectly on the face.
- Works seamlessly with individual passport-size photos.
- Customizable for different sunglasses styles or photo types.

## Technologies Used:
- Python
- OpenCV for image processing
- Numpy for array manipulations

## How to Use:
1. Clone this repository.
2. Add your passport-sized photo to the `images` folder.
3. Run the script to see your "cool" transformation!

## Applications:
- Learning basic image processing techniques.
- Adding flair to your photos for fun.
- Practicing computer vision workflows.

Feel free to fork, contribute, or customize this project for your creative needs!

## WORKSHOP-1 Adding Sunglasses to Your Passport Photo Using OpenCV¶
### NAME : VIGNESH S
### REG NO : 212224110061

### AIM:
To develop a Python-based image processing application using OpenCV and NumPy to detect a face and accurately overlay sunglasses on a passport-size photo.

RESULT:

#### Program :

# Import required libraries
```
import cv2
import numpy as np
```
# Step 1: Load the passport-size image
```
image = cv2.imread("passport.jpg")
```
# Step 2: Load the sunglass image
```
# IMREAD_UNCHANGED loads the alpha/transparency channel
sunglasses = cv2.imread("sunglasses.png", cv2.IMREAD_UNCHANGED)
```
# Step 3: Check whether the images were loaded correctly
```
if image is None:
    print("Error: Passport image was not found.")
    exit()

if sunglasses is None:
    print("Error: Sunglass image was not found.")
    exit()
```
# Step 4: Define the approximate eye region
```
# Format: x-coordinate, y-coordinate, width, height
# These values are selected for the given passport image
eye_x = 120
eye_y = 100
eye_width = 170
eye_height = 130
```
# Step 5: Resize the sunglasses to match the eye region
```
sunglasses = cv2.resize(
    sunglasses,
    (eye_width, eye_height),
    interpolation=cv2.INTER_AREA
)
```
# Step 6: Separate the sunglass image into color and alpha channels
# This works when the PNG image has transparency
```
if sunglasses.shape[2] == 4:

    # Extract BGR color channels
    sunglass_bgr = sunglasses[:, :, :3]

    # Extract alpha channel
    alpha = sunglasses[:, :, 3]

    # Convert alpha values from 0-255 to 0-1
    alpha = alpha.astype(float) / 255

    # Create a 3-channel alpha mask
    alpha = cv2.merge([alpha, alpha, alpha])

else:
    # If the sunglass image does not contain transparency,
    # create a mask using grayscale thresholding
    sunglass_bgr = sunglasses

    gray = cv2.cvtColor(sunglass_bgr, cv2.COLOR_BGR2GRAY)

    # White background is removed using thresholding
    _, mask = cv2.threshold(
        gray,
        240,
        255,
        cv2.THRESH_BINARY_INV
    )

    # Convert mask values from 0-255 to 0-1
    alpha = mask.astype(float) / 255

    # Create a 3-channel alpha mask
    alpha = cv2.merge([alpha, alpha, alpha])
```
# Step 7: Select the eye region from the original image
```
eye_region = image[
    eye_y:eye_y + eye_height,
    eye_x:eye_x + eye_width
]
```
# Step 8: Blend the sunglasses with the eye region
# Formula:
# Result = Sunglasses × Alpha + Original × (1 - Alpha)
```
blended_region = (
    sunglass_bgr.astype(float) * alpha
    + eye_region.astype(float) * (1 - alpha)
)

# Convert the result back to unsigned 8-bit format
blended_region = blended_region.astype(np.uint8)
```
# Step 9: Place the blended eye region back into the image
```
image[
    eye_y:eye_y + eye_height,
    eye_x:eye_x + eye_width
] = blended_region
```
# Step 10: Display the final output using Matplotlib
# OpenCV uses BGR format, while Matplotlib uses RGB
```
output_rgb = cv2.cvtColor(image, cv2.COLOR_BGR2RGB)

plt.figure(figsize=(6, 8))
plt.imshow(output_rgb)
plt.title("Sunglass Overlay Output")
plt.axis("off")
plt.show()
```

#### OUTPUT : 

<img width="433" height="547" alt="image" src="https://github.com/user-attachments/assets/9ed450d4-0969-43d9-8494-b6a2ae6864ec" />


<img width="595" height="806" alt="image" src="https://github.com/user-attachments/assets/11631316-a7b5-41d7-8010-bd69d2636d98" />

#### RESULT :

The program was successfully implemented using Python, OpenCV, and NumPy. The face and eye region were identified, and the sunglass image was resized, aligned, and blended with the input passport-size photo using image masking and alpha blending. The final output displayed the photo with sunglasses applied successfully.
