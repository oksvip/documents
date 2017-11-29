[TOC]

#### JavaScript笔记

##### 1、声明变量

```javascript
var i="hello";
```

##### 2、打印语句

```javascript
console.log(i);
```

##### 3、基本数据类型

```javascript
undefined：未定义类型，该类型只有一个值undefined；
null 空，该类型也只有一个值null；
boolean 布尔值，true和false；
number 数字，整型和小数皆可；
string 字符串，单引号和双引号皆可。
```

##### 4、打印数据类型

```javascript
var text = 12;
console.log(type of text);
```

##### 5、Number转String(数值型转字符型)

```javascript
var num = 23;
tostring(num.tostring());
/*
toString还可以接受三种参数：2、8、16，可以分别将数字转换为对应的进制，
并以字符串显示console.log(070.toString(8));
*/
```

##### 6、类型转换

```javascript
var str = '34';
console.log(parseInt(str));
console.log(parseFloat(str));
/* 
String转Number（字符型转数值型）,
parseInt用来转换为整型；
parseFloat用来转换为浮点型,只有String类型的数字才可以进行转换，如果是将一个普通的字符串转换为整型，得到的结果是NaN（非数字）
*/
```

##### 7、强制类型转换

```javascript
Boolean(value);
String(value);
Number(value);
```

##### 8、数组

```javascript
var arr = ['blue','red','yellow'];
console.log(arr);
//通过下标可以获取到数组的元素：colors[1]为red这跟PHP的数字索引数组是一样的实际上，在JavaScript中，数组是一个对象，因此所有数组的操作都是在数组后面添加.号来调用对象中的方法实现例如：arr.length
```

##### 9、数组-元素添加删除

```javascript
var a = [];
a[0] = "zero";
a[1] = "one";
a[2] = "three";
a.push("four");
delete a['1']; //a在索引1的位置不再有原素
console.log(1 in a);  //false
console.log(a.length); //delete操作不影响数组长度
```

##### 10、数组遍历

```javascript
var a = ["one","two","three","four"];
for (var i=0;i<a.length;i++) {
	console.log(a[i]);
}
```

##### 11、数组操作

```javascript
//join：将数组转变成字符串
var a = ["one","two","three","four"];
var b = a.join(' ');
console.log(b);

//split：将字符串转变成数组
var a = 'one,two,three,four';
var b = a.split(',');
console.log(b);

//slice 切割数组
var a = ['one','two','three','four'];
var b = a.slice(1,2);
console.log(b);
```





