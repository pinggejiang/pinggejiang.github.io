---
layout:     post
title:      "Mouse tracking sample project"
subtitle:   ""
date:       2016-11-16 20:42:07
author:     "Pingge Jiang"
header-img: "img/home-bg.jpg"
catalog:    true
categories: research
tags:
    - [matlab, tracking, image processing]

---
[![Imouse tracking](/img/post/mouse_tracking.png)](https://www.youtube.com/embed/uaoAqCLOTGM "mouse tracking")


This is my sample mouse tracking result produced by image segmentation methods.

The algorithm includes: preprocessing-> pixel labeling-> Gabor filtering-> region clustering-> postprocessing

Two contributive cited Matlab functions for this project:

## imgaussian.m:
```matlab
function I=imgaussian(I,sigma,siz)
% IMGAUSSIAN filters an 1D, 2D color/greyscale or 3D image with an 
% Gaussian filter. This function uses for filtering IMFILTER or if 
% compiled the fast  mex code imgaussian.c . Instead of using a 
% multidimensional gaussian kernel, it uses the fact that a Gaussian 
% filter can be separated in 1D gaussian kernels.
%
% J=IMGAUSSIAN(I,SIGMA,SIZE)
%
% inputs,
%   I: The 1D, 2D greyscale/color, or 3D input image with 
%           data type Single or Double
%   SIGMA: The sigma used for the Gaussian kernel
%   SIZE: Kernel size (single value) (default: sigma*6)
% 
% outputs,
%   J: The gaussian filtered image
%
% note, compile the code with: mex imgaussian.c -v
%
% example,
%   I = im2double(imread('peppers.png'));
%   figure, imshow(imgaussian(I,10));
% 
% Function is written by D.Kroon University of Twente (September 2009)

if(~exist('siz','var')), siz=sigma*6; end

if(sigma>0)
    % Make 1D Gaussian kernel
    x=-ceil(siz/2):ceil(siz/2);
    H = exp(-(x.^2/(2*sigma^2)));
    H = H/sum(H(:));

    % Filter each dimension with the 1D Gaussian kernels\
    if(ndims(I)==1)
        I=imfilter(I,H, 'same' ,'replicate');
    elseif(ndims(I)==2)
        Hx=reshape(H,[length(H) 1]);
        Hy=reshape(H,[1 length(H)]);
        I=imfilter(imfilter(I,Hx, 'same' ,'replicate'),Hy, 'same' ,'replicate');
    elseif(ndims(I)==3)
        if(size(I,3)<4) % Detect if 3D or color image
            Hx=reshape(H,[length(H) 1]);
            Hy=reshape(H,[1 length(H)]);
            for k=1:size(I,3)
                I(:,:,k)=imfilter(imfilter(I(:,:,k),Hx, 'same' ,'replicate'),Hy, 'same' ,'replicate');
            end
        else
            Hx=reshape(H,[length(H) 1 1]);
            Hy=reshape(H,[1 length(H) 1]);
            Hz=reshape(H,[1 1 length(H)]);
            I=imfilter(imfilter(imfilter(I,Hx, 'same' ,'replicate'),Hy, 'same' ,'replicate'),Hz, 'same' ,'replicate');
        end
    else
        error('imgaussian:input','unsupported input dimension');
    end
end
```

## imageDerivatives2D.m:

```matlab
function J=ImageDerivatives2D(I,sigma,type)
% Gaussian based image derivatives
%
%  J=ImageDerivatives2D(I,sigma,type)
%
% inputs,
%   I : The 2D image
%   sigma : Gaussian Sigma
%   type : 'x', 'y', 'xx', 'xy', 'yy'
%
% outputs,
%   J : The image derivative
%
% Function is written by D.Kroon University of Twente (July 2010)

% Make derivatives kernels
[x,y]=ndgrid(floor(-3*sigma):ceil(3*sigma),floor(-3*sigma):ceil(3*sigma));

switch(type)
    case 'x'
        DGauss=-(x./(2*pi*sigma^4)).*exp(-(x.^2+y.^2)/(2*sigma^2));
    case 'y'
        DGauss=-(y./(2*pi*sigma^4)).*exp(-(x.^2+y.^2)/(2*sigma^2));
    case 'xx'
        DGauss = 1/(2*pi*sigma^4) * (x.^2/sigma^2 - 1) .* exp(-(x.^2 + y.^2)/(2*sigma^2));
    case {'xy','yx'}
        DGauss = 1/(2*pi*sigma^6) * (x .* y)           .* exp(-(x.^2 + y.^2)/(2*sigma^2));
    case 'yy'
        DGauss = 1/(2*pi*sigma^4) * (y.^2/sigma^2 - 1) .* exp(-(x.^2 + y.^2)/(2*sigma^2));
end

J = imfilter(I,DGauss,'conv','symmetric');
```
