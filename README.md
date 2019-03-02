{
 "cells": [
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Traffic Sign Classification"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# DataSet Exploration "
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Dataset exploration id performed in cell number 1 to 8\n",
    "\n",
    "In cell number 1 data is imported from the location in the PC, there were \n",
    "three pickle files containing datafiles, for train, test and validation.\n",
    "THe images and the labels for train, test and validation is separated.\n",
    "\n",
    "\n",
    "In Cell number two we are checking the shape of data to understand the \n",
    "volume of data and the proportion of train, test and validation set.\n",
    "\n",
    "<br><br><table>\n",
    "<tr><td>Train Set</td><td>(34799, 32, 32, 3)</td></tr>\n",
    "<tr><td>Test Set</td><td>(12630, 32, 32, 3)</td></tr>\n",
    "<tr><td>Validation Set</td><td>(4410, 32, 32, 3)</td></tr>\n",
    "</table>\n",
    "<br>\n",
    "\n",
    "There are total 43 class images of traffic signs.\n",
    "\n",
    "In cell number 6, 5 randomly selected images from each class are visalized.\n",
    "Analysis shows that, some of the images are completely balck and are not \n",
    "very sharp, images in many classses are blurred.\n",
    "A snippet of that is shown below - \n",
    "\n",
    "<img src=\"writeupimages/dataset_exploration.png\">\n",
    "\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "In cell number 8 A countplot of images in all classes in train, test and \n",
    "validation data is plotted.\n",
    "The plot shows that the proportion of images in all the three sets in\n",
    "respective classes is same BUT the count of images in all the classes is \n",
    "not uniform, In class 1,2,4,5,10,12,13,25,38 is much higher where as in \n",
    "classes 0,6,19,20,21,22,23,24,27,29,32,34,35,36,37,39,40,41,42,43  are \n",
    "very low.\n",
    "\n",
    "Analysis - Image augementation in specific classes in train data can help \n",
    "in making the count of images in all the classes uniform."
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "<img src=\"writeupimages/countplot_1.PNG\">"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Design and Test Model Architecture"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "The Designing and testing of model is implemented in cell number 9  to cell number 12"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Preprocessing"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "1. Scaling\n",
    "\n",
    "All the images are scalled by dividing the pixel values by 255 so that pixel values can range from 0 to 1\n",
    "<br><br>\n",
    "\n",
    "2. Image Augementation\n",
    "\n",
    "In order to make sure that the number of images in each label is similar,\n",
    "image augementation needs to be done.\n",
    "\n",
    "In cell number 9 three functions are created,\n",
    "\n",
    "In function increase_brightness() with every image we are adding a value between 0 to 0.5 choosen randomly.\n",
    "This should increase brightness of the image\n",
    "\n",
    "In function random_rotate(), the images are rotated to random angle between -20 to 20 using\n",
    "openCV.\n",
    "Using cv2.getRotationMatrix2D(), the rotation matrix M is obtained, and using \n",
    "cv2.warpAffine() function rotated image is obtained.\n",
    "\n",
    "In function random_zoom() image zooming is done. First the image is resized to a random\n",
    "size of originial image with zoomsize between 1.2 to 2.0.\n",
    "then from center of the image image of size 32x32 is cropped to obtain the \n",
    "zoomed image of original size.\n",
    "\n",
    "Zoom, brightness and slight random rotate should not change the original meaning of sign.\n",
    "Initially I though of usig flip, but that would change the meaning of some\n",
    "of the traffic signs.\n",
    "A higher rotation angle will also impact the sign meaning, so a lower angle is used.\n",
    "\n",
    "<br><br>\n",
    "\n",
    "In cell number 10 the Image augementation is applied.\n",
    "On labels having less than 750 iamges, the augementaion method is applied.\n",
    "for labels having less than 250 images, 750 new augemented images will be added.\n",
    "for labels having less than 500 and greated than 250 images, 500 new augemented images will be added.\n",
    "for labels having less than 750 and greated than 500 images, 250 new augemented images will be added.\n",
    "\n",
    "\n",
    "After performing augementation, the labels have good volume and uniform number of images.\n",
    "\n",
    "<img src=\"writeupimages/countplot_2.PNG\">"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "After performing above step now the training set has shape (48799, 32, 32, 3) "
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Model Architecture"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "The lenet architecure is modified to achieve the requried result.\n",
    "Follwoing is the architecture used in the current model to achieve results\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "<img src=\"writeupimages/model_architecture.png\">"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Training the model\n",
    "\n",
    "\n",
    "The model is trained for 50 epochs\n",
    "In cell number 15 x and y as feature set and label set placeholders are created.\n",
    "the target is onehot encoded as per the model architecture.\n",
    "In cell number 16 \n",
    "the cost function for softmax function is creted and then the adam optmizier with learning rate 0.00 is used.\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "In Cell number 17 the function for evaluation of model is created."
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "In cell number 18 we are training the model.\n",
    "images in batch size of 128 are loaded and the model is trianed till 50 epochs.\n",
    "In every epoch we are printing the accuracy of model on validation data.\n",
    "In 50th epoch finally we could get 0.96 accuracy on validation data and 0.999 accuracy on trianing data"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "In cell number 19 we are calculating the accurayc of model on test data and it is 0.94"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Testing the model on new images"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "10 new images from google search are collected to for testing\n",
    "<table>\n",
    "<tr><td><img src=\"Web_images/1.jpg\"></td><td><img src=\"Web_images/2.jpg\">\n",
    "</td><td><img src=\"Web_images/3.jpg\"></td><td><img src=\"Web_images/4.jpg\">\n",
    "</td><td><img src=\"Web_images/5.jpg\"></td></tr>\n",
    "<tr><td><img src=\"Web_images/6.jpg\"></td><td><img src=\"Web_images/7.jpg\">\n",
    "</td><td><img src=\"Web_images/8.jpg\"></td>\n",
    "<td><img src=\"Web_images/9.jpeg\">\n",
    "</td><td><img src=\"Web_images/10.jpg\">"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "In cell number 24 the labels are assigned based on observation from images.\n",
    "<br>new_labels = [33,14,34,27,3,13,9,1,4,25]\n",
    "\n",
    "In cell number 25 we are checking the performance of model on these new images which is\n",
    "0.80 obtained here."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": []
  }
 ],
 "metadata": {
  "kernelspec": {
   "display_name": "Python 3",
   "language": "python",
   "name": "python3"
  },
  "language_info": {
   "codemirror_mode": {
    "name": "ipython",
    "version": 3
   },
   "file_extension": ".py",
   "mimetype": "text/x-python",
   "name": "python",
   "nbconvert_exporter": "python",
   "pygments_lexer": "ipython3",
   "version": "3.6.7"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 2
}
