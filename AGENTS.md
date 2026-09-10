# Agent Guidance for Apex

## Project context

- Purpose and users: Apex provides mixed-precision, distributed-training, optimizer, normalization, and related PyTorch extensions for researchers and engineers using NVIDIA GPUs.
- Primary languages and frameworks: Python, C++, CUDA, PyTorch, setuptools, Ruff, clang-format, pytest, and Sphinx.
- Main entry points: the `apex` Python package, optional extensions configured by `setup.py`, and examples under `examples/`.
- Documentation: [published documentation](https://rjensennv.github.io/ApexDemo/) and source under `docs/source/`.

## Repository map

| Path | Purpose | Notes |
| --- | --- | --- |
| `apex/` | Python package and contribution modules | Optional modules can require separately built extensions. |
| `csrc/` | Core C++ and CUDA extension sources | Build selection is controlled by `setup.py` flags and environment variables. |
| `apex/contrib/csrc/` | Sources for optional contribution extensions | Preserve component-specific copyright and license notices. |
| `tests/L0/` | Core extension test runner and suites | Requires the relevant compiled extensions and CUDA-capable hardware. |
| `apex/contrib/test/` | Tests for optional contribution modules | Hardware and extension requirements vary by module. |
| `examples/` | Example training and container workflows | Several examples require external datasets or multiple GPUs. |
| `docs/` | Sphinx documentation source and publishing Makefile | Build locally with `make -C docs html`. |
| `.pre-commit-config.yaml` | Repository lint and formatting hooks | Runs Ruff and clang-format with repository-specific exclusions. |

## Build, test, and verify

Use Python with a compatible PyTorch installation. The current installer queries `nvcc` during metadata generation, so all source installation modes require a discoverable CUDA toolkit. A full build additionally requires Linux, an NVIDIA GPU and compatible driver, a C++ toolchain, and preferably Ninja.

Run setup commands from the repository root:

```bash
python -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install -r requirements.txt -r requirements_dev.txt
python -m pip install pre-commit sphinx-rtd-theme
python -m pip install -v --disable-pip-version-check --no-build-isolation --no-cache-dir .
```

Run the checks applicable to the change:

```bash
pre-commit run --all-files
python tests/L0/run_test.py
python -m pytest apex/contrib/test
make -C docs html
```

The installation command above creates a Python-only build but still requires `CUDA_HOME` and `nvcc`. Follow [README.md](README.md#installation) for C++ and CUDA build options. Do not claim GPU-dependent tests passed unless they ran with the required extensions and hardware; report omitted checks and their prerequisites precisely.

## Project guardrails

Agents may edit source, documentation, tests, and examples and may run relevant verification when requested.

Do not change CI/CD, release configuration, files under `.github/`, `SECURITY.md`, `LICENSE`, `THIRD_PARTY_NOTICES.md`, `.env` files, or the `cudnn-frontend` submodule revision unless the task explicitly requires it.

Always:

- Read and follow [CONTRIBUTING.md](CONTRIBUTING.md).
- Preserve optional build modes and avoid assuming every extension is installed.
- Preserve component-specific copyright, license, and attribution notices.
- Never commit credentials, API keys, private URLs, or secrets.
- Do not add roadmap, support, security, release, or review-time commitments without maintainer confirmation.
- Keep README, CONTRIBUTING, SECURITY, SUPPORT, governance, and issue guidance consistent when changing public policy.
- Do not disclose security vulnerabilities publicly; follow [SECURITY.md](SECURITY.md).
- Keep changes focused and explain compatibility or performance consequences.
