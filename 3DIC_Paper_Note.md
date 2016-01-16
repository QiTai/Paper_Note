##leakage##

####LightSim : A leakage Aware Ultrafast Temperature Simulator####

+ Idea
	* LightSim : An ultra-fast temperature simulator  *perform both staedy and transient thermal analysis*
	* Hankel transform to derive a transient version of the Green's function ---**takes into account the feedback loop between temperature and leakage**---convolve the Green's fucntion with the power map???
	* The crux of the author is to create a transient version of the Green's function (impulse response of a point power source) that takes the leakage feedback loop into account + standard method --- convoles the Green's fucntion with the power profile.

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
	
		![BSIM4](/img/LightSim/BSIM4.PNG) *Mind that: here .png will not show up*

	* P_leak has a complex dependence on temperature, and is exponentially dependent on temperature for high values(>500mV) of the threshold voltage. In modern processors, the threshold voltages and temperature becomes approximately linear. *Has been used to speed up thermal simulator[[9]](http://ecee.colorado.edu/~shangl/papers/liu07mar.pdf) and dynamic thermal management[5,6]*
	
		![Leakage model](/img/LightSim/Leakage_Models.PNG)

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
 	![Result Comparison](/img/LightSim/Speed.PNG)

+ Que
	* Hankel transform
	* Green Function 


