### Training Very Deep Networks

文章大概摘要：受LSTM的启发，提出highway网络，使用可适应的门单元控制信息流，以达到能够训练非常深的网络。

公式
$y = H(x,W_H)· T(x,W_T) + x · (1 − T(x,W_T))$

Attention的起源
