# 🚀 SLURM Sbatch 脚本生成器

> 📚 [English Documentation](README.md)

一个现代化、用户友好的Web应用程序，用于生成SLURM sbatch提交脚本，具有实时预览和一键复制功能。

## ✨ 主要特性

- 🎨 **现代界面**: 基于 Vue 3 + Arco Design 构建，界面美观响应式
- 🔧 **核心SLURM参数**: 全面支持作业调度的关键参数
- 🏷️ **预设分区**: 快速选择常用分区类型 (small, large, ssd, long)
- 📦 **模块支持**: 可选的环境模块加载功能
- 📋 **一键复制**: 瞬间将生成的脚本复制到剪贴板
- 🧹 **快速重置**: 一键清空所有表单字段
- 🌐 **实时预览**: 输入时实时生成脚本
- 📱 **响应式设计**: 在桌面和移动设备上完美运行

## 🛠️ 支持的SLURM参数

| 参数 | 描述 | 示例 |
|------|------|------|
| `--job-name` | 作业名称标识符 | `my-computation` |
| `--partition` | 队列/分区选择 | `small`, `large`, `ssd`, `long` |
| `--nodes` | 计算节点数量 | `1`, `2-4` |
| `--ntasks` | 总任务数 | `24`, `48` |
| `--ntasks-per-node` | 每节点任务数 | `12`, `24` |
| `--cpus-per-task` | 每任务CPU核心数 | `1`, `2`, `4` |
| `--ntasks-per-core` | 每CPU核心任务数 | `1`, `2` |
| `--time` | 墙钟时间限制 | `01:00:00`, `12:30:00` |
| `--distribution` | 任务分布策略 | `block`, `cyclic`, `plane` |
| `--nodelist` | 指定节点选择 | `cn[1-5]`, `node001` |
| `--exclude` | 排除的节点 | `cn[3,4]`, `node002` |
| `--output` | 标准输出文件 | `job_%j.out` |
| `--error` | 错误输出文件 | `job_%j.err` |
| `--chdir` | 工作目录 | `/path/to/workdir` |

## 🚀 快速开始

### 环境要求

- 📦 Node.js (v16 或更高版本)
- 📦 npm 或 yarn 包管理器

### 安装步骤

1. **克隆仓库**
   ```bash
   git clone <repository-url>
   cd slurm-sbatch-generator
   ```

2. **安装依赖**
   ```bash
   npm install
   ```

3. **启动开发服务器**
   ```bash
   npm run dev
   ```

4. **打开浏览器**
   ```
   http://localhost:5173
   ```

### 生产环境构建

```bash
npm run build
```

构建文件将生成在 `dist/` 目录中。

## 💡 使用指南

### 基本工作流程

1. **📝 填写作业详情**
   - 输入描述性的作业名称
   - 选择合适的分区（必填）

2. **⚙️ 配置资源**
   - 设置节点和任务数量
   - 指定CPU需求
   - 设置时间限制

3. **🔧 高级选项**（可选）
   - 配置任务分布
   - 指定节点列表或排除节点
   - 启用模块加载

4. **📋 生成并复制**
   - 实时查看生成的脚本
   - 点击"复制脚本"复制到剪贴板
   - 使用 `sbatch script.sh` 提交到SLURM

### 生成脚本示例

```bash
#!/bin/bash

#SBATCH --job-name=my-computation
#SBATCH --partition=small
#SBATCH --nodes=2
#SBATCH --ntasks=24
#SBATCH --ntasks-per-node=12
#SBATCH --cpus-per-task=1
#SBATCH --time=02:00:00
#SBATCH --output=job_%j.out
#SBATCH --error=job_%j.err

module load gcc/9.3.0
module load openmpi/4.0.3

mpirun ./my_application
```

## 🏗️ 项目结构

```
slurm-sbatch-generator/
├── 📁 src/
│   ├── 📄 App.vue          # 主应用组件
│   └── 📄 main.js          # 应用入口点
├── 📄 index.html           # HTML模板
├── 📄 package.json         # 项目依赖
├── 📄 vite.config.js       # Vite配置
└── 📄 README_zh.md        # 本文件
```

## 🛠️ 技术栈

- **⚡ Vue 3**: 渐进式JavaScript框架
- **🎨 Arco Design**: 企业级UI组件库
- **⚡ Vite**: 下一代前端构建工具
- **📝 JavaScript ES6+**: 现代JavaScript特性

## 🎯 任务分布说明

- **🔄 block (顺序分布)**: 任务按顺序分配到节点，适合需要节点内通信的应用
- **🔄 cyclic (轮流分布)**: 任务轮流分配到各节点，适合负载均衡
- **🔄 plane (负载均衡分布)**: 优化的负载均衡分布，适合大规模并行计算

## 🤝 贡献指南

我们欢迎贡献！请随时提交问题和拉取请求。

1. 🍴 Fork 仓库
2. 🌿 创建功能分支 (`git checkout -b feature/amazing-feature`)
3. 💾 提交更改 (`git commit -m 'Add some amazing feature'`)
4. 📤 推送到分支 (`git push origin feature/amazing-feature`)
5. 🔄 打开 Pull Request

## 📄 许可证

此项目是开源的，基于 [MIT License](LICENSE) 许可证。

## 🆘 技术支持

- 📧 通过 issue 报告错误
- 💡 通过 issue 提交功能请求
- 📖 创建新 issue 前请检查现有问题

## 🎓 使用场景

### 适用用户
- 🎓 **科研人员**: 需要频繁提交计算任务的研究者
- 👨‍💻 **HPC用户**: 高性能计算集群的日常使用者
- 🔬 **数据科学家**: 需要大规模数据处理的专业人士
- 🎯 **系统管理员**: 需要为用户提供易用工具的管理员

### 典型应用
- 🧮 **科学计算**: 数值模拟、分子动力学、天体物理计算
- 🤖 **机器学习**: 大规模模型训练、参数优化
- 📊 **数据分析**: 批量数据处理、统计分析
- 🔬 **生物信息学**: 基因序列分析、蛋白质结构预测

## 🙏 致谢

- 使用 ❤️ 基于 Vue 3 和 Arco Design 构建
- 受到简化HPC作业提交工具需求的启发
- 感谢 SLURM 社区提供的全面文档

---

**计算愉快！** 🎉 如果这个工具有助于简化您的HPC工作流程，请考虑给它一个 ⭐！