## 修改this的指向
```javascript
let obj1 = {
	name:"obj1",
	getName(){
		console.log(this.name)
	}
}

let obj2 = {
	name:"obj2",
	getName(){
		console.log(this.name)
	}
}

//第一种、第二种会改变this，自动执行函数
//第三种会改变this，但需要手动加括号执行函数
obj1.getName.call(obj2)
obj1.getName.apply(obj2)
obj1.getName.bind(obj2)()
```