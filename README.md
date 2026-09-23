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
   explorer    implementation roles  researcher
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

## Setup

1. Clone this repository and enter it:

   ```sh
   git clone https://github.com/FrankHeels/codex-astra-luna-orchestrator.git
   cd codex-astra-luna-orchestrator
   ```

2. Run the installer for your platform:

   macOS/Linux:

   ```sh
   ./setup.sh
   ```

   Windows PowerShell:

   ```powershell
   powershell -ExecutionPolicy Bypass -File .\setup.ps1
   ```

   PowerShell 7:

   ```powershell
   pwsh -File .\setup.ps1
   ```

3. When prompted, enter an **existing target repository other than this one**,
   choose a profile by number or name (Enter selects Pro), and confirm which
   components to install. For example:

   ```text
   Target repository path: ../my-project
   Select Profile [1-6] (default 1): 5
   ```

The installer copies the selected configuration to `.codex/`, its skill to
`.agents/`, and project instructions to `AGENTS.md`. Existing component files
are updated only after confirmation; existing `AGENTS.md` content is preserved.
Codex loads project-scoped configuration only for trusted projects.

### Installed target project

If you install all three components into `../my-project`, the installer adds
these paths alongside the project's existing files:

```text
my-project/
├── .codex/
│   ├── config.toml
│   └── agents/
│       ├── explorer.toml
│       ├── backender.toml
│       ├── frontender.toml
│       ├── researcher.toml
│       ├── reviewer.toml
│       └── tester.toml
├── .agents/
│   └── skills/
│       └── astra-orchestrator/
│           └── SKILL.md
└── AGENTS.md
```

`profiles/<profile>/codex/` becomes `.codex/`, and
`profiles/<profile>/agents/` becomes `.agents/`. The root `AGENTS.md` is
copied to the target, or its instructions are appended if that file exists.
The tree above shows `GPT6-SolMax-LunaMax`, which replaces the generic
`worker.toml` with specialized `backender.toml` and `frontender.toml` roles.
The other profiles keep their original agent sets.

## How to use the skill

From the target repository, launch Codex CLI. For the example above:

```sh
cd ../my-project
codex
```

For complex work, Codex may select the skill automatically, or you can invoke
it explicitly:

```text
$astra-orchestrator
Implement the invoice export endpoint. Use the explorer to map the path,
the backender to implement it, and the tester and reviewer to verify it.
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
| 5 | `GPT6-SolMax-LunaMax` | Sol max | Luna max (`backender`, `frontender`) | Sol max | 4 |
| 6 | `GPT6-SolMedium-LunaMax` | Sol medium | Luna max | Sol medium | 4 |

All models above are GPT-6. `GPT6-SolMax-LunaMax` uses explorer, backender,
frontender, tester, and researcher. The other profiles retain explorer, worker,
tester, and researcher. Named roles pin their models and reasoning levels
independently of the default subagent settings. Each ready-to-copy profile lives
under `profiles/<profile>/`.

For manual project setup, copy the selected profile's `codex/` and `agents/`
to the target repository as `.codex/` and `.agents/`, and add this repository's
`AGENTS.md`. For personal/global setup, copy its `codex/agents/` into
`~/.codex/agents/`, its `agents/skills/astra-orchestrator/` into
`~/.agents/skills/`, and **merge**, rather than replace, its `codex/config.toml`
settings into `~/.codex/config.toml`. Do not overwrite other existing Codex
settings.

## Key directory structure

```text
.
├── profiles/
│   ├── pro/
│   ├── plus/
│   ├── pro-max-2-subagents/
│   ├── plus-max-2-subagents/
│   ├── GPT6-SolMax-LunaMax/
│   └── GPT6-SolMedium-LunaMax/
├── guides/
├── scripts/
│   └── token_usage.py
├── tests/
│   ├── test_profiles.py
│   └── test_token_usage.py
├── AGENTS.md
├── setup.sh
├── setup.ps1
├── README.md
└── LICENSE
```

Each profile contains `codex/config.toml`, `codex/agents/*.toml`, and
`agents/skills/astra-orchestrator/SKILL.md`.

## Guides

- [Pro orchestration and manual configuration](guides/full-orchestration.md)
- [Plus profile and global setup](guides/plus-plan.md)
- [Fast iteration](guides/fast-iteration.md) and [routine coding](guides/routine-coding.md)
- [Complex repository work](guides/complex-repo-work.md)
- [Token usage and measurement](guides/token-usage.md)

## License

Licensed under the [Apache License 2.0](LICENSE).
