### **第 1 步：获取并检出功能分支**

#### 操作命令：

```
git fetch origin  # 从远程仓库（origin）获取最新分支信息
git checkout -b 'feature-250301_CarInfoAndEquipment' 'origin/feature-250301_CarInfoAndEquipment'  # 基于远程分支创建本地分支并切换
```

#### 可能的问题：

- **错误提示**：`fatal: 'origin/feature-250301_CarInfoAndEquipment' is not a commit`
  **原因**：远程分支不存在或本地未同步远程信息。
  **解决方案**：

  1. 确认远程分支存在：

     ```
     git ls-remote --heads origin  # 查看所有远程分支
     ```

  2. 如果分支不存在，联系创建者推送分支；如果存在，确保执行了 `git fetch origin`。

------

### **第 2 步：在本地查看更改**

#### 操作命令：

```
git log --oneline  # 查看提交历史
git diff HEAD~1    # 对比最近一次提交的改动
git status         # 查看当前分支状态（是否有未提交的改动）
```

#### 小技巧：

- 使用可视化工具（如 VS Code、GitKraken）更直观地查看代码差异。

------

### **第 3 步：解决冲突（How to Resolve Conflicts?）**

#### 冲突场景：

当合并其他分支（如 `main`/`master`）到当前分支，或拉取远程更新时，如果同一文件的同一部分被不同分支修改，Git 会提示冲突。

#### 操作步骤：

1. **触发冲突**（例如合并主分支到当前分支）：

   ```
   git merge main  # 尝试合并 main 分支到当前分支
   ```

   - 如果输出 `CONFLICT (content): Merge conflict in <文件名>`，表示有冲突。

2. **定位冲突文件**：

   ```
   git status  # 查看冲突文件列表
   ```

3. **手动解决冲突**：

   - 打开冲突文件，会看到类似标记：

     ```
     <<<<<<< HEAD
     当前分支的代码
     =======
     合并进来的代码（如 main 分支）
     >>>>>>> main
     ```

   - **手动选择保留哪部分代码**，或修改为兼容两者的版本，删除冲突标记（`<<<<<<<`, `=======`, `>>>>>>>`）。

4. **标记冲突已解决**：

   ```
   git add <冲突文件路径>  # 将解决后的文件加入暂存区
   ```

5. **完成合并**：

   ```
   git commit -m "fix: resolve merge conflicts between feature-250301 and main"
   ```

#### 工具推荐：

- **VS Code**：内置 Git 冲突解决工具，可视化操作。
- **IntelliJ IDEA**：右键冲突文件选择 `Resolve Conflicts`。

------

### **第 4 步：推送分支到 GitLab**

#### 操作命令：

```
git push origin 'feature-250301_CarInfoAndEquipment'  # 将本地分支推送到远程仓库
```

#### 可能的问题：

- **权限不足**：确保你有权限推送该分支。
- **分支保护**：如果分支受保护，需通过合并请求（Merge Request）更新。

------

### 完整流程示例

```
# 1. 拉取远程分支并检出
git fetch origin
git checkout -b feature-250301_CarInfoAndEquipment origin/feature-250301_CarInfoAndEquipment

# 2. 合并主分支（模拟冲突场景）
git merge main

# 3. 解决冲突后提交
git add .
git commit -m "解决冲突"

# 4. 推送更新
git push origin feature-250301_CarInfoAndEquipment
```

------

### 注意事项

1. **分支命名规范**：确保分支名与远程一致（注意大小写和特殊字符）。
2. **冲突预防**：频繁拉取（`git pull`）远程更新，减少冲突概率。
3. **测试验证**：解决冲突后运行测试，确保代码功能正常。