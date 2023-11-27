# 指定运行时的python conda环境
## 1. 使用`settings.json`文件
### 1. 在 VS Code 中打开项目：
打开包含你的 Python 代码的项目文件夹。

### 2. 安装 Python 插件：
确保你已经安装了 Python 插件。你可以在 VS Code 中的扩展视图中搜索 "Python" 并安装 Microsoft 的 Python 插件。

### 3.创建 `.vscode` 目录：
在你的项目根目录下创建一个名为 .vscode 的文件夹，如果它还不存在。

### 4.在 .vscode 目录中创建 `settings.json` 文件：
在 .vscode 目录中创建一个名为 `settings.json` 的文件，如果它还不存在。

### 5.编辑 settings.json 文件：
使用`conda info --envs`命令查看conda环境的路径
在 settings.json 文件中添加以下内容，将 <path-to-your-conda-env> 替换为你希望使用的 Conda 环境的路径。

```
{
    "python.pythonPath": "<path-to-your-conda-env>"
}
```
例如，如果你的 Conda 环境在` ~/miniconda3/envs/myenv` 中，那么设置应该是：

```
{
    "python.pythonPath": "~/miniconda3/envs/myenv/bin/python"
}
```
### 6.保存并关闭文件：
保存 `settings.json` 文件。

### 7.使用 F5 键运行调试：
确保你的 Python 文件是当前活动文件，然后按下 F5 键运行调试。VS Code 应该会使用你在 settings.json 中指定的 Conda 环境来执行程序。

## 2. 使用`launch.json` 文件
`settings.json` 用于配置编辑器和插件的设置，而 `launch.json`用于配置调试器和调试选项。在一个项目中，你可能需要同时使用这两个文件来满足不同的配置需求。


如果同时在 `settings.json` 和 `launch.json` 中都配置了 "pythonPath", 则 `launch.json` 中的配置会覆盖 `settings.json` 中的配置。在 Visual Studio Code 中，`launch.json` 中的调试配置会覆盖全局设置，以确保调试时使用特定的配置。  

例如，如果在 settings.json 中设置了默认的 Python 解释器路径:  
```
// settings.json
{
    "python.pythonPath": "/path/to/global/python"
}
```
但在 launch.json 中针对某个调试配置指定了不同的 Python 解释器路径：
```
// launch.json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Python: Debug Current File",
            "type": "python",
            "request": "launch",
            "program": "${file}",
            "pythonPath": "/path/to/specific/python"
        }
    ]
}
```
在这种情况下，当你选择运行调试配置 "Python: Debug Current File" 时，VS Code 将使用 `launch.json` 中指定的 `/path/to/specific/python` 解释器路径，而不是 `settings.json` 中的默认路径。