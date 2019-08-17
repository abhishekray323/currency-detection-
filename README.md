# currency-detection-

1. Abstract 
Recognizing currencies is fake or real is an important issue. People often find it difficult. Our objective is to solve this problem. However, the currency recognition system based on image processing are not sufficient. Our Algorithm is based on the fact that “fake currencies often more than one line in the thin strip while real currency has only one line’. 
Keywords: Currency Recognition, Image Processing 
2. Introduction 
The Aim of our system is to help people who need to recognize real currencies. Our system is based on image processing technique including filtering edge detection, segmentation, image interpolation etc. 
The main steps in the process are 1.) Scanning the image, read it. 2.) Decompose image into HSV and analyse. 3.) Threshold the saturation and value planes to create a binary image. 4.) Do some minor changes like closing 5.) Removing noise 6.) Counting number of black lines 7.) Printing the final result 
2.1 Theoretical Background 
We realise this by programming with MATLAB 
2.1.1 Image Format 
The image we get from a scanner is formatted by JPEG, which involves loss of information and it cannot be recovered. 
2.1.2 Currency Region Extraction 
There are some common features irrespective type of currency. These feature includes a portrait on the right side, some white area on left side (which have a watermark located on it). The value of currency is displayed at the corners. 
Although there are some features which distinguish between real currency from fake. We here use the fact that “real notes have only one black line in their strip while fake has more than one”. 
2.2 Technical Background 
Major technique of this system is image analysis and image processing. The output can be either an image or a set of characteristics or parameter related to the image. We use pre-processing, edge detection, segmentation, pattern matching in the system to recognize the currency. 2.2.1 Edge Detection This is used for finding the boundaries of region. 
2.2.2 Segmentation This divides an image into parts that have a strong correlation with objects. 
2.2.3 Colour RGB model is generally used as basic of colour space. This describes the model based on primary colours red, green, blue. The major purpose of RGB colour model is for the sensing, representation and displays. 
HSV Model has several colour systems used by people. HSV is abbreviated to hue saturation and value. Hue is pure colour and is measured by degrees or percentage. 
Gray Scale describes the colour ranging from black to white. Paints are created by mixing two colours. We have used HSV model in our project. 
2.2.4 Thresholding In thresholding values below particular intensity value is assigned Zero others are assigned one, in such a way a binary image is produced. 
2.2.5 Image Interpolation In this we try to estimate the pixel values of missing pixels by using the intensity of neighbours While processing fake notes we need interpolation because some times due to noise in image more than one lines may occur. 
2.2.6 Gaussian Blur In order to remove the noise in image. Gaussian blur is performed. Convolving the image with a Gaussian function is performed when a Gaussian blur is applied. The equation of a Gaussian function in one dimension is Equation 1. In two dimensions is Equation 
→ Equation 1 Where x is the distance from the origin in the horizontal axis, and σ is the standard deviation of the Gaussian distribution. [5] 
Where x is the distance from the origin in the horizontal axis, y is the distance from the origin in the vertical axis, and σ is the standard deviation of the Gaussian distribution. 
3. Main theme of the work 
Major Concept which distinguishes fake notes from real is “Real notes has its thin strips to be more or less continuous while fake strips has fragmented line in the strip”. So we will try to detect the strip itself first, then we count how many lines we see. If we see only one line fragment then we consider it is real otherwise fake. 
Step 1: Read in the image We use Imread() function reading images. 
clear all; close all; real = imread('real_note.jpg'); %//Real fake = imread('fake_note.jpg'); %//Fake 
Step 2: Decompose image into HSV and analyse This is the pre-processing step, in which we transform the image into HSV and locate each component separately. 
hsvRealImage = rgb2hsv(real); hsvFakeImage = rgb2hsv(fake); figure; imshow([hsvRealImage (:,:,1) hsvRealImage (:,:,2) hsvRealImage (:,:,3)]); title('Real'); figure; imshow([hsvFakeImage (:,:,1) hsvFakeImage (:,:,2) hsvFakeImage (:,:,3)]); title('Fake'); 
This code is displaying each component one by one for Hue, Saturation and Value respectively. After this it is clear from the results shown above that black thin strips has a saturation value quite high, as black is a pure colour. But its value component is very low. 
Step 3: Threshold the saturation and value planes to create a binary image We are trying to extracting the thin strips by saturation and value planes. The criteria for thresholding is that a combination: High saturation and low value are part of black strip. We are cropping out the strips by themselves to make things easier for which we just need to extract out the right columns by leaving the rows and slice them. 
croppedRealImage = hsvRealImage (:,90:95,:); croppedFakeImage = hsvFakeImage (:,93:98,:); satThresh = 0.4; valThresh = 0.3; BWRealImage = (croppedRealImage (:,:,2) > satThresh & croppedRealImage (:,:,3) < valThresh); figure; subplot(1,2,1); imshow(BWRealImage); title('Real'); BWFakeImage = (croppedFakeImage (:,:,2) > satThresh & croppedFakeImage (:,:,3) < valThresh); subplot(1,2,2); imshow(BWFakeImage); title('Fake'); 
Step 4: Interpolation Due to the above process we are still getting the approximate image of black line, that’s why are still getting disconnected real strips. We will try to reconnect the together. This is safe because if we try to do so in the fake image, the part of the strip are so far that closing 
shouldn’t make a difference, but it will help in our real image analysis. As such, we closed these image by six lines that is vertical. 
se = strel('line', 6, 90); BWCloseRealImage = imclose(BWRealImage, se); BWCloseFakeImage = imclose(BWFakeImage, se); figure; subplot(1,2,1); imshow(BWCloseRealImage); title('Real'); subplot(1,2,2); imshow(BWCloseFakeImage); title('Fake'); 
Step 5: Noise Removal We can notice that for each of the images there are some noisy pixels on the edges. As such, Lets use an area opening bwareaopen(). This function removes pixel areas in a black and white image that have less than a certain area. We are choosing here 15 to get rid of the pixels along the edges that don’t belong to the strips. 
figure; areaOpenReal = bwareaopen(BWCloseRealImage, 15); imshow(areaOpenReal); title('Real'); figure; areaOpenFake = bwareaopen(BWCloseFakeImage, 15); imshow(areaOpenFake); title('Fake'); 
Step 6: Count the number of black line 
This is the last step which involves simply counting the number of black lines in each image. If there is just one, this denotes the bank note is real, while if there is more than this says the bank note is fake. We can use bwlabel() and use the second parameter to count the number of objects. [~,count_Real] = bwlabel(areaOpenReal); [~,count_Fake] = bwlabel(areaOpenFake); disp(['The total number of black lines present in the real note is: ' num2str(count_Real)]); disp(['The total number of black lines present in the fake note is: ' num2str(count_Fake)]); 
We get the following output in MATLAB. 
The total number of black lines present in the real note is: 1 The total number of black lines present in the fake note is: 4 
Full Code: 
clear all; close all; real = imread('real_note.jpg'); %//Real fake = imread('fake_note.jpg'); %//Fake 
%%//Pre-analysis hsvRealImage = rgb2hsv(real); hsvFakeImage = rgb2hsv(fake); figure; imshow([hsvRealImage (:,:,1) hsvRealImage (:,:,2) hsvRealImage (:,:,3)]); title('Real'); figure; imshow([hsvFakeImage (:,:,1) hsvFakeImage (:,:,2) hsvFakeImage (:,:,3)]); title('Fake'); 
%%//Initial segmentation croppedRealImage = hsvRealImage (:,90:95,:); croppedFakeImage = hsvFakeImage (:,93:98,:); satThresh = 0.4; valThresh = 0.3; BWRealImage = (croppedRealImage (:,:,2) > satThresh & croppedRealImage (:,:,3) < valThresh); figure; subplot(1,2,1); imshow(BWRealImage); title('Real'); 
BWFakeImage = (croppedFakeImage (:,:,2) > satThresh & croppedFakeImage (:,:,3) < valThresh); subplot(1,2,2); imshow(BWFakeImage); title('Fake'); 
%%//Post-process se = strel('line', 6, 90); BWCloseRealImage = imclose(BWRealImage, se); BWCloseFakeImage = imclose(BWFakeImage, se); figure; subplot(1,2,1); imshow(BWCloseRealImage); title('Real'); subplot(1,2,2); imshow(BWCloseFakeImage); title('Fake'); 
%%//Area open the image figure; areaOpenReal = bwareaopen(BWCloseRealImage, 15); imshow(areaOpenReal); title('Real'); figure; areaOpenFake = bwareaopen(BWCloseFakeImage, 15); imshow(areaOpenFake); title('Fake'); 
%%//Count how many objects there are [~,count_Real] = bwlabel(areaOpenReal); [~,count_Fake] = bwlabel(areaOpenFake); disp(['The total number of black lines present in the real note is: ' num2str(count_Real)]); disp(['The total number of black lines present in the fake note is: ' num2str(count_Fake)]); 
4. Results We have used following images as input: real_note.jpg 
fake_note.jpg 
We have got the final output as: 
The total number of black lines present in the real note is: 1 The total number of black lines present in the fake note is: 4 
5. Discussions and conclusion 
We can clearly see in the output that there was just one line in the real note while the fake note has more than one. This algorithm can be used to detect whether a bank note is real or fake. 
There is another interesting point that if a fake note doesn’t have any strip then we will get number of lines in the final output as Zero. 
