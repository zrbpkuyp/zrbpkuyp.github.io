---
layout: page
title: GPA Miner
description: a self-defined game
img: assets/img/gpaminer.png
importance: 3
category: work
---


    ---
    GPA miner is the final project of AI Game class.
    ---

Basic Settings:
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/miner_rule.png" title="rule" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/miner_item.png" title="item" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

Below are three versions of AI, using different strategies.
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/gif/miner_01.gif" title="easy" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/gif/miner_02.gif" title="middle" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/gif/miner_03.gif" title="difficult" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    EASY | MIDDLE | DIFFICULT
</div>

    ---
    EASY:
    1. If there is an item at the angel, release the hook.
    2. Can not release the hook twice in a row at the same angel.
    ---

    ---
    MIDDLE:
    1. Include 'easy' strategies.
    2. If there is '+X', fetch it first.
    3. If an item is fetched by the other player, drop it.
    ---

    ---
    DIFFICULT:
    1. Include 'middle' strategies.
    2. use an value function to determine which item to get first, 
       based on the item's value and the time to get it.
    3. Tell whether the item can be reached first by the other player, if so, drop it.
    ---

    ---
    RL:(haven't realized yet)
    use DQN
    organize the whole situation as the input
    ---