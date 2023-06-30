---
layout: page
title: Chinese Standard Mahjong
description: a Mahjong bot using supervised learning
img: assets/img/mahjong.jpg
importance: 2
category: work
---

    ---
    This is one of the projects of Introduction to AI - building an AI master that can play Chinese Standard Mahjong.
    Thanks to mentor Wenxin Li and TA Yunlong Lu, with the training data and code framework they provided, I try to train my own AI.
    My AI is mainly based on supervised learning, and I make efforts on reinforcement learning, but it doesn't work well.
    The project is tested on [Botzone](https://botzone.org.cn/)
    ---

Below is one game on Botzone.
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/botzone_01.png" title="mahjong beginning" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/botzone_02.png" title="mahjong middle" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/botzone_03.png" title="mahjong ending" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Each choice should be made in one seconds.
</div>

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/test_01.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Each choice should be made in one seconds.
</div>

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/test_02.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Each choice should be made in one seconds.
</div>

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/test_03.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Each choice should be made in one seconds.
</div>

{% raw %}

```python
# ResNet 18
# Input is a dict, input_dict["obs"]["observation"](shape:34*4*9) is what we need
class ResidualBlock(nn.Module):
    def __init__(self, inchannel, outchannel, stride=1):
        super(ResidualBlock, self).__init__()
        self.left = nn.Sequential(
            nn.Conv2d(inchannel, outchannel, kernel_size=3, stride=stride, padding=1, bias=False),
            nn.BatchNorm2d(outchannel),
            nn.ReLU(inplace=True),
            nn.Conv2d(outchannel, outchannel, kernel_size=3, stride=1, padding=1, bias=False),
            nn.BatchNorm2d(outchannel)
        )
        self.shortcut = nn.Sequential()
        if stride != 1 or inchannel != outchannel:
            self.shortcut = nn.Sequential(
                nn.Conv2d(inchannel, outchannel, kernel_size=1, stride=stride, bias=False),
                nn.BatchNorm2d(outchannel)
            )

    def forward(self, x):
        out = self.left(x)
        out += self.shortcut(x)
        out = F.relu(out)
        return out

class ResNet(nn.Module):
    def __init__(self, ResidualBlock, num_classes=235):
        super(ResNet, self).__init__()
        self.inchannel = 64
        self.conv1 = nn.Sequential(
            nn.Conv2d(34, 64, kernel_size=3, stride=1, padding=1, bias=False),
            nn.BatchNorm2d(64),
            nn.ReLU(),
        )
        # followed 4 blocks
        self.layer1 = self.make_layer(ResidualBlock, 64,  2, stride=1)
        self.layer2 = self.make_layer(ResidualBlock, 128, 2, stride=1)
        self.layer3 = self.make_layer(ResidualBlock, 128, 2, stride=1)
        self.layer4 = self.make_layer(ResidualBlock, 64, 2, stride=1)
        self.fc = nn.Linear(64*4*9, num_classes)

    def make_layer(self, block, channels, num_blocks, stride):
        strides = [stride] + [1] * (num_blocks - 1)
        layers = []
        for stride in strides:
            layers.append(block(self.inchannel, channels, stride))
            self.inchannel = channels
        return nn.Sequential(*layers)

    def forward(self, input_dict):
        self.train(mode = input_dict.get("is_training", False))
        obs = input_dict["obs"]["observation"].float()
        action_logits = self.conv1(obs)
        action_logits = self.layer1(action_logits)
        action_logits = self.layer2(action_logits)
        action_logits = self.layer3(action_logits)
        action_logits = self.layer4(action_logits)
        action_logits = action_logits.view(action_logits.size(0), -1)
        action_logits = self.fc(action_logits)
        action_mask = input_dict["obs"]["action_mask"].float()
        inf_mask = torch.clamp(torch.log(action_mask), -1e38, 1e38)
        return action_logits + inf_mask


def ResNet18():
    return ResNet(ResidualBlock)
```

{% endraw %}