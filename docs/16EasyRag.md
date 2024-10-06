---
title: langchain4j easy-rag
---

在构建检索增强生成（RAG，Retrieval-Augmented Generation）应用时，LangChain4j 提供了一个名为 "Easy RAG" 的功能模块，简化了复杂 RAG 应用的开发。本文将为你详细介绍如何利用 LangChain4j 实现一个简易 RAG 应用，并深入解析核心 RAG API 的使用和场景。

## LangChain4j 内置 Easy RAG
LangChain4j 的 Easy RAG 功能设计旨在帮助开发者快速上手 RAG 应用，隐藏了嵌入、向量存储、嵌入模型等复杂技术细节。你只需提供文档路径，框架会自动完成文档加载、解析、拆分、向量化等处理流程。

### Easy RAG 使用步骤
#### 第一步：引入依赖
在你的项目中添加以下依赖项：

```xml
<dependency>
    <groupId>dev.langchain4j</groupId>
    <artifactId>langchain4j-easy-rag</artifactId>
    <version>0.35.0</version>
</dependency>
```

#### 第二步：加载文档内容
通过自定义服务加载文件系统中的文档：

```java
package com.pig4cloud.ai.xxx;

import java.util.List;

public class LoaderService {

    /**
     * 从文件系统加载文档内容
     * 
     * @param path 文档路径
     * @return 文档列表
     */
    public List<Document> loadFromFileSystem(String path) {
        return FileSystemDocumentLoader.loadDocuments(path);
    }
}
```

`FileSystemDocumentLoader` 使用 `ApacheTikaDocumentParser` 来解析各种格式的文档，你无需担心文档类型的细节。

#### 第三步：文档向量化处理
向量化是将文本转化为可用于语义搜索的嵌入向量：

```java
package com.pig4cloud.ai.xxx;

import java.util.List;

public class EmbeddingService {

    public void embed(List<Document> documents) {
        InMemoryEmbeddingStore<TextSegment> embeddingStore = new InMemoryEmbeddingStore<>();
        EmbeddingStoreIngestor.ingest(documents, embeddingStore);
    }
}
```

#### 第四步：实现 Chat 服务
定义一个接口，用于处理用户的查询请求：

```java
package com.pig4cloud.ai.xxx;

public interface Assistant {

    String chat(String userMessage);
}
```

#### 第五步：创建控制器
控制器负责处理客户端的请求，调用相关服务完成向量化和聊天功能：

```java
package com.pig4cloud.ai.xxx;

import lombok.RequiredArgsConstructor;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.List;

@RestController
@RequiredArgsConstructor
public class EasyRagController {

    private final OpenAiChatModel openAiChatModel;
    private final EmbeddingStore<TextSegment> embeddingStore;
    private final EmbeddingService embeddingService;
    private final LoaderService loaderService;

    @GetMapping("/chat")
    public String chat(String message) {
        Assistant assistant = AiServices.builder(Assistant.class)
                .chatLanguageModel(openAiChatModel)
                .chatMemory(MessageWindowChatMemory.withMaxMessages(10))
                .contentRetriever(EmbeddingStoreContentRetriever.from(embeddingStore))
                .build();
        return assistant.chat(message);
    }

    @GetMapping("/embedded")
    public void embedded() {
        List<Document> documents = loaderService.loadFromFileSystem("文件路径");
        embeddingService.embed(documents);
    }
}
```

### 测试示例
1. 将文档向量化存储，测试路径内容为一本阿里开发手册：

```http
GET http://localhost:8888/embedded
```

2. 提交对手册的询问：

```http
GET http://localhost:8888/chat?message=Java开发手册当前是什么版本？
```

通过上述步骤可以初步实现 RAG 应用，效果可通过进一步调优提升。

## LangChain4j RAG API 详解
LangChain4j 提供了一系列核心 API 来构建和管理 RAG 应用，下面详细介绍几个重要的组件。

### 1. Document 「文档」
`Document` 表示整个文档实体（如 PDF、网页内容）。当前仅支持文本格式，未来将支持图片、表格等内容。

#### 核心方法：
+ `Document.text()`：返回文档的文本内容。
+ `Document.metadata()`：获取文档的元数据。
+ `Document.toTextSegment()`：将文档转换为 `TextSegment`。
+ `Document.from(String, Metadata)`：根据文本和元数据创建文档实例。

### 2. Metadata 「元数据」
每个文档都附带元数据，用于存储相关的文档信息，如名称、来源、更新时间等。

#### 核心方法：
+ `Metadata.from(Map)`：从指定 Map 创建元数据。
+ `Metadata.put(String key, String value)`：向元数据添加条目。
+ `Metadata.toMap()`：将元数据转换为 Map 对象。

### 3. Embedding 「嵌入」
嵌入是将高维度数据转换为低维向量，以便进行语义表达和相似度计算。

#### 核心方法：
+ `Embedding.dimension()`：返回嵌入向量的维度。
+ `CosineSimilarity.between(Embedding, Embedding)`：计算嵌入之间的余弦相似度。

### 4. Embedding Store 「向量存储」
`EmbeddingStore` 允许存储和检索嵌入数据，用于高效的相似性搜索。

#### 核心方法：
+ `EmbeddingStore.add(Embedding)`：将嵌入对象添加到存储中。
+ `EmbeddingStore.search(EmbeddingSearchRequest)`：进行相似性搜索。

## 总结
本文介绍了 LangChain4j 的 Easy RAG 功能，并展示了如何快速构建一个基于 RAG 的简易应用。此外，我们深入解析了 RAG API 的核心方法，为开发更复杂的 RAG 系统提供了基础知识。

通过本示例，你可以轻松构建并扩展 RAG 应用，借助 LangChain4j 提供的强大功能，提升开发效率。

