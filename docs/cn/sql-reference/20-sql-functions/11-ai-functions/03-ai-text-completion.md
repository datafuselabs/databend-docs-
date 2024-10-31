---
title: 'AI_TEXT_COMPLETION'
description: '在 Databend 中使用 ai_text_completion 函数生成文本补全'
---

本文档概述了 Databend 中的 `ai_text_completion` 函数，并演示了如何使用此函数生成文本补全。

主要代码实现可以在[这里](https://github.com/datafuselabs/databend/blob/1e93c5b562bd159ecb0f336bb88fd1b7f9dc4a62/src/common/openai/src/completion.rs)找到。

:::info
从 Databend v1.1.47 开始，Databend 支持 [Azure OpenAI 服务](https://azure.microsoft.com/en-au/products/cognitive-services/openai-service)。

此集成提供了改进的数据隐私。

要使用 Azure OpenAI，请将以下配置添加到 `[query]` 部分：
```sql
# Azure OpenAI
openai_api_chat_base_url = "https://<name>.openai.azure.com/openai/deployments/<name>/"
openai_api_embedding_base_url = "https://<name>.openai.azure.com/openai/deployments/<name>/"
openai_api_version = "2023-03-15-preview"
```
:::

:::caution
Databend 依赖于 (Azure) OpenAI 进行 `AI_TEXT_COMPLETION`，并将补全提示数据发送到 (Azure) OpenAI。

它们仅在 Databend 配置包含 `openai_api_key` 时才会工作，否则它们将处于非活动状态。

此功能默认在 [Databend Cloud](https://databend.com) 上使用我们的 Azure OpenAI 密钥。如果您使用它们，您承认您的数据将由我们发送给 Azure OpenAI。
:::


## ai_text_completion 概述

Databend 中的 `ai_text_completion` 函数是一个内置函数，它根据给定的提示生成文本补全。它对于自然语言处理任务非常有用，例如问答、文本生成和自动补全系统。

该函数接受一个文本提示作为输入，并返回该提示的生成补全。补全使用在大规模文本语料库上预训练的模型创建，捕捉单词和短语之间的连续空间关系。

## 使用 ai_text_completion 生成文本补全

以下是一个使用 Databend 中的 `ai_text_completion` 函数生成文本补全的简单示例：
```sql
SELECT ai_text_completion('What is artificial intelligence?') AS completion;
```

结果：
```sql
+--------------------------------------------------------------------------------------------------------------------+
| completion                                                                                                          |
+--------------------------------------------------------------------------------------------------------------------+
| Artificial intelligence (AI) is the field of study focused on creating machines and software capable of thinking, learning, and solving problems in a way that mimics human intelligence. This includes areas such as machine learning, natural language processing, computer vision, and robotics. |
+--------------------------------------------------------------------------------------------------------------------+
```

在这个示例中，我们将提示 "What is artificial intelligence?" 提供给 `ai_text_completion` 函数，它返回一个生成的补全，简要描述了人工智能。