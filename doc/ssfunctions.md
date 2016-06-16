## Misc

### make_symmetric_matrix

```
int make_symmetric_matrix(matrix x)
```
Ensure a matrix is symmetric.
This returns $0.5 * (x + x')$.

### to_matrix_colwise

```
matrix to_matrix_colwise(vector v, int r, int c)
```
Return a $r \times c$ matrix from a vector $v$ with $r \times c$.
This function assumes that elements in the vector are column-major.

### to_matrix_rowwise

```
matrix to_matrix_rowwise(vector v, int r, int c)
```
Return a $r \times c$ matrix from a length $r c$ vector $v$.
This function assumes that elements in the vector are row-major.

### to_vector_colwise

```
vector to_vector_colwise(matrix x)
```
Return a lengths $r c$ vector from a length $r \times c$ matrix $x$.
This function fills in elements column-major.


### to_vector_rowwise

```
vector to_vector_rowwise(matrix x)
```
Return a length $r c$ vector from a  $r \times c$ matrix $x$.
This function fills in elements row-major.


## Filtering

```
matrix ssm_filter_predict_obs(vector y, matrix Z, matrix H, vector a, matrix P)
```

Given $p \times 1$ vector $\vec{a}_t = \E(\alpha_t | Y_{t - 1})$ and $\mat{P}_t = \Var(\alpha_t | Y_{t - 1})$, calculate the one-step ahead forecast error $v_t = y_t - \E(y_t | Y_{t - 1})$ and the precision of the forecast, $F^{-1}_t = \Var(y_t | Y_{t - 1})^{-1}$.

This returns a $p \times (p + 1)$ matrix containing both $v_t$ and $F^{-1}$,
$$
\begin{bmatrix}
v_t & F^{-1}_t
\end{bmatrix} =
\begin{bmatrix}
v_{t,1} & F^{-1}_{t, 1, 1} &  F^{-1}_{t, 1, 2} & \cdots & F^{-1}_{t, 1, p} \\
v_{t,2} & F^{-1}_{t, 2, 1} &  F^{-1}_{t, 2, 2} & \cdots & F^{-1}_{t, 2, p} \\
\vdots & \vdots & \ddots & \vdots\\
v_{t,p} & F^{-1}_{t, p, 1} &  F^{-1}_{t, p, 2} & \cdots & F^{-1}_{t, p, p}
\end{bmatrix} .
$$

```
matrix ssm_filter_update(...)
```
Given $p \times 1$ vector $\vec{a}_t = \E(\alpha_t | Y_{t - 1})$ and $\mat{P}_t = \Var(\alpha_t | Y_{t - 1})$, the one-step ahead forecast error $v_t = y_t - \E(y_t | Y_{t - 1})$, and the precision of the forecast, $F^{-1}_t = \Var(y_t | Y_{t - 1})^{-1}$, calculate the filtered state $a_{t| t} = \E(\alpha_t | Y_t)$ and $P_{t | t} = \Var(\alpha_t | Y_t)$.

Returns a $m \times (1 + m)$ matrix with $a_{t|t}$ and $P_{t|t}$,
$$
\begin{bmatrix}
a_{t|t} & P_{t|t}
\end{bmatrix} =
\begin{bmatrix}
a_{t|t,1} & P_{t|t, 1, 1} &  P_{t|t, 1, 2} & \cdots & P_{t|t, 1, m} \\
a_{t|t,2} & P_{t|t, 2, 1} &  P_{t|t, 2, 2} & \cdots & P_{t|t, 2, m} \\
\vdots & \vdots & \ddots & \vdots\\
a_{t|t,m} & P_{t|t, m, 1} &  P_{t|t, m, 2} & \cdots & P_{t|t, m, m}
\end{bmatrix} .
$$


```
matrix ssm_filter_predict_state(...)
```
Given $p \times 1$ vector $\vec{a}_t = \E(\alpha_t | Y_{t - 1})$ and $\mat{P}_t = \Var(\alpha_t | Y_{t - 1})$, the one-step ahead forecast error $v_t = y_t - \E(y_t | Y_{t - 1})$, and the precision of the forecast, $F^{-1}_t = \Var(y_t | Y_{t - 1})^{-1}$, predict the state in the next period, $a_{t+ t} = \E(\alpha_{t + 1}| Y_t)$ and $P_{t + t1} = \Var(\alpha_{t + 1} | Y_t)$.

Returns a $p \times (1 + p)$ matrix with $a_{t + 1}$ and $P_{t + 1}$,
$$
\begin{bmatrix}
a_{t + 1} & P_{t + 1}
\end{bmatrix} =
\begin{bmatrix}
a_{t + 1,1} & P_{t + 1, 1, 1} &  P_{t + 1, 1, 2} & \cdots & P_{t + 1, 1, m} \\
a_{t + 1,2} & P_{t + 1, 2, 1} &  P_{t + 1, 2, 2} & \cdots & P_{t + 1, 2, m} \\
\vdots & \vdots & \ddots & \vdots\\
a_{t + 1,m} & P_{t + 1, m, 1} &  P_{t + 1, m, 2} & \cdots & P_{t + 1, m, m}
\end{bmatrix} .
$$

```
matrix ssm_filter_predict_state_2(...)
```
Given $p \times 1$ vector $\vec{a}_{t|t} = \E(\alpha_t | Y_{t})$ and $\mat{P}_{t|t} = \Var(\alpha_t | Y_{t |t})$, predict the state in the next period, $a_{t+ t} = \E(\alpha_{t + 1}| Y_t)$ and $P_{t + t1} = \Var(\alpha_{t + 1} | Y_t)$.

Returns a $p \times (1 + p)$ matrix with $a_{t|t}$ and $P_{t|t}$, in the same format as `ssm_filter_predict_state_2`.