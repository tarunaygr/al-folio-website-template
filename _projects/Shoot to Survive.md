---
layout: page
title: Shoot to Survive
description: A first person shooter game made in Unity
img: assets/img/fps/damage.jpg
importance: 1
category: WIP
---
<h1>Overview</h1>
This is my first big project I am working on. I've been working on it on and off for a year. The game is to shoot AI enemies and gain points. The premise is simple but it showcases more technical gameplay mechanisms. The game is still WIP, so there are missing textures and models right now. They will be added in the futurre with more features! View the current code is on [GitHub](https://github.com/tarunaygr/Shoot_to_Survive).
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
<li>First person controller and movements from scratch</li>
<li>AI enemies</li>
<li>Gun animations</li>
<li>Particle Effects for blood</li>
<li>Raycasts for firing guns</li>


</ul>

<h1>Gameplay</h1>
<div class="row justify-content-center">
<video width="720" controls preload='auto'>
  <source src="../../assets/vid/fps.mp4" type="video/mp4">
  <!-- <source src="movie.ogg" type="video/ogg"> -->
  Your browser does not support the video tag.
</video>
</div>
<br>
<h1>Screenshots</h1>

<div class="row">

    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/fps/damage.jpg" title="start screen" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
  You have a pistol(more guns coming) and kill enemies with it (models to be added). You have a life bar and 3 lives. Movement includes running, jumping, crouching.
</div>


<div class="row">

    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/fps/pause.jpg" title="pause screen" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Pressing the escape key at anytime pauses the game and opens the pause menu. You can restart of end the game from here.
</div>
<div class="row">

    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/fps/gameover.jpg" title="pause screen" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    After the player dies, the screen fades to black and the game over screen pops up. The player can restart the game or quit from here.
</div>
