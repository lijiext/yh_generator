<template>
  <div class="app-container">
    <h1>SLURM Sbatch 脚本生成器</h1>
    
    <a-form
      :model="form"
      layout="vertical"
      class="form-container"
    >

      <a-row :gutter="16">
        <a-col :span="12">
          <a-form-item label="作业名称" field="jobName">
            <a-input v-model="form.jobName" placeholder="my-job" />
          </a-form-item>
        </a-col>
        <a-col :span="12">
          <a-form-item label="分区 *" field="partition" required>
            <a-select v-model="form.partition" placeholder="选择分区" allow-create>
              <a-option value="small">small</a-option>
              <a-option value="large">large</a-option>
              <a-option value="ssd">ssd</a-option>
              <a-option value="long">long</a-option>
            </a-select>
          </a-form-item>
        </a-col>
      </a-row>

      <a-row :gutter="16">
        <a-col :span="8">
          <a-form-item label="节点数 (nodes)" field="nodes">
            <a-input-number v-model="form.nodes" :min="1" placeholder="1" />
          </a-form-item>
        </a-col>
        <a-col :span="8">
          <a-form-item label="总任务数 (ntasks)" field="ntasks">
            <a-input-number v-model="form.ntasks" :min="1" placeholder="1" />
          </a-form-item>
        </a-col>
        <a-col :span="8">
          <a-form-item label="每节点任务数 (ntasks-per-node)" field="tasksPerNode">
            <a-input-number v-model="form.tasksPerNode" :min="1" placeholder="1" />
          </a-form-item>
        </a-col>
      </a-row>

      <a-row :gutter="16">
        <a-col :span="8">
          <a-form-item label="每任务CPU数 (cpus-per-task)" field="cpusPerTask">
            <a-input-number v-model="form.cpusPerTask" :min="1" placeholder="1" />
          </a-form-item>
        </a-col>
        <a-col :span="8">
          <a-form-item label="每核心任务数 (ntasks-per-core)" field="tasksPerCore">
            <a-input-number v-model="form.tasksPerCore" :min="1" placeholder="1" />
          </a-form-item>
        </a-col>
        <a-col :span="8">
          <a-form-item label="时间限制 (time)" field="timeLimit">
            <a-input v-model="form.timeLimit" placeholder="01:00:00" />
          </a-form-item>
        </a-col>
      </a-row>

      <a-row :gutter="16">
        <a-col :span="12">
          <a-form-item label="任务分布 (distribution)" field="distribution">
            <a-select v-model="form.distribution" placeholder="选择分布方式">
              <a-option value="">默认</a-option>
              <a-option value="block">block 顺序分布</a-option>
              <a-option value="cyclic">cyclic 轮流分布</a-option>
              <a-option value="plane">plane 负载均衡分布</a-option>
            </a-select>
          </a-form-item>
        </a-col>
        <a-col :span="12">
          <a-form-item label="加载模块" field="loadModules">
            <a-switch v-model="form.loadModules" />
          </a-form-item>
        </a-col>
      </a-row>
      <a-form-item v-if="form.loadModules" label="模块列表" field="modules">
        <a-textarea
          v-model="form.modules"
          placeholder="module load gcc/9.3.0&#10;module load openmpi/4.0.3"
          :auto-size="{ minRows: 2, maxRows: 4 }"
        />
      </a-form-item>
      <a-row :gutter="16">
        <a-col :span="12">
          <a-form-item label="节点列表 (nodelist)" field="nodelist">
            <a-input v-model="form.nodelist" placeholder="cn[1-5]" />
          </a-form-item>
        </a-col>
        <a-col :span="12">
          <a-form-item label="排除节点 (exclude)" field="exclude">
            <a-input v-model="form.exclude" placeholder="cn[3,4]" />
          </a-form-item>
        </a-col>
      </a-row>
      <a-row :gutter="16">
        <a-col :span="12">
          <a-form-item label="输出文件" field="outputFile">
            <a-input v-model="form.outputFile" placeholder="job_%j.out" />
          </a-form-item>
        </a-col>
        <a-col :span="12">
          <a-form-item label="错误文件" field="errorFile">
            <a-input v-model="form.errorFile" placeholder="job_%j.err" />
          </a-form-item>
        </a-col>
      </a-row>

      <a-form-item label="工作目录" field="workingDir">
        <a-input v-model="form.workingDir" placeholder="/path/to/working/directory" />
      </a-form-item>

      <a-form-item label="执行命令" field="commands">
        <a-textarea
          v-model="form.commands"
          placeholder="在此输入您的命令..."
          :auto-size="{ minRows: 4, maxRows: 8 }"
        />
      </a-form-item>

      <a-form-item label="额外SLURM选项" field="additionalOptions">
        <a-textarea
          v-model="form.additionalOptions"
          placeholder="额外的 #SBATCH 指令（每行一个）"
          :auto-size="{ minRows: 2, maxRows: 4 }"
        />
      </a-form-item>
    </a-form>

    <a-divider />

    <div class="script-section">
      <h3>生成的Sbatch脚本</h3>
      <div class="script-container">
        <pre class="script-content">{{ generatedScript }}</pre>
        <div class="button-group">
          <a-button
            type="primary"
            class="copy-button"
            @click="copyToClipboard"
          >
            复制脚本
          </a-button>
          <a-button
            type="outline"
            status="warning"
            class="clear-button"
            @click="clearForm"
          >
            一键清空
          </a-button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, computed } from 'vue'
