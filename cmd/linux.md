## SCREEN:
```
screen -S your_screen_name
screen -r your_screen_name
Detach：CTRL+A+D
screen -S your_screen_name -X quit
```

## nohup
```
nohup python -u main.py > log.out &
```

- nohup ：用于在系统后台运行命令，使其不受终端关闭或挂起的影响。即使关闭了当前终端会话，命令仍会继续运行。
- \> log.out ：将命令的标准输出（即程序运行时打印的信息）重定向到名为 log.out 的文件中。
- & ：将命令放在后台运行，这样您可以继续在当前终端中执行其他操作，而不必等待该命令执行完毕。

> 如果只使用 nohup 而不使用 & ，命令会在**前台运行，终端窗口会被该命令占用**，直到命令执行完毕。但即便关闭终端，命令仍会继续执行

> 如果只使用 & 而不使用 nohup ，命令会在后台运行，可以在当前终端继续操作，然而，如果**关闭终端，命令可能会被终止**

## ls
以可读性高的方式显示目录详情
```
ls -lh
```

