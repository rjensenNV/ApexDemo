# Apex

Apex is a PyTorch extension for researchers and engineers who need NVIDIA-maintained mixed-precision, distributed-training, normalization, and optimizer utilities.

The project is active and the software is stable. Individual `apex.contrib` modules and the Windows build remain experimental and have their own compatibility constraints. Community support is provided on a best-effort basis, while eligible users of supported NVIDIA PyTorch container configurations can use their NVIDIA Enterprise Support channel. External code and documentation contributions are welcome; see [CONTRIBUTING.md](CONTRIBUTING.md).

## Overview

Some Apex functionality is incorporated into upstream PyTorch over time. Apex makes current NVIDIA GPU training utilities available while that work is developed and integrated.

Key capabilities include:

- Automatic and explicit mixed-precision training utilities.
- Fused optimizers and normalization operations.
- Distributed-training helpers.
- Optional C++ and CUDA extensions for performance-critical operations.
- Specialized experimental modules under `apex.contrib`.

## Getting started

For the shortest installation path, create an environment with PyTorch and a discoverable CUDA toolkit (`CUDA_HOME` and `nvcc`) and use the Python-only build:

```bash
git clone https://github.com/rjensenNV/ApexDemo.git
cd ApexDemo
python -m pip install -v --disable-pip-version-check --no-build-isolation --no-cache-dir .
python -c "import apex; print(apex.__file__)"
```

The final command prints the installed `apex` package path. The Python-only build omits the optional compiled extensions; use the Linux instructions below for full functionality and performance.

## Requirements

