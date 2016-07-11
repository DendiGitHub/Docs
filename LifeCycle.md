# LifeCycle #

## Activity ##

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