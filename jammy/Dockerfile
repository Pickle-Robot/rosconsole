FROM ubuntu:jammy

ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update && apt-get install -y curl gnupg2 lsb-release
RUN curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key  -o /usr/share/keyrings/ros-archive-keyring.gpg
RUN echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | tee /etc/apt/sources.list.d/ros2.list > /dev/null
RUN apt-get update && apt-get install -y python3-rosdep python3-rosinstall-generator python3-vcstool build-essential

RUN rosdep init && rosdep update

RUN mkdir -p /root/ws/src/
RUN cd /root/ws && rosinstall_generator rosconsole --rosdistro noetic --deps --tar > noetic.rosinstall &&  vcs import --input noetic.rosinstall /root/ws/src

RUN rosdep install --from-paths /root/ws/src --ignore-packages-from-source --rosdistro noetic -y

RUN touch /root/ws/src/rosconsole/CATKIN_IGNORE
RUN cd /root/ws && /root/ws/src/catkin/bin/catkin_make_isolated

WORKDIR /root/ws
CMD ["/root/ws/src/catkin/bin/catkin_make_isolated", "--pkg", "rosconsole", "--catkin-make-args", "run_tests"]

