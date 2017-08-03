# 任何类型的值都可以调用Boolean()函数，转换为boolean值

	var message='hello';
	var msgBool = Boolean(message);

|数据类型 |转换为true |转换为false|
|---------|-----------|-----------|	
|Boolean  |true		  |false      |
|Number   |非0数值    |0、NaN     |
|String   |非空字符串 |" "、undefined|
|Object   |任何对象   |null       |

对于boolean的转换规则，在流式控制语句（比如if）很重要，比如下面两个例子输出不一样：

	var message='hello';
	if(message){
		console.log('true');
	}
	//输出‘true’

	if(a==true){
    	console.log("true");
    }else{
    	console.log("false");
    }
	//输出‘false’

可以参考[==与====的判断规则](/==和===的区别及转换规则.md)
	

	




