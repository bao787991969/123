
* 指令
	* 是什么？ 自定义的标签属性、标签，类，注释
	* 作用： 在页面进行数据绑定的操作
	* 内置指令：
		ng-app
		ng-model
		ng-controller
		ng-init
		ng-click
		ng-repeat
		ng-bind :
		ng-show
	    ng-hide
		
		ng-class : 动态引用定义的样式       {aClass:true, bClass:false}
	    ng-style : 动态引用通过js指定的样式对象   {color : 'red', background : 'blue'}
	    ng-include ： 动态插入一个模板页面
	
	    ng-click = 'test($event)'
	    ng-blur
	    ng-keydown
	    ng-keyup
	    ng-mouseenter : 移入
	    ng-mouseleave ： 移出
	    ng-mousedown
	    ng-mouseup
	    $event
	* 自定义指令
		* 使用module对象来定义
		* module.directive('指令名', function(){ //工厂函数
			return {                             //配置对象
				restrict : 'ECMA', //指令的类型
				template : '简单的html标签结构',
				templateUrl : '包含模板标签结构的页面',
				replace : true/false  //默认false, 插入在指令标签中， 如果为true指令标签就会被替换
				priority : 10 //优先级 （优先被解析）
				terminal : true/false  是否终止优先级低的指令的解析
				scope ： false  //直接使用外部作用域作为当前作用域
				scope ： true  //新建一个作用域， 继承于外部作用域
				scope : {     //隔离作用域： 新建一个作用域， 但不从外部外部作用域继承
						模板中需要显示： {{msg}}
					msg : '@'     <my-directive msg='tt'>    --->tt
					msg ： '='    <my-directive msg='tt/tt()'>	 ---->将tt作为表达式， 从外部作用域中取对应的属性显示
						模板中需要显示： {{msg({name:'Tom'})}}
					msg : '&'     <my-directive msg='tt(name)'>  ---->将tt(name)作为表达式, 从外部作用域中取对应的方法调用来显示
 				},
				transclude : false/true  //用来解决指令默认会将指令所在的标签的标签体完全的替换， 但有时我们需要留下它们,
				controller : function($scope, $element, $attrs, $transclude) {  //在初始化时执行一次
                        this.xxx = function(){};
                }
				link : function (scope, elements, attrs, controller) {    //在初始化时执行一次
                       //elements[0]--->指令所在的标签对象
                       //scope---->向scope中添加属性和方法
                       //controller.xxx()
                    }
                require : 找哪个指令的controller对象, 并注入link中来
			}
		})
* 视图与路由
	* angular中用来实现SPA的技术
	* 在不进行页面跳转的情况下, 进行页面的局部更新显示(当请求某个路由路径时, 发送是ajax请求)
	* 使用angular-route.js
		* 将angular-route.js复制到当前项目中
		* 在页面中引入
		* 在创建module时依赖'ngRoute'
	* 在主页中指定<ng-view>/<div ng-view>, 它是用来包含显示不同页面内容的容器
	* 如何得到其它页面： ajax请求（局部刷新， 没有页面跳转）
	* 路由： $routeProvider服务
		module.config(function($routeProvider){
				$routeProvider.when('/xx/yy', {      //发请求：  #/xx/yy?name=Tom
						templateUrl : 'aa.html',
						controller : 'AController'
					})
					.when('/xx/yy/:name', {      //发请求：  #/xx/yy/tom
						templateUrl : 'bb.html',
						controller : 'BController'
					})
					.otherwise('/xx/yy')
		})
		.controller('AController', function($scope){
			//给$scope添加属性、方法  ----》给aa.html使用
		})
		.controller('AController', function($scope, $routeParams){
			$scope.name = $routeParams.name
					//给$scope添加属性、方法  ----》给bb.html使用
		})			
    * spa的整体文件结构
    |- index.html : <ng-view>
    |- controller  : 控制器模块/应用模块对象
    |- route : 路由模块
    |- templates : 模板页面
    |- services : 自定义服务
    |- directive : 自定义指令
    |- filters : 自定义过滤器
        
        