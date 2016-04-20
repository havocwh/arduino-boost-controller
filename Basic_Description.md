Basic Description / Idea of Arduino Based Boost Controller...


Operation:
The goal is to control boost by operating the solenoid with a "Dutycycle", which is the ratio of  the time the solenoid is energized, to the time it is off.  100% Dutycycle means the solenoid is full on.  50% Dutycycle means the solenoid is being turned on and off with equal timing.  10% Dutycycle means that the solenoid is on briefly, then off for much longer.

The controller should function as follows:  During normal driving, the solenoid is not activated.  When the Throttle Position (and/or RPM) threshold(s) is/are exceeded, “Spool mode” is activated, and the solenoid is fully energized (100% dutycycle) which prevents any pressure from passing thru the solenoid and causes the turbo to spool as quickly as possible.  Once the boost pressure exceeds "PSI Start" the solenoid is operated at the "Start DC%".  If the boost pressure exceeds the desired target boost "PSI Target" the solenoid dutycycle is reduced at a rate determined by the "Gain" setting.  The solenoid dutycycle will only be increased to raise the boost if the throttle position exceeds "TPS Start." Alternatively, the “PSI Target” can set up to vary with “Throttle Position” (i.e. more throttle = more boost, such that 90-100% throttle = maximum boost setting).

Installation:
A GM 3 way solenoid is utilized to modify the pressure signal that regulates the boost pressure of a turbocharged engine.  By reducing the pressure signal that the wastegate receives, the boost pressure is increased until the reduced pressure is equal to the original unreduced pressure.  For example, if the wastegate spring is 10 psi (stock boost setting) with the wastegate receiving a pressure signal directly from the turbo/intake plumbing,  reducing the wastegate pressure signal by 50% will raise the boost pressure to 20 psi,  since the wastegate is still receiving its designed 10 psi.  Note: this example ignores many other factors that affect boost like pressure leaks, exhaust backpressure, wastegates that are improperly sized, turbos that are improperly sized, etc.

The solenoid has 3 "ports", 2 ports have hose nipples and are connected in-line with the pressure signal to the wastegate.  The third port should have a muffler/filter to keep contaminants out of the solenoid.  Connect the ports as follows:

Port 1: Silver plug, by itself on one end.
Port 2: Opposite port, next to port 3
Port 3: Comes with a small filter on it.

Port 1: To turbocharger, or optionally another line on the charge air side.
Port 2: To wastegate
Port 3: Back into intake, after the MAF (or vent to atmosphere

MAP sensor must be installed somewhere where it can see the manifold pressure (i.e. boost piping between turbo and intake manifold, post intercooler and other psi losses)

NOTE:  Carefully Secure all hose connections,  if a hose comes loose overboost will occur!

Setup:
The Arduino Controller can be programmed to control the boost pressure to a desired setting as long as a MAP sensor (manifold pressure sensor used to measure boost pressure) is connected to the unit.

1	TPS Spool:  Throttle sensor voltage to enable Boost Control.  Typically set at 1.25 - 2.5 volts.  Lower settings can improve spoolup time.  Higher settings can reduce part throttle surge.  It should be set higher than typical cruising TPS voltages.

1		TPS Spool: ALTERNATIVE… A look up table or function can be programmed to vary the boost setting proportionally to throttle position. A low set point triggers the solenoid to 100% duty cycle, when the desired set point is reached the controller reduces the solenoid duty cycle, if the throttle is depressed more (higher voltage) the boost setting increases proportionally and the duty cycle is adjusted accordingly

2	RPM Spool: Engine RPM to enable Boost Control.  Set higher than typical cruising RPM.
This is a set point that can be used if desired to prevent the solenoid from activating until a given RPM. If this is set high enough, only the wastegate spring will control boost and the bost pressure will not exceed that of stock.

3	TPS Start:  Once Boost control has started, TPS must exceed this for the system to increase solenoid duty cycle.  If it is set too low, the Controller will attempt to raise the boost pressure as the driver "backs out" of the throttle.  If the driver were to floor the throttle again a boost spike could occur.

3		TPS Start, ALTERNATIVE… Alternatively a look up table or function can be used to vary the boost with regard to throttle position

4	PSI Start:  Boost pressure at which the system changes from Spool mode (solenoid full on) to Start mode.  Set this half way between the base boost of the vehicle and the desired boost.  If boost overshoots are excessive, reduce this setting.  If boost overshoot is desired, set this closer to the target boost.

4		PSI Start, ALTERNATIVE… Again this setting will need to vary if boost by throttle position is desired

5	DC% Start:  The Solenoid dutycycle that is set when spool mode completes.  The approximate setting to start with is (1-(base boost/target boost))**100.  First tests should start with about half of this.  Example, base pressure = 10 psi,  target boost = 18 psi.  Estimated Dutycycle =(1-(10/18))** 100 =  44%.  So start at 22% for initial testing.

6	PSI Set:  Boost Pressure the system will try to maintain

6		PSI Set Alternative… (follow theme of previous alternatives)

7	PSI Aux:  Boost Pressure the system will try to maintain when the Aux Trigger input is activated. In this case a switch (or other input can be used, when not using boost by throttle position) to instantly change the boost setting i.e. a high boost switch, or low boost switch

8	Gain:  How fast the system will change the solenoid dutycycle to try to adjust the boost.  Set this only high enough to achieve good boost control.  Too much gain will cause the boost to surge/oscillate.  Setting the Gain to 0.0 will prevent the system from adjusting the solenoid to control boost.

The gain is part of a PID (or PD) loop that must be established to vary the solenoid duty cycle to achieve the desired boost set point.


INPUTS:
Map sensor = manifold pressure (boost pressure)
TPS = (Throttle position)
Switch – (for high boost / low boost switching, not applicable for boost by throttle position)
RPM - (if desired to be used, not applicable for my application)

OUTPUTS:
Duty Cycle to GM solenoid
Serial LCD display (if desired, why not add it…)


Notes:
•	GM 3 way solenoid is a normally open solenoid, so if no power is applied, it is open.
•	Solenoid must be driven by a PWM signal
•	Arduino is happy at 9v DC (FET 7809 may be used to regulate 12vdc in to 9v dc, w. 2 0.1 µF capacitors)
•	PD / PID control loop varies Duty Cycle w. regard to map sensor input to equal setpoint, setpoint is determined by throttle position
•	Serial LCD desired to display boost setpoint, map sensor current boost, (and TPS)

PARTS:
•	Arduino Microcontroller
•	Appropriate power supply (automotive 12v DC isn’t exactly clean and can vary beyond the aduino’s specified input voltages think 10-16 v DC)
•	GM 3 way solenoid GM Part # 1997152
•	Pigtail for GM solenoid GM Part # 12102747 (may be the same as other GM connectors)
•	Serial LCD display (if desired, can be configured to display TPS, target and or actual boost, use 4x20 backlit)
•	3 bar map sensor (Apexi AVC-R HKS EVC-S Greddy, or other) freescale MPX4250A 36 lb sensor

**Disclaimer** This is not based solely off my own work, I have selected and edited various information freely available online and compiled it, along with my own work, within this project.