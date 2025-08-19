# Newton-Raphson Flow for PX4-ROS2 Deployment
This package is the culmination of 3 papers on Newton-Raphson Flow for Quadrotor Control.

This package allows for fast, accuate, and computationally efficient control via the Newton-Raphson Flow (NR Flow) controller developed mainly at Georgia Tech by a team of researchers led by Dr. Yorai Wardi. We introduce integral CBFs (I-CBFs) to smoothly limit control actuation.

The NR Flow controller is an integral-based control strategy based on a continuous time flow-version of the known newton-raphson iterative algorithm for finding the zeros of functions. It has been shown to have desirable theoretical properties in previous work, including known tracking error bounds, and we show in our hardware implementations that it compares favorably to the native control stack of PX4 Autopilot, as well as NMPC. Notably, it outperforms NMPC in terms of speed and computational efficiency (measured by joules of energy expended by the CPU), and on complex trajectories it may even outperform NMPC due to computational constraints. This is an ideal controller when facing on-board computational limitations. In particular, we test and deploy this on an on-board Raspberry Pi 4 Model B on a Holybro x500V2 quadrotor and we compare it against the NMPC controller available in my [`NMPC_PX4_PKG`](https://github.com/evannsm/NMPC_PX4_PKG) directory.

Trajectories are hard-coded into the code. Just change the `self.main_traj` variable to one of the available trajectory functions inside the code. Feel free to add your own trajectory functions!

To make this stack work for your quadrotor, simply change the mass `self.m` in `quad_casadi_model.py`. Our model takes inputs to be thrust and angular rates. This means we don't use rotational dynamics, so we have no need for the inertia matrix.

## How to run:
Pre-requisites are below, complete those first before running
1. Clone this directory into your ROS2 workspace's source directory and build:
```bash
cd <your_ros2_ws/src>
git init
git remote add origin git@github.com:evannsm/NRFlow_PX4_PKG.git
git fetch origin
git checkout -b main --track origin/main
git submodule update --init --recursive
cd ..
colcon build --symlink-install
```
2. If the above is successful (px4_msgs should take a while), set up your PX4 SITL as per their [user guide](https://docs.px4.io/main/en/ros2/user_guide.html)
3. Once complete:
   - initialize your PX4 gz_x500 (or iris) SITL simulation
   - initialize the MicroXRCEAgent
4. With a prepared simulation and PX4-ROS2 bridge initialized, run the code:
```bash
ros2 run quad_nr_px4 nr_quad log.log
```

I have set up inputs from the command line for simulation/hardware as well as trajectory speed settings.
To change the trajectory type, change `reffunc` inside the code


# Papers and their repositories:
[American Control Conference 2024](https://coogan.ece.gatech.edu/papers/pdf/cuadrado2024tracking.pdf)
[Personal Version of Repository](https://github.com/evannsm/MoralesCuadrado_ACC2024)  
[Official FACTSLab Repository](https://github.com/gtfactslab/MoralesCuadrado_Llanes_ACC2024)  

Transactions on Control Systems Technology 2025  
[Personal Version of Repository](https://github.com/evannsm/MoralesCuadrado_Baird_TCST2025)  
[Official FACTSLab Repository](https://github.com/gtfactslab/Baird_MoralesCuadrado_TRO_2025)  

Transactions on Robotics 2025  
[Personal Version of Repository](https://github.com/evannsm/MoralesCuadrado_Baird_TCST2025)  
[Official FACTSLab Repository](https://github.com/gtfactslab/MoralesCuadrado_Baird_TCST2025)  

# Works:
Repositories that hold other Newton-Raphson work  
[2025_NewtonRaphson_QuadrotorComplete](https://github.com/evannsm/2025_NewtonRaphson_QuadrotorComplete)    
[Blimp_SimHardware_NR_MPC_FBL_BodyOfWork2024](https://github.com/evannsm/Blimp_SimHardware_NR_MPC_FBL_BodyOfWork2024)  
