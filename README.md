# 法律 RAG 知识库助手

这个项目是一个面向法律问答场景的本地 RAG 知识库助手，提供 Streamlit 图形界面、命令行入口、离线索引构建和带引用的答案生成能力。它主要服务于法律文本检索、法律条文问答、案例/法规辅助定位等任务。

## 这个项目能做什么

- 支持从本地法律文档中构建知识库并进行检索问答
- 支持 `llm_retrieval` 和 `hybrid` 两种问答模式
- 支持带来源引用的回答输出
- 支持历史会话保存、回看和删除
- 支持在界面中重建索引
- 支持基于本地缓存模型离线运行

## 目录结构

- `app.py`：Streamlit 主界面入口
- `query_legal_rag.py`：命令行问答入口
- `start_rag_app.bat`：Windows 一键启动脚本
- `legal_agent/`：核心检索、记忆、存储和工作流实现
- `requirements.txt`：一键安装依赖清单
- `pdf_data/`：法律 PDF 数据源
- `raw_data/`：原始文档数据源
- `legal_agent_runtime/`：运行时索引与数据库文件
- `docs/`：项目架构和说明文档

## 安装步骤

以下流程适合第一次部署这个项目的情况。

### 1. 准备 Python 环境

建议使用 Python 3.10 或 3.11，并在项目目录下创建虚拟环境：

```bash
python -m venv .venv
```

Windows 激活虚拟环境：

```bash
.venv\Scripts\activate
```

### 2. 安装依赖

```bash
pip install -r requirements.txt
```

如果后续要使用 OCR 功能，通常还需要额外安装系统组件：

- `Tesseract OCR`
- `Poppler`

### 3. 配置模型与密钥

这个项目使用 `config.ini` 保存大模型连接信息。公开仓库中已经将密钥占位处理为：

```ini
api_key = 你的api密钥
```

部署时需要把它替换成你自己的真实密钥，或者改用环境变量配置。

`legal_agent/config.py` 也支持从环境变量读取配置，常用字段如下：

- `RAG_LLM_BASE_URL`
- `RAG_LLM_API_KEY`
- `RAG_LLM_MODEL`
- `RAG_RETRIEVAL_MODE`
- `RAG_ANSWER_PROFILE`

### 4. 预下载本地模型

这个项目支持本地缓存模型离线运行。首次部署时可以先执行：

```bash
python download_local_models.py
```

### 5. 启动应用

#### 方式一：启动图形界面

双击 `start_rag_app.bat`，或者在项目目录下执行：

```bash
streamlit run app.py
```

#### 方式二：命令行问答

```bash
python query_legal_rag.py
```

## 数据与索引

这个项目默认会从以下目录收集知识源：

- `pdf_data/`
- `raw_data/`

运行时索引与数据库会写入：

- `legal_agent_runtime/rag_external.db`
- `legal_agent_runtime/legal_chunks.faiss`
- `legal_agent_runtime/legal_chunks_tfidf.pkl`
- `legal_agent_runtime/manifest.json`

如果要重新构建知识库，可以在界面侧边栏点击“重建索引”，系统会重新解析本地数据并刷新索引。

## 主要依赖

这个项目主要依赖以下第三方库：

- `streamlit`
- `pymupdf`
- `sentence-transformers`
- `faiss-cpu`
- `numpy`
- `scikit-learn`
- `pillow`
- `opencv-python`
- `pytesseract`
- `pdf2image`
- `pdfplumber`
- `huggingface_hub`
- `openai`
- `langchain-core`
- `langgraph`
- `PySide6`

## 设计特点

- 优先使用本地文件和本地索引，不依赖猜测式回答
- 检索和回答过程强调证据可追溯
- 对缺失配置和缺失资源采用明确报错，便于定位问题
- 支持长对话中的分层记忆与历史回看

## 相关文档

- [架构说明](docs/architecture.md)
- [法律 RAG 知识库助手说明](docs/legal_rag_agent_formal.md)
- [内部说明](docs/legal_rag_agent_internal.md)
