## Hawk Eye
### Automatic Birds Eye View Registration of Sports Videos

#### Course Project for CS 6476: Computer Vision (Prof. Devi Parikh) @ Georgia Institute of Technology

#### Further implementation details can be found at: [https://nihal111.github.io/hawk_eye/](https://nihal111.github.io/hawk_eye/)
 
#### Description
The sports broadcasting viewing experience has essentially remained unchanged for decades. A camera position on the side of the playing field which pans according to where the focus of the game is at that moment. Tactics and strategies for sports are largely depicted on the top-view of game field. For a lot of sport analytics to happen, we need to convert the camera view broadcast stream to a top view that helps understand player movements in-game. This is typically achieved with an expensive multi-camera set up around the field. The views from these cameras are then annotated by hand and converted into a top view. This limits the use to only those that can happen after a match has been broadcast. However, if this process is automated, it would open up multiple avenues of analytics and sports viewing that can occur concurrently with the match that is currently being broadcasted. We use Computer Vision techniques to solve this problem in a semi-supervised manner. We test our method specifically for soccer data but it can be adapted to similar sports like basketball and hockey. Our project pipeline takes camera view images and broadcasts them to a top view edge map while also identifying player positions from both teams.

#### Methodology
##### Generating top view edge maps
We generate homographies between images from particular camera views to the corresponding location in the top view field, with the help of a football field edge map blueprint (which will be in the form of an edge map representing a football field) and create a large database of such edge maps along with homography matrices using camera perturbations. Given a database of homographies that map to the top view and camera views of football fields, we can reconstruct the top view of camera views. We introduce the concept of an edge map blueprint of a football field, which will help us map new camera views to their top views. An edge map is also better suited for sports analytics compared to raw image top views. The inverse of the homographies in our initial database will generate camera views from our inital blueprint. These edge maps will then be used to train a Pix2Pix conditional Generative Adversarial Network to map input camera view images to their corresponding edge maps.<br> 
Due to the small size of our existing homography database; we augmented it with camera perturbations such as pan, tilt and zoom; making the dictionary more usable for training Pix2Pix. With the dictionary complete and the Pix2Pix model completely trained, we can input an unseen frame from the test dataset and retrieve back the closest edge maps. Histogram of gradients (HoG) is applied on all edge maps in the dictionary, and using the HoG, a KNN model is trained and used for querying. We only use the closest match (i.e. nearest neighbour) to find the homography for the query image. With the best homography matrix for the query image identified, we can transform the camera view into the closest approximation of its corresponding top view edge map. 

##### Player mapping
The players first have to be identified in the camera view. This is done by first identifying the field in the soccer image by identifying all the green pixels in a particular range. The identified pixels are used as a mask for the field which is then separated from the remaining part of the image, leaving only the players and part of the background. We do morphological closing operations on the filtered image, which will fill out the noise present in the crowd, reducing the possibility of false detections. We then check for contours in the image. For each detected contour, we check if the height is more than the width (at least 1.5 times) which will be our detected players. Further, we resized each detected sub-image of a player to 30x30 pixels and applied K-Means with 2 clusters on all these sub-images. The 2 most detected pixel colours will be the colors of the teams’ jerseys. For each sub-image we search for these two pixel colours and then assign the player to the colour that was dominant. With the player and team identified, they can be mapped to the top view accurately.

#### Team Name: Bijon Mustard
#### Team Members
- Nihal Singh @ https://github.com/nihal111
- Rohit Gajawada @ https://github.com/rohitgajawada
- Sahith Dambekodi @ https://github.com/SND96
- Monica Gupta @ https://github.com/monica1244

#### Project presentation
 Slides: https://docs.google.com/presentation/d/1zinlch7VtyuMFU1dZtBBaVbu1QHmc5oL3z5CUm7ywN4/edit?usp=sharing<br>
 Video: https://youtu.be/Jqh7Ce2R1Kk
