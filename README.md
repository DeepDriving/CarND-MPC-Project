# CarND-Controls-MPC
Self-Driving Car Engineer Nanodegree Program

---


## Model

The model that is used is the Kinematic Model consisting of Vehicle state and Actuators.  The Vehicle state is defined as a six-dimension vector as follows:
 
        x - position on the x axis
	y - position on the y axis
	psi - orientation
	v - speed of the vehicle
	cte - cross track error
	epsi - orientation error

Actuators are set of controls used to navigate the vehicle which are represented as vector (delta, a).  delta stands for next steering angle that vehicle should apply, in real world a vehicle cannot take very sharp turns due to vehicles turn radius, this has been accounted for in the model constraints and vehicle cannot turn more than +/-18 degree (+/- 0.33 radians) in this simulated environment. a referes to acceleration or throttle that should be applied to vehicle . It can have both positive values for speeding and negative values for breaking.

All values are based on vehicle coordinate system. Under this model the transition of the state between the steps is defined as follows:

	x_[t+1] = x[t] + v[t] * cos(psi[t]) * dt
	y_[t+1] = y[t] + v[t] * sin(psi[t]) * dt
	psi_[t+1] = psi[t] + v[t] / Lf * delta[t] * dt
	v_[t+1] = v[t] + a[t] * dt
	cte[t+1] = f(x[t]) - y[t] + v[t] * sin(epsi[t]) * dt
	epsi[t+1] = psi[t] - psides[t] + v[t] * delta[t] / Lf * dt

## N (timestep length) and dt (elapsed duration between timesteps) values.

The number of steps and duration of each step were chosen empirically as follows:

N = 25
dt = 0.1

After some trials, I settled with N=25 and dt =0.1. It seems to give good result.

# 3rd order polynomial to the waypoints

The simulation provided relevant trajectory waypoints for the vehicles's current location, however, both the trajectory waypoints and the vehicle's location and heading were given in the global reference frame. To simplify the MPC calculations, I converted both the waypoints and the vehicle's location to the vehicle coordinate frame. This was acheived by translating, then rotating the points in the global coordinate frame using the vehicle's current location and heading. Then, the trajectory points were fit to a 3rd order polynomial, whose coefficents were fed to the MPC.

# Latency Handling

To handle latency,the current state of the vehicle was projected forward by the latency time using the current actuator values, then used that as the initial state for the MPC. Thus, the optimal action computed by the MPC would be the optimal action at the time that the controls take effect.


