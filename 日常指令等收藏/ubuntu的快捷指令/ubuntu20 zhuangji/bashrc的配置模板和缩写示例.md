
# some more ls aliases
alias ll='ls -alF'
alias la='ls -A'
alias l='ls -CF'
alias vpn='cd ~/clash && ./clash -d .'
alias gb='gedit ~/.bashrc'
alias sb='source ~/.bashrc'
alias sd='source devel/setup.bash'
alias rl='rostopic list'
alias ri='rostopic info'
alias rhz='rostopic hz'
alias re='rostopic echo'
alias ca='catkin_make -DCMAKE_EXPORT_COMPILE_COMMANDS=Yes -DPYTHON_EXECUTABLE=/usr/bin/python3'
alias vins='cd ~/project/Fast-Drone-250/shfiles && sh rspx4.sh'
alias ctrl='roslaunch px4ctrl run_ctrl.launch'
alias takeoff='cd ~/project/Fast-Drone-250/shfiles && sh takeoff.sh'
alias ego='roslaunch ego_planner single_run_in_exp.launch  '
alias vego='roslaunch ego_planner rviz.launch'
alias vvins='rostopic echo /vins_fusion/imu_propagate'
alias rvins='rosbag record /vins_fusion/imu_propagate'
alias config='gedit ~/project/Fast-Drone-250/src/realflight_modules/px4ctrl/config/ctrl_param_fpv.yaml'
alias gc='git clone '
alias osip="avahi-browse -lr _roger._tcp"
alias px4="cd ~/projects/autoland_ws/Firmware && make px4_sitl_default gazebo  "
alias cpx4="roslaunch mavros px4.launch fcu_url:=udp://:14540@localhost:14580"
# Add an "alert" alias for long running commands.  Use like so:
#   sleep 10; alert
alias alert='notify-send --urgency=low -i "$([ $? = 0 ] && echo terminal || echo error)" "$(history|tail -n1|sed -e '\''s/^\s*[0-9]\+\s*//;s/[;&|]\s*alert$//'\'')"'

# Alias definitions.
# You may want to put all your additions into a separate file like
# ~/.bash_aliases, instead of adding them here directly.
# See /usr/share/doc/bash-doc/examples in the bash-doc package.

if [ -f ~/.bash_aliases ]; then
    . ~/.bash_aliases
fi

# enable programmable completion features (you don't need to enable
# this, if it's already enabled in /etc/bash.bashrc and /etc/profile
# sources /etc/bash.bashrc).
if ! shopt -oq posix; then
  if [ -f /usr/share/bash-completion/bash_completion ]; then
    . /usr/share/bash-completion/bash_completion
  elif [ -f /etc/bash_completion ]; then
    . /etc/bash_completion
  fi
fi
#python
#export PYTHON_VERSION=3
#export PYTHON_EXECUTABLE=/usr/bin/python3
#export CATKIN_PYTHON_VERSION=3

#cuda
export PATH=$PATH:/usr/local-11.0/cuda/bin  
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda-11.0/lib64  
export LIBRARY_PATH=$LIBRARY_PATH:/usr/local/cuda-11.0/lib64
 
# >>> conda initialize >>>
# !! Contents within this block are managed by 'conda init' !!
conda config --set auto_activate_base false
__conda_setup="$('/home/b/anaconda3/bin/conda' 'shell.bash' 'hook' 2> /dev/null)"
if [ $? -eq 0 ]; then
    eval "$__conda_setup"
else
    if [ -f "/home/b/anaconda3/etc/profile.d/conda.sh" ]; then
        . "/home/b/anaconda3/etc/profile.d/conda.sh"
    else
        export PATH="/home/b/anaconda3/bin:$PATH"
    fi
fi
unset __conda_setup
# <<< conda initialize <<<

source /opt/ros/noetic/setup.bash 
source ~/projects/2mapping/fast_lio2_ws/devel/setup.bash
source ~/projects/1map/eth_wavemap_ws/devel/setup.bash
source ~/projects/6tools/ws_livox/devel/setup.bash 
source ~/projects/4cloud_seg/test_seg_ws/devel/setup.bash
source ~/projects/5exploration/atare_ws/devel/setup.bash
source ~/projects/4cloud_seg/Roomseg_ws/devel/setup.bash
source ~/projects/7guihua/EGO-Planner-v2/swarm-playground/bmain_ws/devel/setup.bash
source ~/projects/controll/xuzhefan_controol_ws/devel/setup.bash
#export ROS_HOSTNAME=192.168.1.99
#export ROS_MASTER_URI=http://192.168.1.122:11311

 
export PYTHONPATH=${PYTHONPATH}:~/projects/MPC_code/high_mpc
export PATH="/usr/lib/ccache:$PATH"
export RPGQ_PARAM_DIR=~/projects/7guihua/agile_autonomy_ws/catkin_aa/src/rpg_flightmare

#export PX4_ROOT=~/projects/autoland_ws/Firmware
#source ~/projects/autoland_ws/Firmware/Tools/setup_gazebo.bash ~/projects/autoland_ws/Firmware ~/projects/autoland_ws/Firmware/build/px4_sitl_default
#export ROS_PACKAGE_PATH=$ROS_PACKAGE_PATH:~/projects/autoland_ws/Firmware
#export ROS_PACKAGE_PATH=$ROS_PACKAGE_PATH:~/projects/autoland_ws/Firmware/Tools/sitl_gazebo