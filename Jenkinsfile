pipeline {
      agent any 
      environment {
        PYTHONPATH=/home/simulations/public_sim_ws/devel/lib/python3/dist-packages:/opt/ros/noetic/lib/python3/dist-packages:/home/simulations/public_sim_ws/src/all/ros_basics_examples/python_course_class:/home/simulations/public_sim_ws/src/ros_basics_examples/python_course_class:/home/simulations/public_sim_ws/src/all/kinematics_course_utils/kinematics
      }
      stages {
          stage('Run tests') {
              steps {
                  sh 'python -m unittest discover -s tests -v'
              }
          }
          stage('Print + list current directory') {
              steps {
                  sh 'pwd'
                  sh 'ls -al'
              }
          }
          stage('Show ROS environment variables') {
              steps {
                  sh 'env | grep ROS'
              }
          }
          stage('Move the robot') {
              steps {
                  sh '''
                  roslaunch publisher_example move.launch &
                  MOVE_ID=$!
                    sleep 30s
                    kill $MOVE_ID
                  '''
              }
          }
          stage('Stop the robot') {
              steps {
                  sh '''
                  roslaunch publisher_example stop.launch &
                  STOP_ID=$!
                    sleep 5s
                    kill $STOP_ID
                  '''
              }
          }
          stage('Reset the simulation') {
              steps {
                  sh 'rosservice call /gazebo/reset_simulation "{}"'
              }
          }
          stage('Done') {
              steps {
                  sleep 5
                  echo 'Pipeline completed'
              }
          }
      }
  }
  