**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.


My pipeline consists of the following steps:

- convert image to grayscale
- apply gaussian blur filter
- use canny transform to detect edges
- select a region of interest where to look for lanes
- apply hough lines transform to detect the lanes

In order to draw a single line on the left and right lanes, I modified the draw_lines():

- to ignore line segments that have a slope <= 0.5 or >= 0.85 (this is needed in order to make it work with the challenge video)
- to group line segments into left and right by checking the sign of the slope (negative slope -> right, positive slope -> left)
- to average the left and right groups separatelly and extract the slope and the bias from the average
- to apply a weighted exponential average on the averaged extracted parameters from previous step
    - using beta=0.95 translates to average on the last ~20 frames
    - this stabilizes the drawn lines which would normally shake
- to extrapolate the result from previous step in order to output lines given a top and a bottom limit (the bottom of the image)


### 2. Identify potential shortcomings with your current pipeline


By introducing the weighted exponential average I'm preventing lines shake but, because of this, they react slower to change.
The good part is that this can be tweaked using the beta parameter.
I also had to introduce two global parameters (right_params and left_params). Normally, I wouldn't use global variables in python but, in order to prevent too much refactoring, I decided to use this hacky solution for now.

The line segments filtering based on hardcoded values for the slope (0.5 and 0.85) where chosen by me by briefly looking at tipical values.

### 3. Suggest possible improvements to your pipeline

I would refactor the code to get rid of the global variables. This could be avoided by passing the variables to the pipeline.
The line segments filtering based on the slope could be improved by analysing more lanes and extracting the suitable interval from there.
