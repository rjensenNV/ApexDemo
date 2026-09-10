# Contributing to Apex

External contributions are open. Code fixes, performance and compatibility improvements, tests, examples, and documentation updates are welcome when they preserve Apex's purpose as a set of current PyTorch extensions for NVIDIA GPU training.

All project participants must follow the [Code of Conduct](CODE_OF_CONDUCT.md).

## Contribution scope

Focused fixes and documentation improvements can be submitted directly as pull requests. Open an issue before implementing a substantial feature, public API change, new dependency, compatibility change, broad refactor, or performance-sensitive design so the approach and validation plan can be discussed first.

Changes unrelated to Apex's mixed-precision, distributed-training, optimizer, normalization, or supporting extension scope might be declined. Individual `apex.contrib` modules can have narrower hardware and PyTorch compatibility than the core package.

## Reporting bugs and proposing changes

- Report reproducible bugs with the [bug form](https://github.com/rjensenNV/ApexDemo/issues/new?template=01_bug_report.yml). Include the smallest practical reproducer, expected and actual behavior, and output from `python -m torch.utils.collect_env` after removing sensitive information.
- Propose features or other substantial changes through a [new GitHub issue](https://github.com/rjensenNV/ApexDemo/issues/new). Describe the problem, intended users, proposed approach, alternatives, and expected compatibility or performance impact.
- Use [GitHub Issues](https://github.com/rjensenNV/ApexDemo/issues) for best-effort usage questions. Search existing issues first.
- The maintainer prioritizes work according to project scope, reproducibility, compatibility, impact, and available capacity. No response or implementation timeline is guaranteed.

## Development setup

Source installation currently requires a compatible PyTorch installation and a discoverable CUDA toolkit because `setup.py` queries `nvcc` during package metadata generation. The full build is intended for Linux with a compatible NVIDIA GPU, driver, CUDA toolkit, and C++ toolchain. Start from a clean checkout:

```bash
git clone --recurse-submodules https://github.com/rjensenNV/ApexDemo.git
cd ApexDemo
python -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install -r requirements.txt -r requirements_dev.txt
python -m pip install pre-commit sphinx-rtd-theme
python -m pip install -v --disable-pip-version-check --no-build-isolation --no-cache-dir .
```

The final command creates a Python-only build but still requires `CUDA_HOME` and `nvcc`. Follow the extension-specific commands in [README.md](README.md#installation) when a change requires C++ or CUDA functionality.

### Tests and checks

Run the checks relevant to the changed files:

```bash
pre-commit run --all-files
python tests/L0/run_test.py
python -m pytest apex/contrib/test
make -C docs html
```

The L0 and contribution test suites require the extensions exercised by the selected tests. Many tests also require one or more supported NVIDIA GPUs and a compatible CUDA environment. State exactly which commands and hardware you used, and explain checks that could not be run. No additional private maintainer-only validation command is documented for this repository.

### Coding conventions

- Python lint and format behavior is defined in `pyproject.toml` and `.pre-commit-config.yaml`; Ruff uses a 100-character line length.
- C, C++, CUDA, header, and protocol-buffer formatting is enforced by the configured clang-format hook, subject to its documented exclusions.
- Preserve existing public API and compatibility behavior unless the change was discussed in advance.
- Keep optional-extension imports and tests usable when unrelated extensions are not installed.

## Performance-sensitive changes

Include before-and-after results collected with the same hardware, software versions, inputs, warm-up, and measurement method. Provide the exact command, relevant GPU and CUDA details, variability across runs, and any correctness, memory, or compatibility tradeoffs. A performance claim without reproducible methodology might not be accepted.

## Pull request process

1. Create a focused branch from `master` and keep the change limited to one coherent purpose.
2. Link the relevant issue when prior discussion is required.
3. Add or update tests and documentation for changed behavior.
4. Run the applicable checks and record exact results and limitations in the pull request.
5. Use a concise imperative commit subject consistent with the existing history. Apex does not require Conventional Commits.
6. Complete the pull request template and respond to review feedback.

## Legal contribution requirement

This repository does not impose a Contributor License Agreement, Developer Certificate of Origin sign-off, or other project-specific legal gate. Contributions remain subject to the repository's license and applicable GitHub terms.

## Review process

Pull requests are reviewed by the [maintainer](MAINTAINERS.md), with automatic review requests configured in [.github/CODEOWNERS](.github/CODEOWNERS). Reviews and follow-up occur on a best-effort basis, and no initial-review time is guaranteed. If a pull request has not received a response, comment on it to request a status update. A pull request may be closed when requested changes remain unresolved, but there is no fixed inactivity period.

## Governance and maintainer information

- [Governance and decisions](GOVERNANCE.md)
- [Current maintainer](MAINTAINERS.md)

## AI-assisted contributions

AI-assisted contributions are permitted. Disclose significant AI assistance in the pull request, review and understand all submitted material, and remain responsible for its correctness, security, licensing, and validation. Do not use an AI system to invent legal terms, security policy, maintainer decisions, support commitments, benchmark results, or test results.

See [AGENTS.md](AGENTS.md) for repository-specific guidance for coding agents.

## Security issues

Do not report security vulnerabilities through public issues or pull requests. Follow [SECURITY.md](SECURITY.md) to report them privately.
