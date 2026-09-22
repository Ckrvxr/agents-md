## 一、最终消息风格

- 回复紧凑，避免不必要的换行。

## 二、数据安全

- 删除文件或卸载程序时，将目标移到回收站，不要使用 `rm`。
- 未经用户明确同意，不要执行 `commit` 或 `push`。

## 三、资源管理

- 将 skills 统一存放在 `~/.agents`。

## 四、工具使用

### 1. 工具路由

- Windows 环境中，请优先使用 `wsl`，而不是 `powershell`。
- 包管理器 apt > pnpm > npm > uv > scoop 。
- GitHub 交互优先使用 `gh`。
- 查询库和框架文档优先使用 `context7`。
- 环境中已经有以下常见 Unix 工具，请优先使用他们：
  - **文件**：`rg`、`fd`、`fzf`、`file`、`7zip`、`rsync`
    - `fd` 指 sharkdp/fd，是 `find` 的性能更好的现代替代品；查找文件时优先直接使用 `fd`（如 `fd -e rs`、`fd -t f`）。
  - **数据处理**：`jq`、`yq`、`dasel`、`sqlite3`
  - **多媒体**：`ffmpeg`、`imagemagick`、`yt-dlp`、`mediainfo`
  - **网络**：`curl`、`aria2c`、`mtr`、`doggo`、`iperf3`、`tshark`
  - **开发**：`go-task`、`clang`、`lldb`、`cmake`、`tmux`、`timeout`、`hyperfine`
- 其他工具优先使用 `npx` 或 `uvx`。

### 2. tmux（会话、交互、调试增强）

会话命名统一规范：

会话 id 使用 `pi-<项目>-<会话ID>-<任务>` 命名，例如 `pi-sqlite_v3-9766127e-source_download`。

使用 `PI_SESSION_ID` 区分 Pi 会话，取末尾 8 位。

```bash
# 项目 ID
project_id="${PWD##*/}"
# 会话 ID
pi_session_id="${PI_SESSION_ID:-manual}"
# 最终会话名
tmux_session_id="pi-${project_id}-${pi_session_id: -8}-task_name"

# 查看会话列表
tmux list-sessions
# 启动会话
tmux new-session -d -s "$tmux_session_id" 'python -i'
# 发送命令(C-m 模拟回车)
tmux send-keys -t "$tmux_session_id" 'print(2 + 2)' C-m
# 查看输出
tmux capture-pane -p -t "$tmux_session_id"
# 结束会话
tmux kill-session -t "$tmux_session_id"
# ...
```

### 3. fd（文件搜索增强）

- 优先使用 `fd` 快速查找文件和目录。
- 部分环境可能使用 `fdfind`，而非 `fd`
- `fd` 默认递归搜索，`pattern` 默认使用正则匹配文件名
- 默认跳过隐藏路径和被忽略路径；两者是独立的过滤条件

```bash
# 基于当前目录查找
fd 'pattern'
# 指定目录查找
fd 'pattern' '/path/to/search'
# 只查找文件或目录
fd -t f 'pattern'
fd -t d 'pattern'
# 按扩展名查找
fd -e 'rs'
# 使用 glob 匹配文件名
fd -g '*.test.ts'
# 匹配完整路径
fd -p 'src/.*/test_.*\.py'
# 包含隐藏路径
fd -H 'pattern'
# 包含被忽略路径
fd -I 'pattern'
# 与 rg 组合搜索文件内容
fd -e 'rs' -X rg -n 'TODO|FIXME'
# 每个结果执行一次命令
fd -e 'json' -x jq empty
# 将所有结果一次性传给命令
fd -g 'test_*.py' -X printf '%s\n'
# 查看更多帮助
fd --help
```
