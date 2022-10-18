# ESE-519-Lab-2A
ESE 519 LAB 2A

Akshaya Nidhi Bhati: University of Pennsylvania ESE 5190: Introduction to Embedded Systems, Lab 2A
LinkedIn: https://www.linkedin.com/in/akshaya-nidhi-bhati-6467841b3/?originalSubdomain=in

To DO: 

# Part 3: #

## PIO ##
 
 After reading chapter 3 in the Pico C SDK manual as given in resources of my setup guide https://github.com/AkshayaBhati/ESE-519-Lab-2-Setup-Guide/blob/main/README.md
 we can understand how the PIO Module interacts with a WS2812 module. 
 
 **Why is bit-banging impractical on your laptop, despite it having a
much faster processor than the RP2040?** <br>
Bit Banging is impractical for our laptop as the processor is not designed for it as it has CPU polling on GPIO pins which affects the real-time performance. Even if our laptop has a faster processor nowadays the layers between the processor and the outside world of hardware and software have also increased with time in size and number.

**What are some cases where directly using the GPIO might be a
better choice than using the PIO hardware?** <br>

 a. Accessing a pin for a one-time use or outside of a continuous loop. Going through the PIO would add latency.
   b. Low-priority tasks which can be pre-empted by higher priority ones can be bit-banged. 
   c. Designing a system to cut costs: FOr extremely low cost systems that do not require significant real time performance, bit banging could be preferred to 
      offset the investment in a dedicated hardware module. 
The cases where directly using the GPIO might be a better choice than using the PIO hardware includes Blinking of the LED. Then we need to toggle GPIOs via hardware interrupts which are less set of instructions than using PIO. PIO will be a better choice than using GPIO for complex tasks.
The cases where directly using the GPIO might be a better choice than using the PIO hardware includes Blinking of the LED. Then we need to toggle GPIOs via hardware interrupts which are less set of instructions than using PIO. PIO will be a better choice than using GPIO for complex tasks.

**How do you get data into a PIO state machine?** <br>
It is going to OSR from TX FIFO using Pull instruction 
The data is transmitted from FIFO to a PIO state machine. PULL instruction loads a 32-bit word from the TX FIFO into the OSR.

How do you get data out of a PIO state machine?

OUT instruction shift bit count bits out of the Output Shift Register (OSR), and write those bits to Destination. Additionally, increase the output shift count by Bit count, saturating at 32.


**How do you get data out of a PIO state machine?** <br>
**How do you program a PIO state machine?** <br>
In the PIO library the programs are included for UART, I2c ,etc that is for common interfaces so we don't have to write a program for these. The PIO has nine instructions in total mentioned below:
1.	JMP
2.	WAIT
3.	PUSH
4.	PULL
5.	IN
6.	OUT
7.	SET
8.	IRQ
9.	MOV

The textual format describing a PIO program is the PIO assembly. Here each command has one instruction in the output. 

**In the example, which low-level C SDK function is directly
responsible for telling the PIO to set the LED to a new color? How
is this function accessed from the main “application” code?** <br>
The low-level C SDK function that is directly responsible for telling the PIO to set the LED to a new color is pio_sm_put_blocking() function. As using this we can directly push data into state machine's TX FIFO as when the TX FIFO is full it stalls the processor this leds to LED being turned on and off when writing 1 and 0 respectively. This function is accessed form the main application code using put_pixel() function. As given in the example code we are calling put_pixel() function. 
By calling put_pixel() function
As given in the example code main 


**What role does the pioasm “assembler” play in the example, and
how does this interact with CMake?** <br>
Pioasm: it is the PIO assembler included in the SDK. The role it plays in the example is as follows: It processes an input text file of PIO assembly that might contain multiple programs and it writes out the ready-to-use assembled programs. These programs containing constant arrays are emitted in the form of C headers.  
It interacts with the CMake using the pico_generate_pio_header(TARGET PIO_FILE) function as it invokes pioasm and the header is also added in the include path of the target using this. Therefore, we do not have to invoke pioasm in the SDK directly.

LinkedIn: https://www.linkedin.com/in/sahil-m-39a2671b0
Tested on:  HP Probook 650 G1 (15.6-inch, 2014), Window 10

# Brief responses to the reading questions in 3.2:
1. Why is bit-banging impractical on your laptop, despite it having a much faster processor than the RP2040?

Ans: With increase in the speed of processors, the layers between hardware and software between the processor and the outside world have also increased in size and number.  Because of which, the processors keep hundreds of thousands of instructions on a single core at a time. Hence it’s difficult to switch rapidly between hard real time tasks.

Above certain speeds — say a factor of 1000 below the processor clock speed — IRQs become impractical, in part due to the timing uncertainty of actually entering an interrupt handler. It’s difficult to write in assembly, trying to make sure the GPIO reading and writing happens on the exact cycle required. your processor is now busy doing the "bit-banging", and cannot be used for other tasks. If your processor is interrupted even for a few microseconds to attend to one of the hard peripherals it is also responsible for, this can be fatal to the timing of any bit-banged protocol.