- Python with a compatible PyTorch installation. `requirements.txt` currently specifies PyTorch 2.6.0 or newer for repository development.
- The current source installer queries `nvcc` during metadata generation, so even the Python-only build requires a discoverable CUDA toolkit. A compatible NVIDIA GPU, driver, and C++ toolchain are additionally required to build and use the full extensions.
- [Ninja](https://ninja-build.org/) is recommended for faster compilation.
- Windows support is experimental.
- The [NVIDIA PyTorch container](https://catalog.ngc.nvidia.com/orgs/nvidia/containers/pytorch) provides a known integrated environment with Apex extensions.

## Installation
Each [`apex.contrib`](./apex/contrib) module requires one or more install options other than `--cpp_ext` and `--cuda_ext`.
Note that contrib modules do not necessarily support stable PyTorch releases, some of them might only be compatible with nightlies.

### Containers
NVIDIA PyTorch Containers are available on NGC: https://catalog.ngc.nvidia.com/orgs/nvidia/containers/pytorch.
The containers come with all the custom extensions available at the moment. 

See [the NGC documentation](https://docs.nvidia.com/deeplearning/frameworks/pytorch-release-notes/index.html) for details such as:
- how to pull a container
- how to run a pulled container
- release notes

### From Source

To install Apex from source, we recommend using the nightly Pytorch obtainable from https://github.com/pytorch/pytorch.

The latest stable release obtainable from https://pytorch.org should also work.

We recommend installing [`Ninja`](https://ninja-build.org/) to make compilation faster.

#### Linux

For performance and full functionality, we recommend installing Apex with CUDA and C++ extensions using environment variables:

##### Using Environment Variables (Recommended)

```bash
git clone --recurse-submodules https://github.com/rjensenNV/ApexDemo.git
cd ApexDemo
# Build with core extensions (cpp and cuda)
APEX_CPP_EXT=1 APEX_CUDA_EXT=1 pip install -v --no-build-isolation .

# To build with additional extensions, specify them with environment variables
APEX_CPP_EXT=1 APEX_CUDA_EXT=1 APEX_FUSED_CONV_BIAS_RELU=1 pip install -v --no-build-isolation .

# To build all contrib extensions at once
APEX_CPP_EXT=1 APEX_CUDA_EXT=1 APEX_ALL_CONTRIB_EXT=1 pip install -v --no-build-isolation .
```

To reduce the build time, parallel building can be enabled:

```bash
NVCC_APPEND_FLAGS="--threads 4" APEX_PARALLEL_BUILD=8 APEX_CPP_EXT=1 APEX_CUDA_EXT=1 pip install -v --no-build-isolation .
```

When CPU cores or memory are limited, the `--parallel` option is generally preferred over `--threads`. See [pull#1882](https://github.com/NVIDIA/apex/pull/1882) for more details.

##### Using Command-Line Flags (Legacy Method)

The traditional command-line flags are still supported:

```bash
# Using pip config-settings (pip >= 23.1)
pip install -v --disable-pip-version-check --no-cache-dir --no-build-isolation --config-settings "--build-option=--cpp_ext" --config-settings "--build-option=--cuda_ext" ./

# For older pip versions
pip install -v --disable-pip-version-check --no-cache-dir --no-build-isolation --global-option="--cpp_ext" --global-option="--cuda_ext" ./

# To build with additional extensions
pip install -v --disable-pip-version-check --no-cache-dir --no-build-isolation --global-option="--cpp_ext" --global-option="--cuda_ext" --global-option="--group_norm" ./
```

##### Python-Only Build

APEX also supports a Python-only build via:
```bash
pip install -v --disable-pip-version-check --no-build-isolation --no-cache-dir ./
```
A discoverable CUDA toolkit is still required because the current installer queries `nvcc` during package metadata generation.

A Python-only build omits:
- Fused kernels required to use `apex.optimizers.FusedAdam`.
- Fused kernels required to use `apex.normalization.FusedLayerNorm` and `apex.normalization.FusedRMSNorm`.
- Fused kernels that improve the performance and numerical stability of `apex.parallel.SyncBatchNorm`.
- Fused kernels that improve the performance of `apex.parallel.DistributedDataParallel` and `apex.amp`.
`DistributedDataParallel`, `amp`, and `SyncBatchNorm` will still be usable, but they may be slower.


#### [Experimental] Windows
`pip install -v --disable-pip-version-check --no-cache-dir --no-build-isolation --config-settings "--build-option=--cpp_ext" --config-settings "--build-option=--cuda_ext" .` may work if you were able to build Pytorch from source
on your system. A Python-only build via `pip install -v --no-cache-dir .` is more likely to work.  
If you installed Pytorch in a Conda environment, make sure to install Apex in that same environment.


### Custom C++/CUDA Extensions and Install Options

If a requirement of a module is not met, then it will not be built.

|  Module Name  |  Environment Variable  |  Install Option  |  Misc  |
|---------------|------------------------|------------------|--------|
|  `apex_C`     |  `APEX_CPP_EXT=1`      |  `--cpp_ext`     | |
|  `amp_C`      |  `APEX_CUDA_EXT=1`     |  `--cuda_ext`    | |
|  `syncbn`     |  `APEX_CUDA_EXT=1`     |  `--cuda_ext`    | |
|  `fused_layer_norm_cuda`  |  `APEX_CUDA_EXT=1`  |  `--cuda_ext`  | [`apex.normalization`](./apex/normalization) |
|  `mlp_cuda`   |  `APEX_CUDA_EXT=1`     |  `--cuda_ext`    | |
|  `scaled_upper_triang_masked_softmax_cuda`  |  `APEX_CUDA_EXT=1`  |  `--cuda_ext`  | |
|  `generic_scaled_masked_softmax_cuda`  |  `APEX_CUDA_EXT=1`  |  `--cuda_ext`  | |
|  `scaled_masked_softmax_cuda`  |  `APEX_CUDA_EXT=1`  |  `--cuda_ext`  | |
|  `fused_weight_gradient_mlp_cuda`  |  `APEX_CUDA_EXT=1`  |  `--cuda_ext`  | Requires CUDA>=11 |
|  `permutation_search_cuda`  |  `APEX_PERMUTATION_SEARCH=1`  |  `--permutation_search`  | [`apex.contrib.sparsity`](./apex/contrib/sparsity)  |
|  `bnp`        |  `APEX_BNP=1`          |  `--bnp`         |  [`apex.contrib.groupbn`](./apex/contrib/groupbn) |
|  `xentropy`   |  `APEX_XENTROPY=1`     |  `--xentropy`    |  [`apex.contrib.xentropy`](./apex/contrib/xentropy)  |
|  `focal_loss_cuda`  |  `APEX_FOCAL_LOSS=1`  |  `--focal_loss`  |  [`apex.contrib.focal_loss`](./apex/contrib/focal_loss)  |
|  `fused_index_mul_2d`  |  `APEX_INDEX_MUL_2D=1`  |  `--index_mul_2d`  |  [`apex.contrib.index_mul_2d`](./apex/contrib/index_mul_2d)  |
|  `fused_adam_cuda`  |  `APEX_DEPRECATED_FUSED_ADAM=1`  |  `--deprecated_fused_adam`  |  [`apex.contrib.optimizers`](./apex/contrib/optimizers)  |
|  `fused_lamb_cuda`  |  `APEX_DEPRECATED_FUSED_LAMB=1`  |  `--deprecated_fused_lamb`  |  [`apex.contrib.optimizers`](./apex/contrib/optimizers)  |
|  `fast_layer_norm`  |  `APEX_FAST_LAYER_NORM=1`  |  `--fast_layer_norm`  |  [`apex.contrib.layer_norm`](./apex/contrib/layer_norm). different from `fused_layer_norm` |
|  `transducer_joint_cuda`  |  `APEX_TRANSDUCER=1`  |  `--transducer`  |  [`apex.contrib.transducer`](./apex/contrib/transducer)  |
|  `transducer_loss_cuda`   |  `APEX_TRANSDUCER=1`  |  `--transducer`  |  [`apex.contrib.transducer`](./apex/contrib/transducer)  |
|  `cudnn_gbn_lib`  |  `APEX_CUDNN_GBN=1`  |  `--cudnn_gbn`  | Requires cuDNN>=8.5, [`apex.contrib.cudnn_gbn`](./apex/contrib/cudnn_gbn) |
|  `peer_memory_cuda`  |  `APEX_PEER_MEMORY=1`  |  `--peer_memory`  |  [`apex.contrib.peer_memory`](./apex/contrib/peer_memory)  |
|  `nccl_p2p_cuda`  |  `APEX_NCCL_P2P=1`  |  `--nccl_p2p`  | Requires NCCL >= 2.10, [NCCL P2P sources](./apex/contrib/csrc/nccl_p2p)  |
|  `fast_bottleneck`  |  `APEX_FAST_BOTTLENECK=1`  |  `--fast_bottleneck`  |  Requires `peer_memory_cuda` and `nccl_p2p_cuda`, [`apex.contrib.bottleneck`](./apex/contrib/bottleneck) |
|  `fused_conv_bias_relu`  |  `APEX_FUSED_CONV_BIAS_RELU=1`  |  `--fused_conv_bias_relu`  | Requires cuDNN>=8.4, [`apex.contrib.conv_bias_relu`](./apex/contrib/conv_bias_relu) |
|  `distributed_adam_cuda`  |  `APEX_DISTRIBUTED_ADAM=1`  |  `--distributed_adam`  |  [`apex.contrib.optimizers`](./apex/contrib/optimizers)  |
|  `distributed_lamb_cuda`  |  `APEX_DISTRIBUTED_LAMB=1`  |  `--distributed_lamb`  |  [`apex.contrib.optimizers`](./apex/contrib/optimizers)  |
|  `_apex_nccl_allocator`  |  `APEX_NCCL_ALLOCATOR=1`  |  `--nccl_allocator`  | Requires NCCL >= 2.19, [`apex.contrib.nccl_allocator`](./apex/contrib/nccl_allocator)  |

You can also build all contrib extensions at once by setting `APEX_ALL_CONTRIB_EXT=1`.

## Usage

The extension selected at installation determines which Apex modules are available. For example, a full CUDA build can use a fused optimizer:

```python
import torch
from apex.optimizers import FusedAdam

model = torch.nn.Linear(8, 4).cuda()
optimizer = FusedAdam(model.parameters(), lr=1e-3)
```

See the repository [examples](examples/) for mixed-precision and distributed-training programs, including ImageNet, DCGAN, and distributed examples.

## Documentation

- [Published documentation](https://rjensennv.github.io/ApexDemo/)
- [Documentation source](docs/source/)
- [Examples and tutorials](examples/)

## Support and contributions

- [Report a reproducible bug](https://github.com/rjensenNV/ApexDemo/issues/new?template=01_bug_report.yml).
- Use [GitHub Issues](https://github.com/rjensenNV/ApexDemo/issues) for best-effort community questions and feature proposals. Discussions are not enabled.
- Community requests are reviewed on a best-effort basis; response times vary and a response is not guaranteed.
- Eligible users running Apex in a supported NVIDIA PyTorch container configuration should use the NVIDIA support channel provided under their agreement. Consult the [NVIDIA Frameworks Support Matrix](https://docs.nvidia.com/deeplearning/frameworks/support-matrix/) for covered configurations.
- Code and documentation pull requests are welcome. Discuss substantial, compatibility-sensitive, or performance-sensitive changes in an issue before implementation.

See [CONTRIBUTING.md](CONTRIBUTING.md) for contribution guidance and [SUPPORT.md](SUPPORT.md) for support boundaries and routes. All project participants must follow the [Code of Conduct](CODE_OF_CONDUCT.md).

## Known limitations

- A Python-only build omits fused kernels and may be slower, as detailed in [Python-Only Build](#python-only-build).
- `apex.contrib` modules can require additional build flags and may depend on nightly PyTorch versions.
- Windows support is experimental.
- Compiled-extension compatibility depends on the installed PyTorch, CUDA toolkit, compiler, NVIDIA driver, and GPU architecture.

## Governance and maintainers

- [Governance and decision process](GOVERNANCE.md)
- [Current maintainers](MAINTAINERS.md)

## Security

Do not report security vulnerabilities through public GitHub issues or pull requests. Follow [SECURITY.md](SECURITY.md) to report them privately to NVIDIA PSIRT.

## License

Apex is open-source software licensed under the BSD 3-Clause License. See [LICENSE](LICENSE) for the controlling terms.

Some included or referenced components carry separate license and attribution terms. See [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md); the notices do not replace the license text in the affected source files or submodules.
