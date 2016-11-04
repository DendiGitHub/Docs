# Fragment #

> 总是作为Activity界面的组成部分,Fragment可调用getActivity(),Activity可调用FragmentManager的findFragmentById()或者findFragmentByTag()

> 在Activity运行过程中,可调用FragmentManager的add()、remove()、replace()方法动态的增加、删除或替换Fragment

通常来说，需要实现如下三个方法:

1. onCreate()
2. onCreateView()
   - 必须返回View
3. onPause()

Activity中使用Fragment

1. 在布局文件中使用<fragment../>
2. 通过FragmentTransaction对象的add()方法来添加Fragment

```
Fragment fragment = new ConcreteFragment();
Bundle arguments = new Bundle();
//传递信息,可以在Fragment的getArguments()中取得这个Bundle
fragment.setArguments(arguments);

getFragmentManager().beginTransaction()
	.replace(R.id.FrameLayout,fragment)
	.commit();
```

传递数据方式:

1. Activity向Fragment
   - 使用fragment.setArguments(Bundle)
   - 在fragment的onCreate()方法中使用getArgument()
2. Fragment向Activity或Activity需要在Fragment运行中进行实时通信
   - 在Fragment中定义一个内部回调接口，再让包含该Fragment的Activity实现该回调接口

```
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
```

FragmentManager

- findFragmentById()、findFragmentByTag()
- popBackStack()将Fragment从后台栈中弹出(模拟用户按下BACK按键)
- addOnBackStackChangeListener()注册监听器,监听后台栈的变化
- 如果需要添加、删除、替换Fragment,则需要借助于FragmentTransaction对象,FragmentTransaction代表Activity对Fragment执行的多个改变

```
Fragment newFragment = new ExampleFragment();
FragmentTransaction transaction = getFragmentManager().beginTransaction();
//替换container中的Fragment
transaction.replace(R.id.fragment_container,newFragment);
//将事务添加到Back栈，允许用户按BACK按键返回到替换Fragment之前的状态
transaction.addToBackStack(null);
transaction.commit();
```

![](http://www.myexception.cn/img/2015/03/31/192604226.jpg)



## 一些坑

### 内存重启

系统在把APP回收之前，会把Activity的状态保存下来，Activity的FragmentManager负责把Activity中的Fragment保存起来。在“内存重启”后，Activiity的恢复是从栈顶逐步恢复，Fragment会在宿主Activity的onCreate方法调用后紧接着恢复。

### getActivity()返回null

> 异步任务中getActivity时，当前的Fragment已经onDetach()了宿主
>
> 在onCraete()中出现getActivity()的代码，很可能是危险的
>
> 应该尽量避免在Fragment已经onDetach后，再去使用其宿主Activity对象
>
> 如下方法可避免空指针，但使用不当可能造成内存泄露
>
> ```
> protected Activity mActivity;
>
> @Override
> public void onAttach(Activity activity){
>   	super.onAttach(activity);
>   	this.mActivity = activity;
> }
>
> ```

### Fragment重叠

FragmentManager帮我们管理Fragment，每当我们离开该Activity，FragmentManager都会保存它的Fragments，当被意外清理掉时，会从栈底向栈顶的顺序恢复Fragments，并且全部都是以show()的方式，所以会看到界面重叠

```
@Override
protected void onCreate(Bundle savedInstanceState){
  	super.onCreate(savedInstanceState);
  	setContentView(R.layout.activity);
  	
  	//可以使用findViewByTag或者getFragments()获取栈内的所有Fragment
  	if(savedInstanceState != null){
      	getFragmentManager().beginTransaction().show(XX).hide(XX).commit();
  	}
}
```

### 关于Transaction

* hide()、show()只是setVisibility，不会调用声明周期，如onStop()，会调用onHiddenChanged()
* replace()的话会销毁视图，即调用onDestroyView,onCreateView等一系列声明周期
* add（）和replace()不要在同一个阶级的FragmentManager里混搭使用

### 出栈

* 让Fragment出栈，使用remove()并不靠谱，它不能真正将Fragment从栈中移除，只有popBackStack()系列方法才能真正出栈。
* popBackStack的作用是，出栈到tag/id的fragment，即依次多个Fragment被出栈
* 多个Fragment同时出栈，再有fragment入栈，可能会导致栈内顺序异常。
  * 是由FragmentManagerImpl的mAvailIndices导致的
* popBackStack是加入到主线末尾进行处理，会在popBackStack多个Fragment后，紧接着beginTransaction（）增加新的Fragment，再内存重启后，执行popBackStack()后app将crash()

### 转场动画

```
//如果有通过tag/id同时出栈的情况，请谨慎使用.setCustomAnimations(enter,exit,popExter,popExit),
//因为会在某些情况下发生异常，还需要搭配Fragment的onCreateAnimation()临时取消出栈动画
getFragment().beginTransaction().setCustomAnimation(enter,exit);
```

* 使用pop出栈多个Fragment的情况下，务必不能设置转场动画
* ​