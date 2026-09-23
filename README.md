# Codex Sol/Astra Orchestrator + Luna Subagents

Install a Codex profile with GPT-6 Astra, Sol, or Luna as the orchestrator,
GPT-6 Luna execution subagents, and an independent reviewer.

## Orchestration topology

The diagram shows the Astra (Pro) and Sol orchestrators. Plus uses a Luna
root, as shown in the profile table below. Execution roles use GPT-6 Luna;
the reviewer uses Astra for Pro/Plus and Sol for Sol profiles.

```text
              GPT-6 Astra / Sol
             root / orchestrator
                      |
      +---------------+---------------+
      |               |               |
   explorer          worker         researcher
  GPT-6 Luna       GPT-6 Luna       GPT-6 Luna
      |               |
      +-------+-------+
              |
           tester
         GPT-6 Luna
              |
          reviewer
      GPT-6 Astra / Sol
              |
              v
           root agent
      integrate + verify
```

## Quick start

Clone this repository, then run the installer for your platform:

```bash
git clone https://github.com/donvito/codex-astra-luna-orchestrator.git
cd codex-astra-luna-orchestrator
./setup.sh
```

On Windows, run `powershell -ExecutionPolicy Bypass -File .\setup.ps1` (or
`pwsh -File .\setup.ps1` with PowerShell 7).

Enter an existing target repository **other than this one**, choose a profile
by number or name, and confirm which components to install. The installer
copies the selected configuration to `.codex/`, its skill to `.agents/`, and
project instructions to `AGENTS.md`. Existing component files are updated
only after confirmation; existing `AGENTS.md` content is preserved. Codex
loads project-scoped configuration only for trusted projects.

Launch Codex from the target repository. For complex work, Codex may select
the skill automatically, or you can invoke it explicitly:

```text
$astra-orchestrator
Implement the invoice export endpoint. Use the explorer to map the path,
a worker to implement it, and the tester and reviewer to verify it.
```

The skill keeps the `astra-orchestrator` name in every profile so the shared
`AGENTS.md` works; the Sol profiles use Sol according to their configuration.

## Profiles

| Choice | Profile | Root | Execution roles | Reviewer | Concurrent subagents |
|---|---|---|---|---|---:|
| 1 (default) | `pro` | Astra medium | Luna max | Astra low | 4 |
| 2 | `plus` | Luna max | Luna medium | Astra low | 4 |
| 3 | `pro-max-2-subagents` | Astra medium | Luna max | Astra low | 2 |
| 4 | `plus-max-2-subagents` | Luna max | Luna medium | Astra low | 2 |
| 5 | `GPT6-SolMax-LunaMax` | Sol max | Luna max | Sol max | 4 |
| 6 | `GPT6-SolMedium-LunaMax` | Sol medium | Luna max | Sol medium | 4 |

All models above are GPT-6. Execution roles are explorer, worker, tester,
and researcher; named roles pin their models and reasoning levels independently
of the default subagent settings. Each ready-to-copy profile lives under
`profiles/<profile>/`.

For manual project setup, copy the selected profile's `codex/` and `agents/`
to the target repository as `.codex/` and `.agents/`, and add this repository's
`AGENTS.md`. For personal/global setup, copy its `codex/agents/` into
`~/.codex/agents/`, its `agents/skills/astra-orchestrator/` into
`~/.agents/skills/`, and **merge**, rather than replace, its `codex/config.toml`
settings into `~/.codex/config.toml`. Do not overwrite other existing Codex
settings.

## Guides

- [Pro orchestration and manual configuration](guides/full-orchestration.md)
- [Plus profile and global setup](guides/plus-plan.md)
- [Fast iteration](guides/fast-iteration.md) and [routine coding](guides/routine-coding.md)
- [Complex repository work](guides/complex-repo-work.md)
- [Token usage and measurement](guides/token-usage.md)

## License

Licensed under the [Apache License 2.0](LICENSE).
