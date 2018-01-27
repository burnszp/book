## atom 插件安装
- 正常情况下使用atom的插件中心安装插件即可
- 有些情况由于网络或者依赖的原因，会导致安装失败，此时可以考虑离线安装的方式。

- 离线安装方式：
  - cmd进入atom的packages 目录下，git clone 插件项目
  - 重启打开atom，如果插件依赖其他项目，atom会提示，此时在packages目录下的cmd窗口执行：npm install package_name即可。
