自动化的QQ游戏---连连看...
进入游戏界面后运行代码,然后双手离开鼠标和键盘...
over

博客链接在这里 : https://blog.csdn.net/jnxxhzz/article/details/81275542

打包好的.exe可执行文件在这里 :  
_______________________________________________________________________________

1.使用的扩展库
python3.6 + opencv-python + pywin32 + PIL + numpy

若缺失库,这里给出安装命令

pip install opencv-python

pip install pypiwin32

pip install PIL

pip install numpy

若无法安装请自行百度

_______________________________________________________________________________

2.思路
    1)通过win32定位游戏窗体,并获取截图

    2)从截图中定位需要pending的游戏区域

    3)将图片切片成二维单个方块的Img地图,并将方块分类编号

    4)将Img地图转换成二维编号矩阵

    5)处理是否可以消除并模拟点击


_______________________________________________________________________________

3.思考
首先这个代码肯定有许多的不足之处:

1)  窗体大小可修改

    当然如果要解决这个问题也不难,定位窗体时会获得pos[4]四个参数,左上角和右下角的坐标,一般来说游戏界面是等比例放大的,所以只要计算具体的游戏区域占整个窗体的百分比即可 
    
    如果存在上面的问题,那么每个方块的大小也需要修改 ,修改方法也不是很麻烦...同样的首先获取游戏区域的像素大小,不管游戏如何放大,游戏区域内能放下的方格矩阵长宽是不会变的,比如QQ游戏就是11 * 19 , 所以每个方块的大小就用游戏区域的像素大小除一下就行了,边缘处理也可以简单的使用60%,至于为什么是60%....自己想吧

2)  若空白块变成渐变色的问题 /  方块不完全相同  / 同种周围存在某些小特效导致pending结果为不同

    其实也不难,这里判断两个方块是否相同的方式是通过两个图像的标准差为0,那么如果存在渐变色,那么我们只要计算出一个误差上限即可,标准差在误差内,我们就认为它们是同一种方块,这个问题就跟上面的60%一样,因为我们这里一定要标准差为0才认为两个方块是一样的,所以pending方块不能有一点误差,所以需要减掉的边缘需要大一些,那么如果不强制要求100%相似度判断的话,那自然也不需要减掉那么多边缘,因为边缘减掉的越多,也会造成另一种误差

