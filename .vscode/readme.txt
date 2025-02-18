# 旧电脑生成最新的插件列表
# PowerShell命令，使用UTF-8编码
code --list-extensions | Out-File -Encoding UTF8 .vscode/extensions.list
# 或者使用这个命令
code --list-extensions | Set-Content -Encoding UTF8 .vscode/extensions.list

# 新电脑从文件中读取扩展列表并安装
Get-Content '.vscode/extensions.list' | Where-Object { $_ -match '\S' } | ForEach-Object {
    Write-Host "正在安装扩展: $_" -ForegroundColor Green
    code --install-extension $_
}

# 复制 Cursor 的 用户settings.json 到 .vscode 目录
Copy-Item "$env:APPDATA\Cursor\User\settings.json" -Destination ".\.vscode\settings.user.json"

# 列出当前目录下的文件和目录，只显示3级
tree -L 3

# 使用以下命令修复项目中所有文件的格式化和规则警告
ruff check --fix .
- check：运行 lint 检查。
--fix：自动修复可修复的问题。
.：表示当前目录及其子目录中的所有文件。

# 仅格式化代码
ruff format .

# 包管理最佳实践
# 1. 使用 mamba 管理Ai和数据科学相关的包
# 2. 使用 uv pip 管理其他包
# 3. 使用 conda 管理系统包

# mamba 包
mamba env export --from-history > environment.yml
# pip 包
uv pip freeze > requirements.txt 

# 在新电脑上恢复环境
# 创建环境并安装 mamba 包
mamba env create -f environment.yml

# 激活环境
mamba activate 环境名称

# 如果有 pip 包，再安装 pip 包
uv pip install -r requirements.txt

# 更新环境依赖
mamba env update -f environment.yml


# 这样执行半天解析不出来
mamba install langchain-core langgraph>0.2.27 -c conda-forge

# 这样执行可以正常安装
mamba install langgraph langsmith

# 数据库最佳实践
# 1. Ai和数据科学相关的应用数据存取用mongodb(嵌套层次深结构复杂的数据)
# 2. 网站/游戏服务器等普通应用数据存取用postgresql(数据结构简单扁平，嵌套层次浅，需要高性能多表查询事务机制的场景)

# cursor单独搞不定的问题，使用LLM询问解决方案，然后把合适的解决方案发给cursor一起解决, twitter视频的下载就是一个例子
