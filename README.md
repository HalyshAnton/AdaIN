# AdaIN
This project demonstrates the application of style transfer techniques using Adaptive Instance Normalization. It aims to transfer the artistic style of one image (style image) onto the content of another image (content image) to create visually appealing and artistic results. I use three runtime controls: degree of stylization, style interpolation and color control.

## Code and Resources Used 
**Python Version:** 3.10  
**Packages:** numpy, matplotlib, tensorflow, keras  
**Data source:** https://github.com/penny4860/keras-adain-style-transfer  
**Articles:** 
AdaIN model: https://arxiv.org/pdf/1703.06868.pdf
Color mapping: https://mrl.cs.nyu.edu/publications/hertzmann-thesis/hertzmann-thesis-300dpi.pdf

# Model architecture
The model building process consists of the following steps:

Encoder: pre-trained CNN model is used as the encoder to extract the features from both the style and content images. By passing the images through the encoder, we obtain feature maps that capture the visual content. I have used VGG19 to block4_conv1 layer
AdaIN: non-trainable part that normalized content features and linearly shift it to style fearture mean and variance(for more details read article)
Decoder: The reverse of the encoder process is performed using a decoder. It has reversed architecture without any normalization and UpSampling layers instead Pooling ones. This step involves reconstructing the image from the combined features obtained from the AdaIN step.

![alt text](https://github.com/HalyshAnton/AdaIN/blob/main/formulas/model%20architecture.png)

# Degree of stylization
The degree of stylization (alpha) can be regulated by using the operation of linear combination between content feature and AdaIN output before using decoder.  
Here:
* f - encoder
* g - decoder
* c - content image
* s - style image

![alt_text](https://github.com/HalyshAnton/AdaIN/blob/main/formulas/formula%20degree%20of%20stylization.png)  

![alt text](https://github.com/HalyshAnton/AdaIN/blob/main/degree%20of%20stylization.png)

# Style interpolation
To interpolate between a set of style images with corresponding weights we similarly interpolate between AdaIN outputs

![alt_text](https://github.com/HalyshAnton/AdaIN/blob/main/formulas/formula%20style%20interpolation.png)  

![alt text](https://github.com/HalyshAnton/AdaIN/blob/main/weighted%20stylization.png)

# Color control
To preserve the color of the content image, we first match the color distribution of the style image to that of the content image using linear transform for each pixel.I use 3 methods for building matrix A: cholesky, PCA and building symetric matrix. After that I perform a normal style transfer with new style image.

![alt_text](https://github.com/HalyshAnton/AdaIN/blob/main/formulas/formula%20color%20control.png)  
  
![alt text](https://github.com/HalyshAnton/AdaIN/blob/main/color%20control.png)
