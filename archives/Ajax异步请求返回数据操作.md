#Ajax 异步请求返回数据操作
语法：`jQuery.post(url,data,success(data, textStatus, jqXHR),dataType)`等价于：
```
$.ajax({
  type: 'POST',
  url: url,                          //必需。规定把请求发送到哪个 URL。
  data: data,                        //可选。映射或字符串值。规定连同请求发送到服务器的数据。
  success(data, textStatus, jqXHR),  //可选。请求成功时执行的回调函数。对于 jQuery 1.5，也可以向 success 回调函数传递 jqXHR 对象（jQuery 1.4 中传递的是 XMLHttpRequest 对象）。
  dataType: dataType                 //可选。规定预期的服务器响应的数据类型。默认执行智能判断（xml、json、script 或 html）。
});
```
##实例：
一、前台Ajax发送数据
```
$.ajax({
    type: "post",
    url: "/countAjax",
    data: {},
    success:callback,
    error: function (json) {
        console.log(json)
        //alert('您的信息填写不正确，请重新提交！')
    }
});
```  
二、后台PHP接收请求，并返回数据    
```
public function actionCount(){
    $model = new MmMsg();
    $mmNum=  MmMsg::find()->count('id');  // 统计符合条件的总条数；
    @header('Content-Type: application/json; charset=UTF-8');
    $response = array('success' => true,'count'=>$mmNum);
    echo json_encode($response);
    exit;
}
```
三、前台接收返回的json数据
```
function callback(jsondata) {
    var $n=jsondata.count;
}

```
##范例：前台发送并接收返回数据
```
$.post("sever.php", { "name": "pich" },
   function(data){
     alert(data.name); 
   }, "json");
```