# AGENTS.md

Personal fork of Ed Donner's LLM Engineering course (`ed-donner/llm_engineering`).
I work through the labs notebook by notebook. Notes below are how I like to work.

## Repo and git
- `origin` = my fork (`tjtoed/llm_engineering`), `upstream` = Ed's repo.
- Workflow cheat sheet is in `git-commands`. Follow it.
- **Never edit Ed's original notebooks.** Copy first: `cp week4/dayN.ipynb week4/dayN_mine.ipynb`
  (same folder, because notebooks use relative paths like `../assets`). Work in the copy.
- Stage files by name, never `git add .`.
- Ask before committing or pushing. Don't commit unless I say so.
- `week4/main.cpp` and `week4/main` are generated and gitignored.

## How to work with me
- **One cell at a time, in notebook order.** Propose the change to a cell, apply it, then wait.
  Don't jump ahead to later cells or batch changes unless I say "do all of them".
- Prefer commenting code out over deleting it, unless I say remove.
- Notebook edits: re-read the notebook before editing after I've run cells (the file changes on disk).
- When I paste timings or output, check which model/cell it came from. Ask if unlabeled.
- Verify results, not just speed: compare program output against the Python baseline's result.
- Keep costs low. Prefer cheap or flash models. Flag price before using anything expensive.

## Models and API
- All LLM calls go through **LiteLLM + OpenRouter**: `from litellm import completion`.
- Key: `OPENROUTER_API_KEY` in `.env` (gitignored). Pass `api_key=openrouter_api_key` explicitly.
- Model IDs look like `openrouter/<vendor>/<model>`, e.g. `openrouter/deepseek/deepseek-v4.1-flash`.
- The course material is from 2025. Model names go stale. Check availability and price against
  `https://openrouter.ai/api/v1/models` before assuming a model exists (Grok 4, Gemini 3 Pro Preview gone).
- **My OpenRouter account has Zero Data Retention (ZDR) enforced.** A model with no ZDR provider
  returns 404 "ZDR violation". Check `https://openrouter.ai/api/v1/endpoints/zdr` first.
  Known not ZDR: `qwen/qwen3.8-flash`. Don't suggest changing the privacy setting.
- Reasoning is set per model via `extra_body={"reasoning": {...}}` (`{"effort": "low"}` or
  `{"enabled": False}`), because LiteLLM doesn't know the newer models' parameters.
- Harmless noise: LiteLLM prints `Provider List: https://docs.litellm.ai/docs/providers` for unknown models.
- Calls don't stream, so reasoning models can look hung for 1-3 minutes.

## Machine
- Apple M5 (arm64, macOS), Apple clang 21. Compile with `clang++ -std=c++20 -O3 -mcpu=native -flto=thin -DNDEBUG`.
- Python env is the repo `.venv` via `uv`. If a notebook spins forever, select the `.venv` kernel and restart it.
- Ed's hardcoded timings in notebooks won't match my machine. That's expected.
