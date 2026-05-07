# Xiaozhi AI Knowledge Base - Complete Usage Guide

Source documentation:
- Official "Xiaozhi Knowledge Base Usage Instructions"
  Feishu wiki: https://pipic77kjb4.feishu.cn/wiki/QS5ewZHh0iOEI0kdbjncmtY1nFg
  Linked from the master encyclopedia at https://ccnphfhqs21z.feishu.cn/wiki/F5krwD16viZoF0kKkvDcrZNYnhb
- Xiaozhi AI Developer Certification page: https://xiaozhi.me/developer-auth
- Xiaozhi AI Console: https://xiaozhi.me

This document is an English translation and expansion of the official Feishu
documentation, plus the published quotas and entitlements visible on the
xiaozhi.me developer-auth page. The wording has been rewritten in US English
throughout for clarity and consistency.

---

## 1. What the Knowledge Base feature is

The Xiaozhi Knowledge Base is a Retrieval-Augmented Generation (RAG) feature
built into the xiaozhi.me console. It is attached to an **Agent** that already
drives one or more Xiaozhi voice devices, such as ESP32-S3, ESP32-C3, ESP32,
M5Stack, Waveshare boards, or breadboard DIY builds.

Official feature introduction:

> When the AI's answers in certain domains are not good enough, you can use
> the Knowledge Base feature to strengthen its capability. The user uploads
> domain knowledge, and the AI calls the content of the knowledge base to
> answer questions in that domain.

In practice this means the cloud LLM that powers the Xiaozhi voice agent
(Xiaozhi Lite, DeepSeek, Qwen 3, Doubao, GLM 4.7, Kimi K2, and similar models)
gets a new tool. When the user asks something that matches the knowledge base's
declared topic, the model is instructed to call that tool, retrieve relevant
chunks from the uploaded document, and answer using the retrieved text rather
than relying on general training alone.

The knowledge base is therefore not a separate chat surface. It works through
the same wake-word and voice loop you already use.

---

## 2. Status, beta limits, and quotas

The feature is currently in **Beta**. The Feishu page says plainly:

> During the testing period, only one knowledge base and one document upload
> are supported.

That single-document limit applies to ordinary accounts. Larger quotas are
listed on the Developer Certification page at
https://xiaozhi.me/developer-auth, where the per-tier permissions are laid out
as follows.

| Capability | Normal User | Open-Source Developer |
|---|---|---|
| Knowledge Base feature itself | Yes, limited-time free, Beta | Yes, limited-time free, Beta |
| Simple Parse Document mode | Yes | Yes |
| AI Optimized Parse Document mode | No | Yes, limited-time free |
| Number of knowledge bases per account | 1 | 10 |
| Documents per knowledge base | 1 | 10 |
| Devices that can be bound per knowledge base | 10 | 100 |

To unlock the developer tier, bind a GitHub account on the
xiaozhi.me/developer-auth page. The tier also unlocks the heavier LLMs
(Qwen 3, DeepSeek Full, Doubao, GLM 4.7 beta, Kimi K2 beta) and beta MCP
services.

Because the feature is described as limited-time free, the quotas and free
status can change. Always re-check the developer-auth table before making
product decisions.

---

## 3. End-to-end workflow

The official guide is short and is structured into two steps. Below is the full
flow, including the surrounding context you need to make the feature work
end-to-end with a Xiaozhi device.

```diagram
╭─────────────╮   ╭─────────────╮   ╭─────────────╮   ╭──────────────╮
│ Create KB   │──▶│ Write       │──▶│ Upload      │──▶│ Wait for     │
│ (name +     │   │ description │   │ document    │   │ Parse        │
│ description)│   │ (the prompt │   │ (one file   │   │ Complete     │
│             │   │ that tells  │   │ in beta)    │   │ status       │
│             │   │ the LLM     │   │             │   │              │
│             │   │ when to     │   │             │   │              │
│             │   │ call it)    │   │             │   │              │
╰─────────────╯   ╰─────────────╯   ╰─────────────╯   ╰──────┬───────╯
                                                              │
                                                              ▼
╭─────────────╮   ╭─────────────╮   ╭─────────────╮   ╭──────────────╮
│ Talk to     │◀──│ Restart /   │◀──│ Save agent  │◀──│ Open the     │
│ device,     │   │ wake the    │   │ config      │   │ Agent that   │
│ ask a topic │   │ device      │   │             │   │ should use   │
│ question    │   │             │   │             │   │ this KB,     │
│             │   │             │   │             │   │ tick the KB  │
│             │   │             │   │             │   │ in its tool  │
│             │   │             │   │             │   │ list         │
╰─────────────╯   ╰─────────────╯   ╰─────────────╯   ╰──────────────╯
```

