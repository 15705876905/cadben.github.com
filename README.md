#  记录JavaScript高级程序设计（第三版）学习过程
###  4、变量、作用域
+   基本类型
    -   Undefined
    -   Null
    -   Boolean
    -   Number .toFixed(2)保留两位 toExponential()e表示法
    -   String  
>
+   >基本类型保存在栈内存中，引用类型的值是对象，保存在堆内存。
可以为变量添加属性,不能给基本属性类型的值添加属性，这样不会导致错误。
复制变量,var num2=num1,num1和num2独立，没有任何关系。
JavaScript没有块级作用域，例如if、for语句中的变量或者for i=0的i，会将变量添加到当前执行环境，全局环境中。如果在函数中声明var，则会自动添加到最近的环境中（函数局部环境）外不就获取不到，如果没有var，则会被获取。
局部环境中存在同名标志符，就不会使用父环境中的标识符。


#### 垃圾收集

+   标记清除（很常用）
    >   进入环境，标记“进入环境”，函数变量执行结束，标记“离开环境”。
+   引用计数
    >   引用一次+1，如果为0就回收，会造成内存爆炸。
+   管理内存
    >  将全局变量、对象赋值null来解除引用。
   



###  5、引用类型
####    Object
+   实例类型表示
-   >var peson=new Object();
    person.name="ckf";
    peson.age=20;
-   >var person={
        &nbsp;&nbsp;name:"ckf",
        &nbsp;&nbsp;age:209
    };
-   >person ["name"] === person.name ;

####  Array
数组大小可以动态申请，添加自动增长
+   数组创建
-   >var color=new Array(3);(创建包含3项的数组)
    var n=new Array("ckf");创建好汉1项，即ckf
    var colors=['1','2','3'] (推荐这样)
    var colors=[,,,,,,];会导致不同浏览器 产生不同的个数  不能用
    可以去掉new,可以修改长度，自动移除，和增加长度。(.length(3)，也不要用)
+   转化方法
    -   所有对象都具有 toLocaleString()、toString()、valueOf()
    -   toString() 隐式调用、会自动调用
    -   toLocaleString()可能在数字和时间格式上有差别
    -   使用.join('|')可以改变分隔符

+   栈方法
    -   >var colors=new Array();
        colors.push('red','green');
        colors[2]='black';
        colors.pop();
        以上数组可以看成栈,本质数组
+   队列
    -   >var colors=new Array();
        colors.push('red','green');        
        colors.shift();获取第一项.
+   重排序
    -   .reverse();反转数组
    -   .sort();但是会调用每一个toString（）来比较字符串。
        .sort(compare)编写compare函数可以指定表函数。
+   操作方法
    -   .concat()添加新的数组
    -   .splice(0,2)删除
    -   .splice(2,0,'red');在第二个位置插入 不删除
    -   .splice(2,1,'red');在第二个位置插入 先删除第二个
+   位置方法
    -   .indexOf();获取位置
        .lastIndexOf()从后往前
+   迭代方法
    -   every()每一项满足 返回true 否则false
        >var every=numbers.every(function(item,index,array){
            return item>2;
        });
    -   filter() 返回满足true的项的数组;

    -   forEach() 循环 没有返回值;

    -   map() 返回每次函数调用的结果组成的数组

    -   some() 有一项则返回true
####    Date类型
####    RegExp 正则表达式
+   支持正则表达式的接口

####    function
####    String
+   方法
-   .chartAt()返回字符
-   .charCodeAt()返回字符编号
-   [1]方括号加索引
-   .concat(‘asd’) 拼接
    不会修改字符串本身.substring()负值=0、slice()、substr()
- .trim()删除前置和后 缀的空格
-   .toLocalUpperCase()大写Local(地区)
-   .toUpperCase()
####    Global对象
+   encodeURI() 对于整个URI进行编码，不会对冒号、正斜杆、问号、井字
+   encoudeURIComponent()更多 会进行
####    eval()方法 可能造成代码注入
####    Math对象    
+   min()、max()方法
+   舍入
-   Math.ceil()向上舍入 最接近的数值
-   Math.round()四舍五入
-   Math.floor()向下舍入

+   random()方法
    -   介于0-1之间
    -   >以下是 包括 边界值的函数代码
            function selectFrom(low,upper){
                var c=upper-low+1;
                return Math.floor(Math.random()*c+low);
    }

