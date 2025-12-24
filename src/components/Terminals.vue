<template>
  <div class="terminals">
    <div 
      v-for="terminal in terminals" 
      :key="terminal.id"
      v-show="terminal.isOpen"
      class="terminal-window"
      :style="{ 
        left: terminal.position.x + 'px', 
        top: terminal.position.y + 'px',
        zIndex: terminal.zIndex
      }"
      @mousedown="bringToFront(terminal.id)"
    >
      <div class="window-header" @mousedown="startDrag($event, terminal.id)">
        <div class="buttons">
          <span class="dot red" @click.stop="closeTerminal(terminal.id)"></span>
          <span class="dot yellow"></span>
          <span class="dot green"></span>
        </div>
        <div class="title">{{ terminal.title }}</div>
      </div>
      
      <div class="window-body">
        <!-- Content for About Terminal -->
        <template v-if="terminal.id === 'about'">
          <div class="line">
            <span class="prompt">root@nutssss ~</span> <span class="command">cat /home/redstar/me.txt</span>
          </div>
          <div class="output text-content">
            爱好计算机，会去自学自己感兴趣的一切东西<br>
            略懂H5，C#开发，Python<br>
            同时我也很喜欢Minecraft，Raspberry派的开发<br>
            这条路我才刚踏上，未来的路还很长<br>
            路上的坎一定会很多<br>
            在我眼里<br>
            没有什么问题是不能解决的，如果有那就多尝试几次甚至上百次<br>
            即使前方的路看不清，也要有硬生生给自己开出一条路的勇气
          </div>
          <div class="line">
            <span class="prompt">root@nutssss ~ </span><span class="command">ll /home/redstar/project</span>
          </div>
          <div class="output text-content">
            <div class="file-list">
              <div v-for="project in projects" :key="project.id" class="file-item">
                <span class="permissions">drwxr-xr-x</span>
                <span class="links">2</span>
                <span class="user">root</span>
                <span class="group">root</span>
                <span class="size">4096</span>
                <span class="date">{{ project.date }}</span>
                <span class="name clickable" @click="openProject(project)">{{ project.name }}</span>
              </div>
            </div>
          </div>
        </template>

        <!-- Content for Project Terminal -->
        <template v-else-if="terminal.type === 'project'">
          <div class="line">
            <span class="prompt">root@nutssss ~</span> <span class="command">cat README.md</span>
          </div>
          <div class="output text-content">
            <div v-for="(line, index) in terminal.data.description" :key="index">
              {{ line }}<br>
            </div>
          </div>
          <div class="line">
            <span class="prompt">root@nutssss ~</span> <span class="cursor-block"></span>
          </div>
        </template>

        <!-- Content for Links Terminal -->
        <template v-else-if="terminal.id === 'links'">
          <div class="line">
            <span class="prompt">root@nutssss ~</span> <span class="command">./links.sh</span>
          </div>
          <div class="output">
            我的其他站点:
            <ul>
              <li>Blog</li>
              <li>Love</li>
              <li>Note</li>
              <li>Mc</li>
            </ul>
          </div>
          <div class="line">
            <span class="prompt">root@nutssss ~</span> <span class="command">McBlog</span> <span class="comment">rm -rf /过去的自己/*</span>
          </div>
          <div class="line">
            <span class="prompt">root@nutssss ~</span> <span class="cursor-block"></span>
          </div>
        </template>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onUnmounted } from 'vue'

const terminals = ref([
  { 
    id: 'about', 
    title: 'lovexhj - bash', 
    isOpen: true, 
    position: { x: 0, y: 0 }, 
    zIndex: 10 
  },
  { 
    id: 'links', 
    title: 'lovexhj - bash', 
    isOpen: false, 
    position: { x: 30, y: 30 }, 
    zIndex: 9 
  }
])

const projects = [
  {
    id: 'ximu-note',
    name: '西木笔记',
    date: 'Dec 25 20:00',
    description: [
      '西木笔记 - 一个专注于记录的轻量级笔记应用',
      '技术栈: Vue3, TypeScript, Vite',
      '特点: 支持Markdown, 实时保存, 多端同步'
    ]
  },
  {
    id: 'ximu-editor',
    name: '西木编辑器',
    date: 'Nov 11 10:24',
    description: [
      '西木编辑器 - 在线代码编辑器',
      '技术栈: Monaco Editor, Vue3',
      '功能: 代码高亮, 智能提示, 在线运行'
    ]
  },
  {
    id: 'esp8266',
    name: 'esp8266ATPackage',
    date: 'Oct 01 08:00',
    description: [
      'esp8266ATPackage - ESP8266 AT指令封装库',
      '语言: C/C++',
      '适用平台: STM32, Arduino',
      '功能: 简化AT指令操作, 提供稳定可靠的WiFi连接'
    ]
  }
]

const openProject = (project) => {
  const terminalId = `project-${project.id}`
  const existingTerminal = terminals.value.find(t => t.id === terminalId)
  
  if (existingTerminal) {
    existingTerminal.isOpen = true
    bringToFront(terminalId)
  } else {
    // Offset position for new window
    const count = terminals.value.length
    terminals.value.push({
      id: terminalId,
      title: `root@nutssss: ~/projects/${project.name}`,
      isOpen: true,
      position: { x: 50 + (count * 20), y: 50 + (count * 20) },
      zIndex: 100 + count,
      type: 'project',
      data: project
    })
    bringToFront(terminalId)
  }
}

let isDragging = false
let currentTerminalId = null
let startX = 0
let startY = 0
let initialLeft = 0
let initialTop = 0

const bringToFront = (id) => {
  const maxZ = Math.max(...terminals.value.map(t => t.zIndex))
  const terminal = terminals.value.find(t => t.id === id)
  if (terminal && terminal.zIndex < maxZ) {
    terminal.zIndex = maxZ + 1
  }
}

const startDrag = (event, id) => {
  isDragging = true
  currentTerminalId = id
  bringToFront(id)
  
  const terminal = terminals.value.find(t => t.id === id)
  
  startX = event.clientX
  startY = event.clientY
  initialLeft = terminal.position.x
  initialTop = terminal.position.y
  
  window.addEventListener('mousemove', onDrag)
  window.addEventListener('mouseup', stopDrag)
}

const onDrag = (event) => {
  if (!isDragging) return
  const dx = event.clientX - startX
  const dy = event.clientY - startY
  
  const terminal = terminals.value.find(t => t.id === currentTerminalId)
  if (terminal) {
    terminal.position.x = initialLeft + dx
    terminal.position.y = initialTop + dy
  }
}

const stopDrag = () => {
  isDragging = false
  currentTerminalId = null
  window.removeEventListener('mousemove', onDrag)
  window.removeEventListener('mouseup', stopDrag)
}

const closeTerminal = (id) => {
  const terminal = terminals.value.find(t => t.id === id)
  if (terminal) {
    terminal.isOpen = false
  }
}

onUnmounted(() => {
  window.removeEventListener('mousemove', onDrag)
  window.removeEventListener('mouseup', stopDrag)
})
</script>

<style scoped>
/* Terminals */
.terminals {
  position: relative;
  width: 600px;
  height: 500px;
}

