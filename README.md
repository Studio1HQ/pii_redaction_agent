<div align="center">

<img src="https://raw.githubusercontent.com/Studio1HQ/Studio1HQ/main/assets/banners/pii_redaction_agent.webp" alt="PII Redaction Agent on Jozu RIC: Redact PII from PDFs with Qwen3.6 served by a Jozu Rapid Inference Container" width="100%" />

<br />
<br />

# PII Redaction Agent on Jozu RIC

### Redact PII from PDFs with Qwen3.6 served by a Jozu Rapid Inference Container

A notebook that packages a 1-bit Qwen3.6-35B-A3B GGUF as a ModelKit, serves it with a Jozu RIC and runs a redaction agent with an audit guardrail over a sample medical PDF.

<br />

[![Built by Studio1](https://img.shields.io/badge/Built_by_Studio1-f97316?style=for-the-badge&logo=data%3Aimage%2Fsvg%2Bxml%3Bbase64%2CPHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAxNjAgMTYwIiBmaWxsPSJub25lIj48cGF0aCBmaWxsLXJ1bGU9ImV2ZW5vZGQiIGNsaXAtcnVsZT0iZXZlbm9kZCIgZD0iTTAgMzVDMCAxNi42ODA3IDE0LjA3NDQgMS42NDg0NCAzMiAwLjEyNjk1M1YxNDFINTEuMDUyM0M1Mi40MzA4IDEzNy4zNDQgNTQuNzQ0NiAxMzMuNjg5IDU3Ljk5MzggMTMwLjAzM0M2MS4xNDQ2IDEyNi41MDggNjQuNjg5MiAxMjMuNDQgNjguNjI3NyAxMjAuODI5QzcyLjU2NjIgMTE4LjM0OSA3Ni4zNTY5IDExNi43ODIgODAgMTE2LjEyOVY4Ni43NTQ0Qzc1LjQ3MDggODcuOTI5MiA3MS4zODQ2IDg5LjY5MTkgNjcuNzQxNSA5Mi4wNDE1QzY0IDk0LjUyMjUgNjAuNzUwOCA5Ny40NiA1Ny45OTM4IDEwMC44NTRDNTYuNTM0NSAxMDIuNzIgNTUuMjU0NSAxMDQuNjU5IDU0LjE1MzkgMTA2LjY3MVYwSDEyNUMxNDQuMzMgMCAxNjAgMTUuNjY5OSAxNjAgMzVWMTI1QzE2MCAxNDQuMzMgMTQ0LjMzIDE2MCAxMjUgMTYwSDEyNFYxOEgxMDQuOTQ4QzEwMy41NjkgMjEuNjgxNiAxMDEuMjU1IDI1LjM2MjggOTguMDA2MiAyOS4wNDQ0Qzk0Ljg1NTQgMzIuNTk0MiA5MS4zMTA4IDM1LjY4NDEgODcuMzcyMyAzOC4zMTRDODMuNDMzOCA0MC44MTIgNzkuNjQzMSA0Mi4zODk2IDc2IDQzLjA0NzRWNzIuNjMwNEM4MC41MjkyIDcxLjQ0NzMgODQuNjE1NCA2OS42NzI0IDg4LjI1ODUgNjcuMzA1N0M5MiA2NC44MDc2IDk1LjI0OTIgNjEuODQ5MSA5OC4wMDYyIDU4LjQzMDdDOTkuNDY1NSA1Ni41NTEzIDEwMC43NDUgNTQuNTk4NiAxMDEuODQ2IDUyLjU3MjNWMTYwSDM1QzE1LjY3IDE2MCAwIDE0NC4zMyAwIDEyNVYzNVoiIGZpbGw9IiNmZmZmZmYiLz48L3N2Zz4K)](https://studio1hq.com)
[![Built with Jozu](https://img.shields.io/badge/Built_with_Jozu-141414?style=for-the-badge)](https://jozu.com)

[Jozu docs](https://jozu.com/docs) &nbsp;·&nbsp; [Jozu](https://jozu.com) &nbsp;·&nbsp; [KitOps](https://kitops.org) &nbsp;·&nbsp; [More examples](https://github.com/Studio1HQ) &nbsp;·&nbsp; [studio1hq.com](https://studio1hq.com)

</div>

<br />

## Overview

This repo accompanies the blog post "PII (Personal Identifiable Information) Redaction with Qwen 3.6 on Jozu Rapid Inference Container". It holds the notebook (`main.ipynb`) and a sample file, `medical_pii_sample.pdf`.

You download a 1-bit quantized `Qwen3.6-35B-A3B` GGUF, package it as a [KitOps](https://kitops.org) ModelKit, push it to [Jozu Hub](https://jozu.com) and run it with a Jozu Rapid Inference Container (RIC). A redaction agent built with the OpenAI Agents SDK then converts the PDF to markdown and redacts the PII.

## Features

- **Quantized model packaging**: `Qwen3.6-35B-A3B-UD-IQ1_M.gguf` from `unsloth/Qwen3.6-35B-A3B-GGUF` becomes a ModelKit on Jozu Hub.
- **Jozu RIC serving**: The model runs as a `llama-cpp-cuda` container with an OpenAI-compatible API on port 8000.
- **PDF to markdown**: `pymupdf4llm` converts the input PDF before redaction.
- **Redaction agent**: An `Agent` with structured output (`RedactionOutput`) returns the redacted markdown.
- **Review guardrail**: A strict `ReviewAgent` acts as an output guardrail and rejects documents that still contain PII or quasi-identifiers, listing what remains.
- **Retries**: `perform_redaction` takes `max_attempts` (3 in the example) and writes `redacted_medical_sample.md`.

## Tech stack

| Layer | Tool |
| --- | --- |
| Model | [Qwen3.6-35B-A3B GGUF](https://huggingface.co/unsloth/Qwen3.6-35B-A3B-GGUF) (IQ1_M) from Unsloth |
| Packaging | [KitOps](https://kitops.org) 1.5.0 |
| Registry and runtime | [Jozu Hub](https://jozu.ml) and Jozu Rapid Inference Container |
| Agents | [OpenAI Agents SDK](https://openai.github.io/openai-agents-python/) 0.14.2, Pydantic |
| PDF parsing | [PyMuPDF4LLM](https://pymupdf.readthedocs.io/en/latest/pymupdf4llm/) |
| Runtime | Python 3.11+, Docker, Jupyter |

## Getting started

### Prerequisites

- Python 3.11 or later and Jupyter
- Docker
- A Jozu Hub account at [jozu.ml](https://jozu.ml)
- Homebrew on Linux or macOS (the notebook installs it if missing)
- A machine with an NVIDIA GPU and the NVIDIA container toolkit for the `--gpus all` Docker command

### Installation

```bash
git clone https://github.com/Studio1HQ/pii_redaction_agent.git
cd pii_redaction_agent
brew tap kitops-ml/kitops
brew install kitops
pip install kitops==1.5.0 huggingface_hub==1.11.0 openai==2.32.0 openai-agents==0.14.2 pydantic==2.13.2 pymupdf4llm==1.27.2.2
```

### Environment variables

No environment variables. The notebook authenticates with the email and password of your Jozu Hub account and uses placeholders you replace.

| Placeholder | Description |
| --- | --- |
| `email-prefix-here` | The part of your Jozu Hub email before the `@` (for `jack123@gmail.com` use `jack123`). Keep tags lowercase. |
| `full-email-here`, `password-here` | Jozu Hub credentials passed to `UserCredentials` in the notebook. |

### Run

Log in to Jozu Hub in a terminal (you are prompted for email and password), then open the notebook:

```bash
kit login jozu.ml
jupyter notebook main.ipynb
```


## How it works

1. Download the GGUF with `hf_hub_download(repo_id="unsloth/Qwen3.6-35B-A3B-GGUF", filename="Qwen3.6-35B-A3B-UD-IQ1_M.gguf", local_dir="./Qwen3.6-35B-A3B-GGUF")`.
2. Build a Kitfile and push with `ModelKitManager` to `jozu.ml/email-prefix-here/qwen3.6-35b-a3b-gguf:latest`.
3. Run the RIC:
   ```bash
   sudo docker run -it --rm -p 8000:8000 --gpus all "jozu.ml/email-prefix-here/qwen3.6-35b-a3b-gguf/llama-cpp-cuda:latest"
   ```
4. Point an `AsyncOpenAI` client at `http://127.0.0.1:8000/v1` and wrap it in `OpenAIChatCompletionsModel`. Model settings use temperature 0.7, `max_tokens` 8192 and `enable_thinking: False`.
5. Convert `medical_pii_sample.pdf` to markdown and call `perform_redaction(original_content=..., output_file=..., max_attempts=3)`. The review agent guardrail decides whether the output is accepted.

## Resources

- [Jozu documentation](https://jozu.com/docs): Jozu Hub and Rapid Inference Container guides.
- [Jozu](https://jozu.com): Product site.
- [KitOps documentation](https://kitops.org/docs/overview/): ModelKit and Kitfile reference.

- [OpenAI Agents SDK](https://openai.github.io/openai-agents-python/): Agent framework used in the notebook.

<br />

---

<div align="center">

<a href="https://studio1hq.com">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/Studio1HQ/website/main/brand/studio1-logo-on-dark.svg" />
    <img src="https://raw.githubusercontent.com/Studio1HQ/website/main/brand/studio1-logo-on-light.svg" alt="Studio1" width="160" />
  </picture>
</a>

Built by **[Studio1](https://studio1hq.com)**, the technical content and DevRel partner for devtool teams. Made with [Jozu](https://jozu.com).<br />
Browse more open-source examples at [github.com/Studio1HQ](https://github.com/Studio1HQ).

[![Website](https://img.shields.io/badge/studio1hq.com-0b0807?style=for-the-badge&logo=googlechrome&logoColor=white)](https://studio1hq.com)
[![Book a call](https://img.shields.io/badge/Book_a_strategy_call-f97316?style=for-the-badge&logo=googlecalendar&logoColor=white)](https://cal.com/studio1/collab)
[![X](https://img.shields.io/badge/%40Studio1HQ-141414?style=for-the-badge&logo=x&logoColor=white)](https://x.com/Studio1HQ)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-0a66c2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/company/studio1hq/)

<sub><a href="https://studio1hq.com">studio1hq.com</a> · Technical content, docs, video and DevRel for devtools</sub>

</div>
