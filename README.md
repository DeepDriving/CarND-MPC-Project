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

N = 10 
dt = 0.05

I tried N > 10 and found the vehicle couldnot handle sharp turns.  10 seems to be the optimal case for N.  Lower dt seems to cause osicilations. 
This N multiplied by dt (0.05 *10) gives 0.5 second, so our predictive controller only predicts the set of states for next 1 second .  This seems about correct for a highly volatile environment for a vehicle driving on a high speed in a high traffic environment.



## Latency
