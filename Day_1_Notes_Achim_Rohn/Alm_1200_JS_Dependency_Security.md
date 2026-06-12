# Notes from discussion

You have to be careful about transitive dependencies as well.

Using an internal registry can mitigate also some of the public npm registry risks.

Other package manager other than npm already have an ignore scripts, but npm 12 will have it in July.

# Notes from discussion

You have to be careful about transitive dependencies as well.

Using an internalt registry can mitigate also some of the public npm registry risks.

Other package manager other than npm already have an ignore scripts, but npm 12 will have it in July.
# Security Advantages of Hardened npm Config plus VM Isolation

>**Disclaimer**: Almost all of this was generated using AI.

The code for this presentation is here: https://github.com/gorlug/secure-npmrc

---

## 1. The Problem

Modern Node.js projects execute a large, fast-moving dependency graph.

Every `npm install` can introduce risk from:

- malicious packages
- compromised maintainers
- dependency confusion
- install-time lifecycle scripts
- typosquatting
- vulnerable transitive dependencies
- stolen npm credentials or tokens

---

## 2. First Layer: Harden npm Behavior

Start by making npm less permissive.

This repository demonstrates `.npmrc` hardening that reduces common supply-chain attack paths before a package ever gets to run.

Recommended baseline:

```ini
ignore-scripts=true
allow-git=none
min-release-age=3
save-exact=true
engine-strict=true
strict-ssl=true
```

Security goal:

- prevent automatic install-time code execution
- avoid unaudited Git dependency sources
- slow down exposure to brand-new malicious releases
- make dependency versions reproducible
- enforce the expected Node.js runtime
- require valid TLS for registry traffic

---

## 3. `.npmrc`: Disable Lifecycle Scripts

```ini
ignore-scripts=true
```

Run it manually inside `tmp/`:

```sh
npm run verify:npmrc:manual
cd tmp/verify-npmrc-manual/ignore-scripts/app
npm install ./../../shared/scripted-package
test ! -f postinstall-ran.txt
```

Expected: install succeeds, but `postinstall-ran.txt` is not created.

Security advantage:

- blocks automatic `preinstall`, `install`, and `postinstall` execution
- reduces install-time remote code execution risk
- helps defend against malicious dependency scripts and npm worms

This is one of the highest-impact npm hardening settings because many supply-chain attacks rely on lifecycle scripts running automatically during install.

Upcoming change with npm v12: https://github.blog/changelog/2026-06-09-upcoming-breaking-changes-for-npm-v12/ (09.06.2026)

---

## 4. `.npmrc`: Block Git Dependencies

```ini
allow-git=none
```

Run it manually inside `tmp/`:

Use bash for this

```sh
npm run verify:npmrc:manual
cd tmp/verify-npmrc-manual/allow-git/git-package
git init
git config user.name Verifier
git config user.email verifier@example.invalid
git add .
git commit -m init
cd ../app
npm install git+file://$(cd ../git-package && pwd)
```

Expected: install fails because Git dependencies are blocked.

Security advantage:

- prevents dependencies from being pulled directly from Git URLs
- reduces bypass paths around registry controls
- encourages auditable, versioned registry artifacts

Dependency sources should be predictable and reviewable.

---

## 5. `.npmrc`: Quarantine New Releases

```ini
min-release-age=3
```

Run it manually inside `tmp/`:

```sh
npm run verify:npmrc:manual
cd tmp/verify-npmrc-manual/min-release-age/app
cat README.md
# Use the recent and safe versions printed in README.md:
npm install <package>@<recent-version> --package-lock-only
npm install <package>@<safe-version> --package-lock-only
```

Expected: the recent version is rejected, while the older safe version is accepted.

Security advantage:

- delays installation of very new package versions
- gives the ecosystem time to detect malware
- reduces exposure to zero-day supply-chain attacks

A short waiting period can block many fast-moving package compromise campaigns.

---

## 6. `.npmrc`: Exact Versions

```ini
save-exact=true
```

Run it manually inside `tmp/`:

```sh
npm run verify:npmrc:manual
cd tmp/verify-npmrc-manual/save-exact/app
npm install is-number@7.0.0 --package-lock-only
node -p "require('./package.json').dependencies['is-number']"
```

Expected: the printed version is exactly `7.0.0`, not `^7.0.0`.

`save-exact=true` also causes `save-prefix=^`

Security advantage:

- stores exact dependency versions
- avoids surprise upgrades through semver ranges
- improves reproducibility across rebuilds
- makes dependency diffs easier to review

Predictable inputs are easier to audit and safer to reproduce.

---

## 7. `.npmrc`: Enforce Engine Compatibility

```ini
engine-strict=true
```

Run it manually inside `tmp/`:

```sh
npm run verify:npmrc:manual
cd tmp/verify-npmrc-manual/engine-strict/app
npm install ./../../shared/engine-locked
```

Expected: install fails with an engine-related error such as `EBADENGINE`.

Security advantage:

- rejects packages with incompatible Node.js engine declarations
- reduces undefined behavior from unsupported runtimes
- encourages consistent toolchain setup

This pairs well with pinned Node.js and npm versions.

---

## 8. `.npmrc`: Require Valid TLS

```ini
strict-ssl=true
```

Run it manually inside `tmp/`:

```sh
npm run verify:npmrc:manual
cd tmp/verify-npmrc-manual/strict-ssl/app
npm view anything version --registry https://self-signed.badssl.com/ --fetch-retries=0
```

Expected: request fails with an SSL or certificate error.

This is set true by default in npm v11.

Security advantage:

- rejects invalid TLS certificates
- helps prevent man-in-the-middle attacks
- protects package metadata and tarball downloads in transit

