<script setup>
import { ref, computed, onMounted } from 'vue'

// 响应式数据
const searchQuery = ref('')
const activeCategory = ref('all')

// 分类数据
const categories = [
  { key: 'all', name: '全部' },
  { key: 'chatbot', name: '聊天对话' },
  { key: 'writing', name: '写作助手' },
  { key: 'coding', name: '编程开发' },
  { key: 'media', name: '图像视频' },
  { key: 'audio', name: '音频处理' },
  { key: 'music', name: '音乐' },
  { key: 'productivity', name: '办公效率' },
  { key: 'research', name: '学术研究' }
]

// AI网站数据
const aiSites = [
  // 聊天对话类
  {
    name: 'OpenAI',
    url: 'https://openai.com',
    icon: '🧠',
    description: 'OpenAI官网，GPT系列模型开发商',
    category: 'chatbot',
    tags: ['GPT', 'API', 'OpenAI']
  },
  {
    name: 'ChatGPT',
    url: 'https://chat.openai.com',
    icon: '💬',
    description: 'OpenAI开发的强大AI对话助手',
    category: 'chatbot',
    tags: ['对话', '问答', '创作']
  },
  {
    name: 'Claude',
    url: 'https://claude.ai',
    icon: '🎭',
    description: 'Anthropic的AI助手，善于深度思考',
    category: 'chatbot',
    tags: ['对话', '分析', '写作']
  },
  {
    name: 'DeepSeek',
    url: 'https://chat.deepseek.com',
    icon: '🔍',
    description: '国产优秀的AI对话模型',
    category: 'chatbot',
    tags: ['对话', '中文', '编程']
  },
  {
    name: 'Gemini',
    url: 'https://gemini.google.com',
    icon: '✨',
    description: 'Google的多模态AI助手',
    category: 'chatbot',
    tags: ['对话', '多模态', '搜索']
  },
  {
    name: '文心一言',
    url: 'https://yiyan.baidu.com',
    icon: '🎯',
    description: '百度推出的AI对话平台',
    category: 'chatbot',
    tags: ['对话', '中文', '百度']
  },
  {
    name: '通义千问',
    url: 'https://tongyi.aliyun.com',
    icon: '🌟',
    description: '阿里云的大语言模型',
    category: 'chatbot',
    tags: ['对话', '中文', '阿里']
  },
  {
    name: '阶跃星辰',
    url: 'https://www.stepfun.com/',
    icon: '🌠',
    description: '阶跃星辰的AI对话平台',
    category: 'chatbot',
    tags: ['对话', '中文', '国产']
  },
  {
    name: '智谱清言',
    url: 'https://chatglm.cn/main/alltoolsdetail?lang=zh',
    icon: '💡',
    description: '智谱AI推出的对话系统',
    category: 'chatbot',
    tags: ['对话', '中文', '智谱']
  },
  {
    name: 'Kimi',
    url: 'https://www.kimi.com',
    icon: '🤖',
    description: 'Moonshot AI的智能助手',
    category: 'chatbot',
    tags: ['对话', '长文本', '智能助手']
  },
  {
    name: '扣子',
    url: 'https://www.coze.cn/home',
    icon: '🎪',
    description: '字节跳动的AI机器人平台',
    category: 'chatbot',
    tags: ['机器人', '平台', '字节']
  },
  {
    name: 'Dify.AI',
    url: 'https://dify.ai/',
    icon: '🔧',
    description: 'LLMOps平台，构建AI应用',
    category: 'chatbot',
    tags: ['平台', 'LLMOps', '开发']
  },
  
  // 写作助手类
  {
    name: 'Jasper',
    url: 'https://www.jasper.ai',
    icon: '✍️',
    description: '专业的AI写作助手',
    category: 'writing',
    tags: ['写作', '营销', '内容']
  },
  {
    name: 'Copy.ai',
    url: 'https://copy.ai',
    icon: '📝',
    description: 'AI驱动的文案创作工具',
    category: 'writing',
    tags: ['文案', '营销', '创意']
  },
  {
    name: 'Writesonic',
    url: 'https://writesonic.com',
    icon: '🚀',
    description: '快速生成高质量内容',
    category: 'writing',
    tags: ['写作', '博客', 'SEO']
  },
  
  // 编程开发类
  {
    name: 'GitHub Copilot',
    url: 'https://github.com/features/copilot',
    icon: '🐱',
    description: 'AI编程助手，提升开发效率',
    category: 'coding',
    tags: ['编程', '代码', 'GitHub']
  },
  {
    name: 'Cursor',
    url: 'https://cursor.sh',
    icon: '📺',
    description: 'AI驱动的代码编辑器',
    category: 'coding',
    tags: ['编程', '编辑器', 'AI']
  },
  {
    name: 'Codeium',
    url: 'https://codeium.com',
    icon: '⚡',
    description: '免费的AI编程助手',
    category: 'coding',
    tags: ['编程', '免费', '代码补全']
  },
  {
    name: 'Replit',
    url: 'https://replit.com',
    icon: '🔧',
    description: '在线编程环境与AI助手',
    category: 'coding',
    tags: ['编程', '在线', '协作']
  },
  {
    name: 'V0',
    url: 'https://v0.dev',
    icon: '🚀',
    description: 'Vercel的AI界面生成工具',
    category: 'coding',
    tags: ['UI生成', '前端', 'Vercel']
  },
  {
    name: 'Bolt',
    url: 'https://bolt.new/',
    icon: '⚡',
    description: 'AI全栈开发环境',
    category: 'coding',
    tags: ['全栈', '开发', '快速']
  },
  {
    name: 'Max',
    url: 'https://mgx.dev',
    icon: '🎯',
    description: 'AI代码生成和开发工具',
    category: 'coding',
    tags: ['代码生成', '开发', '工具']
  },
  {
    name: 'Devin',
    url: 'https://devin.ai',
    icon: '🤖',
    description: 'AI软件工程师，自主完成编程任务',
    category: 'coding',
    tags: ['AI工程师', '自动化', '全栈']
  },
  
  // 图像视频创作类
  {
    name: 'Midjourney',
    url: 'https://www.midjourney.com',
    icon: '🎨',
    description: '顶级AI图像生成工具',
    category: 'media',
    tags: ['图像', '艺术', '创作']
  },
  {
    name: 'DALL-E 3',
    url: 'https://openai.com/dall-e-3',
    icon: '🖼️',
    description: 'OpenAI的图像生成AI',
    category: 'media',
    tags: ['图像', '创意', 'OpenAI']
  },
  {
    name: 'Stable Diffusion',
    url: 'https://stablediffusionweb.com',
    icon: '🌈',
    description: '开源的图像生成模型',
    category: 'media',
    tags: ['图像', '开源', '免费']
  },
  {
    name: 'Leonardo AI',
    url: 'https://leonardo.ai',
    icon: '🎭',
    description: '专业的AI艺术创作平台',
    category: 'media',
    tags: ['艺术', '游戏', '设计']
  },
  {
    name: 'Runway',
    url: 'https://runwayml.com',
    icon: '🎬',
    description: 'AI视频编辑和生成工具',
    category: 'media',
    tags: ['视频', '编辑', '生成']
  },
  {
    name: 'Pika Labs',
    url: 'https://pika.art',
    icon: '🎪',
    description: 'AI视频生成平台',
    category: 'media',
    tags: ['视频', '生成', '创意']
  },
  {
    name: 'Synthesia',
    url: 'https://www.synthesia.io',
    icon: '🎥',
    description: 'AI虚拟人物视频制作',
    category: 'media',
    tags: ['视频', '虚拟人', '培训']
  },
  {
    name: '可灵',
    url: 'https://app.klingai.com/cn/',
    icon: '✨',
    description: '快手推出的AI视频生成平台',
    category: 'media',
    tags: ['视频', '生成', '快手']
  },
  {
    name: '即梦',
    url: 'https://jimeng.jianying.com/',
    icon: '🌙',
    description: '字节跳动的AI视频创作工具',
    category: 'media',
    tags: ['视频', '创作', '字节']
  },
  {
    name: 'Vidu',
    url: 'https://www.vidu.cn/create',
    icon: '🎦',
    description: '生数科技的AI视频生成平台',
    category: 'media',
    tags: ['视频', '生成', '中文']
  },
  {
    name: '硅基流动',
    url: 'https://account.siliconflow.cn/',
    icon: '🌊',
    description: '硅基流动的AI计算平台',
    category: 'media',
    tags: ['平台', '计算', 'API']
  },
  
  // 音频处理类
  {
    name: 'ElevenLabs',
    url: 'https://elevenlabs.io',
    icon: '🎵',
    description: 'AI语音合成和克隆',
    category: 'audio',
    tags: ['语音', '合成', '克隆']
  },
  {
    name: 'Murf',
    url: 'https://murf.ai',
    icon: '🎤',
    description: 'AI语音生成器',
    category: 'audio',
    tags: ['语音', '配音', '文本转语音']
  },
  
  // 音乐创作类
  {
    name: 'Suno',
    url: 'https://suno.ai',
    icon: '🎶',
    description: 'AI音乐创作平台',
    category: 'music',
    tags: ['音乐', '创作', '生成']
  },
  {
    name: 'Suno音乐',
    url: 'https://ai.sunoyinyue.com/music',
    icon: '🎼',
    description: '基于Suno的中文音乐生成平台',
    category: 'music',
    tags: ['音乐', '中文', '生成']
  },
  {
    name: 'MakeBestMusic',
    url: 'https://makebestmusic.com/app/create-music-new',
    icon: '🎹',
    description: '专业的AI音乐制作工具',
    category: 'music',
    tags: ['音乐', '制作', '专业']
  },
  {
    name: 'Mureka',
    url: 'https://www.mureka.cn',
    icon: '🎺',
    description: '国产AI音乐创作平台',
    category: 'music',
    tags: ['音乐', '创作', '国产']
  },
  
  // 办公效率类
  {
    name: 'Notion AI',
    url: 'https://www.notion.so/product/ai',
    icon: '📋',
    description: 'Notion内置的AI助手',
    category: 'productivity',
    tags: ['笔记', '办公', '效率']
  },
  {
    name: 'Gamma',
    url: 'https://gamma.app',
    icon: '📊',
    description: 'AI驱动的演示文稿制作',
    category: 'productivity',
    tags: ['演示', 'PPT', '设计']
  },
  {
    name: 'Beautiful.AI',
    url: 'https://www.beautiful.ai',
    icon: '🎨',
    description: 'AI智能PPT设计工具',
    category: 'productivity',
    tags: ['PPT', '设计', '演示']
  },
  
  // 学术研究类
  {
    name: 'Consensus',
    url: 'https://consensus.app',
    icon: '📚',
    description: 'AI驱动的学术搜索引擎',
    category: 'research',
    tags: ['学术', '搜索', '研究']
  },
  {
    name: 'Semantic Scholar',
    url: 'https://www.semanticscholar.org',
    icon: '🔬',
    description: 'AI学术论文搜索平台',
    category: 'research',
    tags: ['论文', '学术', 'AI']
  },
  {
    name: 'Perplexity',
    url: 'https://www.perplexity.ai',
    icon: '🤔',
    description: 'AI搜索引擎和研究助手',
    category: 'research',
    tags: ['搜索', '研究', '问答']
  }
]

