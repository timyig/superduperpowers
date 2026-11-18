# Maintaining Superduperpowers

This public repository ships the plugin. Maintainers work on a branch inside the `superduperpowers/` submodule of the development repository. Keep plans, test evidence, and development tools on the development repository’s `main`. Publish fork changes to `origin` (`timyig/superduperpowers`). Upstream pull requests are outside this fork’s workflow.

## Prepare the checkout

From the development repository, run `git submodule update --init --recursive`. In the submodule, create a branch at the recorded pin. Inspect `git remote -v`. If the upstream remote is missing, add it with `git remote add upstream https://github.com/obra/superpowers.git`. Fetch release tags with `git fetch upstream --tags`.

## Maintain the patch series

The fork’s `main` is an upstream release plus a linear series of fork commits. Each fork commit owns one file. Use the subject `rebrand: <path>` and list each before → after edit and its count in the body. For a rewritten README or maintenance guide, describe its purpose. Preserve skill names, internal paths, version numbers, and environment variables unless the approved design changes them.

Before rebasing, note the old and new upstream release tags and fetch the fork’s current tip. Start a maintenance branch at that tip. Replay its fork commits with `git rebase --onto "$new_tag" "$old_tag"`.

- On a conflict, inspect `git show REBASE_HEAD`. During the rebase, Git’s “ours” is the new upstream plus the commits already replayed. “Theirs” is the fork commit being replayed. Start with upstream’s text and reapply the changes described in the commit body. Stage the result and run `git rebase --continue`.
- For conflicts in `README.md` or `AGENTS.md`, keep the fork’s complete version from the commit being replayed. If upstream deleted a patched file, drop its commit with `git rebase --skip`.
- Check for new `superpowers:<skill>` references in `skills/` and `hooks/`. Give each newly affected file its own commit. For an existing patched file, run `git commit --fixup=<owning-commit>`, then `GIT_SEQUENCE_EDITOR=: git rebase -i --autosquash "$new_tag"`. Check that the owning commit’s body describes the resulting counts.

## Verify and publish

Run validation from the development repository’s devcontainer, using its mise-managed Claude Code, Codex, and Node. Keep disposable CLI profiles inside the container, outside the checkout. Use the development repo’s Phase 1 plan for the commands and expected results.

1. Confirm the old skill namespace is absent from `skills/` and `hooks/`. Inspect every commit since the upstream tag for its single, unique file. Parse all four manifests and run `claude plugin validate .`.
2. Install the local fork marketplace in both clients. Check the Claude session-start bootstrap and invoke brainstorming and writing-plans. Check the installed Codex skill inventory and its skill picker. Test the visual companion with telemetry settings on and off. It must show the fork name, version, and link without loading the upstream logo.
3. Rehearse replaying the stack onto the previous upstream release on a disposable branch. Record any conflicts and return to the tested candidate before publishing.
4. For a normal release, fast-forward the public `main`. Before replacing published history during an upstream rebase, keep older submodule pins reachable by publishing a tag for the old tip. Verify the remote tip still matches the revision you reviewed, then use `git push --force-with-lease=refs/heads/main:"$old_tip" origin HEAD:main`.
5. Repeat both installs from `timyig/superduperpowers` with fresh container profiles and anonymous Git access. On the development repo’s `main`, confirm the tested submodule HEAD is reachable from the fork’s `origin/main`, run `git add superduperpowers`, and commit the pin. Verify `git rev-parse HEAD:superduperpowers` equals the tested SHA.
