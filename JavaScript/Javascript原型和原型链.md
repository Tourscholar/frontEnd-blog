###  Javascript原型和原型链

#### prototype
  - javascript函数拥有prototype属性, 函数名.prototype将得到一个对象, 这个对象拥有constructor属性, 函数名.prototype.constructor将得到该函数
  - 构造函数的prototype是实例的原型
    [![IOKl5T.png](https://z3.ax1x.com/2021/11/20/IOKl5T.png)](https://imgtu.com/i/IOKl5T)
  - 将方法添加到构造函数的prototype上, 这样通过new创建出来的实例对象就可以调用内存中的同一个方法从而避免了内存的浪费
    


#### 原型链查找
  - 实例对象可以通过打点访问它原型的属性和方法
    ```javascript
    function People(name, age) {...};
    People.prototype.sex = "男";
    let xiaoming = new People("小明", 12);
    console.log(xiaoming.sex);		// "男"
    ```
  - hasOwnProperty方法检查对象是否真正拥有某属性或者方法
    ```javascript
    xiaoming.hasOwnProperty("name");	// true
    xiaoming.hasOwnProperty("age");		// true
    xiaoming.hasOwnProperty("sex");		// false
    ```
  - in方法检查某个属性或方法是否可以被对象访问, 不能检查是否是自己的属性和方法
    ```javascript
    "name" in xiaoming;		// true
    "age" in xiaoming		// true
    "sex" in xiaoming		// fasle
    ```
#### 原型链的终点
-- javascript中的所有对象可以看成是由Object类通过new创建出来的实例对象, Object是所有实例对象的原型链的终点
  [![IOYNsU.png](https://z3.ax1x.com/2021/11/20/IOYNsU.png)](https://imgtu.com/i/IOYNsU)
  ```javascript
  function People() {...};
  let xiaoming = new People();
  console.log(xiaoming.__proto__.__proto__ === Object.prototype);	// true
  console.log(Object.prototype.__proto__);		// null
  ```

#### 继承
-- 使用javascript特有的原型链来实现继承
  [![IOUOeO.png](https://z3.ax1x.com/2021/11/20/IOUOeO.png)](https://imgtu.com/i/IOUOeO)
  ```javascript
  // 创建父类
  function People(name, age) {...};
  // 创建子类
  function Student(name, age, sex) {...};
  //实现继承, 因为构造函数的prototype是实例对象的原型, 将父类的实例对象挂载到子类的prototype上
  Student.prototype = new People();
  ```