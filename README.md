# studio, a Bonsai pack

The studio's part of every project a studio runs, as a [Bonsai](https://github.com/LastStep/Bonsai) pack. A studio is
the layer above a person's projects: it lists them, reads each project's Bonsai records, shows them on its Desk, and
keeps a home of its own on each machine, `~/.trinetra`, with the keys it works with. This pack holds what a project
needs from that studio:

- **Fifteen walls**: deny rules that stop agents reading the three secrets in the studio's home: `token` (the key
  the studio's bridge uses to reach its server), `deploy_ed25519` (the key that deploys the server) and `salt` (the key
  of every input hash the studio records). Each is spelled five ways, for the five ways one machine reaches that home:
  this home (`~/.trinetra/`), the Windows home from WSL (`//mnt/c/Users/*/.trinetra/`), a WSL home from Windows by its
  two network names (`//wsl.localhost/*/home/*/.trinetra/`, `//wsl$/*/home/*/.trinetra/`), and a WSL home through
  WSLg's mount of it (`//mnt/wslg/distro/home/*/.trinetra/`), which a hand trial found reaches the real home.
- **Two skills**: `studio:desk-asks` (how a question for the person reaches the Desk through `bonsai ask`) and
  `studio:brief` (the daily brief, and its template).
- **One document kind**, `brief`, the daily brief, with its template in `studio:brief`.
- **A two-line block** in the project's `CLAUDE.md` naming the walls and the skills.

It has no roles, no lanes, no labels and no protected paths, and **it carries no code**: no hook line, no file the
engine writes into a project, nothing in the plugin that Claude Code runs on its own. So it links with `--yes` alone.

It is meant to be used with `base`, Bonsai's own pack (its walls on the common key files, Bonsai's document
templates), and a workflow pack for the roles. Only one linked pack may declare a document kind (Bonsai refuses a link
where two do), so a project takes this pack with a workflow pack that does not declare `brief`.

**Where things stand.** Before this pack, no project the studio runs had these walls in its settings: until this pack
is linked in a project, agents there can read the studio's secrets. The skill for the studio's own command,
`studio:trinetra`, comes in a later release, with the command.

## How a project takes it

A first link, with no `bonsai.yaml` yet (a person's command, in the project's checkout):

```sh
bonsai init --name <project> --source https://github.com/LastStep/bonsai-studio --ref v0.1.0 --yes
```

A project already linked adds the pack as an entry in its `bonsai.yaml`, after `base`:

```yaml
packs:
  - id: studio                        # the pack
    source: "https://github.com/LastStep/bonsai-studio.git"   # where it is fetched from
    path: null                        # the pack is at its repository's top
    ref: "v0.1.0"                     # its release tag; the lock records the commit
    roles: null                       # the pack declares no roles
```

then runs `bonsai update`, which previews every change: the fifteen walls, each with its sentence, the block, and the
kind `brief`. The project's `bonsai.yaml` gives `brief` a folder under `documents` (`work/briefs` unless it names
another). Moving to a newer release is a person's step: change `ref`, then run `bonsai update`.

**The walls are tried by hand once on each machine.** A wall that does not hold prints the secret into a session's
transcript, so no agent tries one. The person runs, in a session in a linked project, a read of each spelling that
applies to the machine and sees it refused, for example in WSL: ask Claude Code to read `~/.trinetra/salt`, then
`/mnt/c/Users/<name>/.trinetra/salt` and `/mnt/wslg/distro/home/<name>/.trinetra/salt`; on Windows: `\\wsl.localhost\<distro>\home\<name>\.trinetra\salt` and
`\\wsl$\<distro>\home\<name>\.trinetra\salt`. `salt` is the one to try: it is the least sensitive of the three.

## The folder

| File | What it is |
|---|---|
| `.claude-plugin/plugin.json` | The plugin manifest Claude Code reads. JSON holds no comments, so it is documented here, under "plugin.json" |
| `skills/` | The pack's skills, one folder each with its `SKILL.md`, to the open Agent Skills standard; a skill loads as `studio:<name>` |
| `bonsai/pack.yaml` | The Bonsai manifest (format `bonsai.pack/1`): id, version, needs, the block, the fifteen walls and the kind `brief`. A `#` comment on every key documents it |
| `bonsai/block.md` | The pack's two lines in the project's `CLAUDE.md`; its leading comment documents it |
| `ci/check.sh` | Every check the pack has, in one script; its header documents each subcommand and argument |
| `.github/workflows/pack.yml` | The pack's CI and release, calling `ci/check.sh`. A `#` comment on every key |
| `.gitattributes` | Every file is text with LF line endings on every checkout, Windows included, so `sh ci/check.sh` runs there |
| `LICENSE` | MIT |
| `README.md` | This page |

