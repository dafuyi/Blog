## 给 typecho 加上背景

## 代码如下

### 代码一
```html

<div style="background-size: cover;background-repeat: no-repeat;background-image: url(<!--这里填入图片地址-->);position:fixed;height: 100%;width: 100%;z-index: -999;"></div>

```

实际上插入的代码应该是：

```html

<div style="background-size: cover;background-repeat: no-repeat;background-image: url(http://image.example.comm/test.jpg);position:fixed;height: 100%;width: 100%;z-index: -999;"></div>

```

#### 代码二
```css
#header,#body,#footer{
    background-color: rgba(255,255,255,.8) !important;
}
```
## 使用方法

进入博客的根目录下的，`usr/themes/defaut`目录找的`header.php`文件。

![](http://image.yidaqiang.cn/blog/typecho/path-change-background-image.png)

打开编辑。如下图所示加入上面的代码。

![](http://image.yidaqiang.cn/blog/typecho/change-background-header-file.png)

在同一级目录找到`style.css`。

![](http://image.yidaqiang.cn/blog/typecho/path-to-style-css.png)

把代码二插入文件的末尾。

![](http://image.yidaqiang.cn/blog/typecho/change-style-css.png)

## 最后效果

![](http://image.yidaqiang.cn/blog/typecho/final-result.png)
