
# TensorRT
- What's the tesnsorRT
   - NVIDIA TensorRT is an SDK for optimizing trained deep learning models to enable high-performance inference.
   - TensorRT contains a deep learning inference optimizer for trained deep learning models, and a runtime for execution.
   - Pipeline
     ![pipeline](https://github.com/lililuya/break-stones/assets/141640497/c2a90491-7c2e-41a4-b4f7-4e3254374185)
- WorkFlow
     ![yuque_diagram](https://github.com/lililuya/break-stones/assets/141640497/bae077f2-0358-4d2e-ac95-2aaa9a421b7a)



## 1.Installation
- **My OS Enviroment**
```bash
Distributor ID: Ubuntu
Description:    Ubuntu 20.04.6 LTS
Release:        20.04
Codename:       focal
```

- **My Cuda Enviroment**
```bash
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2023 NVIDIA Corporation
Built on ****
Cuda compilation tools, release 12.1, V12.1.66
Build cuda_12.1.r12.1/compiler.*

cuDNN version: 8.9.0.2
pytorch version: 2.2.2
```
### 1.1.Type of Installation

- Debian install
- RPM install
- Tar install
### 1.2 Choose "Tar" Installation

- **Official reference website：**[https://docs.nvidia.com/deeplearning/tensorrt/install-guide/index.html#installing-tar](https://docs.nvidia.com/deeplearning/tensorrt/install-guide/index.html#installing-tar)
- **1.2.1** 
   - Dependencies installed
      - CUDA (special version include as 11.0, 11.1, ... , 12.2....)
      - cuDNN (optional)
      - python (optional)
- **1.2.2** 
   - Download the TensorRT tar file matches CPU architecture and CUDA version
- **1.2.3**
   - Unpack the tar file
```python
version="10.x.x.x"
arch=$(uname -m)
cuda="cuda-x.x"
tar -xzvf TensorRT-${version}.Linux.${arch}-gnu.${cuda}.tar.gz
```

- **1.2.4**
   - Add absolute path to TensorRt lib directory to environment variable LD_LIBRARY_PATH
   - Add the following line code to 
      - `/etc/profile`
      - `~/.bashrc  (to be on the safe side, save a copy one)`
```bash
export LD_LIBRARY_PATH=<TensorRT-${version}/lib>:$LD_LIBRARY_PATH
```

- **1.2.5**
   - Install the Python TensorRt wheel file
```bash
cd TensorRT-${version}/python
python3 -m pip install tensorrt-*-cp3x-none-linux_x86_64.whl
```

   - Optionally
```python
python3 -m pip install tensorrt_lean-*-cp3x-none-linux_x86_64.whl
python3 -m pip install tensorrt_dispatch-*-cp3x-none-linux_x86_64.whl
```

- **1.2.6**
   - Install the Python onnx-graphsurgeon wheel
```python
cd TensorRT-${version}/onnx_graphsurgeon
python3 -m pip install onnx_graphsurgeon-0.5.0-py2.py3-none-any.whl
```
## 2.Testing
### 2.1 Test MNIST

- `cd TensorRT-${version}/samples/sampleOnnxMNIST`	
- `make`
   - a exe file will generate in`TensorRT-${version}/targets/x_64-linux-gnu/bin/ `
   - execute `./sample_onnx_mnist`will show the result.

![image.png](https://cdn.nlark.com/yuque/0/2024/png/34805283/1712425722353-84e57218-a921-4b92-8ed2-57b9e15cf345.png#averageHue=%232e3541&clientId=u28c8fe4f-ba6f-4&from=paste&height=589&id=ua8638230&originHeight=589&originWidth=476&originalType=binary&ratio=1&rotation=0&showTitle=false&size=16197&status=done&style=none&taskId=ud4037ae8-32a7-424c-8100-39fdfaf6926&title=&width=476)
## 3.Some problem(Cause by cuda 12.2)
### 3.1 The First Problem 

- onnxruntime version
> /onnxruntime_src/onnxruntime/core/session/provider_bridge_ort.cc:1209 onnxruntime::Provider& onnxruntime::ProviderLibrary::Get() [ONNXRuntimeError] : 1 : FAIL : Failed to load library libonnxruntime_providers_tensorrt.so with error: libcublas.so.11: cannot open shared object file: No such file or directory

- Reason
   - **onnxruntime's version does not match the TensorRT's one**
- Refer the relationship
   - [https://onnxruntime.ai/docs/execution-providers/TensorRT-ExecutionProvider.html#requirements](https://onnxruntime.ai/docs/execution-providers/TensorRT-ExecutionProvider.html#requirements)
   - Correlation
   - 
   | ONNX Runtime | TensorRT | CUDA |
   | --- | --- | --- |
   | 1.17-main | 8.6 | 11.8, 12.2 |
   | 1.16 | 8.6 | 11.8 |
   | 1.15 | 8.6 | 11.8 |
   | 1.14 | 8.5 | 11.6 |
   | 1.12-1.13 | 8.4 | 11.4 |
   | 1.11 | 8.2 | 11.4 |
   | 1.10 | 8.0 | 11.4 |
   | 1.9 | 8.0 | 11.4 |
   | 1.7-1.8 | 7.2 | 11.0.3 |
   | 1.5-1.6 | 7.1 | 10.2 |
   | 1.2-1.4 | 7.0 | 10.1 |
   | 1.0-1.1 | 6.0 | 10.0 |

- **Solution**
   - [https://stackoverflow.com/questions/77951682/onnx-runtime-io-binding-bind-input-causing-no-data-transfer-from-devicetype1](https://stackoverflow.com/questions/77951682/onnx-runtime-io-binding-bind-input-causing-no-data-transfer-from-devicetype1)
   - **Install a specific version of onnxruntime**
      - `pip install onnxruntime-gpu --extra-index-url https://aiinfra.pkgs.visualstudio.com/PublicPackages/_packaging/onnxruntime-cuda-12/pypi/simple/` 
      - also can refer [https://aiinfra.visualstudio.com/PublicPackages/_artifacts/feed/onnxruntime-cuda-12/PyPI/onnxruntime-gpu/overview/1.17.1](https://aiinfra.visualstudio.com/PublicPackages/_artifacts/feed/onnxruntime-cuda-12/PyPI/onnxruntime-gpu/overview/1.17.1) to get the resource 
### 3.2 The Second Problem

- CUDAExecutionProvider error
> Why does onnxruntime fail to create CUDAExecutionProvider in Linux(Ubuntu 20)?

- **Solution**
   - [**https://onnxruntime.ai/docs/execution-providers/CUDA-ExecutionProvider.html#requirements**](https://onnxruntime.ai/docs/execution-providers/CUDA-ExecutionProvider.html#requirements)
   - replace `import onnxruntime as rt` with  `import torch` and ` import onnxruntime as rt`


