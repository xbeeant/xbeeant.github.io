# POSTMAN 启用离线模式

* 安装好最新版本后，打开`POSTMAN`，在登陆页面先不进行任何处理，按`ctrl+shift+I`进入控制台（MAC电脑按`option - command - l`），输入`pm.settings.setSetting("offlineAPIClientEnabled",0)`
* 重启`POSTMAN`
* 打开控制台`ctrl+shift+I`（MAC电脑按`option - command - l`），输入`pm.mediator.trigger("hideUserSwitchingExperienceModal")`

大功告成，撒花!!!
