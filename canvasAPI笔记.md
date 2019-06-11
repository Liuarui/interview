# 基本

```JavaScript
window.onload = function () {
            var dw = document.getElementById("abc").getContext("2d");;
            //画直线
            // dw.moveTo(0, 50);
            // dw.lineTo(400, 50);
            // dw.stroke();
            // dw.beginPath();
            // dw.lineWidth = 10;
            // dw.strokeStyle = "#ccc";
            // dw.moveTo(450, 50);
            // dw.lineTo(100, 250);
            // dw.stroke();

            //画圆
            //arc(x,y,r, startAngle ,endAngle ,counterclockwise)
            // dw.arc(200,200,50,0,2*Math.PI,false);
            // dw.stroke();
            //画圆弧
            // a点坐标取上次绘制终点
            //arcTo(bx，by，cx，cy，圆弧半径)

            //贝塞尔曲线
            //起点坐标都取上次绘制最后的点，如需更换可以使用beginPath(),或moveTo()
            // 二次方  quadraticCurveTo(控制点x，控制点y，终点x，终点y)
            //三次方  bezierCurveTo(控制点1x，控制点1y，控制点2x，控制点2y，终点x，终点y)

            // 绘制文本
            // 直接往括号里加要绘制的文字
            //相关属性，font textAlign textBaseline
            //有填充fillText()
            //无填充strokeText()

            //线性渐变
            // 创建线性的渐变对象createLinearGradient(x1,y1,x2,y2)
            // addColorStop(stop,color)  stop为0.0-1.0之间  color为结束位置显示的css颜色值
            // var grd = dw.createLinearGradient(0,0,160,0);
            // grd.addColorStop("0.1","red");
            // grd.addColorStop("0.2","orange");
            // grd.addColorStop("0.3","yellow");
            // grd.addColorStop("0.4","green");
            // grd.addColorStop("0.5","blue");
            // grd.addColorStop("0.6","blue");
            // grd.addColorStop("0.7","purple");
            // dw.fillStyle = grd;
            // dw.fillRect(0,0,150,150);
    
            // 放射渐变
            // var grd = dw.createRadialGradient(x1,y1,r1，x2,y2,r2结束圆半径)
            // var grd = dw.createRadialGradient(100, 100, 5, 100, 100, 30);
            // grd.addColorStop("0.1","red");
            // grd.addColorStop("0.2","orange");
            // dw.fillStyle = grd;
            // dw.fillRect(0, 0, 200, 200);
    
            //阴影
            // dw.shadowColor = "black";
            // dw.shadowBlur = 10;
            // dw.shadowOffsetX = 0;
            // dw.shadowOffsetY = 0;
            // dw.fillStyle = "blue";
            // dw.fillRect(20,20,100,100);
    
    		
        }
```

# 高级

## 放大

```javascript

		//原大小
        dw.strokeRect(10, 10, 40, 20);
        //放大，将坐标放大，如果使用负数则为翻转
        dw.scale(2, 2);
        //放大之后再画
        dw.strokeRect(10, 10, 40, 20);
```

## 平移旋转



```JavaScript
 dw.translate(70,70);  //将绘制起点坐标平移到70，70
 
 dw.rotate(30*Math.PI/180); //接收弧度单位
```

## 矩阵变换

```javascript
dw.transform(m11,m12,m21,m22,dx,dy);
```

矩阵变化可以实现平移，旋转，缩放

## 合成

globalCompositeOperation

### 属性值

| 值               | 描述                                                         |
| :--------------- | :----------------------------------------------------------- |
| source-over      | 默认。在目标图像上显示源图像。                               |
| source-atop      | 在目标图像顶部显示源图像。源图像位于目标图像之外的部分是不可见的。 |
| source-in        | 在目标图像中显示源图像。只有目标图像内的源图像部分会显示，目标图像是透明的。 |
| source-out       | 在目标图像之外显示源图像。只会显示目标图像之外源图像部分，目标图像是透明的。 |
| destination-over | 在源图像上方显示目标图像。                                   |
| destination-atop | 在源图像顶部显示目标图像。源图像之外的目标图像部分不会被显示。 |
| destination-in   | 在源图像中显示目标图像。只有源图像内的目标图像部分会被显示，源图像是透明的。 |
| destination-out  | 在源图像外显示目标图像。只有源图像外的目标图像部分会被显示，源图像是透明的。 |
| lighter          | 显示源图像 + 目标图像。                                      |
| copy             | 显示源图像。忽略目标图像。                                   |
| xor              | 使用异或操作对源图像与目标图像进行组合                       |

## 碰撞检测

实现一般通过封装好的轮子去实现即可

### 圆形

判断圆心距离是否小于两圆的半径和

### 矩形

# 其他应用

## 压缩和解压

## 图片处理



