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
