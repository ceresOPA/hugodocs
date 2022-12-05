



> 生成模型时间线：GAN $\rightarrow$ AE $\rightarrow$ DAE $\rightarrow$ VAE $\rightarrow$ VQVAE $\rightarrow$ Diffusion Model



- GAN
  - 结构：Generator	Discriminator
  - 优点：能够生成非常逼真的图片，需要的数据量不多
  - 缺点：缺乏多样性，因为本身就是为了学习真实图片去优化的；网络不够稳定，因为需要平衡两个模型，训练不好容易坍塌；不是一个概率模型，生成都是隐式的，无法了解分布，在数学上不如VAE和diffusion model优美
- AE（auto-encoder）
  - 结构：$x \rightarrow encoder \rightarrow bottleneck \rightarrow decoder \rightarrow \hat{x} $
    - $x$和$\hat{x}$大小一样，中间特征提取的大小会小些，所以叫做$bottleneck$
    - 自己重建自己，所以是auto-encoder
- DAE（Denoise auto-encoder）
  - 结构：在训练的时候，使用的是加了噪声后的$x$，而不是原始的$x$，但是在还原的时候，是要还原出原始的$x$
  - 优点：由于图像一般是冗余度比较高的，所以加入一定的噪声，并不影响还原，反而能够增强泛化的能力
  - 缺点：学习的是bottleneck的特征，而不是一个概率分布，也不像GAN里的随机噪声，bottleneck适用于分类、检测任务，却不适用于生成任务
- VAE（Variational auto-encoder）
  - 结构：整体结构上和AE类似，但是中间提取的不是特征，而是分布，distribution，训练中会去预测均值和方差
  - 优点：由于是分布，所以可以进行采样，多样性比GAN多
- VQ-VAE
  - 结构：特征量化，codebook，离散化，聚类中心（因为distribution更难学）
  - 优点：优化比VAE容易
  - 缺点：学习到codebook特征，又不像VAE一样能够进行采样，会再训练一个prior网络去做图像生成
- Diffusion Model
  - 结构：Forward Diffusion 和 Reverse Diffusion；逐步增加高斯分布的噪声，最终变为噪声，然后又从噪声还原为图像
  - 优点：多样性非常好，且是概率模型，可解释性很好
  - 缺点：计算成本很高，原始的diffusion model需要一步步steps去逐步进行；逼真程度在以前不如GAN

