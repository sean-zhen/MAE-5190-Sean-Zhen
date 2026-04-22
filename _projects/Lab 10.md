---
layout: project
title: Lab 10
description: Lab 10
image: /assets/images/Lab10_Plot.jpg
---

<h3>Grid Localization using Bayes Filter</h3>

Below is the code for each function:

<script src="https://gist.github.com/sean-zhen/4da04f952a530669a5055973814c7b62.js"></script>

<script src="https://gist.github.com/sean-zhen/74bc3c789eebf8e74923620625124d3e.js"></script>

<script src="https://gist.github.com/sean-zhen/193121e10e92576c9ae2fad08e07a4be.js"></script>

<script src="https://gist.github.com/sean-zhen/14d869e0695e511d0fa0be7a5f41c946.js"></script>

<script src="https://gist.github.com/sean-zhen/32e230b86bc7bea687e29155d4f3cf17.js"></script>

Below is the video of the grid localization for the sample trajectory:

<iframe width="600" height="337" src="https://youtube.com/embed/6a8X7Z3DtQU" allowfullscreen> </iframe>

<figure>
    <img src="{{ '/assets/images/Lab10_Plot2.jpg' | relative_url }}"
        width="600">
</figure>

<figure>
    <img src="{{ '/assets/images/Lab10_Table.jpg' | relative_url }}"
        width="600">
</figure>

<div style="margin-top: 20px;"></div>
<h3>Discussion</h3>

The Bayes filter works well in predicting the position of the robot but is a very poor predictor of the robot's rotation. It appears that the Bayes filter works well when there are more unique landmarks for the robot to reference. For example, the error seems to be low when the robot is in the center of the map and is able to see multiple sides of the map. When the robot is on the right side of the map, the Bayes filter appears to have the most error since the robot is unable to reference other parts of the world.

<div style="margin-top: 20px;"></div>
<h3>Collaborators</h3>
I used Angela Voo's writeup as a reference. I collaborated with Sam Zhen while working on this lab.