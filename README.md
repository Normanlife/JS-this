# JS-/闭包/作用域/this
关于这三者的实际代码理解
# 作用域
    var t = 100;
    function test() {
        console.log(t);
        var t;
    }
    test(); // undefined ,因为var会前置，也就是函数内部会先var t;再console.log(t)，然后函数内部会先读取内部的变量，故输出undefined
# 闭包
    function test() {
        var k = 100;
        return function() {
            return k;
        }
    }
    var t = test()();
    console.log(t); //100
    //闭包：拿到了本不该拿到的东西，如果var t=test(),输出的是return的函数;闭包需要再加一个();
    //缺点：容易造成内存泄漏，JS在前端泄漏不会很明显，在后端nodejs会比较明显
# this指针
## 情况1
    this.m = 1000;
    function test3() {
        var m = 1;
        console.log(this.m);
    }
    test3(); //1000;因为 test3()被挂载在window上面，也就是最外面的1000; 最外面的this.m===window.m
## 情况2    
    this.n = 1;
    var obj = {
        n: 1000,
        test4: function() {
            console.log(this.n)
        }
    }
    obj.test4(); //1000, 因为这时候test4()被挂载在obj上，读取的n也要读取obj里面的n,而不是window的n
## 情况3  
    this.n1 = 1;
    var obj1 = {
        n1: 1000,
        test5: function() {
            console.log(this.n1);
            return function() {
                console.log(this.n1);
            }
        }
    }
    obj1.test5()(); //1000 & 1 先输入第一个为1000，同上原理，第二个为闭包，this被抛到最外层，所以指向window,不再指向对象 boj1 ，所以结果输出为 1;
## 情况4    
    <input type="button" id="input" style="color: red">
    <script type="text/javascript">
    var style = {
        color: "green"
    }
    var input = document.getElementById('input');
    input.onclick = test6; //点击以后console.log=>red ;因为此时的test6()被绑定在input上面
    test6(); // green;因为绑定在window上
    
    function test6() {
        console.log(this.style.color)
    }
    </script>
## 情况5    
    this.a = 1000;
    function test7() {
        this.a = 1
    }
    test7.prototype.geta = function() {
        return this.a
    }
    var p = new test7
    console.log(p.geta());//1 因为拍的geta挂载在了test7的原型链上，this指向test7，所以读取test7里面的值
    
# 构造函数主动权比较大
    function test8(){
    		this.a=1
    	}
    	test8.prototype.a=1000;
    	var p=new test8;
    	console.log(p.a);//1 同理，p.a挂载在test8的原型链即构造函数上，构造函数的主动权比window大，所以值取构造函数里面的值
    
    