Registry traffic should fail closed when transport security is broken.

---

## 9. The Core Security Goal

Limit the blast radius.

Hardened npm settings reduce the chance that risky package behavior happens.

But if malicious code still gets through, the ideal outcome is:

- it can only affect the development environment
- it cannot read personal host files
- it cannot steal unrelated SSH keys
- it cannot access browser cookies or password stores
- it cannot tamper with other projects
- it can be destroyed and rebuilt quickly

That is where the VM becomes the next security layer.

---

## 10. Second Layer: VM Isolation

Running Node/npm inside a VM creates a hard boundary between the project and the host OS.

The VM contains:

- project source code
- `node_modules`
- npm cache
- build artifacts
- local dev servers
- project-specific tools

The host keeps:

- personal files
- browser profiles
- primary SSH keys
- password manager state
- unrelated repositories
- daily-driver OS configuration

---

## 11. Why npm Projects Deserve Isolation

Node/npm workflows commonly involve:

- downloading code from the public internet
- executing build tooling from dependencies
- running local dev servers
- invoking package manager scripts
- handling environment variables and credentials

That means a compromised dependency may not just be “code in the project.” It may become code running on your machine.

A hardened `.npmrc` lowers that risk. A VM contains the damage if the risk materializes anyway.

---

## 12. Reduced Credential Exposure

A VM allows you to use scoped, disposable credentials.

Recommended pattern:

- no personal global npm token in the VM
- no broad GitHub token in the VM
- use project-specific deploy keys where possible
- use read-only registry tokens where possible
- avoid mounting the host home directory
- keep secrets short-lived and revocable

If the VM is compromised, only the VM’s limited secrets are exposed.

---

## SSH Agent coming from Host

### Example macOS to Linux

Use socat to forward the SSH agent socket from the host to the VM.

On Mac:

```bash
ssh-add --apple-use-keychain ~/.ssh/2022-12-08
ssh-add -S /usr/lib/ssh-keychain.dylib ~/.ssh/id_ecdsa_sk_rk # this uses to mac enclave with fingerprint unlock
socat TCP-LISTEN:12346,bind=10.211.55.2,range=10.211.55.7/32,reuseaddr,fork UNIX-CONNECT:$SSH_AUTH_SOCK
```

On Linux:

```bash
#!/bin/bash
killall socat
rm ~/.ssh/agent.sock
socat UNIX-LISTEN:$SSH_AUTH_SOCK,fork TCP:10.211.55.2:12346 &
ssh-add -l
```

## 13. Disposable Environments

VMs make risky development environments easier to reset.

Useful workflows:

- snapshot before testing unknown dependencies
- rollback after suspicious behavior
- rebuild from infrastructure scripts
- destroy the VM after high-risk work
- keep separate VMs per client, project, or trust level

This converts many incidents from “clean my laptop” to “delete and recreate the VM.”

---

## 14. Better Separation Between Projects

Without isolation, multiple projects often share:

- global npm cache
- shell configuration
- environment variables
- SSH agent access
- package manager state
- globally installed tools

With VMs, each project or risk class can have its own isolated development environment.

A malicious dependency in one project has less opportunity to affect another.

---

## 15. Network Controls Become Easier

A VM can be placed behind stricter network policy than the host.

Examples:

- block access to internal networks
- allow only npm registry and Git provider egress
- disable inbound access except IDE/SSH
- use a proxy for logging package downloads
- route risky projects through separate network profiles

This helps contain malware that attempts command-and-control or lateral movement.

---

## 16. How JetBrains Gateway Preserves Developer Experience

JetBrains Gateway lets the IDE backend run inside the VM while the UI runs locally.

You still get:

- native JetBrains editor UI
- local keyboard shortcuts
- indexing and inspections
- integrated terminal
- debugger support
- test runner integration
- Git tooling

But the project execution happens remotely inside the VM.

---

## 17. Gateway Architecture

```text
Host machine                         Development VM
-------------                        ----------------
JetBrains Gateway UI  <---------->   JetBrains IDE backend
Keyboard/display                     Project files
Native desktop feel                  Node.js / npm
No local node_modules                Terminal commands
Personal OS protected                Build/test/dev servers
```

The host provides the interface. The VM performs the work.

---

## 18. Recommended Workflow

1. Create or start the project VM.
2. Install Node.js and npm inside the VM.
3. Configure hardened `.npmrc` settings.
4. Clone the repository inside the VM.
5. Connect with JetBrains Gateway.
6. Run `npm install`, builds, tests, and dev servers in the VM.
7. Keep host filesystem mounts disabled or minimal.
8. Snapshot before risky dependency updates.
9. Destroy/rebuild the VM when needed.

---

## 19. What This Protects

Hardened npm plus VM-based development helps protect the host’s:

- home directory
- SSH keys
- browser cookies
- password manager integrations
- unrelated source code
- global npm configuration
- cloud credentials
- system services
- local network identity

The VM becomes the sacrificial workspace for package activity that survives npm hardening.

---

## 20. What VM Isolation Does Not Solve

A VM is not magic.

It does not eliminate the need for:

- dependency review
- lockfile review
- patching the VM OS
- updating Node.js and npm
- restricting secrets inside the VM
- monitoring suspicious network behavior
- auditing CI/CD separately

Malicious code can still damage anything available inside the VM.

---

## 21. Closing Message

Node/npm supply-chain risk is real because dependency code often becomes local machine code.

A hardened `.npmrc` reduces the chance that malicious package behavior runs in the first place.

A VM limits the damage if something still gets through.

JetBrains Gateway keeps the workflow pleasant.

Together, they provide a practical, developer-friendly security model.
