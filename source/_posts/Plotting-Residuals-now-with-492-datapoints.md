title: Plotting Residuals (now with ~492 datapoints)
author: N Ouyang
date: 2018-04-10 14:22:52
tags:
---

For this week, I generated graphs of the residuals of the torque estimate using the simple linear
model: `torque = k * theta + constant`.

## Sanity Check

![](full_data_csv.png)

Observe the "N" and "R" columns, which are the torque Y, and estimated torque Y,
respectively. Furthermore we have the residual itself in column "V". The
estimates mostly pass the sanity check (seem reasonable). The error is pretty
large, if we say the average x position is ~3.5cm, then the **average torque error**
is 20g. This is not great!

# Questions

**Difference between numpy and sklearn K estimates**

Questions: Numpy's least squares answer for K -- was very different from
sklearns.  sklearn's K looked more reasonable (didn't have tiny values) -- is
this just due to normalization? Sklearn's K passed the sanity check, but I did
not check numpy's K. 

See following image, which shows the K as calculated by `numpy.lstsq` vs `sklearn.linreg`, as calculated across all 492 datapoints.

![numpyvssklearnlinregK.png](numpy_vs_sklearn_linreg_K.png)


**What to plot**
Do we really need *only* the torque residuals? Or should we also look at thetheta or force or
position residuals? I can't think of a reason we would need the latter.


# Plots

## Torque Estimated

I think that only (epsX vs torX) and (epsY vs torY) should be meaningful, but here are a ll four
combinations (made my code easier).

* TorqueX, Estimated 
    ![](Torq Est X.png)
* TorqueY, Estimated
    ![](Torq Est Y.png)

## Force

* ForceZ 
    ![](ForceZ.png) 

## Thetas 

* ThetaX
    ![](ThetaX.png) 
* ThetaY
    ![](ThetaY.png)
* ThetaZ
    ![](ThetaZ.png)

## Positions 

* PositionX 
    ![](PositionX.png)
* PositionY
    ![](PositionY.png)

## Math 
\begin{align}
\begin{bmatrix}
    \tau_{x}       \\
    \tau_{y}       \\
    \tau_{z}       \\
\end{bmatrix} =
\Bigg[ \; K \; \Bigg]_{3x3}  \;
\begin{bmatrix}
    \theta_{x}       \\
    \theta_{y}       \\
    \theta_{z}       \\
\end{bmatrix} \\
\end{align}

# Thought Process 

If we take our stiffness matrix estimate, $\hat{K}$, and plug it back into our
$\tau = K \theta$ equation, we will get a $\hat{\tau}$ as predicted by our
model. We can then compare $\hat{\tau}$ to our measured $\tau$. We can then also
calculate our torque estimate residuals.

I had a lot of confusion about what exactly I should calculate for the
residuals. Did I need to calculate estimates of the force, and then residuals
for that, and the estimates of the deflection using the measured torque and the estimated K, and then plot them against everything? This would result in an intensely combinatorial data set.

Eventually I settled on, the torque is ultimately what we want to be able to
estimate, as based.  We assume the deflections as measured by the IMU can give
give us the torque. Assuming as well that we get an accurate reading of the
force contact location, we can then derive the force applied ( $F = \tau /\ r$ ). Thus, we only need the residuals for the torque estimates for x, y (assuming torque z is always zero).

Additionally, I was sad to learn that the K bottom row estimate would always be
zero. I was hoping that, since I had datapoints with nonzero $\theta$ x,y,z,
then the least squares solution might find something interesting, e.g. $0 = -0.2x + 1.5y -.1z$. 
However, it appears that our assumption that force is applied at $r_z = 0$
always, means that we cannot solve for anything meaningful for the K matrix.


## Todo

Actually interpret the graphs... and reduce the rmse.
(handtune a basis function to play around, then maybe NN to learn the correct basis function?)

## Project Timeline tracking
* Goal this week 
[TODO]
* Accomplished this week 
[TODO]
* Goal upcoming week 
[TODO]

