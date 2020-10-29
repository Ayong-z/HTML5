[toc]
## 课程大纲
- 新增属性
    - placeholder（占位符）
    - Calender, date, time, email, url, search
    - ContentEditable
    - Draggable
    - Hidden
    - Content-menu
    - Data-Val（自定义标签）

- 新增标签
    - 语义化标签（一群类似Div的东西）
    - canvas（画板）
    - sag
    - Audio（声音播放） 
    - Video（视频播放）

- Api
    - 定位（需要地理位置功能，GPS）
    - 重力感应（陀螺仪）
    - request-animation-frame（动画优化）
    - History（控制当前页面的历史记录）
    - LocalStorage, SessionStorage（存储信息，比如：历史最记录）
    - WebSocket（通信，例如：聊天室，在线聊天）
    - FileReader（文件读取，预览）
    - WebWorker（文件的异步，提升性能，提升交互体验）
    - Fetch（传说中要替代Ajax的东西，兼容不好）


## 新增属性
### placeholder
- 一般用于输入框的提示信息
    ```html
    <input type="text" placeholder="请输入用户名">
    <input type="password" placeholder="请输入密码">
    ```

### input的type属性
-  时间相关属性
    ```html
    <!-- calendar类, 兼容不好（chrome支持，其他浏览不支持）,而且丑，一般不用-->
    <form action="">
        <input type="date">     <!--日期选择-->
        <input type="time">     <!--时间选择-->
        <input type="week">     <!--周选择-->
        <input type="datestime-local">      <!--组合选择-->
        <input type="submit" value="提交">
    </form>
    ```
- 其他属性
    ```html
    <form action="">
        <input type="number">     <!--chrome支持，其他浏览不支持-->
        <input type="email">     <!--chrome,firefox支持，其他浏览不支持-->
        <input type="color">     <!--chrome支持，其他浏览不支持-->
        <input type="color">      <!--组合选择-->
        <input type="range" min="1" max="100">      <!--chrome,safari支持，其他浏览不支持-->
        <input type="search" name="serach">     <!--chrome,safari支持，其他浏览不支持-->
        <input type="url">      <!--chrome,firefox支持，其他浏览不支持-->
        <input type="submit" value="提交">
    </form>
    
    ```

### contenteditable
- 作用：用于点击文本进入编辑状态。
- 属性值：trur/false

    ```html
    <!-- 属性值true,文本为可被编辑；false,文本不可编辑 -->
    <div contenteditable="true">monkey</div>
    ```
- 当父元素设置了 contenteditable 属性后，其中的子元素会继承父元素的contenteditable属性。例：
    ```html
    <!-- monkey和panada都为编辑状态 -->
    <div contenteditable="true">
        <span>monkey</span>
        <br />
        <span>panada</span>
    </div>
    ```
- 问：为什么不建议设置 contenteditable 属性的标签嵌套子元素?
    > 答：
    当设置了该属性的子元素内的文本内容被删除之后，再次点击删除，子元素标签结构就会当成一个整体被删除，会破坏原有的元素结构。
    
    ```html
    <div contenteditable="true">
        <span contenteditable="false">姓名：</span>涵涵
        <br>
        <span contenteditable="false">性别：</span>女
    </div>
    ```

### draggable
- 作用：用于设置元素是否可被拖拽。
- 属性值：true/false
- 效果：按下鼠标，元素可以被虚幻的拖拽。
- 兼容：chrome, safari 兼容，其他浏览器不兼容。
    ```html
    <style>
        .demo {
            width: 100px;
            height: 100px;
            background-color: #abcdef;
        }
    </style>
    
    <div class="demo" draggable="true"></div>
    ```
- **注**：img、a 标签默认就是可以被拖拽的。
- 问：拖拽的声明周期和拖拽的组成部分有哪些？
    > 答：
    拖拽的声明周期：拖拽开始（按下鼠标），拖拽进行中（拖拽移动中），拖拽结束（松开鼠标）。
    拖拽的组成部分：拖拽的物体和目标区域（目标元素）。
