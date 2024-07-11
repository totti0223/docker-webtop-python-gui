FROM lscr.io/linuxserver/webtop:ubuntu-kde

# Update and install necessary packages
RUN apt-get update && apt-get upgrade -y
RUN apt-get install -y fonts-takao git wget \
    build-essential \
    gcc \
    g++ \
    python3-dev \
    liblapack-dev \
    libsuitesparse-dev
ENV PATH "$PATH:/config/.local/bin"
ENV MAMBA_DIR "/opt/miniforge"

# Install Mambaforge
RUN wget -nc https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-Linux-x86_64.sh && \
    sh Miniforge3-Linux-x86_64.sh -b -p $MAMBA_DIR && \
    $MAMBA_DIR/bin/mamba && \
    $MAMBA_DIR/bin/mamba init --verbose

# ln -s /opt/miniforge/etc/profile.d/conda.sh /etc/profile.d/conda.sh
SHELL ["/bin/bash", "-ic"]
# # Initialize Conda and Mamba for all users
# RUN /opt/miniforge/bin/conda init && /opt/miniforge/bin/mamba init

##### SLEAP    #####
RUN mamba create -y -n sleap -c conda-forge -c nvidia -c sleap -c anaconda sleap=1.3.3

##### PlantSeg #####
RUN if [ "$GPU_AVAILABLE" = "1" ]; then \
    # If GPU is available, install PyTorch with CUDA support
    mamba create -n plant-seg -c pytorch -c nvidia -c conda-forge pytorch pytorch-cuda=11.8 pyqt plant-seg; \
    else \
    # If GPU is not available, install PyTorch with CPU-only support
    mamba create -n plant-seg -c pytorch -c nvidia -c conda-forge pytorch cpuonly pyqt plant-seg; \
    fi

##### ChronoRoot #####
# need cuda 10.0 and cudnn7.4
RUN git clone https://github.com/ngaggion/ChronoRoot.git
RUN mamba env create -f ChronoRoot/env.yml
# Activate the ChronoRoot environment
RUN conda activate &&  activate ChronoRoot
RUN pip install opencv-python git+https://github.com/lucasb-eyer/pydensecrf.git && \
    conda install -c anaconda tensorflow-gpu==1.15.0

RUN echo ". /opt/miniforge/etc/profile.d/conda.sh" >> /config/.bashrc