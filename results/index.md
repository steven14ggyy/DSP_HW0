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
	
	_ |type0 | 
	  :-----:|:-----:|
	Orignial image|  <img src=https://github.com/steven14ggyy/DSP_Lab_HW0/blob/master/results/image/image.jpg width="70%"/> |
	Processed image| <img src=https://github.com/steven14ggyy/DSP_Lab_HW0/blob/master/results/image/flipping_image.jpg width="70%"/> | 
	
	 _ |type1| type2 |
	:-----:|:-------:|:------:|	
	 Orignial image|<img src=https://github.com/steven14ggyy/DSP_Lab_HW0/blob/master/results/DSC_0531/DSC_0531.JPG width="70%"/> | <img src=https://github.com/steven14ggyy/DSP_Lab_HW0/blob/master/results/DSC_1182/DSC_1182.JPG width="70%"/>|  
	 Processed image|<img src=https://github.com/steven14ggyy/DSP_Lab_HW0/blob/master/results/DSC_0531/flipping_image.jpg width="70%"/> | <img src=https://github.com/steven14ggyy/DSP_Lab_HW0/blob/master/results/DSC_1182/flipping_image.jpg width="70%"/>|  
	 
2. rotation.m:  
	 In rotation.m, images will be rotation clockwise by an angle we assign in radian. The function has two arguements: One is an input image we want to process; the other is an angle (unit: radian).  
	 So same as in flip.m, we get **R**, **G** and **B**, 3 channel pixel values, and the height and width of the input image. Then  calculate locations of the new vertices to determine new image size after rotating. Use a rotation matrix multiplication and find the minimum x and y value, which is how much to shift the rotationed image to the positive axis. Calculate the difference of minimum x and maximum x to get the new width of the new image, and do the same thing on y to get the new height of the new image.  
	 ```Matlab
	 % RGB channel
	R(:,:) = I(:,:,1);
	G(:,:) = I(:,:,2);
	B(:,:) = I(:,:,3);

	% get height, width, channel of image
	[height, width, channel] = size(I);

	%% create new image
	% step1. record image vertex, and use rotation matrix to get new vertex.
	matrix = [cos(radius) -sin(radius) ; sin(radius) cos(radius)];
	% combine four vertices in a matrix: (1,1), (1, width), (height, 1) and (height, width)
	vertex = [1, width, width, 1; 1, 1, height, height]; 
	vertex_new = matrix*vertex;

	% step2. find min x, min y, max x, max y, use "min()" & "max()" function is ok
	min_x = min(vertex_new(1,:)); 
	min_y = min(vertex_new(2,:));
	max_x = max(vertex_new(1,:));
	max_y = max(vertex_new(2,:));

	% step3. consider how much to shift the image to the positive axis
	x_shift = -min_x;
	y_shift = -min_y;

	% step4. calculate new width and height, if they are not integer, use
	% "ceil()" & "floor()" to help get the largest width and height.
	width_new = ceil(max_x) - floor(min_x);
	height_new = ceil(max_y) - floor(min_y);

	% step5. initial r,g,b array for the new image
	R_rot = zeros(height_new, width_new);
	G_rot = zeros(height_new, width_new);
	B_rot = zeros(height_new, width_new);
	 ```
	 After getting the new size, we have to find pixel values (R, G and B) to show correct color arrangement on the screen. Use backward-warping, which means shifting and rotationing back to get corresponding color values in the original input image, so we need to shift points back and multiply each cooridinate of each point in the new image with the inverse of the rotation matrix. 
	 ```Matlab
	 location_new = [x_new;y_new];
         location_old = inv(matrix)*(location_new - [x_shift; y_shift]);
	 x_old = location_old(1,1);
         y_old = location_old(2,1);
	 ```
	 By bilinear interpolation, we could obtain close right pixel values. Find two integers that sit at the both sides of the point calculated in the previous backward-warping step, so we get 4 interpolation coordinates: **w1(x1, y1)**, **w2(x2, y1)**, **w3(x1, y2)** and **w4(x2, y2)**
	 ```Matlab
	 x1 = floor(x_old);
         x2 = ceil(x_old);
         y1 = floor(y_old);
         y2 = ceil(y_old);
	 ```
	 Fill the point "black color" if the backward-warping point is outside of the input image. If the point is inside the input image, we calculate weights of above 4 interpolation coordinates and sum them up to get the pixel values. Note that if x1 = x2 or y1 = y2, it means our **"bilinear interpolation"** degenerates into 1-D linear interpolation (w1 = w2 and w3 = w4 in **"x1=x2"** case; w1 = w4 and w2 = w3 in **"y1=y2"** case), so we just set wa = 0 or 1



