# Crescent Trace

**Find where your AI agent got stuck.**

Paste a failed agent trace. Crescent Trace finds the loop or the error, points to the exact step, tells you how many tokens were wasted, and suggests how to fix it. Everything runs in your browser: no install, no API key, and your trace is never uploaded.

**Live:** https://nischita44.github.io/crescent-trace/

## What it detects

- **Loops**: exact repeats, near-repeats (same tool, rephrased input), ping-pong between two tools, retries after errors
- **Tool calls that never returned** (for example, a tool that raised and crashed the run)
- **Runs cut off by a framework step limit**
- **Token waste**: tokens spent inside each loop, as a share of the whole run

Each failure is sorted into one of five buckets (recovery policy, tool execution, tool selection, tool arguments, state orchestration) with a fix checklist for that bucket.

## Supported trace formats

- OpenTelemetry spans (OpenInference or `gen_ai.*` attributes): Arize Phoenix, Langfuse v3+, smolagents
- LangChain / LangGraph message dumps (`model_dump()`)
- OpenAI-style `messages[]` with `tool_calls`
- Generic JSON / JSONL event logs

## How well it works

Tested against the [TRAIL benchmark](https://github.com/patronus-ai/trail-benchmark) (148 human-labeled agent traces from GAIA and SWE-bench). On the error types it is built for (tool failures and resource abuse), Crescent Trace places about **half** of the labeled errors at the correct agent step, with no LLM.

It does **not** judge reasoning mistakes, such as picking the wrong tool on purpose or misreading a correct result. Read the timeline before you trust the verdict.

## Design choice: rules first, AI second

Most agent failures that burn money are mechanical: loops, retry storms, crashed tools. Rules catch those instantly, for free, deterministically, and without sending production logs anywhere.

The version hosted on Claude adds an optional **"Ask the dragon"** button. It sends only the flagged steps (not the whole trace) to Claude for a second opinion on the root cause and on reasoning errors the rules cannot see. Trace content is passed as data, so instructions inside a trace cannot steer the model. On this standalone site that section hides itself; the rule-based diagnosis works fully.

## Run it locally

It is a single file. Download `index.html` and open it in a browser.

## Built with

Plain HTML, CSS and JavaScript, plus [Three.js](https://threejs.org/) for the 3D dragon and trail.

Built by [Nischitha Sadananda](https://www.linkedin.com/in/nischitha-sadananda).