// 计算属性：过滤后的网站
const filteredSites = computed(() => {
  let sites = aiSites

  // 按分类过滤
  if (activeCategory.value !== 'all') {
    sites = sites.filter(site => site.category === activeCategory.value)
  }

  // 按搜索关键词过滤
  if (searchQuery.value.trim()) {
    const query = searchQuery.value.toLowerCase().trim()
    sites = sites.filter(site => 
      site.name.toLowerCase().includes(query) ||
      site.description.toLowerCase().includes(query) ||
      site.tags.some(tag => tag.toLowerCase().includes(query))
    )
  }

  return sites
})

// 获取分类下的工具数量
const getCategoryCount = (categoryKey) => {
  if (categoryKey === 'all') return aiSites.length
  return aiSites.filter(site => site.category === categoryKey).length
}

// 方法
const filterSites = () => {
  // 搜索时自动切换到全部分类
  if (searchQuery.value.trim()) {
    activeCategory.value = 'all'
  }
}

const openSite = (url) => {
  window.open(url, '_blank')
}

onMounted(() => {
  console.log('AI导航网站已加载')
})
</script>

<template>
  <div id="app">
    <!-- 头部 -->
    <header class="header">
      <div class="container">
        <div class="logo">
          <div class="logo-icon">🤖</div>
          <div class="logo-text">
            <h1>AI导航</h1>
            <p>发现最好的AI工具和服务</p>
          </div>
        </div>
        <div class="search-box">
          <div class="search-icon">🔍</div>
          <input 
            type="text" 
            v-model="searchQuery" 
            placeholder="搜索AI工具..." 
            @input="filterSites"
            class="search-input"
          />
          <div class="search-results-count" v-if="searchQuery">
            {{ filteredSites.length }} 个结果
          </div>
        </div>
      </div>
    </header>

    <!-- 主要内容 -->
    <main class="main">
      <div class="container">
        <!-- 统计信息 -->
        <div class="stats-section">
          <div class="stat-card">
            <div class="stat-number">{{ aiSites.length }}</div>
            <div class="stat-label">AI工具</div>
          </div>
          <div class="stat-card">
            <div class="stat-number">{{ categories.length - 1 }}</div>
            <div class="stat-label">分类</div>
          </div>
          <div class="stat-card">
            <div class="stat-number">{{ filteredSites.length }}</div>
            <div class="stat-label">当前显示</div>
          </div>
        </div>

        <!-- 分类导航 -->
        <nav class="category-nav">
          <button 
            v-for="category in categories" 
            :key="category.key"
            @click="activeCategory = category.key"
            :class="['category-btn', { active: activeCategory === category.key }]"
          >
            <span class="category-name">{{ category.name }}</span>
            <span class="category-count">{{ getCategoryCount(category.key) }}</span>
          </button>
        </nav>

        <!-- AI网站列表 -->
        <div class="sites-section">
          <div class="section-header">
            <h2>{{ activeCategory === 'all' ? '全部工具' : categories.find(c => c.key === activeCategory)?.name }}</h2>
            <div class="section-subtitle">{{ filteredSites.length }} 个优质AI工具</div>
          </div>
          
          <div class="sites-grid">
            <div 
              v-for="(site, index) in filteredSites" 
              :key="site.name"
              class="site-card"
              @click="openSite(site.url)"
              :style="{ '--delay': index * 0.1 + 's' }"
            >
              <div class="site-header">
                <div class="site-icon">{{ site.icon }}</div>
                <div class="site-status">🔥</div>
              </div>
              <div class="site-content">
                <h3 class="site-name">{{ site.name }}</h3>
                <p class="site-desc">{{ site.description }}</p>
                <div class="site-tags">
                  <span v-for="tag in site.tags" :key="tag" class="tag">{{ tag }}</span>
                </div>
              </div>
              <div class="site-footer">
                <button class="visit-btn">访问 →</button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </main>

    <!-- 底部 -->
    <footer class="footer">
      <div class="container">
        <div class="footer-content">
          <div class="footer-info">
            <div class="footer-logo">🤖 AI导航</div>
            <p>&copy; 2024 AI导航. 发现AI的无限可能</p>
          </div>
          <div class="footer-links">
            <a href="#" class="footer-link">关于我们</a>
            <a href="#" class="footer-link">联系方式</a>
            <a href="#" class="footer-link">提交工具</a>
          </div>
        </div>
      </div>
    </footer>
  </div>
