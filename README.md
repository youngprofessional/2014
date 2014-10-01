Lufthansa Systems Hungária Kft. - Test for Software Developers
======================================

Please create a complete software package which fulfils the specification below. In case you would like to pursue a career in software testing, then you should create a comprehensive test plan for this software module.

In order to facilitate the understanding, you should include code comments, software documentation, build scripts and test cases as you feel appropriate.

You can choose from the following languages: Java, Javascript, C/C++, Scala. You may use any mainstream framework / library for the implementation, only requirement is that the code compiles and runs according to the specification.‎

Implementations which hard-codes the order of calculations and checks won't be accepted as a valid solution.

You will have 1 week to provide a solution. You might submit partial solutions in case you think it can be evaluated.

Specification
-------------

### Weight and Trim calculation 

Airlines need to calculate various physical parameters before flight. The aim for the weight and trim calculator is to provide an extensible software module which can aid the calculations and checks.

Calculations may depend on intermediate results, eg. Dry-Operation-Weight (DOW) is the weight of the plane without fuel and payload. The calculation of the DOW reads the Basic-Weight (STARTW) of the aircraft (each aircraft is measured individually, they vary in weight) and adds the weight of water and galley (eg. catering equipments). In case another calculation needs the DOW then it will have to be deferred until the value is available. In the above case DOW calculation will take place after the STARTW is available.

Airlines and manufacturers can define limits and other checks to any intermediate results. Checks might result in a warning or an error. For example, in case the underload is too low, then a warning will inform the load controller about the possibly over-utilized aircraft. Another example is aircraft type related checks for Take off Weight (TOW) and Landing Weight (LAW) checks. In case the aircraft is too heavy at TOW then it cannot take off or in case of LAW the gears may be damaged during landing. They are errors and the calculation must halt.
 
### List of calculations needed:

Calculation: *LAW (Landing Weight)*   
Function: *TOW - flight.tripFuelWeight*

Calculation: *DOW (Dry Operation Weight)*    
Function: *aircraft.STARTW + flight.waterWeight + flight.galleyWeight*

Calculation: *ZFW (Zero Fuel Weight)*      
Function: *DOW + flight.paxWeight + flight.payloadWeight*

Calculation: *TOW (Take off Weight)*      
Function: *ZFW + flight.tripFuelWeight + (flight.taxiFuelWeight / 2)*

Calculation: *Underload (Amount of free capactity)*      
Function: *Min( aircrafttype.maxLAW - LAW, aircrafttype.maxTOW - TOW, aircrafttype.maxZFW - ZFW, aircrafttype.maxTXW - TXW )*

Calculation: *TXW (Taxi Weight)*      
Function: *ZFW + flight.tripFuelWeight + flight.taxiFuelWeight*

### List of checks needed:

Check: *ZFW check*  
Assert Function: *ZFW < aircrafttype.maxZFW*  
Type: *ERROR - Too much zero fuel weight*  

Check: *TXW check*  
Assert Function: *TXW < aircrafttype.maxTXW*  
Type: *ERROR - Too much taxi / ramp weight*  

Check: *TOW check*  
Assert Function: *TOW < aircrafttype.maxTOW*  
Type: *ERROR - Too much take-off weight*  

Check: *LAW check*  
Assert Function: *LAW < aircrafttype.maxLAW*  
Type: *ERROR - Too much landing weight*  

Check: *Fuel check*  
Assert Function: *flight.tripFuelWeight + flight.taxiFuelWeight < aircrafttype.maxFuel*  
Type: *ERROR - Too much fuel*  

Check: *Underload*  
Assert Function: *Underload > airline.minUnderload*  
Type: *WARN - Flight is nearly full!*  
 

### Runtime

The calculator need to calculate all values, while checks must be executed at the first possible time. In case of errors the calculation must halt. You may add other intermediate calculations, if needed.

**Input of the program:**
Masterdata and Flight data file provided.

**Output of the program:**
The program must print the calculated results for each flight in the input data to the screen. 
After listing all calculated values the program should list the messages (warn / error) if any.
‎


**Example:**   
LX179 - SIN-ZRH   
DOW 234555   
TOW 370033   
ERROR: Too much take-off weight.    
(please note that the above example is not the expected output of the example input file.)
