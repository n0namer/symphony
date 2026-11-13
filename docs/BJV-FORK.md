# Downstream fork workflow (BJV)

Repository ownership:
- `origin`: `https://github.com/n0namer/symphony.git` — the user's fork.
- `upstream`: `https://github.com/openai/symphony.git` — the original project.
- `main`: clean upstream-tracking branch; do not commit local changes here.
- `custom/bjv`: versioned downstream branch for reviewed modifications to the Symphony engine.

## Safely sync upstream

In a clean working tree:

```sh
git fetch upstream main
git switch main
git merge --ff-only upstream/main
git push origin main
git switch custom/bjv
# Review new upstream changes and run relevant tests first.
git merge --no-ff main
# Resolve conflicts, verify tests, then push.
git push origin custom/bjv
```

Never force-push or automatically merge a conflicting integration. If the working tree is not clean, stop and reconcile before switching branches.

BJV's running Symphony services use separately deployed WSL2 paths; **pushing this fork does not update or restart those services**. Production/test rollout requires a tested build, explicit version/SHA evidence, independent QA, controlled service update and readback. Keep FormPilot/private operational workflow and credentials **out** of this public repository. The private configuration snapshot is maintained separately.
