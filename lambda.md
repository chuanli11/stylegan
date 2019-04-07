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

**Resolution 256 x 256**

Memory Requirement (MiB)

| Batch Size  | Memory  |
|---|---|
| bs=4  | 6GB  |
| bs=8  | 11GB  |
| bs=16 | 11GB  |
| bs=32 | 24GB  |
| bs=64 | 48GB  |

Throughput (samples/sec)

|   | 2060  | 2070  | 2080  |  1080 Ti | 2080 Ti | TitanRTX | Quadro RTX 6000 | V100 | Quadro RTX 8000 |
|---|---|---|---|---|---|---|---|---|---|
| bs=4  | 5.99  | 6.97  | 8.39  | 8.11  |  10.51 |  11.5 | 11.03  | 11.78  | 11.38  |
| bs=8  |  OOM | OOM  | OOM  | 9.24  |  12.33 | 13.50  | 12.30  | 14.93  | 13.33  |
| bs=16  | OOM | OOM  | OOM  | 10.47  | 15.08  |  16.44 | 14.56  | 17.55  | 15.77  |
| bs=32  | OOM | OOM  | OOM | OOM | OOM  | 17.12  | 14.85  | 19.35  | 16.55  |
| bs=64  | OOM | OOM  | OOM | OOM  |  OOM |  OOM | OOM  | OOM  | 16.66  |

**Resolution 512 x 512**

Memory Requirement (MiB)

| Batch Size  | Memory  |
|---|---|
| bs=2  | 6GB  |
| bs=4  | 11GB  |
| bs=8  | 11GB |
| bs=16  | 24GB  |
| bs=32  | 48GB  |

Throughput (samples/sec)

|   | 2060  | 2070  | 2080  |  1080 Ti | 2080 Ti | TitanRTX | Quadro RTX 6000 | V100 | Quadro RTX 8000 |
|---|---|---|---|---|---|---|---|---|---|
| bs=2  | 3.47  | 4.04  | 4.77  | 4.46  | 5.99  | 6.58  | 5.89  | 6.72  | 6.49  |
| bs=4  |  OOM | OOM | OOM  |  5.02 | 7.13  |  7.73 | 7.15  | 8.56  | 7.59  |
| bs=8  |  OOM | OOM  | OOM  | 5.63  | 7.33  | 7.92  | 8.79  | 9.80  | 7.83  | 8.41  |
| bs=16  | OOM  | OOM  | OOM  | OOM  | OOM  | 9.64 | 8.22 | 10.72  | 9.23  |
| bs=32  | OOM  | OOM  | OOM  |  OOM | OOM | OOM | OOM  | OOM  | 9.36  |


**Resolution 1024 x 1024** 

Memory Requirement (MiB)

| Batch Size  | Memory  |
|---|---|
| bs=1 | 6GB  |
| bs=2  | 11GB  |
| bs=4  | 11GB  |
| bs=8  | 24GB  |
| bs=16  | 48GB  |


Throughput (samples/sec)

|   | 2060  | 2070  | 2080  |  1080 Ti | 2080 Ti | TitanRTX | Quadro RTX 6000 | V100 | Quadro RTX 8000 |
|---|---|---|---|---|---|---|---|---|---|
| bs=1  | 1.92 |  2.25  | 2.60 | 2.39  | 3.18  |  3.57 | 3.30  | 3.72  | 3.5  |
| bs=2  |  OOM | OOM  | OOM  | 2.65   |3.80  | 4.18 | 3.89  | 4.61  | 4.08  |
| bs=4  |  OOM | OOM  | OOM  | 2.97 | 4.22  |  4.76 | 4.03  | 5.24  | 4.53  |
| bs=8  |  OOM | OOM  | OOM  | OOM  |  OOM | 4.94  | 4.25  | 5.70  | 4.70  |
| bs=16  | OOM  | OOM  | OOM  |  OOM | OOM |  OOM | OOM  | OOM  | 4.96  |

**Time Cost**

Highest-quality StyleGAN (configuration F in Table 1) for the FFHQ dataset at 1024×1024 resolution 

GPU: Titan RTX

| GPUs | 1024&times;1024  | 512&times;512    | 256&times;256    |
| :--- | :--------------  | :------------    | :------------    |
| 1    | 49 days 8 hours | 29 days 21 hours | 17 days 22 hours |
| 2    | 26 days 7 hours | 15 days 23 hours  | 11 days 1 hours   |
| 4    | 13 days 14 hours  | 8 days 21 hours   | 5 days 21 hours  |
| 8    | 7 days 22 hours  | 5 days 7 hours  | 4 days 1 hours   |
