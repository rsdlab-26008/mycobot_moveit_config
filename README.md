# mycobot_moveit_config

MyCobot 280（6自由度マニピュレータ）のURDFモデルを自作し、MoveIt 2で動作確認するためのROS 2パッケージです。

## 概要

- ロボットの寸法図（J1〜J6の関節位置が明示された仕様図）から自分で座標系（各関節の回転軸・オフセット）を導出
- [csv2urdf](https://github.com/ttakubo/csv2urdf) を使ってCSV → URDFを生成
- MoveIt Setup AssistantでMoveIt 2用パッケージ（SRDF、コントローラ設定等）を作成
- `demo.launch.py` でRViz2 + MoveIt上での動作を確認済み

## 環境

- ROS 2 Humble
- MoveIt 2
- Python 3 / jinja2（csv2urdfの実行に必要）

## セットアップ手順

1. `csv2urdf` をクローンし、`csv/mycobot280.csv` を配置してURDFを生成
   ```bash
   git clone https://github.com/ttakubo/csv2urdf.git
   cd csv2urdf
   pip3 install jinja2
   python3 create_robot_csv.py mycobot280.csv
   ```
2. `check_urdf mycobot280.urdf` で生成物を確認
3. MoveIt Setup Assistantを起動し、生成したURDFを読み込んでパッケージを作成
   ```bash
   ros2 launch moveit_setup_assistant setup_assistant.launch.py
   ```
   - Planning Groups / Robot Poses / End Effectors / ROS 2 Controllers を設定
   - Configuration Files で `mycobot_moveit_config` として保存
4. ワークスペースでビルド
   ```bash
   cd ~/colcon_ws
   colcon build --symlink-install
   source install/setup.bash
   ```
5. 起動
   ```bash
   ros2 launch mycobot_moveit_config demo.launch.py
   ```
6. RViz2のMotionPlanningパネルでインタラクティブマーカーを動かし、Plan → Executeで動作確認
