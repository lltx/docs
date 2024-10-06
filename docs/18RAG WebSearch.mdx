---
title: langchain4j rag 增强搜索
---

[SearchApi](https://www.searchapi.io/) 是一个实时搜索引擎结果页面（SERP）API。你可以使用它进行 Google、Google News、Bing、Bing News、Baidu、Google Scholar 等搜索引擎的查询，获取有机搜索结果。

## 使用方法
### 依赖配置
在你的项目 `pom.xml` 中添加以下依赖：

```xml
<dependency>
  <groupId>com.pig4cloud.ai</groupId>
  <artifactId>langchain4j-web-search-engine-searchapi</artifactId>
  <version>0.35.0</version>
</dependency>

```

### 示例代码
```java
package com.pig4cloud.ai.search;


/**
 * SearchApiTool 示例类
 * 使用 SearchApi 执行网络搜索并生成自然语言响应
 * 作者: lengleng
 */
public class SearchApiTool {

    // 定义一个接口，用于 AI 助理与用户的互动
    interface Assistant {
        @SystemMessage({
                "你是一名网络搜索支持助理。",
                "如果有任何尚未发生的事件，",
                "你必须根据用户的查询创建网络搜索请求，",
                "并使用网络搜索工具获取有机搜索结果。",
                "请务必在最终的回答中包含来源链接。"
        })
        String answer(String userMessage);
    }

    // API 密钥配置
    private static final String SEARCHAPI_API_KEY = "YOUR_SEARCHAPI_KEY";
    private static final String OPENAI_API_KEY = "YOUR_OPENAI_KEY";

    public static void main(String[] args) {
        // 可选参数配置，例如指定语言和地区
        Map<String, Object> optionalParameters = new HashMap<>();
        optionalParameters.put("gl", "us");
        optionalParameters.put("hl", "en");
        optionalParameters.put("google_domain", "google.com");

        // 初始化搜索引擎
        SearchApiWebSearchEngine searchEngine = SearchApiWebSearchEngine.builder()
                .apiKey(SEARCHAPI_API_KEY)
                .engine("google")
                .optionalParameters(optionalParameters)
                .build();

        // 初始化语言模型
        ChatLanguageModel chatModel = OpenAiChatModel.builder()
                .apiKey(OPENAI_API_KEY)
                .modelName(OpenAiChatModelName.GPT_3_5_TURBO)
                .logRequests(true)
                .build();

        // 创建搜索工具
        WebSearchTool webTool = WebSearchTool.from(searchEngine);

        // 创建 AI 助理服务，结合聊天模型和搜索工具
        Assistant assistant = AiServices.builder(Assistant.class)
                .chatLanguageModel(chatModel)
                .tools(webTool)
                .build();

        // 执行搜索并获取答案
        String answer = assistant.answer("我的家人下周要来马德里，列出适合全家参与的最佳旅游活动");
        System.out.println(answer);

        /*
            输出示例：
            1. **Parque del Retiro** - 适合家庭享受大自然的美丽公园。
            2. **Prado Museum** - 著名的艺术博物馆，适合所有年龄段的人参观。
            3. **Mercado de San Miguel** - 可以探索和品尝美味西班牙食品的市场。
            4. **Royal Palace** - 探索马德里皇家宫殿的壮丽景色。
            5. **Plaza Mayor** 和 **Puerta del Sol** - 历史悠久的广场，充满活力的氛围。
            6. **Santiago Bernabeu Stadium** - 适合体育爱好者和足球迷的圣地。
            7. **Gran Via** - 著名的购物和娱乐街道，适合观光。
            8. **National Archaeological Museum** - 通过考古文物了解西班牙丰富的历史。
            9. **Templo de Debod** - 位于马德里市中心的古埃及神庙。
        */
    }
}
```

### Langchain4j 支持的搜索引擎
| SearchApi 引擎 | 是否支持 |
| --- | --- |
| [Google Web Search](https://www.searchapi.io/docs/google) | ✅ |
| [Google News](https://www.searchapi.io/docs/google-news) | ✅ |
| [Bing](https://www.searchapi.io/docs/bing) | ✅ |
| [Bing News](https://www.searchapi.io/docs/bing-news) | ✅ |
| [Baidu](https://www.searchapi.io/docs/baidu) | ✅ |


任何返回 `organic_results` 数组的搜索引擎都可以被此库支持，即使不在上述列表中，只要有 `title`、`link` 和 `snippet` 字段的有机搜索结果。



