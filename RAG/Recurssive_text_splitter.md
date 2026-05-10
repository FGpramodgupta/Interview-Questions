Yes — `RecursiveCharacterTextSplitter` dynamically tries multiple separators in order until the chunk size condition is satisfied.

By default, it uses this hierarchy:

```python
["\n\n", "\n", " ", ""]
```

Meaning:

1. First try splitting by paragraphs (`\n\n`)
2. If chunk is still too large → split by newline (`\n`)
3. Still too large → split by spaces (` `)
4. Still too large → split character by character (`""`)

That’s why it is called recursive.

Example:

```python
from langchain_text_splitters import RecursiveCharacterTextSplitter

text = """
This is paragraph 1.

This is paragraph 2.
It has multiple lines.

This is paragraph 3.
"""

splitter = RecursiveCharacterTextSplitter(
    chunk_size=50,
    chunk_overlap=10
)

chunks = splitter.split_text(text)

print(chunks)
```

Flow internally:

```text
Step 1 → Try \n\n
    If chunk > 50 chars
        ↓
Step 2 → Try \n
    If still > 50
        ↓
Step 3 → Try spaces
    If still > 50
        ↓
Step 4 → Split character-wise
```

This helps preserve semantic meaning as much as possible.

---

You can also customize separators:

```python
splitter = RecursiveCharacterTextSplitter(
    chunk_size=1000,
    chunk_overlap=200,
    separators=[
        "\n## ",   # markdown heading
        "\n\n",
        "\n",
        ". ",
        " ",
        ""
    ]
)
```

This is very useful for:

* Markdown documents
* Emails
* OCR documents
* Legal docs
* Insurance documents
* HTML converted text

For your email extraction / LangGraph workflows, a good production strategy is:

```text
Email Body
   ↓
Structure-aware split
   ↓
RecursiveCharacterTextSplitter
   ↓
LLM extraction
```

Especially useful when:

* Long email chains
* Embedded signatures
* Forwarded conversations
* OCR extracted attachments
* Policy documents

Typical production config:

```python
RecursiveCharacterTextSplitter(
    chunk_size=1200,
    chunk_overlap=150,
    length_function=len
)
```

For OpenAI token-aware chunking:

```python
from tiktoken import get_encoding

enc = get_encoding("cl100k_base")

splitter = RecursiveCharacterTextSplitter.from_tiktoken_encoder(
    chunk_size=500,
    chunk_overlap=50
)
```

This is generally better for:

* GPT-4
* Bedrock Claude
* Long-context models
* Cost optimization
* Avoiding token overflow
