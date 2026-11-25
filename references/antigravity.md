# Antigravity：启动进程没有继承系统代理

## 已验证与未验证
2026-10-29 的 Antigravity 2.28.0 模型进程未继承代理；带 HTTP/HTTPS 代理环境重启后，原生 Gemini 真实返回 OK。后来另一次新账号登录 IPv6 超时，重启后主进程/语言服务具备代理环境，但新账号最终登录成功没有记录。不要混为全部已修复。

## 执行
1. 确认当前 App 路径、版本及日志超时；对照当前系统代理是否启用且可达。只检查必要的代理变量，不打印含凭据的完整环境。
2. 保存工作、正常退出 Antigravity，确认主进程和旧语言服务已结束。应用仍运行时再次 open 不会可靠替换原环境。
3. 优先检查本机是否已有可用启动器；没有时参考以下启动方式。读取 `scutil --proxy` 的 HTTPEnable/HTTPProxy/HTTPPort，未启用时停止，随后通过 `open -a /Applications/Antigravity.app --env ...` 注入 HTTP_PROXY、HTTPS_PROXY 及小写变体，并让 localhost、127.0.0.1、::1 绕过代理。运行前核对路径存在、内容无异常和当前端口监听。
4. 按上述逻辑使用当前 App 和代理启动，不写全局 shell 配置。不盲用历史固定端口。IPv6 代理地址需要合法 URL 方括号格式。
5. 查看新进程是否正确继承并观察实际连接；已有登录态先直接测试，不强迫重新登录。确实需要登录时完成官方浏览器 OAuth，确认回到正确账号。
6. 实际发“只回复 OK”，再完整退出并用同一启动方式复测。若仅手工启动能用，说明依赖启动器，不声称普通 Dock 启动已经永久修好。若当前版本支持官方代理设置，查该版文档再选择持久配置。
7. 无效时正常退出后按原方式重开；没有更改全局代理时无需“恢复”其他程序设置。若错误是 location unsupported 或 Verify，转入资格/身份分流，不继续网络重启循环。

## 来源与局限
- 本流程来自作者 macOS 实测，历史版本为2.28.0；新版本需重新核对。
- [官方认证流程](https://antigravity.google/docs/install/)：个人账号经浏览器 OAuth 返回 App。文档随版本变动，旧版修复不得无条件套新架构。

## 启动示例
仅在保存工作并完整退出应用、确认已有代理正常运行后执行。将示例端口换成当前系统 HTTP 代理端口；不要把 SOCKS 端口误填为 HTTP。

```sh
open -a /Applications/Antigravity.app \
  --env 'HTTP_PROXY=http://127.0.0.1:YOUR_HTTP_PORT' \
  --env 'HTTPS_PROXY=http://127.0.0.1:YOUR_HTTP_PORT' \
  --env 'http_proxy=http://127.0.0.1:YOUR_HTTP_PORT' \
  --env 'https_proxy=http://127.0.0.1:YOUR_HTTP_PORT' \
  --env 'NO_PROXY=localhost,127.0.0.1,::1' \
  --env 'no_proxy=localhost,127.0.0.1,::1'
```
