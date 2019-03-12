### 一、Math ###
* 1、Math.ceil()
    * 分页计算 Math.ceil()。求得最大上限整数值
    
    ```
    有每页显示的条数，和改元素的序号，求改元素所在的页码数。
    const index = 4 // 下标是从0开始的，所以加1
    const pageSize = 10 // 每页显示的条数。 
    const pageNum = Math.ceil((index + 1)/pageSize)
    ```
* 2、Math.max() 求数组的最值
    ```
    const a = [1,2,3,4,5]
    const max = Math.max(...a) //需要展开 5
    const max2 = Math.max(a) //不展开的话 NaN

    ```
* 3、Math.random()和Math.round()
    * random 生成0-1的随机数
    * round 四舍五入一个最近的整数。
    ```
    // 生成20-30的随机整数
    const random20 = Math.round(20 + Math.random()*10)
    ```
* 4、Math.floor()和Math.sqrt()
    * Math.floor() 向下取整、返回=<参数。
        * Math.floor(-5.9) // -6
        * Math.floor(0.6) // 0
        * Math.floor(0.4) // 0
    * Math.sqrt()  平方根必须是正数
        * var d=Math.sqrt(0.64); // 0.8
    ```
    判断一个数是否是素数
    素数：大于1的自然数，不可以被自然整数（除了1和自己）整除的数。
    function is_prime(n){
        if (n <= 1){
            return fales
        }
        const N = Math.floor(Math.sqrt(n));
        let is_prime = true;
        for(let i = 2; i <= N; i++){
            if(n % i === 0 ){
                is_prime = false;
                break;
            }
        }
        return is_prime
    }
    
    ```
### 二、Array ###    
    
*  Array.lenght 长度
*  Array.indexOf() 获取元素的序号。
    *  [1,2,3].indexOf(2) 1 
*  Array.isArray: 判断是是否是数组
    * Array.isArray([]) true  
* forEach 遍历
    * 和for循环类似，不能break
* push/pop/shift/unshift
    * push  数组尾部添加
    * pop 删除最后一个元素并返回此元素，原来数组改变了长度和内容。
        * [1,2,3].pop() // 3 ,[1,2,3] 变成了[1,2]
    * shift 删除第一个元素并返回此元素，原来数组改变了长度和内容
        * [1,2,3].shift() // 1, 原数组变成可[2,3] 
    * unshift 往数组头部添加元素，并返回长度值
        * [1,2,3].unshift('add') // 4，原来数组变成了 ['add',1,2,3]
* map/reduce/reduceRight
    * reduce ：两两处理,前2个的处理结果作为一个参数的值
        * [1,2,3].reduce((x,y) => x+y) ;// 6
        * 1+2=3（3成了第一个参数x,第三个参数成了y,依次类推）,3+3=6
    * reduceRight 从右往左同reduce。 
* filter 
    * [1,2,3].filter(x=>x>2) //3 
* from 类数组对象或者可遍历的对象转换正一个数组；第二参数（fun）作用相当于map，
    * 字符串转换数组 
    ```
    let  str = 'hello world!';
    console.log(Array.from(str)) // ["h", "e", "l", "l", "o", " ", "w", "o", "r", "l", "d", "!"]
    ```
* slice(start,end) 剪切,不会改变原来数组，返回新的数组,start必选，如果是-1是最后一个元素，-1是倒数第二个元素，依次类推
    *  console.log([1,2,3].slice(0,1)) // [1]
    *  console.log([1,2,3,4].slice(-1)) // [4]
    *  console.log([1,2,3,4].slice(-3,-1)) //[2, 3]
* splice(index,howmany,...others) index:位置，howmany要处理的数量；others新的项目。会修改原来的数组。

    
    * 插入：
        * let arr = [1,2,3];  arr.splice(2,0,'ee') // arr = [1,2,'ee',3]
    * 替换：
        * let arr = [1,2,3];  arr.splice(2,1,'bb') // arr = [1,2,'bb',3] 
    * 删除并添加：    
        let arr = [1,2,3];
        arr.splice(2,3,'cc')
        // [1,2,'cc']
