# Android绘图机制 #
## 基础概念 ##
- 屏幕大小
	- 对角线长度，如5.5寸手机
- 分辨率
	- 手机屏幕的像素点个数
- PPI
	- 每英寸像素Pixels Per Inch， 又被称为Dots Per Inch，由对角线的像素点除以屏幕的大小得到

>度量单位
>
- dp\dip
	- 设备独立像素
- px、pixels
	- 像素，不同设备显示效果相同
- pt、point
	- 标准长度单位
- sp:scaled pixels
	- 放大像素，主要用于字体显示
- in
	- 英寸
- mm

## Layer图层 ##
>使用saveLayer()方法来创建一个图层，图层是基于栈结构进行管理的

>通过saveLayer()方法、saveLayerAlpha()方法将一个图层入栈，使用restore()方法、restoreToCount()方法将一个图层出栈。

>入栈时，后面所有的操作都发生在这个图层上，而出栈的时候，则会把图像绘制到上层Canvas上。

    @override
	protected void onDraw(Canvas canvas){
		canvas.drawColor(Color.WHITE);
		mPaint.setColor(Color.BLUE);
		canvas.drawCircle(150,150,100,mPaint);

		canvas.saveLayerAlpha(0,0,400,400,127,LAYER_FLAGS);
		mPaint.setColor(Color.RED);
		canvas.drawCircle(200,200,100,mPaint);
		canvas.restore();
	}