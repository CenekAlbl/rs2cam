<p align="center"><iframe id="intro_movie" width="256" height="144" src="https://www.youtube.com/embed/SubN2Nralt8" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</p>
<p>
<img src="imgs/patent_pending.jpg" alt="patent pending" style="float: right;"/>

This is a project page dedicated to our CVPR 2020 paper.<br><br>

For more information see the pre-print version of the paper <a href="arxiv.pdf">here</a><br><br>
</p>

# Motivation
Majority of cameras use rolling shutter to capture images and videos today. When a rolling shutter camera moves relative to a scene, such as during hand held video capture or a camera mounted on a vehicle, the images become distorted. These distortions cause visual as well as computational issues.  
  
* Despite almost two decades of research, image distortions caused by camera motion remain an issue.  
* It is common to have multiple cameras on a single device - smartphones, cars, head trackers.
* Few methods focus on using images from two or more cameras mounted on the same device. 
* No previous method is capable of utilizing two RS images captured by cameras that are mounted close together - such as on a smartphone.


  
# Contribution
We present a new way to
* Deal robustly with even extreme RS distortions on multi-camera devices
* Utilize those distortions to compute the motion of the device
* Remove the RS distortions caused by any type of motion
* Improve RS SfM, generate depth maps

All this is facilitated by using different readout directions for each camera. This is easily achievable e.g. by rotating one of the sensors or changing the direction of the electronical shutter.

## The idea
If a device ( e.g. a smartphone) with two RS cameras moves, the images contain distortions. In current devices the cameras both use identical readout directions, which causes the distortions to look identical.

<p align="center">
<img src="imgs/arrow_text_down.png"  height="150"/>
<img src="imgs/phone_identical_no_arrows.jpg" height="150"/>
<img src="imgs/arrow_text_down.png"  height="150"/>
</p>

Our idea is to **roll the shutters in the opposite directions**.  

<p align="center">
<img src="imgs/arrow_text_down.png"  height="150"/>
<img src="imgs/phone_opposite_no_arrows.jpg"  height="150"/>
<img src="imgs/arrow_text_up.png"  height="150"/>
</p>

Having such differences in the distortion allows us to compute the motion of the device from a few sparse correspondences.  
  
The motion parameters can then be used to e.g. undistort the image:  

<p align="center">
<img src="imgs/re_rot4.jpg" height="300" alt="down"/>
</p>

## Examples

Here is another example of identical RS readout directions:

<p float="left" align="center">
<img src="imgs/anim_down.gif"  height="150"/>
<img src="imgs/arrow_text_down.png"  height="150"/>
<img src="imgs/anim_down.gif"  height="150"/>
<img src="imgs/arrow_text_down.png"  height="150"/>
</p>

and opposite directions:

<p float="left" align="center">
<img src="imgs/anim_down.gif"  height="150"/>
<img src="imgs/arrow_text_down.png" height="150"/>
<img src="imgs/anim_up.gif" height="150"/>
<img src="imgs/arrow_text_up.png" height="150"/>
</p>

We can warp both input images and then combine them to obtain a more complete undistorted image:

<p align="center">
<img src="imgs/rot11_combined_res.jpg" height="300"/>
</p>

For smartphones it is typical that one camera has a different FOV due to e.g. a telephoto lens. We show that even such combination can be efficiently used:

<table style="margin: 0px auto;">
	<tr>
		<td>Wide</td>
		<td>Zoom</td>
		<td>Undistorted</td>
	</tr>
	<tr>
		<td><img src="imgs/wide_features.jpg" height="150"/></td>
		<td><img src="imgs/narrow_features.jpg" height="150"/></td>
		<td><img src="imgs/wide_narrow_undist.jpg" height="150"/></td>
	</tr>
</table>

If the motion contains also translation:

<p float="left" align="center">
<img src="imgs/re_tr17_1.jpg" height="150"/>
<img src="imgs/re_tr17_2.jpg" height="150"/>
</p>

We can obtain dense correspondences using e.g. optical flow and using the motion parameters we can compute the depth for each pixel:

<p float="left" align="center">
<img src="imgs/tr17_depth_fused.png" height="150"/>
</p>

And backproject to create and undistorted image: 

<p float="left" align="center">
<img src="imgs/tr17_res.jpg" height="150"/>
</p>

Note that the quality of the result will depend on the quality of the correspondences. The few artefacts are caused by having imperfect optical flow. We did not use any post-processing methods to remove those artefacts.  
  
We can also correct the sparse correspondences and use them in a traditional SfM pipeline to obtain a better 3D reconstruction than with the RS images.

<table style="margin: 0px auto;">
	<tr>
		<td>Sparse features image 1</td>
		<td>Sparse features image 2</td>
		<td>Undistorted sparse features</td>
	</tr>
	<tr>
		<td><img src="imgs/sparse_cam1.jpg" height="150"/></td>
		<td><img src="imgs/sparse_cam2.jpg" height="150"/></td>
		<td><img src="imgs/sparse_undist.png" height="150"/></td>
	</tr>
</table>

And here is the result of the 3D reconstruction for the original RS images, our undistorted ones and images captured by a GS camera:

<p float="left" align="center">
<img src="imgs/sfm.jpg"/>
</p>

## The method
Here is a diagram briefly describing the workflows depending on the motion we want to model.
<p align="center">
<img src="imgs/method_poster_section_6dof.png"/>
&nbsp;&nbsp;&nbsp;&nbsp;
<img src="imgs/method_poster_section_rot.png"/>
</p>

## License
Patent pending. For more information please contact: cenek.albl@gmail.com .

<script>
window.addEventListener('load', function () {
 var w = window.outerWidth;
 if (w > 560)
   document.getElementById("intro_movie").width = "560";
   document.getElementById("intro_movie").height = "315";
 end
});
</script>
