---
title: langchain4j rag 解析
---

### LangChain4j系列：LangChain4j Document Loaders和 Parsers 详解与应用
#### 引言
在构建RAG（Retrieval-Augmented Generation）应用时，文档的加载和解析是至关重要的步骤。LangChain4j框架通过提供多种文档加载器（Document Loaders）和解析器（Document Parsers），为开发者提供了强大的工具支持。本文将详细介绍LangChain4j中的文档加载器和解析器的使用方法，为后续开发RAG应用打下坚实的基础。

#### Document Loaders
LangChain4j框架目前支持多种文档加载器，涵盖了从云存储到本地文件系统的各种场景。

| Loaders | APIs | Examples | Description |
| --- | --- | --- | --- |
| Amazon S3 | AmazonS3DocumentLoader | AmazonS3DocumentLoaderIT.java | 从S3存储中加载文档 |
| Azure Blob Storage | AzureBlobStorageDocumentLoader | AzureBlobStorageDocumentLoaderIT | 从Azure二进制存储中加载文档 |
| File System | FileSystemDocumentLoader | FileSystemDocumentLoaderTest | 从文件系统加载文档 |
| Github | GitHubDocumentLoader | GitHubDocumentLoaderIT | 从Github中加载文档 |
| Selenium | SeleniumDocumentLoader | SeleniumDocumentLoaderIT |  |
| Tencent COS | TencentCosDocumentLoader | TencentCosDocumentLoaderIT | 腾讯云存储加载文档 |
| URL | UrlDocumentLoader | UrlDocumentLoaderTest | 根据URL加载文档 |


##### UrlDocumentLoader
`UrlDocumentLoader` 是一个用于从指定URL加载文档的工具类。以下是其核心代码：

```java
public class UrlDocumentLoader {

    /**
     * 从指定的url获取文档。
     *
     * @param url            文件的url地址
     * @param documentParser 从url中解析文本内容的解析器。
     * @return document 文档内容
     */
    public static Document load(URL url, DocumentParser documentParser) {
        return DocumentLoader.load(UrlSource.from(url), documentParser);
    }

    public static Document load(String url, DocumentParser documentParser) {
        return load(createUrl(url), documentParser);
    }

    // 根据字符串url创建 URL对象。
    static URL createUrl(String url) {
        try {
            return new URL(url);
        } catch (MalformedURLException e) {
            throw new IllegalArgumentException(e);
        }
    }
}
```

示例：

```java
public class UrlDocumentLoaderTest implements WithAssertions {
    @Test
    public void should_load_text_document_from_url() {
        String url = "https://raw.githubusercontent.com/langchain4j/langchain4j/main/langchain4j/src/test/resources/test-file-utf8.txt";
        Document document = UrlDocumentLoader.load(url, new TextDocumentParser());
        assertThat(document.text()).isEqualTo("test\ncontent");
        assertThat(document.metadata().getString("url")).isEqualTo(url);
    }

    @Test
    public void test_bad_url() {
        String url = "bad_url";
        assertThatExceptionOfType(IllegalArgumentException.class)
                .isThrownBy(() -> UrlDocumentLoader.load(url, new TextDocumentParser()))
                .withMessageContaining("no protocol");
    }
}
```

##### FileSystemDocumentLoader
`FileSystemDocumentLoader` 使用单例设计模式，并提供了多种加载文件的方法：

+ `loadDocument()`：加载指定目录下的一个文件。
+ `loadDocuments()`：加载指定目录下的所有文件。
+ `loadDocumentsRecursively()`：递归加载指定目录及其子目录下的所有文件。

以下是核心代码：

