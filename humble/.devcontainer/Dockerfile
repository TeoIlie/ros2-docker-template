FROM osrf/ros:humble-desktop-full
# Set ROS_DOMAIN_ID for separation of concerns
ENV ROS_DOMAIN_ID=20

# Install additional tools as `root` user here
RUN apt-get update && apt-get install -y \
    python3-colcon-common-extensions \
    python3-rosdep \    
    nano \
    git \
    curl \
    build-essential \
    sudo \
    bash-completion \
    # Reduce package size
    && rm -rf /var/lib/apt/lists/* 

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
RUN mkdir -p /home/$USERNAME/ros2_ws/src && \
    chown -R $USERNAME:$USERNAME /home/$USERNAME/

# Set the default user to non-root for working with ROS
USER $USERNAME

# Set the default working directory
WORKDIR /home/$USERNAME/ros2_ws

# Add ROS and workscape sourcing in bashrc
RUN echo "source /opt/ros/humble/setup.bash" >> ~/.bashrc && \
    echo "source ~/ros2_ws/install/setup.bash" >> ~/.bashrc && \
    echo 'eval "$(starship init bash)"' >> ~/.bashrc && \
    echo 'echo "[~/.bashrc] ROS Domain ID: ${ROS_DOMAIN_ID}"' >> ~/.bashrc

# CMD allows overriding commands, unlike ENTRYPOINT 
CMD ["/bin/bash"]
