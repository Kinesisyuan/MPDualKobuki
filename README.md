# Dual kobuki Running in Gazebo
## Installing kobuki
We use kobuki robot to try doing the multi-robot running simulation in Gazebo.  
First install kobuki and clone this repository:

    sudo apt-get install ros-$(your_ros_distro)-kobuki-gazebo
    //go to your desired directory
    git clone https://github.com/Kinesisyuan/MPDualKobuki.git

## Boosting simulation speed
The simulated world in gazebo by default has the same time flowing rate with real world. But in cases when 
larger simulation speed is demanded for purposes such as training, we can actually adjust the time factor of the world 
by changing the **time factor relevant variables in the .world file**. For example, if it is desired that the time 
passing in the simulation is 10x the time in real world, the relevant .world file that is used later can be found with
the command

    roscd kobuki_gazebo/worlds
    sudo gedit empty.world
    
Several lines of code can be found with <max_step_size>， <real_time_factor> and <real_time_update_rate>, and in most 
cases <max_step_size> times <real_time_update_rate> equals to <real_time_factor>, which is 1. Stating all 3 of them 
with an obvious redundancy doesn't mean they should remain consistent. In fact, simply change the <real_time_update_rate> 
to a 10x value without changing the result <real_time_factor> still works in gazebo, which will simply multiply the step 
and rate to get the real time factor. For example, the following lines will result in a 10x speed simulation enviornment:

    <max_step_size>0.01</max_step_size>
    <real_time_factor>1</real_time_factor>
    <real_time_update_rate>1000</real_time_update_rate>

## Multi-Processing
By changing the localhost, we can call multiple gazebo windows with independent robots inside at a same time. First 
launch several gazebo with robots (3 here, can be less, or more by creating a similar .sh file)

    cd MP_Dual/launch
    ./DualSimulation0.sh
    // open another terminal
    ./DualSimulation1.sh
    // open another terminal
    ./DualSimulation2.sh

Now three gazebo windows is shown, and the robots inside can be drived to run with desired or random speed (both linear 
and angular) with python script with command:

    python2 ../driving_scripts/MPDual.py

and now the robots in each gazebo window starts to run randomly.

Due to the nature of multi-processing, rqt_graph for each gazebo window is unavailable during the run. For structure of
each robot, try running simplerun.sh with DualSimulation.py to have a look at information including rostopic list and rqt_graph.

