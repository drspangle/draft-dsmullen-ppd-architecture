# Template Operations

These notes summarize how this repository uses Martin Thomson's
`i-d-template` tooling. Keep this file focused on repeatable commands and
operational state; put draft-content work in `draft-work-log.md`.

## Sources Checked

- Template setup: <https://github.com/martinthomson/i-d-template/blob/main/doc/SETUP.md>
- Template submission workflow: <https://github.com/martinthomson/i-d-template/blob/main/doc/SUBMITTING.md>
- IETF Datatracker submission tool: <https://datatracker.ietf.org/submit/tool-instructions/>
- IETF Datatracker submission API: <https://datatracker.ietf.org/api/submission>

## Repository Shape

- Draft source: `draft-dsmullen-ppd-architecture.md`.
- Local Makefile includes `lib/main.mk`.
- If `lib/main.mk` is absent, the Makefile either:
  - initializes a `lib` submodule when `.gitmodules` points at `lib`;
  - symlinks `$(ID_TEMPLATE_HOME)` to `lib` when that variable points at a
    local template checkout; or
  - clones `https://github.com/martinthomson/i-d-template` into `lib`.
- Generated render outputs are intentionally ignored by git, including
  `*.html`, `*.txt`, `*.pdf`, `draft-dsmullen-ppd-architecture.xml`,
  `versioned/`, `.venv/`, `.gems/`, `node_modules/`, and `lib/`.
- Note-only changes under `internal-notes/` are ignored by the editor-copy
  workflow trigger.

## Toolchain

The upstream setup guidance assumes a POSIX environment. On Windows, use WSL
with Ubuntu for local builds.

Minimum local tools:

- `git`
- GNU `make`
- `python3`, `pip`, and `venv`
- `ruby`, `gem`, and `bundler`

Recommended local tools:

- `npm` and Node.js, needed when a draft has Node-managed dependencies such as
  `aasvg`
- `libxml2-utils`, useful for XML inspection
- `curl` and `jq`, useful for Datatracker API calls and JSON status checks

Optional tools:

- `mmark`, only when the draft source uses mmark syntax with files beginning
  with `%%%`
- `aasvg@0.4.3`, only when using aasvg; upstream warns not to use newer aasvg
  versions with xml2rfc

Observed on this machine on 2026-05-05 before WSL setup completed:

- Native PowerShell has `git`, `node`, `npm.cmd`, and `curl.exe`.
- Native PowerShell did not expose `make`, `python`, `ruby`, `gem`,
  `bundler`, `jq`, or `gh`.
- `wsl.exe` is present, but no WSL distribution is installed yet.
- A bundled Codex Python exists at
  `C:\Users\Daniel Smullen\.cache\codex-runtimes\codex-primary-runtime\dependencies\python\python.exe`,
  but this does not satisfy the full template environment by itself because
  GNU make and Ruby are still missing.
- PowerShell maps `curl` to `Invoke-WebRequest`; use `curl.exe` when following
  curl examples from Datatracker docs.
- PowerShell blocks `npm.ps1` under the current execution policy; use
  `npm.cmd` from PowerShell or run npm inside WSL.
- This repository currently has no `package.json`, no apparent mmark source
  marker (`%%%`), and no aasvg references, so Node/npm, mmark, and aasvg are not
  required for the current draft unless future edits add those features.

## Local Dependency Test Results

Tested on 2026-05-05 from native PowerShell, without running `make upload`,
`make publish`, `make next`, tag pushes, workflow dispatches, or Datatracker API
calls.

Passed:

- `git --version`: `git version 2.54.0.windows.1`
- `node --version`: `v24.14.0`
- `npm.cmd --version`: `11.9.0`
- `curl.exe --version`: `curl 8.19.0 (Windows)`

Missing or blocked:

- `make`: not recognized; local render cannot start.
- `python`, `python3`, and `pip`: not found on PATH.
- `ruby`, `gem`, `bundle`, and `bundler`: not found on PATH.
- `jq`, `xmllint`, and `gh`: not found on PATH.
- `wsl -l -v`: reports WSL is not installed because no Linux distribution is
  installed.
- `npm --version`: blocked by PowerShell execution policy because it resolves
  to `npm.ps1`; `npm.cmd --version` works.
- `curl --version`: resolves to PowerShell `Invoke-WebRequest`; `curl.exe`
  works.
- `wsl --install -d Ubuntu`: still reports WSL is not installed.
- Targeted checks did not find Docker, Podman, conda, uv, Scoop, Chocolatey,
  Winget, MSYS2, Cygwin, Ruby, or system Python.

Additional setup attempted on 2026-05-05:

- Enabled `Microsoft-Windows-Subsystem-Linux`.
- Enabled `VirtualMachinePlatform`; DISM returned exit code `3010`, meaning a
  reboot is needed.
