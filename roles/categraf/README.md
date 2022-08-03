## 关于 inputs 和 conf 使用和维护

### inputs
* categraf 插件基于 inputs 文件夹进行管理，需要开启哪个插件，直接将对应插件（如 inputs.mem）放置到 conf.toml 同级目录。
* 基础监控插件随 categraf 安装包直接部署
* 定制化监控插件独立维护
    * 插件模板统一放置在 templates/inputs.toml 目录，随 categraf 版本更新
    * 定制化插件使用时，复制 inputs.toml 到 inputs.d 进行编辑 & 维护
    
### conf
* categraf 支持多种数据源，由多个配置文件进行管理
* 默认只开启总配置文件 config.toml
* 其他配置文件在放置在 conf.d 目录，随 categraf 版本更新
