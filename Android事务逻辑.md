# Android事务逻辑 #

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


- standard

- singleTop
>判断当前栈顶是否是要启动的Activity,若是则调用onNewIntent()方法




- singleTask
>整个任务栈中Activity唯一,会将Activity上的任务都销毁。如果Activity已经在后台一个任务栈中了,那么启动后,后台的这个任务栈将一起被切换到前台。

>可以用来退出整个应用,将主Activity设为singleTask,在onNewIntent()中加上finish()




- singleInstance
>Activity会出现在一个新的任务栈中,并且该任务栈只存在这一个Activity,常用于需要与程序分离的界面。


不同Task之间不能传递数据的,如果一定要传递,只能通过Intent来绑定数据。
如果在singleTop和singleInstance中通过startActivityForResult()方法来启动另一个Activity,那么系统将直接返回Activity.RESULT_CANCELED而不会再去等待返回。


### IntentFlag ###

-  Intent.FLAG_ ACTIVITY_ NEW_ TASK
>使用一个新的Task来启动一个Activity,通常使用在从Service中启动Activity的场景,由于在Service中并不存在Activity栈,所以使用该Flag来创建一个新的Activity栈



-  FLAG_ ACTIVITY_ SINGLE_ TOP
>同singleTop



- FLAG_ ACTIVITY_ CLEAR_ TOP
>singleTask



-  FLAG_ ACTIVITY_ NO_ HISTORY
>使用这种模式启动Activity,当该Activity启动其他Activity后,该Activity就消失了,不会保留在Activity栈中。

### 清空任务栈 ###
activity中可以添加如下标签

- clearTaskOnLaunch
>每次返回该Activity时,都将该Activity之上的所有Activity都清除


- finishOnTaskLaunch
>当离开这个Activity所处的Task,那么当用户再返回时,该Activity会被finish掉


- alwaysRetainTaskState
>该Activity所在的Task将不接受任何清理命令，一直保持当前Task状态

### Timer ###

    Timer timer = new Timer();
	TimerTask task = new TimerTask(){
		public void run(){}
	}
	//从1秒钟开始，每5秒执行一次Task
	timer.schedule(task,1000,5000);

### PendingIntent ###
>Intent是及时启动，随所在的Activity消失而消失
>
>PendingIntent可以看做是对intent的包装，通常通过PendingIntent.getActivity()、getBroadcast()、getService()来得到pendingIntent的实例
>
>并不马上启动，而是在外部执行pendingintent时才进行调用。里面包含当前App的Context,使它赋予外部App可以如同当前App一样的执行Intent。

>可以理解为封装给别人执行或者延迟执行的Intent


### 资源文件的使用 ###

	//id的获取
	//in XML
	@[<package_name>:]<resource_type>/<resource_name>
	//in Code
	@<package_name>.R.<resource_type>.<resource_name>

    Resource res = getResources();
	res.getString(int id);
	res.getDeminsion(int id);
	res.getInteger(int id);

### 动态获取id ###

   	    int ridInt = mContext.getResources().getIdentifier(sb.toString(),
                "string", mContext.getPackageName());
        if (ridInt == 0) {
            resultString = "未定义";
        } else {
            resultString = mContext.getString(ridInt);
        }


### 获取版本号 ###

    PackageManager manager = this.getPackageManager();
	PackageInfo info = manager.getPackageInfo(this.getPackageName(),0);
	String version = info.versionName;


### 国际化和本地化 ###
- Internationalization = I18N
- Localization = L10N

国际化思路：
>将程序中的标签、提示等信息放在资源文件中,程序需要支持哪些国家、语言环境，就提供响应的资源文件，资源文件是key-value对，每个资源文件中的key是不变的,但value则随不用国家、语言改变。

资源文件的命名:
>BaseName是资源文件的基本名,language和country必须是Java所支持的语言和国家
>
- baseName_language_country.properties
- baseName_language.properties
- baseName.properties

相关类：

- ResourceBundle:用于加载一个国家、语言资源包
- Locale：用于封装一个特定的国家/区域、语言环境
- MessageFormat:用于格式化带占位符的字符串
 >

    public static void main(String args[]){
		//取得系统默认的国家、语言环境
		Locale myLocale = Locale.getDefault(Locale.Category.FORMAT);
		//根据指定国家、语言环境加载资源文件
		ResourceBundle bundle = ResourceBundle.getBundle("mess",myLocale);
		//输出从资源文件中取得的消息
		System.out.println(bundle.getString("hello"));
	}
	
	// mess_zh_CN.properties
	hello = 您好！
	// mess_en_US.properties
    hello = Welcome!

	//非西欧字符的资源文件
	native2ascii 源资源文件 目的资源文件


提供国际化资源：

- 文件夹命名方式
	- values-语言代码-r国家代码
	- drawable-语言代码-r国家代码