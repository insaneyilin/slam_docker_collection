FROM ros:melodic

RUN apt-get update \
 && apt-get install -y --no-install-recommends \
 && apt-get install -y --no-install-recommends wget nano build-essential python-pip \
                                               ros-melodic-geodesy ros-melodic-pcl-ros ros-melodic-nmea-msgs \
                                               ros-melodic-rviz ros-melodic-tf-conversions ros-melodic-libg2o \
 && apt-get clean \
 && rm -rf /var/lib/apt/lists/*

RUN pip install -U pip setuptools
RUN pip install ProgressBar2

RUN mkdir -p /root/catkin_ws/src
WORKDIR /root/catkin_ws/src
RUN /bin/bash -c '. /opt/ros/melodic/setup.bash; catkin_init_workspace'
RUN git clone https://github.com/koide3/ndt_omp.git
RUN git clone https://github.com/SMRT-AIST/fast_gicp.git --recursive

# RUN git clone https://github.com/koide3/hdl_graph_slam.git && cd hdl_graph_slam && git checkout arch-melodic
COPY hdl_graph_slam /root/catkin_ws/src/hdl_graph_slam/

WORKDIR /root/catkin_ws
RUN /bin/bash -c '. /opt/ros/melodic/setup.bash; catkin_make'
RUN sed -i "6i source \"/root/catkin_ws/devel/setup.bash\"" /ros_entrypoint.sh

WORKDIR /

ENTRYPOINT ["/ros_entrypoint.sh"]
CMD ["bash"]
