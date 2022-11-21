---
layout: page
title: Assembly Game Simulation
description: A simulation game made under Dr. Vinay Panicker at NITC
img: assets/img/assembly/start.jpg
importance: 1
category: WIP
---
<h1>Overview</h1>
The goal of the project was to build a simulation where subjects build pens from their components. The study measures the impact of customised workspaces on the productivity of employees. The subjects build the pens in a number of scenarios and the reaction time and other metrics are measured. View the complete code is on [GitHub](https://github.com/tarunaygr/Assembly-Game).
<!-- Every project has a beautiful feature showcase page.
It's easy to include images in a flexible 3-column grid format.
Make your photos 1/3, 2/3, or full width.
-->
<!-- To give your project a background in the portfolio page, just add the img tag to the front matter like so:  -->
<!-- 
    ---
    layout: page
    title: project
    description: a project with a background image
    img: /assets/img/12.jpg
    ---
 -->

<h1>Key Features</h1>

<ul>
<li>Drag and drop objects</li>
<li>Store information such as response time, time to complete etc. in csv file after each level </li>
<li>Custom pen parts</li>
<li>Randomization of level order</li>

</ul>
<!-- </div> -->
<h1>Gameplay</h1>
<div class="row justify-content-center">
<video width="720" controls preload='auto'>
  <source src="../../assets/vid/assembly.mp4" type="video/mp4">
  <!-- <source src="movie.ogg" type="video/ogg"> -->
  Your browser does not support the video tag.
</video>
</div>
<br>
<h1>Screenshots</h1>

<div class="row">

    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/assembly/start.jpg" title="start screen" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
  Start Screen of the simulator.
</div>
<div class="row">

    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/assembly/consent.jpg" title="game screen" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
   The simulation asks for consent to record data from the participant.
</div>

<div class="row">

    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/assembly/details.jpg" title="pause screen" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
   Participants enters their details.
</div>
<div class="row">

    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/assembly/3.jpg" title="pause screen" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    The levels are in random order. Level 3 clubs the parts according to their type. Each level has a time limit.
</div>
<div class="row">

    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/assembly/5.jpg" title="pause screen" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    The levels are in random order. Level 5 hides the parts and provides a description of the parts instead. (not shown)
</div>
<div class="row">

    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/assembly/2.jpg" title="pause screen" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Level 2 shows an image of the parts to refer and no written instructions.
</div>
<div class="row">

    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/assembly/survey.jpg" title="pause screen" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    A survey screen shows up after each level to collect feedback from the participant about the level they just completed.
</div>
