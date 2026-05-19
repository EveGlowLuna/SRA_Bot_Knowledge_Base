# SRAcli命令行工具

SRA 2.0版本提供独立的命令行程序`SRA-cli.exe`，是SRA的实际功能执行者。主程序通过嵌入式调用SRA-cli来完成任务，也可直接使用SRA-cli执行任务（需先通过主程序配置任务）。

## SRA启动参数

- `--use-python`：启用使用Python从源代码运行后端的功能
- 同时支持传递任意参数给后端程序，直接转发给SRA-cli

## 简单使用

运行SRA-cli即可看到控制台窗口。输入help查看可用命令。

可用命令：EOF（退出）、exit、help、run（运行指定配置）、single（运行单个任务）、task（管理任务）、trigger（管理触发器）、version。

### 运行任务

使用`task run [配置名称...]`命令。不指定配置名称则默认运行全部配置。

示例：`task run Default PlanB` — 依次运行Default和PlanB配置

### 停止任务

使用`task stop`命令停止正在运行的所有任务。

### 触发器管理

使用`trigger`命令管理触发器。示例：`trigger run`启动触发器服务，`trigger enable AutoDialogueTrigger`启用自动对话功能，`trigger set-bool AutoDialogueTrigger skip_plot true`启用跳过对话功能。

## 完整命令列表

### task命令

```
task run [<name | path>...] — 运行指定配置文件中的所有选中任务
task single (task_name | task_index) [<config_name | config_path>] — 运行单个任务
task stop — 停止所有任务
```

示例：`task run 114 514`、`task single ReceiveRewardsTask`、`task single 2`

### 其他命令

- exit：退出SRA-cli
- version：显示版本
- help：显示帮助
- run：同`task run`但阻塞命令行直到完成
- single：同`task single`但阻塞命令行直到完成

## 命令行参数

```
SRA-cli.exe [-h] [--inline] [--embed] [--command [COMMAND ...]]
             [--version] [--log-level LEVEL] {run,single} ...
```

- `--inline`：内联模式，关闭prompt，专注打印日志
- `--embed`：嵌入模式，已过时，建议用`--inline`
- `--command`或`-c`或`--execute`或`-e`：启动后执行命令，多个命令用`+`分隔
- `--version`：显示版本并退出
- `--log-level`：设置日志级别，默认TRACE

示例：
- `SRA-cli.exe -e task run Default` — 启动后立即运行Default配置
- `SRA-cli.exe -e "task run Default PlanB"` — 运行多个配置
- `SRA-cli.exe -e run + exit` — 运行全部配置并在完成后退出（必须用run而非task run）

## 高级使用

### 将SRA与第三方程序集成

SRA-cli支持内嵌模式（`--inline`），关闭prompt专注于打印日志，适合被其他程序调用。

监控SRA-cli的日志输出：日志存放在`SRA根目录/log/`下，按日期分割。开始任务时打印含`[Start]`的DEBUG日志，任务结束时打印含`[Done]`的DEBUG日志。

确保SRA-cli的工作目录为安装目录，否则可能无法正确加载任务类。

### 使用Windows计划任务程序

您可以结合Windows的计划任务程序，实现开机自启动SRA-cli并运行任务、定时运行任务等功能。

1. 按下您键盘上的Windows徽标键，搜索“任务计划程序”，并打开它。
2. 点击创建任务，填写任务名称，例如“SRA自动运行”。
3. 注意勾选“使用最高权限运行”，以确保SRA-cli能够正常运行。
4. 点击触发器，设置任务的触发条件，例如“在登录时”或“按计划”。
5. 点击操作，添加一个新操作，选择“启动程序”。
6. 在“程序或脚本”栏中，填写SRA-cli的完整路径，例如 `C:\Program Files\SRA\SRA-cli.exe`。
7. 在“添加参数”栏中，填写运行命令，例如 `run --config Default --once`。
8. 在“起始于”栏中，填写SRA-cli的安装目录，例如 `C:\Program Files\SRA`。
