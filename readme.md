
## main.py 结构

在training mode下：
- guidance = StableDiffusion 或者 CLIP，将text参数变成embedding/image
- optimizer = Adam
- model: NeRFNetwork
- train_loader = NeRFDataset,固定参数focal和intrinsic,该类的get_item方法返回随机参数rays_o, rays_d（get_rays函数生成的，N = min(N, H*W)的随机的点云）,dirs(前后左右各个角度)
- Trainer，参数有guidance，optimizer，lr_scheduler，model：
  - 主要实现train_step：
    - self.text_z:guidance和text生成的psudo GT image
    - 调用model.render，优化rays_o, rays_d这两个参数，返回pre_image，和self.text_z计算loss，backward
    - rays_o, rays_d: [B, N, 3], assumes B == 1