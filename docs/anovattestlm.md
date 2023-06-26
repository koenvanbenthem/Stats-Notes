# Differences between two groups
Here, I try to show that ANOVA, two sample t-test and lm return the same result when the data only consist of two groups. The recipes for ANOVA, t-test and lm were taken from @whitlock2015 .

## Notation
Assume we have an explanatory variable $x$ that was only measured at two specific values: $A$ and $B$ (e.g. $x$ was temperature and only two different values, e.g. $A=18^\circ C$ and $B=21^\circ C$). In order to simplify the notation, the datapoints have been ordered, such that the first $N_A$ datapoints have $x=A$ and the last $N_B$ have $x=B$, with a total number of data points $N=N_A+N_B$. We assume the response variable $y$ to be normally distributed. It may for example represent body size. 

We note that the group specific means of $y$ are:
\begin{equation}
\overline{y_A} = \frac{1}{N_A}\sum_{i=1}^{N_A} y_i
\end{equation}


\begin{equation}
\overline{y_B} = \frac{1}{N_B}\sum_{i=N_A+1}^{N} y_i
\end{equation}
and the overall mean:

\begin{equation}
\overline{y} = \frac{1}{N}\sum_{i=1}^{N} y_i
\end{equation}

## t-test
For a simple t-test, we first have to calculate the standard error, which in turn depends on the pooled sample variance, which in turn depends on the separate variances. We denote the variances for group A and B as:
\begin{align}
s_A^2 &= \sum_{i=1}^{N_A}\frac{(y_i-\overline{y_A})^2}{N_A-1} \\
s_B^2 &= \sum_{i=N_A+1}^{N_B}\frac{(y_i-\overline{y_B})^2}{N_B-1}
\end{align}
The pooled sample variance is then:
\begin{equation}
s_p^2 = \frac{\text{df}_As_A^2 + \text{df}_Bs_B^2}{\text{df}_A+\text{df}_B} = \frac{\sum_{i=1}^{N_A}(N_A-1)\frac{(y_i-\overline{y_A})^2}{N_A-1} + \sum_{i=N_A+1}^{N_B}(N_B-1)\frac{(y_i-\overline{y_B})^2}{N_B-1} }{N - 2}
\end{equation}
The standard error is then:
\begin{equation}
\text{SE}_{\overline{y_A}-\overline{y_B}} = \sqrt{s_p^2\left(\frac{1}{N_A}+\frac{1}{N_B}\right)} = \sqrt{\frac{\sum_{i=1}^{N_A}(y_i-\overline{y_A})^2 + \sum_{i=N_A+1}^{N_B}(y_i-\overline{y_B})^2 }{N - 2}
\left(\frac{1}{N_A}+\frac{1}{N_B}\right)} 
\end{equation}
the test statistic is:
\begin{equation}
t=\frac{\overline{y_A}-\overline{y_B}}{\text{SE}_{\overline{y_A}-\overline{y_B}}} = \sqrt{\frac{N_AN_B(N-2)(\overline{y_A}-\overline{y_B})^2}{N\left(\sum_{i=1}^{N_A}(y_i-\overline{y_A})^2 + \sum_{i=N_A+1}^{N_B}(y_i-\overline{y_B})^2\right)}}
\end{equation}

