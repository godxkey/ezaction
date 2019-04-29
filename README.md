# ezaction
An extension animation framework for cocos creator.

基于cocos creator的 2D 动画扩展库，接口简单易理解，支持可编程式自定义缓动曲线（缓动曲线算法源于`greensock` https://greensock.com/customease）


### 举很多🌰 

        
1. 执行一个moveTo动作

			let act = ezaction.moveTo(2.0,cc.v2(200,200));
			// 使用和cocos类似，不过首字母大写，详细请查看ezaction/ezaction.d.ts
			this.node.RunAction(act); 
			
			et act = ezaction.moveTo(2.0,cc.v2(200,200));


2. 延时动作

		    //delay1秒
		    let act = ezaction.delay(1.0); 
		    // 加速2倍
		    act.setSpeed(2.0);

3. 延迟4秒执行moveBy,并repeat10次 

		    let act = ezaction.moveBy(2.0,cc.v2(200,200),{delay:4.0}).repeat(10);
		    this.node.RunAction(act);

4. ezaction的属性动态动画

    `ezaction`动画目标可以是任意对象，也就是说动画的目标可以不是cc.Node。
    `HActionTweenBase`的`setTarget`方法可以修改目标对象，`HActionTween`和`HActionTweenBy`继承了`HActionTweenBase`。
    
    <a name="fenced-code-block">你唯一需要注意的是tween定义的属性名必须在target上能够找到。</a>
    如果你使用过`Tweenlite`，你可能非常容易理解这种使用方式。

		    let target = {
		        hp:0,
		        mp:11,
		        cp:2
		    }
		    let act = ezaction.tween(2.0,{hp:100,mp:233});
		    act.setTarget(target);
		    act.onStoped( ()=>{
		        cc.log(target);
		    } )
		    this.node.RunAction(act);

5. repeatForever? 支持的!
 
		    let act = ezaction.moveBy(2.0,cc.v2(200,200),{delay:4.0}).repeatForever();
		    this.node.RunAction(act);
    
6. 支持then式语法

		    let act1 = ezaction.scaleTo(0.2,{scale:1.7}).onStoped( ()=>{
		        // TODO
		    } );
		    let act2 = ezaction.scaleTo(0.2,{scale:1}).onStoped(next);
		    // act1执行完毕以后调用act2
		    act1.then(act2);
		    this.node.RunAction(act1);
    
7. Sequence 或Spawn ? 支持! 

		    let act = ezaction.spawn( [ezaction.moveBy(2.0,cc.v2(200,0),{delay:0.5}), ezaction.scaleTo(3.3,{scaleX:3.0,scaleY:2.0})]  );
		    this.node.RunAction( act.repeat(5) ); // spawn 5次
		    
		    let act = ezaction.sequence( [ezaction.moveBy(2.0,cc.v2(200,0),{delay:1.0}), ezaction.scaleTo(3.3,{scaleX:3.0,scaleY:2.0}) ]  );
		    this.node.RunAction( act );
    
8. Action的回调方法

		    let act = ...
		
			//每次act开始执行回调, 如果repeat 三次，则onComplete回调三次
		    act.onStart(function( action )
		    {
		    });
		    
		    //每次update回调
		    act.onUpdate(function( action, dt )
		    {
		    });
		
		    //act 完成，如果repeat 三次，则onComplete回调三次
		    act.onComplete(function( action )
		    {
		    });
		
		    //act 停止
		    act.onStoped(function( action, dt )
		    {
		    });
    
9. 支持缓动。
    
		    let act = ...
		    act.easing(ezaction.ease.easeBackOut(0.5));
		
		    // ezaction兼容了creator的缓动算法，所以以下用法有效。
		    act.easing( cc.easeBackIn() );

10.   支持可编程式自定义缓动曲线

		    let ce = ezaction.HCustomEase.create("custom_ease","M0,0 C0.548,0.482 0.62,0.913 0.804,1.02 0.873,1.06 0.938,1.012 1,1");
		    let easeFunc = ezaction.ease.customEase(ce);
		    act.easing( easeFunc );

    你可能会问，类似这样的动画曲线标记字符`M0,0 C0.548,0...`这是怎么来的？
    该内容是一段`SVG <path>`曲线，你可以从其他地方拷贝或者使用`Adobe Illustrator`来生成一条path路径曲线。
    前面说到过，自定义缓动曲线算法基于greensock提供的开源代码，我在它的核心算法上做了封装。
    幸运的是greensock提供了<path>路径曲线在线编辑工具，可以非常直观的获取曲线标记字符内容。
    https://greensock.com/customease

    
11. 支持action的`pause`、`resume`、`clone`。



<br>

## ezaction的继承谱系
![extends](https://raw.githubusercontent.com/haroel/imageHub/master/haction_f.png)


`ezaction.tween(ezaction.moveTo/scaleTo/skewTo/fadeTo...) `返回的是一个`HActionTween`类实例，
`ezaction.tweenBy(ezaction.moveBy/scaleBy/skewBy...)`返回的是一个`HActionTweenBy`类实例，


    
当然, 还有很多等你发现。。。