</template>

<style scoped>
/* CSS变量定义 */
:root {
  --primary-gradient: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  --secondary-gradient: linear-gradient(135deg, #f093fb 0%, #f5576c 100%);
  --accent-gradient: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%);
  --success-gradient: linear-gradient(135deg, #43e97b 0%, #38f9d7 100%);
  --card-shadow: 0 10px 40px rgba(0, 0, 0, 0.1);
  --card-shadow-hover: 0 20px 60px rgba(0, 0, 0, 0.15);
  --border-radius: 20px;
  --transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}

/* 全局样式重置 */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

#app {
  min-height: 100vh;
  background: var(--primary-gradient);
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Roboto', sans-serif;
  position: relative;
}

/* 添加背景装饰 */
#app::before {
  content: '';
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: 
    radial-gradient(circle at 20% 80%, rgba(120, 119, 198, 0.3) 0%, transparent 50%),
    radial-gradient(circle at 80% 20%, rgba(255, 119, 198, 0.3) 0%, transparent 50%),
    radial-gradient(circle at 40% 40%, rgba(120, 219, 255, 0.2) 0%, transparent 50%);
  pointer-events: none;
  z-index: -1;
}

.container {
  max-width: 1400px;
  margin: 0 auto;
  padding: 0 24px;
}

