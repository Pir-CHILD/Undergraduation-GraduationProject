# running error

> 目前运行失败，但环境配置正常，推测代码问题。

配置问题可参考：
1. [理清GPU、CUDA、CUDA Toolkit、cuDNN关系以及下载安装](https://blog.csdn.net/qq_42406643/article/details/109545766)
2. [win10+RTX2070+tensorflow-gpu+cuda9.0+pycharm配置小结](https://www.jianshu.com/p/8f774b7866c7)

该代码分为 **ImageAliment** 和 **ImageReconstruction** 两部分，现在还 block 在第一部分。 
根据报错信息进行debug，目前可能存在的问题有：
### GPU 资源使用率
运行时报错:    

    InternalError (see above for traceback): Blas xGEMM launch failed : a.shape=[1,3,3], b.shape=[1,3,16384], m=3, n=16384, k=3    

其中**Blas xGEMM launch failed**根据搜索，得到如下解决方案：
1. [keras 或 tensorflow 调用GPU报错：Blas GEMM launch failed](https://blog.csdn.net/Leo_Xu06/article/details/82023330)
2. [TensorFlow: Blas GEMM launch failed](https://stackoverflow.com/questions/43990046/tensorflow-blas-gemm-launch-failed)

上面两个方案均表示为显存分配问题，故添加了如下代码：

    config.gpu_options.per_process_gpu_memory_fraction = 0.8  # 程序最多只能占用指定 GPU 80%的显存
    config.gpu_options.allow_growth = True  # 程序按序申请显存

应该可以解决问题，可惜并没有...

### BatchSize
> 这里会补贴为什么该方案可能有效的url  

在源代码中我也修改了 `TRAIN_BATCH_SIZE` ，可惜依然不行...