## ANOVA
We first calculate the group sum of squares:
\begin{equation}
\text{SS}_\text{groups} = N_A (\overline{y_A}-\overline{y})^2 + N_B (\overline{y_B}-\overline{y})^2
\end{equation}
as well as the error sum of squares:
\begin{equation}
\text{SS}_\text{error} = \sum_{i=1}^{N_A} (y_i - \overline{y_A})^2 + \sum_{i=N_A+1}^{N} (y_i - \overline{y_B})^2
\end{equation}
From here, we can easily obtain the $F$-statistic as:
\begin{equation}
F = \frac{(N-2)\left(N_A (\overline{y_A}-\overline{y})^2 + N_B (\overline{y_B}-\overline{y})^2\right)}{\sum_{i=1}^{N_A} (y_i - \overline{y_A})^2 + \sum_{i=N_A+1}^{N} (y_i - \overline{y_B})^2}
\end{equation}
This already looks a bit like $t$, but in order to see the equivalency between the two, we have a bit more rewriting to do. Specifically, we will focus on:
\begin{align}
N_A (\overline{y_A}-\overline{y})^2 + N_B (\overline{y_B}-\overline{y})^2 &= N_A\overline{y_A}^2 - 2N_A \overline{y_A}\overline{y} + N_A\overline{y}^2 + N_B\overline{y_B}^2 - 2N_B\overline{y_B}\overline{y} + N_B \overline{y}^2 \\
&= N_A\overline{y_A}^2 - 2(N_A \overline{y_A}+N_B\overline{y_B})\overline{y} + (N_A+N_B)\overline{y}^2 + N_B\overline{y_B}^2\\
&=N_A\overline{y_A}^2 - \frac{(N_A \overline{y_A}+N_B\overline{y_B})^2}{N} + N_B\overline{y_B}^2\\
&= N_A\overline{y_A}^2 - \frac{N_A^2 \overline{y_A}^2+N_B^2\overline{y_B}^2 + 2N_AN_B\overline{y_A}\overline{y_B}}{N} + N_B\overline{y_B}^2\\
&= \frac{N_AN_B}{N}\left(\frac{N}{N_B}\overline{y_A}^2 - \frac{N_A}{N_B} \overline{y_A}^2-\frac{N_B}{N_A}\overline{y_B}^2 - 2\overline{y_A}\overline{y_B} + \frac{N}{N_A}\overline{y_B}^2\right)\\
&= \frac{N_AN_B}{N}\left(\frac{N-N_A}{N_B}\overline{y_A}^2 - 2\overline{y_A}\overline{y_B} + \frac{N-N_B}{N_A}\overline{y_B}^2\right)\\
&= \frac{N_AN_B}{N}\left(\overline{y_A}^2 - 2\overline{y_A}\overline{y_B} + \overline{y_B}^2\right)\\
&= \frac{N_AN_B}{N}\left(\overline{y_A} -  \overline{y_B}\right)^2\\
\end{align}
Where in going from the second to the third line, we have used that $N\overline{y}=N_A\overline{y_A}+N_B\overline{y_B}$, and on going from the sixt to the seventh line that $N-N_A = N_B$ and equivalently $N-N_B= N_A$. We can not use this identity to rewrite our $F$-statistic:
\begin{equation}
F = \frac{(N-2)N_AN_B\left(\overline{y_A}-\overline{y_B}\right)^2}{N\left(\sum_{i=1}^{N_A} (y_i - \overline{y_A})^2 + \sum_{i=N_A+1}^{N} (y_i - \overline{y_B})^2\right)} = t^2
\end{equation}

and hence $F=t^2$.

## linear model
Using the formula in @whitlock2015 , we can determine the slope for the line between the points as:
\begin{equation}
b=\frac{\sum_{i=1}^N (x_i-\overline{x})(y_i-\overline{y})}{\sum_{i=1}^N (x_i - \overline{x})^2}
\end{equation}
with standard error:
\begin{equation}
\text{SE}_b = \sqrt{\frac{\sum_{i=1}^N (y_i-\overline{y})^2-b\sum_{i=1}^N (x_i-\overline{x})(y_i - \overline{y})}{(N-2)\sum_{i=1}^N (x_i-\overline{x})^2}}
\end{equation}
In order to simplify this for the specific case with only two possible values of x, we first focus on the following simpler expressions:
\begin{align}
\overline{x} &= \frac{N_A A + N_B B}{N}
\end{align}
We can use this to write:
\begin{align}
\sum_{i=1}^N (x_i-\overline{x})(y_i-\overline{y}) &= \sum_{i=1}^{N_A} (A - \frac{N_A A + N_B B}{N})(y_i - \overline{y}) + \sum_{i=N_A+1}^{N} (B - \frac{N_A A + N_B B}{N})(y_i - \overline{y})  \\
 &= \sum_{i=1}^{N_A} (\frac{NA}{N} - \frac{N_A A + N_B B}{N})(y_i - \overline{y}) + \sum_{i=N_A+1}^{N} (\frac{NB}{N} - \frac{N_A A + N_B B}{N})(y_i - \overline{y})  \\
 &= \frac{1}{N}\left(\sum_{i=1}^{N_A} N_B (A - B)(y_i - \overline{y}) + \sum_{i=N_A+1}^{N} N_A (B - A)(y_i - \overline{y})\right)  \\
