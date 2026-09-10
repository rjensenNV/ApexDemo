# Support for Apex

Community support for the Apex source repository is provided on a best-effort basis. GitHub Issues are reviewed as maintainer capacity allows; response times vary, and a response or resolution is not guaranteed. Commercial support, where applicable, is separate from this community channel and is governed by the customer's NVIDIA support agreement and the supported product configuration.

## Supported versions and environments

- GitHub reports should reproduce against the current `master` branch when practical and identify the exact commit tested. This repository does not publish a separate long-term-support schedule for source builds.
- Full C++ and CUDA functionality is intended for Linux with a compatible NVIDIA GPU, driver, CUDA toolkit, compiler, and PyTorch installation. Windows support is experimental.
- NVIDIA PyTorch containers provide integrated Apex builds. Consult the [NVIDIA Frameworks Support Matrix](https://docs.nvidia.com/deeplearning/frameworks/support-matrix/) and the applicable [PyTorch container release notes](https://docs.nvidia.com/deeplearning/frameworks/pytorch-release-notes/) for container-specific compatibility.
- `apex.contrib` modules can require additional build options and might require nightly PyTorch versions. Arbitrary source, dependency, compiler, operating-system, and hardware combinations are not guaranteed to work.

## Request routes

- Reproducible bugs: use the [Apex bug form](https://github.com/rjensenNV/ApexDemo/issues/new?template=01_bug_report.yml).
- Usage questions: search or open a [GitHub issue](https://github.com/rjensenNV/ApexDemo/issues). This is a best-effort channel, not a guaranteed help desk.
- Feature proposals: open a [GitHub issue](https://github.com/rjensenNV/ApexDemo/issues/new) before beginning substantial work.
- NVIDIA product support: eligible customers should use the support channel provided under their NVIDIA agreement and verify coverage in the [NVIDIA Frameworks Support Matrix](https://docs.nvidia.com/deeplearning/frameworks/support-matrix/).

## Security vulnerabilities

Do not report security vulnerabilities through public issues or pull requests. Follow [SECURITY.md](SECURITY.md) to report them privately.