`bonsai check --schema bonsai.pack` prints every field of `pack.yaml` with its allowed values.

## Role and skill files

Each file in `agents/` and `skills/` has one row here, and each row names a file there: `ci/check.sh` fails otherwise.

| File | What it is |
|---|---|
| `skills/desk-asks/SKILL.md` | The skill `studio:desk-asks`, no template: when to ask the person, how to word an ask for a phone (context first, one question, two to four options and a recommendation), filing it with `bonsai ask` and reading the answer |
| `skills/brief/SKILL.md` | The skill `studio:brief`, the template of the document kind `brief`: how to write the daily brief, its fields table and the template to copy |

### How each kind of file documents itself

Every file the pack ships says its purpose, when to use it, and every field.

- **A YAML file** (`pack.yaml`, `pack.yml`): a short header, and a `#` comment on every key. `bonsai check --pack`
  fails a key in `bonsai/` without one.
- **A template skill** (`brief`): a fields table, `| Field | Meaning | Allowed values | Example |`, above its
  template, the first fenced block after the table; `bonsai check --pack` holds the two equal.
- **A skill that is no template** (`desk-asks`): an HTML comment just after the frontmatter, pointing here.
- **`ci/check.sh`**: its header. **`plugin.json`**: this page. **`block.md`**: its leading comment.

### Skill files: frontmatter fields

| Field | Meaning | Allowed values | Example |
|---|---|---|---|
| `name` | The skill's name; the folder is `skills/<name>/`, and the skill loads as `studio:<name>` | a lower-case letter, then lower-case letters, digits and dashes | `brief` |
| `description` | What the skill holds and when to open it: Claude reads it to decide whether to load the skill | one line of text, quoted | `"The daily brief ..."` |

## plugin.json

| Field | Meaning | Allowed values | Example |
|---|---|---|---|
| `name` | The plugin's name, which Claude Code puts before every skill (`studio:brief`) | the pack's id, exactly as `bonsai/pack.yaml`'s `id` | `"studio"` |
| `description` | What the pack is, in a sentence, as Claude Code's plugin lists show it | text | `"The studio's part of a project ..."` |
| `author` | Who makes the pack | an object; `name` is the maker's name | `{"name": "Trinetra"}` |
| `repository` | Where the pack's source is | a URL | `"https://github.com/LastStep/bonsai-studio"` |
| `license` | The pack's license | an SPDX id | `"MIT"` |

**Never add `version`** (below).

## Versions and releases

- **A project pins the pack by commit.** `bonsai.yaml` names a tag or commit; Bonsai's lock records the 40-character
  commit and refuses a tag that later moves. `plugin.json` never carries a `version`, and `claude plugin validate
  --json` warns about that one thing, which `ci/check.sh` allows.
- **`bonsai/pack.yaml`'s `version` names tags only.** A release is a tag `v` and that version (`v0.1.0`). To release:
  raise `version`, commit, push, then tag that commit `v<version>` and push the tag. The CI checks everything again,
  checks that the tag equals the version, and makes the GitHub release with its notes.

## Checks, CI and release

`ci/check.sh` is the one home of the pack's checks (its header documents each subcommand):

- `sh ci/check.sh check <bonsai> <folder> <source> <commit> [<path>]`: `bonsai check --pack` on the folder; this page's
  rows against `skills/`; a scratch project linked to the pack at the commit, with `claude` off the PATH and a
  temporary Bonsai home; `bonsai check` there. Locally, from the pack's top, on committed work:
  `sh ci/check.sh check "$(command -v bonsai)" . . "$(git rev-parse HEAD)"`.
- `sh ci/check.sh deny <folder>`: `pack.yaml`'s deny list is exactly the fifteen walls, none missing, none extra, none
  twice.
- `sh ci/check.sh validate <folder>`: `claude plugin validate --json`, failing on every error and warning but the
  missing `version`. It needs no Claude login.
- `sh ci/check.sh release <tag> <folder>`: fails unless the tag is `v` and `pack.yaml`'s `version`.

`.github/workflows/pack.yml` runs on every push and pull request (it uses no secret): `check` and `deny` on Linux and
Windows with a bonsai built from `BONSAI_COMMIT` (Bonsai `v1.0.0-rc.2` today); `validate` on Linux with the pinned
Claude Code; then `release`, which checks the tag the version needs on every push and, on a `v*` tag only, makes the
GitHub release.
