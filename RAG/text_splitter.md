In [LangChain Text Splitters Documentation](https://docs.langchain.com/oss/python/integrations/splitters/index?utm_source=chatgpt.com), there are multiple types of text splitters available depending on your use case like RAG, summarization, semantic search, code understanding, markdown parsing, etc.

Here are the most commonly used text splitters in [LangChain](https://www.langchain.com?utm_source=chatgpt.com):

---

# 1. RecursiveCharacterTextSplitter (Most Recommended)

RecursiveCharacterTextSplitter

This is the default and most commonly used splitter.

It recursively splits using:

* Paragraphs (`\n\n`)
* Lines (`\n`)
* Spaces
* Characters

Best for:

* General RAG pipelines
* PDFs
* Long documents
* Mixed text

```python
from langchain_text_splitters import RecursiveCharacterTextSplitter

splitter = RecursiveCharacterTextSplitter(
    chunk_size=1000,
    chunk_overlap=200
)

chunks = splitter.split_text(text)
```

Why popular:

* Preserves semantic meaning better
* Avoids breaking sentences abruptly
* Works well for embeddings

([LangChain Docs][1])

---

# 2. CharacterTextSplitter

CharacterTextSplitter

Simple splitter using a separator.

Best for:

* Structured plain text
* Paragraph-based chunking

```python
from langchain_text_splitters import CharacterTextSplitter

splitter = CharacterTextSplitter(
    separator="\n\n",
    chunk_size=1000,
    chunk_overlap=100
)
```

Example:

* Split by paragraphs
* Split by newline

([LangChain Reference Docs][2])

---

# 3. TokenTextSplitter

TokenTextSplitter

Splits based on tokens instead of characters.

Best for:

* OpenAI models
* Token-sensitive applications
* Avoiding context limit overflow

```python
from langchain_text_splitters import TokenTextSplitter

splitter = TokenTextSplitter(
    chunk_size=256,
    chunk_overlap=20
)
```

Useful when:

* GPT context windows matter
* You want exact token control

([LangChain Reference Docs][2])

---

# 4. MarkdownHeaderTextSplitter

MarkdownHeaderTextSplitter

Splits markdown documents using headers.

Best for:

* Documentation
* README files
* Knowledge bases

```python
from langchain_text_splitters import MarkdownHeaderTextSplitter

headers_to_split_on = [
    ("#", "Header 1"),
    ("##", "Header 2"),
]

splitter = MarkdownHeaderTextSplitter(
    headers_to_split_on=headers_to_split_on
)
```

Advantages:

* Keeps section hierarchy
* Adds metadata from headings

([LangChain Docs][3])

---

# 5. MarkdownTextSplitter

MarkdownTextSplitter

Markdown-aware recursive splitter.

Best for:

* Markdown chunking
* Technical docs

```python
from langchain_text_splitters import MarkdownTextSplitter

splitter = MarkdownTextSplitter(
    chunk_size=1000,
    chunk_overlap=100
)
```

([LangChain Reference Docs][4])

---

# 6. HTMLHeaderTextSplitter

HTMLHeaderTextSplitter

Splits HTML using tags like:

* `<h1>`
* `<h2>`
* `<h3>`

Best for:

* Web pages
* HTML docs
* Scraped websites

```python
from langchain_text_splitters import HTMLHeaderTextSplitter
```

([LangChain Reference Docs][5])

---

# 7. HTMLSemanticPreservingSplitter

HTMLSemanticPreservingSplitter

Preserves semantic HTML structure.

Best for:

* Complex websites
* Nested HTML content

([LangChain Reference Docs][5])

---

# 8. RecursiveJsonSplitter

RecursiveJsonSplitter

Splits JSON while preserving hierarchy.

Best for:

* APIs
* Structured documents
* JSON datasets

```python
from langchain_text_splitters import RecursiveJsonSplitter
```

([LangChain Reference Docs][2])

---

# 9. Language-Specific Code Splitters

LangChain has code-aware splitters for programming languages.

Examples:

* PythonCodeTextSplitter
* JS/TS splitters
* JSX/Vue/Svelte splitters
* Latex splitter

Best for:

* Source code RAG
* Repo chatbots
* Code embeddings

```python
from langchain_text_splitters import PythonCodeTextSplitter
```

([LangChain Reference Docs][2])

---

# 10. NLTKTextSplitter

NLTKTextSplitter

Uses sentence tokenization via NLTK.

Best for:

* Sentence-level chunking
* NLP pipelines

([LangChain Reference Docs][2])

---

# Practical Recommendation

| Use Case                | Best Splitter                  |
| ----------------------- | ------------------------------ |
| General RAG             | RecursiveCharacterTextSplitter |
| Markdown Docs           | MarkdownHeaderTextSplitter     |
| Codebase Chatbot        | PythonCodeTextSplitter         |
| HTML/Websites           | HTMLHeaderTextSplitter         |
| Exact LLM Token Control | TokenTextSplitter              |
| JSON APIs               | RecursiveJsonSplitter          |
| Sentence Chunking       | NLTKTextSplitter               |

---

# Common Production Strategy

Most production RAG systems use:

```text
Document Loader
   ↓
Structure-aware splitter
   ↓
RecursiveCharacterTextSplitter
   ↓
Embeddings
   ↓
Vector DB
```

Example:

* MarkdownHeaderTextSplitter → RecursiveCharacterTextSplitter
* HTMLHeaderTextSplitter → RecursiveCharacterTextSplitter

This preserves structure first, then optimizes chunk size.

([LangChain Docs][3])

[1]: https://docs.langchain.com/oss/python/integrations/splitters/index?utm_source=chatgpt.com "Text splitters - Docs by LangChain"
[2]: https://reference.langchain.com/python/langchain-text-splitters/langchain_text_splitters?utm_source=chatgpt.com "langchain_text_splitters | LangChain Reference"
[3]: https://docs.langchain.com/oss/python/integrations/splitters/markdown_header_metadata_splitter?utm_source=chatgpt.com "Split markdown - text splitter integration - Docs by LangChain"
[4]: https://reference.langchain.com/python/langchain-text-splitters/markdown/MarkdownTextSplitter?utm_source=chatgpt.com "MarkdownTextSplitter | langchain_text_splitters | LangChain Reference"
[5]: https://reference.langchain.com/python/langchain-text-splitters/html?utm_source=chatgpt.com "html | langchain_text_splitters | LangChain Reference"
