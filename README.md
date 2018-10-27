
一个好的项目通常都是多人合作的结果，然而每个人有每个人的开发习惯，并不统一。

所以 commit message 就显得格外的重要。有些不规范的 commit 可能过个一个月之后你自己都不知道当时的提交目的了ﾍ(;´Д｀ﾍ)，

所以，为了能使得日后复（zhao）盘（guo）的时候更加的方便，团队之间遵守同一套 commit message 规范还是很有必要的。

本篇文章介绍的是 [Angular 规范](https://docs.google.com/document/d/1QrDFcIiPjSLDn3EL15IJygNPiHORgU1_OOAqWjiDU5Y/edit#heading=h.greljkmo14y0)，这是目前使用最广的写法，比较合理和系统化，并且有配套的工具。
# `commit message` 的作用
- 提供更多的历史信息，方便快速浏览。
- 可以过滤某些commit（比如文档改动），便于快速查找信息。
- 可以直接从commit生成Change log。

# `commit message` 的格式
刚刚有说到，我们使用的是 [Angular 规范](https://docs.google.com/document/d/1QrDFcIiPjSLDn3EL15IJygNPiHORgU1_OOAqWjiDU5Y/edit#heading=h.greljkmo14y0)，下面来大致介绍下：

使用 `git commit` 可以提交多行 `commit message`.

`commit message` 包括三个部分：`Header`，`Body` 和 `Footer`
```
<type>(<scope>): <subject>
// 空一行
<body>
// 空一行
<footer>
```
其中，`Header` 是必填，`Body` 和 `Footer` 是选填。


### `Header`
`Header` 包括三个字段：`type`（必填）、`scope`（选填）和 `subject`（必填）

#### `type`
`type` 用于说明 `commit` 的类别，只允许使用下面 7 个标识。
- feat：新功能（feature）
- fix：修补bug
- docs：文档（documentation）
- style： 格式（不影响代码运行的变动）
- refactor：重构（即不是新增功能，也不是修改bug的代码变动）
- test：增加测试
- chore：构建过程或辅助工具的变动

`type` 为 `feat` 和 `fix`，则该 `commit` 将肯定出现在 `Change log` 之中。

#### `scope`
`scope` 用于说明 `commit` 影响的范围，比如数据层、控制层、视图层等等，视项目不同而不同。

#### `subject`
`subject` 是 `commit` 目的的简短描述，不超过50个字符
```
以动词开头，使用第一人称现在时，比如 change，而不是 changed 或 changes
第一个字母小写
结尾不加句号（.）
```

### `body`
`Body` 部分是对本次 `commit` 的详细描述，可以分成多行。下面是一个范例。

```
More detailed explanatory text, if necessary.  Wrap it to 
about 72 characters or so. 

Further paragraphs come after blank lines.

- Bullet points are okay, too
- Use a hanging indent
```

### `Footer`
`Footer` 部分只用于两种情况
- 不兼容变动

如果当前代码与上一个版本不兼容，则 `Footer` 部分以 `BREAKING CHANGE` 开头，后面是对变动的描述、以及变动理由和迁移方法。

- 关闭 `Issue`
如果当前 `commit` 针对某个 `issue`，那么可以在 `Footer` 部分关闭这个 `issue` 
```
Closes #123, #245, #992
```

# `Commitizen` -- 自动生成合格的 `commit message`
根据上述的描述，你是不是在感慨写个 `commit message` 好麻烦，这里介绍下 `Commitizen` -- 能够根据提示自动生成符合规范的 `commit message`

## 安装
```bash
$ npm install -g commitizen
```

## 在项目中使用
然后，在项目目录里，运行下面的命令，使其支持 `Angular` 的 `Commit message` 格式。

```bash
$ commitizen init cz-conventional-changelog --save --save-exact
```

## commit 
在提交的时候就可以使用 `git cz` 就可以根据提示，生成自动化的 `commit message`
![Commitizen](https://user-gold-cdn.xitu.io/2018/10/27/166b47239dd94158?w=640&h=400&f=gif&s=2101792)

# `validate-commit-msg` 检查你的 `commit-message` 规范
`Commitizen` 可以帮助我们规范自己的 `commit-message`，但是在团队合作中，如何规范其他成员的 `commit` 规范呢？

可以使用 `validate-commit-msg` 来检查你的项目的 `commit-message` 是否符合格式

## `validate-commit-msg` 安装
```
npm install --save-dev validate-commit-msg
```

## `husky` 安装
按照 [validate-commit-msg 中 README ](https://github.com/conventional-changelog-archived-repos/validate-commit-msg) 中写的，可以用 `validate-commit-msg` 作为一个 githook 来验证提交消息，并且推荐了 `husky`。

> This provides you a binary that you can use as a githook to validate the commit message. I recommend husky. You'll want to make this part of the commit-msg githook, e.g. when using husky, add "commitmsg": "validate-commit-msg" to your npm scripts in package.json.

执行
```bash
npm install husky --save-dev
```

并且在 `package.json` 中的 `scripts` 字段中加入 
```bash
"commitmsg": "validate-commit-msg"
```

----------------------------

然后每次 `git commit` 之后，就会自动检查 `commit message` 是否合格。如果不合格，就会报错
```bash
husky > commit-msg (node v9.2.1)
INVALID COMMIT MSG: does not match "<type>(<scope>): <subject>" !
change
husky > commit-msg hook failed (add --no-verify to bypass)
```

# 生成 Change log
如果你的所有 commit 都符合 Angular 格式，那么发布新版本时， Change log 就可以用脚本自动生成。

生成的文档包括以下三个部分。
- New features
- Bug fixes
- Breaking changes.

每个部分都会罗列相关的 commit ，并且有指向这些 commit 的链接。当然，生成的文档允许手动修改，所以发布前，你还可以添加其他内容。

## `conventional-changelog` 自动根据 `commit` 生成 `change log`

### `conventional-changelog` 安装

```bash
npm install -g conventional-changelog-cli
```

### `conventional-changelog` 工作流
- Make changes
- Commit those changes
- Make sure Travis turns green
- Bump version in package.json
- conventionalChangelog
- Commit package.json and CHANGELOG.md files
- Tag
- Push

`conventionalChangelog` 这一步有两种选择

```
# 不会覆盖以前的 Change log，只会在 CHANGELOG.md 的头部加上自从上次发布以来的变动
$ conventional-changelog -p angular -i CHANGELOG.md -s -p 

# 生成所有发布的 Change log
$ conventional-changelog -p angular -i CHANGELOG.md -w -r 0
```
**注意：** 
- 这里安装的是 conventional-changelog-cli，安装 conventional-changelog 会报 conventional-changelog: command not found 的错误
- 查阅了很多文章使用的是 `conventional-changelog -p angular -i CHANGELOG.md -w`，这样只能在命令行中 log 出 CHANGELOG 的内容，不会生成文件，如果要生成文件需要使用 `conventional-changelog -p angular -i CHANGELOG.md -s`。更多的 config 可以使用 `conventional-changelog --help` 查看
- 还需要注意的是，在生成 changlog 之前，需要先使用 `$ npm version [version]` 更改版本号，然后再生成 changelog，这一步很多的博文都没有写，就会导致增量生成的 CHANGELOG 一直都有之前的 commit 记录。

### 自动判断版本号
上面步骤有两个需要优化的地方
- 需要在 conventionalChangelog 之前先要更改版本号
- 生成 CHANGELOG.md 之后又造成了新的改动，所以还需要再执行一遍提交

所以我们需要自动化的执行这些，`commitizen` 还依据 `conventional message`，创建起一个生态
- [conventional-changelog-cl](https://github.com/conventional-changelog-archived-repos/conventional-changelog-cli)：通过提交记录生成 CHANGELOG.md
- [conventional-github-releaser](https://github.com/conventional-changelog/releaser-tools)：通过提交记录生成 github release 中的变更描述
- [conventional-recommended-bump](https://github.com/conventional-changelog-archived-repos/conventional-recommended-bump)：根据提交记录判断需要升级 Semantic Versioning 哪一位版本号

使用这些工具可以简化发布流程。
```bash
cp package.json _package.json &&
preset=`conventional-commits-detector` && 
echo $preset &&
bump=`conventional-recommended-bump -p angular` &&
echo ${1:-$bump} &&
npm --no-git-tag-version version ${1:-$bump} &>/dev/null &&
conventional-changelog -i CHANGELOG.md -s -p ${2:-$preset} &&
git add CHANGELOG.md package.json package-lock.json &&
version=`cat package.json` &&
git commit -m'docs(CHANGELOG): $version' &&
mv -f _package.json package.json &&
npm version ${1:-$bump} -m 'chore(release): %s' &&
git push --follow-tags 
```

# 在你的项目中规范 `commit message` 并且根据 `commit` 自动生成 `CHANGELOG.md`
1. 安装依赖
```bash
npm install -g commitizen conventional-changelog conventional-changelog-cli conventional-commits-detector conventional-recommended-bump husky validate-commit-msg

```

安装完成之后版本如下（我遇到过 `conventional-recommended-bump` 是 `4.x` 的版本会报错`Error: Unable to load the "angular" preset package. Please make sure it's installed.`。降低下版本就好。
```
/usr/local/lib
├── commitizen@3.0.4
├── conventional-changelog@2.0.3
├── conventional-changelog-cli@2.0.5
├── conventional-commits-detector@0.1.1
├── conventional-recommended-bump@0.3.0
└── npm@6.1.0
```
2. 在 `package.json` 中增加 `script` 字段
```bash
    "changelog": "cp package.json _package.json &&preset=`conventional-commits-detector` && echo $preset && bump=`conventional-recommended-bump -p angular` && echo ${1:-$bump} && npm --no-git-tag-version version ${1:-$bump} &>/dev/null && conventional-changelog -i CHANGELOG.md -s -p ${2:-$preset} && git add CHANGELOG.md package.json package-lock.json && version=`cat package.json` && git commit -m'docs(CHANGELOG): $version' && mv -f _package.json package.json && npm version ${1:-$bump} -m 'chore(release): %s' && git push --follow-tags "
```

3. 使用
使用 `git cz` 就可以根据提示，生成自动化的 `commit message`
使用 `npm run changelog` 生成 changelog, tag, 升级 version, 并自动执行 git push

# 问题
改方案还有一个问题尚未解决，就是使用 `conventional-recommended-bump` 更换推荐版本，我实验的项目都是只改动了中间的版本号，如果你的项目对版本号要去比较严格，建议使用手动更换版本号的方式。
