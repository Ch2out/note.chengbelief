# Vue知识点||js知识点

## 常见问题

<hr>

### 在管理页面或者只需要在页面存在的元素为什么不能和后端交互的东西杂到一起

> vue只能监听到一级对象的增删改操作(且包括二级值得改变)会对页面进行影响刷新页面重新抽离，在我们通过赋值重新对二级对象删除修改时 vue无法监听到 所以在页面操作并不会被监听 也就无法重新抽离
>
> 解决方法<br>通过页面和后端数据交互数据隔 在设计时将仅需要控制页面或者只需要页面显示的时候  我们分开把他分开成两个对象 这样便能很好的处理这个问题

### [VUE this.$router.push跳转页面传值不同 created 函数只调用一次 页面不刷新解决办法](https://blog.csdn.net/alokka/article/details/118614131)

> 或者直接在push后面通过location.reload()来刷新页面

### 通过js实现 我通过电脑端访问一个页面 和手机端访问一个页面显示出不同的页面

```
<script type="text/javascript">
    let system ={};  
       let p = navigator.platform;       
       system.win = p.indexOf("Win") == 0;  
       system.mac = p.indexOf("Mac") == 0;  
       system.x11 = (p == "X11") || (p.indexOf("Linux") == 0);     
       if(system.win||system.mac||system.xll){
               window.location.href="#";  //如果是电脑跳转到
       }else{ 
              window.location.href="#";   //如果是手机,跳转到
       }
</script>
```

通过vue(js)来控制超链接取消跳转

``` js
  
   deleteEmployee:function (event) {
                //通过id获取表单标签
                var delete_form = document.getElementById("delete_form");
                //将触发事件的超链接的href属性为表单的action属性赋值
                delete_form.action = event.target.href;
                //提交表单
                delete_form.submit();
                //阻止超链接的默认跳转行为
                event.preventDefault();
            }

```

## 知识点

### [js中可拓展的形参列表](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/arguments)

