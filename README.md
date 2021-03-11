# **Index**

1. ## **Abstract**
1. ## **Introduction**
1. ## **Design**
1. ## **MATLAB/Simulink Model**
1. ## **Fuzzy Controller Configuration**
1. ## **Output**
1. ## **Conclusion**
1. ## ` `**References**
#

# **Abstract-**
Speed control is a common application in various types of industrial systems, to either mitigate for any type of fluctuations or disturbances as well as to meet specific speed requirements of the system. The usual controllers used in these cases are PI or PID controllers, which are based on mathematical models of the system and hence, have high accuracy. But these controllers are very sensitive to external disturbances, non-linear gains or other non-linear characteristics of the system, as well as any unmodelled changes.

So, to overcome these challenges, in this project I have designed a speed control system of a standard DC motor with fixed parameters using a fuzzy logic controller on MATLAB Simulink. The controller takes in two inputs, the speed, and the change in speed error, and gives out necessary voltage as output. The fuzzy controller works based on fuzzy logic, which, instead of depending on a strict parameters, creates different scenarios and uses a set of rules called rule base to match those scenarios with the necessary output. This makes the system much more robust and dynamic as compared to the PI/PID controller, and also increases the efficiency of the system.

# **Introduction-**
- ## **DC Speed Control-**

One of the most important features of the dc motor is that its speed can be quite easily controlled based on any requirements using fairly simple methods. This ease of control is not possible in an AC motor.

The concept of speed control is different from speed regulation. In speed regulation, the speed of the motor changes naturally whereas in speed control of a dc motor, the speed of the motor is changed manually by the operator or using some automatic control device, such as PI or PID controller.

The speed (N) of the DC Motor is given as 
`	`![](Aspose.Words.6e40e44f-fce8-4c0f-92b9-9d70c4d112fe.001.png)

Hence, I can see that speed can be controlled by changing-

1. Terminal voltage of the armature, V.
1. External resistance in the armature circuit, Ra.
1. Flux per pole, φ.

Changing the terminal voltage and external resistance involve a change that affects the armature circuit, while changing the flux involves a change in the magnetic field. Therefore speed control of DC motor can be classified into-

1. Armature Control Methods
1. Field Control Methods

In this project, armature control has been implemented.
- ## **Fuzzy Logic-**

The term fuzzy refers to things which are not clear or are vague. Fuzzy Logic is a type of multivalued computing that, unlike the traditional Boolean logic of clear and absolute 1s or 0s, deals with degrees of correctness. So, in situations where there isn’t any clear binary state, fuzzy logic helps us in adding more flexibility in reasoning and operation.**

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Instead of using strict mathematical models, like a PI or PID controller,

it takes into consideration a number of input scenarios and maps it with the desired output using different linguistic if-then conditions called rules for all the different scenarios, which makes it closer to real world scenarios and hence, easier to use. The fuzzy logic is applied in 4 steps-

1. Rule Base- It contains the set of rules and the If-Then conditions, on the basis of linguistic information.
1. Fuzzification- It is used to convert inputs i.e. crisp numbers (which are the inputs from the control system such as speed, pressure, temperature, etc.) into fuzzy sets.
1. Inference Engine- It determines the matching degree of the current fuzzy input with respect to each rule and decides which rules are to be fired according to the input field.
1. Defuzzification- It is used to convert the fuzzy sets obtained by the inference engine into a crisp output.
# **Design-**
- **Modelling of DC Motor-**

  --------------------------
The standard model of the DC Motor is as shown below-
`	`![](Aspose.Words.6e40e44f-fce8-4c0f-92b9-9d70c4d112fe.002.png)

So, based on the model, the parameters for the DC Motor we’ve used are-

![](Aspose.Words.6e40e44f-fce8-4c0f-92b9-9d70c4d112fe.003.png)

- **Modelling of the Fuzzy Logic Controller-**

  --------------------------------------------
1. Rule Base-

The rule base for the fuzzy controller that I are currently using was developed through an intuitive trial and error process. 


![](Aspose.Words.6e40e44f-fce8-4c0f-92b9-9d70c4d112fe.004.png)

1. Fuzzification-


It’s the process of converting a numerical or crisp input into a linguistic variable (fuzzy number). This is done with the different types of fuzzifiers or member functions. There are generally three types of fuzzifiers, which are used for the fuzzification process; they are

\1. Singleton fuzzifier.

\2. Gaussian fuzzifier.

\3. Trapezoidal or triangular fuzzifier.

The ones I have used are the triangular and trapezoidal for the error in speed input, and the just the triangular fuzzifier for the change in error input. 

1. Inference engine-

An inference engine is an information processing system, such as a computer program that systematically employs the inference steps similar to that of a human brain. Here the inference engine is the MATLAB Fuzzy Logic toolbox that is performing these functions. There are two popular methods (max-min and max-product), in this project the max-min has been used.

1. Defuzzification-

There are many defuzzification methods but we’ve used the most commonly used one which is called Center of gravity (COG).

For discrete sets COG is called center of gravity for singletons (COGS) where the crisp control value is the abscissa of the center of gravity of the fuzzy set is calculated as follows:

![](Aspose.Words.6e40e44f-fce8-4c0f-92b9-9d70c4d112fe.005.png)

Where xi is a point in the universe of the conclusion (i=1, 2, 3…) and μc (xi) is the membership value of the resulting conclusion set. For continuous sets summations are replaced by integrals.

# **MATLAB/Simulink Model-**
![](Aspose.Words.6e40e44f-fce8-4c0f-92b9-9d70c4d112fe.006.png)

