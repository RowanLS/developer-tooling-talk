# Presenter utilities

These scripts support the **live demonstration** used in the *Better Code, Less Pain* presentation.

> **These scripts are presenter utilities, not part of the tutorial.**
>
> They deliberately use destructive Git commands to throw away changes made during the live demonstration. If you are exploring the repository yourself, **do not use these scripts**. Use normal Git commands to move between the tagged stages instead.

## For participants

You do **not** need anything in this directory.

To explore a stage of the demonstration, enter the `rt-demo` repository and use Git directly:

```bash
cd rt-demo
git switch --detach 03-types
```

To move to another stage:

```bash
git switch --detach 04-complexity
```

To see what changed between stages:

```bash
git diff 03-types 04-complexity
```

If you want to experiment with the code and preserve your changes, create a branch first:

```bash
git switch --detach 05-tests
git switch -c my-experiments
```

This is safer than using the presenter utilities.

---

# Presenter setup

The scripts are:

| Command | Purpose |
|---|---|
| `demo-start` | Reset the demo and go to `00-start` |
| `demo-next` | Discard demo changes and advance one tag |
| `demo-back` | Discard demo changes and return one tag |
| `demo-reset` | Discard changes and restore the current tag |

They are stored outside the `rt-demo` submodule so that switching between historical demo tags does not affect them.

## Make the commands available

From the **root of the `developer-tooling-talk` repository**, run:

```bash
export PATH="$(pwd)/scripts/presenter:$PATH"
```

You can then enter the demo repository:

```bash
cd rt-demo
```

and use:

```bash
demo-start
demo-next
demo-back
demo-reset
```

The `export` command only affects the current shell session. Open a new terminal and you will need to run it again.

### Verify the setup

From the repository root:

```bash
export PATH="$(pwd)/scripts/presenter:$PATH"
which demo-next
```

This should print a path ending in:

```text
developer-tooling-talk/scripts/presenter/demo-next
```

Make sure the scripts are executable:

```bash
chmod +x scripts/presenter/demo-*
```

This normally only needs to be done once; Git preserves the executable bit.

---

# Before the presentation

From the parent repository, start the presentation:

```bash
quarto preview presentation/talk.qmd
```

In a second terminal:

```bash
cd /path/to/developer-tooling-talk

export PATH="$(pwd)/scripts/presenter:$PATH"

cd rt-demo
demo-start
```

The second terminal is now ready for the live demonstration.

It is worth checking the current state before starting:

```bash
git status
git describe --tags --exact-match
```

The latter should report:

```text
00-start
```

---

# During the presentation

Advance to the next stage:

```bash
demo-next
```

For example:

```text
00-start
    ↓
01-uv
    ↓
02-ruff
    ↓
03-types
    ↓
04-complexity
    ↓
05-tests
    ↓
06-precommit
    ↓
07-ci
    ↓
08-dependabot
    ↓
09-final
```

If you accidentally advance too far:

```bash
demo-back
```

If you make changes during a demonstration and want to restore the current stage:

```bash
demo-reset
```

To return all the way to the beginning:

```bash
demo-start
```

---

# Warning: these commands destroy work

The presenter scripts are designed for a **disposable live-demo working tree**.

They use commands such as:

```bash
git reset --hard
git clean -fd
```

These can:

- discard modifications to tracked files;
- delete untracked files and directories;
- permanently destroy work that has not been committed elsewhere.

There is intentionally no confirmation prompt during the presentation.

**Do not use these utilities in a repository containing work you want to keep.**

In particular, participants following the tutorial should use:

```bash
git switch --detach TAG
```

rather than the presenter scripts.

---

# The demo repository is a submodule

`rt-demo` is a separate Git repository included in the presentation repository as a Git submodule.

During the presentation, switching between demo tags changes the commit currently checked out in the submodule.

As a result, the parent repository may report:

```text
modified: rt-demo (new commits)
```

after a demonstration.

This is expected. Do not commit this temporary submodule state to the parent repository.

After the presentation, return to the parent repository:

```bash
cd ..
```

and restore the recorded submodule commit:

```bash
git submodule update --init rt-demo
```

Only do this once you are certain there are no changes inside `rt-demo` that you need to preserve.

---

# Recovery

If the live demonstration gets into an unexpected state, first try:

```bash
demo-reset
```

If that does not work, inspect the repository:

```bash
git status
git tag --points-at HEAD
```

You can manually return to a known stage with:

```bash
git reset --hard
git clean -fd
git switch --detach 05-tests
```

Replace `05-tests` with the required stage.

Again, these commands are deliberately destructive and are appropriate only for the disposable presentation checkout.

## Demo stages

The presenter utilities expect these tags, in this order:

```text
00-start
01-uv
02-ruff
03-types
04-complexity
05-tests
06-precommit
07-ci
08-dependabot
09-final
```

If tags are renamed, added, or removed, update the tag lists in the presenter scripts accordingly.