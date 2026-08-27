# odoopack-semver

Odoo-aware version parsing, comparison and resolution for the odoopack ecosystem, shared by the registry and the CLI.

An Odoo module version follows the convention `<series>.<major>.<minor>.<patch>` where the series is the two-part Odoo version:

```
19.0.1.2.3
└──┘ └───┘
series  module semver (major.minor.patch)
```

## Two kinds of version

- **Release**: five numeric components, e.g. `19.0.1.2.3`.
- **Dev**: a branch pseudo-version, e.g. `dev-19.0` or `dev-main`.

Anything else is rejected.

## Ordering

Releases sort by series first, then by module semver; dev versions sort below all releases. Comparison is numeric, so `18.0.10.0.0` is greater than `18.0.9.9.9`.

## Constraints (basic)

| Constraint | Matches |
| --- | --- |
| `19.0.1.2.3` | that exact release |
| `19.0` | the highest release in series 19.0 |
| `dev-19.0` | the dev version for that branch |

## Usage

```go
import odoosemver "github.com/wimwenigerkind/odoopack-semver"

v, err := odoosemver.Parse("19.0.1.2.3")

c, _ := odoosemver.ParseConstraint("19.0")
best, ok := odoosemver.Resolve(c, versions) // highest 19.0 release
```
