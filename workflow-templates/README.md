# Workflow files

The upload token had no `workflow` scope, so these files are kept here instead of `.github/workflows/`.
Move them back to enable automated releases and CI:

```bash
git mv workflow-templates .github/workflows
```

This can also be done in the GitHub web UI.