```java
public class FileSystemDocumentLoader {
    private static final DocumentParser DEFAULT_DOCUMENT_PARSER = getOrDefault(loadDocumentParser(), TextDocumentParser::new);

    public static Document loadDocument(Path filePath, DocumentParser documentParser) {
        if (!isRegularFile(filePath)) {
            throw illegalArgument("'%s' is not a file", filePath);
        }

        return DocumentLoader.load(from(filePath), documentParser);
    }

    public static List<Document> loadDocuments(Path directoryPath,
                                               PathMatcher pathMatcher,
                                               DocumentParser documentParser) {
        if (!isDirectory(directoryPath)) {
            throw illegalArgument("'%s' is not a directory", directoryPath);
        }

        try (Stream<Path> pathStream = Files.list(directoryPath)) {
            return loadDocuments(pathStream, pathMatcher, directoryPath, documentParser);
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }

    public static List<Document> loadDocumentsRecursively(Path directoryPath,
                                                          PathMatcher pathMatcher,
                                                          DocumentParser documentParser) {
        if (!isDirectory(directoryPath)) {
            throw illegalArgument("'%s' is not a directory", directoryPath);
        }

        try (Stream<Path> pathStream = Files.walk(directoryPath)) {
            return loadDocuments(pathStream, pathMatcher, directoryPath, documentParser);
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }

    private static List<Document> loadDocuments(Stream<Path> pathStream,
                                                PathMatcher pathMatcher,
                                                Path pathMatcherRoot,
                                                DocumentParser documentParser) {
        List<Document> documents = new ArrayList<>();

        pathStream
                .filter(Files::isRegularFile)
                .map(pathMatcherRoot::relativize)
                .filter(pathMatcher::matches)
                .map(pathMatcherRoot::resolve)
                .forEach(file -> {
                    try {
                        Document document = loadDocument(file, documentParser);
                        documents.add(document);
                    } catch (BlankDocumentException ignored) {
                        // blank/empty documents are ignored
                    } catch (Exception e) {
                        String message = e.getCause() != null ? e.getCause().getMessage() : e.getMessage();
                        log.warn("Failed to load '{}': {}", file, message);
                    }
                });

        return documents;
    }

    private static DocumentParser loadDocumentParser() {
        Collection<DocumentParserFactory> factories = loadFactories(DocumentParserFactory.class);

        if (factories.size() > 1) {
            throw new RuntimeException("Conflict: multiple document parsers have been found in the classpath. " +
                    "Please explicitly specify the one you wish to use.");
        }

        for (DocumentParserFactory factory : factories) {
            return factory.create();
        }

        return null;
    }
}
```

#### DocumentSource
`DocumentSource` 主要负责读取文件并转换为 `InputStream` 和元数据。每种加载器的实现略有不同，以下是其类结构：

```java
public interface DocumentSource {
    InputStream inputStream();
    Metadata metadata();
}

public class UrlSource implements DocumentSource {
    private final URL url;

    public InputStream inputStream() {
        // 具体实现
    }

    public Metadata metadata() {
        // 具体实现
    }

    public static UrlSource from(String url) {
        return new UrlSource(createUrl(url));
    }

    public static UrlSource from(URL url) {
        return new UrlSource(url);
    }

    public static UrlSource from(URI uri) {
        return new UrlSource(createUrl(uri.toString()));
    }
}

public class FileSystemSource implements DocumentSource {
    private final Path path;

    public InputStream inputStream() {
        // 具体实现
    }

    public Metadata metadata() {
        // 具体实现
    }

    public static FileSystemSource from(Path filePath) {
        return new FileSystemSource(filePath);
    }

    public static FileSystemSource from(String filePath) {
        return new FileSystemSource(Paths.get(filePath));
    }

    public static FileSystemSource from(File file) {
        return new FileSystemSource(file.toPath());
    }

    public static FileSystemSource from(URI uri) {
        return new FileSystemSource(Paths.get(uri));
    }
}
```

#### Document Parsers
LangChain4j框架支持多种文档解析器，涵盖了从文本到PDF的各种文档格式。

| 解析器名称 | 作用 | APIs | 使用示例 |
| --- | --- | --- | --- |
| Text | 解析文本 | TextDocumentParser | TextDocumentParserTest |
| Apache Tika | 解析文档 | ApacheTikaDocumentParser | ApacheTikaDocumentParserTest |
| Apache POI | 解析文档 | ApachePoiDocumentParser | ApachePoiDocumentParserTest |
| Apache PDFBOX | 解析 PDF 文件 | ApachePdfBoxDocumentParser | ApachePdfBoxDocumentParserTest |


LangChain4j框架支持的四种解析器，实现基于大家日常开发中非常熟悉的底层技术，如POI、PDFBox等。对于Apache Tika，它在文档解析和MimeType探测方面非常强大，值得深入学习。

#### 总结
本文详细介绍了LangChain4j框架中的文档加载器和解析器的使用方法。文档加载器负责从不同来源加载文档，而文档解析器则负责解析这些文档。通过这些工具，开发者可以轻松地将文档转换为`Document`对象，为后续的RAG应用打下坚实的基础。

