FROM lscr.io/linuxserver/webtop:ubuntu-kde

# Update and install necessary packages
RUN apt-get update && apt-get upgrade -y
RUN apt-get install -y fonts-takao git wget
ENV PATH "$PATH:/config/.local/bin"
# Install Mambaforge
RUN wget -nc https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-Linux-x86_64.sh && \
    sh Miniforge3-Linux-x86_64.sh -b -p /opt/miniforge && \
    ln -s /opt/miniforge/etc/profile.d/conda.sh /etc/profile.d/conda.sh
# Initialize Conda and Mamba for all users
RUN /opt/miniforge/bin/conda init && /opt/miniforge/bin/mamba init 


##### SLEAP    #####
RUN /opt/miniforge/bin/mamba create -y -n sleap -c conda-forge -c nvidia -c sleap -c anaconda sleap=1.3.3
##### PlantSeg #####
RUN if [ "$GPU_AVAILABLE" = "1" ]; then \
    # If GPU is available, install PyTorch with CUDA support
    /opt/miniforge/bin/mamba create -n plant-seg -c pytorch -c nvidia -c conda-forge pytorch pytorch-cuda=11.8 pyqt plant-seg; \
    else \
    # If GPU is not available, install PyTorch with CPU-only support
    /opt/miniforge/bin/mamba create -n plant-seg -c pytorch -c nvidia -c conda-forge pytorch cpuonly pyqt plant-seg; \
    fi
##### ChronoRoot #####
# need cuda 10.0 and cudnn7.4
RUN git clone https://github.com/ngaggion/ChronoRoot.git;
RUN /opt/miniforge/bin/mamba env create -f ChronoRoot/env.yml
RUN /opt/miniforge/bin/mamba activate ChronoRoot && \
    pip install opencv-python git+https://github.com/lucasb-eyer/pydensecrf.git && \
    conda install -c anaconda tensorflow-gpu==1.15.0





RUN echo ". /opt/miniforge/etc/profile.d/conda.sh" >> /config/.bashrc