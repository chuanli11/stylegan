Installation
===

```bash
sudo pip3 install virtualenv
virtualenv -p /usr/bin/python3.6 venv-stylegan
. venv-stylegan/bin/activate

pip install -r requirements.txt
```


Prepare Dataset
===

**Download ffhq dataset**
```bash
git clone https://github.com/chuanli11/ffhq-dataset
cd ffhq-dataset
python download_ffhq.py --json --images
```

**Generate tfrecords**

```bash
cd stylegan
python dataset_tool.py create_from_images datasets/ffhq /home/ubuntu/git/ffhq-dataset/images1024x1024 --max_images=1000
```

Training
===

```bash
# Modify training.py and training_loop.py for sift through the training process

# minibatch_repeats in training_loop.py need to accommondate with batch_size

CUDA_VISIBLE_DEVICES=0 python train.py
```

Resolution 256 x 256 

**Memory Requirement (MiB)**

| Batch Size  | Memory  |
|---|---|
| bs=4  | 6GB  |
| bs=8  | 11GB  |
| bs=16 | 11GB  |
| bs=32 | 24GB  |
| bs=64 |   |

**Throughput (samples/sec)** 

|   | 2060  | 2070  | 2080  |  2080 Ti | TitanRTX | V100 | Quadro RTX 6000 | Quadro RTX 8000 |
|---|---|---|---|---|---|---|---|---|
| bs=4  | 5.99  | 6.97  |   |  10.51 |  11.5 |   |   |   |
| bs=8  |  OOM | OOM  |   |  12.33 | 13.50  |   |   |   |
| bs=16  |  |   |   | 15.08  |  16.44 |   |   |   |
| bs=32  |  |   |   | OOM  | 17.12  |   |   |   |
| bs=64  |  |   |   |   |  OOM |   |   |   |

Resolution 512 x 512 

**Memory Requirement (MiB)**

| Batch Size  | Memory  |
|---|---|
| bs=2  | 6GB  |
| bs=4  | 11GB  |
| bs=8  | 11GB |
| bs=16  | 24GB  |
| bs=32  |   |

**Throughput (samples/sec)** 

|   | 2060  | 2070  | 2080  |  2080 Ti | TitanRTX | V100 | Quadro RTX 6000 | Quadro RTX 8000 |
|---|---|---|---|---|---|---|---|---|
| bs=2  | 3.47  | 4.04  |   | 5.99  | 6.58  |   |   |   |
| bs=4  |  OOM | OOM |   | 7.13  |  7.73 |   |   |   |
| bs=8  |   |   |   | 7.92  | 8.79  |   |   |   |
| bs=16  |   |   |   | OOM  | 9.64 |   |   |   |
| bs=32  |   |   |   |  | OOM |   |   |   |


Resolution 1024 x 1024 

**Memory Requirement (MiB)**

| Batch Size  | Memory  |
|---|---|
| bs=1 | 6GB  |
| bs=2  | 11GB  |
| bs=4  | 11GB  |
| bs=8  | 24GB  |
| bs=16  |   |


**Throughput (samples/sec)** 

|   | 2060  | 2070  | 2080  |  2080 Ti | TitanRTX | V100 | Quadro RTX 6000 | Quadro RTX 8000 |
|---|---|---|---|---|---|---|---|---|
| bs=1  | 1.92 |  2.25  |  | 3.18  |  3.57 |   |   |   |
| bs=2  |  OOM | OOM  |   | 3.80  | 4.18 |   |   |   |
| bs=4  |   |   |   | 4.22  |  4.76 |   |   |   |
| bs=8  |   |   |   |  OOM | 4.94  |   |   |   |
| bs=16  |   |   |   |  |  OOM |   |   |   |