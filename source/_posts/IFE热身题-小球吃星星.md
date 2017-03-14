---
title: IFE热身题-小球吃星星
date: 2017-03-13 17:38:47
tags: [JS, ES6, 测试题]

---

　　下午做的四道题，觉得第四道hin有趣，又玩了几遍，做个记录~

---

　　先看下题目
	![Step4](http://ommn78ss1.bkt.clouddn.com/static/images/IFE%E7%83%AD%E8%BA%AB%E9%A2%98-%E5%B0%8F%E7%90%83%E5%90%83%E6%98%9F%E6%98%9F/Step4.PNG)

　　一开始看题目简直是一脸懵逼，后来在我不断钻研下，终于从万能的网友回答中看出了些门路~~~-_-|||
  
　　根据文本框的提示`调用'ball'的api移动小球，例如'ball.at(82, 46, ball => ball.turnRight())'`大概知道这里用到的是ES6的[箭头函数](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/Arrow_functions)。`Ctrl+Shift+I`调出控制面板，输入`console.log(ball)`查看ball的API，然并卵，控制器输出的是`Uncaught ReferenceError: ball is not defined`。哼，我才不会就这么死心呢。打开控制台的`Sources`面板，`2017/asset\js`文件里面竟然有3个js文件！那好吧，格式下js文件（点击左下角的`{}`），`Ctrl+F`调出查找面板，查找`turnRight`，当当当，以下就是找到的关于`ball`的几个方法。
    ```
    	o.fackBall = {
            x: 0,
            y: 0,
            direction: "",
            at: function(t, e, s) {
                return c.at(t, e, s),
                this
            },
            turnLeft: function() {
                switch (c.direction) {
                case "t":
                    c.direction = "l";
                    break;
                case "l":
                    c.direction = "b";
                    break;
                case "b":
                    c.direction = "r";
                    break;
                case "r":
                    c.direction = "t"
                }
                return this
            },
            turnRight: function() {
                switch (c.direction) {
                case "t":
                    c.direction = "r";
                    break;
                case "r":
                    c.direction = "b";
                    break;
                case "b":
                    c.direction = "l";
                    break;
                case "l":
                    c.direction = "t"
                }
                return this
            },
            turnBack: function() {
                switch (c.direction) {
                case "t":
                    c.direction = "b";
                    break;
                case "b":
                    c.direction = "t";
                    break;
                case "l":
                    c.direction = "r";
                    break;
                case "r":
                    c.direction = "l"
                }
                return this
            },
            wait: function(t) {
                return c.waitStartTime = Date.now(),
                c.waitTime = t,
                this
            }
        }
    ```
    
　　大概看下代码，`turnLeft`是向左转，`turnRight`是向右转，`turnBack`是向后转，`wait`是使时间延迟。
　　那么，回来看题目。这XX我怎么知道坐标啊。再次感谢万能的网络MUA~用鼠标点击屏幕控制台会显示点击点的坐标，诺，像酱~
   ![position](http://ommn78ss1.bkt.clouddn.com/static/images/IFE%E7%83%AD%E8%BA%AB%E9%A2%98-%E5%B0%8F%E7%90%83%E5%90%83%E6%98%9F%E6%98%9F/position.gif)
   
   有了坐标，有了API，那就开始动手吧
   在笔记上规划好路径，写下每个转折点的坐标。
   我的代码如下：
   ```
    ball.at(0, 46, ball => ball.wait(1500));
    var right = (x, y) => ball.at(x, y, ball => ball.turnRight());
    var left = (x, y) => ball.at(x, y, ball => ball.turnLeft());
    var back = (x, y) => ball.at(x, y, ball => ball.turnBack());
    right(76, 46);
    left(76, 136);
    right(130, 136);
    right(130, 243);
    left(91, 243);
    right(91, 364);
    back(18, 364);
    right(176, 364);
    right(176, 468);
    back(18, 468);
    left(372, 468);
    left(372, 46);
    left(270, 46);
    right(270, 134);
    left(222, 134);
    right(222, 243);
    back(173, 243);
    right(273, 243);
    left(273, 290);
    left(367, 290);
    right(367, 105);
    left(469, 105);
    right(469, 14);
    right(552, 14);
    left(552, 105);
    right(618, 105);
    right(618, 190);
    left(577, 190);
    left(577, 473);
   ```

---
　　
　　 **然而！！！然而！！！然而这不是我的解题过程**。傻傻如我，是直接调出控制台记录每个区域块的左上顶点坐标和宽高值，所以当后来我知道可以用鼠标点击获取坐标时我就这表情/(ㄒoㄒ)/~~
      ![解题过程1](http://ommn78ss1.bkt.clouddn.com/static/images/IFE%E7%83%AD%E8%BA%AB%E9%A2%98-%E5%B0%8F%E7%90%83%E5%90%83%E6%98%9F%E6%98%9F/%E6%97%A0%E6%A0%87%E9%A2%98.png)

　　我规划的路径是(1)->(2)->(3)->(4)->(5)->(6)->(7)->(8)->(9)->(10)->(11)->(12)->(13)->(14)->(3)->(4)->(15)->(16)->(17)->(18)->(19)->(20)->(21)->(22)->(23)->(24)->(25)->(26)
　　不过后来发现这里有个坑：小球经过同个点的时候只能有一种行为，比方说上面的规划路径(3)、(4)是定义了两次，那么实际上小球在这两个点上最终的行为是以后定义的为准的，而我在这两个点定义的两次方向是相反的，所以小球第一次经过(3)点的时候就报错了。解决的方法有两个：1. 定义一个附近的点，但要注意这个点之前不会经过；2. 重新规划一条路线。我这里选择了重新规划路线，最终流程图是这样的：
      ![解题过程2](http://ommn78ss1.bkt.clouddn.com/static/images/IFE%E7%83%AD%E8%BA%AB%E9%A2%98-%E5%B0%8F%E7%90%83%E5%90%83%E6%98%9F%E6%98%9F/%E6%97%A0%E6%A0%87%E9%A2%98%20-%20%E5%89%AF%E6%9C%AC.png)

　　应该会有更优的方案，这里没有做对比。最后运行结果如图：
      ![运行结果](http://ommn78ss1.bkt.clouddn.com/static/images/IFE%E7%83%AD%E8%BA%AB%E9%A2%98-%E5%B0%8F%E7%90%83%E5%90%83%E6%98%9F%E6%98%9F/IFE_ball.gif)

　　关于小球运动后的两颗星星如何吃到。可以在前面加上`ball.at(0, 46, ball => ball.wait(1500));`，让小球先等一会再动。

　　总结：**知无涯**。当学习到的越多，就会越加意识到先前的浅薄无知，所以，保持虚心。在知乎看过一个关于前端的回答：前端的学习是先平缓后陡峭再平缓的，也许现在的我就在这陡峭的山脚下，但，我是一个喜欢爬上山顶看风景的人~