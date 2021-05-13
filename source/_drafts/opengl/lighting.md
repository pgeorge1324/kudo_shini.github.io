# Lighting

- ### Point/Omni Light 

  - ##### 平方反比衰减光源 light = light0*pow(r0/r,2);

    1. 问题：当距离太近时出现过曝；
       - 解决1： light = light0*pow(r0,2)/(pow(r,2)+e);  ue4使用e=1cm;	
       - 解决2： light = light0*pow(r0,2)/pow(max(r,rmin),2);
    2. 问题：当距离太远时光照影响几乎没有，但计算影响性能