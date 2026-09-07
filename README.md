<div align="center">

# ᛉ Yggdrasil

**A self-hosted home infrastructure that reads like a saga** — a repurposed 2-in-1 tablet turned into an immutable, reproducible, AI-tended home server.

![Fedora Silverblue](https://img.shields.io/badge/Fedora-Silverblue-51A2DA?logo=fedora&logoColor=white)
![Podman](https://img.shields.io/badge/Podman-rootless-892CA0?logo=podman&logoColor=white)
![Home Assistant](https://img.shields.io/badge/Home%20Assistant-Heimdallr-41BDF5?logo=homeassistant&logoColor=white)
![Python](https://img.shields.io/badge/Python-agents-3776AB?logo=python&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-green)

</div>

## The idea

**Yggdrasil** is the world-tree that holds my home together: home automation, media,
backups, and a small court of AI agents — all running on a single repurposed
**HP Elite x2** under **Fedora Silverblue**, an immutable, image-based OS.

Every service is a **rootless Podman** container. Every dotfile and config lives in
a **bare git repository** that can **bootstrap the whole machine from zero with one
command**. The day-to-day is driven through a **single Telegram channel** by an
agentic AI. And the naming is Norse — each service is a being from the myth, chosen
for what it does.

## The realms

**Home & media**

| Realm | Role | Stack |
|---|---|---|
| **Heimdallr** | The watchman — home automation & dashboards | Home Assistant |
| **Bragi** | The bard — personal media library | Jellyfin |

**The AI court**

| Realm | Role | Stack |
|---|---|---|
| **Ratatoskr** | The messenger — agentic AI over Telegram (text + voice) | Python · LLM · Whisper |
| **Kvasir** | The counselor — keeps my professional presence coherent | Python · LLM |
| **Huginn** | Thought — a sandboxed AI coding agent, one home per identity | Podman · Playwright |
| **Nidavellir** | The dwarves' forge — the dev toolbox the agents are built from | Toolbox · Podman |

**Keepers & network**

| Realm | Role | Stack |
|---|---|---|
| **Muninn** | Memory — automated encrypted backups | BorgBackup |
| **Draupnir** | The ring that drips gold — a small price watcher | Python |
| **Njord** | The sea-god — a private VPN network gateway | Gluetun · VPN |

## Architecture

```mermaid
flowchart TB
    me([Me]) --> TG([Telegram])
    me -. remote .-> TS([Tailscale])

    subgraph host["HP Elite x2 · Fedora Silverblue · rootless Podman"]
        direction TB

        subgraph home["🏠 Home &amp; media"]
            H[Heimdallr · Home Assistant]
            B[Bragi · Jellyfin]
        end

        subgraph court["🧠 The AI court"]
            R[Ratatoskr · Telegram agent]
            K[Kvasir · brand agent]
            HU[Huginn · AI sandbox]
            NID[Nidavellir · dev forge]
        end

        subgraph keep["🛡️ Keepers &amp; network"]
            M[Muninn · backups]
            D[Draupnir · price watch]
            NJ[Njord · VPN gateway]
        end
    end

    TG <--> R
    TS <--> host
    R <--> H
    R --> LLM([LLM · free-tier / local])
    K --> LLM
    HU -. built from .-> NID
```

> Access is either the **Telegram** bot (day to day) or **Tailscale** (a private
> mesh, for admin) — never a port exposed to the open internet.

## Principles

- **Immutable base** — Fedora Silverblue / `rpm-ostree`: the OS is an image.
  Experiments never rot the system, and updates roll back cleanly.
- **Rootless & isolated** — every service is a rootless Podman container; the AI
  coding agent runs in a sandbox that sees only the directory it is handed.
- **Reproducible** — a bare git dotfiles repo plus a bootstrap script rebuild the
  whole machine from a fresh install. Secrets never live in git.
- **One channel** — control and notifications converge on a single Telegram bot
  instead of a dozen apps.
- **Free / local-first AI** — the agents run on a free-tier model today and are
  built to swap to a fully local model (Ollama) tomorrow; secrets never reach the
  model.
- **Named like a saga** — because infrastructure you enjoy is infrastructure you
  keep maintaining.

## Reproducibility & secrets

The live configuration lives in a **private** repository and is intentionally not
public. This repository is the **architecture and the story**, kept clean of
secrets and machine-specific state. Credentials are stored outside git, with strict
permissions, and are never passed on a command line.

## License

[MIT](LICENSE) © Ivan Del Fatti
