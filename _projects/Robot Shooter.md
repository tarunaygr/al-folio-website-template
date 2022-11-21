---
layout: page
title: Robot Shooter
description: A third person shooting game with robots
img: assets/img/top_down_shooter/game.jpg
importance: 1
category: completed
---
<h1>Overview</h1>
This was a quick game we made as a demo for a workshop. Then we taught people with no game development experience how to make this in day. You play as a robot and shoot other robots to get points. Game ends when you take 3 points of damage. View the complete code is on [GitHub](https://github.com/tarunaygr/Top-Down-Shooter).
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
<li>Custom Assets</li>
<li>Third Person Camera</li>
<li>Randomised Enemy spawns</li>
<li>Pause Feature</li>


</ul>
<!-- </div> -->
<h1>Gameplay</h1>
<div class="row justify-content-center">
<video width="720" controls preload='auto'>
  <source src="../../assets/vid/top_down_shooter.mp4" type="video/mp4">
  <!-- <source src="movie.ogg" type="video/ogg"> -->
  Your browser does not support the video tag.
</video>
</div>
<br>
<h1>Screenshots</h1>

<div class="row">

    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/top_down_shooter/game.jpg" title="start screen" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
  You control the robot in the middle and fire bullets at enemy robots. Maximum of 3 robots spawn at a time.
</div>
<div class="row">

    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/top_down_shooter/damage.jpg" title="game screen" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    The health is reflected at the top left of the screen. You can take 2 points of damage before you die and the game ends.
</div>

<div class="row">

    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/top_down_shooter/pause.jpg" title="pause screen" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Pressing the escape key at anytime pauses the game and opens the pause menu. You can restart of end the game from here.
</div>
<div class="row">

    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/top_down_shooter/gameover.jpg" title="pause screen" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    After the player dies, the game over screen pops up. The player can restart the game or quit from here.
</div>
