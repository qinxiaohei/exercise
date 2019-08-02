#### react的setState
##### setState在reactJS(react控制的事件处理过程)中是异步的。但,当它不属于reactJS中的机制时,它可能是同步的。
1. call,apply,bind的区别
```
call,apply会直接调用执行该方法
bind不会直接调用执行该方法
```
2. render函数在什么时候才会执行
```
初始化时会执行
在state状态或者props属性更改时触发render函数
```
3. 修改react中的this指向三种方法
```
1). 箭头函数
2). render里的bind
3). constructor里的bind
其中,三者在优化方面有很大的区别。render里的bind性能优化最低;constructor里的bind性能优化最高。
```
4. reactJS中的setState
###### reactJS在处理setState时,会将setState中的内容放在一个队列式中,当多个setState的内容相同时,这个队列式会将相同的多个setState放在同一位置,即后者覆盖前者。
```
<body>
    <div id="root"></div>

    <script src="js/react.js"></script>
    <script src="js/react-dom.js"></script>
    <script src="js/babel.js"></script>
    <script type="text/babel">
        class Content extends React.Component{
            state = {
                count:1
            }
            handleClick = () => {
                this.setState({
                    count:this.state.count + 1
                })
                console.log(this.state.count,'one') // 1,'one'
                this.setState({
                    count:this.state.count + 1
                })
                console.log(this.state.count,'two') // 1,'two'
                this.setState({
                    count:this.state.count + 1
                })
                console.log(this.state.count,'three') // 1,'three'
            }
            render(){
                let {count} = this.state
                return <div>
                    {count}
                    <button onClick={this.handleClick}>修改count</button>
                </div>
            }
        }
        ReactDOM.render(<Content />,document.getElementById('root'))
    </script>
</body>

// handleClick方法中虽然写了三个相同内容的setState,但在执行时,reactJS机制会将这三个setState放在队列(一个数组)的同一位置,而且在reactJS中setState是异步的.所以console的结果会在setState之前打印,即打印结果是1.
```

###### react的setState不在reactJS处理机制中时,setState是同步的。
```
<body>
    <div id="root"></div>

    <script src="js/react.js"></script>
    <script src="js/react-dom.js"></script>
    <script src="js/babel.js"></script>
    <script type="text/babel">
        class Content extends React.Component{
            state = {
                count:1
            }
            handleClick = () => {
                this.setState({
                    count:this.state.count + 1
                })
                console.log(this.state.count,'one') // 1,'one'
                setTimeout(() => {
                    this.setState({
                        count:this.state.count + 1
                    })
                    console.log(this.state.count,'three') // 3,'three'
                    this.setState({
                        count:this.state.count + 1
                    })
                    console.log(this.state.count,'four') // 4,'four'
                }, 0);
                this.setState({
                    count:this.state.count + 1
                })
                console.log(this.state.count,'two') // 1,'two'
            }
            render(){
                let {count} = this.state
                return <div>
                    {count}
                    <button onClick={this.handleClick}>修改count</button>
                </div>
            }
        }
        ReactDOM.render(<Content />,document.getElementById('root'))
    </script>
</body>

// setTimeout是异步函数,该方法不属于reactJS机制,所以写在里面的setState是同步。
```

```
<body>
    <div id="root"></div>

    <script src="js/react.js"></script>
    <script src="js/react-dom.js"></script>
    <script src="js/babel.js"></script>
    <script type="text/babel">
        class Content extends React.Component{
            state = {
                count:1
            }
            componentDidMount(){ this.refs.div.addEventListener('click',this.handleClick.bind(this))
            }
            handleClick = () => {
                this.setState({
                    count:this.state.count + 1
                })
                console.log(this.state.count,'one') // 2,'one'
                setTimeout(() => {
                    this.setState({
                        count:this.state.count + 1
                    })
                    console.log(this.state.count,'three') // 4,'three'
                    this.setState({
                        count:this.state.count + 1
                    }) console.log(this.state.count,'four') // 5,'four'
                }, 0);
                this.setState({
                    count:this.state.count + 1
                })
                console.log(this.state.count,'two') // 3,'two'
            }
            render(){
                let {count} = this.state
                return <div>
                    {count}
                    <button ref="div">修改count</button>
                </div>
            }
        }
        ReactDOM.render(<Content />,document.getElementById('root'))
    </script>
</body>

// addEventListener属于dom二级事件,不属于reactJS机制内,所以setState都是同步的。
```