- Installed WSL package version `2.6.3` using `wsl.exe --install -d Ubuntu
  --no-launch`.
- `wsl --version` now reports WSL `2.6.3.0` and kernel `6.6.87.2-1`.
- `wsl --status` and `wsl -l -v` currently fail with
  `Wsl/EnumerateDistros/Service/E_ACCESSDENIED`, and the WSL installer log
  says changes will not be effective until reboot.

Post-reboot result observed on 2026-05-06:

- WSL reports default version `2`.
- Ubuntu is installed as a WSL2 distribution and can run non-interactively from
  the repository path.
- Ubuntu currently launches as `root` in this automation context. That is
  acceptable for local rendering, but do not use repository, GitHub, or
  Datatracker credentials as Linux account credentials if an interactive user
  account is later created.
- Installed the Ubuntu package dependencies needed for local rendering:
  `git`, `make`, `python3-pip`, `python3-venv`, `ruby`, `ruby-bundler`, `npm`,
  `libxml2-utils`, `curl`, and `jq`.
- `internal-notes/scripts/check-template-env.sh` passes.
- The first local render populated ignored template dependencies under `lib/`
  and generated the editor-copy HTML and text outputs.
- The render needed `BUNDLE_PATH` to be an absolute Ubuntu path because Bundler
  otherwise looked in the wrong local gem directory when running
  `kramdown-rfc` from this Windows-mounted path. The local render script now
  exports that path before running `make`.

Result: local environmental dependencies are now satisfied for rendering the
draft with the template in Ubuntu WSL.

## Post-Reboot Resume Plan

After rebooting Windows, resume from the repository root in PowerShell:

```powershell
.\internal-notes\scripts\post-reboot-wsl-check.ps1
```

If Ubuntu is not listed by `wsl -l -v`, finish the distro install:

```powershell
wsl --install -d Ubuntu
```

If Ubuntu launches for first-time setup, create the Linux user account when
prompted. Do not use repository, GitHub, or Datatracker credentials for that
Linux account.

Then install the i-d-template package dependencies inside Ubuntu:

```powershell
wsl -d Ubuntu -- bash -lc "cd /mnt/c/Users/Daniel\ Smullen/Documents/draft-dsmullen-ppd-architecture && bash internal-notes/scripts/install-ubuntu-template-deps.sh"
```

Check the installed commands:

```powershell
wsl -d Ubuntu -- bash -lc "cd /mnt/c/Users/Daniel\ Smullen/Documents/draft-dsmullen-ppd-architecture && bash internal-notes/scripts/check-template-env.sh"
```

Render the local editor's copy:

```powershell
wsl -d Ubuntu -- bash -lc "cd /mnt/c/Users/Daniel\ Smullen/Documents/draft-dsmullen-ppd-architecture && bash internal-notes/scripts/render-local-editor-copy.sh"
```

The render script only runs `make`. It does not run `make upload`,
`make publish`, `make next`, tag commands, workflow dispatches, or Datatracker
API calls.

Generate a local HTML diff against the most recent prior tagged draft version:

```powershell
wsl -d Ubuntu -- bash -lc "cd /mnt/c/Users/Daniel\ Smullen/Documents/draft-dsmullen-ppd-architecture && BUNDLE_PATH=/mnt/c/Users/Daniel\ Smullen/Documents/draft-dsmullen-ppd-architecture/lib/.gems make diff"
```

Expected output:

- `diff-draft-dsmullen-ppd-architecture.html`

Observed on 2026-05-06:

- `make diff` compared the current working draft against local tag
  `draft-dsmullen-ppd-architecture-04`
- the generated redline treated the working draft as `-05` for display and
  produced `diff-draft-dsmullen-ppd-architecture.html`

## First Local Setup on Windows

Run this from an elevated Windows prompt if WSL is not installed:

```powershell
wsl --install -d Ubuntu
```

Then open Ubuntu and install the template prerequisites:

```sh
sudo apt-get update
sudo apt-get install -y git make python3-pip python3-venv
sudo apt-get install -y ruby-bundler npm libxml2-utils curl jq
```

From Ubuntu, enter the repository under `/mnt/c/...` or clone it into the Linux
filesystem. Building inside the Linux filesystem is usually faster and avoids
some Windows filesystem edge cases.

## Rendering the Draft

Build the normal editor's-copy outputs from PowerShell using the local wrapper:

```powershell
wsl -d Ubuntu -- bash -lc "cd /mnt/c/Users/Daniel\ Smullen/Documents/draft-dsmullen-ppd-architecture && bash internal-notes/scripts/render-local-editor-copy.sh"
```

From Ubuntu, the equivalent direct command is:

```sh
BUNDLE_PATH="$PWD/lib/.gems" make
```

