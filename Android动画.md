# Android动画 #

Animation框架定义了透明度、旋转、缩放和位移几种常见的动画，而且控制的是整个View。

实现原理是每次绘制视图时View所在的ViewGroup中的drawChild函数获取该View的Animation的Transformation值，然后调用canvas.concat(transformToApply.getMatrix())通过矩阵运算完成动画帧。如果动画没有完成，就继续调用invalidate()函数，启动下次绘制来驱动动画。

    //透明度动画
	AlphaAnimation anim = new AlphaAnimation(0,1);
	//旋转动画
	RotateAnimation anim = new RotateAnimation(0,360,100,100);
	//位移动画
	TranslateAnimation anim = new TranslateAnimation(0,200,0,300);
	//缩放动画
	ScaleAnimation anim = new ScaleAnimation(0,2,0,2);
	aa.setDuration(1000);
	view.startAnimation(aa);
	
	//动画集合
	AnimationSet animationSet = new AnimationSet(true);
	animationSet.setDuration(1000);
	animationSet.add(XXX);
	view.startAnimation(animationSet);

	//监听回调
	animation.setAnimationListener(new Animation.AnimationListener(){
		@override
		public void onAnimationStart(Animation animation){}

		@override
		public void onAnimationEnd(Animation animation){}

		@override
		public void onAnimationRepeat(Animation animation){}

		@override
		public void onAnimationCancel(Animation animation){}
	})

	//Adapter
	animation.addListener(new AnimatorListenerAdapter(){
		@Override
		public void onAnimationEnd(Animator animation){}
	})

## ObjectAnimation ##
是属性动画框架中最重要的实行类。通过静态工厂类传入参数（对象，对象的属性名字），但这个属性必须有get和set函数，因为框架内部会通过Java反射机制来调用set函数修改对象属性值。

以前的动画框架所产生的动画，并不能改变时间相应的位置，只是单纯的修改了显示。而属性动画则不同，由于它真实的改变了一个View的属性，所以时间相应的区域也同样发生了改变。

	ObjectAnimator animator = ObjectAnimator.ofFloat(view,"translationX",300);
	animator.setDuration(300);
	animator.start();

常见的可以直接使用属性动画的值：

- translationX\translationY
	- 控制着View对象从它布局容器的左上角坐标开始的位置
- rotation\rotationX\rotationY
- scaleX\scaleY
- pivotX\pivotY
	- 控制指点位置，默认情况下为View对象的中心点
- x y
	- 描述了在容器中的最终位置rawXY
- alpha

### PropertyValuesHolder ###
类似视图动画中的AnimationSet，在属性动画中，如果针对一个对象的多个属性，要同事作用多种动画，可以通过PropertyValuesHolder来实现
    PropertyValuesHolder pvh1 = PropertyValuesHolder.ofFloat("translationX",300f);
	PropertyValuesHolder pvh2 = PropertyValuesHolder.ofFloat("translationX",1f,0,1f);
	PropertyValuesHolder pvh3 = PropertyValuesHolder.ofFloat("translationY",1f,0,1f);
	ObjectAnimator.ofPropertyValuesHolder(view,pvh1,pvh2,pvh3).setDutarion(1000).start();

## ValueAnimator ##
本身不提供任何动画效果，更像一个数值发生器，用来产生具有一定规律的数字，从而让调用者来控制动画的实现过程。ObjectAnimator就是继承自ValueAnimator

    ValueAnimator animator = ValueAnimator.ofFloat(0,100);
	animator.setTarget(view);
	animator.setDuration(1000).start();
	animator.addUpdateListener(new AnimatorUpdateListener(){
		@Override
		public void onAnimationUpdate(ValueAnimator animation){
			Float value = (Float)animation.getAnimatedValue();
			//TODO
		}
	
	})