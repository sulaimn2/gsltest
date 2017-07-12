# gsltest

Noticed weird output when I ran my library on 64 bit linux in dev, so I tried to hunt down where the problem was.
In my library I am using gsl_multifit_linear from libgsl. I have to tried to reproduce the problem with a simpler setup as follows.

Setup:
We have an X matrix with 26 coefficients and 151 observations. See x_matrix [] in gsltest.cpp
We also have a Y vector with 151 observations. See y_matrix [] in gsltest.cpp
We run gsl_multifit_linear on X and Y and produce a c vector as the output.
In 32 bit, the output was: 

```
c = [2.70409, 9.5864, -0.670502, 24.2728, 13.9393, 3.23134, 8.22729, 9.4926, -0.213974, 6.85192, 0.215701, 7.49826, 7.22713, -3.39073, 13.5852, 17.3663, -0.559321, -1.70083, 18.8802, 5.18303, 3.48551, 1.42608, 9.17107, 22.2, -5.66091, 2.83766] //formatted for human readability.
```

In 64 bit, the output was:
```
c = [2.87233e+13, 2.87233e+13, 2.87233e+13, 2.87233e+13, 2.87233e+13, 2.87233e+13, 2.87233e+13, 2.87233e+13, 2.87233e+13, 2.87233e+13, 2.87233e+13, 2.87233e+13, 2.87233e+13, 2.87233e+13, -2.87233e+13, -2.87233e+13, -2.87233e+13, -2.87233e+13, -2.87233e+13, -2.87233e+13, -2.87233e+13, -2.87233e+13, -2.87233e+13, -2.87233e+13, -2.87233e+13, -2.87233e+13]
```

gsl_multifit_linear works when I had smaller test cases (with less observations) so I tried to hunt down and see if there was a threshold. In gsltest.cpp line 171, all outputs matched between 32 bit and 64 bit for all number of observations until n=100. The moment n=101, the outputs started being all garbage values for 64 bit linux. This number might just be a coincidence but I am not sure.
