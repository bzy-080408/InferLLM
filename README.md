# InferLLM 
[中文 README](./README_Chinese.md)

InferLLM is a lightweight LLM model inference framework that mainly references and borrows from the llama.cpp project. llama.cpp puts almost all core code and kernels in a single file and use a large number of macros, making it difficult for developers to read and modify. InferLLM has the following features:

- Simple structure, easy to get started and learning, and decoupled the framework part from the kernel part.
- High efficiency, ported most of the kernels in llama.cpp.
- Defined a dedicated KVstorage type for easy caching and management.
- Compatible with multiple model formats (currently only supporting alpaca Chinese and English int4 models).
- Currently supports CPU and GPU, optimized for Arm, x86, CUDA and riscv-vector. And it can be deployed on mobile phones, with acceptable speed.

In short, InferLLM is a simple and efficient LLM CPU inference framework that can deploy quantized models in LLM locally and has good inference speed.

## Latest News
- 2023.08.16: Add support for LLama-2-7B model.
- 2023.08.8: Optimized the performance on Arm, which optimized the int4 matmul kernel with arm asm and kernel packing.
- berfor: support chatglm/chatglm2, baichuan, alpaca, ggml-llama model.

## How to use
### Download model
Currently, InferLLM uses the same models as llama.cpp and can download models from the llama.cpp project. In addition, models can also be downloaded directly from Hugging Face [kewin4933/InferLLM-Model](https://huggingface.co/kewin4933/InferLLM-Model/tree/main). Currently, two alpaca, llama2, chatglm/chatglm2 and baichuan models are uploaded in this project, one is the Chinese int4 model and the other is the English int4 model.

### Compile InferLLM
#### Download Compiles:
cmake  
mingw w64  (thread)
#### Local compilation
```shell
mkdir build
cd build
cmake ..
make
```
GPU is disabled default, if you want to enable GPU, please use `cmake -DENABLE_GPU=ON ..` to enable GPU. Now only CUDA is supported, before use CUDA, please install CUDA toolkit first. 

#### Android cross compilation
According to the cross compilation, you can use the pre-prepared tools/android_build.sh script. You need to install NDK in advance and configure the path of NDK to the NDK_ROOT environment variable.
```shell
export NDK_ROOT=/path/to/ndk
./tools/android_build.sh
```
### Run InferLLM
Running ChatGLM model please refer to [ChatGLM model documentation](./application/chatglm/Readme.md).

If it is executed locally, execute `./chatglm -m chatglm-q4.bin -t 4` directly. If you want to execute it on your mobile phone, you can use the adb command to copy alpaca and the model file to your mobile phone, and then execute `adb shell ./chatglm -m chatglm-q4.bin -t 4`. 

The default device is CPU, if you want to inference with GPU, please use `./chatglm -m chatglm-q4.bin -g GPU` to specify the GPU device.

- x86 is：Intel(R) Xeon(R) CPU E5-2620 v4 @ 2.10GHz
![x86 running](./assets/ChatGLM-x86.gif )
- android is xiaomi9，Qualcomm SM8150 Snapdragon 855
![android running](./assets/arm-mi9.gif)
- CPU is SG2042, with riscv-vector 0.7, 64 threads
![sg2042 running](./assets/sg2042.gif)

According to [x86 profiling result](./docs/profile.md), we strongly advise using 4 threads.

### Supported model
Now InferLLM supports the fellowing models:
* [ChatGLM2-6B](https://github.com/THUDM/ChatGLM2-6B): usage please refer to [ChatGLM](./application/chatglm/Readme.md)
* [ChatGLM-6B](https://github.com/THUDM/ChatGLM-6B): usage please refer to [ChatGLM](./application/chatglm/Readme.md)
* [llama](https://github.com/facebookresearch/llama)
* [llama2](https://huggingface.co/meta-llama/Llama-2-7b-chat-hf)
* [alpaca](https://crfm.stanford.edu/2023/03/13/alpaca.html)
* [baichuan](https://github.com/baichuan-inc/baichuan-7B) : usage please refer to [baichuan](./application/baichuan/Readme.md)
### License
InferLLM is licensed under the Apache License, Version 2.0
