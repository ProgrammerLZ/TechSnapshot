```javascript
$.ajax({
                   url: url,
                   type: 'POST',
                   data: posFormData,
                   contentType: false, //禁止设置请求类型
                   processData: false, //禁止jquery对DAta数据的处理,默认会处理
                   beforeSend: function(){
                       //返回的参数item，即为当前的input DOM对象
                       index = layer.load(1,{shade: [0.3,'grey']});
                   },
                   success: function(data) {
                        if (typeof data != "object") {
                           jsonReturn = eval("("+data+")");
                         }else{
                             jsonReturn=data;
                         }
                          //关闭遮罩层
                          layer.close(index);
                         if(jsonReturn.code == "true"){
                            layer.msg(jsonReturn.message,{icon:1,time: 1000},function(){
                               f_cancel();
                               top.f_getframe("goods_index_do").f_goods_sale_query();
                            });
                         }else{
                            layer.msg(jsonReturn.message,{icon: 7,time: 2000});
                         }
                   }

         });
```

