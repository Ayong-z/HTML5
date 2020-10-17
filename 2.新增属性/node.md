[toc]
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
- [拖拽练习](./practices/1.drag.html)
- 补充：拖拽事件对象中 dataTransfer 属性用于改变光标样式。兼容性极差。
    - e.dataTransfer.effectAllowed = 'link / copy / move / copymove / linkmove / all'; 必须在 ondragstart 事件中设置该属性。
    - e.dataTransfer.dropEffect = 'link / copy / move / copymove / linkmove / all'; 必须在 ondrop 事件中设置该属性。
    - 必须两个属性同时使用，并且属性值相对应