/* 头部样式 */
.header {
  background: rgba(255, 255, 255, 0.15);
  backdrop-filter: blur(20px);
  border-bottom: 1px solid rgba(255, 255, 255, 0.2);
  padding: 24px 0;
  position: sticky;
  top: 0;
  z-index: 100;
}

.header .container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
  gap: 24px;
}

.logo {
  display: flex;
  align-items: center;
  gap: 16px;
}

.logo-icon {
  font-size: 3rem;
  filter: drop-shadow(0 4px 8px rgba(0, 0, 0, 0.2));
  animation: bounce 2s infinite;
}

@keyframes bounce {
  0%, 20%, 50%, 80%, 100% { transform: translateY(0); }
  40% { transform: translateY(-10px); }
  60% { transform: translateY(-5px); }
}

.logo-text h1 {
  color: white;
  font-size: 2.5rem;
  font-weight: 800;
  background: linear-gradient(45deg, #fff, #f0f8ff);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  margin-bottom: 4px;
}

.logo-text p {
  color: rgba(255, 255, 255, 0.9);
  font-size: 1rem;
  font-weight: 500;
}

.search-box {
  position: relative;
  display: flex;
  align-items: center;
  background: rgba(255, 255, 255, 0.95);
  border-radius: 50px;
  padding: 6px 24px 6px 50px;
  min-width: 350px;
  box-shadow: var(--card-shadow);
  transition: var(--transition);
}

.search-box:focus-within {
  transform: translateY(-2px);
  box-shadow: var(--card-shadow-hover);
}

.search-icon {
  position: absolute;
  left: 18px;
  font-size: 18px;
  color: #666;
}

.search-input {
  flex: 1;
  border: none;
  outline: none;
  padding: 14px 0;
  font-size: 16px;
  background: transparent;
  color: #333;
}

.search-input::placeholder {
  color: #999;
}

.search-results-count {
  background: var(--accent-gradient);
  color: white;
  padding: 4px 12px;
  border-radius: 20px;
  font-size: 12px;
  font-weight: 600;
  white-space: nowrap;
}

/* 主要内容样式 */
.main {
  padding: 48px 0;
}

/* 统计信息 */
.stats-section {
  display: flex;
  gap: 24px;
  margin-bottom: 48px;
  justify-content: center;
}

.stat-card {
  background: rgba(255, 255, 255, 0.2);
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.3);
  border-radius: var(--border-radius);
  padding: 24px 32px;
  text-align: center;
  transition: var(--transition);
}

