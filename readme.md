
# 简单地进行了一些调整  
### 和pip解耦
咱想让运行环境使用git和这个仓库同步，这样方便修改和同步，于是使用pip进行依赖安装，但不使用pip安装synapse本体
* 版本检查将失效
* 依赖检查将失效(这应该不影响pip的依赖处理)

### 适应MC玩家id
* 使synapse允许注册数字用户  
* 将访客uid调整至大于16个字符，以避开mc的玩家id

# 如何安装
```
pip install "matrix-synapse[postgres]"
pip uninstall "matrix-synapse[postgres]"
```
这会留下依赖项，但是删除synapse本体  
```
git clone https://github.com/OurWorldCommunity/synapse.git
cd synapse
```
完成~  
可以使用`python -m synapse.app.homeserver`测试  
*一些例子:*
```
python -m synapse._scripts.synctl start ./work_dir/homeserver.yaml

python -m synapse._scripts.synctl stop ./work_dir/homeserver.yaml

python -m synapse._scripts.synapse_port_db --sqlite-database ./work_dir/old.db \
    --postgres-config ./work_dir/homeserver.yaml

python -m synapse.app.homeserver \
    --server-name matrix.smyhw.online \
    --config-path ./work_dir/homeserver.yaml \
    --generate-config \
    --report-stats=yes
```
**之后对git上的版本进行修改后在运行环境中使用`git pull`即可**  
# 注:
* 注意需要在git克隆出来的目录中执行`python -m`相关命令，因为实际python包位于项目目录中的`synapse`子目录下
* 注意配置文件位置，不建议直接放在项目根目录下，小心手残用git给盖掉(
* 将项目目录扔进`PYTHONPATH`的方案不可行，会导致命名重复?
* 将项目目录扔进`site-packages`的方案可行，但是注意只能用软链接把子目录`synapse`扔进去，原因同第一条

# 如何升级
1. 需要先升级依赖  
`pip install -U "matrix-synapse[postgres]"=="1.90.0"`
是的，这里手动指定了版本号，也建议始终从官方仓库查看最新版本号并在升级依赖时手动指，*以防止pip反向升级(*  
`pip uninstall "matrix-synapse[postgres]"`
和安装同理，卸载pip安装的本体

2. 更新本体  
`git pull` 通过git拉取本体

3. 完成  
启动matrix，更新完成
