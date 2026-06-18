# terraform-modules

Reusable, public OpenTofu/Terraform modules for [Speedforge](https://speedforge.dev) infrastructure. Layer split: Terraform = cloud, Helm = k8s.

## Modules

| Module | Description |
|--------|-------------|
| [arc-runner-scale-set](modules/arc-runner-scale-set/) | GitHub Actions Runner Controller (ARC) runner scale set via the upstream `gha-runner-scale-set` Helm chart |

## Usage

Consume modules by **git source pinned to a semver tag** (`?ref=vX.Y.Z`). Never
reference an unpinned `main` and never copy a module into a consumer repo.

```hcl
module "runners" {
  source = "git::https://github.com/speedforge/terraform-modules.git//modules/arc-runner-scale-set?ref=v0.1.0"

  name          = "agents-gke"
  chart_version = "0.12.1"

  github_config_url    = "https://github.com/your-org"
  github_config_secret = "github-runner-auth"

  min_runners = 0
  max_runners = 2
}
```

## Versioning

Modules are released as immutable semver git tags. Consumers pin to a tag so an
upstream change can never silently alter a consumer's plan
(MAJOR = breaking variable/output change, MINOR = new module/variable,
PATCH = fix). Pre-`v1.0.0` minors may include breaking changes.

To cut a new release: merge to `main`, then push a new `vX.Y.Z` tag.

## CI

- **tofu fmt** - formatting check
- **tflint** - Terraform linting
- **trivy** - security misconfiguration scanning
- **tofu test** - module tests using mock providers
- **terraform-docs** - README drift check
- **tofu validate** - syntax and configuration validation

## License

[MIT](LICENSE)
