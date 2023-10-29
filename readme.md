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

## 问题
>  `an error occurred while publishing @holyer-space/test-a: E404 Not Found - PUT https://registry.npmjs.org/@holyer-space%2ftest-a - Scope not found` 
> 无法将私有包发布到公有仓库中 需要在npmjs官网中添加同名组织
> 这个错误提示表明在发布 @holyer-space/test-a 包时出现了问题。具体错误是 E404 Not Found - PUT https://registry.npmjs.org/@holyer-space%2ftest-a - Scope not found，意味着无法找到 @holyer-space 这个作用域。

可能的原因和解决方法如下：

作用域不存在：确保你在 npm 上注册了 @holyer-space 这个作用域。如果没有，请先在 npm 上创建该作用域。

登录问题：确认你已经登录到正确的 npm 帐户。可以使用 npm login 命令登录到正确的帐户，并确保你具有发布 @holyer-space 作用域下的包的权限。

检查仓库地址：检查你的 .npmrc 文件或 package.json 文件中的仓库地址是否正确。确保你的仓库地址指向正确的 npm 仓库。

重试发布：如果以上步骤都没有解决问题，可以尝试重新发布。在重新发布之前，可以尝试删除本地的 node_modules 文件夹，并重新安装依赖项。
