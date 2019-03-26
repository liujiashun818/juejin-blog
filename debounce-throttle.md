    /**
     * @节流：固定时间内仅仅调用一次函数（无需频繁调用）
     * @throttle 处理函数,处理什么时间执行参数 fun
     * @func 参数 最终要执行的函数
     * @delay 延迟的时间
     */
     
  ```
    var throttle = function (func, delay) {
      let time = null;
      return function () {
        let _this = this;
        if(!time) { // 如果已经赋值就不要执行（赋值）了，
          time = setTimeout(function () {  // 通过赋值
            func.apply(_this, arguments); // func 参数
            time = null; // 约定时间执行完后 赋值为null 新的生命周期开始
          }, delay)
        }
      }
    };
    function doSomfun(){
      console.log('节流',Math.random());
    };
    window.addEventListener('scroll',throttle(doSomfun, 1000))
    
   ```
   
     /**
     * @防抖：当持续事件发生时候，5秒内没有再次触发事件才执行函数，如果5秒内又触发了一次，重新(5秒后)再计算时间
     * @debounce 处理函数，
     * @func 参数 函数
     * @delay 延迟的时间
     *
     */
     
     ```
         function debounce(func, delay) {
      let timeOut = null; // 定义一个定时器函数；
      return function () {
        if(timeOut !== null){ // 如果已经赋值了，就清空定时器重新计算。
          clearTimeout(timeOut);
        }
        timeOut = setTimeout(func,delay); //每次触发就赋值，执行最后一次赋值的函数
      }
    }

    function handle() {
      console.log('抖动')
    }

    window.addEventListener('scroll',debounce(handle,500))
     ```
