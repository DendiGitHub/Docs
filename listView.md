# ListView #

## 使用ViewHolder重用缓存 ##

>避免每次都通过findViewById()实现控件，能提高50%以上的效率
    
	public final class ViewHolder{
		public ImageView img;
		public TextView title;
	}

	@override
	public View getView(int position,View convertView,ViewGroup parent){
		ViewHolder holder = null;
		if(convertView == null){
			holder = new ViewHolder();
			convertView = mInflater.inflate(R.layout.viewholder_item,null);
			holder.img = (ImageView)convertView.findViewById(R.id.imageView);
			holder.title=(TextView)convertView.findViewById(R.id.textView);
			convertView.setTag(holder);
		}else{
			holder = (ViewHolder)convertView.getTag();
		}
		holder.img.setBackgroundResource(R.drawable.ic_launcher);
		holder.title.setText(mData.get(position));
		return convertView;
	}

## 设置项目分割线 ##

    android:divider="@android:color/darker_gray"
	android:dividerHeight="10dp"

## 隐藏ListView的滚动条 ##

	android:scrollbars="none"
## 取消ListView的Item点击效果 ##

	android:listSelector="#00000000"
	android:listSelector="color/transparent"

## 设置ListView需要显示在第几项 ##

	listView.setSelection(N);

	方法原理类似scrollTo，是瞬间完成的移动。除此之外，还可以使用如下代码来实现平滑移动：
	mListView.smoothScrollBy(distance,duration);
	mListView.smoothScrollByOffset(offset);
	mListView.smoothScrollToPosition(index);

## 动态修改ListView ##

当然可以通过重新设置ListView的Adapter来更新ListView，但这需要重新获取数据，相当于重新创建ListView，显然不是很友好。

可以修改传递给Adapter的映射List，然后通过调用Adapter的notifyDataSetChanged()方法，通知ListView更改数据源即可完成对ListView的动态修改。

	public void btnAdd(View view){
		mData.add("new");
		mAdapter.notifyDataSetChanged();
		mListView.setSelection(mData.size()-1);
	}

	@override
	protected void onCreate(Bundle savedInstanceState){
		super.onCreate(savedInstanceState);
		setContentView(R.layout.notify);

		mData = new ArrayList<String>();
		for(int i=0;i<20;i++){
			mData.add(""+i);
		}

		mListView = (ListView)findViewById(R.id.listView);
		mAdapter = new NotifyAdapter(this,mData);
		mListView.setAdapter(mAdapter);

		//遍历ListView中的所有Item
		for(int i=0;i<mListView.getChildCount();i++){
			View view = mListView.getChildAt(i);
		}
	}

## 处理空ListView ##
>当列表中午数据时，ListView不会显示任何数据或提示，按照完善用户体验的要求，应该给以无数据的提示。

    ListView listView = (ListView)findViewById(R.id.listview);
	listView.setEmptyView(findViewById(R.id.empty_view));
    
	

	