.stat-card:hover {
  transform: translateY(-4px);
  background: rgba(255, 255, 255, 0.25);
}

.stat-number {
  font-size: 2.5rem;
  font-weight: 800;
  color: white;
  margin-bottom: 8px;
  background: linear-gradient(45deg, #fff, #f0f8ff);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

.stat-label {
  color: rgba(255, 255, 255, 0.9);
  font-weight: 600;
  font-size: 0.9rem;
}

/* 分类导航样式 */
.category-nav {
  display: flex;
  flex-wrap: wrap;
  gap: 12px;
  margin-bottom: 48px;
  justify-content: center;
}

.category-btn {
  display: flex;
  align-items: center;
  gap: 8px;
  background: rgba(255, 255, 255, 0.9);
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.5);
  border-radius: 50px;
  padding: 12px 24px;
  font-size: 14px;
  font-weight: 600;
  cursor: pointer;
  transition: var(--transition);
  color: #333;
}

.category-btn:hover {
  background: rgba(255, 255, 255, 0.95);
  color: #222;
  transform: translateY(-2px);
  box-shadow: 0 8px 25px rgba(0, 0, 0, 0.15);
}

.category-btn.active {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  transform: translateY(-2px);
  box-shadow: 0 8px 25px rgba(0, 0, 0, 0.2);
}

.category-count {
  background: var(--accent-gradient);
  color: white;
  padding: 2px 8px;
  border-radius: 12px;
  font-size: 11px;
  font-weight: 700;
}

.category-btn.active .category-count {
  background: var(--secondary-gradient);
}

/* 网站区域 */
.sites-section {
  margin-bottom: 48px;
}

.section-header {
  text-align: center;
  margin-bottom: 40px;
}

.section-header h2 {
  font-size: 2.5rem;
  font-weight: 800;
  color: white;
  margin-bottom: 8px;
  background: linear-gradient(45deg, #fff, #f0f8ff);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

.section-subtitle {
  color: rgba(255, 255, 255, 0.8);
  font-size: 1.1rem;
  font-weight: 500;
}

/* 网站卡片网格 */
.sites-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(320px, 1fr));
  gap: 32px;
  margin-top: 40px;
}

/* 网站卡片样式 */
.site-card {
  background: rgba(255, 255, 255, 0.95);
  border-radius: var(--border-radius);
  padding: 0;
  cursor: pointer;
  transition: var(--transition);
  box-shadow: var(--card-shadow);
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.2);
  overflow: hidden;
  animation: fadeInUp 0.6s ease-out forwards;
  animation-delay: var(--delay);
  opacity: 0;
  transform: translateY(30px);
}

