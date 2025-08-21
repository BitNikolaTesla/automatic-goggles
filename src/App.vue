<template>
  <div class="app-container">
    <!-- 顶部导航栏 -->
    <header class="top-nav">
      <div class="nav-left">
        <div class="logo-container">
          <img src="/logo.svg" alt="Logo" class="logo" />
        </div>
      </div>
      
      <div class="nav-center">
        <div class="nav-tabs">
          <button 
            v-for="tab in navTabs" 
            :key="tab.key"
            :class="['nav-tab', { active: activeTab === tab.key }]"
            @click="activeTab = tab.key"
          >
            {{ tab.label }}
          </button>
        </div>
      </div>
      
      <div class="nav-right">
        <div class="login-icon">
          <svg width="24" height="24" viewBox="0 0 24 24" fill="currentColor">
            <path d="M12 12c2.21 0 4-1.79 4-4s-1.79-4-4-4-4 1.79-4 4 1.79 4 4 4zm0 2c-2.67 0-8 1.34-8 4v2h16v-2c0-2.66-5.33-4-8-4z"/>
          </svg>
        </div>
      </div>
    </header>

    <!-- 主要内容区域 -->
    <main class="main-content">
      <!-- 左侧面板 -->
      <aside class="left-panel">
        <!-- 知识库目录 -->
        <div class="knowledge-base">
          <div class="section-title">知识库目录</div>
          <div class="tree-container">
            <div class="tree-node folder" v-for="folder in knowledgeTree" :key="folder.id">
              <div class="node-content" @click="toggleFolder(folder.id)">
                <svg class="expand-icon" :class="{ expanded: folder.expanded }" width="16" height="16" viewBox="0 0 16 16" fill="currentColor">
                  <path d="M6 12l4-4-4-4v8z"/>
                </svg>
                <svg class="node-icon" width="16" height="16" viewBox="0 0 16 16" fill="currentColor">
                  <path d="M1.75 2.5a.25.25 0 00-.25.25v10.5c0 .138.112.25.25.25h12.5a.25.25 0 00.25-.25v-8.5a.25.25 0 00-.25-.25H7.5L6.25 2.5H1.75z"/>
                </svg>
                <span class="node-label">{{ folder.label }}</span>
              </div>
              <div class="children" v-if="folder.expanded && folder.children">
                <div class="tree-node file" v-for="file in folder.children" :key="file.id">
                  <div class="node-content">
                    <svg class="node-icon" width="16" height="16" viewBox="0 0 16 16" fill="currentColor">
                      <path d="M2 2.75C2 1.784 2.784 1 3.75 1h6.586c.464 0 .909.184 1.237.513l2.914 2.914c.329.328.513.773.513 1.237v8.586A1.75 1.75 0 0113.25 16h-9.5A1.75 1.75 0 012 14.25V2.75z"/>
                    </svg>
                    <span class="node-label">{{ file.label }}</span>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>

        <!-- 底部功能区 -->
        <div class="bottom-actions">
          <div class="action-item" @click="handleUpload">
            <div class="action-content">
              <svg class="action-icon" width="16" height="16" viewBox="0 0 16 16" fill="currentColor">
                <path d="M8.5 1.5a.5.5 0 00-1 0v5.793L6.354 6.146a.5.5 0 10-.708.708l2 2a.5.5 0 00.708 0l2-2a.5.5 0 00-.708-.708L8.5 7.293V1.5z"/>
                <path d="M3 9.5a.5.5 0 01.5-.5h9a.5.5 0 010 1h-9a.5.5 0 01-.5-.5z"/>
                <path d="M3 12.5a.5.5 0 01.5-.5h9a.5.5 0 010 1h-9a.5.5 0 01-.5-.5z"/>
              </svg>
              <span class="action-text">上传文件</span>
            </div>
            <div class="action-plus">
              <svg width="16" height="16" viewBox="0 0 16 16" fill="currentColor">
                <path d="M8 4a.5.5 0 01.5.5v3h3a.5.5 0 010 1h-3v3a.5.5 0 01-1 0v-3h-3a.5.5 0 010-1h3v-3A.5.5 0 018 4z"/>
              </svg>
            </div>
          </div>
          
          <div class="action-item" @click="handleUserManagement">
            <div class="action-content">
              <svg class="action-icon" width="16" height="16" viewBox="0 0 16 16" fill="currentColor">
                <path d="M8 8a3 3 0 100-6 3 3 0 000 6zm2-3a2 2 0 11-4 0 2 2 0 014 0zm4 8c0 1-1 1-1 1H3s-1 0-1-1 1-4 6-4 6 3 6 4zm-1-.004c-.001-.246-.154-.986-.832-1.664C11.516 10.68 10.289 10 8 10c-2.29 0-3.516.68-4.168 1.332-.678.678-.83 1.418-.832 1.664h10z"/>
              </svg>
              <span class="action-text">用户管理</span>
            </div>
            <div class="action-plus">
              <svg width="16" height="16" viewBox="0 0 16 16" fill="currentColor">
                <path d="M8 4a.5.5 0 01.5.5v3h3a.5.5 0 010 1h-3v3a.5.5 0 01-1 0v-3h-3a.5.5 0 010-1h3v-3A.5.5 0 018 4z"/>
              </svg>
            </div>
          </div>
        </div>
      </aside>

      <!-- 右侧内容区域 -->
      <section class="right-content">
        <!-- 预留空白区域 -->
        <div class="content-placeholder">
          <div class="placeholder-text">
            右侧内容区域（待实现）
          </div>
        </div>
      </section>
    </main>
  </div>
