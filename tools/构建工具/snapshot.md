SNAPSHOT快照，在maven和gradle中被同等对待，主要用途为促使maven或gradle在
每次构建时，对版本为SNAPSHOT的依赖，都去远程仓库拉取最新版本；  

被标记为SNAPSHOT标志着该依赖仍在开发，可能会经常更新；在发布的项目中，不
应当依赖SNAPSHOT版本的依赖，因为可能由于快照版本的更改，造成线上环境突然
构建失败；