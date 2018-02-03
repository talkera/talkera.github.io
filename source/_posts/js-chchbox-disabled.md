---
title: JavaScript设置checkbox不生效
date: 2016-10-11
categories: 工作点滴
tags: [javascript]
---
有段代码用jquery设置checkbox总是不生效。并不是常见的那种checked存不存在的问题，而是出在了dom结构上。

```
<!--dom-->
<label>
  <input name="brands[]" type="checkbox" value="1">
  <div class="type-unit">tag1</div>
</label>
<label>
  <input name="brands[]" type="checkbox" value="1">
  <div class="type-unit">tag1</div>
</label>

<script>
$('.type-unit').on('click',function(){
	var checkbox = $(this).prev();
	if($(this).hasClass('active')){
		checkbox.prop('checked',false);
		$(this).removeClass('active');
	}else{
		checkbox.prop('checked',true);
		$(this).addClass('active');
	}
});
</script>
```

这段代码的问题在于，每次.type-unit元素被click后，会对checkbox进行两次操作。

label这个标签比较特殊，label内的所有元素被点击都将对checkbox进行操作。由于绑定了click时间，也会调用jquery对checkbox操作，因此无法生效。
这种情况下只要去掉jquery对checkbox的处理即可。

```
$('.type-unit').on('click',function(){
	if($(this).hasClass('active')){
		$(this).removeClass('active');
	}else{
		$(this).addClass('active');
	}
});
```

其实label标签一般做如下用法：

```
<input name="brands[]" id="val1" type="checkbox" value="1">
<label for="val1">tag1</label>
<input name="brands[]" id="val2" type="checkbox" value="1">
<label for="val2">tag1</label>
```

在label中使用for为label绑定目标，for的值为目标元素的id，这样点击label或点击checkbox都会生效。

这种用法不仅用于checkbox，select，radio等同样适用。
