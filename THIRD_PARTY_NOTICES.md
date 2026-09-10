# Third-Party Notices for Apex

Apex as a whole is distributed under the BSD 3-Clause License in [LICENSE](LICENSE). The repository also contains or references components with their own copyright and license terms. This inventory records those terms and does not replace the controlling text retained in the affected files or submodule.

## Apache Software Foundation group batch normalization sources

- Files:
  - `apex/contrib/csrc/groupbn/batch_norm.h`
  - `apex/contrib/csrc/groupbn/batch_norm_add_relu.h`
  - `apex/contrib/csrc/groupbn/nhwc_batch_norm_kernel.h`
- Integration revision: Apex commit [`fedfe0d7159711198a77ca1a6ba8cc20d665ddce`](https://github.com/NVIDIA/apex/commit/fedfe0d7159711198a77ca1a6ba8cc20d665ddce)
- Copyright: Copyright (c) 2018 by Contributors
- License: [Apache License 2.0](https://www.apache.org/licenses/LICENSE-2.0)
- Source: the retained headers identify the Apache Software Foundation and use Apache MXNet identifiers. Apex history does not identify an exact upstream MXNet revision.
- Use in this distribution: source for the optional group batch-normalization extension.

The retained headers direct recipients to the NOTICE distributed with the work. The Apache MXNet notice contemporaneous with the Apex integration states:

> Apache MXNET (incubating)
>
> Copyright 2017-2018 The Apache Software Foundation
>
> This product includes software developed at
>
> The Apache Software Foundation (http://www.apache.org/).

See the [Apache MXNet 1.4.1 NOTICE](https://github.com/apache/mxnet/blob/1.4.1/NOTICE). Preserve the complete license headers in the listed files.

## PyTorch and Caffe2 softmax cross-entropy source

- File: `apex/contrib/csrc/xentropy/xentropy_kernel.cu`
- Integration revision: Apex commit [`0c74571f3407bf020a1b65a70548e20f34c62162`](https://github.com/NVIDIA/apex/commit/0c74571f3407bf020a1b65a70548e20f34c62162)
- License: BSD 3-Clause terms retained at the top of the source file
- Source: [PyTorch](https://github.com/pytorch/pytorch) and Caffe2
- Use in this distribution: source for the optional softmax cross-entropy extension.

The source file preserves these copyright notices:

> Copyright (c) 2016- Facebook, Inc. (Adam Paszke)
> Copyright (c) 2014- Facebook, Inc. (Soumith Chintala)
> Copyright (c) 2011-2014 Idiap Research Institute (Ronan Collobert)
> Copyright (c) 2012-2014 Deepmind Technologies (Koray Kavukcuoglu)
> Copyright (c) 2011-2012 NEC Laboratories America (Koray Kavukcuoglu)
> Copyright (c) 2011-2013 NYU (Clement Farabet)
> Copyright (c) 2006-2010 NEC Laboratories America (Ronan Collobert, Leon Bottou, Iain Melvin, Jason Weston)
> Copyright (c) 2006 Idiap Research Institute (Samy Bengio)
> Copyright (c) 2001-2004 Idiap Research Institute (Ronan Collobert, Samy Bengio, Johnny Mariethoz)
> Copyright (c) 2016-present, Facebook Inc. All rights reserved.
> Copyright (c) 2016 Facebook Inc.
> Copyright (c) 2015 Google Inc. All rights reserved.
> Copyright (c) 2015 Yangqing Jia. All rights reserved.
> Copyright (c) 2013, 2014, 2015, the respective Caffe contributors. All rights reserved.
> Copyright (c) 2015, 2016, the respective other contributors. All rights reserved.

Redistribution in source or binary form must preserve the complete copyright, license conditions, and disclaimer at the top of `apex/contrib/csrc/xentropy/xentropy_kernel.cu`. The names of the copyright holders and contributors may not be used to endorse derived products without prior written permission.

## cuDNN group batch normalization sample

- Files:
  - `apex/contrib/csrc/cudnn_gbn/norm_sample.cpp`
  - `apex/contrib/csrc/cudnn_gbn/norm_sample.h`
- Integration revision: Apex commit [`7a12ee65d872f6e6279adab908ec72fe9da6638e`](https://github.com/NVIDIA/apex/commit/7a12ee65d872f6e6279adab908ec72fe9da6638e)
- Copyright: Copyright (c) 2020, NVIDIA CORPORATION. All rights reserved.
- License: MIT-style terms retained at the top of both files
- Use in this distribution: source for the optional cuDNN group batch-normalization extension.

The license permits use, copying, modification, distribution, sublicensing, and sale, provided that the copyright and permission notice are included in all copies or substantial portions. The software is provided without warranty. Preserve the complete notice and disclaimer in both files.

## Separately licensed NVIDIA Apache-2.0 sources

The following sources carry their own NVIDIA copyright notices and Apache License 2.0 headers:

- `csrc/megatron/` fused softmax and rotary positional-embedding sources
- `apex/contrib/csrc/nccl_p2p/nccl_p2p.cpp`
- `apex/contrib/csrc/nccl_p2p/nccl_p2p_cuda.cuh`
- `apex/contrib/csrc/peer_memory/peer_memory.cpp`
- `apex/contrib/csrc/peer_memory/peer_memory_cuda.cuh`
- `apex/contrib/test/openfold_triton/test_fused_adam_swa.py`

These files are used as Apex extension or test sources. Preserve their headers and comply with the [Apache License 2.0](https://www.apache.org/licenses/LICENSE-2.0).

## cuDNN Frontend submodule

- Path: `apex/contrib/csrc/cudnn-frontend`
- Revision: [`171a7a986f7fbd9ed71bd0cf3c7ad4f55843d6b3`](https://github.com/NVIDIA/cudnn-frontend/tree/171a7a986f7fbd9ed71bd0cf3c7ad4f55843d6b3)
- Copyright: Copyright (c) 2020, NVIDIA CORPORATION. All rights reserved.
- License: MIT-style terms in the submodule's [`LICENSE.txt`](https://github.com/NVIDIA/cudnn-frontend/blob/171a7a986f7fbd9ed71bd0cf3c7ad4f55843d6b3/LICENSE.txt)
- Source: [NVIDIA/cudnn-frontend](https://github.com/NVIDIA/cudnn-frontend)
- Use in this distribution: optional Git submodule used when building cuDNN-based extensions.

The pinned submodule also contains nlohmann JSON under the MIT License. Its copyright and license are retained in [`include/contrib/nlohmann/json/LICENSE.txt`](https://github.com/NVIDIA/cudnn-frontend/blob/171a7a986f7fbd9ed71bd0cf3c7ad4f55843d6b3/include/contrib/nlohmann/json/LICENSE.txt). Preserve both submodule license files when redistributing initialized submodule content.

## Package dependencies

Python packages listed in `requirements.txt`, `requirements_dev.txt`, and `setup.py` are obtained separately from their package sources rather than copied into this repository. Their own licenses apply when they are installed or redistributed.
