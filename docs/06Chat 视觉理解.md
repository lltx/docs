---
title: langchain4j 视觉理解
---

以下是关于使用LangChain4j和Dashscope进行视觉理解的示例整理，包括上传图片并分析图片内容的基本流程。

### 示例代码：基于Dashscope的LangChain4j视觉理解
这个示例展示了如何通过Dashscope的API，结合LangChain4j进行图像理解。使用的模型是`qwen-vl-max`，其支持视觉-语言的多模态任务。

```java
package com.pig4cloud.ai.example;

import com.dashscope.openai.OpenAiChatModel;
import com.dashscope.chat.ChatRequest;
import com.dashscope.chat.ChatResponse;
import com.dashscope.chat.content.ImageContent;
import com.dashscope.chat.content.TextContent;
import com.dashscope.chat.message.UserMessage;
import com.dashscope.util.FileUtil;
import java.util.Base64;

/**
 * 作者: lengleng
 * 视觉理解示例 - 通过Dashscope进行图像分析
 */
public class VisualUnderstandingExample {

    public void dashscopeVisualUnderstanding() {

        // 初始化OpenAiChatModel，使用qwen-vl-max模型
        OpenAiChatModel chatModel = OpenAiChatModel.builder()
                .apiKey(System.getenv("DASHSCOPE_KEY"))  // 从环境变量中读取API Key
                .modelName("qwen-vl-max")  // 设置使用的模型名称
                .baseUrl("https://dashscope.aliyuncs.com/compatible-mode/v1")  // Dashscope的基础URL
                .strictJsonSchema(true)  // 设置为严格JSON模式
                .build();

        // 读取图片并进行Base64编码
        String imageBase64 = Base64.getEncoder().encodeToString(FileUtil.readBytes("/Users/lengleng/Downloads/3.jpg"));

        // 构建用户消息，包含图片和文本提示
        UserMessage userMessage = UserMessage.from(
                TextContent.from("读取图片中的 中间位置：【发票号码】，底部位置：【金额】"),  // 用户输入的文本内容
                ImageContent.from(imageBase64, MediaType.IMAGE_JPEG_VALUE)  // 图片内容
        );

        // 创建聊天请求
        ChatRequest chatRequest = ChatRequest.builder()
                .messages(userMessage)
                .build();

        // 调用聊天模型，获取回复
        ChatResponse response = chatModel.chat(chatRequest);

        // 输出AI的文本回复
        System.out.println(response.aiMessage().text());
    }
}
```

### 说明：
1. **模型选择**：`qwen-vl-max` 是一个多模态大模型，支持图片和文本的结合输入，适用于视觉-语言任务。
2. **图片上传**：通过`Base64`编码将图片转化为字符串，结合`ImageContent`和`TextContent`一起发送到模型进行处理。
3. **API调用**：使用`OpenAiChatModel`来构建请求，并通过`chat()`方法调用模型。请求内容包括文本提示和图片，模型会根据输入返回分析结果。
4. **解析与输出**：从`ChatResponse`中获取AI的回复，打印出处理后的结果。

### 应用场景：
+ **图像文字提取**：如发票信息的提取、表单中的数据信息识别等。
+ **图像标注和理解**：结合文本输入，根据特定位置或内容要求对图像进行详细分析。

这种方法可以应用在实际业务中，如OCR（光学字符识别）、图像审核、合规性检查等。

