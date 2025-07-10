# 🚀 SLURM Sbatch Script Generator

> 📚 [中文文档](README_zh.md)

A modern, user-friendly web application for generating SLURM sbatch submission scripts with real-time preview and one-click copying functionality.

## ✨ Features

- 🎨 **Modern UI**: Built with Vue 3 + Arco Design for a sleek, responsive interface
- 🔧 **Core SLURM Parameters**: Comprehensive support for essential job scheduling parameters
- 🏷️ **Preset Partitions**: Quick selection for common partition types (small, large, ssd, long)
- 📦 **Module Support**: Optional environment module loading functionality
- 📋 **One-Click Copy**: Instantly copy generated scripts to clipboard
- 🧹 **Quick Reset**: Clear all form fields with a single click
- 🌐 **Real-time Preview**: Live script generation as you type
- 📱 **Responsive Design**: Works seamlessly on desktop and mobile devices

## 🛠️ Supported SLURM Parameters

| Parameter | Description | Example |
|-----------|-------------|---------|
| `--job-name` | Job name identifier | `my-computation` |
| `--partition` | Queue/partition selection | `small`, `large`, `ssd`, `long` |
| `--nodes` | Number of compute nodes | `1`, `2-4` |
| `--ntasks` | Total number of tasks | `24`, `48` |
| `--ntasks-per-node` | Tasks per node | `12`, `24` |
| `--cpus-per-task` | CPU cores per task | `1`, `2`, `4` |
| `--ntasks-per-core` | Tasks per CPU core | `1`, `2` |
| `--time` | Wall time limit | `01:00:00`, `12:30:00` |
| `--distribution` | Task distribution strategy | `block`, `cyclic`, `plane` |
| `--nodelist` | Specific node selection | `cn[1-5]`, `node001` |
| `--exclude` | Nodes to exclude | `cn[3,4]`, `node002` |
| `--output` | Standard output file | `job_%j.out` |
| `--error` | Error output file | `job_%j.err` |
| `--chdir` | Working directory | `/path/to/workdir` |

## 🚀 Quick Start

### Prerequisites

- 📦 Node.js (v16 or higher)
- 📦 npm or yarn package manager

### Installation

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd slurm-sbatch-generator
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Start development server**
   ```bash
   npm run dev
   ```

4. **Open your browser**
   ```
   http://localhost:5173
   ```

### Build for Production

```bash
npm run build
```

The built files will be in the `dist/` directory.

## 💡 Usage Guide

### Basic Workflow

1. **📝 Fill in Job Details**
   - Enter a descriptive job name
   - Select appropriate partition (required)

2. **⚙️ Configure Resources**
   - Set number of nodes and tasks
   - Specify CPU requirements
   - Set time limits

3. **🔧 Advanced Options** (Optional)
   - Configure task distribution
   - Specify node lists or exclusions
   - Enable module loading

4. **📋 Generate & Copy**
   - Review the generated script in real-time
   - Click "复制脚本" to copy to clipboard
   - Submit to SLURM with `sbatch script.sh`

### Example Generated Script

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

## 🏗️ Project Structure

```
slurm-sbatch-generator/
├── 📁 src/
│   ├── 📄 App.vue          # Main application component
│   └── 📄 main.js          # Application entry point
├── 📄 index.html           # HTML template
├── 📄 package.json         # Project dependencies
├── 📄 vite.config.js       # Vite configuration
└── 📄 README.md           # This file
```

## 🛠️ Technology Stack

- **⚡ Vue 3**: Progressive JavaScript framework
- **🎨 Arco Design**: Enterprise-class UI component library
- **⚡ Vite**: Next generation frontend tooling
- **📝 JavaScript ES6+**: Modern JavaScript features

## 🤝 Contributing

We welcome contributions! Please feel free to submit issues and pull requests.

1. 🍴 Fork the repository
2. 🌿 Create your feature branch (`git checkout -b feature/amazing-feature`)
3. 💾 Commit your changes (`git commit -m 'Add some amazing feature'`)
4. 📤 Push to the branch (`git push origin feature/amazing-feature`)
5. 🔄 Open a Pull Request

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

## 🆘 Support

- 📧 Create an issue for bug reports
- 💡 Submit feature requests via issues
- 📖 Check existing issues before creating new ones

## 🙏 Acknowledgments

- Built with ❤️ using Vue 3 and Arco Design
- Inspired by the need for user-friendly HPC job submission tools
- Thanks to the SLURM community for comprehensive documentation

---

**Happy Computing!** 🎉 If this tool helps streamline your HPC workflow, please consider giving it a ⭐!