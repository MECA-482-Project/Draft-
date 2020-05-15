# MECA 482 Spring 2020
### Treadmill Stabilizer 

-------------------------------------------------------------------------------------

Control System Design of Low Impact Treadmill



-------------------------------------------------------------------------------------


<p align = "center">
  <img src = "photos/HeaderPhoto.PNG" height = "320px" style="margin:10px 10px">
</p>



<center>
   <h4> California State University Chico</h4>
   <h4> College of Engineering, Computer Science, and Construction Management</h4> 
   <h4> MECA 482 Control System Design</h4> 
   <h4> Control System Design of Low Impact Treadmill</h4> 
</center>

## 1. Introduction 
The actively damped treadmill system consists of a treadmill and suspension system which reduces the peak reaction force felt by an object impacting the treadmill. The control is achieved by tuning the damping and spring coefficients in the suspension system, taking into account the spring and damping coefficients of the frame itself. The Project Team’s goal is to develop a comprehensive solution (explained in more details in the deliverables section) to reduce the impact for any weight within the limits.

#### Table of Contents
- [1. Introduction](#1-Introduction)
- [2. Control Theory Modeling](#2-Control-Theory-Modeling)
- [3. Dynamic Modeling](#3-Dynamic-Modeling) 
- [4. Design and Simulation](#4-Design-and-Simulation)
- [5. Appendix](#5-Appendix)
- [6. References](#6-References)

## 2. Control Theory Modeling
Sample High-Level Architecture:

<p align = "center">
  <img src = "photos/Treadmill%20System%20ModelResize.png" height = "260px" style="margin:10px 10px">
</p>

## 3. Dynamic Modeling 

insert system dynamic modeling here....



## 4. Design and Simulation

System simulation was done using Coppelia-Sim. Treadmill spring dampeners were siumulated using prismaic joints with an accompanying 
Lua script in order to simulate a realistic spring with a spring constant value of 566,440 N/M reacting to a downward force of 25 kg
The resulting Coppelia sumulation is shown below

<p align = "center">
<iframe src="https://drive.google.com/file/d/1JGDH5E4Qt0_5jSQPU98jLqbb_XijHtdo/preview" width="640" height="480"></iframe>
</p>

        *** function sysCall_threadmain()
            sim.setThreadAutomaticSwitch(false)
            cyhandle=sim.getObjectHandle('Cylinder')
            cyhandle0=sim.getObjectHandle('Cylinder1')
            initPosition=sim.getObjectPosition(cyhandle,cyhandle0)
            k=30
            while sim.getSimulationState()~=sim.simulation_advancing_abouttostop do
            tempPosition=sim.getObjectPosition(cyhandle,cyhandle0)
            distance=tempPosition[3]-initPosition[3]
            print(distance)
            lastforce=distance*k
           sim.addForceAndTorque(cyhandle0,{0,0,distance*k},{0,0,0})
           sim.switchThread() -- resume in next simulation step
          end
      end
      
In addition we alsl simulated a mass-spring system using a Visual Python extension. This allowed for a more accurate representation of the model's spring system. The same values were used to represent with spring with the constant at 566,440 N/M and a downward forcer of 25 kg. In addition to this we were able to model the spring radius, number of coils, and thickness. These values are 1.25, 10, and .625 respectively. Below is the Visual Python simulation of the spring system. 

<p align = "center">
<iframe src="https://drive.google.com/file/d/1UyK8NhcIz9nqFU8-gtub95kVcnHaFl65/preview" width="640" height="480"></iframe>
</p>

        *** from vpython import *
            #GlowScript 2.9 VPython
             display(width=700,height=700,center=vector(7,0,0),background=color.white)
             wall=box(pos=vector(0,1,0),size=vector(0.2,12,12),color=color.white)
             floor=box(pos=vector(6,-1.75,-1),size=vector(18,0.2,10),color=color.white)
             Mass=box(pos=vector(12,0,0),velocity=vector(0,0,0),size=vector(1,1,1),mass=10.0,color=color.green)
              pivot=vector(0,0,0)
              spring=helix(pos=pivot,axis=Mass.pos-pivot,radius=1.25,constant=566440,thickness=0.625,coils=10,color=color.orange)
              eq=vector(9,0,0)
              #spring constant is in units of N/M
              #spring and block size values are in units of mm
              t=0
              dt=0.001
              while (t<50):
              rate(100)
              acc=(eq-Mass.pos)*(spring.constant/Mass.mass)
              Mass.velocity=Mass.velocity+acc*dt
              Mass.pos=Mass.pos+Mass.velocity*dt
              spring.axis=Mass.pos-spring.pos
              t=t+dt
              
              Here is a summary of the simulation results with the requirements as shown:
              
insert code snippets here? 

## 5. Appendix

insert references here 

## 6. References



    *** Note other deliverables needed include 
    - mathematical model of system
    - simulink control system
    - coppelia simulation 
