[toc]
### svg
- 绘制直线
    - <line><line>
    ```html
    <style>
        #svg1 {
            border: 1px solid #f40;
        }
        .l1 {
            stroke: red;
            stroke-width: 2px;
        }
        .l2 {
            stroke: yellow;
            stroke-width: 5px;
        }
    </style>

    <svg id="svg1" width="500px" height="500px">
        <!--
            x1,y1 : 线段的起始坐标
            x2,y2 : 线段的结束坐标
        -->
        <line x1="100" y1="100" x2="200" y2="100" class="l1"></line>
        <line x1="200" y1="100" x2="200" y2="200" class="l2"></line>
    </svg>
    ```

- 绘制矩形
    - <rect></recy>
    ```html
    <style>
        #svg1 {
            border: 1px solid #f40;
        }
    </style>

    <svg id="svg1" width="500px" height="500px">
        <!-- 
            x : 矩形的起始坐标
            x : 矩形的结束坐标
            width : 矩形宽度
            height : 矩形高度
            rx : 矩形x方向上圆角半径
            ry : 矩形y方向上圆角半径
         -->
        <rect x="100" y="100" width="100" height="50" rx="10" ry="10"></rect>
    </svg>
    ```
    - **注：** 在svg中，所有的封闭图形都会自动填充

- 绘制正圆
     - <circle></circle>
     ```html
     <style>
        #svg1 {
            border: 1px solid #f40;
        }
    </style>

    <svg id="svg1" width="500px" height="500px">
        <!-- 
            r : 圆的半径
            cx : 圆心x轴坐标
            cr : 圆心y轴坐标
         -->
        <circle r="50" cx="100" cy="100"></circle>
    </svg>
    ```

- 绘制椭圆
    - <ellipse></ellipse>
    ```html
    <style>
        #svg1 {
            border: 1px solid #f40;
        }
    </style>

    <svg id="svg1" width="500px" height="500px">
        <!-- 
            rx : x轴半径
            ry : y轴半径
            cx : 圆心x轴坐标
            cy : 圆心y轴坐标
        -->
        <ellipse rx="100" ry="50" cx="200" cy="200"></ellipse>
    </svg>
    ```

- 绘制折线
    - <polyline></polyline>
    - 折线点坐标x y使用空格隔开，折线点之间使用逗号隔开。
    - 绘制折线去掉填充不会自动闭合。
    ```html
    <style>
        #svg1 {
            border: 1px solid #f40;
        }
        polyline {
            fill: transparent;      /** 设置填充为透明 **/
            stroke: blue;
            stroke-width: 3px;
        }
    </style>

    <svg id="svg1" width="500px" height="500px">
        <!-- 
            points : 折线点坐标
         -->
        <polyline points="0 0, 50 50, 50 100, 100 100, 100 50"></polyline>
    </svg>
    ```

- 绘制多边形
    - <polugon></polygon>
    - 多边形顶点坐标x y使用空格隔开，折线点之间使用逗号隔开。
    - 多边形会自动闭合。
    ```html
    <style>
        #svg1 {
            border: 1px solid #f40;
        }
        polygon {
            fill: transparent;      /** 设置填充为透明 **/
            stroke: blue;
            stroke-width: 3px;
        }
    </style>

    <svg id="svg1" width="500px" height="500px">
        <!-- 
            points : 多边形顶点坐标
         -->
        <polygon points="0 0, 50 50, 50 100, 100 100, 100 50"></polygon>
    </svg>
    ```

- 绘制文本
    - <text></text>
    ```html
    <style>
        #svg1 {
            border: 1px solid #f40;
        }
        text {
            stroke: blue;
            stroke-width: 3px;
        }
    </style>

    <svg id="svg1" width="500px" height="500px">
        <!-- 
            x : 文本左上角x轴坐标
            y : 文本左上角y轴坐标
         -->
        <text x="100" y="100">Hellow Word</text>
    </svg>
    ```

- 透明度和线端样式
    ```html
    <style>
        #svg1 {
            border: 1px solid #f40;
        }
        polyline {
            fill: black;      /** 背景填充 **/
            stroke: blue;
            stroke-width: 10px;
            fill-opacity: 0.3;      /** 描边透明度 **/
            stroke-opacity: 0.3;    /** 填充透明度 **/
            stroke-linecap: round;      /** 描边线端样式：butt / round / square **/
            stroke-linejoin: round;     /** 描边相交点样式 **/
        }
    </style>

    <svg id="svg1" width="500px" height="500px">
        <polyline points="0 0, 50 50, 50 100, 100 100, 100 50"></polyline>
    </svg>
    ```

