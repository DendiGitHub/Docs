# RecyclerView #

## 几个重要组件 ##
RecyclerView只负责回收与复用，其他的必须由自己去设置，因此具有高度的解耦性。

- LayoutManager
	- 布局管理器控制其显示的方式
- ItemDecoration
	- 控制Item间的间隔
- ItemAnimator
	- 控制Item增删的动画
>

    mRecyclerView = findView(R.id.recyclerview);
	//设置布局管理器
	mRecyclerView.setLayoutManager(layout);
	//设置adapter
	mRecyclerView.setAdapter(adapter);
	//设置Item增加、移除动画
	mRecyclerView.setItemAnimator(new DefaultItemAnimator());
	//添加分割线
	mRecyclerView.addItemDecoration(new DividerItemDecoration(getActivity(),DividerItemDecoration.HORIZONTAL_LIST);

>
### Adapter ###

RecyclerView已封装好了ViewHolder，只需要实现功能即可

Android并没有给RecyclerView增进点击事件，所以我们需要自己使用接口回调机制进行实现。

    //关于Adapter
	class ConcreteAdapter extends RecyclerView.Adapter<RecyclerAdapter.MyViewHolder>{

		private List<String> mData;

		@Override
		public ViewHolder onCreateViewHolder(ViewGroup viewGroup,int i){
			MyViewHolder holder = new MyViewHolder(
			LayoutInflater.from(XXXActivity.this).inflate(R.layout.xx,parent,false));
			return holder;
		}

		@Override
		public void onBindViewHolder(MyViewHolder holder,int position){
			holder.tb.setText(mDatas.get(mDatas.get(positions)));
		}

		@Override
		public int getItemCount(){
			return mDatas.size();
		}

		class MyViewHolder extends ViewHolder{
			TextView tv;

			public MyViewHolder(View view){
				super(view);
				tv = (TextView) view.findViewById(R.id.xxx);
			}
		}

	}

### ItemDecoration ###

通过mRecyclerView.addItemDecoration()添加分隔线
	
    public static abstract class ItemDecoration{

		public void onDraw(Canvas c,RecyclerView parent,State state){
			onDraw(c,parent);
		}

		public void onDrawOver(Canvas c,RecyclerView parent,State state){
			onDrawOver(c,parent),((LayoutParams) view.getLayoutParams()).getViewLayoutPosition(),parent);
		}

		public getItemOffsets(Rect outRect,View view,RecyclerView parent, State state){
			getItemOffsets(outRect,
		}


	}