<div align="center">

# 🎬 Generative AI Video Production & Multi-Channel Publishing Pipeline

### One topic in. One cinematic vertical video out. Published across three channels, tracked end to end.

An **n8n** workflow that turns a single video topic into an AI-generated 9:16 short, using **GPT-5-mini** for ideation, **GPT-4.1-mini** for cinematic prompt engineering and **VEO 3** for generation, then distributes the result to **TikTok**, **Instagram** and **YouTube** while tracking each run in **Google Sheets**.

<br>

![n8n](https://img.shields.io/badge/n8n-Orchestration-EA4B71?style=for-the-badge&logo=n8n&logoColor=white)
![OpenAI](https://img.shields.io/badge/OpenAI-GPT--5--mini%20%2B%20GPT--4.1--mini-412991?style=for-the-badge&logo=openai&logoColor=white)
![VEO 3](https://img.shields.io/badge/VEO%203-veo3__fast%20·%209:16-1A73E8?style=for-the-badge)
![Google Sheets](https://img.shields.io/badge/Google%20Sheets-Tracking-34A853?style=for-the-badge&logo=googlesheets&logoColor=white)

![TikTok](https://img.shields.io/badge/TikTok-Blotato-000000?style=for-the-badge&logo=tiktok&logoColor=white)
![Instagram](https://img.shields.io/badge/Instagram-Blotato-E4405F?style=for-the-badge&logo=instagram&logoColor=white)
![YouTube](https://img.shields.io/badge/YouTube-Direct%20Upload-FF0000?style=for-the-badge&logo=youtube&logoColor=white)

<sub>📦 Portfolio-ready n8n workflow export · inactive by default · requires external service configuration before deployment</sub>

</div>

---

## ✨ What This Automation Does

A user enters **one video topic** in an n8n form. From there the workflow handles the entire production chain:

- 💡 an AI agent develops **one** original short-form concept, with caption, environment and sound direction
- 📊 the idea metadata is logged to a Google Sheets production record
- 🧠 a second AI agent expands the idea into a **structured cinematic prompt**
- 🎥 the prompt is submitted to **VEO 3** (`veo3_fast`) in **9:16** vertical format
- ⏳ the pipeline waits, then retrieves the finished video URL by task ID
- 📡 the video is published to **YouTube** (direct upload) and to **TikTok** and **Instagram** (through Blotato)
- ✅ the Google Sheets record is updated at the end of the run

This is not a single "text to video" call. It is an orchestrated pipeline with ideation, structured prompt engineering, external job handling, asset tracking and distribution.

---

## 🧭 Project Snapshot

| 🧩 Area | ⚙️ Implementation |
|---|---|
| 🔄 Automation Engine | n8n |
| 🎯 Input | `Video Topic` (single required form field) |
| 💡 Creative AI | `gpt-5-mini` |
| 🧠 Prompt Engineering | `gpt-4.1-mini` |
| 🎥 Video Engine | VEO 3 via Kie AI endpoint, model `veo3_fast` |
| 📐 Output Format | `9:16` vertical |
| 📊 Tracking Layer | Google Sheets |
| 📡 Distribution | Blotato (TikTok, Instagram) + YouTube node (direct upload) |
| 🔌 Workflow Size | 22 nodes |
| 🚦 Export Status | `active: false` |

---

## 🖼️ Workflow Overview

<img width="800" height="332" alt="image" src="https://github.com/user-attachments/assets/60de615f-659e-47f6-9b18-4baa85e485d6" />


The canvas shows the full pipeline: the topic form and creative agent on the left, prompt construction and VEO 3 generation in the middle, and the tracking and publishing nodes on the right.

---

## 🚀 Core Capabilities

| 🎯 Capability | 🔍 What the workflow actually does |
|---|---|
| 🎯 **Topic-driven generation** | Accepts one required `Video Topic` through an n8n Form Trigger |
| 💡 **AI creative ideation** | Generates exactly one concept, kept directly relevant to the topic |
| 🧾 **Structured metadata** | Returns `Caption`, `Idea`, `Environment`, `Sound`, `Status` through an output parser |
| 🧠 **Cinematic prompt engineering** | Expands the idea into a full structured prompt schema (camera, lighting, VFX, audio, ending) |
| 🎥 **VEO 3 generation** | POSTs the prompt to the Kie AI VEO endpoint with `veo3_fast` and `9:16` |
| ⏳ **Rendering wait** | Pauses for 3 minutes before requesting the result |
| 🔗 **Result retrieval** | Queries the record-info endpoint with the task ID and extracts the first result URL |
| 📊 **Production tracking** | Appends and updates a Google Sheets row across the run |
| 📡 **Multi-channel publishing** | YouTube direct upload, plus TikTok and Instagram posts through Blotato |
| ✅ **Status updates** | Production column moves through `for production`, `done` and `Publish` |

---

## 🛠️ Tech Stack

| 🧩 Layer | 🛠️ Technology |
|---|---|
| 🔄 Orchestration | n8n (Form Trigger, AI Agent, HTTP Request, Wait, Code, Set, Google Sheets nodes) |
| 🤖 Language Models | OpenAI `gpt-5-mini`, OpenAI `gpt-4.1-mini` |
| 🧠 AI Support | Think tool, Structured Output Parser (two instances) |
| 🎥 Video Generation | VEO 3 (`veo3_fast`) called through the `api.kie.ai` endpoint |
| 📊 Tracking | Google Sheets (OAuth2) |
| 📡 Social Distribution | Blotato n8n node (media upload, TikTok, Instagram) |
| ▶️ Direct Publishing | n8n YouTube node (OAuth2) |
| 🔐 HTTP Auth | Generic HTTP header authentication for the Kie AI endpoints |

> 🔎 **Precision note:** `api.kie.ai` appears in the workflow as the HTTP API endpoint used to reach VEO 3. The workflow does not configure it as a model provider of its own.

### 🔌 Integration Table

| 🔌 Integration | 🎯 Purpose | 🧩 Workflow Role |
|---|---|---|
| OpenAI | AI generation | Creative ideation (`gpt-5-mini`) and prompt generation (`gpt-4.1-mini`) |
| Kie AI VEO endpoints | Video generation | `generate` submits the job, `record-info` retrieves the result |
| Google Sheets | Production tracking | Idea metadata, status and final video URL |
| Blotato | Distribution | Media upload, TikTok post, Instagram post |
| YouTube | Direct publishing | Uploads the downloaded video file |
| n8n | Orchestration | End-to-end control flow |

---

## 🎬 Creative Intelligence Layer

> 🎬 **Creative Layer**
> Converts a raw topic into a concise, cinematic short-form concept.

The **Generate Creative Video Idea** agent runs on **`gpt-5-mini`** (node: *LLM: Generate Raw Idea (GPT-5)*).

| 🧩 Aspect | 📋 Behavior in the workflow |
|---|---|
| 🎯 Output count | Exactly **one** idea per run |
| 🧭 Topic relevance | The idea must stay directly about the submitted topic |
| 📐 Orientation | Vertical 9:16 for TikTok, Instagram Reels and YouTube Shorts |
| 🎞️ Concept format | Explanation, story, reveal, experiment, fact, transformation, action or another fitting format; BEFORE/AFTER is not forced |
| ✍️ Idea | Maximum 15 words |
| 🌍 Environment | Maximum 20 words, matching the idea |
| 🔊 Sound | Maximum 15 words |
| 🏷️ Status | Always set to `for production` |
| 🧠 Review step | The agent has a **Think** tool (*Tool: Inject Creative Perspective (Idea)*) to review its own output |

The Think tool is an AI self-review step. It is not factual verification, moderation or human approval.

### 💬 Social Caption Generation

The caption is generated in the same stage. The prompt specifies a short, punchy caption with **one emoji** and **exactly 12 lowercase hashtags**: 4 topic-relevant, 4 all-time popular, and 4 currently trending hashtags. The prompt instructs the model to incorporate currently trending hashtags based on live research. The workflow itself does not guarantee trend accuracy.

### 🧾 Structured Output

**Parse AI Output (Idea, Environment, Sound)** is a Structured Output Parser attached to the agent. Its example schema defines an array item with `Caption`, `Idea`, `Environment`, `Sound` and `Status`.

---

## 🧠 Prompt Engineering Engine

> 🤖 **Prompt Engineering Layer**
> Converts the idea into structured instructions for VEO 3.

Two nodes work together here:

1. **Set Master Prompt** stores a full JSON prompt template as `json_master`, along with `model = veo3_fast` and `aspectRatio = 9:16`.
2. **AI Agent: Generate Video Script** (`gpt-4.1-mini`) receives the saved `idea`, `environment_prompt` and `sound_prompt`, then writes a `final_prompt` that preserves the full master structure.

The agent is instructed to explicitly cover a strong **opening/hook**, the **main visual action or story**, **camera movement**, **lighting**, **color palette**, **style**, **sound**, **mood**, **motion** and an **ending shot**. It also has its own Think tool for self-review.

It generates prompt text only. It does not generate the video.

### 🎥 Cinematic Prompt Schema

| 🎬 Field | 🎯 Role |
|---|---|
| `description` | Brief narrative description of the scene |
| `style` | Visual treatment (cinematic, photorealistic, stylized, gritty, elegant) |
| `camera` | `type`, `movement`, `lens` |
| `lighting` | `type`, `sources`, `FX` |
| `environment` | `location`, `set_pieces`, `mood` |
| `elements` | Main physical items in the scene |
| `subject` | `character` (description, pose, lip-sync line) and `product` (brand, model, action) |
| `motion` | `type` and step-by-step `details` of the visual flow |
| `VFX` | `transformation`, `impact`, `particles`, `environment` |
| `audio` | `music`, `sfx`, `ambience`, `voiceover` (delivery, line) |
| `ending` | Final shot description |
| `text` | Text treatment (none, overlay, tagline, logo pulse) |
| `format` | Output format specification |
| `keywords` | Reinforcing keywords (scene style, motion type, camera style, sound mood, theme) |

### 🧪 Why a structured prompt matters

- Every run follows the same prompt skeleton, so downstream steps receive predictable input.
- Camera, lighting, motion, audio and ending are specified explicitly instead of left to chance.
- Unused fields are kept (as `none`, empty string or empty array) so the structure stays complete.
- Output is machine-readable, which suits automation.

A structured prompt improves control but does not guarantee visual consistency across generations.

<details>
<summary><b>🔍 Prompt agent output contract</b></summary>

<br>

The video prompt agent is attached to a second **Structured Output Parser** whose example schema is:

| Key | Type | Meaning |
|---|---|---|
| `title` | string | Short descriptive title (max 15 words per the system prompt) |
| `final_prompt` | string | Stringified JSON following the master prompt structure |

The system prompt requires valid JSON only, with no markdown or explanations, and requires inner quotes in `final_prompt` to be escaped.

</details>

---

## 🎥 VEO 3 Generation

| 🧩 Generation Stack | ⚙️ Value |
|---|---|
| 🧱 Prompt formatter | **Format Prompt** (Code node) |
| 🌐 Generation endpoint | `POST https://api.kie.ai/api/v1/veo/generate` |
| 🎬 Model | `veo3_fast` |
| 📐 Aspect ratio | `9:16` |
| 🆔 Job handle | `data.taskId` from the generation response |
| ⏳ Delay | **Wait for VEO3 Rendering**, 3 minutes |
| 🔗 Result endpoint | `GET https://api.kie.ai/api/v1/veo/record-info` with the `taskId` query parameter |
| 📦 Extracted result | `data.response.resultUrls[0]` |

**Format Prompt** takes the agent's `final_prompt`, escapes it with `JSON.stringify`, and sets `model` and `aspectRatio` to produce a generation-ready payload. The request is authenticated with an HTTP header credential.

> ⏳ The 3-minute wait is an automation delay, not a rendering guarantee. The workflow does not claim a fixed duration, resolution or success rate.

---

## 📊 Production Control Layer

Google Sheets acts as a lightweight production tracking layer. It is not a database or an analytics platform.

| 🗂️ Column | 📝 Written by | 🎯 Content |
|---|---|---|
| `id` | Save Idea & Metadata | Row-based identifier (`ROW()-1`) |
| `idea` | Save Idea & Metadata | Generated concept (also used as the match key for later updates) |
| `caption` | Save Idea & Metadata | Caption with hashtags |
| `production` | Save, URL Final Video, Update Status | Run status |
| `environment_prompt` | Save Idea & Metadata | AI-generated environment direction |
| `sound_prompt` | Save Idea & Metadata | AI-generated sound direction |
| `final_output` | URL Final Video | Final generated video URL |

### ✅ Status values represented in the workflow

| 🚦 Value | 📍 Set at |
|---|---|
| `for production` | Idea creation (value produced by the creative agent) |
| `done` | **URL Final Video**, after the video URL is retrieved |
| `Publish` | The final **Update Status to "DONE"** node |

The last node is named *Update Status to "DONE"* but writes the value `Publish`. This is a simple status progression across Google Sheets updates, not a formal state machine.

---

## 📡 Multi-Channel Publishing

> 📡 **Distribution Layer**
> Routes the generated video into configured social publishing destinations.

| 📡 Platform | 🔧 Publishing Path | 🎯 Content Sent |
|---|---|---|
| ▶️ YouTube | n8n YouTube node (direct upload) | Downloaded video file + title (idea) + description (caption) |
| 🎵 TikTok | Blotato node | Caption + media URL, with `postCreateTiktokOptionIsAiGenerated: true` |
| 📸 Instagram | Blotato node | Caption + media URL |

### Publishing details

- **YouTube:** **Download Final Video for YouTube** fetches the video as a file, then **YouTube (Direct Upload)** uploads it. The configured options are `categoryId: 22`, `regionCode: US`, `privacyStatus: public`, `publicStatsViewable: true`, `embeddable: true`, `notifySubscribers: false` and `selfDeclaredMadeForKids: false`. The title comes from the generated idea and the description from the generated caption.
- **Blotato media upload:** **Upload Video to BLOTATO** uploads the final video URL as media and returns a hosted media URL.
- **TikTok:** The Blotato TikTok node posts the caption with the uploaded media URL and flags the post as AI-generated.
- **Instagram:** The Blotato Instagram node posts the caption with the uploaded media URL.

The workflow does not configure scheduling, analytics, comments, thumbnails, playlists, tags or subtitles.

---

## 🛠️ Technical Architecture

| 🧩 Component | 🛠️ Node / Technology | 🎯 Responsibility |
|---|---|---|
| Topic Intake | `Trigger: Enter Video Topic` (Form Trigger) | Capture the required `Video Topic` |
| Creative Agent | `Generate Creative Video Idea` (AI Agent) | Generate one short-form concept |
| Creative LLM | `LLM: Generate Raw Idea (GPT-5)` | `gpt-5-mini` powering the idea agent |
| Creative Review | `Tool: Inject Creative Perspective (Idea)` (Think) | Self-review of the idea output |
| Metadata Parser | `Parse AI Output (Idea, Environment, Sound)` | Structure caption, idea, environment, sound, status |
| Idea Tracker | `Save Idea & Metadata to Google Sheets` (append) | Create the production row |
| Prompt Template | `Set Master Prompt` (Set) | Store master schema, model and aspect ratio |
| Prompt Agent | `AI Agent: Generate Video Script` | Produce `title` + `final_prompt` |
| Prompt LLM | `OpenAI Chat Model` | `gpt-4.1-mini` powering the prompt agent |
| Prompt Review | `Think` | Self-review of the prompt output |
| Prompt Parser | `Structured Output Parser` | Return `title` and `final_prompt` |
| Payload Builder | `Format Prompt` (Code) | Build the generation-ready payload |
| Video Generation | `Generate Video with VEO3` (HTTP) | Submit the VEO 3 job |
| Rendering Wait | `Wait for VEO3 Rendering` | 3-minute automation delay |
| Result Retrieval | `Download Video from VEO3` (HTTP) | Fetch job record by task ID |
| Output Tracking | `URL Final Video` (Sheets update) | Write `final_output`, set `done` |
| Direct Upload Prep | `Download Final Video for YouTube` (HTTP) | Fetch the video as a file |
| YouTube Upload | `YouTube (Direct Upload)` | Publish the video to YouTube |
| Media Upload | `Upload Video to BLOTATO` | Upload media for social posting |
| Social Posts | `TikTok`, `Instagram` (Blotato) | Publish captioned video posts |
| Final Tracking | `Update Status to "DONE"` (Sheets) | Set `production` to `Publish` |

---

## 🔄 Workflow Lifecycle

### 01 — 🎯 Topic Intake
The **Generate AI Video** form collects one required `Video Topic`.

### 02 — 💡 Creative Generation
The creative agent (`gpt-5-mini`) develops one short-form concept with a Think tool review.

### 03 — 🧾 Structured Metadata
The parser returns caption, idea, environment, sound and status.

### 04 — 📊 Idea Tracking
The metadata is appended to Google Sheets.

### 05 — 🎬 Master Prompt Setup
**Set Master Prompt** stores the cinematic schema, `veo3_fast` and `9:16`.

### 06 — 🧠 Video Script Generation
The prompt agent (`gpt-4.1-mini`) returns `title` and `final_prompt`.

### 07 — ⚙️ Prompt Formatting
**Format Prompt** escapes the prompt and builds the generation payload.

### 08 — 🎥 VEO 3 Generation
The payload is POSTed to the VEO generation endpoint, which returns a task ID.

### 09 — ⏳ Rendering Wait
The workflow waits 3 minutes.

### 10 — 🔗 Result Retrieval
The record-info endpoint is queried with the task ID.

### 11 — 📊 Record Update
The first result URL is written to `final_output` and `production` is set to `done`.

### 12 — ▶️ YouTube Upload
The video is downloaded as a file and uploaded directly to YouTube.

### 13 — 📡 Blotato Distribution
The video URL is uploaded to Blotato, then posted to TikTok and Instagram.

### 14 — ✅ Final Status
**Update Status to "DONE"** sets `production` to `Publish` on the matching row.

<details>
<summary><b>🔍 Source-accurate execution order</b></summary>

<br>

In the exported connections, publishing is sequential: **YouTube upload** runs first, followed by **Upload Video to BLOTATO**, then **TikTok**, then **Instagram**, then the final Sheets update. The three channels are not published in parallel branches.

</details>

---

## ⚙️ Configuration

> 🧩 **Workflow implementation** lives in the JSON. **Credentials and identifiers required for deployment** do not, and you must supply them.

| 🔧 Item | 📌 State in the export | ✅ What you need to do |
|---|---|---|
| Google Sheets document ID | Empty placeholder in all four Sheets nodes | Select your own spreadsheet |
| Google Sheets sheet name | Empty placeholder | Select the target sheet and create the columns listed above |
| Google Sheets credential | OAuth2 credential reference | Connect your Google account |
| OpenAI credential | Referenced by both chat models | Add your OpenAI API credential |
| Kie AI / VEO credential | HTTP header auth reference | Create a header auth credential with your API key |
| Blotato credential | Referenced by upload, TikTok and Instagram nodes | Add your Blotato API credential |
| TikTok account | Empty placeholder | Select your connected TikTok account |
| Instagram account | A hard-coded account handle is present in the export | Replace it with your own connected account |
| YouTube credential | OAuth2 credential reference | Connect your YouTube channel |

Before deploying, also review the YouTube settings, since the export sets `privacyStatus` to `public`.

---

## 🧪 Testing

These are suggested validation steps. They have **not** been executed as part of this repository.

| # | 🧪 Check |
|---|---|
| 1 | Submit a simple educational topic |
| 2 | Submit a visual storytelling topic |
| 3 | Confirm `Caption`, `Idea`, `Environment`, `Sound` and `Status` are generated |
| 4 | Confirm the idea row appears in Google Sheets |
| 5 | Confirm `title` and `final_prompt` are generated |
| 6 | Confirm the VEO 3 generation call returns a task ID |
| 7 | Confirm the final video URL is retrieved after the wait |
| 8 | Confirm `final_output` and `production = done` are written to the sheet |
| 9 | Confirm YouTube, TikTok and Instagram nodes receive the expected media and caption data |
| 10 | Confirm the final status update to `Publish` |

### 📝 Example Input

**Video Topic:** `How black holes work in 30 seconds`

This is the example shown in the form's placeholder text, not a recorded production result. No generated video is claimed.

---

## 🔐 Security Notes

- Keep API keys and OAuth tokens in **n8n credentials** or environment configuration.
- Never commit private credentials or tokens to the repository.
- Replace account identifiers and Google Sheets placeholders with your own values, and avoid publishing real account handles unnecessarily.
- Use least-privilege access for connected accounts where possible.
- Review the public YouTube privacy setting before activating the workflow.



---

## 📁 Repository Structure

| 📂 Path | 📝 Description |
|---|---|
| `README.md` | Project documentation |
| `Generative AI Video Production & Multi-Channel Publishing Pipeline.json` | Exported n8n workflow |
| `screenshots/workflow-overview.png` | Workflow canvas screenshot |

---

## 🏁 Final Takeaway

This project shows how **n8n** can coordinate **AI ideation**, **structured prompt engineering**, **VEO 3 video generation**, **asset tracking** and **multi-channel publishing** in a single workflow. It demonstrates practical work with agent orchestration, structured output handling, external API job retrieval, Google Sheets tracking, Blotato distribution and YouTube upload, all grounded in the nodes of the exported workflow.

<div align="center">

<sub>🎬 Creative AI · ⚙️ Workflow Automation · 📡 Multi-Channel Publishing · 📊 Production Tracking</sub>

</div>
