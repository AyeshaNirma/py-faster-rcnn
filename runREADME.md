### Introduction

[README.md](https://github.com/OneDirection9/py-faster-rcnn/blob/master/README.md) may not be so helpful when you try to run py-faster-rcnn on windows, so I write down my installation process. Hope to help beginners.

# py-faster-rcnn on windows

You can see my environment on the top of [README.md](https://github.com/OneDirection9/py-faster-rcnn/blob/master/README.md).

I refer the [caffe installation](http://www.cnblogs.com/LaplaceAkuir/p/6445189.html) and [windows py-faster-rcnn config](http://www.cnblogs.com/LaplaceAkuir/p/6484500.html).

### Contents

1. [Cudnn installation](#cudnn-installation)
2. [Caffe installation](#caffe-installation)
3. [Faster rcnn installation](#faster-rcnn-installation)
4. [Demo](#demo)
5. [Problems and solutions](#problems-and-solutions)

### Cudnn installation

Download the [zip](https://developer.nvidia.com/rdp/cudnn-download) and extract it. Then copy the files to corresponding location in `C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v7.5`.
If you do that, you don't need to add `$Cudnn_PATH` to environment variables.

### Caffe installation

I use the [Microsoft caffe](https://github.com/Microsoft/caffe) and assume that you have installed VS2013, CUDA7.5, and e.g.

1. Clone [Microsoft caffe](https://github.com/Microsoft/caffe) from github

   ```make
   # recommond u to use --recursive
   git clone --recursive https://github.com/Microsoft/caffe.git
   ```

2. Copy `caffe/windows/CommonSettings.props.example` to `caffe/windows/CommonSettings.props`

   Modify `caffe/windows/CommonSetting.props` with sublime or other text editor. You can refer to the [Microsoft caffe README.md](https://github.com/Microsoft/caffe/blob/master/README.md) for details.

3. Install packages
  
   Install some python dependency packages.
  
   ```make
   conda install --yes numpy scipy matplotlib scikit-image pip
   pip install protobuf
   ```
  
4. Config faster-rcnn

   Due to the use of `roi_pooling_layer` in `faster-rcnn`, we need to add `.hpp`, `.cu`, and `.cpp` files to the `libcaffe`. Then rebuild the whole Solution and wait for the end of compile.
  
   - set PythonPath environment variable to point to `<caffe_root>\Build\x64\Release\pycaffe`, or
   - copy folder `<caffe_root>\Build\x64\Release\pycaffe\caffe` under `<python_root>\lib\site-packages`.
   
   Otherwise you may meet: No module named caffe.
   
### Faster rcnn installation

1. Clone [py-faster-rcnn](https://github.com/rbgirshick/py-faster-rcnn) from github
  
   ```make
   # recommond u use --recursive
   # because the py-faster-rcnn repository has a submodules.
   git clone --recursive https://github.com/rbgirshick/py-faster-rcnn.git
   ```
  
2. Clone [lib](https://github.com/MrGF/py-faster-rcnn-windows) from github

   Download the lib and replace it with the original lib provied by rbgirshick.
   
   ```make
   git clone https://github.com/MrGF/py-faster-rcnn-windows.git
   ```
  
3. Copy `caffe` to `py-faster-rcnn`
   
   Copy files under `<caffe_root>\Build\zx64\Release\pycaffe\` you have built to `<py-faster-rcnn_root>\caffe-fast-rcnn\python\`.

4. Install depency packages

   ```make
   conda install numpy pyqt
   ```
  
5. Run
   
   ```make
   cd <py-faster-rcnn_root>\lib\
   python setup.py install
   ```
   
   Modify the cuda path in line 33 setup_cuda.py file.
   
   ```make
   python setup_cuda.py install
   ```
  
### Demo

To run demo you need download prebuild [models](http://www.cs.berkeley.edu/~rbg/faster-rcnn-data). Extract it and move the folder to `<py-faster-rcnn_root>\data\`.

```make
cd $py-faster-rcnn_root
python ./tools/demo.py
```

You will see the demo pictures. Congratulations.

### Problems and solutions

I will list some problems I meet. Hope it helpful to you.

1. **error: Microsoft Visual C++ 9.0 is required. Get it from aka.ms/vcpython27**
   
   You may meet the error when you run `python setup.py install` or `python setup_cuda.py isntall`. In my experience downloading it according to the hint is not the right way. You can try the following command:
   
   ```make
   # open command line
   # The set value is depend on your visula studio version.
   # visual studio 2013
   SET VS90COMNTOOLS=%VS120COMNTOOLS%
  
   # # visual studio 2015
   # SET VS90COMNTOOLS=%VS140COMNTOOLS%
   # # visual studio 2012
   # SET VS90COMNTOOLS=%VS110COMNTOOLS%
   # # visual studio 2010
   # SET VS90COMNTOOLS=%VS100COMNTOOLS%
   ```
  
   If you have downloaded Visual C++ 9.0, uninstall it and try command again. I have downloaded it and run `python setup.py install`, another error like `Cannot open include file: 'stdbool.h': No such file or directory error: command <Visual_C++_root>\\9.0\\VC\\Bin\\amd64\\cl.exe' failed with exit status2` will occur.
  
2. **TypeError: object of type 'NoneType' has no len()**

   ```make
   os.path.dirname(find_executable("cl.exe", PATH))
   File "D:\Anaconda2\lib\ntpath.py", line 215, in dirname
   return split(p)[0]
   File "D:\Anaconda2\lib\ntpath.py", line 180, in split
   d, p = splitdrive(p)
   File "D:\Anaconda2\lib\ntpath.py", line 115, in splitdrive
   if len(p) > 1:
   TypeError: object of type 'NoneType' has no len()
   ```
   
   To solve this problems, add the `C:\Program Files (x86)\Microsoft Visual Studio 12.0\VC\bin` to your Environment Variables 'PATH'. Note that it is work only when you add the path to the `PATH` not the others.

3. **AttributeError: ‘ProposalLayer’ object has no attribute ‘param_str_’**

   Open the `<py-faster-rcnn_root>\lib\rpn\proposal_layer.py` and modify the `param_str_` to 'param_str'.
   
4. **pre_nms_topN  = cfg[cfg_key].RPN_PRE_NMS_TOP_N KeyError: '1'**

   Modify the same file as before in line 64.

5. **No module named 'cv2'**
   
   Download [OpenCV](http://opencv.org/) and extract it. Then copy the `<opencv_root>\build\python\2.7\x64\cv2.pyd` to `<python_root>\Lib\site-packages`.