@keyframes fadeInUp {
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.site-card:hover {
  transform: translateY(-12px);
  box-shadow: var(--card-shadow-hover);
  background: white;
}

/* 移动端触摸反馈 */
@media (hover: none) and (pointer: coarse) {
  .site-card:active {
    transform: scale(0.98);
    transition: transform 0.1s ease;
  }
  
  .site-card:hover {
    transform: none;
    background: rgba(255, 255, 255, 0.95);
  }
  
  .visit-btn:active {
    transform: scale(0.96);
    transition: transform 0.1s ease;
  }
  
  .category-btn:active {
    transform: scale(0.95);
    transition: transform 0.1s ease;
  }
}

.site-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 24px 24px 16px;
}

.site-icon {
  font-size: 3.5rem;
  filter: drop-shadow(0 2px 4px rgba(0, 0, 0, 0.1));
}

.site-status {
  background: var(--secondary-gradient);
  color: white;
  padding: 4px 8px;
  border-radius: 12px;
  font-size: 12px;
}

.site-content {
  padding: 0 24px 20px;
}

.site-name {
  font-size: 1.5rem;
  font-weight: 700;
  color: #2d3748;
  margin-bottom: 12px;
}

.site-desc {
  color: #718096;
  font-size: 0.95rem;
  line-height: 1.6;
  margin-bottom: 16px;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

.site-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
}

.tag {
  background: var(--accent-gradient);
  color: white;
  padding: 4px 12px;
  border-radius: 16px;
  font-size: 0.75rem;
  font-weight: 600;
}

.site-footer {
  padding: 0 24px 24px;
}

.visit-btn {
  width: 100%;
  background: var(--primary-gradient);
  color: white;
  border: none;
  padding: 12px;
  border-radius: 12px;
  font-weight: 600;
  font-size: 0.9rem;
  cursor: pointer;
  transition: var(--transition);
}

.visit-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 8px 20px rgba(102, 126, 234, 0.3);
}

/* 底部样式 */
.footer {
  background: rgba(0, 0, 0, 0.3);
  backdrop-filter: blur(10px);
  border-top: 1px solid rgba(255, 255, 255, 0.1);
  padding: 48px 0;
  margin-top: 80px;
}

.footer-content {
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
  gap: 24px;
}

.footer-logo {
  font-size: 1.5rem;
  font-weight: 800;
  color: white;
  margin-bottom: 8px;
}

.footer-info p {
  color: rgba(255, 255, 255, 0.7);
  font-size: 0.9rem;
}

.footer-links {
  display: flex;
  gap: 32px;
}

.footer-link {
  color: rgba(255, 255, 255, 0.8);
  text-decoration: none;
  font-weight: 500;
  transition: var(--transition);
}

.footer-link:hover {
  color: white;
  text-decoration: none;
}

/* 响应式设计 */
@media (max-width: 1024px) {
  .sites-grid {
    grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
    gap: 24px;
  }
  
  .stats-section {
    flex-wrap: wrap;
    gap: 16px;
  }
  
  .stat-card {
    min-width: 120px;
  }
}

