# Superduperpowers

Superduperpowers brings the Superpowers 7.0.2 workflow to Claude Code and Codex under the `superduperpowers` skill namespace. Use it to brainstorm a design, write an implementation plan, and carry the work through testing and review. Phase 1 keeps the upstream workflow. Integration with taskmd is planned for a later phase.

## Claude Code

If Superpowers is already installed, open `/plugin`, select the installed `superpowers` plugin, and uninstall it. Then run these commands in Claude Code.

```text
/plugin marketplace add timyig/superduperpowers
/plugin install superduperpowers@superduperpowers-dev
```

Start a new session and invoke `/superduperpowers:brainstorming` to check the installation.

## Codex

If Superpowers is already installed, find its marketplace with `codex plugin list` and remove the installed entry with `codex plugin remove superpowers@<marketplace>`. Then run these commands in your terminal.

```sh
codex plugin marketplace add timyig/superduperpowers
codex plugin add superduperpowers@superduperpowers-dev
```

Run `codex plugin list --marketplace superduperpowers-dev` to check the installation. Open a new Codex session and look for `superduperpowers:writing-plans` in the skill picker.

This fork supports Claude Code and Codex. For other harnesses, see [upstream Superpowers](https://github.com/obra/superpowers).
