---
title: langchain4j 动态json
---

### 1. **定义基础的JSON Schema**
首先要有一个基础的JSON Schema结构，`LangChain4j`允许我们动态地生成和调整这些结构。可以基于业务需求，使用不同的类型（如字符串、数组、整数等）。

```java
public JsonSchema createDynamicJsonSchema(Map<String, JsonElementSchema> properties) {
    return JsonSchema.builder()
            .rootElement(JsonObjectSchema.builder()
                    .properties(properties)
                    .build())
            .build();
}
```

在这里，`properties`是一个`Map<String, JsonElementSchema>`，其中键是JSON字段名，值是字段的类型，可以是字符串、数组、整数等。比如：

+ `"title": JSON_STRING_SCHEMA`
+ `"preparationTimeMinutes": JSON_INTEGER_SCHEMA`

### 2. **根据业务逻辑动态生成JSON Schema**
假设你有不同的业务场景需要输出不同的字段组合，你可以通过逻辑选择不同的字段。

```java
public JsonSchema buildSchemaForBusiness() {
    Map<String, JsonElementSchema> properties = new HashMap<>();

        properties.put("invoiceNumber", JSON_STRING_SCHEMA);
        properties.put("totalAmount", JSON_NUMBER_SCHEMA);
        properties.put("items", JsonArraySchema.builder()
                .items(JsonObjectSchema.builder()
                        .properties(Map.of(
                                "itemName", JSON_STRING_SCHEMA,
                                "price", JSON_NUMBER_SCHEMA
                        ))
                        .build())
                .build());
    
    return createDynamicJsonSchema(properties);
}
```

### 3. **将JSON Schema集成到请求中**
一旦我们根据业务场景生成了相应的JSON Schema，就可以将其嵌入到`ChatRequest`中，用于控制输出格式。

```java
public void processChatWithDynamicSchema(String businessType, String inputText, String imagePath) {
    OpenAiChatModel chatModel = OpenAiChatModel.builder()
            .strictJsonSchema(true)
            .build();

    // 动态生成JSON Schema
    JsonSchema dynamicSchema = buildSchemaForBusiness(businessType);

    ResponseFormat responseFormat = ResponseFormat.builder()
            .type(JSON)
            .jsonSchema(dynamicSchema)
            .build();

    ChatRequest chatRequest = ChatRequest.builder()
            .messages(UserMessage.from(
                    TextContent.from(inputText)
            ))
            .responseFormat(responseFormat)
            .build();

    ChatResponse response = chatModel.chat(chatRequest);
    System.out.println(response.aiMessage().text());
}
```

### 4. **实际使用示例**
例如，你需要根据业务场景处理不同类型的输入（例如OCR和发票信息），可以通过调用`processChatWithDynamicSchema`动态设置Schema。这样就可以根据业务需求动态生成不同的`JSON Schema`并控制LangChain4j的输出格式。

