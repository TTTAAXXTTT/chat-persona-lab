# Chat Persona Lab

Local chat analysis, communication-style memory, and human-reviewed reply drafts.

[中文](README.md) · [Download complete v0.1.0 source](chat-persona-lab-v0.1.0-source.zip?raw=true)

This repository currently distributes the complete project as a source ZIP.
Extract it and enter `chat-persona-lab` before running the commands below.
All 70 project files are included: Python core, Chinese local UI, fictional
examples, tests, full Chinese/English README files, documentation, templates,
and maintenance scripts. No private chats, profiles, credentials, databases,
or old Git history are included.

Requires Python 3.11+. The source checkout's core needs no third-party runtime
packages.

```sh
python scripts/create_demo.py
python -m persona_agent --analyze-only
python -m persona_agent --offline-fake-model
```

Open http://127.0.0.1:8765 and submit a fictional greeting. Approval defaults to
`approved_not_sent`. The offline model returns deterministic fixtures to verify
the workflow; it does not demonstrate real AI response quality.

The project imports formatted chat text and XLSX profile rows, builds a local
SQLite index, computes self-message style statistics, retrieves contact-scoped
examples, and offers editable drafts with explicit human approval. Optional
cloud providers are DeepSeek official and OpenCode Go. Experimental Windows
adapters are provided separately from the offline workflow.

Cloud processing and external sending are disabled by default. Opting into
cloud mode can transmit selected messages, profile rows, context, examples,
and style statistics to the provider. Redaction is best effort. Local storage
is plaintext. Do not expose the loopback HTTP service publicly.

The first release passed 146 local regression tests and a real local HTTP
smoke test from a clean extraction. Live providers and actual WeChat accounts
were not part of this acceptance. CI configuration is included inside the ZIP;
it does not run directly in this archive-distribution repository.

Detailed guides, contributing instructions, and security notes are in the
extracted project. Do not post private data in issues.

[MIT License](LICENSE) · [Third-party notices](NOTICE.md) · [Checksums](SHA256SUMS.txt)