&= \frac{(A - B)}{N}\left(\sum_{i=1}^{N_A} N_B (y_i - \overline{y}) - \sum_{i=N_A+1}^{N} N_A (y_i - \overline{y})\right)  \\
&= \frac{(A - B)}{N}\left(N_B \sum_{i=1}^{N_A} y_i - N_BN_A\overline{y} - N_A \sum_{i=N_A+1}^{N} y_i + N_AN_B \overline{y}\right)  \\
&= \frac{(A - B)}{N}\left(N_B \sum_{i=1}^{N_A} y_i  - N_A \sum_{i=N_A+1}^{N} y_i \right)  \\
&= \frac{(A - B)}{N}\left(N_BN_A \overline{y_A}  - N_A N_B \overline{y_B}\right)  \\
&= \frac{(A - B)N_AN_B}{N}\left( \overline{y_A}  -  \overline{y_B}\right)  \\
\end{align}
Similarly, we attempt to simplify the term:
\begin{align}
\sum_{i=1}^N (x_i - \overline{x})^2 &= \sum_{i=1}^N (x_i - \frac{N_A A + N_B B}{N})^2 \\
&= \sum_{i=1}^{N_A} (A - \frac{N_A A + N_B B}{N})^2 + \sum_{i=N_A+1}^N (B - \frac{N_A A + N_B B}{N})^2 \\ 
&= \sum_{i=1}^{N_A} (\frac{NA}{N} - \frac{N_A A + N_B B}{N})^2 + \sum_{i=N_A+1}^N (\frac{NB}{N} - \frac{N_A A + N_B B}{N})^2 \\
&= \sum_{i=1}^{N_A} (\frac{N_B (A - B)}{N})^2 + \sum_{i=N_A+1}^N (\frac{N_A (B-A)}{N})^2 \\
&= N_A (\frac{N_B (A - B)}{N})^2 + N_B (\frac{N_A (B-A)}{N})^2 \\
&= (A-B)^2\left(N_A \frac{N_B^2 }{N^2} + N_B \frac{N_A^2 }{N^2}\right) \\
&= (A-B)^2\left( \frac{N_AN_B^2 + N_B N_A^2 }{(N_A+N_B)^2}\right) \\
&= (A-B)^2\left( \frac{N_AN_B (N_A + N_B)}{(N_A+N_B)^2}\right) \\
&= (A-B)^2 \frac{N_AN_B }{N} \\
\end{align}
After a lot of rewriting, we can now use these expressions to find a much expected answer:
\begin{align}
b&=\frac{\sum_{i=1}^N (x_i-\overline{x})(y_i-\overline{y})}{\sum_{i=1}^N (x_i - \overline{x})^2}\\
&=\frac{\overline{y_A}-\overline{y_B}}{A-B}
\end{align}
in other words: when we have only two distinct values for the $x$-variable, the slope is equal to the difference in the means in $y$ between these to groups and the difference between the $x$-values. This result was of course completely to be expected, and we could have obtained it through reasoning alone, without the need to rewrite these equations at length.

