# LifeCycle #

## Activity ##

- onCreate
- onStart
- onResume
- onPause
	- 失去焦点但是部分可见
- onStop
	- 失去焦点且不可见
	- 应释放用户不适用时不需要的资源，因为极端情况下系统会终止应用进程而不调用Activity的onDestroy回调。
- onDestroy

## 意外销毁时保存Bundle对象 ##
- onSaveInstanceState()
- onCreate()
- onRestoreInstanceState()
	- 在onStart()之后调用，若Bundle为null，则不会被调用


注意onRestart在系统调用onStop()之后再启动

### Activity之间互相传递信息 ###

#### 构建 ####

#### 回退 ####
    
    {
        Intent intent = new Intent();
        intent.putExtra(_KEY , _VALUE);
        setResult(_RESULT_CODE , intent);
    }


    @Override
	protected void onActivityResult(int requestCode, int resultCode, Intent data) {
		super.onActivityResult(requestCode, resultCode,  data);
	}

### A将数据存放在intent中，启动B，则B可以通过getIntent获取，即使APP被销毁亦然 ###

## Fragment ##

## Application ##