</template>

<script setup>
import { ref } from 'vue'

// 导航标签页
const navTabs = [
  { key: 'knowledge', label: '知识库' },
  { key: 'chat', label: '聊天' },
  { key: 'document', label: '文档管理' },
  { key: 'config', label: '配置' }
]

const activeTab = ref('knowledge')

// 知识库树形数据
const knowledgeTree = ref([
  {
    id: 1,
    label: '技术文档',
    type: 'folder',
    expanded: true,
    children: [
      { id: 11, label: 'Vue.js指南.pdf', type: 'file' },
      { id: 12, label: 'JavaScript高级教程.docx', type: 'file' },
      { id: 13, label: 'CSS样式指南.md', type: 'file' }
    ]
  },
  {
    id: 2,
    label: '产品规格',
    type: 'folder',
    expanded: true,
    children: [
      { id: 21, label: '需求文档v1.0.pdf', type: 'file' },
      { id: 22, label: '原型设计.fig', type: 'file' }
    ]
  },
  {
    id: 3,
    label: '会议记录',
    type: 'folder',
    expanded: true,
    children: [
      { id: 31, label: '2024-01-15 项目启动会.docx', type: 'file' },
      { id: 32, label: '2024-01-22 需求评审.pdf', type: 'file' },
      { id: 33, label: '2024-01-29 技术方案讨论.md', type: 'file' }
    ]
  }
])

// 切换文件夹展开状态
const toggleFolder = (folderId) => {
  const folder = knowledgeTree.value.find(f => f.id === folderId)
  if (folder) {
    folder.expanded = !folder.expanded
  }
}

// 处理上传文件
const handleUpload = () => {
  console.log('上传文件')
}

// 处理用户管理
const handleUserManagement = () => {
  console.log('用户管理')
}
</script>

<style scoped>
.app-container {
  display: flex;
  flex-direction: column;
  height: 100vh;
  background-color: #f5f5f5;
}

/* 顶部导航栏 */
.top-nav {
  display: flex;
  align-items: center;
  justify-content: space-between;
  height: 60px;
  background: white;
  border-bottom: 1px solid #e5e5e5;
  padding: 0 24px;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
}

