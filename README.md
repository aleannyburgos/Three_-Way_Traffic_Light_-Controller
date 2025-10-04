# Three-Way Traffic Light Controller
The purpose of this project was to implement a three-way traffic light configuration using FSM design.

## Task
There is a three-way intersection that has a traffic light the North, South and east side. For all the traffic lights, there is red yellow and green lights. Additionally, for the cars heading east, there is another light specifically for cars turning right and for the cars heading north there is a green light for cars turning left. There are three different times intervals:

  1) m = 20 secs, for when the lights are EG, NR and SR
  2) d = 10 secs, for the yellow lights
  3) M = 60 secs, for all the other stages

There are also two sensors for detect if there are cars in their respective lanes. The first is the E sensor for the cars coming from west to east and would like to make a right turn. The other is the L sensor that detects the cars coming from south to north and would like to make a left turn.


The task here is to design a finite state machine to transition from the different states in a circular motion.

<img width="2021" height="2031" alt="Trafficlightdiagram" src="https://github.com/user-attachments/assets/561738b3-c5fa-4498-be7b-3321c159ae99" />


## How Its Made
### General Design
The design chosen for this task is a **five state moore based FSM** as shown below. The **inputs** are:

  1) Present state represented by 3 bits
  3) The E and L car sensors
  4) The three time signals: d,m and M

The **outputs** for the design are:
  1) Next state represented by 3 bits
  2) The light signals represented from MSB to LSB: EG,EY,ER,RG,NG,NY,NR,LG,SR,SY and SG
  3) The load signals, Sd,Sm and SM as well as a Sel signal

Below are the load signals for the time intervals m,M and d.
| Load signals for Sd/Sm/SM  | Binary representation |
| ------------- | ------------- |
| Sm  | 00  |
| Sd  | 01  |
| Not used  | 10  |
| SM  | 11  |

Below are the lights that are on at each of the traffic lights depending on the state.
| State  | Binary representation | Light configuration at state |
| ------------- | ------------- | ------------- |
|S0 |	000 | EG, NR, SR |
|S1 |	001 | EY, NR, SR | 
|S2 |	010 | ER, NG, SG |
|S3 |	011 |	ER, SR, LG, NG, RG |
|S4 |	100 |	ER, NY, SY |

Below is the final FSM for the design. The inital state is "000" and the EG,NR and SR lights are high. The timer is counting down the from m (20 seconds) and will then move on to the next state which is 001 once m goes high. This also loads in the d signal and will stay in that state until the timer runs out and the design continues from state to state coming back to 000 and restarting again.

<img width="1409" height="845" alt="Drawing 1" src="https://github.com/user-attachments/assets/2f99e175-34dd-49aa-9bd0-665697a86504" />

Below is a rough design of the hardware involved for this design. It involves a ROM to go from state to state depending on the inputs and and producing the outputs. There is also a state register to hold the next state values and enabling clock synchronization. The E and L sensors are also synchronized with the clock and fed to the ROM. There is another registor that holds the values of the m, M, and d signals to be loaded into the countdown timer when sel is high.

<img width="8150" height="6153" alt="Drawing 3" src="https://github.com/user-attachments/assets/fcca47c3-9235-4779-ac1a-0028dbb98247" />
### VHDL Implementation
