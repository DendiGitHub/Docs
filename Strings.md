# Android string #

## 复数名词 ##

使用getQuanttyStrng(int id,int quantity)

	<!-- strings.xml start -->
	<plurals name="minutes">
    	<item quantity="one">minute</item>
    	<item quantity="other">minutes</item>
	</plurals>
	<!-- strings.xml end -->

	int minutes = Calendar.getInstance().get(Calendar.MINUTE);	
	String text = getResources().getQuantityString(R.plurals.minutes, minutes);


## 文本高浪 ##

>使用html进行高亮

    <string name="html_text" formatted="false">
	<![CDATA[        
	<font color=\'#28b5f5\'>Discover</font> and <font color=\'#28b5f5\'> play </font> games.    
	]]>
	</string>