The first run will populate `lib/` and install Python/Ruby dependencies under
ignored local directories. To refresh the template-managed tool dependencies:

```sh
make update-deps
```

The default build should create the rendered editor's-copy files, including
HTML and text outputs, from `draft-dsmullen-ppd-architecture.md`. Generated
outputs are ignored locally because GitHub Actions publishes them.

To build a local redline against the most recent tagged version:

```sh
BUNDLE_PATH="$PWD/lib/.gems" make diff
```

This should create:

```text
diff-draft-dsmullen-ppd-architecture.html
```

If template-generated repository files need to be regenerated after renaming
drafts, changing editors, or changing titles, run:

```sh
make update-files
```

Be careful with `make update-files`: the template notes that it can overwrite
customizations to generated files such as `README.md`, `CONTRIBUTING.md`,
`.github/CODEOWNERS`, and `Makefile`.

## GitHub Pages Editor's Copy

Workflow: `.github/workflows/ghpages.yml`.

On pushes and pull requests that touch functional draft content, GitHub Actions
runs the template action to build the draft. On pushes, it also runs the
template `gh-pages` target to update the published editor's copy.

The README points at:

- Editor's copy:
  <https://drspangle.github.io/draft-dsmullen-ppd-architecture/#go.draft-dsmullen-ppd-architecture.html>
- Editor's-copy diff:
  <https://drspangle.github.io/draft-dsmullen-ppd-architecture/#go.draft-dsmullen-ppd-architecture.diff>

## Datatracker Publishing by Tag

Workflow: `.github/workflows/publish.yml`.

The normal automated path is:

```sh
git push origin main
git tag -a draft-dsmullen-ppd-architecture-05 -m "Submit draft-dsmullen-ppd-architecture-05"
git push origin draft-dsmullen-ppd-architecture-05
```

The tag name must be the full draft name with the numeric revision and no file
extension. Existing local tags show `-00` and `-04`, so `-05` is the next likely
submission number unless Datatracker or remote tags show otherwise.

Use an annotated tag so the submission can use the tagger email. The chosen
email must be known to the IETF Datatracker account. If the email is wrong,
delete the tag locally and remotely, fix the issue, and retag before
resubmitting.

After the GitHub Action uploads the draft, Datatracker still requires the normal
confirmation email round trip before posting.

## Datatracker Publishing by GitHub Release

A GitHub release whose tag is named like
`draft-dsmullen-ppd-architecture-05` also triggers the publish workflow.
Upstream notes that release-triggered or lightweight-tag submissions are
attributed to the first author listed in the draft.

## Datatracker Publishing by Manual Workflow Dispatch

The `Publish New Draft Version` workflow supports `workflow_dispatch` with an
optional `email` input. Use that when a publish run needs to force
`UPLOAD_EMAIL` without committing it to the repository.

Do not commit `UPLOAD_EMAIL` or credentials.

## Datatracker Publishing from Local Make

Use this only if GitHub Actions is unavailable or unsuitable.

```sh
export UPLOAD_EMAIL="verified-address@example.com"
git tag -a draft-dsmullen-ppd-architecture-05 -m "Submit draft-dsmullen-ppd-architecture-05"
make publish
```

The upstream template describes `make publish` as the semi-automated local path
for publishing tagged drafts. This uses the same general upload flow as CI.

## Manual XML Generation and Upload

Generate the next versioned XML:

```sh
make next
```

The versioned XML should appear under `versioned/`, for example:

```text
versioned/draft-dsmullen-ppd-architecture-05.xml
```

Upload the XML, not the text rendering, at:

```text
https://datatracker.ietf.org/submit/
```

Before submitting, run the generated draft through idnits or the Datatracker
validation flow and review the extracted metadata carefully.

## Datatracker API

The Datatracker API accepts XML-only uploads with multipart form data and
requires a Datatracker account. A successful API submission still requires the
normal email confirmation round trip.

Example shape:

```sh
curl -s \
  -F "user=verified-address@example.com" \
  -F "xml=@versioned/draft-dsmullen-ppd-architecture-05.xml" \
  https://datatracker.ietf.org/api/submission | jq
```

The response includes a `status_url`. Poll that URL until the state is no
longer `validating`, then complete any Datatracker authentication or email
confirmation step.

## Failure Recovery

If a tagged publishing build fails because the draft content is broken:

```sh
git tag -d draft-dsmullen-ppd-architecture-05
git push origin :draft-dsmullen-ppd-architecture-05
```

Then fix the draft, push the branch, wait for the editor-copy build to pass,
and recreate the annotated tag.

If the upload reaches Datatracker but metadata is wrong, use the Datatracker
adjust/manual-posting flow or contact the appropriate IETF support path rather
than repeatedly submitting bad metadata.
