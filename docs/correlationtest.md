# Correlation test and t-test
Here, I try to show that a correlation test and t-test on the slope of a regression analysis (single linear regression) give identical results. The recipes for the two tests were taken from @whitlock2015 .

## Notation
Assume we have an explanatory variable $x$ and a response variable $y$. The former might for example be average foraging rate and the latter body size. In total we have $N$ observations, with $x_i$ the value $x$ for individual $i$ and $y_i$ the value of $y$ for individual $i$. The average values of $x$ and $y$ are given by $\overline{x}$ and $\overline{y}$ respectively. Throughout the following we use the following definitions for variance ($\sigma_x^2$, $\sigma_y^2$) and covariance ($\sigma_{x,y}$):
\begin{align}
\sigma_x^2 &= \frac{\sum_{i=1}^N (x_i - \overline{x})^2}{N-1}\\
\sigma_y^2 &= \frac{\sum_{i=1}^N (y_i - \overline{y})^2}{N-1}\\
\sigma_{x,y} &= \frac{\sum_{i=1}^N (x_i - \overline{x})(y_i - \overline{y})}{N-1}\\
\end{align}



## Linear model
Using the formula in @whitlock2015 , we can determine the slope for the line between the points as:
\begin{equation}
b=\frac{\sum_{i=1}^N (x_i-\overline{x})(y_i-\overline{y})}{\sum_{i=1}^N (x_i - \overline{x})^2} = \frac{\sigma_{x,y}}{\sigma_x^2}
\end{equation}
with standard error:
\begin{align}
\text{SE}_b &= \sqrt{\frac{\sum_{i=1}^N (y_i-\overline{y})^2-b\sum_{i=1}^N (x_i-\overline{x})(y_i - \overline{y})}{(N-2)\sum_{i=1}^N (x_i-\overline{x})^2}}\\
&= \sqrt{\frac{\sigma_y^2-b\sigma_{x,y}}{(N-2)\sigma_x^2}}\\
&= \sqrt{\frac{\sigma_x^2\sigma_y^2-\sigma_{x,y}^2}{(N-2)\sigma_x^4}}\\
\end{align}
and t-statistic ($t_b$):
\begin{align}
t_b = \frac{b}{\text{SE}_b} &= \sqrt{\frac{\sigma_{x,y}^2}{\sigma_x^4}\frac{(N-2)\sigma_x^4}{\sigma_x^2\sigma_y^2-\sigma_{x,y}^2}}\\
&= \sqrt{\frac{(N-2)\sigma_{x,y}^2}{\sigma_x^2\sigma_y^2-\sigma_{x,y}^2}}
\end{align}


## Correlation test
the correlation coefficient $r$ is equals [@whitlock2015]:
\begin{equation}
r = \frac{\sum_{i=1}^N (x_i - \overline{x})(y_i - \overline{y})}{\sqrt{\sum_{i=1}^N (x_i - \overline{x})^2}\sqrt{\sum_{i=1}^N (y_i - \overline{y})^2}} = \frac{\sigma_{x,y}}{\sigma_x\sigma_y}
\end{equation}
this regression coefficient has an associated standard error ($\text{SE}_r$) of:
\begin{align}
\text{SE}_r &= \sqrt{\frac{1-r^2}{N-2}}\\
&= \sqrt{\frac{1-\frac{\sigma_{x,y}^2}{\sigma_x^2\sigma_y^2}}{N-2}}\\
&= \sqrt{\frac{\sigma_x^2\sigma_y^2-\sigma_{x,y}^2}{(N-2)\sigma_x^2\sigma_y^2}}\\
\end{align}
the corresponding t-statistic is then:
\begin{align}
t_r = \frac{r}{\text{SE_r}} &= \sqrt{\frac{\sigma_{x,y}^2}{\sigma_x^2\sigma_y^2}\frac{(N-2)\sigma_x^2\sigma_y^2}{\sigma_x^2\sigma_y^2-\sigma_{x,y}^2}} \\
&= \sqrt{\frac{(N-2)\sigma_{x,y}^2}{\sigma_x^2\sigma_y^2-\sigma_{x,y}^2}} \\
&= t_b
\end{align}
Hence, both methods should give identical results.
