---
layout: page
title: NoGo
description: a NoGo bot using MCTS   
img: assets/img/go.jpg
importance: 1
category: work
---

    ---
    This is one of final project of Computing Generality A. 
    Actually, it's not the one when I took the lesson. 
    But I have a great interest in it, so I have a simple try using the method MCTS.
    Below is something about it.
    ---

Below is one game I (white side) played with the AI master (black side).
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/nogo_01.png" title="nogo beginning" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/nogo_02.png" title="nogo middle" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/nogo_03.png" title="nogo ending" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<div class="caption">
    Setting: (Python) searching time is limited to 3 seconds.
</div>

We can see that the AI master is 'stupid' at the beginning. But as the game goes to the middle, it begins to act as an expert, attempting to disrupt my intentions.

The code has been uploaded to [github](https://github.com/zrbpkuyp/NoGo). Below is the main framework.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/mcts_frame.png" title="mcts framework" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<div class="caption">
    This is the frame work of MCTS method, for more information, you can go to [CSDN](https://blog.csdn.net/weixin_45828785/article/details/102879695)
</div>


{% raw %}
```Python
# examine whether the location is in board, return True or False
def in_board(x, y) 

# build the monte carlo tree
class TreeNode:
    # init the basic varibles
    def __init__(self, _parent, _board, _color)
    # examine whether the piece has air
    def dfs_air(self, fx, fy)
    # judge whether the location is available
    def judge_available(self, fx, fy, col)
    # return all the actions that are available
    def get_valid_action(self)

# choose the best child, return the node of the best child
def best_child(node)

# expand the child, return the new node just expended
def expand(node)

# give a certain score based on the current situation, return the score
def default_policy(node)

# give the score to its parent, do not return
def backup(node, r)

# the combination of choosing the best child and expend
def tree_policy(node)

# combination of the whole process
def monte_carlo_tree_search(node)

{% endraw %}