- 拖拽事件
    ```html
    <style>
        .demo {
            width: 100px;
            height: 100px;
            background-color: #abcdef;
        }
        .target {
            width: 200px;
            height: 200px;
            border: 1px solid #f40;
            position: absolute;
            top: 10px;
            left: 300px;
        }
    </style>

    <div class="demo" draggable="true"></div>
    <div class="target"></div>

    <script>
        var oDemo = document.getElementsByClassName('demo')[0],
            oTarget = document.getElementsByClassName('target')[0];
        // 当按鼠标并拖动元素是触发一次该事件，只是按下鼠标并不会触发
        oDemo.ondragstart = function(e) {
            console.log('ondragstart');
        }

        // 当元素正在被拖拽移动中不停的触发该事件
        oDemo.ondrag = function(e) {
            // console.log('ondrag')
        }

        // 当松开鼠标是触发一次该事件
        oDemo.ondragend = function(e) {
            console.log('ondragend')
        }

        // 当拖拽元素的鼠标进入目标区域时触发一次该事件，并不是被拖拽的元素进入目标区域时触发该事件。
        oTarget.ondragenter = function(e) {
            console.log('ondragenter')
        }

        //  当拖拽元素的鼠标进入目标区域后不停的触发该事件，元素在目标区内不移动同样不停的触发该事件。
        oTarget.ondragover = function(e) {
            console.log('ondragover');
            // e.preventDefault();
        }

        // 当拖拽元素的鼠标进入目标区域后再离开目标区域时触发一次该事件。
        oTarget.ondragleave = function(e) {
            console.log('ondragleave');
        }

        // 当拖拽元素的鼠标在目标区域松开鼠标时触发一次该事件。触发该事件必须在 ondrapover 事件中阻止默认事件
        oTarget.ondrop = function(e) {
            console.log('ondrop');
        }
    </script>
    ```

- **注**：所有的标签元素，在拖拽周期结束之后的默认事件都是回到原处。
- 问：为什么触发 ondrop 事件必须在 ondrapover 事件种阻止默认事件？
    > 答：
    事件是由行为触发的，但是一个行为可以触发不止一个事件。所以拖拽中当鼠标抬起时可以触发默认触发被拖拽的元素回到原点事件和 ondrop 事件。事件执行队列中，ondragover 事件在 ondrop 事件之前。所以必须在 ondragover 事件中阻止默认的回到原点事件，ondrop 事件才能被触发
- [拖拽练习](2.新增属性/practices/1.drag.html)
- 补充：拖拽事件对象中 dataTransfer 属性用于改变光标样式。兼容性极差。
    - e.dataTransfer.effectAllowed = 'link / copy / move / copymove / linkmove / all'; 必须在 ondragstart 事件中设置该属性。
    - e.dataTransfer.dropEffect = 'link / copy / move / copymove / linkmove / all'; 必须在 ondrop 事件中设置该属性。
    - 必须两个属性同时使用，并且属性值相对应


