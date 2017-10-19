# 翁啟文 <span style="color:red">(103061112)</span>

# HW0 / Pixel array manipulation

## Overview
The project is related to two methods of image processing: flipping and rotation operations. To implement these two operations, we write two functions: flip.m and rotation.m. Then we call both functions by lab0_main.m to run the processing in Matlab. Below are the implementation details.

> flip.m  
> rotation.m  
## Implementation
1. flip.m:  
	### Matlab code:
	In **flip.m**, there is a function **I_flip()** inside. The function has two argument: one is an image array with 3 channels (R, G and B); the other is operating type (0: horizontal flipping, 1: vertical flipping and 2: horizontal+vertical flipping). In the beginning, the function saves R, G and B from the input image array, and gets the height and width of the image:
	``` Matlab
	% RGB channel
	R = I(:,:,1);
	G = I(:,:,2);
	B = I(:,:,3);
	% get height, width, channel of image
	[height, width, channel] = size(I);
	```
	Next, use **if statement** to judge which operating type should be apply:
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
	*  horizontal flipping (type 0): 
	*  vertical flipping (type 1):
	*  horizontal+vertical flipping (type 2):
	### Results

		<table border=1>
		<tr>
		<td>
		<img src="placeholder.jpg" width="24%"/>
		<img src="placeholder.jpg"  width="24%"/>
		<img src="placeholder.jpg" width="24%"/>
		<img src="placeholder.jpg" width="24%"/>
		</td>
		</tr>

		<tr>
		<td>
		<img src="placeholder.jpg" width="24%"/>
		<img src="placeholder.jpg"  width="24%"/>
		<img src="placeholder.jpg" width="24%"/>
		<img src="placeholder.jpg" width="24%"/>
		</td>
		</tr>

		</table>