### Step 1 - Create the knowledge base

In the xiaozhi.me console, open the Knowledge Base page and click the New
Knowledge Base button. A dialog opens. Two fields are required:

- Knowledge Base Name
  This is the label that will appear in the knowledge-base list when you later
  attach the KB to an Agent's role configuration. Use a short, recognizable
  name.

- Knowledge Base Description
  This is the most important field in the entire feature. The console itself
  flags it as important. The description is not a human-facing blurb. It is
  the prompt fragment the LLM sees when deciding whether to invoke the
  knowledge base as a tool. If the description is vague, the model will not
  call the tool and the KB will appear broken.

The official template, translated into English:

```text
This document contains content related to "xxxxx".
When the user mentions these things, I must not answer directly.
On every such turn I must call this tool to look up the material first,
and then answer based on the lookup result.

## The following is the content of "xxxxx"
(write the document summary here)
```

Replace each `xxxxx` with the actual topic or scope. The summary block under
`## The following is the content of "xxxxx"` should be a compact, plain-text
outline of what the document covers. The model uses this summary the same way
it would use an OpenAI tool description: as the routing signal for whether to
call the tool. The more concretely you list entities, places, products, model
numbers, foods, ingredients, and dates, the more reliably the model will
trigger retrieval.

A practical pattern that works well:

1. One sentence that clearly states the tool must be called and memory must not
   be used.
2. A `## The following is the content of ...` heading.
3. A flat list of the names, terms, SKUs, IDs, or trigger phrases that should
   activate retrieval.
4. A short paragraph describing the document's structure, such as chapters,
   chunk organization, or how to interpret references.

### Step 2 - Upload the document

After the knowledge base exists, click View on its row in the knowledge-base
list. You are taken to the document panel for that KB.

In the upper-right corner, click New Document and pick a file from your local
disk.

What happens next is automatic:

1. The file is uploaded to the Xiaozhi backend.
2. The backend immediately starts to parse the document. This step extracts
   text, applies the chosen parser, and chunks the result into retrieval units.
3. The parse can take from a few seconds to several minutes depending on
   document size and the parser chosen.
4. There is a refresh button next to the document row. Use it to poll progress.
5. When the parse is finished the status changes to Parse Complete. Until you
   see that status the KB will not return anything to the model.

If the status sticks on parsing or fails, the usual causes are an unsupported
binary format, a scanned PDF with no embedded text layer, an encrypted or
locked PDF, or a file size beyond what the parser accepts. Try the same
document re-exported as plain text or markdown.

### Step 3 - Attach the knowledge base to an Agent

The Knowledge Base feature is per-Agent. Once the KB shows Parse Complete:

1. Open the Console at https://xiaozhi.me.
2. Pick the Agent that drives your device, meaning the same Agent you created
   when you bound the device with its 6-digit code.
3. Click Configure Role.
4. In the role configuration, under the tools or knowledge-base section, tick
   the knowledge base you just created. The KB name from Step 1 is the label
   you will see here.
5. Click Save. Configuration is per-Agent, not global.
6. Reboot the device or wake it again. Existing live sessions do not always
   pick up the new tool list, so a restart or a new wake cycle is the reliable
   way to apply the change.

After this, when you talk to the device and ask a question that the description
text says it should answer from the KB, the LLM calls the retrieval tool,
receives the relevant chunks, and answers from them.

---

## 4. Parse modes - Simple vs AI Optimized

The developer-auth page lists two parsing modes:

