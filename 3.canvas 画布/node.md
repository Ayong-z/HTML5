[toc]
### canvas
- 作用：用于绘制图形和制作动画
- 用法：
    ```html
    <style>
        canvas {
            border: 1px solid #f40;
        }
    </style>
    <!-- 画布的大小必须设置为行间样式 -->
    <canvas id="can" width="500px" height="500px"></canvas>
    
    <script>
        var oCanvas = document.getElementById('#can'),
            ctx = oCanvas.getContext('2d');     // getContext('2d') 获取画布的可绘制区域。参数”2d“表示画布的类型 
    </script>
    ```

- 绘制直线
    ```js
    ctx.lineWidth = 1;      // lineWidth 设置绘画笔的宽度
    ctx.moveTo(50, 50);     // moveTo(x,y) 方法设置路径的起点
    ctx.lineTo(200, 50);    // lineTo(x,y) 方法设置路径的终点
    ctx.lineTo(200, 200);   // 多次调用 lineTo() 方法可绘制连续路径
    ctx.closePath();        // closePath() 闭合路径，形成封闭的路径。也可以使用 lineTo() 方法设置路径起点闭合路径
    ctx.stroke();       // stroke() 方法绘制直线
    ctx.fill();     //  fill() 方法闭合路径并填充，当使用填充时会自动闭合路径

    ctx.beginPath();    // beginPaht() 绘制新路径
    ctx.lineWidth = 5;      // 当绘制新路径时可重新设置绘画笔的宽度
    ctx.moveTo(250, 250);
    ctx.lineTo(400, 250);
    ctx.lineTo(400, 400);
    ctx.closePath();
    ctx.stroke();
    ```

- 绘制矩形
    ```js
    ctx.rect(50, 50, 200, 100);     // rect(x,y,w,h) 方法构建矩形路径
    ctx.stroke();

    ctx.strokeRect(50, 200, 200, 100);      // ctx.strokeRect() 构建路径并绘制矩形

    ctx.fillRect( 50, 350, 200, 100);      // fillRect()  构建路径并填充矩形
    ```
    - 补充：clearRect(x,y,w,h) 方法：清除画布的内容。例：下落的矩形
    ```html
    <style>
        canvas {
            border: 1px solid #f40;
        }
    </style>

    <canvas id="can" width="500px" height="500px"></canvas>
    
    <script>
        var oCanvas = document.getElementById('can'),
            ctx = oCanvas.getContext('2d');     // getContext('2d') 获取画布的可绘制区域。参数”2d“表示画布的类型 
        
        ctx.strokeRect(150, 50, 50, 50);

        var height = 50;
        setInterval(function() {
            ctx.clearRect(0, 0, 500, 500);      // 清除画布
            height += 2;
            ctx.strokeRect(150, height, 50, 50);
        }, 1000 / 60)
    </script>
    ```

- 绘制弧线
    - 弧线参数：圆心(x,y), 半径(r), 弧度(起始弧度，结束弧度), 方向(顺时针0, 逆时针1);
    - 弧度表示：Math.PI（180度）;
    ```js
    ctx.arc(100, 100, 50, 0, Math.PI * 1.7, 0);     // arc() 构建弧度路径
    ctx.lineTo(100, 100);
    ctx.closePath();
    ctx.stroke();
    ```
    
- 绘制圆角矩形
    - A点：圆角矩形相邻的两条边延长线交叉点
    - B点：圆角矩形一条边的终点
    - arcTo(aX, ay, bX, by, r)
    ```js
    ctx.moveTo(100, 110);       // 圆角矩形一条直角边的起点, 圆角半径10
    ctx.arcTo(100, 200, 200, 200, 10);
    ctx.arcTo(200, 200, 200, 100, 10);
    ctx.arcTo(200, 100, 100, 100, 10);
    ctx.arcTo(100, 100, 100, 200, 10);
    ctx.stroke();
    ```

- 绘制贝塞尔曲线
    - 二次贝塞尔曲线：quadraticCueveTo();
    - 三次贝塞尔曲线：bezierCueveTo();
    - 注：数学不好，贝尔塞曲线跳过😂

- 平移
    - canvas 画布的默认坐标原点为左上角。
    - 坐标系平移是在画布中平移坐标原点的位置。
    ```js
    ctx.beginPath();
    ctx.translate(200, 200);    // 平移坐标原点到X:200, Y:200的位置
    ctx.strokeRect(0, 0, 150, 80);
    ```

- 缩放
    - 作用：把画布的坐标系进行一定比例的缩放。
    - 用法：scale(x轴缩放比例，y轴缩放比例)
    ```js
    ctx.beginPath();
    ctx.scale(2, 1);    // x轴放大2倍，y轴不变
    ctx.strokeRect(100, 100, 100, 100);
    ```

