---
layout: page
title: Galaxy Shooter
description: A 2D top down space shooter for Android
img: assets/img/space_shooter/game.jpg
importance: 1
category: completed
---
<h1>Overview</h1>
This was the first game I built in Unity (and ever)! The game was originally built to run on Windows but I later adapted it to work on Android which is where development was focused on. I learned the basics of Unity (animations, UI, handling inputs etc.) through this project. View the complete code is on [GitHub](https://github.com/tarunaygr/Space-Shooter).
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
<li>Power-ups</li>
<li>Multi-platform support</li>
<li>Touch-Screen support</li>
<li>Floating Joystick controls</li>
<li>Animations</li>
<li>Music and Sound</li>
<li>High Score System</li>
<li>Pause Feature</li>
<li>Music and Sound</li>
</ul>
<!-- </div> -->
<h1>Gameplay</h1>
<div class="row justify-content-center">
<video width="720" controls preload='auto'>
  <source src="../../assets/vid/space_shooter.mp4" type="video/mp4">
  <!-- <source src="movie.ogg" type="video/ogg"> -->
  Your browser does not support the video tag.
</video>
</div>
<br>
<h1>Screenshots</h1>

<div class="row">

    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/space_shooter/menu.jpg" title="start screen" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
   Main Menu. The game opens to this screen.
</div>
<div class="row">

    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/space_shooter/game.jpg" title="game screen" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    This is the game screen. Player controls the jet and shoots oncoming fighter jets and scores points.
</div>

<div class="row">

    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/space_shooter/power.jpg" title="pause screen" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    You can collect power ups to get temporary boosts. These include a shield, triple shot to fire 3 shots at once and slow speed of the enemies.
</div>
<div class="row">

    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/space_shooter/damage.jpg" title="pause screen" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Damage taken by the player is seen on the jet. The player can take 2 points of damage before dying. The health is also displayed on the top left corner of the screen.
</div>
<div class="row">

    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/space_shooter/pause.jpg" title="pause screen" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    The pause menu lets you restart the level, go back to the main menu or adjust the sound settings.
</div>
<div class="row">

    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/space_shooter/gameover.jpg" title="pause screen" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    The gameover screen is displayed after the player dies.
</div>