@media (max-width: 768px) {
  .header {
    padding: 20px 0 30px;
  }
  
  .header .container {
    flex-direction: column;
    text-align: center;
    gap: 24px;
  }
  
  .logo {
    margin-bottom: 8px;
  }
  
  .search-box {
    min-width: 280px;
    width: 100%;
    max-width: 400px;
    padding: 14px 20px 14px 50px;
    font-size: 16px; /* 防止iOS缩放 */
  }
  
  .search-icon {
    left: 18px;
    font-size: 1.1rem;
  }
  
  .sites-grid {
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    gap: 20px;
  }
  
  .site-card {
    padding: 24px;
    min-height: 200px;
  }
  
  .category-nav {
    gap: 8px;
    overflow-x: auto;
    padding: 0 20px 20px;
    margin: 0 -20px;
    -webkit-overflow-scrolling: touch;
    scrollbar-width: none;
    -ms-overflow-style: none;
  }
  
  .category-nav::-webkit-scrollbar {
    display: none;
  }
  
  .category-btn {
    padding: 12px 20px;
    font-size: 14px;
    white-space: nowrap;
    min-width: fit-content;
    border-radius: 25px;
  }
  
  .category-btn:first-child {
    margin-left: 20px;
  }
  
  .category-btn:last-child {
    margin-right: 20px;
  }
  
  .logo-text h1 {
    font-size: 2.2rem;
    margin-bottom: 8px;
  }
  
  .logo-text p {
    font-size: 1rem;
    opacity: 0.9;
  }
  
  .section-header h2 {
    font-size: 2rem;
    margin-bottom: 8px;
  }
  
  .section-subtitle {
    font-size: 1rem;
  }
  
  .stats-section {
    flex-direction: row;
    justify-content: center;
    flex-wrap: wrap;
    gap: 16px;
    margin-bottom: 40px;
  }
  
  .stat-card {
    flex: 1;
    min-width: 100px;
    max-width: 120px;
    padding: 20px 16px;
  }
  
  .footer-content {
    flex-direction: column;
    text-align: center;
    gap: 24px;
  }
  
  .footer-links {
    justify-content: center;
    flex-wrap: wrap;
    gap: 24px;
  }
  
  .main {
    padding: 40px 0;
  }
}

@media (max-width: 480px) {
  .container {
    padding: 0 20px;
  }
  
  .header {
    padding: 16px 0 24px;
  }
  
  .main {
    padding: 24px 0;
  }
  
  .sites-grid {
    grid-template-columns: 1fr;
    gap: 16px;
  }
  
  .site-card {
    margin: 0;
    padding: 20px;
    min-height: 180px;
  }
  
  .site-icon {
    font-size: 2rem;
  }
  
  .site-name {
    font-size: 1.2rem;
  }
  
  .site-desc {
    font-size: 0.9rem;
    line-height: 1.5;
  }
  
  .search-box {
    min-width: 280px;
    max-width: 100%;
    padding: 12px 16px 12px 45px;
    font-size: 16px; /* 防止iOS缩放 */
  }
  
  .search-icon {
    left: 16px;
  }
  
  .logo {
    flex-direction: column;
    gap: 12px;
  }
  
  .logo-icon {
    font-size: 2.8rem;
  }
  
  .logo-text h1 {
    font-size: 2rem;
  }
  
  .logo-text p {
    font-size: 0.95rem;
  }
  
  .category-nav {
    padding: 0 20px 16px;
    gap: 12px;
  }
  
  .category-btn {
    padding: 10px 16px;
    font-size: 13px;
  }
  
  .section-header {
    text-align: center;
    margin-bottom: 24px;
  }
  
  .section-header h2 {
    font-size: 1.8rem;
  }
  
  .stats-section {
    margin-bottom: 32px;
  }
  
  .stat-card {
    padding: 16px 12px;
    min-width: 85px;
  }
  
  .stat-number {
    font-size: 1.8rem;
  }
  
  .stat-label {
    font-size: 0.8rem;
  }
  
  .visit-btn {
    padding: 14px;
    font-size: 0.95rem;
    border-radius: 10px;
  }
  
  .tag {
    padding: 6px 10px;
    font-size: 0.7rem;
    border-radius: 12px;
  }
  
  .footer {
    padding: 32px 0;
    margin-top: 60px;
  }
  
  .footer-links {
    gap: 16px;
  }
  
  .footer-link {
    font-size: 0.9rem;
  }
}

/* 滚动条样式 */
::-webkit-scrollbar {
  width: 8px;
}

::-webkit-scrollbar-track {
  background: rgba(255, 255, 255, 0.1);
  border-radius: 4px;
}

::-webkit-scrollbar-thumb {
  background: rgba(255, 255, 255, 0.3);
  border-radius: 4px;
  transition: var(--transition);
}