- Simple Parse Document
  Available to every account, including normal users. This is a straightforward
  text extraction and chunking pipeline. It is the right choice for already
  clean inputs such as plain text files, markdown, or documents that are mostly
  prose with minimal layout. Parse time is short.

- AI Optimized Parse Document
  Available only to certified Open-Source Developers, currently as a limited
  time free entitlement. This pipeline runs an AI layer over the raw extraction
  to clean up structure such as tables, multi-column PDFs, header hierarchy,
  lists, and similar elements that a naive extractor would flatten. It produces
  noticeably better chunks for messy or richly formatted source material at the
  cost of longer parse time.

Practical rule of thumb:

- If you control the source and can ship clean Markdown or plain text, Simple
  Parse is enough and it is faster.
- If you must upload a real-world PDF, such as a manual, scanned brochure with
  OCR, or a tabular spec sheet, and you have the developer tier, use AI
  Optimized.

---

## 5. How the device, agent, and KB fit together

Xiaozhi devices speak to the xiaozhi.me cloud over WebSocket or MQTT plus UDP
(see the protocol docs at
https://github.com/78/xiaozhi-esp32/blob/main/docs/websocket.md). The firmware
does not store or query the knowledge base itself; everything happens on the
server side. The flow per voice turn looks like this:

```diagram
╭────────────╮   audio   ╭────────────╮   text   ╭────────────╮
│  Device    │──────────▶│  ASR       │─────────▶│  Agent LLM │
│ (ESP32)    │           │ (cloud)    │          │            │
╰─────▲──────╯           ╰────────────╯          ╰─────┬──────╯
      │                                                 │ tool
      │ TTS audio                                       │ call?
      │                                                 ▼
      │                                          ╭────────────╮
      │                                          │ Knowledge  │
      │                                          │ Base RAG   │
      │                                          │ (chunks +  │
      │                                          │ vectors)   │
      │                                          ╰─────┬──────╯
      │                                                 │ chunks
      │                                                 ▼
      │                                          ╭────────────╮
      │              text     ╭────────────╮     │  Agent LLM │
      ╰──────────────────────│    TTS     │◀────│  composes  │
                              ╰────────────╯     │  answer    │
                                                 ╰────────────╯
```

The Knowledge Base description text from Step 1 is what tells the Agent LLM
when it should make the tool call. The chunked document content from Step 2 is
what the tool actually returns when called.

Because the Knowledge Base is bound to an Agent and the Agent is bound to
devices, multiple devices that share an Agent share the same knowledge base.
The devices-per-knowledge-base quota in the Developer Certification table
(10 for normal users, 100 for developers) is the cap on how many physical
devices can be linked through a KB-enabled Agent.

---

## 6. Writing effective documents for Xiaozhi RAG

The official documentation does not specify exact chunk sizes for the Xiaozhi
parser, but the behavior is the standard pattern of vector RAG: the parser
splits the file into many small fragments, each fragment is embedded, and at
query time the top matching fragments are returned to the model as additional
context.

Two consequences follow that are worth designing for:

1. The model often sees only one or two retrieved chunks. A chunk that only
   makes sense in the context of surrounding chunks will fail. Each chunk needs
   to repeat enough identifying context to stand alone, such as the topic name,
   entity names, or the kind of question it answers.
2. Layout is fragile. Markdown headings, bullet lists, tables, and blank lines
   may collapse during parsing. Important signals should be carried in the
   prose itself, using repeated labels like `Use for:`, `Ask when:`, and
   `Answer:`, not in formatting that may not survive.

If you are preparing reference material for a Xiaozhi knowledge base, the
following practices help:

- Keep paragraphs short and self-contained.
- Repeat the topic name and the key entities in every paragraph that talks
  about them, even at the cost of redundancy.
- Use plain ASCII punctuation and avoid heavy tables.
- Put critical trigger terms, meaning the phrases a user might actually say,
  literally in the text so the embedding model can match them.
- Mirror common spelling variants, such as Male and Malé, Canton and
  Guangzhou, or product codes with and without dashes.
- For long source material, write a short routing index at the top whose
  individual entries are themselves self-contained, rather than relying on the
  reader or model to scroll.

These are general RAG hygiene rules, but they matter more for Xiaozhi because
of the strict beta limit of one document per KB for normal users. Your single
uploaded file has to carry everything.

---

## 7. Description-prompt patterns that work in practice

The official template from Step 1 is generic. In practice the description
prompt is where most of the routing quality comes from, and expanding it with
concrete entities and phrasings dramatically improves when the model decides to
call the tool. Two expansion patterns:

### 7.1 Topic-List Pattern

```text
This document contains content about "<TOPIC>",
including: <person A>, <person B>, <place A>, <place B>,
<product code A>, <product code B>, <event A>, <event B>.

When the user asks anything that mentions any of these names,
or asks "<sample question 1>", "<sample question 2>",
"<sample question 3>", I must not answer from memory.
On every such turn I must call this tool first and then
answer using only what the tool returns.

## The following is the content of "<TOPIC>"
<one-paragraph summary of what is inside the document and how
it is organized, for example: "the file starts with a routing index
of short blocks, followed by the full text with numbered lines">
```

### 7.2 Q&A Trigger Pattern

```text
This document is the source of truth for the following questions:
- <question 1>
- <question 2>
- <question 3>
- <question 4>
For these questions I must call this tool every time and answer
strictly from its returned content. I must not answer from
general knowledge.

## The following is the content
<short summary of structure>
```

Both patterns work because they give the LLM an explicit must-call
instruction, a list of literal trigger strings that match what users actually
say, and a short sketch of what is inside.

---

## 8. Troubleshooting

| Symptom | Likely cause | Fix |
|---|---|---|
| Document is stuck on parsing forever | Scanned or encrypted PDF, or unsupported binary | Re-export as plain text or markdown and re-upload |
| Status is Parse Complete but the device never uses the KB | The Agent does not have the KB ticked, or the description prompt is too vague | Re-open the Agent's Configure Role page, tick the KB, save, and reboot the device |
| Device uses the KB sometimes but not for the obvious question | Trigger phrasings are missing from the description | Add the literal phrasings the user says into the description prompt |
| Cannot create a second KB | Account is on the normal tier, with a one-KB cap | Bind a GitHub account on /developer-auth to upgrade to 10 KBs |
| Cannot upload a second document | Normal tier is capped at 1 document per KB | Upgrade the tier, or merge content into one file |
| Answers are confidently wrong | Retrieved chunk lacked context | Rewrite the source so each paragraph stands alone, repeat key entities, and keep paragraphs short |
| Knowledge base disappears after some time | Beta entitlement or limited-time free period changed | Re-check https://xiaozhi.me/developer-auth for current status |

---

## 9. Quick checklist

Before you ship a Xiaozhi knowledge base to a real device, verify:

- The KB exists and is named recognizably.
- The KB description includes an explicit must-call-this-tool instruction and a
  list of trigger entities or phrasings.
- The document status reads Parse Complete.
- The Agent that drives the target device has the KB ticked under Configure
  Role and has been saved.
- The device has been rebooted or re-woken since the change.
- You have asked at least one question whose answer you know is in the
  document, and the device returned that answer rather than a generic one.
- You have asked at least one question that should not trigger the KB and
  confirmed the device still answers normally, otherwise the description is
  over-firing.

---

## 10. Reference URLs

- Master encyclopedia:
  https://ccnphfhqs21z.feishu.cn/wiki/F5krwD16viZoF0kKkvDcrZNYnhb
- Knowledge Base usage page:
  https://pipic77kjb4.feishu.cn/wiki/QS5ewZHh0iOEI0kdbjncmtY1nFg
- Developer Certification, quotas, models, and KB tiers:
  https://xiaozhi.me/developer-auth
- Xiaozhi AI Console:
  https://xiaozhi.me
- Voice-clone console guide:
  https://rcnv1t9vps13.feishu.cn/wiki/Kxw3wBk0EiepeQkOHpncWTOrn4r
- Common Q&A:
  https://rcnv1t9vps13.feishu.cn/wiki/JiQowaSe1itt07kyVvZcHFcQnee
- WebSocket protocol, how the device talks to the cloud where the KB lives:
  https://github.com/78/xiaozhi-esp32/blob/main/docs/websocket.md
