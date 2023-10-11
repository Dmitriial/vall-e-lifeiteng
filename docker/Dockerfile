FROM pytorch/pytorch:1.13.1-cuda11.6-cudnn8-devel

# python config here
ENV PYTHONUNBUFFERED=1 \
    PYTHONDONTWRITEBYTECODE=1 \
    PYTHONPATH=/opt/icefall:$PYTHONPATH

RUN set -x \
    && : "install system packages" \
    && apt update \
    && DEBIAN_FRONTEND=noninteractive TZ=Etc/UTC apt install -y vim wget git libsndfile1 espeak-ng \
    && apt clean

RUN set -x \
    && pip install torchmetrics==0.11.1 librosa==0.8.1 phonemizer==3.2.1 pypinyin==0.48.0 lhotse==1.16.0 matplotlib h5py \
    && pip install https://huggingface.co/csukuangfj/k2/resolve/main/cuda/k2-1.23.4.dev20230224+cuda11.6.torch1.13.1-cp310-cp310-linux_x86_64.whl \
    && pip install torchaudio==0.13.1+cu116 --extra-index-url https://download.pytorch.org/whl/cu116

WORKDIR /opt/valle

RUN set -x \
    && : "setup icefall library" \
    && git clone --branch v1.1 https://github.com/k2-fsa/icefall /opt/icefall \
    && cd /opt/icefall \
    && pip install -r requirements.txt

# a trick for GPU working (?)
RUN cp /opt/conda/lib/libpython3.10.so.1.0 /usr/lib/x86_64-linux-gnu/

RUN set -x \
    && : "setup vall-e library" \
    && git clone --branch example https://github.com/Dmitriial/vall-e-lifeiteng.git /opt/valle \
    && cd /opt/valle \
    && pip install -e .