::-webkit-scrollbar-thumb:hover {
  background: rgba(255, 255, 255, 0.5);
}

/* 移动端专用样式优化 */
@media (max-width: 768px) {
  /* 优化头部在移动端的显示 */
  .logo-icon {
    animation: bounce 2s ease-in-out infinite;
  }
  
  @keyframes bounce {
    0%, 20%, 50%, 80%, 100% {
      transform: translateY(0);
    }
    40% {
      transform: translateY(-10px);
    }
    60% {
      transform: translateY(-5px);
    }
  }
  
  /* 移动端卡片阴影调整 */
  .site-card {
    box-shadow: 0 4px 20px rgba(0, 0, 0, 0.08);
  }
  
  /* 移动端按钮优化 */
  .visit-btn {
    font-weight: 700;
    letter-spacing: 0.5px;
    text-transform: uppercase;
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    box-shadow: 0 4px 15px rgba(102, 126, 234, 0.2);
  }
  
  /* 移动端分类按钮文字增强 */
  .category-btn {
    background: rgba(255, 255, 255, 0.9);
    color: #333;
    font-weight: 600;
    border: 1px solid rgba(255, 255, 255, 0.5);
    text-shadow: none;
  }
  
  .category-btn:hover {
    background: rgba(255, 255, 255, 0.95);
    color: #222;
  }
  
  /* 移动端标签样式优化 */
  .tag {
    font-weight: 700;
    text-transform: uppercase;
    letter-spacing: 0.3px;
  }
  
  /* 移动端搜索框样式增强 */
  .search-box {
    box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
    border: 2px solid rgba(255, 255, 255, 0.3);
  }
  
  .search-box:focus-within {
    border-color: rgba(255, 255, 255, 0.6);
    box-shadow: 0 12px 40px rgba(0, 0, 0, 0.15);
  }
}

@media (max-width: 480px) {
  /* 超小屏幕的额外优化 */
  .site-card {
    border-radius: 16px;
    box-shadow: 0 2px 16px rgba(0, 0, 0, 0.06);
  }
  
  .header {
    background: linear-gradient(135deg, rgba(102, 126, 234, 0.1) 0%, rgba(118, 75, 162, 0.1) 100%);
    border-radius: 0 0 24px 24px;
    margin-bottom: 16px;
  }
  
  /* 移动端网格间距优化 */
  .sites-grid {
    padding: 0 4px;
  }
  
  /* 分类导航增强 */
  .category-nav {
    background: rgba(255, 255, 255, 0.05);
    backdrop-filter: blur(10px);
    border-radius: 20px;
    margin: 0 -16px 24px -16px;
    padding: 16px 20px;
    position: relative;
  }
  
  /* 移动端未选中按钮样式 */
  .category-btn {
    background: rgba(255, 255, 255, 0.85);
    color: #333;
    border: 1px solid rgba(255, 255, 255, 0.6);
    font-weight: 600;
    text-shadow: none;
  }
  
  .category-btn:hover {
    background: rgba(255, 255, 255, 0.95);
    color: #222;
    text-shadow: none;
  }
  
  .category-btn.active {
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    color: white;
    font-weight: 700;
    box-shadow: 0 4px 12px rgba(102, 126, 234, 0.3);
    border: 1px solid transparent;
    text-shadow: none;
  }
  
  /* 移动端分类数量标签优化 */
  .category-btn .category-count {
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    color: white;
    font-weight: 700;
    border: none;
  }
  
  .category-btn.active .category-count {
    background: rgba(255, 255, 255, 0.25);
    color: white;
    border: none;
  }
  
  /* 移动端滚动指示器 */
  .category-nav::after {
    content: '';
    position: absolute;
    right: 0;
    top: 50%;
    transform: translateY(-50%);
    width: 20px;
    height: 100%;
    background: linear-gradient(to left, rgba(255, 255, 255, 0.1), transparent);
    pointer-events: none;
    border-radius: 0 20px 20px 0;
  }
  
  /* 移动端更好的卡片交互 */
  .site-card {
    will-change: transform;
  }
  
  /* 移动端文字优化 */
  .site-name {
    letter-spacing: -0.02em;
  }
  
  .site-desc {
    color: #4a5568;
    font-weight: 400;
  }
}

/* 加载动画 */
.loading {
  display: inline-block;
  animation: spin 1s linear infinite;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}
</style>
