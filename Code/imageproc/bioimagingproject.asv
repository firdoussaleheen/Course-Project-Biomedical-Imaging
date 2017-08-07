clear all; close all; clc;
%% Read image file
% read rgb image file
Irgb = imread('163.bmp');
Iref = imread('163ref.bmp');
% subtract the reference image from the data image
I = Iref-Irgb; 
%figure, imshow(I)
% convert rgb file to grayscale
imshow(Irgb);
Igray = rgb2gray(I);
Igray = imadjust(Igray);
%Igray = wiener2(Igray,[3 3]);
figure,imshow(Igray);title 'Original Grayscale Image';


%% Add Gaussian Random Noise
% imnoise is a function that uses RAND to generate Gaussian random noises
% m and v are mean and variances of the noise distribution
m=0;
v1=0.0001;
v2=0.001;
v3=0.01;
v4=0.1;
v5=1;
J1 = imnoise(Igray,'gaussian',m,v1);
J2 = imnoise(Igray,'gaussian',m,v2);
J3 = imnoise(Igray,'gaussian',m,v3);
J4 = imnoise(Igray,'gaussian',m,v4);
J5 = imnoise(Igray,'gaussian',m,v5);
figure,imshow(J1);title 'Image with Gaussian Noise with \mu=0 and var=0.0001';
figure,imshow(J2);title 'Image with Gaussian Noise with \mu=0 and var=0.001';
figure,imshow(J3);title 'Image with Gaussian Noise with \mu=0 and var=0.01';
figure,imshow(J4);title 'Image with Gaussian Noise with \mu=0 and var=0.1';
figure,imshow(J5);title 'Image with Gaussian Noise with \mu=0 and var=1';
% Jarray = zeros(480,640,1,4);
% Jarray(:,:,:,1) = J1;
% Jarray(:,:,:,2) = J2;
% Jarray(:,:,:,3) = J3;
% Jarray(:,:,:,4) = J4;
% figure,montage(Jarray,'DisplayRange', []);
subplot(1,2,1);
imshow(J1); title 'Image with a LITTLE noise (mean=0,variance=0.0001)';
subplot(1,2,2);
imshow(J5); title 'Image with a LOT noise (mean=0,variance=1)';

%% Signal to Noise Ratio Calculation
% calculate variance of original image using std2 function
% noise variance is known
% SNR = signal variance/noise variance
% SNR(dB) = 10*log(SNR); 10 base log

SNR1 = 10*log10(std2(Igray).^2/v1);
SNR2 = 10*log10(std2(Igray).^2/v2);
SNR3 = 10*log10(std2(Igray).^2/v3);
SNR4 = 10*log10(std2(Igray).^2/v4);
SNR5 = 10*log10(std2(Igray).^2/v5);


%% SNR in homogeneous and nonhomogeneous region
% three small regions are cropped from J1 using imtool, tissue, tumor, tear
% imtool(J1); load homogeneous; load nonhomogeneous; load tear
%SNRhom = 10*log10(std2(homogeneous).^2/v1);
%SNRnonhom = 10*log10(std2(nonhomogeneous).^2/v1);
%SNRtear = 10*log10(std2(tear).^2/v1);

%% Application of Smoothing filter
% averaging filter and gaussian filter
hsize = [9 9];
H = fspecial('average',hsize);
avIgray = imfilter(Igray,H,'replicate');
subplot(1,2,1); 
imshow(avIgray);title('Image filtered with Averaging Filter');

sigma = 1;
H = fspecial('gaussian',hsize,sigma);
gaussIgray = imfilter(Igray,H,'replicate');
subplot(1,2,2); 
imshow(gaussIgray);title('Image filtered with Gaussian Filter');

% noise effect
hsize = [9 9];
H = fspecial('average',hsize);
avIgray = imfilter(J3,H,'replicate');
subplot(1,2,1); 
imshow(avIgray);title('Image filtered with Averaging Filter');

sigma = 1;
H = fspecial('gaussian',hsize,sigma);
gaussIgray = imfilter(J3,H,'replicate');
subplot(1,2,2); 
imshow(gaussIgray);title('Image filtered with Gaussian Filter');

%iterative application of filter
hsize = [9 9];
H = fspecial('average',hsize);
iter = 10;
avIgray = Igray;
for ii=1:iter
     avIgray = imfilter(avIgray,H,'replicate');
end 

sigma = 1;
H = fspecial('gaussian',hsize,sigma);
gaussIgray = Igray;
for ii=1:iter
     gaussIgray = imfilter(gaussIgray,H,'replicate');
end 
subplot(1,2,1); 
imshow(avIgray);title('Image filtered with Averaging Filter (Iteration=10)');
subplot(1,2,2); 
imshow(gaussIgray);title('Image filtered with Gaussian Filter (Iteration=10)');
%% Application of Edge Detection Filter
% Sobel filter and Canny filter
% preprocessing required before edge detection

se = strel('disk',3);
I2 = imerode(Igray,se);
I3 = imadjust(I2);
imshow(I3);

edgeIgray = edge(I3,'sobel',0.2,'both');
subplot(1,2,1)
imshow(edgeIgray);title('Edge detection with Sobel Filter')
edgeIgray1 = edge(I3,'canny',0.2,'both');
subplot(1,2,2)
imshow(edgeIgray1);title('Edge detection with Canny Filter')

% noise effect
se = strel('disk',3);
I2 = imerode(J2,se);
I3 = imadjust(I2);
imshow(I3);

edgeIgray = edge(I3,'sobel',0.2,'both');
subplot(1,2,1)
imshow(edgeIgray);title('Edge detection with Sobel Filter')
edgeIgray1 = edge(I3,'canny',0.2,'both');
subplot(1,2,2)
imshow(edgeIgray1);title('Edge detection with Canny Filter')

