---
layout:     post
title:      "Bspline image registration for 3D medical images in Java"
subtitle:   ""
date:       2017-12-07 06:55:00
author:     "Pingge Jiang"
header-img: "img/home-bg.jpg"
catalog:    true
categories: research
tags:
    - [B-spline, registration, java]

---
A Java version for 3D B-spline registration on medical images(mha) is uploaded to my ![github repository](https://github.com/pinggejiang/bspline_java_3d).

__Note: the code is for resarch use only, performance is not optimized__

It's a translated and simplified version from C++ project written by Dr. James A. Shackleford from Drexel University. Implemention details please refer to
#### Shackleford JA, Kandasamy N, Sharp GC. On developing B-spline registration algorithms for multi-core processors. Physics in medicine and biology. 2010 Oct 12;55(21):6329.

### files included are:

Bspline_main.java
BSPLINE.java
Bspline_optimize_data.java
BSPLINE_Options.java
Dev_Pointers_Bspline.java
BSPLINE_Opts.java
LBFGS.java
BSPLINE_Parms.java
Mcsrch.java
BSPLINE_SCORE_CAL.java
BSPLINE_Score.java
Volume.java
BSPLINE_State.java
Volume_proc.java
BSPLINE_Xform.java
lbfgs_parameter_t.java
mha_oper.java


### usage:

javac Bspline_main.java


A more comprehensive implementation of 3D B-spline registration in C++ please find from the open source software ![plastimatch](http://plastimatch.org/)

#### Copyright reserved.

Pingge Jiang
