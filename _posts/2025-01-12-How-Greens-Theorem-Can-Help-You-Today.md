---
layout: post
title: "How Green's Theorem Can Help You Today"
categories: [Math]
---
This idea came to me while I was sitting in my Calc III class in sophmore year. I did some tests and it actually works pretty well. I haven't seen anyone else write/do something like this before, so I think it might be novel. However, it is kinda niche and pretty trivial, so I don't think it warrants a paper. So instead, I bless you guys, my faithful readers, to this neat result.

I also wrote a python package to facilitate the use of my result. Can be found [here](https://github.com/xan3c/vectorcalc).


---
## APPLICATIONS OF GREEN’S THEOREM FOR NUMERICAL QUADRATURE


### Abstract:
Certain integrals of two variables over a planar region are divergent or may take significant wall time when computed numerically. This paper presents the usage of Green's theorem to transform such integrals over a plane to a line integral over its boundary. This can reduce the order of a numerical integration from $$O(N^2)$$ to $$O(N)$$ for a class of functions defined on regions bounded by $$C^1$$ curves. We describe a general method of applying Green's theorem to such integrals, as well as the improved convergence and wall times as a result of this application. Additionally, we introduce a new Python package, called Vectorcalc, which aids in computing numerical line integrals. Experimental results demonstrate the effectiveness of the proposed method compared to traditional double quadrature methods, showcasing its ability to converge efficiently for highly variable integrals. The application of Green's theorem offers a practical solution for numerical quadrature in cases where traditional methods fail to converge or are computationally intensive.

Full report can be found [here](/assets/greens_theorem_report.pdf). Disclaimer: I wrote this in my 2nd year, so my writing and results are not as mature. I would've done a few things differently now.