The next step is to rewrite the standard error for the slope:
\begin{align}
\text{SE}_b &= \sqrt{\frac{\sum_{i=1}^N (y_i-\overline{y})^2-b\sum_{i=1}^N (x_i-\overline{x})(y_i - \overline{y})}{(N-2)\sum_{i=1}^N (x_i-\overline{x})^2}} \\
&= \sqrt{\frac{\sum_{i=1}^N (y_i-\overline{y})^2-\frac{\overline{y_A}-\overline{y_B}}{A-B}\frac{(A - B)N_AN_B}{N}\left( \overline{y_A}  -  \overline{y_B}\right)}{(N-2)(A-B)^2 \frac{N_AN_B}{N}}} \\
&= \frac{1}{A-B}\sqrt{\frac{\sum_{i=1}^N (y_i-\overline{y})^2-\frac{N_AN_B}{N}\left( \overline{y_A}  -  \overline{y_B}\right)^2}{(N-2) \frac{N_AN_B}{N}}} 
\end{align}
Now, the test statistic for testing whether $b$ is equal to zero is ($t_b$):
\begin{align}
t_b &= \frac{b}{\text{SE}_b}\\
&= \sqrt{\frac{(N-2) \frac{N_AN_B}{N}(\overline{y_A}-\overline{y_B})^2}{\sum_{i=1}^N (y_i-\overline{y})^2-\frac{N_AN_B}{N}\left( \overline{y_A}  -  \overline{y_B}\right)^2}} \\ 
&= \sqrt{\frac{(N-2) N_AN_B(\overline{y_A}-\overline{y_B})^2}{N\sum_{i=1}^N (y_i-\overline{y})^2-N_AN_B\left( \overline{y_A}  -  \overline{y_B}\right)^2}}
\end{align}
Let's first rewrite the following:
\begin{align}
N&\sum_{i=1}^N (y_i-\overline{y})^2-N_AN_B\left( \overline{y_A}  -  \overline{y_B}\right)^2  = N\sum_{i=1}^N (y_i^2-2y_i\overline{y} + \overline{y}^2)-N_AN_B\left( \overline{y_A}  -  \overline{y_B}\right)^2\\
&= N\sum_{i=1}^N y_i^2-2N^2\overline{y}^2 + N^2\overline{y}^2-N_AN_B\left( \overline{y_A}  -  \overline{y_B}\right)^2\\
&= N\sum_{i=1}^N y_i^2-(N\overline{y})^2-N_AN_B\left( \overline{y_A}  -  \overline{y_B}\right)^2\\
&= N\sum_{i=1}^{N}   y_i^2  -(N_A\overline{y_A}+N_B\overline{y_B})^2-N_AN_B\left( \overline{y_A}  -  \overline{y_B}\right)^2\\
&= N\sum_{i=1}^{N}   y_i^2  -N_A^2\overline{y_A}^2-N_B^2\overline{y_B}^2-2N_AN_B\overline{y_A}\overline{y_B}-N_AN_B\overline{y_A}^2 - N_AN_B\overline{y_B}^2 + 2N_AN_B\overline{y_A}\overline{y_B}\\
&= N\sum_{i=1}^{N}   y_i^2  -N_A(N_A+N_B)\overline{y_A}^2-N_B(N_B+N_A)\overline{y_B}^2 \\
&= N\left(\sum_{i=1}^{N}   y_i^2  -N_A\overline{y_A}^2-N_B\overline{y_B}^2\right) \\
&= N\left(\sum_{i=1}^{N}   y_i^2  -2N_A\overline{y_A}^2+N_A\overline{y_A}^2-2N_B\overline{y_B}^2+N_B\overline{y_B}^2\right) \\
&= N\left(\sum_{i=1}^{N}   y_i^2  -\sum_{i=1}^{N_A} 2\overline{y_A}y_i+\sum_{i=1}^{N_A}\overline{y_A}^2-2\sum_{i=N_A+1}^N\overline{y_B}y_i+\sum_{i=N_B+1}^N\overline{y_B}^2\right) \\
&= N\left(\sum_{i=1}^{N_A}   y_i^2 + \sum_{i=N_A+1}^{N}   y_i^2  -\sum_{i=1}^{N_A} 2\overline{y_A}y_i+\sum_{i=1}^{N_A}\overline{y_A}^2-2\sum_{i=N_A+1}^N\overline{y_B}y_i+\sum_{i=N_B+1}^N\overline{y_B}^2\right) \\
&= N\left(\sum_{i=1}^{N_A}   \left(y_i^2 - 2\overline{y_A}y_i+ \overline{y_A}^2 \right) + \sum_{i=N_A+1}^{N}   \left(y_i^2 -2\overline{y_B}y_i+\overline{y_B}^2 \right)\right) \\
&= N\left(\sum_{i=1}^{N_A}   \left(y_i - \overline{y_A} \right)^2 + \sum_{i=N_A+1}^{N}   \left(y_i -\overline{y_B}\right)^2\right) \\
\end{align}
By using this simplification, we can rewrite the value of the $t$-statistic as:
\begin{equation}
t_b = \sqrt{\frac{(N-2) N_AN_B(\overline{y_A}-\overline{y_B})^2}{ N\left(\sum_{i=1}^{N_A}   \left(y_i - \overline{y_A} \right)^2 + \sum_{i=N_A+1}^{N}   \left(y_i -\overline{y_B}\right)^2\right)}} = t = \sqrt{F}
\end{equation}
and hence we conclude that $t=t_b$.

