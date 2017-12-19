#  mockJS 的使用

### mock.js的使用(用于拦截ajax的请求生成模拟数据)
	1. 使用浏览器中直接加载页面中的数据
	2. 使用node.js
	3. 在angular框架中的使用



### 在浏览器中的使用网络资源
	
	- 模拟代码:
		- <!-- （必选）加载 Mock -->
			<script src="http://mockjs.com/dist/mock.js"></script>
			<script>
			// 使用 Mock
			var data = Mock.mock({
			    'list|1-10': [{
			        'id|+1': 1
			    }]
			});
			$('<pre>').text(JSON.stringify(data, null, 4))
			.appendTo('body')
			</script>
	- 返回值
		- {
			"list": [
			    {
			        "id": 1
			    },
			    {
			        "id": 2
			    },
			    {
			        "id": 3
			    }
			    ]
			}
	- JQuery 配置模拟数据
		- 
		Mock.mock('http://g.cn', {
		    'name'     : '@name',
		    'age|1-100': 100,
		    'color'    : '@color'
		});
	-发送ajax请求
		$.ajax({
		    url: 'http://g.cn',
		    dataType:'json'
		    }).done(function(data, status, xhr){
		    console.log(
		    JSON.stringify(data, null, 4)
		    )    
		})；

	- 返回数据
		- // 结果1
			{
			"name": "Elizabeth Hall",
			"age": 91,
			"color": "#0e64ea"
			}
			
			// 结果2
			{
			"name": "Michael Taylor",
			"age": 61,
			"color": "#081086"
			}

### 转载自    [mockJS](https://segmentfault.com/a/1190000003087224 "mockJS")