前面所有層與後面層連接
DenseNet是直接concat來自不同層的特徵圖，可以實現特徵重用，提升效率，特點是網絡與ResNet，是這唯一的區別。
![densenet](DenseNetPic/Dnp1.jpg)

block內，每一層輸出的特徵圖，特徵通道串聯concatenate，作為下一層的輸入；
其中，每一層的複合激活函Composite Function為：H(x) = BN + ReLU + Conv；
![](DenseNetPic\DB.png)
