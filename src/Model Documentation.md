# Valid Trajectories
Here is the detailed description on how to generate paths.

## 1. The methold of generating smooth paths
First, We will creat a list of widely spaced (x,y) waypoints,evenly spaced at 30m.

* We use the car or previous path's end point as starting reference, and also extract the previous point of the starting reference.Use the two points to make the path tangent to the previous path's end point.
* In Frenet, add 30m,60m,90m spaced points ahead of the starting reference.
* Shift car reference angle to 0 degree.
* creat a spline and set (x,y) points to it.

Second, creat a list that contains the previous path's waypoints and the new points we just created.
So the path points becames recursive, We ensure there are 50 points in our list every cycle. When add the new points,we need to rotate back them to normal.

Because of considering the previous path's points, when changing lanes,it will be more smoothly.


## 2. how to ensure parameters meets the requirement
There are some parameters that we need to ensure them to meet requirements. There are speed, acceleration, and jerk.

#### The car drives according to the speed limit.
The car doesn't drive faster than the speed limit. Also the car isn't driving much slower than speed limit unless obstructed by traffic.

* I'll detect if there is a car in my lane by the d value. If there is no car, the car will drive at a max speed 49.5mph. If there is a car on my lane, calculate it's position in the future.  Set a s gap value between my car and the detected car. If the gap is less then s, make the car slow down with an acceleration.


#### Max Acceleration and Jerk are not Exceeded.
The car does not exceed a total acceleration of 10 m/s^2 and a jerk of 10 m/s^3.

* Set the acceleration 5 m/s2 by changing ref_vel add or minus 0.224mph per 20ms.
* we also have solved the jerk problem, when add the new points to the list in step 1.

#### Car does not have collisions.
The car must not come into contact with any of the other cars on the road

* Get the result from controling the s gap when following and changing lanes.

#### The car stays in its lane, except for the time between changing lanes.
The car doesn't spend more than a 3 second length out side the lane lanes during changing lanes, and every other time the car stays inside one of the 3 lanes on the right hand side of the road.

* we achieved that by controlling the d value in each step.

## 3. The car is able to change lanes
The car is able to smoothly change lanes when it makes sense to do so, such as when behind a slower moving car and an adjacent lane is clear of other traffic.

* First, Detect if there is a car in my lane
* If there is a car ahead of me and there is enough space for me to change lanes(left or right), and the car can't go off the road, Change lanes. otherwise, keep following.