* sort(fun) 排序
    * let b = [1,32,43,4,3,545]
    b.sort() //[1, 3, 32, 4, 43, 545]
    ```
    let arr = [23,4,32,33]
    function sortNumber(a,b)
    {
    return a - b
    }
    arr.sort(sortNumber)
    ```
    * 中文排序
    ```
    ['王文成'，'王峰','蒋雪','李明'].sort((a,b) => a.localeCompare(b, 'zh'))
    ```
* every(fun) 检测数组中的每个元素是否符合函数提供的条件。如果一个元素不满足，return false，剩余的元素不在检测。 
````
var ages = [32, 33, 16, 40];

function checkAdult(age) {
    return age >= 18;
}

function myFunction() {
    document.getElementById("demo").innerHTML = ages.every(checkAdult);
}
// false
````
### 三、迭代器和生成器 ###
 * 迭代器iterator 是一种设计模式，它提供了一种遍历内容的方法，不需要关心内部的构造。生成器（generator）本身也是一种设计模式，用于构造复杂对象，js中的生成器用于构造迭代器。
 * 优势： 
    * 简化语法（树）
    * 节省空间
    * 分散执行片段（节省单位时间处理量）（对于单线程的前端很重要）
    * 构造异步语法。
  * 参考函数库[ranmda](https://ramdajs.com/)  
    
 ````
   const s = new Set([1,2,3,4,5]);
    const it = s.values();
    console.log(it);
    // SetIterator { 1, 2, 3, 4, 5}
 ````
 * while取值
 
 ```
  // 跌代器取值1
    const s = new Set([1,2,3,4,5]);
    const it1 = s.values();
    let val = null;
     while( !(val = it1.next()).done ){
        console.log(val);    
        // 有bug
        // 不会出现{value：1，done:false}.
     }
 ```
 * ...展开符取值
 ```
  // 跌代器取值2
     const s = new Set([1,2,3,4,5]);
     const it2 = s.values();
     console.log([...it2])
     // [1,2,3,4,5]
 ```
 * for(...){}取值
 ```
     // 跌代器取值3
     const s = new Set([1,2,3,4,5]);
     const it3 = s.values();
     for(const val of it3){
         console.log(val)
     }
     // 1,2,3,4,5
 ```
 * Array.from(arrayLike,mapFn,thisArg) 取值
    * arrayLike:伪数组对象或者可迭代的对象
    * mapFn:可选，如果有，新数组中的每个元素都会执行此回调函数
    * thisArg:可选，回调函数的this指向对象
 ```
    //迭代器取值4
        const s = new Set([1,2,3,4,5]);
        const it4 = s.values();
        
        const arr = Array.from(Array(5),it4.next,it4).map(x => x.value)
        console.log(arr)
        // [1,2,3,4,5]
 ```
 * 生成器构造无穷斐波那契数列
 ```
 // 求前10项
 function* fibonaqi(){
     let a = 1, b = 1;
     yield a;
     yield b;
     while(true){
     const t = b // 存储 第一位
     b = a + b;  // 第三项等于前两项的值相加
     a = t; // 赋值第一位 
         yield b; // 执行一次
     }
 }
 const it = fibonaqi();
 const feb10 = Array.from(Array(10), it.next, it).map(x => x.value)
 console.log(feb10);
 // [1,1,2,3,5,8,13,21,34,55]
 ```
* 数组的展平
```
function* flatten(arr){
    for(let i = 0; i < arr.length; i++){
     if(Array.isArray(arr[i])){
         yield *flatten(arr[i])
     }else{
     yield arr[i]
    }
}
    
}
console.log(flatten([1,2,[3,4,[5]]]))
// flatten{<suspended>}
// 必须用展开符[...flatten()]
console.log([...flatten([1,2,[3,4,[5]]])])
// [1,2,3,4,5]
```
* generator 异步语法 // 需要TODO
```
function request(url){
    return cb => {
        setTimeout(() => {
            cb(Math.random())
        }, 1000)
    }
}
// 自调用此函数收到一个函数，并执行返回的函数
create_runner(function*(){
    const val1 = yield request('some url');
    const val2 = yield request('some url');
    console.log(val1, val2)
})();
// 传入一个函数返回一个经过处理的函数，
function create_runner(genFunc){
    const it = genFunc(); // 迭代器
    // data 是谁？ cb吗？
    function run(data){
        const itVal = it.next(data);
        if(!itVal.done){
            itVal.value(run)
        }
    }
    return run 
}
```
* 迪卡尔积 
  * [1,2]*['a','b'] = [[1,'a'],[1,'b'],[2,'a'],[2,['b']]]
  ```
        function cartesian_product(...Matrix){
        if(Matrix.length === 0){
            return []; 
        }
        if (Matrix.length === 1){
            return Matrix[0];
        }
        console.log('Matrix',Matrix);
        return Matrix.reduce((A, B) => {
            console.log('a',A)
            console.log('B',B)
            const product = [];
            for(let i = 0; i < A.length; i++) {
                for(let j = 0; j < B.length; j++){
                    product.push(
                        Array.isArray(A[i])
                        ?[...A[i],B[j]]
                        :[A[i],B[j]]
                    )  
                }
              
            }
            return product;
        })
    }  
  let a  = cartesian_product(['a','b'],[1,2])
    //
    //0: (2) ["a", 1]
    //1: (2) ["a", 2]
    //2: (2) ["b", 1]
    //3: (2) ["b", 2]
  ```
 * 链式操作
   * ..map().filter().sort().map()
   * 优点: 语义清晰、思考方便
   * 缺点： 性能、空间
   * 场景： 数据返回数组。小于1万。
 ### 四、常用算法 ### 
 * 递归 就是自己调用自己
 ```
 //阶乘
 function factorial(n){
     return n === 0 ? 1 :factorial(n-1) * n
 }
 let fa4 = factorial(4);
 //1*2*3*4 24
 ```
 ```
  // 斐波那契数列
  funcition fibonacci(n){
      let [a,b] = [0,1];
      for(let i = 0; i < n; i ++){
          [a,b] = [b,a+b]
      }
      return b;
  }
  // fibonacci(5) //8
 
  function fibonacci(n){
    return Array(n).fill().reduce(([a,b],_) => {
        return [b, a+b]
    }, [0,1])[1]
  }
  let a = fibonacci(5);//8
  
 ```
 ```
 //fill 通过序数拿值
 Array(10).fill(0).map((x,i)=>i)
 //[0,1,2,3,4,5,6,7,8,9]
 Array.from({length: 10},(_, i) => i)
 // [0,1,2,3,4,5,6,7,8,9]
 //
 [...Array(10)].map(_,i)=>i);
 //[0,1,2,3,4,5,6,7,8,9]
 ```
 ```
  //深度拷贝对象
  
  function clone(obj){
      if(obj ==null || typeof obj !== 'object'){
          return obj
      }
      const newObj = new obj.constructor();
      for(let key in Object.getOwnpropertyDescriptors(obj)){
          newObj[key] = clone(obj[key])
      }
      return newObj;
  }
 ```
 ```
 //深度比较
 function deepCompare(a,b){
    if(a === null || typeof a !== 'object' || b ===null || typeof b !=="object"){
        return a === b;
    }
    const propsA = Object.getOwnPropertyDescriptors(a);
    const propsB = Object.getOwnPropertyDescriptors(b);
    if(Object.keys(propsA).length !== Object.keys(propsB).length){
        return false;
    }
    return Object.keys(propsA).every(key => deepCompare(a[key],b[key]))
}
 ```
````
//树

````
 
 
  

    
