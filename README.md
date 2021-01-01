# Neural Net for Sentence Classification
Introduction and Architecture details:
Sentence classifier presented for this assignment is based on below papers:
1. Convolutional Neural Networks for Sentence Classification by Yoon Kim
2. MobileNetV2: Inverted Residuals and Linear Bottleneck by Mark Sandler et. al.
Although MobileNetV2 is for image classification, I wanted to try out the same architecture to classify sentences.

## Network Architecture:
MobileNetV2 architecture uses a concept called ‘BottleNeck Residual layer’. This is special CNN layers followed by each of the CNN layer mentioned in above table. This ‘BottleNeck Residual layer’ takes the output channel of the CNN layer, expands its output channel further by the ‘expansion factor’ mentioned above and again brings it back to the original output channel length before passing it to next layer. For example, in the output channel size of third layer if 24, bottleneck layer expands it by the expansion factor of 6 which is 144 using a CNN layer and again scales it back to 24 with another CNN layer before passing on to next CNN layer. This is inline with Yoon Kim’s paper which mentions us to use varying layers of output channels. Along with this, each layer is repeated by ‘Iterations(IN)’ times.
![NeuralNetArch](https://user-images.githubusercontent.com/46570073/103436154-9b992b80-4be6-11eb-941f-7453e3ca637b.JPG)
![BottleNeck](https://user-images.githubusercontent.com/46570073/103436156-9c31c200-4be6-11eb-85c0-001e2f00215d.JPG)

Along with this trimmed down version (shown below) of the original architecture was used but did not give optimal results. Results shared in Experiments section
Less complex MobilenetV2:
input_channels = [1, 32, 16, 24, 32, 64, 96]
output_channels = [32, 16, 24, 32, 64, 96, 160]
expansion_factor = [1, 1, 3, 3, 3, 3, 1]
iterations = [1, 1, 2, 3, 2, 3, 1]
strides = [1, 1, 2, 2, 2, 1, 1]
Activation: Also, each of the CNN layer( both normal and bottleneck) is followed by batchnorm2d layer. Batch Norm layer was followed by Tanh layers. But experiments were done with relu, leaky relu and tanh. Tanh was finally chosen because it gave optimal result.
Drop out: Final layer is fully connected Linear layer. Experiments were conducted with and without drop out layer between this fully connected linear layer. Finally drop out of 0.5 was chosen. Though it did not have great effect on the final result.
Prediction: For prediction Softmax layer is used followed by argmax to predict the label number.

## Training:
![Training](https://user-images.githubusercontent.com/46570073/103436155-9b992b80-4be6-11eb-9d46-5a1eefdcdf67.JPG)

## Word to vectors:
Used Pretrained vector for Google news data from below link. It has ~2.8mn words. I used only top 600,000 words to build the word vectors for each sentence. In the below link google news word vector is in text format.
http://vectors.nlpl.eu/repository/

Experiments:
1. Without Drop out and RelU activation:
![Exp1](https://user-images.githubusercontent.com/46570073/103436157-9c31c200-4be6-11eb-94db-9a685eb520b9.JPG)

2. With drop out of 0.2 during training and ReLU:
![exp2](https://user-images.githubusercontent.com/46570073/103436149-9a67fe80-4be6-11eb-9cda-18695dc0b413.JPG)

3. With Dropout of 0.5 during training and Leaky ReLU:
![exp3](https://user-images.githubusercontent.com/46570073/103436151-9b009500-4be6-11eb-881f-bda894c2cf52.JPG)

4. Less complex version of MobilenetV2, with Dropout of 0.5 during training and Tanh activation:
![exp4](https://user-images.githubusercontent.com/46570073/103436152-9b992b80-4be6-11eb-94c2-36a37f2a461b.JPG)

5. Full MobilenetV2 with Dropout of 0.5 during training and Tanh activation.
![exp5](https://user-images.githubusercontent.com/46570073/103436153-9b992b80-4be6-11eb-9742-69d0dc4617f9.JPG)

## Conclusions:
• MobileNetV2 architecture can be used for sentence classification with accuracy of 80.88% on validation data. Although it is primarily designed for image classification.
• It was interesting to see how leaky ReLU had no effect compared to ReLU and Tanh performed the best.
• Also, in above experimentations, it noteworthy that drop out layer in final layer of a complex architecture like MobileNetV2 had little to no effect in the outcome of the result.
