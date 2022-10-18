# ESE-519-Lab-2A
ESE 519 LAB 2A


To DO: 

# Part 3: #

## PIO ##
 
 After reading chapter 3 in the Pico C SDK manual as given in resources of my setup guide https://github.com/AkshayaBhati/ESE-519-Lab-2-Setup-Guide/blob/main/README.md
 we can understand how the PIO Module interacts with a WS2812 module. 
 
 **Why is bit-banging impractical on your laptop, despite it having a
much faster processor than the RP2040?** <br>
Bit Banging is impractical for our laptop as the processor is not designed for it. Even if our laptop has a faster processor nowadays the layers between the processor and the outside world of hardware and software have also increased with time in size and number.

Bit-Banging" (i.e. using the processor to hammer out the protocol via the GPIOs) is very hard. The processor isn’t really designed for this. It has other work to do… for slower protocols you might be able to use an IRQ to wake up the processor from what it was doing fast enough (though latency here is a concern) to send the next bit(s). Indeed back in the early days of PC sound it was not uncommon to set a hardware timer interrupt at 11kHz and write out one 8-bit PCM sample every interrupt for some rather primitive sounding audio! Doing that on a PC nowadays is laughed at, even though they are many order of magnitudes faster than they were back then. As processors have become faster in terms of overwhelming number-crunching brute force, the layers of software and hardware between the processor and the outside world have also grown in number and size. In response to the growing distance between processors and memory, PC-class processors keep many hundreds of instructions in-flight Raspberry Pi Pico C/C++ SDK 3.1. What is Programmable I/O (PIO)? 30 on a single core at once, which has drawbacks when trying to switch rapidly between hard real time tasks. However, IRQ-based bitbanging can be an effective strategy on simpler embedded systems. Above certain speeds — say a factor of 1000 below the processor clock speed — IRQs become impractical, in part due to the timing uncertainty of actually entering an interrupt handler. The alternative when "bit-banging" is to sit the processor in a carefully timed loop, often painstakingly written in assembly, trying to make sure the GPIO reading and writing happens on the exact cycle required. This is really really hard work if indeed possible at all. Many heroic hours and likely thousands of GitHub repositories are dedicated to the task of doing such things (a large proportion of them for LED strings). Additionally of course, your processor is now busy doing the "bit-banging", and cannot be used for other tasks. If your processor is interrupted even for a few microseconds to attend to one of the hard peripherals it is also responsible for, this can be fatal to the timing of any bit-banged protocol. The greater the ratio between protocol speed and processor speed, the more cycles your processor will spend uselessly idling in between GPIO accesses. Whilst it is eminently possible to drive a 115200 baud UART output using only software, this has a cost of >10,000 cycles per byte if the processor is running at 133MHz, which may be poor investment of those cycles. Whilst dealing with something like an LED string is possible using "bit-banging", once your hardware protocol gets faster to the point that it is of similar order of magnitude to your system clock speed, there is really not much you can hope to do. The main case where software GPIO access is the best choice is LEDs and push buttons. Therefore you’re back to custom hardware for the protocols you know up front you are going to want (or more accurately, the chip designer thinks you might need).


**What are some cases where directly using the GPIO might be a
better choice than using the PIO hardware?** <br>
The cases where directly using the GPIO might be a better choice than using the PIO hardware includes Blinking of the LED. Then we need to toggle GPIOs via hardware interrupts which are less set of instructions than using PIO. PIO will be a better choice than using GPIO for complex tasks.

**How do you get data into a PIO state machine?** <br>
The four state machines execute from a shared instruction memory. System software loads programs into this memory, configures the state machines and IO mapping, and then sets the state machines running. PIO programs come from various sources: assembled directly by the user, drawn from the PIO library, or generated programmatically by user software. From this point on, state machines are generally autonomous, and system software interacts through DMA, interrupts and control registers, as with other peripherals on RP2040. For more complex interfaces, PIO provides a small but flexible set of primitives which allow system software to be more hands-on with state machine control flow. Figure 39. State machine overview. Data flows in and out through a pair of FIFOs. The state machine executes a program which transfers data between these FIFOs, a set of internal registers, and the pins. The clock divider can reduce the state machine’s execution speed by a constant factor.
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


**What role does the pioasm “assembler” play in the example, and
how does this interact with CMake?** <br>


Part 3.4 

Spreadsheet :
https://docs.google.com/spreadsheets/d/13l0bCfyE5TmSJbdV2kjk0inrah82hIrh6xzRkMsBtd8/edit?usp=sharing
