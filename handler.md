# Handler #

TODO：有空看一下源码。

## 注意点 ##
- Can't create handler inside thread that has not called Looper.prepare()

## Looper ##
工人，做MessageQueue里面的任务
## Handler ##
传送带，将消息放入MessageQueue
## MessageQueue ##
任务队列，FIFS

![](G:\Private\Handler.bmp)



### Looper.prepare() ###

    public static final void prepare(){
		if(sThreadLocal.get()!=null){
			throw new RuntimeException("Only one Looper may be created per thread");
		}
		sThreadLocal.set(new Looper());
	}

## 标准的异步消息处理线程写法 ##

    class LooperThread extends Thread{
		public Handler mHandler;

		public void run(){
			Looper.prepare();

			mHandler = new Handler(){
				public void handleMessage(Message msg){
					//TODO
				}
			}
		}
	}

>

    //也可以使用ThreadHandler
	HandlerThread handlerThread = new HandlerThread("thread_name");
	handlerThread.start();
	//MyHandler extends android.os.Handler
	mHandler = new MyHandler(handlerThread.getLooper());

## 其他可以进行UI操作的方法 ##
- Handler的post()
- View的post()
- Activity的runOnUiThread()