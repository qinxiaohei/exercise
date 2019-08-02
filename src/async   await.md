            async function async1() {
            
        console.log('async1 start');//2
        
        await async2();
        
        console.log('async1 end');//6
        
        }
        async function async2() {
        
            console.log('async2');//3
            
        }
        console.log('script start');//1
        
        setTimeout(function() {
        
            console.log('setTimeout');//8
            
        }, 0)
        
        async1();
        
        new Promise(function(resolve) {
        
            console.log('promise1');//4
            
            resolve();
            
        }).then(function() {
        
               console.log('promise2');//7
               
        });
        
        console.log('script end');//5
            
            
        执行过程的分析：

            1.遇到 console.log('script start'), 放在主线程中立即执行，所以第一步先输出 script start
            2.遇到 setTimeout，因为 setTimeout 是异步的，所以将回调函数放入异步队列中去等待，等待主线程中的任务执行完之后，再通过事件轮询的方式去调用
            3.遇到 async1 函数的调用，此时走到 async1 函数里面输出 async1 start
            4.遇到 await 关键字，进入到 async2 函数中输出 async2,将 await 后面的代码放入微任务中，执行后面的操作
            5.遇到 Promise 直接输出promise1,将回调函数放入微任务中，执行后面的操作
            6.遇到 console.log('script end'), 放在主线程中立即执行，所以第一步先输出 script end，到这个时候主线程中的代码就执行完了
            7.主线程中的代码执行完之后，立即执行所有的微任务。分别输出 async1 end，promise2
            8.最后通过事件轮询的方式将异步队列中代码，拿到主线程中来执行 (输出 settimeout)
            9.重复步骤8  