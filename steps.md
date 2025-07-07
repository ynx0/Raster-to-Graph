# steps

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

set up runpodctl or use scp to transfer weight file








