# 课程第一周

本周目的是建立并巩固前端的知识体系，并通过简单案例来训练Js的编码逻辑

## 前端知识脑图

### 前言

掌握前端的学习方法与构建知识体系，通过整理法和追溯法等学习方法来完善自身知识体系

## 案例一：TicTacToe小游戏

### 前言

TicTacToe是三连棋游戏，两个棋手交替落子，当行、列或对脚三子相同时即判定获胜

### 实现

- 棋盘绘制

棋盘采用而为数组描述信息，双方选手落子标志分别为 -1(❌) 1(⭕️)，0表示空位置

```js
// 棋盘数组
const pattern = [
        [1, 0, 0],
        [0, -1, 0],
        [0, 0, 0]
    ]
```

- 胜负判断

TicTacToe的胜负判断比较简单，分别是行、列、对角中当棋子相同时，即为胜利，检测逻辑如下：

```js
// 检测胜负
    function checkWin(pattern, color) {
        // 检测行
        for (let i = 0; i < 3; i++) {
            let win = true
            for (let j = 0; j < 3; j++) {
                // 行棋子不同，必定不会获胜
                if (pattern[i][j] !== color) {
                    win = false
                }
            }
            // 行棋子相同，则获胜
            if (win) {
                return true
            }
        }

        // 检测列
        for (let i = 0; i < 3; i++) {
            let win = true
            for (let j = 0; j < 3; j++) {
                // 列棋子不同，必定不会获胜
                if (pattern[j][i] !== color) {
                    win = false
                }
            }
            // 列棋子相同，额获胜
            if (win) {
                return true
            }
        }


        // 检测对角：左上 -> 右下
        {
            let win = true
            for (let i = 0; i < 3; i++) {
                // 棋子不同，必定不会获胜
                if (pattern[i][i] !== color) {
                    win = false
                }
            }
            // 棋子相同，则获胜
            if (win) {
                return true
            }
        } 
        
        // 对角检测：右上 -> 左下
        {
            let win = true
            for (let i = 0; i < 3; i++) {
                // 棋子不同，必定不会获胜
                if (pattern[2 - i][i] !== color){
                    win = false
                }
            }
             // 棋子相同，则获胜
            if (win) {
                return true
            }
        }
        return false
    }
```

-   智能AI

智能AI是通过遍历棋盘位置，寻找对自己最有利的位置的过程，这里需要找到能够胜利的棋子位置，返回结果为1表示可以胜利，0表示没有最好位置，检测方法如下：

```js
// 检测是否会赢
function willWin(pattern, color) {
        for (let i = 0; i < 3; i++) {
            for (let j = 0; j < 3; j++) {
                // 如果已经落子，则不再统计范围内
                if (pattern[i][j] !== 0) {
                    continue
                }

                const temp = clone(pattern)
                temp[i][j] = color // 模拟落子
                // 当检测可以赢的时候，返回这个位置
                if (checkWin(temp, color)) {
                    return [i, j]
                }
            }
        }
        return null
    }

// 寻找最优位置
function bestChoice(pattern, color) {
        let p
        // 如果可以赢，则返回位置和结果 1
        if (p = willWin(pattern, color)) {
            return {
                point: p,
                result: 1,
            }
        }

        let result = -2
        let point = null
        for (let i = 0; i < 3; i++) {
            for (let j = 0; j < 3; j++) {
                if (pattern[i][j] !== 0) {
                    continue
                }
                const temp = clone(pattern)
                temp[i][j] = color // 模拟落子

                // 找到对方将要赢的位置，就是我方需要去封堵的位置
                let r = bestChoice(temp, 0 - color).result
                if (-r > result) {
                    result = -r
                    point = [i, j]
                }
            }
        }

        // 当没有点可走，则说明双方和棋
        return {
            point,
            result: point ? result : 0
        }
    }
```

### 总结

TicTacToe中的智能AI模块是需要考虑的地方，其实就是遍历整个棋盘，模拟棋手落子，之后检测是否会赢，如果可以赢，则表示该位置是一个可选位置，遍历所有空位置寻找估值最大的点，赢面就越大

# 案例二：红绿灯问题

## 前言

关于Js中的异步编程，通过一个案例来描述基本使用方法，

案例：红绿灯问题

需求描述：

一个路口的红绿灯，会按照绿灯亮10秒，黄灯亮2秒，红灯亮5秒的顺序无限循环

下面对红绿灯问题，通过不同的异步方式，实现相关功能

## 正文

Js有三种异步处理的机制，分别是callback回调、Promise以及async/await，通过这三种方式分别实现对红绿灯问题的处理

### callback

callback回调是最早期Js中处理异步问题的唯一方式，通过传入回调函数实现嵌套调用，当异步情况比较复杂时，嵌套层次会非常深，常常被调侃为回调地狱，主要代码实现如下：

```js
function go() {
        // 回调地狱
        green()
        setTimeout(() => {
            yellow()
            setTimeout(() => {
                red()
                setTimeout(() => {
                    go()
                }, 5000)
            }, 2000)
        }, 10000)
    }
```

可以看到，红绿灯问题的callback实现是嵌套了三层，即不利于阅读，也不利于后期维护，在编码中应该尽量避免

### promise

Promise是很多编程语言异步处理的方式，Js中通过Promise实现链式调用，代码实现如下：

```js
function go() {
        green()
        sleep(10000).then(() => {
            yellow()
            return sleep(3000)
        }).then(() => {
            red()
            return sleep(5000)
        }).then(() => {
            go()
        })
    }
```

相比较于callback方式，Promise就清爽了很多，避免了无限嵌套的编码风格，方便阅读和维护，通过Promise背后的编程思想，可以通过像Promise.all，Promise.race 实现竞争关系，实现异步并行调用

### async/await

async/await 基于Promise的语法上的封装，通过同步的写法实现异步逻辑，代码处理如下：

```js
async function go() {
        green()
        await sleep(10000)
        yellow()
        await sleep(3000)
        red()
        await sleep(5000)
        go()
    }
```

# 总结

通过红绿灯问题，可以学习了解Js中异步处理的三种方式以及优缺点，在以后的项目开发中寻找最友好的方式，避免留坑