import { Message } from '@arco-design/web-vue'

export default {
  name: 'App',
  setup() {
    const form = ref({
      jobName: 'my-job',
      partition: '',
      nodes: null,
      ntasks: null,
      tasksPerNode: null,
      cpusPerTask: null,
      tasksPerCore: null,
      timeLimit: '01:00:00',
      distribution: '',
      nodelist: '',
      exclude: '',
      loadModules: false,
      modules: '',
      outputFile: 'job_%j.out',
      errorFile: 'job_%j.err',
      workingDir: '',
      commands: '',
      additionalOptions: ''
    })

    const generatedScript = computed(() => {
      let script = '#!/bin/bash\n\n'
      
      script += `#SBATCH --job-name=${form.value.jobName}\n`
      
      if (form.value.partition) {
        script += `#SBATCH --partition=${form.value.partition}\n`
      }
      if (form.value.nodes) {
        script += `#SBATCH --nodes=${form.value.nodes}\n`
      }
      if (form.value.ntasks) {
        script += `#SBATCH --ntasks=${form.value.ntasks}\n`
      }
      if (form.value.tasksPerCore) {
        script += `#SBATCH --ntasks-per-node=${form.value.tasksPerNode}\n`
      }
      if (form.value.cpusPerTask) {
        script += `#SBATCH --cpus-per-task=${form.value.cpusPerTask}\n`
      }
      
      if (form.value.tasksPerCore) {
        script += `#SBATCH --ntasks-per-core=${form.value.tasksPerCore}\n`
      }
      
      if (form.value.timeLimit) {
        script += `#SBATCH --time=${form.value.timeLimit}\n`
      }
      
      if (form.value.distribution) {
        script += `#SBATCH --distribution=${form.value.distribution}\n`
      }
      
      if (form.value.nodelist) {
        script += `#SBATCH --nodelist=${form.value.nodelist}\n`
      }
      
      if (form.value.exclude) {
        script += `#SBATCH --exclude=${form.value.exclude}\n`
      }
      
      if (form.value.outputFile) {
        script += `#SBATCH --output=${form.value.outputFile}\n`
      }
      
      if (form.value.errorFile) {
        script += `#SBATCH --error=${form.value.errorFile}\n`
      }
      
      if (form.value.workingDir) {
        script += `#SBATCH --chdir=${form.value.workingDir}\n`
      }
      
      if (form.value.additionalOptions) {
        const options = form.value.additionalOptions.split('\n')
        options.forEach(option => {
          if (option.trim()) {
            script += option.startsWith('#SBATCH') ? `${option}\n` : `#SBATCH ${option}\n`
          }
        })
      }
      
      script += '\n'
      
      if (form.value.loadModules && form.value.modules) {
        script += form.value.modules + '\n\n'
      }
      
      if (form.value.commands) {
        script += form.value.commands + '\n'
      }
      
      return script
    })

    const copyToClipboard = async () => {
      try {
        await navigator.clipboard.writeText(generatedScript.value)
        Message.success('脚本已复制到剪贴板!')
      } catch (err) {
        Message.error('复制脚本失败')
      }
    }

    const clearForm = () => {
      form.value = {
        jobName: 'my-job',
        partition: '',
        nodes: null,
        ntasks: null,
        tasksPerNode: null,
        cpusPerTask: null,
        tasksPerCore: null,
        timeLimit: '01:00:00',
        distribution: '',
        nodelist: '',
        exclude: '',
        loadModules: false,
        modules: '',
        outputFile: 'job_%j.out',
        errorFile: 'job_%j.err',
        workingDir: '',
        commands: '',
        additionalOptions: ''
      }
      Message.success('表单已清空')
    }

    return {
      form,
      generatedScript,
      copyToClipboard,
      clearForm
    }
  }
}
</script>

<style scoped>
.app-container {
  padding: 16px;
  max-width: 1200px;
  margin: 0 auto;
  background: #fff;
}

.form-container {
  margin-bottom: 20px;
}


.script-section {
  margin-top: 20px;
}

.script-container {
  position: relative;
  background: #f8f9fa;
  border: 1px solid #dee2e6;
  border-radius: 6px;
  padding: 16px;
  margin-top: 8px;
}

.script-content {
  font-family: 'Consolas', 'Monaco', 'Courier New', monospace;
  font-size: 13px;
  line-height: 1.4;
  margin: 0;
  white-space: pre-wrap;
  word-wrap: break-word;
  color: #212529;
}

.button-group {
  position: absolute;
  top: 8px;
  right: 8px;
  display: flex;
  gap: 8px;
}

.copy-button,
.clear-button {
  font-size: 12px;
  padding: 4px 12px;
  height: 28px;
}

h1 {
  color: #1d2129;
  margin: 0 0 20px 0;
  font-size: 24px;
  font-weight: 600;
}

h3 {
  color: #1d2129;
  margin-bottom: 8px;
  font-size: 16px;
  font-weight: 600;
}
</style>