# Android Fragments #

### inflate ###
>inflate()的作用就是将一个用xml定义的布局文件查找出来

>inflate是加载一个布局文件,而findViewById是从布局文件中查找一个控件

    LayoutInflater.from(this);
	getLayoutInflater();
	(LayoutInflater)this.getSystemService(LAYOUT_INFLATER_SERVICE);

### ListView ###

1. View	
	- ListView
	- AotuCompleteTextView
	- GridView
	- ExpandableListView
	- AdapterViewFlipper
	- StackView
2. Adapter	用来将数据映射到ListView上的中间
	- ArrayAdapter
		- new ArrayAdapter<String>(Context,id,String[])
	- SimpleAdapter
		- new SimpleAdapter(Context,List<Map<String,Object>>,layoutId,String[],int[])
	- SimpleCursorAdapter
	- BaseAdapter  
		- getCount()
		- getItem(int position)
		- getItemId(int position)
		- getView(int position,View convertView,ViewGroup parent)
3. 数据
	- listView.clearTextFilter()
	- listView.setFilterText(String);


[博客](http://blog.csdn.net/guolin_blog/article/details/44996879)

### ProgressBar ###

style="@android:style/Widget.ProgressBar.

- Horizontal	水平进度条
- Inverse	普通大小的环形进度条
- Large
- Small

>更新操作进度

	setProgress(int)
	incrementProgressBy(int)

#### SeekBar  拖动条 extends ProgressBar####
    seekBar.setOnSeekBarChangeListener(new OnSeekBarChangeListener(){
		@override
		public void onProgressChanged(SeekBar arg0,int progress,boolean fromUser){
		}
	});

#### RatingBar  星级评分条 ####

    android:isIndicator //true为不允许用户更改
	android:numStars  //总共的星级数
	android:rating	//默认的星级
	android:stepSize //每次至少要改变多少个星级

	ratingBar.setOnRatingBarChangeListener(new OnRatingBarChangeListener(){
		@override
		public void onRatingChanged(params...){
			//TODO
		}
	}

### ViewAnimator ###



![](C:\Users\Dendi\Pictures\Android\ViewAnimator.png)

#### ViewSwitcher ####

	android:inAnimation=""
	android:outAnimation=""

    switcher.setFactory(new ViewSwitcher.ViewFactory(){
		@override
		public View makeView(){
			
		}
	})

#### ViewFlipper ####
>与AdapterViewFlipper不用的是需要开发者通过addView(View)添加多个View，而不是传入Adapter


### Toast ###
	//simple toast
    Toast toast = Toast.makeText(Context,String,Toast.LENGTH_SHORT);
	toast.show();

	//更换Toast显示的内容
	toast.setView(View);

### 杂项 ###
#### 获取时间 ####
    Calendar c = Calendar.getInstance();
	int year = c.get(Calendar.YEAR);
	//month,day,hour,minute同理

- CalendarView
- DataPicker

	`dataPicker.init(year,month,day,new OnDataChangedListener(){});`

- TimePicker
- NumberPicker

#### SearchView ####

	//该搜索框是否默认自动缩小为图标
    setIconifiedByDefault(boolean)
	//是否显示搜索按钮
	setSubmitButtonEnabled(boolean)
	//搜索框内默认显示的提示文本
	setQueryHint(CharSequence)
	//事件监听
	setOnQueryTextListener(OnQueryTextListener)

#### TabHost ####
	//xml
    <TabHost
		<TabWidge
		<FrameLayout

	//java
	TabSpec tab1 = tabHost.newTabSpec("tab1")
		.setIndicator("")//标题
		.setContent(R.id.tab01);//内容
	tabHost.addTab(tab1);

>已过时，推荐使用Fragment

#### StrollView ####

>由FrameLayout派生而出，是为普通组件添加滚动条的组件，最多只能包含一个组件，作用就是为该组件添加垂直滚动条
>若需要水平滚动条，可以添加HorizontalScrollView

#### Notification ####
>显示在手机状态栏的通知，代表具有全局效果的通知，一般通过NotificationManager服务来发送Notification

1. 调用getSystemService(NOTIFICATION_SERVICE)方法获取系统的NotificationManager服务
2. 通过构造器创建一个Notification对象
3. 为Notification设置属性
4. 通过NotificationManager发送Notification
>

    //Notification.Builder中的方法

	//设置通知LED灯、音乐、振动等
	setDefaults();

	//点集通知后，状态栏自动删除通知
	setAutoCancel();

	//设置通知标题
	setContentTitle()

	//设置通知内容
	setContentText();

	//为通知设置图标
	setSmallIcon();
	setLargeIcon();

	//设置通知在状态栏的提示文本
	setTicker();

	//设置点击通知后将要启动的程序组件对应的PendingIntent
	setContentIntent()

	setWhen(System.currentTimeMillis());
	
	===========================================================
	/*CODE*/

	Notification notify = new Notification.Builder(this)
	//set操作
	.build();
	
	NotificationManager nm = (NotificationManager)getStstemService(NOTIFICATION_SERVICE);
	//
	nm.notify(NOTIFICATION_ID,notify);
	nm.cancel(NOTIFICATION_ID);


### 对话框 ###

![](C:\Users\Dendi\Pictures\Android\AlertDialog.png)

#### AlertDialog ####
>主要分为四块：图标、标题、内容、按钮

1. 创建AlertDialog.Builder对象
2. 调用AlertDialog.Builder的setTitle()或setCustomTitle()设置标题
3. setIcon()设置图标
4. 设置对话框内容 
	- setMessage()设置对话内容为简单文本
	- setItems()设置对话框内容为简单列表项
	- setSingleChoiceItems()单选列表项
	- setMultiChoiceItems()多选列表项
	- setAdapter()自定义列表项
	- setView()自定义View
5. setPositiveButton()、setNegativeButton()和setNeutralButton()添加按钮
6. create()创建出AlertDialog,使用show()方法显示

#### ProgressDialog ####
    progressDialog = new ProgressDialog(MainActivity.this);
	progressDialog.setTitle("");
	progressDialog.setMessage("");
	progressDialog.setCancelable("");
	progressDialog.setProgressStyle(ProgressDialog.STYLE_HORIZONTAL);
	//设置对话框的进度条是否显示进度
	progressDialog.setIndeterminate(true);
	progressDialog.show();


	progressDialog.setProgress(int);
	progressDialog.setMax(int)
#### Menu ####

>SubMenu:代表一个子菜单，可以包含1~N个MenuItem
>
>ContextMenu:代表一个上下文菜单，可以包含1~N个MenuItem

    /**Menu接口定义
	**通常在add时传入ID便于以后的判断
	**/
	MenuItem add(...);
	SubMenu addSubMenu(...);

	/**SubMenu接口定义**/
	SubMenu setHeaderIcon(...);
	SubMenu setHeaderTitle(...);
	SubMenu setHeaderView(View view);

	/**MenuItem接口定义**/
	setIntent(Intent intent);
	setOnMenuItemClickListener(MenuItem.OnMenuItemClickListener menuItemClickListener);

1. 重写Activity的onCreateOptionsMenu(Menu menu)方法
2. 重写onOptionsItemSelected(MenuItem mi)
	- 通过mi.getItemId()获得ID针对性的做出响应

#### ContextMenu ####
1. 重写Activity的onCreateContextMenu(ContextMenu menu,View source,MenuInfo menuInfo);
2. 调用Activity的registerForContextMenu(View view)为view组件注册上下文菜单
3. 希望为菜单项提供响应，可以重写onContextItemSelected(MenuItem mi)

#### PopupMenu ####
1. 调用new popUpMenu(Context context,View anchor)创建下拉菜单，anchor代表要激发该弹出菜单的组件
2. 调用MenuInflater的inflate()方法将菜单资源填充到PopupMenu中
3. 调用PopupMenu的show()方法显示弹出式菜单

#### ActionBar ####
    android:theme="@android:style/Theme.Material.NoActionBar"

	//item中的属性always always|withText
	android:showAsAction=""

	//是否将应用程序图标转变成可点击的图标,并在图标上增加向左的箭头
	setDisplayHomeAsUpEnabled(boolean)
	//是否显示应用程序
	setDisplayShowHomeEnabled(boolean)
	//是否将应用程序图标转变成可点击的按钮
	setHomeButtonEnabled(boolean)
- 实现Tab导航

1. 调用ActionBar的setNavigationMode(ActionBar.NAVIGATION_MODE_TABS)方法设置使用Tab导航方式
2. 调用ActionBar的addTab()方法添加多个Tab标签，并为每个Tab标签添加事件监听器


### Fragment ###
>总是作为Activity界面的组成部分,Fragment可调用getActivity(),Activity可调用FragmentManager的findFragmentById()或者findFragmentByTag()

>在Activity运行过程中,可调用FragmentManager的add()、remove()、replace()方法动态的增加、删除或替换Fragment


通常来说，需要实现如下三个方法:

1. onCreate()
2. onCreateView()
	- 必须返回View
3. onPause()
	
Activity中使用Fragment

1. 在布局文件中使用<fragment../>
2. 通过FragmentTransaction对象的add()方法来添加Fragment

    Fragment fragment = new ConcreteFragment();
	Bundle arguments = new Bundle();
	//传递信息,可以在Fragment的getArguments()中取得这个Bundle
	fragment.setArguments(arguments);
	
	getFragmentManager().beginTransaction()
		.replace(R.id.FrameLayout,fragment)
		.commit();

传递数据方式:

1. Activity向Fragment
	- 使用fragment.setArguments(Bundle)
2. Fragment向Activity或Activity需要在Fragment运行中进行实时通信
	- 在Fragment中定义一个内部回调接口，再让包含该Fragment的Activity实现该回调接口
>

    public class DendiFragment extends Fragment{
		private Callbacks mCallbacks;
		
		public interface Callbacks{
			public void onItemSelected(Integer id);
		}

		@override
		public void onAttach(Activity activity){
			//TODO
			mCallbacks = (Callbacks)activity;
		}

		@override
		public void onDetach(){
			//TODO
			mCallbacks = null;
		}
	}

FragmentManager

- findFragmentById()、findFragmentByTag()
- popBackStack()将Fragment从后台栈中弹出(模拟用户按下BACK按键)
- addOnBackStackChangeListener()注册监听器,监听后台栈的变化
- 如果需要添加、删除、替换Fragment,则需要借助于FragmentTransaction对象,FragmentTransaction代表Activity对Fragment执行的多个改变

    Fragment newFragment = new ExampleFragment();
	FragmentTransaction transaction = getFragmentManager().beginTransaction();
	//替换container中的Fragment
	transaction.replace(R.id.fragment_container,newFragment);
	//将事务添加到Back栈，允许用户按BACK按键返回到替换Fragment之前的状态
	transaction.addToBackStack(null);
	transaction.commit();
![](http://www.myexception.cn/img/2015/03/31/192604226.jpg)