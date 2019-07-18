* [编程书籍索引](https://github.com/DaveTon/sass/blob/master/docs/free_prog.md)

## SASS 应用

* [example](https://github.com/DaveTon/sass/blob/master/docs/example_st.md)
* vscode 配置 sass 编译环境

    * 安装[node](https://nodejs.org/zh-cn/)
    * 安装ruby

    ```
        npm install ruby
    ```

    * 安装sass
    
    ```
        npm install sass
    ```

    * 安装[VScode](https://code.visualstudio.com/)
    * 安装easysass插件
    * 配置编译环境
    ```
    {
        /** Easy Sass 插件 **/
        "easysass.compileAfterSave": true,  // 保存后是否自动编译
        "easysass.excludeRegex": "_",  // 配置忽略编译的文件格式 eg：_variable.scss
        "easysass.formats": [
            {
                "format": "compact",  // 简洁格式的 css 代码(代码风格)
                "extension": ".min.css"
            }
        ],
        "easysass.targetDir": "source/css"  // 自定义css输出文件路径（相对于根目录）
    }
    ```