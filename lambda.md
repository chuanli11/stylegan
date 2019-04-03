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
# Modify training.py for sift through the training process

python train.py
```


**Memory Requirement (MiB)**


| Batch Size  | Memory  |
|---|---|
| bs=1  | 4660 |
| bs=2  | 8756 |
| bs=4  | 16948  |
| bs=8  | 23380  |
| bs=16  |   |

**Throughput (samples/sec)** 

|   | 2060  | 2070  | 2080  |  2080 Ti | TitanRTX | V100 | Quadro RTX 6000 | Quadro RTX 8000 |
|---|---|---|---|---|---|---|---|---|
| bs=1  | 3.86  | 3.92  |   | 5.48  | 5.75  |   |   |   |
| bs=2  |  4.48 | 4.50 |   | 6.45  |  6.92 |   |   |   |
| bs=4  | OOM  | OOM  |   | 7.04  |  7.84 |   |   |   |
| bs=8  | OOM  | OOM  |   |  OOM |  8.24 |   |   |   |
| bs=16  | OOM | OOM  |   | OOM  |  OOM |   |   |   |