# secure-delivery-pipeline

A reference implementation of a **security-gated CI/CD pipeline**: every commit is linted, tested, scanned (SAST, secrets, dependencies, IaC, licenses, container image), and only signed, attested, provenance-verified images reach the deploy stage.

This mirrors patterns I built and rolled out across 150+ repositories in production — rebuilt here from scratch, in the open, as a reference.

## Architecture

```
commit ──► PR CI ──► security gate ──► build & sign ──► verify ──► deploy
            lint      Semgrep SAST      container        cosign     (gated)
            test      Gitleaks          image build      verify
                      Trivy (deps,      SBOM (Syft)      SLSA
                      IaC, license,     cosign keyless   provenance
                      image)            signing          check
```

*(Architecture diagram coming — see roadmap.)*

## Security gates

| Gate | Tool | Status |
| --- | --- | --- |
| Static analysis (SAST) | Semgrep | 🔜 planned |
| Secret scanning (full history) | Gitleaks | 🔜 planned |
| Dependency + image scanning | Trivy | 🔜 planned |
| IaC + license scanning | Trivy | 🔜 planned |
| SBOM generation | Syft | 🔜 planned |
| Image signing (keyless) | cosign | 🔜 planned |
| Provenance attestation | SLSA | 🔜 planned |
| Verify-before-deploy | cosign + policy | 🔜 planned |

## Design principles

- **Audit before enforce** — every gate ships in audit mode first, so adoption never blocks a team overnight.
- **Deterministic gates** — same inputs, same verdict; no flaky security checks.
- **Pin everything** — every action pinned to a commit SHA, read-only token defaults, finite timeouts.
- **Fail with context** — a blocked PR gets a summary saying exactly what failed and how to fix it, not a wall of JSON.

## Roadmap

- [ ] Demo app + base CI (lint → test → build)
- [ ] Semgrep SAST with PR-summary output
- [ ] Gitleaks full-history secret scan
- [ ] Trivy dependency + container-image scans
- [ ] Trivy IaC + license scanning
- [ ] Configurable audit/enforce mode
- [ ] SBOM (Syft) uploaded as build artifact
- [ ] cosign keyless image signing
- [ ] SLSA provenance attestation + verification
- [ ] Verify signature + provenance before deploy
- [ ] Architecture diagram
- [ ] "Why these gates" — tool choices and tradeoffs

## License

MIT
