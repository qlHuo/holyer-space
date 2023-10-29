# holyer-space

1. 安装 pnpm
2. npm init 
3. 修改 package.json 包管理器限制

```json
{
  // 只允许使用 pnpm 开发
  // preinstall 在 install 之前（首次）执行
  // postinstall 在 install 之后（首次）执行
  "scripts": {
    "preinstall": "npx only-allow pnpm"
  },
  // 防止最外城包被发布出去，设为true以后发布时会提醒
  "private": true,
  "engines": {
    "node": ">=16"
  }
}
```

5. 添加文件 pnpm-workspace.yaml 
```yaml
packages:
  # all packages in direct subdirs of packages/
  - 'packages/*'
```
6. 添加子包 test
7. 进入子包 pnpm init
8. 修改package.json

```json
{
  "publishConfig": {
    "access": "public"
  }
}
```
9. 子包添加其他子包依赖

> 给 @holyer-space/test 添加 @holyer-space/test1 依赖

```bash
pnpm -F @holyer-space/test add @holyer-space/test1
```

10. npm login 登陆 npmjs
11. 安装发包依赖

```bash
pnpm install @changesets/cli -w --save-dev
pnpm changeset init
```

12. 预发布

```bash
pnpm changeset pre enter <tag>

# alpha 内部测试版本，一般不想外部发布，会有很多 bug，一般只有测试人员使用

# beta 也是测试版本，这个阶段的版本会添加新功能，在 alpha 版本之后推出

# rc （release candidate）发行候选版本。rc 版本不会加入新的功能，主要用于除错

pnpm changeset

pnpm changeset version

pnpm changeset publish

# 退出预发布
pnpm changeset pre exit
```

13. 正式发布

```bash
pnpm changeset

pnpm changeset version

pnpm changeset publish
```
