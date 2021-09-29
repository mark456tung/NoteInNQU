
## 什麼是卷積
滑動 + 內積，利用 filter 在input image上滑動得出的feature map
綠底為input 黃色為filter 
![Convolution](DenseNetPic\Convolution.gif)
圖片取自[[ 機器學習 ML NOTE ] Convolution Neural Network 卷積神經網路](https://medium.com/%E9%9B%9E%E9%9B%9E%E8%88%87%E5%85%94%E5%85%94%E7%9A%84%E5%B7%A5%E7%A8%8B%E4%B8%96%E7%95%8C/%E6%A9%9F%E5%99%A8%E5%AD%B8%E7%BF%92-ml-note-convolution-neural-network-%E5%8D%B7%E7%A9%8D%E7%A5%9E%E7%B6%93%E7%B6%B2%E8%B7%AF-bfas8566744e9)
## 什麼是pooling?
對一定範圍內的數值做pooling可以達到降維且保留重要特徵的效果
常見的pooling有 maxpooling meanpooling minpooling
![maxpooling](DenseNetPic\maxpool.gif)
## Batch Normalization
以 mini-batch 為單位，依照各個 mini-batch 來進行正規化到平均值為0、標準差為1的常態分佈，如此一來可以將分散的數據統一，有助於減緩梯度消失以及解決 Internal Covariate Shift 的問題，同時可以加速收斂。
使用時機:
遇到收歛速度很慢，或梯度爆炸等無法訓練的狀況時可以嘗試
在一般使用情况下也可以加入，用來加快訓練速度，提高模型效能。
![](DenseNetPic\BN.png
)
下圖左為無Normaliztion  
![](DenseNetPic\BN2.png)
最大好處就是讓每一層的值在有效的範圍內傳遞下去。
![](DenseNetPic\BN2.jpg)



## 參考
* https://ithelp.ithome.com.tw/articles/10204106
* https://medium.com/ching-i/batch-normalization-%E4%BB%8B%E7%B4%B9-135a24928f12