- 绘制路径
    - <path></path>
    ```html
    <style>
        #svg1 {
            border: 1px solid #f40;
        }
        path {
            fill: transparent;
            stroke: red;
        }
    </style>

    <svg id="svg1" width="500px" height="500px">
        <!-- 
            d : 路径参数
            M : moveTo, 路径起始位置
            L / l : lineTo, 路径结束位置(大写表示绝对路径, 小写表示相对路径)
            h : 水平方向距离
            v : 垂直方向距离
            z : 闭合
            
         -->
        <path d="M 100 50 L 200 50 L 200 90"></path>
        <path d="M 100 100 L 200 100 l 50 50"></path>
        <path d="M 100 150 h 100 v 100 z"></path>
    </svg>
    ```
    - path 绘制圆弧
    ```html
    <style>
        #svg1 {
            border: 1px solid #f40;
        }
        path {
            fill: transparent;
            stroke: red;
        }
    </style>

    <svg id="svg1" width="500px" height="500px">
        <!-- 
            A x轴半径 y轴半径 旋转角度(0-360) 大小弧(1/0) 顺/逆时针(1/0) x轴终点 y轴终点
         -->
        <path d="M 200 200 A 100 50 90 1 0 150 200"></path>     
        <!-- 起点200 200，终点150 200，x轴半径100，y轴半径50，旋转90度，小弧，逆时针-->
    </svg>
    ```

- 线性渐变
    ```html
    <style>
        #svg1 {
            border: 1px solid #f40;
        }
    </style>

    <svg id="svg1" width="500px" height="500px">
        <defs>      <!-- 定义渐变 -->
            <linearGradient id="bg1" x1="0" y1="0" x2="100%" y2="100%">   <!-- 创建渐变纹理并设置起始点-->
                <!-- 设置颜色节点位置和颜色 -->
                <stop offset="0%" style="stop-color: rgb(255, 255, 0)"></stop>
                <stop offset="100%" style="stop-color: rgb(255, 0, 255)"></stop>
            </linearGradient>
        </defs>
        <!-- 绘制图形，填充背景渐变 -->
        <rect x="100" y="100" width="200" height="100" style="fill: url(#bg1)"></rect>
    </svg>
    ```

- 高斯模糊
    ```html
    <style>
        #svg1 {
            border: 1px solid #f40;
        }
    </style>

    <svg id="svg1" width="500px" height="500px">
        <defs>      <!-- 定义渐变 -->
            <linearGradient id="bg1" x1="0" y1="0" x2="100%" y2="100%">   <!-- 创建渐变纹理并设置起始点-->
                <!-- 设置颜色节点位置和颜色 -->
                <stop offset="0%" style="stop-color: rgb(255, 255, 0)"></stop>
                <stop offset="100%" style="stop-color: rgb(255, 0, 255)"></stop>
            </linearGradient>

            <filter id="gaussian">    <!-- 定义模糊 -->
                <feGaussianBlur in="SourceGraphic" stdDeviation="10"></feGaussianBlur>  <!-- 设置模糊参数 -->
            </filter>
        </defs>
        <!-- 绘制图形，填充背景渐变，设置高斯模糊 -->
        <rect x="100" y="100" width="200" height="100" style="fill: url(#bg1); filter: url(#gaussian)"></rect>
    </svg>
    ```

- 绘制虚线
    ```html
    <style>
        #svg1 {
            border: 1px solid #f40;
        }
        .l1 {
            stroke: red;
            stroke-width: 6px;
            stroke-dasharray: 5 10;    /** 设置虚线间隔，可添加多个数值，虚线和实体一次排列 **/
            stroke-dashoffset: 10;      /** 设置虚线向左偏移量 **/
        }
    </style>

    <svg id="svg1" width="500px" height="500px">
        <line x1="100" y1="100" x2="200" y2="100" class="l1"></line>
    </svg>
    ```

- 比例尺
    ```html
    <style>
        #svg1 {
            border: 1px solid #f40;
        }
        .l1 {
            stroke: red;
            stroke-width: 6px;
            stroke-dasharray: 5 10;    /** 设置虚线间隔，可添加多个数值，虚线和实体一次排列 **/
            stroke-dashoffset: 10;      /** 设置虚线向左偏移量 **/
        }
    </style>
    
    <svg id="svg1" width="500px" height="300px" viewbox="0 0 250 150">  <!-- 比例缩放：以x0,y0为起点。x轴y轴进行比例缩放 -->
        <rect x="0" y="0" width="100" height="50"></rect>
    </svg>
    ```