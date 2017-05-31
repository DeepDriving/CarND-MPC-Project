# CarND-Controls-MPC
Self-Driving Car Engineer Nanodegree Program

---


## Model

The model that is used is the Kinematic Model.  The state is defined as a six-dimension vector as follows:
 
 x - position on the x axis
	y - position on the y axis
	psi - orientation
	v - speed of the vehicle
	cte - cross track error
	epsi - orientation error

actuators: {steer angle, accelaration}

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