.nav-left {
  display: flex;
  align-items: center;
}

.logo-container {
  width: 40px;
  height: 40px;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: #f0f0f0;
  border-radius: 8px;
}

.logo {
  width: 32px;
  height: 32px;
  object-fit: contain;
}

.nav-center {
  display: flex;
  align-items: center;
}

.nav-tabs {
  display: flex;
  background: #f8f9fa;
  border-radius: 8px;
  padding: 4px;
  gap: 4px;
}

.nav-tab {
  padding: 8px 16px;
  border: none;
  background: transparent;
  border-radius: 6px;
  font-size: 14px;
  font-weight: 500;
  color: #666;
  cursor: pointer;
  transition: all 0.2s ease;
}

.nav-tab:hover {
  background: rgba(0, 0, 0, 0.05);
  color: #333;
}

.nav-tab.active {
  background: white;
  color: #409eff;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
}

.nav-right {
  display: flex;
  align-items: center;
}

.login-icon {
  width: 40px;
  height: 40px;
  display: flex;
  align-items: center;
  justify-content: center;
  background: #f8f9fa;
  border-radius: 50%;
  cursor: pointer;
  transition: all 0.2s ease;
  color: #666;
}

.login-icon:hover {
  background: #e9ecef;
}

/* 主要内容区域 */
.main-content {
  display: flex;
  flex: 1;
  overflow: hidden;
}

/* 左侧面板 */
.left-panel {
  width: 280px;
  background: white;
  border-right: 1px solid #e5e5e5;
  display: flex;
  flex-direction: column;
}

.knowledge-base {
  flex: 1;
  padding: 16px;
  overflow-y: auto;
}

.section-title {
  font-size: 16px;
  font-weight: 600;
  color: #333;
  margin-bottom: 16px;
  padding-bottom: 8px;
  border-bottom: 1px solid #f0f0f0;
}

.tree-container {
  margin-top: 8px;
}

.tree-node {
  margin-bottom: 4px;
}

.tree-node.folder .children {
  margin-left: 20px;
  margin-top: 4px;
}

.node-content {
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 6px 8px;
  border-radius: 6px;
  cursor: pointer;
  transition: background-color 0.2s ease;
}

.node-content:hover {
  background: #f5f7fa;
}

.expand-icon {
  color: #666;
  transition: transform 0.2s ease;
  flex-shrink: 0;
}

.expand-icon.expanded {
  transform: rotate(90deg);
}

.tree-node.file .node-content {
  padding-left: 24px;
}

.node-icon {
  color: #666;
  flex-shrink: 0;
}

.node-label {
  font-size: 14px;
  color: #333;
  flex: 1;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

/* 底部功能区 */
.bottom-actions {
  padding: 16px;
  border-top: 1px solid #f0f0f0;
  background: #fafafa;
}

.action-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 12px 16px;
  margin-bottom: 8px;
  background: white;
  border: 1px solid #e5e5e5;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.2s ease;
}

.action-item:last-child {
  margin-bottom: 0;
}

.action-item:hover {
  border-color: #409eff;
  box-shadow: 0 2px 8px rgba(64, 158, 255, 0.1);
}

.action-content {
  display: flex;
  align-items: center;
  gap: 8px;
}

.action-icon {
  color: #666;
  flex-shrink: 0;
}

.action-text {
  font-size: 14px;
  color: #333;
}

.action-plus {
  width: 24px;
  height: 24px;
  display: flex;
  align-items: center;
  justify-content: center;
  background: #f8f9fa;
  border-radius: 4px;
  color: #666;
}

/* 右侧内容区域 */
.right-content {
  flex: 1;
  background: white;
  display: flex;
  align-items: center;
  justify-content: center;
}

.content-placeholder {
  text-align: center;
  color: #999;
}

.placeholder-text {
  font-size: 18px;
  font-weight: 500;
}
</style>