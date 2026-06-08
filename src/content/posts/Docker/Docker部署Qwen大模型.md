---
title: Docker 部署 Qwen 大模型
published: 2026-01-12
image: api
category: 教程
tags: [AI, Docker, 大模型]
---

# Docker 部署 Qwen 大模型

## 镜像

作者的 GitHub 仓库删了,但是 DockerHub 的镜像没删

```bash
docker pull ollama/ollama
```

## 下载 Qwen 的 GGUF 模型

```yaml
# https://huggingface.co/Qwen/Qwen2.5-0.5B-Instruct-GGUF
# https://modelscope.cn/models/qwen/Qwen2.5-0.5B-Instruct-GGUF
# https://ollama.com/library/qwen2.5
```

## 编写 Modelfile 文件

```dockerfile
# 注意GGUF模型文件的地址要与Dockerfile中保持一致
FROM /tmp/qwen2.5-0.5b-instruct-q2_k.gguf
TEMPLATE "{{ if .System }}<|im_start|>system
{{ .System }}<|im_end|>
{{ end }}{{ if .Prompt }}<|im_start|>user
{{ .Prompt }}<|im_end|>
{{ end }}<|im_start|>assistant
{{ .Response }}<|im_end|>
"
PARAMETER stop <|im_start|>
PARAMETER stop <|im_end|>
```

## 编写 Dockerfile 文件

```dockerfile
FROM ollama/ollama
EXPOSE 11434

ADD Modelfile /tmp/Modelfile
ADD qwen2.5-0.5b-instruct-q2_k.gguf /tmp/qwen2.5-0.5b-instruct-q2_k.gguf

ENTRYPOINT ["sh","-c","/bin/ollama serve"]
```

## 构建与运行容器

```bash
docker build -t ollama_qwen2.5_0.5b:1.0 -f Dockerfile .

docker run -itd --name ollama_qwen -p 11434:11434 ollama_qwen2.5_0.5b
```

## 加载 Qwen 模型

```bash
# 加载
ollama create qwen:0.5b -f /tmp/Modelfile

# 运行
ollama run qwen:0.5b
```

终端出现 `>>`，开启和 Ollama 的对话旅程吧~

## API 服务

```bash
curl http://localhost:11434/api/generate -d '{
  "model": "llama3.2",
  "prompt": "Why is the sky blue?",
  "stream": false
}'
```

更多参数和使用，可参考 API 文档：https://github.com/ollama/ollama/blob/main/docs/api.md
