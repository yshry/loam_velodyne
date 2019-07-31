FROM ros:melodic-ros-core

ARG user
ENV HOME /home/${user}
RUN echo ${user} && echo ${HOME}


RUN apt update && apt -y upgrade
WORKDIR /opt

RUN apt install -y ros-melodic-desktop-full

RUN apt install -y wget

RUN wget https://github.com/PointCloudLibrary/pcl/archive/pcl-1.8.1.tar.gz && \
	tar xvf pcl-1.8.1.tar.gz && \
        cd pcl-pcl-1.8.1 && \
        mkdir build && cd build && \
	cmake .. && make -j4 && make install

RUN useradd ${user} && mkdir ${HOME} && chown -R ${user}:${user} ${HOME}

RUN rm /bin/sh && ln -s /bin/bash /bin/sh

USER ${user}
WORKDIR ${HOME}

RUN echo "source /opt/ros/melodic/setup.bash" >> ~/.bashrc &&\
    source ~/.bashrc && \
    mkdir -p ~/catkin_ws/src && \
    cd ~/catkin_ws/src && \
    git clone https://github.com/yshry/loam_velodyne.git && \
    cd .. && \
    catkin_make -DCMAKE_BUILD_TYPE=Release

RUN echo "source ~/catkin_ws/devel/setup.bash" >> ~/.bashrc

CMD bash
