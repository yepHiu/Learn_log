# python使用说明书

## 变量范围
### 全局变量
在python中对于全局变量的声明不像C语言中是根据变量声明的位置觉得了，现象使用全局变量这必须在代码声明变量的部分使用```global```关键字去声明这个变量是全局变量
- 注意全局变量在函数内赋值是无效的，无法更改全局变量的值
- 想要在函数内更改全局变量的值必须要有global关键字
- 代码示范  
    ```python
    global val
    val = 10

    def main():
        global val
        val=0
        print(val)

    if __name__ = '__main__':
        main()
    
    ```