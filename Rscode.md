# Rscode

## 前端工程部署构建流程

> 对照shell日志学习前端部署构建流程
>
> 总结来看就是通过package.json中run build执行一系列的bash脚本命令

**预构建**

+ 环境配置
+ 获取当前分支
+ 通知飞书群

**构建中**

+ pnpm安装第三方依赖
+ 使用上次umi缓存加速构建
+ umi框架构建打包分包

**构建后**

+ 将最新umi缓存存起来

#### 激活入口

![image-20221124212308228](/Users/xufei/Library/Application Support/typora-user-images/image-20221124212308228.png)

#### 入口触发`smart-qw/code/package.json`运行

```js
{
  "scripts": {
    "start": "pm2 start process.json",
    "prebuild": "sh +x ../server-conf/emit_webhooks.sh",
    "build": "sh +x ./client/scripts/build-for-rscode.sh"
  },
}
```

> code目录下package.json执行的操作

先执行prebuild

再执行build

```bash
#prebuild
sh +x ../server-conf/emit_webhooks.sh
#build
sh +x ./client/scripts/build-for-rscode.sh
```

#### emit_webhooks.sh主要干了什么呢

+ 环境配置
+ 获取当前分支
+ 通知飞书群

> 此时进入到/code/client目录下继续构建脚本执行

```bash
# @desc 在 rscode 部署采用 pnpm
pwd
cd ./client
pnpm config set registry http://npm.internal.focus.cn
pnpm install --prefer-offline  #利用全局缓存加快速度
yarn build
```

#### build-for-rscode.sh主要干了什么呢

+ 进入项目代码
+ 设置pnpm源为focus源
+ pnpm安装package
+ yarn build执行项目构建

> 此时开始部署smart-qw前端代码，
>
> 进入到/code/client/package.json
>
> 执行prebuild sh ../../server-conf/scripts/get-client-cache.sh

```json
{
  "name": "smartQw",
  "version": "1.0.0",
  "private": true,
  "description": "smart-qw fe",
  "scripts": {
    "prebuild": "sh ../../server-conf/scripts/get-client-cache.sh",
    "build": "umi build",
    "postbuild": "yarn copyIndex && yarn cacheClient",
    } 
}
```

#### get-client-cache.sh主要干了什么呢

```bash
cache_umi_path=../../../client-cache/smart-qw/.umi-production
mkdir -p $cache_umi_path && cp -rf $cache_umi_path ./src/
```

把上一次的umi缓存拿来用

> 执行umi build项目打包分包构建
>
> 完成后执行postbuild

#### cache-client.sh主要干了什么呢

```bash
# cp -rf ./node_modules ../../server-conf/ # todo: 执行时间太长：1分钟+
cache_path=../../../client-cache/smart-qw
rm -rf $cache_path && mkdir -p $cache_path && cp -rf ./src/.umi-production $cache_path
```

把下一次的umi缓存存起来

















#### Emit_webhooks主要干了什么呢

> 路径 smart-qw/code/server-conf/emit_webhooks.sh

+ 获取环境配置
+ 获取当前分支
+ 通知飞书群

```bash
# ---- config start ----
# TODO: 需要配置的变量
# 配置各个环境的webhooks test[1-10]_webhooks 
# rscode只会拷贝`user_defined_script.sh和user_defined_first_script.sh`文件，故需要同步维护
test_webhooks="https://open.feishu.cn/open-apis/bot/v2/hook/07bc1219-260b-4b5d-82a7-42d300496e1c" # TODO: 待添加
wait_time="5min" # 项目大概部署的时间
# ---- config end ----
# 获取当前部署的分支
reg_pwd="^\/opt\/rscode\/vcs_root\/deploy\/(.*)\/code$"
# PWD="/opt/rscode/vcs_root/deploy/feature-test-1/admin-smart-phone/code" # TODO: 本地调试。提交代码需要注释当前行
current_branch_path=""
if [[ "$PWD" =~ $reg_pwd ]];then
  current_branch_path=${BASH_REMATCH[1]}
else
  echo "error"
fi
# 获取具体分支
current_branch=(${current_branch_path//// })
echo $current_branch
# webhooks url
webhooks_url=$test_webhooks
# project url
project_url=""
# 获取具体的分支
reg_env="^feature\-test\-([1-9][0-9]?)$"
if [[ "$current_branch" =~ $reg_env ]];then
  project_url="$npm_package_name.focus-test${BASH_REMATCH[1]}.cn"
else
  project_url="$npm_package_name.focus-test.cn"
fi
echo $npm_package_name
echo $webhooks_url
echo $project_url
# 部署通知飞书群
if [[ "$current_branch" =~ $reg_env ]];then
  curl -X POST -H "Content-Type:application/json" -d '{"msg_type":"post","content":{"post":{"zh_cn":{"title":"企微管理后台test'${BASH_REMATCH[1]}'正在部署","content":[[{"tag":"text","text":"请耐心等候'$wait_time'左右..."}]]}}}}' $webhooks_url
  echo 'start test'${BASH_REMATCH[1]}' deploy !'
elif [[ "$current_branch" == 'test' ]];then
  curl -X POST -H "Content-Type:application/json" -d '{"msg_type":"post","content":{"post":{"zh_cn":{"title":"企微管理后台'$current_branch'正在部署","content":[[{"tag":"text","text":"请耐心等候'$wait_time'左右..."}]]}}}}' $webhooks_url
  echo 'start '$current_branch' deploy !'
elif [[ "$current_branch" == 'work' ]];then
  echo 'start '$current_branch' deploy !'
else 
  echo 'unknow feature'
fi
```



#### build-for-rscode.sh主要干了什么呢

> 路径 smart-qw/code/client/scripts/build-for-rscode.sh

+ 进入项目代码
+ 设置pnpm源为focus源
+ pnpm安装package
+ yarn build执行项目构建

```bash
# @desc 在 rscode 部署采用 pnpm
pwd
cd ./client
pnpm config set registry http://npm.internal.focus.cn
pnpm install --prefer-offline  #利用全局缓存加快速度
yarn build
```













#### 常见报错定位

> 打开shell任务日志，从下往上找到Error报错。报错主要有以下几方面引起

+ `ds-conf`,`nginx-conf`未正确配置

  对部署tes环境的分支，需要正确配置当前分支

  

+ npm包安装失误，组件引入失败

  

+ merge失败，没有本地合并最新的master分支代码

prebuild=>build[client prebuild build]=>start

流程图飞书通知

