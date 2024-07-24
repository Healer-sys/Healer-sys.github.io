# 使用cmake的笔记



## CMakeLists.txt配置

1. 必要三个参数
```cmake
    # 配置最低版本
    cmake_minimum_required(VERSION 2.8.12)
    # 配置项目名称
    project(project_name)
    # 配置编译器
    set(CMAKE_C_COMPILER /usr/bin/aarch64-linux-gnu-gcc)
    # 添加可执行文件
    add_executable(main main.c) 
```
2. 简单为项目添加构建目标
```cmake
    # 配置最低版本
    cmake_minimum_required(VERSION 2.8.12)
    # 配置项目名称
    project(project_name)
    # 配置编译器
    set(CMAKE_C_COMPILER /usr/bin/aarch64-linux-gnu-gcc)
    # 导入头文件
    include_directories(${PROJECT_SOURCE_DIR}/include)
    # 指定第三方库
    link_libraries(lib_name1)
    link_libraries(lib_name2)
    # 指定库路径
    link_directories(${PROJECT_SOURCE_DIR}/lib_path)
    # 指定源文件
    file(GLOB_RECURSE SOURCES &#34;${PROJECT_SOURCE_DIR}/src/*.c&#34;)
    # 添加可执行文件
    add_executable(${CMAKE_PROJECT_NAME} ${SOURCES})
```

## 配置cmake

```bash
    # 简单配置
    cmake -S . -B build (-DCMAKE_BUILD_TYPE=Release 可有可无)
    # 带参数配置
    cmake -DCMAKE_BUILD_TYPE:STRING=Debug            // 调试 or 发布
    / -DCMAKE_EXPORT_COMPILE_COMMANDS:BOOL=TRUE      // 生成编译命令
    / -DCMAKE_C_COMPILER:FILEPATH=/usr/bin/aarch64-linux-gnu-gcc    // 指定编译器
    / --no-warn-unused-cli                           // 忽略未使用的参数
    / -S/your_project_path                           // 源码路径
    / -B/your_project_path/build                     // 构建路径
    / -G Ninja                                       // 使用Ninja构建
```

## 构建
```bash
    cmake --build build                        // 构建

    # 补充
    cmake --build build --target all            // 构建全部
    cmake --build build --target clean           // 清空构建
    cmake --build build --target install         // 安装
    cmake --build build --target uninstall        // 卸载
    cmake --build build --target run              // 运行
    cmake --build build --target test             // 测试
    cmake --build build --target package          // 打包
    cmake --build build --target package_source    // 打包源码
```

## 添加运行/调试需要传入的参数

    1. 直接使用cmake tool调试，需要给cmake传入参数，在Settings.json中添加以下内容
    ```
      &#34;cmake.debugConfig&#34;: {
          &#34;args&#34;: [
          &#34;arg1&#34;,
          &#34;arg2&#34;,
          &#34;....&#34;
          ]
      }
    ```

    2. 如果是vscode gdb 启动，需要添加启动配置，可以在launch.json中添加设置(运行环境是./vscode)
    &gt;   有空可以研究一下vscode的三个配置文件(tasks.json, launch.json, settings.json)

&gt;   建议使用vscode&#43;cmake插件
&gt;   因为cmake插件可以快捷设置cmake参数，自动生成编译命令，可以实时编译
&gt;   可以用cmake插件生成调试配置，用vscode调试可以图形化显示变量，添加断点，单步运行，更简单直观调试流程


---

> 作者: 莫德飞  
> URL: http://localhost:1313/posts/%E4%BD%BF%E7%94%A8cmake%E7%9A%84%E7%AC%94%E8%AE%B0/  

