# CoordinatorLayout #

通常将CoordinatorLayout作为顶层布局来协调其自布局之间的动画效果

子View1在布局中通过设置behavior属性与子View2关联，当移动view2的时候view1产生响应的效果，而这个效果具体是怎么样由behavior决定

view1叫做child，view2叫做dependency