2. What are some cases where directly using the GPIO might be a  better choice than using the PIO hardware?

Ans: Simple task such as LED blinking is better done with GPIO than using pio where we’ll send data into state machine having more sets of instruction than what we need to toggle LED with GPIO. PIO suits better for performing complex tasks efficiently.


3. How do you get data into a PIO state machine? 

Ans: From TX FIFO into OSR using pull instruction.


4. How do you get data out of a PIO state machine?  

Ans: From ISR into RX FIFO out instruction.


5. How do you program a PIO state machine? 

Ans: PIO state machines execute short, binary programs. The PIO has a total of nine instructions: JMP, WAIT, IN, OUT, PUSH, PULL, MOV, IRQ, and SET. We write program using assembly language.


6. In the example, which low-level C SDK function is directly responsible for telling the PIO to set the LED to a new color? How is this function accessed from the main “application” code? 

Ans: Pio_sm_put_blocking . Calling put_pixel function. 


7. What role does the pioasm “assembler” play in the example, and how does this interact with CMake? 

Ans: The PIO assembler is included with the SDK, and is called pioasm. This program processes a PIO assembly input text file, which may contain multiple programs, and writes out the assembled programs ready for use. For the SDK these assembled programs are emitted in form of C headers, containing constant arrays. the CMake function pico_generate_pio_header(TARGET PIO_FILE) takes care of invoking pioasm and adding the generated header to the include path of the target TARGET for you.


# 3.4: Spreadsheet of initial PIO register states

https://docs.google.com/spreadsheets/d/18KsNoGRULzMHCPo81uQk1wKcW_Gdkq5YdlxhDTKC2yk/edit?usp=sharing

1.	Which PIO instance is being used? 

Ans: Pio0

2.	Which state machine is being used with this PIO instance?

Ans: SM0

3.	Which pin is this state machine configured to control? (you can  either use settings from the example program, or for the Qt Py  LED pin yours will be connected to)  

Ans: Default pin of mcu or pin 2 of mcu

4.	How long is this state machine’s clock cycle? 

Ans: 8MHz

5.	How much is this state machine’s clock scaled down relative to the system clock? (i.e. the “clock divisor”)  

Ans: 125M/(800k*10)=15.625

6.	In which direction will this state machine shift bits out of its  “output shift register”?

Ans: LSB first (right shift)

# 3.5 State machine:

.wrap_target                                          
bitloop: 			            
 	out x, 1 side 0 [T3 - 1] ; 	             L1   Shift 1 bit from OSR into x and set side set pio pin low and wait for T3-1 cycles
	 jmp !x do_zero side 1 [T1 - 1] ;        L2 Jump to do_zero loop if x is zero and set side set pio pin high and wait for T1-1 cycles
[10:54 PM, 10/17/2022] Sahil Mangaonkar Embedded: do_one:                                                    
 	jmp bitloop side 1 [T2 - 1] ;	           L3 Jump to bitloop if x was 1 and set side set pio pin high and wait for T2-1 cycles 
 do_zero:                                                   
 	nop side 0 [T2 - 1] ; 		               L4 No Operation if x was 0 and set side set pio pin low and wait for T2-1 cycles
.wrap				

1.	What basic circuitry does a WS2812 LED need to operate? 

Ans: Supply (GND and VDD=5v), data pin, a resistor of value 150, a capacitor.

2.	How do you connect a WS2812 to a microcontroller? 

Ans: Connect GND of microcontroller to GND of WS2812, a gpio pin of microcontroller controlled using PIO to data in (DI) pin of ws2812.

3.	How does a WS2812 translate bits to color values? 

Ans: Bits are sent sequentially to the module to turn on and off the led so fast that we can see a color generated as we wanted.

4.	How do you send a single 1 or 0 bit to the WS2812? 

Ans: Make DI pin high or low using data pin of microcontroller.

5.	How many bits does it take to send a single color value? 

Ans: 8

6.	What happens if you send more bits than this in a packet? 

Ans: It will take data as it comes and will not give correct color.

7.	How do you tell a WS2812 you’re done sending data? 

Ans: Out instruction waits for data and keep side pin low.

8.	How do you send data to more than one WS2812 in a chain?

Ans: Connect DO of 1 module to DI of other and so on.



# At the end of your writeup of section 3, reflect on the tools you used for modeling from a user interface design perspective.


o What were some strengths/weaknesses of working with paper?

strengths: Flexibility in representation	

weaknesses: time consuming and making changes is difficult


o What were some strengths/weaknesses of working with spreadsheets?

strengths: much cleaner than paper model and saves time		

weaknesses: Regid way of representation (only tabular)


o How might you approach this task using other tools available to you?

Flowchart can also be used to show the flow of the code

Include lab questions, screenshots, analysis, etc. (Remember, this is public, so don't put anything here you don't want to share with the world.)




Part 3.4 

Spreadsheet :
https://docs.google.com/spreadsheets/d/13l0bCfyE5TmSJbdV2kjk0inrah82hIrh6xzRkMsBtd8/edit?usp=sharing
