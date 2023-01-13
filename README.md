# changelog-standard

Commit specification+commitlint+CHANGELOG automatically generates a one-stop service

## 目的

git commit 消息规范化的目的就是为了能自动生成 changelog, 自动生成 changelog 的目的，是为了方便让自己和别人知道，每次版本更新，都有哪些变动（增减了哪些功能, 修复了哪些 bug）

## 依赖包安装

```bash
npm install --save-dev git-cz husky commitlint @commitlint/config-conventional standard-version
```

## 生成/更改相关配置文件

执行命令， 生成 husky 命令文件夹.husky

```bash
npx husky install

```

在.husky 文件夹新建 commit-msg 文件，文件内容如下

```bash
#!/bin/sh
. "$(dirname "$0")/_/husky.sh"

npx --no-install commitlint --edit "$1"
```

新增.commitlintrc.js 配置文件，文件内容如下：（用于对 git commit 消息进行验证）commitlint 工具的配置文件

```js
module.exports = {
  extends: ["@commitlint/config-conventional"],
};
```

新增 changelog.config.js 配置文件, 文件内容如下: (用于规定 git commit 消息的规范)git-cz 工具的配置文件

```js
// changelog 配置，commit 规则也在这里进行配置
// 参考文档：https://www.npmjs.com/package/git-cz

module.exports = {
  disableEmoji: false,
  // format: '{type}{scope}: {emoji}{subject}',
  list: [
    "test",
    "feat",
    "fix",
    "chore",
    "docs",
    "refactor",
    "style",
    "ci",
    "perf",
  ],
  maxMessageLength: 64,
  minMessageLength: 3,
  questions: [
    "type",
    "scope",
    "subject",
    "body",
    "breaking",
    "issues",
    "lerna",
  ],
  scopes: [],
  types: {
    chore: {
      description:
        "对构建过程或辅助工具和库的更改,不影响源文件、测试用例的其他操作",
      emoji: "",
      value: "chore",
    },
    ci: {
      description: "修改了 CI 配置、脚本",
      emoji: "",
      value: "ci",
    },
    docs: {
      description: "文档更新(如：README)",
      emoji: "",
      value: "docs",
    },
    feat: {
      description: "新的特性",
      emoji: "",
      value: "feat",
    },
    fix: {
      description: "bug 修复",
      emoji: "",
      value: "fix",
    },
    perf: {
      description: "优化了性能的代码改动",
      emoji: "",
      value: "perf",
    },
    refactor: {
      description: "一些代码结构上优化，既不是新特性也不是修 Bug",
      emoji: "",
      value: "refactor",
    },
    release: {
      description: "Create a release commit",
      emoji: "",
      value: "release",
    },
    style: {
      description: "代码的样式美化，不涉及到功能修改等",
      emoji: "",
      value: "style",
    },
    test: {
      description: "添加、修改测试用例",
      emoji: "",
      value: "test",
    },
  },
};
```

修改 package.json

```json
{
...
    "scripts": {
        "git-commit": "git-cz"
    },
    "config": {
      "commitizen": {
        "path": "git-cz"
      }
    }
...
}
```

git-commit 该命令用于代替原始的 git commit, 让用户直接按照模板填写规范化的消息

此时可以进行一次 git commit，测试是否有对提交消息进行验证
