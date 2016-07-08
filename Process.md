# Process #

## 副作用 ##
- 多线程数据安全
- 死锁
- 内存消耗
- 生命周期管理
- UI的卡顿

## 进程优先级Priority ##
线程寄宿在进程当中，线程的生命周期你直接被进程所影响，进程的存活又与优先级直接相关。

- ForgroundProcess
	- 一般指visible
- BackgroundProcess
	- 一般指invisible
- Empty Process
	- 保存着App相关的Clean Memory

***
按优先级划分：

- Active and visible
- visible 、 with service
- background
- empty

### 拓展IOS中的内存 ###

iOS的世界里，内存被分为Clean Memory和DirtyMemory

- Clean Memory
	- App启动被加载到内存之后原始占用的那一部分内存
	- stack、heap、text、data等segment
- Dirty Memory
	- 由于用户操作所改变的那部分内存，即APP的状态值

系统在Low Memory Warning的时候会首先清理掉Dirty Memory，对用户来说，操作的进度就全部消失了。所以手机重启后app打开的速度都比较慢。

## Thread Scheduling ##
Android系统基于精简过后的linux内核，Linux系统的调度器在分配time slice的时候，采用的CFS（Completely fair scheduler）策略，不仅会参考单个线程的优先级，还会追踪每个线程已经获取到的time slice数量。优先级高的线程不一定能在争取timeslice上有绝对的优势。


Android将线程分为多个group，其中两类比较重要

- default group
	- UI线程
	- 获得绝大部分timeslice
- background group
	- 工作线程
	- 最多分配5~10%的timeslice

Android开发者需要显式的将工作线程归于background group

    new Thread(new Runnable(){
		@Override
		public void run(){
			Process.setThreadPriority(Process.THREAD_PRIORITY_BACKGROUND);
		}
	}).start();

进程的属性变化也会影响到线程的调度，当一个app进入后台的时候，该app所属的整个进程都将进入到background group，以确保处于foreground

## Think twice ##

- 每一个新启的线程至少消耗64KB的内存
- switch context会带来额外的开销
- 尽量重用已有的工作线程

## Thread ##

    new Thread(new Runnable(){
		@Override
		public void run(){
		}
	}).start();



- 启动了新的线程，没有任务的概念，不能做状态的管理。
- start之后，run当中的代码就一定会执行到底，中途无法取消。
- 作为匿名内部类持有了外部类的引用，在线程退出之前，会阻碍GC的回收，在一段时间内造成内存泄露
- 没有线程切换的接口，要传递处理结果到UI线程，需要些额外的线程切换代码
- 如果从UI线程启动，该线程优先级默认为Default


### AsyncTask ###

重写方法:

- doInBackground(Params...)
	- 在后台线程执行，返回Result,执行过程中调用publicProgress(Progress)来进行任务进度的更新
- onPostExecute(Result)
	- 在UI线程中执行
- onProgressUpdate(Progress...)
	- 在主线程执行,在publicProgress方法调用后执行

>在不同的系统版本上串行与并行的执行行为不一致

必须遵守的规则:

1. Task实例必须在UI线程中创建
2. execute方法必须在UI线程中调用
3. 该Task只能被执行一次,多次调用时会出现异常
4. 不要手动调用onPreExecute....等方法

>Thread、AsyncTask适合处理单个任务的场景，HandlerThread适合串行处理多任务的场景。当需要并行的处理多任务时，ThreadPoolExecutor是更好的选择。

    public static Executor THREAD_POOL_EXECUTOR
			= new ThreadPoolExecutor(CORE_POOL_SIZE, MAXIMUM_POOL_SIZE, KEEP_ALIVE,
			TimeUnit.SECONDS, sPoolWorkQueue, sThreadFactory);
