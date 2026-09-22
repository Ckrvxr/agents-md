## 最终消息风格

- 回复紧凑，避免不必要的换行。

## 工具选择

- Windows 环境中，请优先使用 `wsl`，而不是 `powershell`。
- 包管理器 apt > pnpm > npm > uv > scoop 。
- GitHub 交互优先使用 `gh`。
- 查询库和框架文档优先使用 `context7`。
- 环境中已经有以下常见 Unix 工具，请优先使用他们：
  - **文件**：`rg`、`fd`、`fzf`、`file`、`7zip`、`rsync`
  - **数据处理**：`jq`、`yq`、`dasel`、`sqlite3`
  - **多媒体**：`ffmpeg`、`imagemagick`、`yt-dlp`、`mediainfo`
  - **网络**：`curl`、`aria2c`、`mtr`、`doggo`、`iperf3`、`tshark`
  - **开发**：`go-task`、`clang`、`lldb`、`cmake`、`tmux`、`timeout`、`hyperfine`
- 其他工具优先使用 `npx` 或 `uvx`。

## 数据安全

- 删除文件或卸载程序时，将目标移到回收站，不要使用 `rm`。
- 未经用户明确同意，不要执行 `commit` 或 `push`。

## 资源管理

- 将 skills 统一存放在 `~/.agents`。

## tmux（会话、交互、调试增强）

### 会话命名统一规范

会话 id 使用 `pi-<项目>-<会话ID>-<任务>` 命名，例如 `pi-api-01a0c831-source_download`。

- 使用 `PI_SESSION_ID` 区分 Pi 会话，只取前 8 位。
- 其他 Pi 变量不用于命名会话。

```bash
project="$(basename "$PWD")" # 项目名
session_id="${PI_SESSION_ID:-manual}" # 会话 ID
session_id="${session_id:0:8}" # 只取前 8 位
session="pi-${project}-${session_id}-python" # 最终会话名

### 基本使用

# 查看会话列表
tmux list-sessions
# 启动会话
tmux new-session -d -s "$session" 'python -i'
# 发送命令(C-m 模拟回车)
tmux send-keys -t "$session" 'print(2 + 2)' C-m
# 查看输出
tmux capture-pane -p -t "$session"
# 结束会话
tmux kill-session -t "$session"
```
