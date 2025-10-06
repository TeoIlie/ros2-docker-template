FROM osrf/ros:noetic-desktop-full
# Set ROS_DOMAIN_ID for separation of concerns
ENV ROS_DOMAIN_ID=10

# Install additional tools as `root` user here
RUN apt-get update && apt-get install -y \
    # ROS1 pkgs
    ros-noetic-ackermann-msgs \
    ros-noetic-map-server \
    ros-noetic-joy \
    ros-noetic-rviz \
    ros-noetic-tf2-geometry-msgs \
    ros-noetic-xacro \
    ros-noetic-robot-state-publisher \
    # Install other dependencies as well
    python3-colcon-common-extensions \
    python3-rosdep \    
    python3-pip \
    libeigen3-dev \
    tmux \
    nano \
    git \
    curl \
    build-essential \
    sudo \
    bash-completion 

RUN apt-get -y dist-upgrade
# RUN pip3 install transforms3d

# Install Starship for prettiness
RUN curl -fsSL https://starship.rs/install.sh | sh -s -- -y

# Create a non-root user called `user1000` with UID, GID set to 1000
ARG USERNAME=user1000
ARG USER_UID=1000
ARG USER_GID=$USER_UID

RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME

# Set up workspace
RUN mkdir -p /home/$USERNAME/catkin_ws/src && \
    chown -R $USERNAME:$USERNAME /home/$USERNAME/

# Set the default user to non-root for working with ROS
USER $USERNAME

# Set the default working directory, this is where f1tenth_gym_ros src code will be
WORKDIR /home/$USERNAME/catkin_ws

# Add ROS and workscape sourcing in bashrc
RUN echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc && \
    echo "source ~/catkin_wis/devel/setup.bash" >> ~/.bashrc && \
    echo 'eval "$(starship init bash)"' >> ~/.bashrc && \
    echo 'echo "[~/.bashrc] ROS Domain ID: ${ROS_DOMAIN_ID}"' >> ~/.bashrc

# CMD allows overriding commands, unlike ENTRYPOINT 
CMD ["/bin/bash"]
