  以下我们将为大家介绍 JavaScript 保留两位小数的实现方法:
四舍五入
以下处理结果会四舍五入:
var num =2.446242342;
num = num.toFixed(2); // 输出结果为 2.45

不四舍五入
以下处理结果不会四舍五入:
第一种，先把小数边整数：
Math.floor(15.7784514000 * 100) / 100
// 输出结果为 15.77

第二种，当作字符串，使用正则匹配：
Number(15.7784514000.toString().match(/^\d+(?:\.\d{0,2})?/))
// 输出结果为 15.77,不能用于整数如 10 必须写为10.0000

注意：如果是负数，请先转换为正数再计算，最后转回负数
javascript保留两位小数的实例：
<script type="text/javascript">
//保留两位小数
//功能：将浮点数四舍五入，取小数点后2位
function toDecimal(x) {
var f = parseFloat(x);
if (isNaN(f)) {
return;
}
f = Math.round(x*100)/100;
return f;
}

//制保留2位小数，如：2，会在2后面补上00.即2.00
function toDecimal2(x) {
var f = parseFloat(x);
if (isNaN(f)) {
return false;
}
var f = Math.round(x*100)/100;
var s = f.toString();
var rs = s.indexOf('.');
if (rs < 0) {
rs = s.length;
s += '.';
}
while (s.length <= rs + 2) {
s += '0';
}
return s;
}

function fomatFloat(src,pos){
return Math.round(src*Math.pow(10, pos))/Math.pow(10, pos);
}
//四舍五入
alert("保留2位小数：" + toDecimal(3.14159267));
alert("强制保留2位小数：" + toDecimal2(3.14159267));
alert("保留2位小数：" + toDecimal(3.14559267));
alert("强制保留2位小数：" + toDecimal2(3.15159267));
alert("保留2位小数：" + fomatFloat(3.14559267, 2));
alert("保留1位小数：" + fomatFloat(3.15159267, 1));

//五舍六入
alert("保留2位小数：" + 1000.003.toFixed(2));
alert("保留1位小数：" + 1000.08.toFixed(1));
alert("保留1位小数：" + 1000.04.toFixed(1));
alert("保留1位小数：" + 1000.05.toFixed(1));

//科学计数
alert(3.1415.toExponential(2));
alert(3.1455.toExponential(2));
alert(3.1445.toExponential(2));
alert(3.1465.toExponential(2));
alert(3.1665.toExponential(1));
//精确到n位，不含n位
alert("精确到小数点第2位" + 3.1415.toPrecision(2));
alert("精确到小数点第3位" + 3.1465.toPrecision(3));
alert("精确到小数点第2位" + 3.1415.toPrecision(2));
alert("精确到小数点第2位" + 3.1455.toPrecision(2));
alert("精确到小数点第5位" + 3.141592679287.toPrecision(5));
</script>  