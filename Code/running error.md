# running error

> 目前运行失败，但环境配置正常，推测代码问题。

该代码分为 **ImageAliment** 和 **ImageReconstruction** 两部分，现在还 block 在第一部分。 
根据报错信息进行debug，目前可能存在的问题有：
### GPU 资源使用率
> 这里会补贴为什么该方案可能有效的url  

    config.gpu_options.per_process_gpu_memory_fraction = 0.8  # 程序最多只能占用指定 GPU 80%的显存
    config.gpu_options.allow_growth = True  # 程序按序申请显存
根据 google，我在源代码上新增了上述代码，应该解决了问题，可惜并没有...

### BatchSize
> 这里也会再补贴为何该方案可能有效的url

在源代码中我也修改了 `TRAIN_BATCH_SIZE` ，可惜依然不行...