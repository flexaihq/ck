﻿# About

This portable CM script runs image classification
while automatically managing all related artifacts and adapting
them to your environment using the next generation of the CK framework (CM).

## Install CM

Please follow this [guide](https://github.com/mlcommons/ck/blob/master/docs/installation.md).

## Run this script on CPU

This modular image classification is [assembled](https://github.com/mlcommons/ck/blob/master/cm-mlops/script/app-image-classification-onnx-py/_cm.yaml#L19) 
from ([reusable and portable CM scripts](https://github.com/mlcommons/ck/tree/master/cm-mlops/script)).

It helps CM to automatically detect, download, install and build all related artifacts 
and tools to adapt this workflow to a user platform with Linux, Windows or MacOS:

```bash
cm pull repo mlcommons@ck

cm run script --tags=app,image-classification,onnx,python --quiet
```

or using Python scripting as a reusable micro-service:
```python
import cmind
r=cmind.access({'action':'run', 'automation':'script'
                'tags':'app,image-classification,onnx,python',
                'out':'con',
                'quiet':True})
print (r)
```

It may take a few minutes to run this workflow for the first time and adapt it to your platform (depending on the Internet speed).
Note that all the subsequent runs will be much faster because CM automatically caches the output of all portable CM scripts to be quickly reused
in this and other CM workflows.

## Manage individual tools and artifacts via CM

You can also force to install specific versions of ML artifacts 
(models, data sets, engines, libraries, tools, etc) 
using individual CM scripts to automatically plug them into the above ML task 
(see [image classification dependencies using CM database of scripts](https://github.com/mlcommons/ck/blob/master/cm-mlops/script/app-image-classification-onnx-py/_cm.json#L9)):

```bash
cm run script --tags=detect,os --out=json
cm run script --tags=get,python --version_min=3.9.1
cm run script --tags=install,python-venv --name=my-virtual-env
cm run script --tags=get,ml-model-onnx,resnet50
cm run script --tags=get,dataset,imagenet,original,_2012-500
cm run script --tags=get,onnxruntime,python-lib --version=1.12.0

cm show cache

cm run script --tags=app,image-classification,onnx,python (--input=my-image.jpg)
```

## Run this script with CUDA

```bash
cm run script "get cuda"
cm run script "get cuda-devices"

cm run script "python app image-classification onnx _cuda"
cm run script "python app image-classification onnx _cuda" --input=my-image.jpg
```

This script was tested on Windows 10 with
* ONNX Runtime 1.13.1 with CUDA
* CUDA 11.6
* cuDNN 8.5.0.96