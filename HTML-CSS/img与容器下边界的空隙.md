## img与容器下边界的空隙（缝隙）的解决方法

### 造成该问题的原因
图片、文字等inline或inline-block元素默认是和父级元素的baseline对齐的，而baseline又和父级底边有一定距离
（这个距离和 font-size，font-family 相关），所以设置 vertical-align:top/bottom/text-top/text-bottom
都可以避免这种情况出现。而且不光li，其他的block元素中包含img也会有这个现象。

### 解决办法
    1.定义图片img标签vertical-align:bottom，vertical-align:middle，vertical-align:top

    2.设置img的display:block

    3.设置父元素的line-heignt：0

    4.设置父元素的font-size：0
