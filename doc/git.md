本项目中git上传有格式规范

简单来说，你的 **Git 提交失败了**，因为你的提交信息（commit message）不符合项目预设的格式规范。

这个过程是由 `husky` 和 `commitlint` 这两个工具自动执行的。

---

### ## 详细分解

1. **你执行了 `git commit`**：你尝试提交代码，并输入了提交信息 “创建了学习文档”。
2. **触发 `lint-staged`**：`husky` 捕获到你的 `commit` 动作，并自动运行 `lint-staged`。这个工具对你暂存区（staged）的文件（一个 `package.json` 和一个 `.md` 文件）使用 `prettier` 进行了代码格式化。**这一步是成功的**。
3. **触发 `commitlint`**：代码格式化后，`husky` 继续执行下一步，即检查你的提交信息。它使用了 `commitlint` 这个工具。
4. **校验失败**：`commitlint` 发现你的信息 “创建了学习文档” 不符合预设的 **Conventional Commits** 规范，并报告了两个错误：
   - `✖ type may not be empty [type-empty]`: **类型不能为空**。
   - `✖ subject may not be empty [subject-empty]`: **主题不能为空**。

因为校验失败，`husky` 中止了这次 `commit` 操作，所以你的代码提交没有成功。

---

### ## 核心问题：什么是“符合规范的提交信息”？

你的项目配置了 **Conventional Commits** 规范，它要求提交信息必须遵循特定的格式，通常是：

```
<type>(<scope>): <subject>
```

- **type (必需):** 说明提交的类别，比如：
  - `feat`: 新功能
  - `fix`: 修复 bug
  - `docs`: 只修改了文档
  - `style`: 代码格式修改（不影响代码逻辑）
  - `refactor`: 代码重构
  - `test`: 增加或修改测试
  - `chore`: 构建过程或辅助工具的变动
- **scope (可选):** 说明本次提交影响的范围，比如 `(user)``(api)` 等。
- **subject (必需):** 简短地描述本次提交的内容。

你的提交信息 “创建了学习文档” 缺少了必需的 `type`。

---

### ## 💡 解决方案

你需要按照规范的格式重新编写提交信息。根据你的描述 “创建了学习文档”，可以修改为：

**`docs: 创建学习文档`**

或者，如果这个文档是项目的一个新功能：

**`feat: 创建学习文档`**

**操作步骤：**

再次执行你的 `git commit` 命令，并在提示输入提交信息时，使用上面这种带 `type` 的格式即可。
