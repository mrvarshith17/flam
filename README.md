This project fits the model v(t) = e^{M|t|} * sin(0.3t) to rotated and shifted (x, y) data using parameter optimization. Each (x, y) point is transformed into (t, v) using:

t = (x − X) * cos(θ) + (y − 42) * sin(θ)  
v = −(x − X) * sin(θ) + (y − 42) * cos(θ)

The objective is to minimize the mean squared error between v_actual (from data) and v_pred = e^{M|t|} * sin(0.3t), with the constraint that t ∈ (6, 60).

Optimization was performed using Differential Evolution with the following configuration:  
- Bounds: θ ∈ [0, 50], M ∈ [−0.05, 0.05], X ∈ [0, 100]  
- maxiter=1000, popsize=30, mutation=(0.5, 1), recombination=0.7, tol=1e-6  
- Parallel workers enabled if supported

Final parameters obtained:
- θ = 29.99997293°
- M = 0.03000000
- X = 54.99999821
- Final MSE = 1.2153319830668634e-11

Distance Computation:  
We sampled t ∈ [6, 60] using 10,000 uniformly spaced points.  
We computed v_pred(t) using the optimized parameters.  
Expected values were obtained by transforming the original (x, y) dataset using the same θ and X.  
We interpolated these discrete expected v values to match the t sample grid.  
We calculated the L1 distance using trapezoidal integration:  
L1 = ∫ |v_expected(t) − v_pred(t)| dt ≈ Σ |Δ| * dt

Score Normalization:  
We used relative magnitude normalization:  
Score_L1 = max(0, 100 * (1 − L1 / ∫ |v_expected(t)| dt))  
This yields 100 for a perfect fit and scales down proportionally with error.

Explanation & Process Documentation:  
- Dataset overview with shape, structure, and plots of (x, y)  
- Detailed transformation equations for t and v  
- Full model definition and loss formulation  
- Optimization settings and rationale  
- Visual plots of transformed (t, v), model fit overlay, and residuals  
- Step-by-step L1 integration with code and plots  
- Score computation and normalization explanation  
- Conclusions, caveats, and suggestions for improvement

Final Submission Summary:
θ = 29.99997293°, M = 0.03, X = 54.99999821  
Final MSE = 1.2153319830668634e-11  
L1 = 0.006731894929031658 
Score_L1 = 99.9936849059994/100 (≈ 99.99) 
discrete L1 on original points = 0.0001385975250055115
