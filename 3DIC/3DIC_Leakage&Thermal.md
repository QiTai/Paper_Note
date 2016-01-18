##leakage##

####[LightSim : A leakage Aware Ultrafast Temperature Simulator](http://www.cse.iitd.ac.in/~srsarangi/files/papers/lightsim.pdf) *2014ASPDAC* ####
	
> Also Refer to [Slides](http://www.aspdac.com/aspdac2014/technical_program/pdf/9C-3.pdf)

+ Idea
	* LightSim : An ultra-fast temperature simulator  *perform both staedy and transient thermal analysis*
	* Hankel transform to derive a transient version of the Green's function ---**takes into account the feedback loop between temperature and leakage**---convolve the Green's fucntion with the power map
	* The crux of the author is to create a transient version of the Green's function (impulse response of a point power source) that takes the leakage feedback loop into account + standard method --- convoles the Green's fucntion with the power profile.
	* For 2D chip
	* Use Hankel transform to simplify a 2-D problem into a 1-D problem, reducing its complexity and running time.

+ Deficiency
	* the leakage at the rim of the chip are not derived rigorously, and use empirical factors.

+ Past Methodology on model temperature
	* finite difference and finite element methods **SLOW**
	* HotSpot[[14]](http://www.cs.virginia.edu/~skadron/Papers/hotspot_isca03.pdf)---creates an electrical circuit, processes it using existing circuit simulation methods **FASTER**  
	* Sesc-Therm **FASTER**
	* PowerBlur [[12]](https://www.researchgate.net/publication/224129688_Experimental_validation_of_the_power_blurring_method)
	* CONTILTS [[4]](http://euler.ecs.umass.edu/research/hkk-Jolpe-2007.pdf)
	* Liu et. al. [[7]](http://www.pitt.edu/~juy9/papers/thermal_iccad05.pdf)
	
+ Note
	* Temperature has a direct effect on the amount of leakage power.
	* The static or leakage power is a funciton of temperature, and traditionally described by Equation1 in the simplified BSIM4 model.
	
		![BSIM4](/img/3DIC/LightSim/BSIM4.PNG)
	> *Mind that: here .png will not show up*

	* P_leak has a complex dependence on temperature, and is exponentially dependent on temperature for high values(>500mV) of the threshold voltage. In modern processors, the threshold voltages and temperature becomes approximately linear. *Has been used to speed up thermal simulator[[9]](http://ecee.colorado.edu/~shangl/papers/liu07mar.pdf) and dynamic thermal management[5,6]*
	
		![Leakage model](/img/3DIC/LightSim/Leakage_Models.PNG)

	* This heat spread function(f_sp) is also known as the Green's function of the system at the center of the die.
	* use zero order Hankel transform to reduce the 2D problem to a 1D problem. A zero order Hankel transform is equivalent to 2D Fourier transforms of radially symmetric functions in polar coordinates.
	* Corrections for the edges and corners
		* technique proposed by Park et. al.[[12]](https://www.researchgate.net/publication/224129688_Experimental_validation_of_the_power_blurring_method)
	* Using the Green's functions--same as prior approaches[[18]](http://www.ee.umn.edu/users/sachin/conf/iccad05yz.pdf)
	* The open source C code for Hotspot 5 is available for download.

+ Implementation Detail
	* offline part : R; HotSpot tool to compute three different Green's function(center, edge, corner)
	* online part : C

+ Result Comparison

    ![Result Comparison](/img/3DIC/LightSim/Speed.PNG)

+ Further Reading
	* Hankel transform
	* Green Function 
	* 2D Leakage related paper
	* [Green's function for thermal simulation[18]](http://www.ee.umn.edu/users/sachin/conf/iccad05yz.pdf)


####A Fast Leakage Aware 3D Thermal Simulator ####

+ Idea
	* propose two 3D temperature simulations, 3DSimF and 3DSim 
	* quite like the former LightSim paper, just extends it to 3DIC
	* Prerequisite: over the operating temperature range of real ICs, leakage may be assumed to be linearly dependent on temperature, as was shown experimentally in [[9]](http://ecee.colorado.edu/~shangl/papers/liu07mar.pdf)[[14]](http://www.cse.iitd.ac.in/~srsarangi/files/papers/lightsim.pdf)
	* main concern : speed -- an alg. which is fast and can be employed by designers to predict thermal performance before the detailed layout is available.
	* considering leakage, but avoid running mulriple iterations to determine the temperature profile including the effects of leakage.
	* [[14]](http://www.cse.iitd.ac.in/~srsarangi/files/papers/lightsim.pdf) is the only work that provides a closed form solution for incorporating leakage (albeit for 2-D chips). We extend this method to 3-D chips having multiple layers, and model the effects of cross layer leakage power.
+ Past Methodology on model temperature	
	* For 2D chip: 
		+ Fast Methodology Refer to [[14]](http://www.cse.iitd.ac.in/~srsarangi/files/papers/lightsim.pdf)[[5]](http://pan.baidu.com/s/1PChmY)*Differentiating the roles of ir measurement and simulation for power and temperature-aware design_2009ISPASS*
		+ 3-D Thermal-ADI[[16]](http://robertdick.org/talp/papers/wang-thermal-adi.pdf)
		+ Hotspot[[4]](http://www.cs.virginia.edu/~skadron/Papers/hotspot_tvlsi06.pdf)
		+ Green's Function [[19]](http://www.ee.umn.edu/users/sachin/conf/iccad05yz.pdf) [[22]](http://pan.baidu.com/s/1mh05vXe)*Power Blurring: Fast Static and Transient Thermal Analysis Method for Packaged Integrated Circuits and Power Devices_2014TVLSI*[[14]](http://www.cse.iitd.ac.in/~srsarangi/files/papers/lightsim.pdf)
	* For 3D chip: (limited!)
		+ HS3D[[6]](https://www.ece.ucsb.edu/~yuanxie/Papers/ISQED06-3D.pdf)
		+ 3D-ICE[[15]](http://esl.epfl.ch/files/content/sites/esl/files/3dice/3D-ICE_ICCAD2010.pdf)
		+ [[13]](http://pan.baidu.com/s/1PChmY)*Thermal Simulator of 3d-ic with modeling of anisotropic tsv conductance and microchannel entracnce effects_2013ASPDAC*[[13]slides](http://www.aspdac.com/aspdac2013/archive/pdf/6B-1.pdf)
		
+ Note
	* In 45nm tech and beyond, an estimated 30-50% of the total power is due to leakage[1]
	* Green's Function--The impulse response of a unit point power source(the Dirac Delta function)
	* Hankel Transform--The 2D Fourier transform of a radially symmetric function is equivalent to a zero order 1-D Hankel transform.
	* Full chip model of the 3-D chip
	
	![Full_Chip_Model](/img/3DIC/Fast_Leakage_Aware_3DIC_Thermal_Simulator/Full_Chip_Model.PNG)

	* The temperature rise in a layer is affected by 3 factos:
		+ Dynamic Power 
		+ Leakage Power sources created in that layer 
		+ Leakage Power sources created in other layers 
	* 3D Leakage Aware Heat Spreading Function

+ Implementation Detail
	* Modeling of the 3D Chip
		+ Use Ansys Icepak for modeling the chip for thermal simulations
		+ Use R to implement computing leakage aware Green's fucntions using Hankel transform(3DSim)
		+ Use matlab for 3DSimF 
		+ Matlab is known to be faster than R implementation[17,10]

+ Result Comparison

	![Result_Comparison](/img/3DIC/Fast_Leakage_Aware_3DIC_Thermal_Simulator/Result_Comparison.PNG)

+ Further Reading
	* 3D-ICE[[15]](http://esl.epfl.ch/files/content/sites/esl/files/3dice/3D-ICE_ICCAD2010.pdf)
	* [[22]](http://pan.baidu.com/s/1mh05vXe) *Power Blurring: Fast Static and Transient Thermal Analysis Method for Packaged Integrated Circuits and Power Devices_2014TVLSI*
	* [Also Green's Function[19]](http://www.ee.umn.edu/users/sachin/conf/iccad05yz.pdf)
	* [5]*Differentiating the roles of ir measurement and simulation for power and temperature-aware design*

## Thermal ##

####Power Blurring: Fast Static and Transient Thermal Analysis Method for Packaged Integrated Circuits and Power Devices  *2014TVLSI* ####

+ Idea
	* Power Blurring
		+ matric convolution in analogy with image blurring
		+ applicable to both static and transient thermal simulation
		
+ Deficiency
+ Past Methodology on model temperature
	* FEA (ANSYS)
	* ISAC 
	* HotSpot
	
	
+ Note
	* real geometry of the heat spreader is reversed pyramid structure
	* Error Reduction Step:
		+ Method of Images ??
		+ Intrinsic Error Compensation ??
	* Transient Thermal Simulation ??
	* [HotSpot Thermal Simulator](http://lava.cs.virginia.edu/HotSpot/index.htm)
		+ two modes: 1) the block model mode 2)the grid model mode
	* In this paper, PB relies on two FEA simulations: 
		+ giving the unit impulse response at the center of the chip 
		+ the additional correction factor from the uniform power dissipation profile.
		+ Both are offline.

+ Implementation Detail
	

+ Result Comparison


+ Further Reading
	* [23]*Fast thermal analysis of vertically integrated circuits (3D ICs) using powre blurring methods*  [24]*fast thermal simulations of vertically integrated circuits (3d ics) including thermal vias*-- PB is applicable for vertically 3-D ICs including thermal vias.
+ Que
	* How does the FEA give the Green's Function???