1. ## **Step Input Block**
The Step Input block provides a constant input to the system initially, 800 at 0 seconds and then 1450 at 5 seconds; this will check if the fuzzy logic controller is responsive to changes in input. Sample time is kept at 0.0001 to get a smoother curve.

![](Aspose.Words.6e40e44f-fce8-4c0f-92b9-9d70c4d112fe.007.png)

1. **Torque Step Input Block**

   ---------------------------
Torque Step Input gives a back emf of some sorts to disrupt the output. This is done to check if the controller is responsive to any type of changes in the system.

![](Aspose.Words.6e40e44f-fce8-4c0f-92b9-9d70c4d112fe.008.png)

1. **Gain Blocks**

   ---------------
The gain block at the input brings the range of the input in between -1 to 1 which standardized the input. The second block simply brings it back to the normal value.

1. **Derivative & Saturation Blocks**

   ----------------------------------
This calculates the rate of change in speed error and limits it between -2 and 2.
**Fuzzy Controller Configuration-**

===================================
1. **Fuzzy Inference System (FIS):**

   ---------------------------------
![](Aspose.Words.6e40e44f-fce8-4c0f-92b9-9d70c4d112fe.009.png)

1. **Membership Functions of Inputs-**

   -----------------------------------
Membership functions characterize fuzziness (i.e., all the information in fuzzy sets), whether the elements in fuzzy sets are discrete or continuous. Membership functions can be defined as a technique to solve practical problems by experience rather than knowledge. Membership functions are represented by graphical forms. Rules for defining fuzziness are fuzzy too.

- **Error:**

It signifies the difference between input step response and the output speed. 

Value\_High and Value\_Low Member functions are trapezoidal while Value\_Med, Value\_Med\_High and Value\_Medium\_Low are triangular.

![](Aspose.Words.6e40e44f-fce8-4c0f-92b9-9d70c4d112fe.010.png)

- **Change:**


Derivative of error (i.e. change in error with respect to time).

Error\_High\_Negative and Error\_High\_Positive show drastic changes in the error as the step response changes. This is useful when I have to decide if I need to increase or decrease the output or hold it in one place.

![](Aspose.Words.6e40e44f-fce8-4c0f-92b9-9d70c4d112fe.011.png)

1. **Membership Functions of Output-**

   -----------------------------------
The Membership Functions here are all triangular. This determines what kind of output the fuzzy logic controller block will give in order to control the speed of DC Motor.

![](Aspose.Words.6e40e44f-fce8-4c0f-92b9-9d70c4d112fe.012.png)
# **Output-**
- ## **Error, Change in Error vs. Time**
![](Aspose.Words.6e40e44f-fce8-4c0f-92b9-9d70c4d112fe.013.png)
- ## **Fuzzy Controller Output vs. Time**
![](Aspose.Words.6e40e44f-fce8-4c0f-92b9-9d70c4d112fe.013.png)
- ## **Step Response and Speed of DC Motor**

**![](Aspose.Words.6e40e44f-fce8-4c0f-92b9-9d70c4d112fe.014.png)**


**We see the following trends in all the plots-**

1. We see an initial fluctuation when the controller first starts functioning and first stabilizes the speed to a constant.
1. We see the next change at 5 seconds, when the input step function changes from 800 to 1450 rpm, and so the controller then stabilizes it again.
1. We see a final change at 7 seconds, a minor fluctuation due to the torque step input, which is again quickly corrected by the controller. 
# **Conclusion-**
We have successfully obtained multiple plots-

1. Error in Speed vs. Time
1. Change in Error in Speed vs. Time
1. Fuzzy Controller Output Voltage vs. Time
1. Speed Response of the Model

From these I see that the Fuzzy Logic Controller is quite fast and robust. It is highly responsive and adaptive to any kind of change, such as the ones given in the input step and the torque step functions, immediately correcting the output voltage as necessary. 

Hence, I also see that this model manages to overcome the limitations of the traditional PI/PID controllers, which are lack of adaptability and flexibility to external disturbances, any non-linear characteristics in the system or any type of unmodelled changes.

Further possible future improvements-

1. Further variations and tuning of the Fuzzy Controller for even more efficiency.
1. Addition of a PID controller along with the Fuzzy Logic controller for increased accuracy and efficiency.
#
# **References-**
- Yasser Ali Almatheel and Ahmed Abdelrahman “Speed Control of DC Motor Using Fuzzy Logic Controller” 2017 International Conference on Communication, Control, Computing and Electronics Engineering (ICCCCEE), Khartoum, Sudan <https://www.researchgate.net/publication/314204285_Speed_control_of_DC_motor_using_Fuzzy_Logic_Controller>
- Umesh Kumar Bansal and Rakesh Narvey “Speed Control of DC Motor Using Fuzzy PID Controller ” Advance in Electronic and Electric Engineering. ISSN 2231-1297, Volume 3, Number 9 (2013), pp. 1209-1220 © Research India Publications <http://www.ripublication.com/aeee/070_pp%20%20%20%20%201209-1220.pdf?links=false>
- A. A. Sadiq, G. A. Bakare, E. C. Anene and H. B. Mamman “A Fuzzy-Based Speed Control of DC Motor Using Combined Armature Voltage and Field Current” 3rd IFAC International Conference on Intelligent Control and Automation Science September 2-4, 2013. Chengdu, China
  <https://www.researchgate.net/publication/269809012_A_Fuzzy-Based_Speed_Control_of_DC_Motor_Using_Combined_Armature_Voltage_and_Field_Current> 
- <https://in.mathworks.com/help/index.html> 