.terminal-window {
  width: 600px;
  min-height: 400px;
  background: rgba(255, 255, 255, 0.95);
  border-radius: 8px;
  box-shadow: 0 20px 50px rgba(0,0,0,0.3);
  overflow: hidden;
  position: absolute;
  font-family: "Menlo", "Monaco", "Consolas", "Courier New", monospace;
  font-size: 14px;
}

.window-header {
  background: #e8e8e8; /* Light grey title bar */
  padding: 8px 15px;
  display: flex;
  align-items: center;
  position: relative;
  border-bottom: 1px solid #dcdcdc;
  cursor: grab;
  user-select: none;
}

.window-header:active {
  cursor: grabbing;
}

.buttons {
  display: flex;
  gap: 6px;
  z-index: 10; /* Ensure buttons are clickable */
}

.dot {
  width: 12px;
  height: 12px;
  border-radius: 50%;
  cursor: pointer;
}

.red { background: #ff5f56; border: 1px solid #e0443e; }
.yellow { background: #ffbd2e; border: 1px solid #dea123; }
.green { background: #27c93f; border: 1px solid #1aab29; }

.title {
  position: absolute;
  left: 0;
  right: 0;
  text-align: center;
  color: #666;
  font-weight: 500;
  pointer-events: none;
}

.window-body {
  padding: 15px;
  color: #333;
  line-height: 1.5;
}

.line {
  margin-bottom: 5px;
}

.prompt {
  color: #27c93f; /* Green prompt */
  font-weight: bold;
  margin-right: 8px;
}

.command {
  color: #333;
}

.comment {
  color: #999;
  margin-left: 10px;
}

.output {
  margin-bottom: 10px;
  color: #555;
}

.weather-icon {
  font-size: 24px;
}

.text-content {
  font-size: 13px;
  color: #666;
}

ul {
  list-style: disc;
  margin-left: 20px;
}

.cursor-block {
  display: inline-block;
  width: 8px;
  height: 16px;
  background: #333;
  vertical-align: middle;
  animation: blink 1s step-end infinite;
}

@keyframes blink {
  50% { opacity: 0; }
}

@media (max-width: 900px) {
  .terminals {
    width: 90%;
    max-width: 500px;
    height: auto;
    display: flex;
    flex-direction: column;
    gap: 20px;
  }

  .terminal-window {
    position: relative;
    width: 100%;
    top: auto !important;
    left: auto !important;
    right: auto !important;
    bottom: auto !important;
  }
}

.file-list {
  font-family: "Menlo", "Monaco", "Consolas", "Courier New", monospace;
  display: flex;
  flex-direction: column;
}

.file-item {
  display: flex;
  gap: 10px;
  white-space: pre;
}

.permissions {
  color: #666;
}

.links, .user, .group, .size {
  color: #666;
  display: none; /* Hide details on small screens if needed, or keep */
}

/* Show details on larger screens */
@media (min-width: 600px) {
  .links, .user, .group, .size {
    display: inline-block;
  }
}

.date {
  color: #666;
  min-width: 100px;
}

.name {
  color: #333;
  font-weight: bold;
}

.name.clickable {
  color: #27c93f;
  cursor: pointer;
  text-decoration: underline;
}

.name.clickable:hover {
  color: #1aab29;
}
</style>
