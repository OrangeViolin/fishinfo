# fishinfo

`fishinfo` is a high-density meeting understanding skill for Codex-style AI agents.

It turns long transcripts, meeting notes, recorded-call text, and group chat discussions into two deliverables:

1. A structured Markdown meeting map.
2. A dense, browser-openable HTML infographic in the 01fish visual style.

The goal is not to make a pretty summary. The goal is to reconstruct the meeting so that a reader who never attended can still understand the topic flow, each participant's contribution, key cases, evidence, tensions, decisions, and unresolved questions.

## 中文简介

`fishinfo` 是一个用于处理高密度会议、聊天逐字稿和访谈记录的 Agent Skill。

它不是普通“会议摘要”工具，而是把一场复杂对话整理成一张可反复阅读的**会议理解地图**：

- 这场会从哪里开始，如何一步步转向更深的问题。
- 每个参与者说了什么、贡献了什么、观点如何变化。
- 哪些案例、故事、冲突和细节真正重要。
- 哪些是原话，哪些是整理者归纳，哪些只是开放问题。
- 最后留下了哪些方法、判断、未决问题和下一步。

它默认输出两份文件：

```text
[原文件名]_会议纪要.md
[原文件名]_信息图.html
```

适合用来整理：

- 公司会议
- 创业讨论
- 用户访谈
- 社群聊天
- 课程讨论
- 咨询沟通
- 多人头脑风暴
- 长录音转写后的逐字稿

### 中文快速安装

如果你使用 Codex，并且 Codex 会读取 `~/.codex/skills`：

```bash
mkdir -p ~/.codex/skills
git clone https://github.com/OrangeViolin/fishinfo.git ~/.codex/skills/fishinfo
```

安装后可以这样使用：

```text
$fishinfo

请把这个会议逐字稿整理成：
1. 完整 Markdown 会议纪要
2. 01fish 风格 HTML 信息图

文件路径：
/path/to/会议逐字稿.txt
```

如果你的 Agent 使用的是 `~/.agents/skills`：

```bash
mkdir -p ~/.agents/skills
git clone https://github.com/OrangeViolin/fishinfo.git ~/.agents/skills/fishinfo
```

### 中文使用示例

```text
$fishinfo

这是一场 3 小时、5 人参与的聊天会议。
我不想要简单摘要，我希望读者只看最终信息图，就能理解整场会议的主线、每个人的贡献、关键案例、方法和未决问题。

请输出：
- Markdown 会议纪要
- HTML 信息图

文件：
/Users/me/Documents/transcripts/sunroom.txt
```

如果你觉得上一次输出太薄，可以继续要求：

```text
$fishinfo

内容还是太少了。
请按时间段、发言人、议题、案例、互动关系和证据做覆盖审计。
尤其不要漏掉会议中段和后半段的转折。
请重写 Markdown 和 HTML。
```

## What It Does

`fishinfo` is designed for meetings where a short summary is not enough:

- Long strategy calls.
- Founder or team discussions.
- User interviews and research debriefs.
- Multi-person brainstorming sessions.
- Community or group-chat meetings.
- Emotionally rich conversations where people, stories, and turning points matter.

It emphasizes:

- Topic timeline instead of generic bullet summaries.
- Participant-specific observation without psychological labeling.
- Claims tied to transcript evidence.
- Concrete cases, conflicts, and reinterpretations.
- Interaction chains: who responded to whom, and how the discussion changed.
- Clear separation of decisions, suggestions, open questions, and organizer analysis.
- Long-form HTML infographics that preserve substance instead of compressing everything into slogans.

## Repository Structure

```text
fishinfo/
├── SKILL.md                         # Main agent instructions
├── README.md                        # Human-facing installation and usage guide
├── agents/
│   └── openai.yaml                  # Optional agent metadata
├── evals/
│   └── evals.json                   # Example evaluation prompts
├── references/
│   └── fishinfo-template.html       # 01fish-style infographic starter template
└── evolution-log.md                 # Design history and iteration notes
```

## Installation

### Option A: Install Into Codex Skills

Clone this repository into your Codex skills directory:

```bash
mkdir -p ~/.codex/skills
git clone https://github.com/OrangeViolin/fishinfo.git ~/.codex/skills/fishinfo
```

Restart your Codex session if your environment does not hot-reload skills.

You should then be able to call it with:

```text
$fishinfo
```

or by asking for meeting notes, a meeting infographic, or a high-density transcript analysis.

### Option B: Install Into an Agents Skill Directory

If your agent reads skills from `~/.agents/skills`, clone it there instead:

```bash
mkdir -p ~/.agents/skills
git clone https://github.com/OrangeViolin/fishinfo.git ~/.agents/skills/fishinfo
```

### Option C: Copy Manually

Download the repository and copy the full `fishinfo` folder into any skill root supported by your agent.

The important file is `SKILL.md`, but the `references/` template and eval examples should be kept with it.

## Basic Usage

Give your agent a transcript file and ask it to use `fishinfo`.

Example:

```text
$fishinfo

Please turn this transcript into:
1. a complete Markdown meeting map
2. an HTML infographic

File:
/path/to/meeting-transcript.txt
```

You can also write naturally:

```text
Please create a high-density meeting infographic from this transcript.
I want to understand the whole meeting, not just a short summary.
```

If the skill is installed correctly, the agent should read `SKILL.md` and follow the fishinfo workflow.

## Recommended Input Format

Plain text works best.

Good input:

```text
[00:00:04] Alice: I think the real problem is not traffic, but trust.
[00:01:20] Bob: Can you give an example?
[00:02:05] Alice: Last week a user told us...
```

Also acceptable:

```text
Alice: I think the real problem is not traffic, but trust.
Bob: Can you give an example?
Alice: Last week a user told us...
```

Supported source types depend on your agent environment, but the skill is written to handle:

- `.txt`
- `.md`
- `.docx` through local text extraction
- `.pdf` when a PDF text extraction tool is available
- pasted transcript text

## Expected Outputs

For an input file such as:

```text
weekly-strategy-call.txt
```

the skill expects the agent to create:

```text
weekly-strategy-call_会议纪要.md
weekly-strategy-call_信息图.html
```

The Markdown file should include:

- Basic meeting information.
- What really happened in the meeting.
- Topic timeline.
- Participant observations.
- Core topic map.
- Key case reconstruction.
- Methods and transferable frameworks.
- Disagreements, tensions, and unresolved issues.
- Real decisions and TODOs, only when they actually exist.
- Quote excerpts.
- Questions to track next time.

The HTML file should include:

- A 01fish-style visual layout.
- Dense cards with real substance.
- Timeline, people, topics, cases, methods, quotes, and open questions.
- Mobile-friendly responsive layout.
- Browser-openable single-page output.

## Quality Standard

`fishinfo` intentionally sets a high bar.

A good output should let a reader answer:

1. What started the meeting?
2. Which topics appeared, deepened, or changed direction?
3. What did each participant contribute?
4. Which stories, examples, and cases mattered?
5. Which claims are supported by transcript evidence?
6. What was decided, what was merely suggested, and what remains unresolved?
7. What methods or frameworks can be reused?

An output is considered weak if it only contains:

- A meeting title.
- Three generic takeaways.
- A few inspirational quotes.
- A pretty but shallow visual card.
- TODOs invented by the summarizer.

## Prompt Templates

### Full Meeting Map

```text
$fishinfo

Please process this transcript as a complete meeting understanding map.

Requirements:
- Do not reduce it to a short summary.
- Preserve topic transitions and participant contributions.
- Reconstruct key cases in detail.
- Separate direct quotes, meeting-level conclusions, and your analysis.
- Output both Markdown and HTML.

File:
/path/to/transcript.txt
```

### Dense Infographic Only

```text
$fishinfo

Create a dense 01fish-style HTML infographic from this transcript.
The reader should understand the full meeting by reading the infographic alone.

Please include:
- overview
- timeline
- participant observations
- topic map
- cases
- methods
- quotes
- open questions
```

### Improve a Thin Output

```text
$fishinfo

The previous version is too thin.
Please audit coverage by:
- time segment
- speaker
- topic
- case
- interaction chain
- evidence

Then revise the Markdown and HTML so the middle and later parts of the meeting are not lost.
```

## Design Philosophy

Most meeting summaries over-compress.

They flatten:

- people into roles
- stories into themes
- disagreements into fake consensus
- open questions into premature action items
- long meetings into executive summaries

`fishinfo` does the opposite. It treats a meeting as a living information structure:

```text
event → story → response → concept → disagreement → method → unresolved question
```

It preserves enough detail for the meeting to remain useful later.

## Notes on Privacy

Meeting transcripts may contain private information.

Before publishing generated outputs, review and remove:

- personal phone numbers
- addresses
- identity numbers
- private financial details
- client secrets
- sensitive internal company information
- any material you do not have permission to share

This skill is a workflow instruction set. It does not automatically anonymize all sensitive content unless your agent is explicitly asked to do so.

## License

MIT License. See `LICENSE`.
