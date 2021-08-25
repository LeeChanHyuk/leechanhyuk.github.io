---
title: "[Concept summary] Pytorch multi-GPU learning"
date: 2021-08-20 13:30:06
author: Leechanhyuk
categories: Machine_Learning
tags: Machine_Learning
use_math: true
cover: "/assets/instacode.png"
toc: true
---

# 시작하기전에

 - 본 포스팅은 pytorch에서 Multi-GPU 사용 방법을 간단하게 다루고 있습니다.

 - Multi-GPU에서는 아래와 같은 과정을 따릅니다.

 1. GPU의 개수만큼 Process를 생성 및 GPU에 Process 할

 2. 각 Process마다 Model 생성

 3. 데이터를 GPU의 개수에 따라 나눔 (Scatter)

 4. Inference - 출력을 한 곳에 모음 (Gather)

 5. Backpropagation 

# Data Parallel

 - 간단하게 한 줄로 실행가능하지만, Multi-GPU 간의 사용하는 Memory가 달라 batch-size를 크게 키울 수 없다.

 ```python
 import torch.nn as nn

 model = nn.DataParallel(model)
 ```

# DistributedDataParallel

 ```python
 import torch.distributed as dist
 from torch.nn.parallel import DistributedDataParallel
 
 
 def main():
     args = parser.parse_args()
 
     ngpus_per_node = torch.cuda.device_count()
     args.world_size = ngpus_per_node * args.world_size
     mp.spawn(main_worker, nprocs=ngpus_per_node, 
              args=(ngpus_per_node, args))
     
     
 def main_worker(gpu, ngpus_per_node, args):
     global best_acc1
     args.gpu = gpu
     torch.cuda.set_device(args.gpu)
     
     print("Use GPU: {} for training".format(args.gpu))
     args.rank = args.rank * ngpus_per_node + gpu
     dist.init_process_group(backend='nccl', 
                             init_method='tcp://127.0.0.1:FREEPORT',
                             world_size=args.world_size, 
                             rank=args.rank)
     
     model = Bert()
     model.cuda(args.gpu)
     model = DistributedDataParallel(model, device_ids=[args.gpu])
 
     acc = 0
     for i in range(args.num_epochs):
         model = train(model)
         acc = test(model, acc)
 ```

 - GPU 4개당 1 Node로 보고 world_size = node * num_gpu로 설정

 - mp.spawn(func)은 func 함수를 GPU마다 실행함.

 - dist.init_process_group는 분산 학습 초기화를 담당함. 꼭 실행시켜주기.

 - 각 GPU에 Dataloader가 입력을 전달해주기 위해서는 DistributedSampler를 사용

 ```python
 
 from torch.utils.data.distributed import DistributedSampler
 
 train_dataset = datasets.ImageFolder(traindir, ...)
 train_sampler = DistributedSampler(train_dataset)
 
 train_loader = torch.utils.data.DataLoader(
     train_dataset, batch_size=args.batch_size, shuffle=False,
     num_workers=args.workers, pin_memory=True, sampler=train_sampler)
 ```

 - DataSampler는 전체 데이터를 섞은 후 쪼개서 각 GPU-Sampler에 할당함.

 - 따라서 각 epoch마다 data의 index는 계속 변화.

 - train_sampler.set_epoch(epoch)를 매 epoch마다 실행시켜줘야 계속 섞어줌.

 - 여기서 batch_size는 GPU의 개수에 따라 나누어서 할당해준다.

 - Batch_size는 GPU마다 Data가 분할되어 들어가니까 당연하다.

 - num_workers는 4 * GPU 개수를 추천한다고 한다. (https://discuss.pytorch.org/t/guidelines-for-assigning-num-workers-to-dataloader/813/4)

 - 그 이유는 요새는 CPU 코어가 많이 달려 나오는데, 너무 많은 CPU-Core를 할당하면 연산 부분 외에 오버헤드가 발생하고, 너무 적은 CPU-Core를 할당하면 GPU를 제대로 할당할 수 없기 때문이다. (CPU가 GPU에 Data를 전달해주는 역할을 한다.)

# Reference

 - https://medium.com/daangn/pytorch-multi-gpu-%ED%95%99%EC%8A%B5-%EC%A0%9C%EB%8C%80%EB%A1%9C-%ED%95%98%EA%B8%B0-27270617936b

 - https://blog.si-analytics.ai/12

 - https://github.com/pytorch/examples/blob/master/imagenet/main.py

 - https://discuss.pytorch.org/t/guidelines-for-assigning-num-workers-to-dataloader/813/4