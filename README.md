# EasySR
## *Easy Web UI for Generative 3D Inference of Rat Brain MRI*

#### *EasySR HF-StreamlitApp* https://huggingface.co/spaces/hwonheo/easysr  

![image](https://github.com/hwonheo/easysr/assets/109127356/83508ef7-39e5-4426-954e-ccd86744fb10)
---

# *Overview*

 - **EasySR** is a cutting-edge project focused on **super-resolution reconstruction of rat brain MR images**. This project's training code specifically addresses enhancing axial slice resolution, commonly recorded as thicker slices in rat brain MRIs. Our approach is tailored to work with high-precision isovoxel rat brain MRI data, captured at a fine resolution of 0.2mm or less.

---
# *Requirements*

    Python 3.x
    PyTorch

---
# *Installation*
```bash
git clone https://github.com/hwonheo/easysr.git
cd easysr
pip install -r requirements.txt
```

---
# *Dataset Preparation (before training dataset)*

#### 128 x 192 x 128 resampled data needs
We provided PreProc Script for the training dataset (*data_preproc.sh, mri_preproc.py, data_splitter.py*)

To use the data_preproc.sh script, navigate to the directory containing the script in your terminal, then execute it with your input folder path followed by either --t1 or --t2 flag to specify the template type. For example: ./data_preproc.sh -i [input_folder] --t1. Ensure Python and necessary libraries are installed in your environment.
```bash
./data_preproc.sh -i [input_folder] --t1 (or --t2)
```
or
```bash
python mri_preproc.py --input [input_folder] --output [output_folder] --t2 #or --t1
python utils/data_splitter.py --input [output_folder]
```
 
---

# *Usage (train.py)*

Training own your data (examples) 
```bash
python train.py --epochs 100 --batch_size 4 --save_path './ckpt'
```
or, training on going your new data in train folder
```bash
python train.py --epochs 100 --batch_size 4 --final './ckpt'
```
or, training on going your data in train folder using the provided pre-train checkpoint
```bash
python train.py --epochs 100 --batch_size 4 --final
```
'--help' provide more info
```bash
python train.py --help
```

---

 # *Usage (inference.py):* *for cli*
 
 To perform inference, users can run the script with the following command, specifying the paths to their input MRI images, the model checkpoint, and the desired output directory:
```bash
python inference.py --input [input_path] --ckpt [checkpoint_path] --output [output_path]
```

---

 # *WebUI-based Inference (Streamlit App)*

 ## *Usage (app.py):* *local*
   ```bash
      streamlit run app.py
   ```
   or
 #### *Test your low-res MR images on* [*EasySR HF-StreamlitApp*](https://huggingface.co/spaces/hwonheo/easysr)

---

## More information about EasySR

<details>
<summary>More information</summary>

## Overview

 - **EasySR** is a cutting-edge project focused on **super-resolution reconstruction of rat brain MR images**. This project's training code specifically addresses enhancing axial slice resolution, commonly recorded as thicker slices in rat brain MRIs. Our approach is tailored to work with high-precision isovoxel rat brain MRI data, captured at a fine resolution of 0.2mm or less.

  - The primary goal of EasySR is to upscale these images to an even finer resolution of 0.15mm, achieving an isotropic output. This enhancement allows for more detailed and precise anatomical studies. However, it's important to note that while EasySR excels in spatial resolution improvement, it does not maintain the original signal intensity due to the normalization process involved. Therefore, this tool may not be suitable for experiments that rely heavily on signal intensity measurements.

  - Currently, EasySR is in an experimental stage and requires extensive validation. The effectiveness of the super-resolution reconstruction heavily depends on the quality and quantity of the training data. We recommend using at least five high-quality datasets for training to achieve optimal results. Users are encouraged to train the model using the provided pre-trained checkpoints and to contribute back by sharing their well-trained checkpoints. This collaborative approach will significantly enhance the utility and accuracy of EasySR for the scientific community.

  - As this project does not involve data collection, we rely on contributions from users for improved training datasets. **With more data and shared checkpoints, EasySR has the potential to become a more robust and reliable tool for rat brain MRI analysis.**

---

## Key Features of EasySR
### 1. High-Performance Architecture:

   Built on PyTorch, EasySR offers a high-efficiency and flexible framework, ideal for advanced MRI super-resolution tasks.

### 2. Specialized MRI Processing:

   Features a custom MRIDataset class for efficient MRI handling, including dynamic resampling and normalization, enhancing image clarity and detail.

### 3. Innovative GAN Structure:

   Utilizes a state-of-the-art Generative Adversarial Network with a Resnet-based generator and patch discriminator, specifically optimized for MRI super-resolution.

### 4. Advanced Validation Metrics:

   Incorporates rigorous image quality assessments using SSIM, PSNR, and MSE, tailored for 3D MRIs, ensuring structural integrity and high fidelity in generated images.

### 5. Efficient Training and Analysis:

   Supports CUDA-optimized training, checkpointing, loss tracking, and automatic metric logging, making the model training and evaluation both efficient and comprehensive.

---

## Preprocessing flow chart
```

# MRI Image Processing Script Data Flow

[ Main Function: main() ]
    |-> Parse Input Arguments
    |   [ Input: NIfTI file or folder (--input) ]
    |   [ Use T2 or T1 template (--t2, --t1) ]
    |   [ Output folder (--output) ]
    |
    |-> Check and Download Model If Needed
    |   [ Function: download_model_if_needed(templates_folder) ]
    |
    |-> Determine Fixed Image Path
    |   [ Select T1 or T2 template based on arguments ]
    |
    v
[ Instantiate MRIProcessor Class ]
    |-> Input: Parsed arguments (input, output, fixed_image_path)
    |
    |-> Process Files
    |   [ Method: process_files() ]
    |   [ Process each file using affine registration ]
    |
    v
[ Affine Registration for Each File ]
    |-> Method: affine_registration(moving_image_path, output_path)
    |   [ Read fixed and moving images ]
    |   [ Perform affine registration using AntsPy ]
    |   [ Save the registered image to output path ]
    |
    v
[ Completion ]
    |-> Output: "MRI preprocessing is complete."

```
To effectively train the EasySR model, it's essential to prepare your dataset with specific requirements in mind. Ensure your data meets the following criteria for optimal results:

 1. Resolution and Type: 
   The dataset should consist of high-resolution rat brain MRI images. Each image must be captured with a resolution of **0.2mm** or finer to ensure the model can effectively learn and enhance the image quality. This fine resolution is crucial for achieving the desired super-resolution outcomes.

 2. Isotropy: 
   The images should be **isovoxel**, meaning they maintain equal resolution in all three dimensions. This isotropy is key for the model to uniformly upscale the images in the axial plane without distorting the anatomy.

 3. File Format: 
   Prepare your data in the **NIfTI format**, which is widely used in medical imaging. The files should be either in .nii or .nii.gz format. This standard format ensures compatibility with the data loading and processing methods used in the EasySR code.

 4. Dataset Size: 
   For effective training, it's recommended to use a minimum of five distinct datasets. This diversity in the training data helps the model generalize better and enhances its ability to upscale various images accurately.

 5. Data Organization: 
   Organize your dataset in a structured manner, preferably in a dedicated directory. This organization facilitates easier loading and batch processing of the images during training.

 Ensure that your dataset is prepared with care, as the training data quality significantly influences the EasySR model's performance. By adhering to these guidelines, you set the foundation for successful super-resolution reconstruction of rat brain MRI images.


## Code Structure

 1. ResnetBlock
 - Purpose: A fundamental building block of the ResnetGenerator, designed to create a convolutional block with residual learning.
Implementation 
 - Details:
   - Each ResnetBlock consists of two convolutional layers (nn.Conv3d), each followed by batch normalization (nn.BatchNorm3d) and the first followed by a LeakyReLU activation (nn.LeakyReLU).
   - The block implements a skip connection by adding the input to the output of the convolutional block, facilitating gradient flow and mitigating the vanishing gradient problem in deep networks.
   - This structure is crucial for learning identity mappings and refining features across the network.
 2. DeUpBlock
 - Purpose: Specifically designed for upsampling in the width dimension, this block is integral for increasing the resolution of the MRI slices in the desired dimension.
 - Implementation Details:
   - The DeUpBlock uses a 3D transposed convolution (nn.ConvTranspose3d) for upsampling. This approach is tailored to upsample the MRI images selectively along the width dimension, aligning with the project's focus on axial slice super-resolution.
   - A LeakyReLU activation follows the transposed convolution, adding non-linearity to the upscaling process.
   - This selective upsampling is a unique aspect of the EasySR project, differentiating it from typical 3D upsampling methods that uniformly scale across all dimensions.
 3. PatchDiscriminator
 - The PatchDiscriminator class in the EasySR project is a key component of the adversarial network, designed to differentiate between real and generated super-resolution MRI images.
 - Initialization: The discriminator takes grayscale images as input (input_nc=1) and begins with a relatively small number of filters (ndf=16). This setting is optimal for processing the MRI data.
 - Convolutional Layers:
   - The model consists of several convolutional layers, each forming a conv_block. These blocks are composed of a 3D convolution (nn.Conv3d), batch normalization (nn.BatchNorm3d), and LeakyReLU activation (nn.LeakyReLU), progressively increasing the depth of the feature maps.
   - The series of convolutions (from conv1 to conv4) gradually downsample the input, extracting increasingly abstract and complex features from the images.
   - The final convolution layer (conv5) is designed to reduce the output to a single channel, setting the stage for binary classification.
  - Output Layer:
    - Following the convolutional layers, the model employs a flattened layer (nn.Flatten) and a fully connected layer (nn.Linear). The number '539' in the linear layer's input should be adjusted based on the flattened output size of the preceding layers.
    - The final output is obtained through a sigmoid activation function (nn.Sigmoid), providing a probability score indicating whether the input image is real or generated.
  - Purpose and Function:
    - The PatchDiscriminator is tailored to assess localized regions (or 'patches') of the input images, making it particularly effective for tasks like super-resolution where fine details are crucial.
    - By distinguishing between real and upscaled images, this discriminator plays a crucial role in training the generator to produce more realistic super-resolution outputs.
```
ResnetGenerator 
│   (
│    ngf=32, input_nc=1, output_nc=1, 
│    n_residual_blocks=2
│   )
│   Input: [1, 128, 32, 128]
│
├── conv_block1
│   ├── Conv3d (ngf=32) → Size: [32, 128, 32, 128]
│   └── LeakyReLU
│
├── ResnetBlock x 2 (Repeat for n_residual_blocks)
│   ├── Conv3d (ngf=32) → Size: [32, 128, 32, 128]
│   ├── BatchNorm3d
│   ├── LeakyReLU
│   ├── Conv3d (ngf=32) → Size: [32, 128, 32, 128]
│   ├── BatchNorm3d
│   └── Residual Connection → Size: [32, 128, 32, 128]
│
├── conv_block2
│   ├── Conv3d (ngf=32) → Size: [32, 128, 32, 128]
│   └── BatchNorm3d
│
├── DeUpBlock (Upsampling)
│   ├── ConvTranspose3d (ngf=32) → Size: [32, 128, 192, 128]
│   └── LeakyReLU
│
└── conv3
    └── Conv3d (output_nc=1) → Size: [1, 128, 192, 128]
```
```
PatchDiscriminator (input_nc=1, ndf=3)
│ Input: [1, 128, 192, 128]  # MR Image Size
│
├── conv1 (Convolution Block)
│   ├── Conv3d → Size: [3, 64, 96, 64]  
│   │    # Kernel Size: 4, Stride: 2, Padding: 1
│   ├── BatchNorm3d
│   └── LeakyReLU
│
├── conv2 (Convolution Block)
│   ├── Conv3d → Size: [6, 32, 48, 32]
│   │    # Kernel Size: 4, Stride: 2, Padding: 1
│   ├── BatchNorm3d
│   └── LeakyReLU
│
├── conv3 (Convolution Block)
│   ├── Conv3d → Size: [12, 16, 24, 16]
│   │    # Kernel Size: 4, Stride: 2, Padding: 1
│   ├── BatchNorm3d
│   └── LeakyReLU
│
├── conv4 (Convolution Block)
│   ├── Conv3d → Size: [24, 8, 12, 8]
│   │    # Kernel Size: 4, Stride: 2, Padding: 1
│   ├── BatchNorm3d
│   └── LeakyReLU
│
├── conv5 (Final Convolution)
│   └── Conv3d → Size: [1, 5, 6, 5]
│        # Kernel Size: 4, Padding: 1
│
├── Flatten
├── Fully Connected Layer
└── Sigmoid Activation
```


## Inference Process

 The EasySR project includes a comprehensive inference script that allows users to apply the trained ResnetGenerator model to their own MRI images for super-resolution reconstruction. Here's an overview of how this process works:

 Key Components:

 *MRIInference Class:* A dedicated class for handling the inference process. It loads the MRI images, processes them for input into the model, and saves the output images.

 *Image Resampling:* The script includes functionality to resample input images to isotropic resolution using the ants library, ensuring consistency in image dimensions before feeding them to the model.

 *Affine Registration:* Post-processing includes an affine registration step to align the generated image with the original, maintaining anatomical accuracy.


 **Process Workflow:**

 *Loading the Model:* The script initializes the ResnetGenerator model and loads the pre-trained weights from a specified checkpoint.

 *Image Processing:* For each input MRI image, the script performs normalization, rotation, and resampling to match the model's input shape requirements.
 Model Inference: The processed image is fed into the model, generating a super-resolved output.

 *Post-Processing:*
 The output image is resampled back to the desired isotropic resolution.
Affine registration aligns the generated image with the original MRI scan, facilitating direct comparison.

 *Saving the Result:* The final super-resolution image is saved, providing a detailed and enhanced view of the original MRI scan.


## Enhanced Validation with Advanced Image Quality Metrics
 - Robust Image Quality Assessment:
    The latest update to EasySR introduces a robust validation mechanism, vital for ensuring the super-resolution reconstruction maintains high fidelity compared to the original MR images. Our validation now integrates advanced image quality metrics, including *Structural Similarity Index (SSIM)*, *Peak Signal-to-Noise Ratio (PSNR)*, and *Mean Squared Error (MSE)*. These metrics provide a comprehensive evaluation of image quality, crucial for medical imaging applications where precision is paramount.

 - Tailored for 3D MRI Data:
    Recognizing the unique challenges of 3D MRI data, we have customized the SSIM metric to effectively process and analyze the structural integrity of multi-dimensional images. Our implementation calculates SSIM for each 2D slice within the *3D MRI volume*, providing an *average score* that reflects the overall structural similarity. This approach ensures a thorough assessment of the generated images' quality, focusing on preserving crucial anatomical details.

 - **Streamlined Validation Process**:
    To enhance user experience and efficacy, EasySR now automates the recording of these image quality metrics at specified intervals during the training phase. This feature ensures meticulous logging and storage of the validation results in **CSV format**, enabling easy tracking and analysis of the model's performance over time. Such detailed monitoring is essential for ongoing model refinement and aligns with our commitment to delivering reliable and accurate tools for the scientific community.

    **This validation upgrade marks a significant step in EasySR's development**, reinforcing our dedication to advancing the field of super-resolution MRI analysis through innovative and reliable solutions.


</details>
  

# **References**

https://github.com/imatge-upc/3D-GAN-superresolution


https://antspyx.readthedocs.io/en/latest/index.html


https://github.com/eriklindernoren/PyTorch-GAN


https://paperswithcode.com/method/gan


https://github.com/freesurfer/freesurfer

---

# **Contributing**

https://github.com/hwonheo  
https://github.com/ssimu  
https://github.com/olivepicker  

---

# **License**

not yet decided.
please contact

---

# **Contact**

heohwon@gmail.com

---

## **Acknowledgements**

This research was supported by the Basic Science Research Program through the National Research Foundation of Korea (NRF) funded by the Ministry of Education (NRF-2022R1I1A1A01072397)