- 旋转
    - 作用：把画布的坐标系进行一定角度的旋转。
    - 用法：rotate(Math.PI);
    - Math.PI,表示180度。
    ```js
    ctx.beginPath();
    ctx.translate(200, 200);    // 平移坐标原点到X:200, Y:200的位置
    ctx.rotate(Math.PI / 3);    // 坐标系旋转60度
    ctx.strokeRect(0, 0, 150, 80);
    ```

- 坐标系存储和恢复
    - 作用：存储原始坐标系，可存储坐标系的平移、缩放和旋转。
    ```js
    tx.save();     // 存储原始坐标系
    ctx.beginPath();
    ctx.translate(200, 200);    // 平移坐标原点到X:200, Y:200的位置
    ctx.rotate(Math.PI / 3);    // 坐标系旋转60度
    ctx.strokeRect(0, 0, 150, 80);

    ctx.beginPath();
    ctx.restore();      // 恢复为原始坐标系
    ctx.fillRect(100, 0, 200, 100);
    ```

- 背景填充
    - 作用: 用于背景颜色，背景图片的填充
    - 背景的填充是以坐标系原点进行填充的。
    ```js
    // 填充背景颜色
        ctx.fillStyle = '#abcdef';      // 设置填充颜色
        ctx.fillRect(50, 50, 100, 50);
        
        // 填充图片
        var img = new Image();
        img.src = 'data:image/png;base64,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'
        img.onload = function() {       // 图片加载完成后进行填充
            ctx.beginPath();
            ctx.translate(150, 100);       
            var bg = ctx.createPattern(img, 'no-repeat')        // 创建图片纹理，纹理背景设置为不重复
            ctx.fillStyle = bg;     // 设置填充背景
            ctx.fillRect(0, 0, 200, 200);
        }
    ```

- 线性渐变
    - 作用：用于背景渐变填充。
    - 用法：createLinearGradient(x0, y0, x1, y1);
        - x0, y0: 渐变的起始位置
        - x1, y1: 渐变的终止位置
    - 渐变的起始点同样为坐标原点。
    ```js
    ctx.beginPath();
    var bg = ctx.createLinearGradient(0, 0, 200, 100);      // 创建线性渐变
    // 添加颜色节点（0 - 1）
    bg.addColorStop(0, 'red');
    bg.addColorStop(0.5, 'green');
    bg.addColorStop(1, 'blue');
    ctx.fillStyle = bg;
    ctx.fillRect(0, 0, 200, 100);
    ```

- 辐射渐变
    - 用法：createRadialGradient(x0, y0, r0, x1, y1, r1);
        - x0, y0, r0: 渐变起始圆的圆心位置和半径
        - x1, y1, r1: 渐变结算圆的圆心位置和半径
    - 起始圆和结束圆位置半径不同可组合成不同的渐变效果
    ```js
    ctx.beginPath();
    var bg = ctx.createRadialGradient(100, 100, 30, 100, 100, 100);      // 创建辐射渐变渐变
    // 添加颜色节点（0 - 1）
    bg.addColorStop(0, 'white');
    bg.addColorStop(0.5, 'black');
    bg.addColorStop(1, 'white');
    ctx.fillStyle = bg;
    ctx.fillRect(0, 0, 200, 200);
    ```

- 阴影
    - 属性：shadowColor = "";
    ```js
    ctx.beginPath();
    ctx.shadowColor = 'red';    // 阴影颜色
    ctx.shadowBlur = 10;    // 阴影模糊
    ctx.shadowOffsetX = 10;     // x轴阴影偏移量
    ctx.shadowOffsetY = 10;     // y轴阴影偏移量
    ctx.fillRect(50, 50, 100, 60);
    ```

- 绘制文本
    ```js
    ctx.beginPath();
    ctx.font = '40px Georgia';     // 设置文本大小和字体样式
    ctx.fillStyle = 'red'
    ctx.strokeText('panada', 200, 50);      // 文字描边
    ctx.fillText('money', 200, 150);        // 文字填充
    ```

- 线端样式
    ```js
    // 单条线端样式
    ctx.beginPath();
    ctx.lineWidth = 14;
    ctx.moveTo(100, 50);
    ctx.lineTo(200, 50);
    ctx.lineCap = 'round';     // 线端可设置三种样式：butt / square / round
    ctx.stroke();

    // 两条线相交后线端样式
    ctx.beginPath();
    ctx.moveTo(100, 200);
    ctx.lineTo(200, 200);
    ctx.lineTo(100, 250);
    ctx.lineJoin = 'miter';     // 相交后线端可设置三种样式：miter / round / bevel
    ctx.miterLimit = 3;     // 当线端样式为 miter 时，可使用 miterLimit 属性控制尖角程度
    ctx.stroke();
    ```