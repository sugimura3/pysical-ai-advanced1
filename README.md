# pysical-ai-advanced1

★Docker Desktop の起動

## ● Step 1: ROS 1 Noetic の Docker を起動する

★PowerShell（ターミナル） の起動

docker stop pai_ros2  
cd ~/Desktop  
mkdir -p ros1_lecture  
cd ros1_lecture  

XXXXXXXXXXXXXXXXX  
docker run -d --name ros1_noetic -p 6080:80 --shm-size=512m -v "${PWD}:/workspace" tiryoh/ros-desktop-vnc:noetic  
XXXXXXXXXXXXXXXXX  

docker start ros1_noetic　　　#コンテナIDが表示されます。= ros1_noetic  
docker ps　　　　　　　　　　#ros1_noetic が表示され、STATUSが Up になっていれば起動


（・ブラウザで localhost:6080 を開く → noVNC が起動 →接続）

（ターミナルA）  
rosversion -d　　　　　# noetic と出ればOK  
gazebo --version　　　　# 11.x と出ればOK



## ● Step 2: 必要なパッケージを入れる

sudo apt update  
sudo apt install -y ros-noetic-ros-control ros-noetic-ros-controllers ros-noetic-gazebo-ros-control ros-noetic-xacro


## ● Step 3: ワークスペースを作ってソースを取得する

mkdir -p ~/catkin_ws/src  
cd ~/catkin_ws/src  
git clone https://github.com/project-srs/ros_lecture.git  

cd ~/catkin_ws  
catkin_make -DCATKIN_WHITELIST_PACKAGES="gazebo_urdf_lecture"  

catkin_make  
source devel/setup.bash  


## ● Step 4: シミュレーションを起動する

source ~/catkin_ws/devel/setup.bash   # ターミナルを開くたびに必要  
roslaunch gazebo_urdf_lecture dtw_gazebo.launch


## ● Step 5: 動かしてみる

（ターミナルB）Ctrl+Shift+E

source ~/catkin_ws/devel/setup.bash を実行


rostopic pub -r 10 /dtw_robot/diff_drive_controller/cmd_vel geometry_msgs/Twist "linear:  
  x: 0.4  
  y: 0.0  
  z: 0.0  
angular:  
  x: 0.0  
  y: 0.0  
  z: 0.5"  

ロボットが前に進めば成功

（ターミナルC）  
現在の位置と速度も見られます。

rostopic echo /dtw_robot/diff_drive_controller/odom

・送信ターミナル（ターミナルA）で Ctrl+C を押します。  
yaml の cmd_vel_timeout: 1.0 の設定により、指令が1秒途切れるとロボットは自動で止まります。  
・画面外に出てしまったロボットを元に戻すには、Gazebo 上で Ctrl+R(World をリセット)を押します。メニューなら Edit → Reset World です。

