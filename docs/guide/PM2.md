# PM2

```
pm2 start app.js               # 启动app.js应用程序

pm2 start app.js --name="demo"  # 启动应用程序并命名为 "demo"

pm2 start app.js --watch       # 当文件变化时自动重启应用

pm2 start script.sh            # 启动 bash 脚本

pm2 list                       # 列表 PM2 启动的所有的应用程序

pm2 show [app-name]            # 显示应用程序的所有信息

pm2 logs                       # 显示所有应用程序的日志

pm2 logs [app-name]            # 显示指定应用程序的日志

pm2 stop all                   # 停止所有的应用程序

pm2 stop 0                     # 停止 id为 0的指定应用程序

pm2 restart all                # 重启所有应用

pm2 restart 0                  # 重启id为0 的应用程序

pm2 delete all                 # 关闭并删除所有应用

pm2 delete 0                   # 删除指定应用 id 0

pm2 startup                    # 创建开机自启动命令

pm2 save                       # 保存当前应用列表

pm2 restart start.json --max-memory-restart 20M # 内存达到20m重启
```
