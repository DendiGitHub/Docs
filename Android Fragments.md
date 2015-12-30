# Android Fragments #

### ListView ###

1. ListView	
用来展示列表的View
2. 适配器	用来将数据映射到ListView上的中间
3. 数据


### ANR ###
Application not responding

- 主线程在5秒内没有响应输入时间
- BroadcastReceiver没有在10秒内完成返回

在主线程和BroadcastReceiver中进行网络操作、缓慢的磁盘操作、高耗时的计算


### AsyncTask ###

重写方法:

- doInBackground(Params...)
	- 在后台线程执行，返回Result,执行过程中调用publicProgress(Progress)来进行任务进度的更新
- onPostExecute(Result)
	- 在UI线程中执行
- onProgressUpdate(Progress...)
	- 在主线程执行,在publicProgress方法调用后执行



必须遵守的规则:
1. Task实例必须在UI线程中创建
2. execute方法必须在UI线程中调用
3. 该Task只能被执行一次,多次调用时会出现异常
4. 不要手动调用onPreExecute....等方法

### 事件的分发处理 ###



- 事件传递:由上到下,由大到小。
	- dispatchTouchEvent()
	- onInterceptTouchEvent(),返回true则拦截



- 事件处理:由下到上,由小到大
	- onTouchEvent(),返回true则不用审核

### AndroidMainifest启动模式 ###
1. standard
2. singleTop
>判断当前栈顶是否是要启动的Activity,若是则调用onNewIntent()方法

3. singleTask
>整个任务栈中Activity唯一,会将Activity上的任务都销毁。如果Activity已经在后台一个任务栈中了,那么启动后,后台的这个任务栈将一起被切换到前台。

>可以用来退出整个应用,将主Activity设为singleTask,在onNewIntent()中加上finish()


4. singleInstance
>Activity会出现在一个新的任务栈中,并且该任务栈只存在这一个Activity,常用于需要与程序分离的界面。


不同Task之间不能传递数据的,如果一定要传递,只能通过Intent来绑定数据。
如果在singleTop和singleInstance中通过startActivityForResult()方法来启动另一个Activity,那么系统将直接返回Activity.RESULT_CANCELED而不会再去等待返回。


### IntentFlag ###
1. Intent.FLAG_ACTIVITY_NEW_TASK
>使用一个新的Task来启动一个Activity,通常使用在从Service中启动Activity的场景,由于在Service中并不存在Activity栈,所以使用该Flag来创建一个新的Activity栈


2. FLAG_ACTIVITY_SINGLE_TOP
>同singleTop


3. FLAG_ACTIVITY_CLEAR_TOP
>singleTask


4. FLAG_ACTIVITY_NO_HISTORY
>使用这种模式启动Activity,当该Activity启动其他Activity后,该Activity就消失了,不会保留在Activity栈中。

### 清空任务栈 ###
activity中可以添加如下标签
- clearTaskOnLaunch
>每次返回该Activity时,都将该Activity之上的所有Activity都清除


- finishOnTaskLaunch
>当离开这个Activity所处的Task,那么当用户再返回时,该Activity会被finish掉


- alwaysRetainTaskState
>该Activity所在的Task将不接受任何清理命令，一直保持当前Task状态