## 语义化标签
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
        img.src = 'data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAIAAAACACAYAAADDPmHLAAARdUlEQVR4Xu1de1xUVR7/nTvAMAPDayAfoSJg28pmqZla7a6VaLlpr9WNLB+5a+uzsrVse9lWlqZiApplq/kobT/Z7ic/tZurYS2aDwKRt3MnQUGEuQPMHd7MPfu5dxjCYYa5d+YyDMw5/8H9nfP7ne/3e869c54ISPJrBJBf155UHogA/FwERABEAH6OgJ9Xn/QARAB+joCfV5/0AEQAfo6An1ef9ABEAH6OgJ9Xn/QAMgogITrsM8B4NGBYTBvZEzIW3WtFEQHICG2CVoM7ittLM+w8MUULogEA2mCaI8ZebhsiABkRTdBqvgCABwGgimbYIa6Kjo/SLEQI/s7bIYTv0hnMma7yyP2cCEBGRBOjNIswgp18kRi4qXqm4WhPxSdoQ88CoPEAuJxmzCNkDEV0UUQAoqFybRgfEzYKcbhUsMTwLm1kn3eWKzEqdDZGSOj+AdBKmjGlufYgvwURgMyYJmg1JwFgEgD8k2bYh5wVnxCloQFBPADk0wx7k8xhiC6OCEA0VOIM46M19wPGKygMz+qM5kKnAuj4YMQIL9EbzO+LK11+KyIAF5jGRUTEKRSW1zDGZXqjea1cFPCvAA4gSc4y3YmNCMAFavFRmicRgo+sZjgHIVjVF1/r7pArJg8RgAiUErSajwGg83c9QpCqM7CrRGT1eRMiAJEUJWpDl2FAbwOARvhuR3iLzmB+VmR2t80So0LnYISWYAAWEPxFb2CtvzJkSkQAEoAcqQ27jQJYB4DvAcBpNGNe6Sx7YrRmFsZwIyAYBRgSAXAeBigGwBf0TMN/e3IbFxMyOICjFmKABQBwg822NwaLiAAkCMBmmhAdPp421Gc7yhofrbkBAXoTMJ7ttGiEDtIG06POnv88QNRpUYYx3t0bH4xEAG4IwGmrtw7ubAaAWN5GrVI1DIqJZmKHDK0ru3wpsqracF1rW6vSlh9z3Bh9bcN5+/K6zCmcRxh2t1qoXeX19bUyhtpZFBGATKheO7IH8PyyJSduvfmm27sWjwAZz+TmGjZs29HRrTseAk6IDl2MOKjVGc2HAMAiU4gOiyECkAFde/LfX78uJyoyYqyjopVBSkAIsmbNX3QH/xwDrNEz7HoZwnCrCCIAt2D7OZM9+Ts2vJMTGRHmhPwgiB0yBIKVSnhlw6avDn319Qy+JEUrxJSyrMHDUNzKTgTgFmzWTPbk73x3fU5YuMYx+YFBEDvUSj6fTGZz3uT7Hxwj/MHBnXQtm+VBKG5nJQJwE7pu5G9cnxsWprnFUXFBgYFCy1cFB3c+5jju6k13TxskvAYwLNIbWWFdgLcTEYAbiNuT/9Gmd3M1mhDH5AcECi2/K/kdLpuTpky1KeIFmmE3uBGKx1mIACRC2I38zRvPaULVNzsqJjAgEIY5Jh9aW9v0Y6fdx08HAwb8iJ4Rvvi9nogAJEBuT/7u1E3n1CEqJ+QHCN2+WqVy6KHiStWpaSmPTxQ+ATju5p9qG/IkhCKbKRGASCjtyd+VujkvJCTY+hFnlwIUAULLd0a+heOqZv/pz40ltD4eI/ipNZhNunwZmroW02U0MBNjnAkUyraAJbvM0HhFZMiizIgARMBkT/7HW1LzVGqlE/IVEDtkKISoHbd83t26tIyT+z//YrLQ/SM0V28wfWIfRpfRwGseIYzn6Izmf4gIW5QJEYALmOzJ35u2JU+pDHJIvoJSwLCh4snvaUJpZGTIGIpCszBGyQjBb2xhYoxfl3NOgAigBwF0J/+980ploMP1e5LJR+gfYvcCJA4OjYE2ajo/C61jTHtFNW2RRkQAToCyJ39fxtbzQYEBTsinYNjQ60V3+yCBfJE8um1GBOAAuu7kp+UHBSp+5QhlClEw/Pr+ST5fHyIAO1btyf90W1q+ImBgkk8E4Ir8jK0FisCAJEctHwGCEbGxsnT7idGhU/pqoSnpATrYtW/5n2xPKwxQKEb3NvkJUZodgGAxArxfx5gfd/tl7mZGIgAHs3qfbs8oVCiQQ/J5nONih8nS8vmyErSabwFgitAdY/ijzsh2LEF3k1GJ2fxeAPYt/8D29CJKQf3SGY5yks/74HcSIQxfdvhrRoi6Q2eo/1Eij26b+7UAupH/fnoxRVE3eot8m594rWYdAnjR+jf6L82Ykt1mVGJGvxWAPfkH388oRhTyOvk2vhK0mmMAcBf/N82wXuPFa44kCrNXzbu3/IwSikK/8HbL7+pPWE7O4bUYoFTOoV5XQPqdALqRvyOjlEKoc/OFPWCu3vmffvGv42++l/Zba+8tfnjXFTHeeu5XAuje7W+7gCgY5W7Lr6yqOpX8qHVOvz+SL4TtLaX1tR978j/bse2CsG3LSXLV8vlsTz3/4vn/nT5jnR9A1K3Odgv1dd178u8XAnBAvg4Qv1/PcRJDPgC0jU2ewXXs9NlNM+xCXybaWWwDXgDduv0dGTRCKMFD8qGxual4wr0zhV8Nfbmmz1PRiRJAYlToaA7A+WZH8VGI8ueqOIqCTDFj591bfoYOEPK05QvhWSyWyjH3TB/aEWufrep1hZWr5y4JidOGTVQA/sFVQd5+jjBO6ukMnu4ffOkXEEV59M63r+N9c+eXlVdUjAAEebSBdbg41Nu4SPXnUgD8TBXGiB+v9qnU01757lO66aWKAMrtn3rOKn7o63+ffGX9RuvaPpmXankLbEkCeOTDb86HhUdY358uc8pfBZOxtuTzp6bbtl5lAoJm4KAMA64KVqg2FtbUmO3J35+xtTQwMEB28m21e3LV6sunfswRtoMDwBkEcBQQ/o/8tXdQIgfV7WFm/cWL0OyuP5c0du0BeAFEREb12Zl2dQyT00UA9nXORBhv+/nwRYD9aVtKApVBbo/wiQX14UV/yi6hfxov1r4X7AoA8Lc0Y14htex+K4CJd/42j6mpVupKihwSvDdtS7FSGeT22L5UIA/+68tv9n1+aLS+/JKtN5BahAz2uBxhuK+nbyN7J/1WAO/t3FukVKl/iTlLWUlBYXnG5rdva2ttEbbe7nlvc1GwKtjtKV1PmOA4rsrEslWelCE2b1Nzc/OV6urmE2ey1dv37LvNls+iYVViXwv9XgB8pWNHxAGFUPEDUybHtbW1BN91++Qflix4gj+u1WEKUakhblgfNlSxDEuwoy+WFcxasEhYvoYAbdcxpqVisksSwPCX92Vx6kjhZIu+SJSZOV6+bp4w8WLrAWwCUKtDIPfsqWOrFs+/m//frtRNBSEhKofr+fjnccJ6PnVfVKPXfB757rs9z7z6N+E8QwRonpg9BJIEEPmXXd8hjbZzl0qv1cRJwVxdzbG61EUCwY4EYLFwNckTRsfwz/+2etXZG0cl3uosxtjBQyA8TDjyb0Cl1W+8mf3V0Uz+g/QQzbCPuKrcgBIAX9kVT6ZcKMjNGTUzOfnHJ2Y/NM4ZAIOioyE6KsoVPv3u+eEjx0688Na62wGDgTayQmPoKQ04AbyyalluVubRW349cWLeikXzHe7h4wG5TquFGK3WFT797vmVq9Vnpv7hsQnCa4CyjNPVNOYQAThAYKAK4HTOueMLn33OukAFtw+njU2XiAD8SACpH+zM2vnJAf5D/SLNsCNddWHkFeAKoX70vL29vWzS7x6IaWppUQOGD2gj+5Sr8IkAXCHUj57PXf50cW5+gXX0U+QKJSKAfkSws1CvVtecTlm2csTVmhrh2Dn+EBKaYV8SUzUiADEo9YFNeUXFycILupaeXF+qqFR8/8Op6Ozz+V2HvSUtTyMC6ANyxbhM37X7f9s/3nenGNtOG4Sfog3mD6TkIQKQgpYXbf+deTzzubVvCJtGxSQM8LaeYf8qxrarDRGAVMR8wR7j+qqamsLcwqKWa0SC8QHaaE6REiIRgBS0fNC2obGx7LYZszqvncUYPas3mraIDZUIQCxSPmzX1t5eesvUe23L3iotGjahV9YD+PpsIM+Rv84FXKqsPHHvY/OEG0qk3EZKegAfbtlSQmtta784NvneOEEAElYoEwFIQdm3bVuSpkwVlsQRAfjhdDB/APWYu6cNtmoUPU0zpq1i9Ep6ADEo9QObepbNu33mQ9b1Dxg/ShvNB8WETQQgBqV+YPPCW+vOHj5yjF8C14oQNVnsQVNEAP2AXFchfn746+OvbtzUsQgENtJGdrWrPLbnRABikfIxO4wxW1VdU/TW1vSgb7NOWO8rQpCHuPbf6YxNl8WGSwQgFikv2dXV1+e+vGFTj95MJpPSbgaQP6PgGwviFki9UYQIwEvEinWz/KVXcztbtLhMLAZIj2TY17IB2sRl+dmKCEAqYr1sz88CHj5yNMLejbGuTnmuoNBuuxv+D0XhNRdqGnLdDYsIwF3k+iIfxvWVV6tL5z+zKrGy6mqk9RefZ1fIEAH0BZEy+Jy7fGVubn5hx2WVaD7NmPa4UywRgDuo+UAejHH1xPsfUDQ0NPK7W/JphnXr3AYiAB8g090Qfsj+8btFzz0v7NVsR5ahUn8BWH85ukhdTwgh08Gu0PLuc5Y1F0ya+aCwAxojmKk3sIelRkAEIBUxH7NPmjJViMjdj0EiAB8jVEo4/KDRHQ88Yru1/GGaYb+Qkp+8Avr57uCsM2ePL169RpgDQAoqUVddTxMB+Mn2cH4u4DcPz24z1tZF8cPAesbM3ywqOZFXgGTIfCND15PKxR4H4yhyIgDf4FNUFHyrv3ylqiBlyfIbauvrrcebeHhJBRGAKOh718jZ+H9Xr45mAD0ln3wE+shHoBszgDTCKENnNKV6Kk3SA3iKoAz5e1oDUF5REX7lanVMY1NTqM0VBWjSBcZ0SgbXZCRQDhC9UcaR49+ff+a114XxfoTgXLiBneDO/L99rKQH8AZ7Mvmoqq7JumdOinBQJwaUoWdMyz0tmgjAUwS9nH/nJwcPp37w4f28WwugSRc9fBUMWAGMu+lXRWtWLHV6YHR0ZBQMion2Mn2eu2tpaaXHTZ8h3Nng7vh/1ygGrAA0IWrTR6kbw5xBrgkJgeHXX+85I31Qwj2/T6mpMtTEYMD79R5eOT9gBcDzcmB7+iVKQQ1zxJEyMAgSRwp7KftdSlm6siSvsPAXgOBr2sDO8KQCA04AS5+YU1pckCfslX/n5TXZ8cOHO73JY+SwYaBWqTzBz/t5Ma5Puis53OoYp9GMeaUnQQwoAVgslqrkCUkdGyStsBzYnmGgFMjhy14drIKRwx12EJ5g2qt5r1ytPj31D491XA6BV9CMOd0ThwNKAMX5ed8vnTfn110BGRU/svqtNauvcwaSNjISBse4PFTbE4xlzTs95fGKy1eqhI8XDnPTfjI2HPHEgSQBhP/xnVMofJD1suQ+SJyxKsu060Xhd7Cj+wLuHtd5RdBFQOgMYCxcdhkZEV655fW1l1QqpcPY+Z4gdshgCAwM7INaiXPJcVzN3GVP1+YVFdmOgpHlskpJAtAsePM0UoZ03k0jLnQZrZrMJ017XhHu6esUAELGivKLBW+sWdW15S+kGXZ3QnTYZzYR8HleembFidjBQ9URYaFaRYDimr4/gFJAaGgIBCuDQRWshGClEiiKkjF494oymxvyC0pLmBUvvza+obFzOFjUQdBiPPZbAfC3hplMdcFFeeeuuRPQ/rdxolbzIgbgz8/rHEsXA4zP2iDIog2stAMke6hMvxWAgzpVYow/1BvNa+2fJcaox2KOWgCAnuDfCD5Lbs+B5QPAXpphN8gZf38WwDU3h1ooboerdfEJUaphgALGY4QHAwfX/FqQCVQsUzmdxVCI0oMCnXBnvZ+YWCQJIGTmUlmmIMUE5sym4cttwodcT3cHe1K+v+WVJABfAocIQB42XAqAd5OgDT0LgPryblz72p6kGVY4FJEkzxAQJQDPXJDcvowAEYAvs+OF2IgAvACyL7sgAvBldrwQGxGAF0D2ZRdEAL7MjhdiIwLwAsi+7IIIwJfZ8UJsRABeANmXXRAB+DI7XoiNCMALIPuyi/8DldHlF/S8GH4AAAAASUVORK5CYII='
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