# 翁啟文 <span style="color:red">(103061112)</span>

# HW0 / Pixel array manipulation

## Overview
The project is related to two methods of image processing: flipping and rotation operations. To implement these two operations, we write two functions: flip.m and rotation.m. Then we call both functions by lab0_main.m to run the processing in Matlab. Below are the implementation details.

> flip.m  
> rotation.m  
## Implementation
1. flip.m:  
	In **flip.m**, there is a function **I_flip()** inside. The function has two arguments: one is an image array with 3 channels (R, G and B); the other is operating type (0: horizontal flipping, 1: vertical flipping and 2: horizontal+vertical flipping). In the beginning, the function saves **R**, **G** and **B** from the input image array, and gets the **height** and **width** of the image:
	``` Matlab
	% RGB channel
	R = I(:,:,1);
	G = I(:,:,2);
	B = I(:,:,3);
	% get height, width, channel of image
	[height, width, channel] = size(I);
	```
	Next, use **"if statement"** to judge which operating type should be apply:
	``` Matlab
	%%  horizontal flipping
	if type==0
	......
	%% vertical flipping
	if type==1
	......
	%%  horizontal + vertical flipping
	if type==2
	......
	```
	After knowing the type, do the corresponding operations:    
	* **horizontal flipping (type 0):**   
	arrange elements of R, G and B upside down horizontally. For example, **R[h][1]** will be placed at **R_flip[h][width]**, **R[h][2]** at  **R_flip[h][width-1]**,..., **R[h][n]** at  **R_flip[h][width-n+1]**.
	* **vertical flipping (type 1):**    
	arrange elements of R, G and B upside down vertically. For example, **R[1][w]** will be placed at **R_flip[height][w]**, **R[2][w]** at  **R_flip[height-1][w]**,..., **R[n][w]** at  **R_flip[height-n+1][w]**.
	* **horizontal+vertical flipping (type 2):**   
		combine two operations (type 0 and type1) mentioned above. For example, **R[1][1]** will be placed at **R_flip[height][width]**, **R[2][3]** at  **R_flip[height-1][width-2]**,..., **R[m][n]** at  **R_flip[height-m+1][width-n+1]**.
		
	##### Matlab code:
	```Matlab
	%Type 0:
	for h = 1 : height
		for w = 1 : width 
			R_flip(h, w) = R(h,width-w+1);
			G_flip(h, w) = G(h,width-w+1);
			B_flip(h, w) = B(h,width-w+1);
      		end
   	end
	
	%Type 1:
	for h = 1 : height
		for w = 1 : width 
			R_flip(h, w) = R(height-h+1,w);
			G_flip(h, w) = G(height-h+1,w);
			B_flip(h, w) = B(height-h+1,w);
		end
	    end
	
	%Type 2:
	for h = 1 : height
		for w = 1 : width 
			R_flip(h, w) = R(height-h+1,width-w+1);
			G_flip(h, w) = G(height-h+1,width-w+1);
        		B_flip(h, w) = B(height-h+1,width-w+1);
		end
	end
	```
	Finally, convert the data type from double to uint8, and return a processed image:
	```Matlab
	% save R_flip, G_flip, B_flip to output image
   	I_flip(:,:,1) = uint8(R_flip);
	I_flip(:,:,2) = uint8(G_flip);
	I_flip(:,:,3) = uint8(B_flip);
	```
	##### Results
	
	type0 | 
	:-----:|
	<img src=https://github.com/steven14ggyy/DSP_Lab_HW0/blob/master/results/image/image.jpg width="70%"/> |
	<img src=https://github.com/steven14ggyy/DSP_Lab_HW0/blob/master/results/image/flipping_image.jpg width="70%"/> | 
	
	    type1| type2 |
	:-------:|:------:|	
	 <img src=https://github.com/steven14ggyy/DSP_Lab_HW0/blob/master/results/DSC_0531/DSC_0531.JPG width="70%"/> | <img src=https://github.com/steven14ggyy/DSP_Lab_HW0/blob/master/results/DSC_1182/DSC_1182.JPG width="70%"/>|  
	 <img src=https://github.com/steven14ggyy/DSP_Lab_HW0/blob/master/results/DSC_0531/flipping_image.jpg width="70%"/> | <img src=https://github.com/steven14ggyy/DSP_Lab_HW0/blob/master/results/DSC_1182/flipping_image.jpg width="70%"/>|  



