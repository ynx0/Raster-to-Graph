# steps



## Set up the container

In runpod

Use image `nvidia/cuda:11.1.1-cudnn8-devel`

Override container start command:

```bash
bash -c '
    apt update;
    apt install -y wget;
    apt-get install -y python3.7 python3-pip git build-essential
    update-alternatives --install /usr/bin/python python /usr/bin/python3.7 1
    update-alternatives --install /usr/bin/pip pip /usr/bin/pip3 1
    mkdir -p /workspace;
    DEBIAN_FRONTEND=noninteractive apt-get install openssh-server -y;
    mkdir -p ~/.ssh;
    chmod 700 ~/.ssh;
    echo "$PUBLIC_KEY" >> ~/.ssh/authorized_keys;
    chmod 700 ~/.ssh/authorized_keys;
    service ssh start;
    pip3 install -U --no-cache-dir jupyterlab jupyterlab_widgets ipykernel ipywidgets;
    jupyter lab --allow-root --no-browser --port=8888 --ip=* --ServerApp.terminado_settings="{\"shell_command\":[\"/bin/bash\"]}" --ServerApp.token=$JUPYTER_PASSWORD --ServerApp.allow_origin=* --FileContentsManager.preferred_dir=/workspace;
    sleep infinity
'
```

If we don't include `sleep infinity`, the container will exit immediately.


## Use the container



clone the repo


apt-get update && apt-get install libgl1

mkdir -p ~/miniconda3
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda3/miniconda.sh
bash ~/miniconda3/miniconda.sh -b -u -p ~/miniconda3
rm ~/miniconda3/miniconda.sh
source ~/miniconda3/bin/activate
conda init --all

conda deactivate

conda create -n r2g python=3.7

conda activate r2g

conda install -c conda-forge pycocotools=2.0.4

pip install -r requirements.txt -f https://download.pytorch.org/whl/torch_stable.html

---

cd models/ops/
sh make.sh
cd ../..

---

load any images to evaluate into data/dataset_v5/test

set up runpodctl or use scp to transfer weight file into root of repo


`python demo.py`








