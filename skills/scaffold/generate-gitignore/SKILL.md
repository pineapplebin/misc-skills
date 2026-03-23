---
name: scaffold-generate-gitignore
description: Generates .gitignore via the toptal gitignore API by detecting project tech stack and merging language, OS, and IDE templates. Supports repo root or a target directory (e.g. monorepo subproject). Use when the user asks to generate, create, or update a gitignore or .gitignore file.
---

# Generate .gitignore

## External service

Uses **toptal** gitignore API. You pass **tags** (language, OS, IDE); the API returns merged ignore rules. Do not hand-write or manually edit .gitignore, always fetch from the API and write the response to the chosen target path.

## Workflow

0. **Determine target directory**  
   - If the user specifies a path (e.g. "for `packages/web`", "in the frontend folder", "for apps/api"), use that directory as the **target**. Resolve it relative to the repo root (e.g. `packages/web`, `apps/backend`).  
   - If no path is given, use the **repo root** as the target.  
   - Write the generated file to `{target}/.gitignore`. For repo root, that is `.gitignore`; for a subproject, e.g. `packages/web/.gitignore`.

1. **Detect project tech stack**  
   Infer from the **target directory** (not necessarily repo root): look for config files in that directory (e.g. `package.json` → node, `pyproject.toml` or `requirements.txt` → python, `pubspec.yaml` → flutter, `Cargo.toml` → rust, `go.mod` → go). Pick the **single primary** tag that matches; if multiple apply, prefer the main runtime (e.g. python for a Python app with some Node tooling). If the target has no clear stack, fall back to the repo root to infer, or ask the user.

2. **Fetch all available tags**  
   GET `https://www.toptal.com/developers/gitignore/api/list`  
   Response is plain text: multiple lines, comma-separated tag names (no spaces). Find the tag that matches the detected stack; use exact tag names from this list.  
   **List response example (excerpt):**
   ```
   lilypond,linux,lithium,localstack,logtalk
   lsspice,ltspice,lua,lyx,macos
   ...
   nim,ninja,node,nodechakratimetraveldebug,nohup
   ...
   pydev,python,pythonvanilla,qml,qooxdoo
   ...
   visualstudiocode,vivado,vlab,vrealizeorchestrator,vs
   vue,vuejs,vvvv,waf,wakanda
   werckercli,windows,wintersmith,wordpress,wyam
   ...
   jetbrains+all,jetbrains+iml,jgiven,jigsaw,jmeter
   ```
   Match project type to one tag (e.g. `python`, `node`, `rust`, `go`, `flutter`, `macos`, `windows`, `linux`, `visualstudiocode`, `jetbrains+all`).

3. **Build the tag list**  
   - One language/runtime tag from step 2.  
   - OS: default **macos** unless the user specifies Windows/Linux (use **windows** or **linux**).  
   - IDE: default **visualstudiocode** and **jetbrains+all** unless the user specifies otherwise.

4. **Generate and write .gitignore**  
   GET `https://www.toptal.com/developers/gitignore/api/{tags}`  
   Replace `{tags}` with comma-separated tags, no spaces (e.g. `python,macos,visualstudiocode,jetbrains+all`).  
   Write the full response body to **`{target}/.gitignore`** (overwrite existing). Create the target directory if it does not exist.

## API reference

| Purpose              | URL                                                                 | Method |
|----------------------|---------------------------------------------------------------------|--------|
| List all tags        | `https://www.toptal.com/developers/gitignore/api/list`              | GET    |
| Get merged gitignore | `https://www.toptal.com/developers/gitignore/api/{tag1,tag2,...}`   | GET    |

Tags in the second URL must be comma-separated, no spaces. Use exact tag names from the list response.
