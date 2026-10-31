# pi-verification

**只做一件事**：在说"它工作了"之前，先证明它真的工作。语言无关、引擎无关。

单技能包 —— 安装它**不会**带进任何其它技能。
（想要 Godot 那份实战手册，装 [pi-skills](https://github.com/CooCoode/pi-skills) 这个合集包。）

## 安装

```bash
pi install git:github.com/CooCoode/pi-verification@v0.1.0
```

Claude Code / Cursor / 其它 Agent Skills 客户端：

```bash
git clone https://github.com/CooCoode/pi-verification
cp -R pi-verification/skills/verification ~/.claude/skills/    # 或 <repo>/.agents/skills/
```

不需要 agent，只想要执行器与模板：

```bash
curl -O https://raw.githubusercontent.com/CooCoode/pi-verification/v0.1.0/skills/verification/templates/test_runner.sh
chmod +x test_runner.sh
./test_runner.sh "unit|pytest -q tests/unit" "e2e|npm run e2e"
```

## 里面是什么

| 文件 | 内容 |
|---|---|
| `SKILL.md` | 六条硬规则、三阶段检查清单（动手前/验证中/声称完成前）、性能测量约定、报告话术、反自欺速查表 |
| `references/false-signals.md` | 假信号分类学：执行类 / 度量类 / 副作用类 / 语义类，每类给识别与验证手法 |
| `templates/test_runner.sh` | 语言无关执行器：超时、挂死检测、错误模式 grep、退出码汇总 |
| `templates/playbook.md` | 证据库骨架，含「未解决」字段与写作约定 |

## 为什么

大多数"测试通过"是假的：框架吞掉错误、退出码为 0、指标动了但内容错、
工具返回 success 但副作用没发生、基准被编译器优化掉。这些**没有任何框架会提醒你**。

`test_runner.sh` 解决最常见的三种（退出码 / 错误日志 / 挂死），
`false-signals.md` 负责让人意识到剩下的。

## 自测

```bash
npm test     # bash scripts/self_test.sh
```

用四种用例检验执行器的判定：通过 / 非零退出 / 只打错误日志 / 挂死。
**这个自测抓到过一个真实缺陷**：默认错误模式漏了 `^ERROR:`，
导致"打印 ERROR 却退出 0"被误判为通过。

## 许可

MIT。纯 Markdown + 一个 Bash 脚本，无扩展代码，可安全审计。
