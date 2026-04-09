# Branch Strategy

## Branches

| Branch | Purpose | Protected |
|--------|---------|-----------|
| `main` | Stable, tested, production-ready releases | Yes |
| `dev` | Active development, feature integration | Yes (require PR) |
| `feature/*` | Individual features (branch from `dev`, merge to `dev`) | No |

## Workflow

```
feature/touchstone-export ──┐
feature/copper-loss ────────┤──→ dev ──→ main (release)
feature/gerber-import ──────┘
```

1. New work branches from `dev` as `feature/<name>`
2. Feature branches merge to `dev` via PR
3. When `dev` is stable and tested, merge to `main` as a release
4. Hotfixes branch from `main`, merge to both `main` and `dev`

## Naming

- `feature/<name>` — new functionality
- `fix/<name>` — bug fixes
- `refactor/<name>` — code cleanup without behavior change
