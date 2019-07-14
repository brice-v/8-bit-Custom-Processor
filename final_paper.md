![https://lh3.googleusercontent.com/uMwXMt\_0hCkoQl1Fk9n-bvLGKnDq6OHEozY8Vp5YOE2GY3Nyhw5F2nTJkysBCD6LmaazhxwV55OdClt-XCGAiDiPlhR2yY0PTsNCnfimFSS2AC41OFTPz-fXUr25UuWF6goFf3\_k](media/image1.png){width="4.177083333333333in"
height="0.8541666666666666in"}

Microprocessor System Design, Simulation, and Implementation

By

Brice Vadnais

*B.S. Computer Engineering*

And

Aristotle Nafpliotis

*B.S. Computer Engineering*

Presented to the Faculty of the College of Engineering,

University of Hartford

As a partial requirement for the Degree of Bachelors of Science in
Engineering

May 14^th^ 2018

Acknowledgements
================

We would like to thank our Professor and Technical Advisor, Ph.D. Krista
Hill, for all of her support and encouragement throughout the entire
project. Without her and her previous course lessons nothing that was
done during this project would have been possible. In the past year we
have both learned so much about the process of designing a project from
the ground up and what it means to be an engineer.

We would also like to thank the University of Hartford Engineering
Department for providing labs and development boards for the testing
that was done throughout the entire project design phase.

Finally, we would like to thank all of the faculty and students who
encouraged the development of the project and came to listen in on the
final presentation.

List of Figures
===============

[Figure 1. Structure of Instruction Set Simulator 5](#_Toc514031701)

[Figure 2. Structure of Registers and Memory in Simulator
5](#_Toc514031702)

[Figure 3. Dictionary Built for Look up of Instruction Functions during
Assembly 6](#_Toc514031703)

[Figure 4. Basic Assembly Program with Syntax 6](#_Toc514031704)

[Figure 5. Basic Computer System Overview Block Diagram
7](#_Toc514031705)

[Figure 6. Von Neuman Computer Architecture Block Diagram
8](#_Toc514031706)

[Figure 7. S-Record Format Information \[7\] 12](#_Toc514031707)

[Figure 8. Initial Gantt Chart Project Plan 13](#_Toc514031708)

[Figure 9. Data Path Block Diagram 18](#_Toc514031709)

[Figure 10. Final Schematic for Data Path 19](#_Toc514031710)

[Figure 11. VHDL 8-bit General Purpose Register 20](#_Toc514031711)

[Figure 12. VHDL din1 and din2 Registers 20](#_Toc514031712)

[Figure 13. 16-bit General Purpose Register 21](#_Toc514031713)

[Figure 14. PC Register VHDL 22](#_Toc514031714)

[Figure 15. ALU VHDL Code 23](#_Toc514031715)

[Figure 16. ALU MUX A VHDL Code 24](#_Toc514031716)

[Figure 17. ALU MUX B VHDL Code 25](#_Toc514031717)

[Figure 18. DoutMux VHDL Code 26](#_Toc514031718)

[Figure 19. ADX MUX VHDL Code 26](#_Toc514031719)

[Figure 20. Hold Low Register VHDL Code 27](#_Toc514031720)

[Figure 21. Borrow or Carry Out VHDL Code 28](#_Toc514031721)

[Figure 22. adx\_en VHDL Code 29](#_Toc514031722)

[Figure 23. ROM VHDL Code 30](#_Toc514031723)

[Figure 24. RAM VHDL Code 31](#_Toc514031724)

[Figure 25. Assembly Language Test Code 36](#_Toc514031725)

[Figure 26. Compiled S19 Assembly File 36](#_Toc514031726)

[Figure 27. Compiled Assembly Loaded into ROM LUT 37](#_Toc514031727)

[Figure 28. Signals Checked after each Simulation Increment
40](#_Toc514031728)

[Figure 29. Load A Immediate Simulation 41](#_Toc514031729)

List of Tables
==============

[Table 1. Design Elements (not simulation elements) 15](#_Toc514031689)

[Table 2. Registers Seen by User 15](#_Toc514031690)

[Table 3. Encoding of First 4-bits and Associated Addressing Mode
17](#_Toc514031691)

[Table 4. Operations Supported by ALU 22](#_Toc514031692)

[Table 5. Addressable Memory Addresses 29](#_Toc514031693)

[Table 6. Load Enables and Device 32](#_Toc514031694)

[Table 7. Din and CCR Load Enable Outputs 33](#_Toc514031695)

[Table 8. ALU MUX A Control Signal I/O 33](#_Toc514031696)

[Table 9. ALU MUX B Control Signal I/O 33](#_Toc514031697)

[Table 10. Dout MUX Control Signal I/O 34](#_Toc514031698)

[Table 11. ADX Select Control Signal I/O 34](#_Toc514031699)

[Table 12. ALU Control Signal I/O 34](#_Toc514031700)

Executive Summary
=================

This report covers extensively the design of a custom 8-bit
microprocessor system with a 16-bit memory bus. Initially the project
starts off with the explanation of the basics involved in how a
processor operates and then gradually reveals more and more to what
makes the processor unique in the space of all other embedded
processors. The instruction set architecture describes what is possible
and capable on the processor. The internal modules carry out this
architecture description and through careful description no errors are
made and the processor can execute code directly from a custom assembly
level language. All source code for the processor is written in VHDL and
will be released under an open permissive MIT license for future
computer engineers and enthusiasts to make improvements as seen fit.
This document represents the work of a full semester on a single
project. This device is an excellent tool in learning the possibilities
of VHDL microprocessors and can be used for a number of specific
targeted use cases. The report also covers the testing methodology that
allows for such a project to be created. By the end of the report one
should be able to replicate this processor with little to no external
help.

Chapter 1: Project Overview
===========================

Outline, Background, and Overall Objective
------------------------------------------

Microprocessors are the brains of all computer systems. They tell the
rest of the computer what to do when certain functions are being called
or what needs to put into memory for later use. Microprocessors are now
used in nearly every electronic device as a way of making these devices
“intelligent”. Some microprocessors are built as a unit to run a
personal computer where their tasks mainly focus on the daily use from a
user. Other microprocessors are built smaller to be used as the
processor for a cell phone and some are built with the functionality of
a simple way of interfacing with a variety of other devices to allow
them to work together easily.

The way that microprocessors can and are built are very different. For
the complex ones found in the personal computers of most people the
design is limited by the manufacturing process. This process allows for
smaller and smaller complex chip designs to be possible. However, the
design for this microprocessor will not be built with any fancy
manufacturing techniques but rather by using an FPGA. An FPGA is a Field
Programmable Gate Array and its uses are countless. An FPGA can be used
to program and build complex circuits while abstracting a lot of the
complex detail. This makes an FPGA a great way to build a complex
circuit such as a microprocessor. FPGAs are also limited by the number
of gates that they are able to construct on one chip however so this
does limit what the most complex design will be.

The objective of this project is to design a microprocessor from the
ground up, then simulate it thoroughly using the software available, and
finally implement the entire design onto an FPGA. The design portion of
the project will primarily be done in a program called Xilinx which is
used as a means to simulate and program digital circuits. In addition,
some of the design will be done on paper or through simulations in other
programs, such as the instruction set, because there needs to be grounds
on which all the other digital logic is designed. The implementation is
straightforward because after all the circuit is designed in Xilinx, it
will be flashed to the FPGA allowing for testing to be performed on the
microprocessor.

### Impact

The main use for small microprocessors are their ease of use with
peripheral devices. This project will aim for the same ease of use while
also being easy to configure for a variety of devices. Microprocessors
are constantly becoming more useful in the world because of the devices
that we design. This is also an extremely useful learning tool for
seeing what goes into the entire design of a microprocessor. For the
user, it will provide a way to load custom programs onto the
microprocessor and carry out a variety of simple tasks. In addition,
because of the design of the microprocessor, it will be fairly simple to
load the processor onto another FPGA to be used for another application.

### Context

The microprocessor that is being deigned will be similar to the Arduino
microcontroller. The Arduino can be used in a variety of applications
that use peripheral devices while still being simple in implementation.
The microprocessor we are designing will have some of this functionality
while also being better for implementation by the user. One way that
these devices are similar will be the way that the user interacts with a
peripheral device, such as a temperature sensor. In contrast, the
devices will also be different in the way that the user loads the
program, which interacts with the sensor, onto the device. The design of
our microprocessor will also serve as a tool for learning the complexity
that goes into designing a microprocessor-based system from the ground
up.

Detailed Use Case
-----------------

This project’s primary use case is as a versatile general-purpose
microprocessor built from the ground up. The primary users are us, as we
are the ones to benefit from its design and the process of building the
microprocessor. The microprocessor allows us to write programs and load
them onto the processor through a bootloader. In addition, this
functionality can also be used in conjunction with interfacing with
peripherals. The peripheral devices that are able to be used need to use
a serial protocol that we have implemented. It is then fairly simple to
use the processor in embedded type applications and as a data
acquisition tool.

Another user for out microprocessor are engineering teams that want to
test and configure their own peripheral devices. In this case all that
needs to be done are some simple programs written that will interact
over the serial protocol to the engineer’s peripheral device. This is
beneficial because a microprocessor implemented on an FPGA has a
significant amount of expandability and modularity. This means that if
the specific device needed more input and output, it could just be added
to the microprocessors specifications. Also, if more memory or less
memory was needed to perform a certain task the engineers could modify
the code on the board to fit their specific needs.

The last users this device targets are computer and electronics
hobbyists. Electronic hobbyists would like this system because it
provides an easy to use real time system that can interact with the
electronic sensors and peripherals that are used in many hobbyists’
projects. Computer hobbyists would like the system because of its
uniqueness in the space of microcontrollers, microprocessors, and single
board computers. This device itself would be fun to work with and modify
for a variety of uses or just to see what the maximum potential is of an
FPGA microprocessor.

Chapter 2: Previous Work
========================

Some of the classes that are required for the computer engineering
degree taught us significantly about the uses and possibilities of
microprocessors. In fact, one such class introduced the students to a
microprocessor that was also built and implemented onto an FPGA.
Learning about this processor made the idea of building a processor in a
year, less fantasy and more reasonable. Although there are still limits
to what is possible in such a time frame. This microprocessor, for
example, is still able to do many simple programs such as multiplying,
searching, and storing arrays. Its use for many floating-point
operations are basically nonexistent.

Inspiration
-----------

The main inspiration for this project comes from the nod4 project in the
Computer Architecture Course as well as all the things that were learned
in the Microprocessor Applications and Computer Systems Laboratory
courses. In addition, as computer engineers, building a custom
microprocessor has been a passion project that could only fully be
realized in the senior capstone project class due to the complexity and
length of the project.

Nod4 is a microprocessor that was developed by Ph.D. Krista Hill. It is
a simple yet non-trivial microprocessor intended for teaching the basics
of computer architecture. This processor is 8-bits and computes all data
with only 8-bits. The memory system is also 8-bits so the addressable
space by the user is 256 8-bit values. The processor is described as
being simple yet non-trivial for many different reasons. It is simple
because there are enough instructions to do basic functions and learn
the beginning of assembly, and also because the 8-bits makes encoding
and decoding instructions easier to the user. In addition, it is simple
just due to the fact that there are not a lot of registers or complex
architecture to deal with from a user’s perspective. It is non-trivial
for a variety of other reasons such as its many useful addressing modes
and beneficial architectural features such as a stack and an indexed
register.

This entire project became a huge inspiration to the current project.
One reason for this was just because it was one of the best explained
introductions to computer architecture. By keeping the architecture in a
similar fashion to nod4 it was much easier to understand and expand the
use ability of our microprocessors. An example of this is the load-store
based architecture. This in itself is something critical to the design
of the microprocessor. By using this it only improved what can be done
with the processor and now, with a much stronger understanding of
computer architecture there are many more designs that could be made for
a microprocessor.

In addition to the nod4 project, learning about microprocessor
applications and computer systems made the process of learning and
completing this project much easier. In microprocessor applications,
different computer architectures were learned and the class focused on a
specific microprocessor. Using this knowledge only enhanced what would
be possible when putting everything together for the final project. In
addition, in computer systems, there was a microprocessor that was built
and simulated piece by piece to learn about microprocessors and finally
it was used with a device to interact with peripheral serial devices.
This in and of itself is where the concept for a peripheral interfacing
microprocessor system came from. The procedure for simulating the
computer systems laboratory processor is similar to what is used for our
processor system.

Proof of Concept
----------------

Before any components would be built we had to first make sure that the
project was feasible. This was done by researching other projects that
were similar and could prove that the idea was possible. For example,
one of the first places that we looked into was the microprocessor that
was built in one of our previous classes. This would give us a nice
place to start by looking into the way that processors are designed.
Also, this design was implemented onto an FPGA so already at this stage
in the project we were well aware that what we were designing and
building was possible because it had been done before.

To start with more research on the project, it was important to test
some of the fundamental principles of the microprocessor to ensure the
likelihood that it would work and be a functional project. One way in
which this was done, was by simulating the instruction set that was
designed for the microprocessor. This simulation was done in Python, a
high-level programming language, which allowed for highly modular code
to be designed that would present the instruction set in a way that
would allow us to ensure that all the necessary functionality was there.
Also, by designing the instruction set in a high-level language, it was
easy to add on new instructions as necessary as well as remove others
that were not. This process allowed our instruction set to only include
the essentials that would make up the microprocessor’s architecture.

The instruction set simulator was designed as follows. All of the
instructions were designed as a function, within Python, which took one
parameter which was the operand (see fig. 1).

![](media/image2.png){width="3.4270833333333335in"
height="2.1477602799650044in"}

[]{#_Toc514031701 .anchor}Figure . Structure of Instruction Set
Simulator

An empty matrix was used to simulate the program memory, which could be
read or written to according to the instruction given, and other
variable names took the place for each of the necessary registers (see
fig. 2).

![](media/image3.png){width="3.3854166666666665in"
height="2.1354166666666665in"}

[]{#_Toc514031702 .anchor}Figure . Structure of Registers and Memory in
Simulator

Then all the instructions were put into a dictionary which would allow
the program running on the simulated instruction set to parse out the
function of the instruction and carry out its intended purpose.

![https://lh5.googleusercontent.com/Tx0OX\_nvtnYU\_cWXRuP1JQYxNuWcZtN0g4JV-DGEUQ9qRoZd8Q-2tGHMo3LJhjp5FHnZW-om\_KArfcXvH36Ef82JqwyPM3O4531wmDG7MDPi9YfYaJZx00SyGVsl1qnXMxFy-rIf](media/image4.png){width="5.625in"
height="2.4479166666666665in"}

[]{#_Toc514031703 .anchor}Figure . Dictionary Built for Look up of
Instruction Functions during Assembly

After all of these core principles were built for the simulator. A basic
assembler was written so that assembly could be written in a particular
syntax that would allow the simulator to operate as if it were given the
individual instructions.

![](media/image5.png){width="4.114583333333333in" height="2.03125in"}

[]{#_Toc514031704 .anchor}Figure . Basic Assembly Program with Syntax

Instructions from the assembly were parsed and then placed into memory
along with any data for the operand and with Python, functions were able
to be executed after calling them from a dictionary. This is what made
Python programming so effective in simulating the instruction set.

One conclusion we drew after designing the instruction set simulator, is
that we would not likely need any branch instructions because we were
going to use jumps instead. This would allow for simpler implementation
and would allow the instructions themselves to be unique in purpose. In
general, the instruction set simulator proved to be an effective ‘quick
and dirty’ approach to a prototype that would benefit the design of our
microprocessor.

Another conclusion that was drawn, was the ability of the simulator to
become more than just an instruction set simulator. In this case many of
the functions of an actual microprocessor could be programmed so that
the simulator would behave as such. For example, rather than using the
actual names of the instructions in the assembler, there could be
encoding done on each of the functions that would actually represent the
correct instruction. Seeing as this was not the goal of the simulator,
we only made it to simulate instructions and further the design of the
microprocessor.

Chapter 3: Engineering Specifications and Design Constraints
============================================================

The specifications are what set microprocessors apart from each other.
Each processor is designed and built for a specific use and our use case
is as a versatile general-purpose microprocessor. The design for this
microprocessor is done from the ground up. This means that all the
critical components for the processor will be designed and implemented
by us. Using the detailed use case as well as the things learned in the
proof of concept instruction set simulator, it is possible to choose the
correct specifications that the microprocessor system should adhere to.

Fundamental Principles
----------------------

In any microprocessor system there are three main components that make
the processor a full system. A controller, a memory system, and a data
path are the primary components that are used in our microprocessor. The
controller is what sends all the necessary control signals to the memory
system and data path in order for certain operations to be performed.
The data path is where the movement of data happens within the processor
and also where all the computers registers and data is located. The
memory system contains all of the instructions that are supposed to be
used as well as the data that is being manipulated during operation. A
simple block diagram of this computer system is shown below (see fig.
5).

![C:\\Users\\Brice\\Documents\\Capstone\\hardware\_sys\_overview.jpg](media/image6.jpeg){width="4.166666666666667in"
height="2.4830577427821523in"}

[]{#_Toc514031705 .anchor}Figure . Basic Computer System Overview Block
Diagram

These principles were first originally defined by John von Neuman. He
described an architecture that had a control unit, an arithmetic logic
unit, a memory unit, and input output. These are all modules that are
contained within the processor and the design fully follows this Von
Neuman architecture. Below is an image to show the basic idea and how
similar it is to our processors architecture.

![https://upload.wikimedia.org/wikipedia/commons/thumb/e/e5/Von\_Neumann\_Architecture.svg/510px-Von\_Neumann\_Architecture.svg.png](media/image7.png){width="4.197916666666667in"
height="2.428206474190726in"}

[]{#_Toc514031706 .anchor}Figure . Von Neuman Computer Architecture
Block Diagram

Architectural Decisions
-----------------------

Architectural decisions are the design choices made for our project that
will impact the way it operates compared to other processor designs. The
critical architectural decisions are shown below along with their impact
to the overall design of the microprocessor system.

### *8-bit Accumulator-Based Microprocessor*

Using an accumulator-based microprocessor has a big effect on what the
processor is capable of and so it is a significant architectural
decision.  The microprocessor uses 8 bits for each of its two general
purpose accumulators and most of the data registers within the data
path.  This makes it a much more capable processor than that of a 4-bit
processor.  To our project, this means that the processor has the
ability to do more.  This also means that the processor has a more
complex design. This complex design has the potential to use more space
for implementation.  Another effect of using an 8-bit accumulator design
is the ease of use for some designated processes.  The accumulators take
the decision of deciding what register to use out of the programmer’s
hands and instead just forces the operators to all perform on the
accumulator register itself. In this circumstance it is easier to use an
8-bit processor for some use cases, rather than a complex
microcontroller or microprocessor.

### *Load/Store Architecture*

A load store architecture is pretty self-explanatory to what it actually
means with respect to computer architecture. Basically, the load is when
data is being written to a register within the data path and store is
when the data is being written to somewhere else, such as the memory
system. This is an architectural decision because there are many other
ways of moving data around within a processor, such as moves. The reason
that load and store were chosen is primarily because of our familiarity
with the architecture and also because it provides a simple way to
interact with the two general purpose accumulators.

### *16-bit Memory System*

The decision was made to use 16-bits of addressable memory for a couple
of reasons. One of these reasons is simply that more addressable space
leaves more room for programs to run. Also, if the memory system only
used 8-bits it would not be able to store the more complex programs that
would need to be run by the processor such as the bootloader. Another
reason for using 16-bits is increasing the capability of what can be
done with the processor. At this scale there is plenty of room for all
of the memory system but also room for many peripheral devices. This
also becomes expandable as necessary for the application being done.
This decision to use 16-bits make the design and architecture much more
complex but at the tradeoff of having a much more useful processor and
one that satisfies the use case.

### *Addressing Modes*

Every microprocessor has addressing modes that define the way a certain
operation will interact with its operand. In this microprocessor there
are 5 distinct addressing modes, inherent, immediate, direct, indexed,
and stack. These were chosen as an architectural decision because it
allows for all the basic needs of a microprocessor to be completed in
operation. Inherent addressing means that there is no operand and the
operation can be performed with just the operation code. Immediate
addressing is when the data is immediately after the operation code.
This data is what is used to perform the operation. Direct addressing
has the effective address of where the data is to be used for the
operation. Indexed addressing contains an offset in the operand that is
used to find the effective address of the data that needs to be used to
perform an operation. Finally, the stack is used to store values to and
from a point in memory.

### *Jumps Only*

There are multiple ways in which microprocessors change the location of
where the program counter is pointing. One of these ways is by using
branching which involves an offset to the program counter to point it to
another spot. Another way is by jumping, this actually replaces the
program counter with a new location to point the program counter. It was
decided that it would make sense to just use jumps only instead of both
jumps and branches because it would simplify the instruction set as well
as the complexity of the processor without introducing any redundancy.
These jumps would also be useful in setting up the subroutines.

### *Subroutines*

Subroutines provide a way of doing a certain small program within a
bigger program. By implementing subroutines into the design, the
complexity of the design increases. Subroutines are something that are
very useful when programming so the inclusion of them into the design
seemed to make a lot of sense. The subroutine return value for the
address was stored at the same place in memory so that the subroutine
would always work.

### *Bootloader*

One of the impacts of using a bootloader is that it is a more efficient
way of programming the microprocessor.  The bootloader is used for on
the fly changes to the program that is run by the processor over some
sort of communication port.  This affects the design of the processor
because in order to use a bootloader it is necessary to develop the
necessary device that allows the communication between an external
computer and the processor.  For FPGA’s it is possible to program the
board when it is being designed but it requires that the entire board
will need to be flashed each time with all of the programming
information first and each time a new program is used.  Allowing a
bootloader will significantly speed up this process and allow for a more
dynamic programming environment for the user.  This makes the use case
much more practical for each program to be done by the user on another
computer.  The downside to using a bootloader is that is a more complex
design and also that it is possible for loaded programs to be corrupt if
not designed properly. Lastly if no program is sent or there is no
program to start, then the processor will lose a lot of functionality by
waiting.

### *Implementing on an FPGA*

Implementing the entire circuit onto the FPGA is an architectural
decision because it limits what is possible with the microprocessor but
also improves the expandability and speed of development. FPGAs prove to
be a great way of implementing complex specific integrated circuits
because of their ability to have a lot of input and output. In addition,
it is impossible to implement an integrated circuit with a significantly
cheaper cost than an actual manufactured integrated circuit. This makes
it the best choice for designing our custom microprocessor on.

Constraints
-----------

When designing a microprocessor there are many constraints that need to
be taken into consideration in order for the microprocessor to work per
accordingly with the use case. Below list all the possible constraints
of the project and what their solution was.

### *Language for Hardware Modules*

One constraint when designing digital logic for a FPGA is choosing which
hardware description language to use. The primary options are VHDL or
Verilog with support from the largest manufacturers of FPGAs,
Altera/Intel and Xilinx. VHDL was chosen because of our familiarity with
it, however any hardware description language could have been used in
addition to some new niche languages that support translation to VHDL
fairly simply. Also, this constraint was cemented in place once the FPGA
device target was chosen to be from the Xilinx Spartan Family. These
devices in particular are only supported on the legacy software known as
ISE and ISE does not support Verilog.

### *Number of Gates on FPGA*

All FPGAs have a limited amount of logic elements that can be used for
digital circuit design. Even within the same family of a FPGA chip there
are many different package formats that have far more or far less logic
elements. The reason for this disparity in packages is that for some
applications it is not necessary to have an enormous amount of logic
devices. These chips typically will be cheaper. In addition, some large
scale specific integrated circuits might need a lot of logic elements
and for these designs a larger chip package can be chose to cover the
need for that specific applications. These chips end up costing more but
still have the same functionality and ease of use as the smaller chips.
The way that this was decided for this particular project was first by
using a family of chips that were familiar, such as the Spartan 6, and
then through many revisions, the final chip was chosen by synthesizing
on other packages to reach optimal efficiency.

### *Mathematic Operations*

One decision that needed to be made when constructing this processor is
the inclusion or exclusion of certain mathematical operators. This
becomes a constraint because it will limit the potentially functionality
of the processor but also the complexity of the design and code. Some
things that could have been included would be a multiply and divide
instruction. These would have improved the capability of the processor
but would have ended up making the design more complex as well as then
needing a way to store floating point numbers. In order to keep the
processor to a reasonable size and complexity these instructions were
ignored due to the fact that they could be remade by using repeated
addition or repeated subtraction to calculate in some specific
application.

### *Addressable Data Size and Memory to Hold Complex Programs*

The constraint for the data bus’s side was determined by seeing what a
useful programs size might be. In the case of this microprocessor
system, it makes much more sense to allow for a 16-bit memory bus so
that much more data could be addressed and used for programming. One way
this was determined was by writing simple and complex programs in the
instruction set simulator. By using this, the length of the programs
could be seen and for something as complex as a bootloader it was
obvious that much more data would need to be stored and used rather than
something as small as 8-bits. One thing that was kept the same however
was the size of the data itself on the bus. This data was all kept to
8-bits to make the speed much faster when transferring data as well as
when calculations were being performed.

### *Power Use and Heat*

The power use for the system is something that is a constraint but not
something that is used to limit the design of the processor. In
particular, the power use would be that from a USB so at most 5V at 1A
which would just be 5W. This is similar to many small embedded
processors and microcomputers. Heat is something that doesn’t
necessarily need to be considered into the design either because with
the low enough power use of the processor there shouldn’t be too much
heat produced unless the speed of the processor was increased.

### *Budget and Time*

In any project there are budget and time constraints. The specific
constraint for this project is time because there is one full semester
to accomplish the use case that was declared for the project. With more
time there is definitely more that can be done with the actual project
and a lot of improvements can be done. The budget for this project is a
constraint to some regard, because without a sponsor, any parts for the
project comes out of our own pockets.

Chapter 4: Professional Standards
=================================

When designing something, there are standards that you need to follow
based on principles of openness, balance, consensus, and due process.
These standards can be for a number of things, like technical, safety,
regulatory, and societal and market needs. A lot of the standards are
provided by Institute of Electrical and Electronics Engineers (IEEE) for
our project.

JTAG Standard
-------------

A standard that needed to be adhered to in our project are the Joint
Test Action Group (JTAG) for Boundary Scan Instructions. The JTAG
interface is something that is used in order to load the FPGA board with
the code to program it to make it our microprocessor. It is not
necessary in the end product but more rather for debugging purposes as
well as quick updates to the system design.

VHDL Standard
-------------

In order to program our microprocessor, we need to choose a language.
The language we were taught and most familiar with was VHSIC (Very High
Speed Integrated Circuit) Hardware Description Language (VHDL). VHDL is
the programming language that can be used to implement hardware onto an
FPGA. It is a very versatile language and has a lot of features to make
programming the board more accessible and an easier process.
Specifically, we are using VHDL-93 because that is what our version of
ISE Design Suite was compatible with. It is also used for lower end
processors mainly for testing and checking and reading signals.

Peripheral Standard
-------------------

The IEEE Standards Working Group 1284 is a group of standards that
relates to parallel communication specifically for connectivity needs
whether it be over PS/2, USB, or via a network connection. One of the
standards included in this group is the standard for Bidirectional
Parallel Peripheral Interface for Personal Computers. This is what is
going to be used in order to communicate with our microprocessor.

SREC Standard
-------------

Another standard needed to be followed was the SREC file format type.
This standard for SREC file format configures binary information in
ASCII hex text form. It is primarily used for programming flash memory
in microcontrollers and logic devices. The typical application uses an
assembler and converts a program’s source code into machine code and
outputs the hex file. The file is then imported to put the machine code
into non-volatile memory or the target system for loading and execution.
The s19 record will be less than or equal to seventy-eight bytes in
length. The format for the s-record is 2 bits for type, 2 bits for
count, 4, or 9 bits for the address, up to 64 bits for the data, and 2
bits for the checksum. The type field describe the type of record. The
count field displays the count of remaining character pairs in the
record when it is paired with as a hexadecimal value. The address field
displays the address at which the data field is to be loaded into
memory. The length of the field depends on the number of bytes necessary
to hold the address. The data field represents the memory loadable data.
The checksum field display the least significant byte of the ones
compliment of the sum of the byte values. These values are represented
by the pairs of characters that make up the count, the address, and the
data fields. Specifically, our project utilizes the s19 file format
because it has sixteen-bit addresses.

![Motorola SREC
Chart.png](media/image8.png){width="3.7441863517060368in"
height="2.5777832458442695in"}

[]{#_Toc514031707 .anchor}Figure . S-Record Format Information

Chapter 5: Project Plan
=======================

In order to get the project started, we had created a projected timeline
of what needed to be done in order for this project to be completed.
This timeline started off at the beginning of the spring semester. With
that in place, work was also expected to be started over winter break
which was about one-month long. Once winter break ended, it was time to
follow this timeline in order to have this project completed on time.

![](media/image9.png){width="6.394168853893263in"
height="2.1347222222222224in"}

[]{#_Toc514031708 .anchor}Figure . Initial Gantt Chart Project Plan

The first thing on the timeline to complete was the instruction set
architecture. By the completion of the instruction set architecture. The
goal at the time was to finish the instruction set architecture by only
including the necessary instructions for the controller to be started.
This was one of the discoveries made because of many duplicate or
unnecessary instructions that were removed. As part of the instruction
set architecture, the encoding was done for all the instructions. The
encoding was crucial to getting the controller started as that was a
lengthy process. At that point in time, our project was currently behind
by one week. In order to get back on track, when it came to the design
of the controller, as long the design did not last more than 3 weeks,
there would still be more than enough time to work on everything
necessary for the project.

After the completion of the instruction set architecture and encoding,
work on the assembler began, as well as the VHDL code for the
multiplexers of the data path. The data path was still being worked on
to iron out any uncertainties that may arise during the testing and
simulation. The goal of that week in correlation with our projected
timeline, was to work on the assembler at the same as the data path. The
assembler began to create s19 files from the assembly code. With these
components being completed, the design of the controller would be smooth
and easy. Something needed to be figured out during this week was how
sixteen-bit addresses were going to be handled. This led to certain
options and decisions like having two clock cycles per bus cycle to
avoid any current and potential errors. This is the critical point in
our timeline because we need to be one hundred percent confident in the
data path so the controller design will be smooth. This will help us
keep on track with our timeline and ensure that finish all the remaining
design elements. The current plan was to create the entire data path
schematic in Xilinx during this next week. Test benches were then being
written for a variety of instructions. This week is where more thought
was being put towards the controller and its design using state
diagrams. An issue that arose was handling sixteen-bit addresses with
the ALU as well as the data out multiplexer. We managed to solve both of
these problems in a timely fashion so it did not set us back. The
project at this point is making progress and moving forward. In the next
week, the controller and memory system should be finished to ensure
enough time can be left for simulations and testing.

Next on the projected timeline was the design of the interface module,
then serial communication port, and PCB design. In order for us to have
a working PCB design it would need to be completed before spring break
for it to be shipped in time. This was because we would need to test it
and check for any issues in order for us to get another one. In the end
we were not able to get working PCB design due to the fact that it was
not critical to the design and functionality of the project because a
breadboard can still be used in significantly less time. Instead the
time went to making final touches on the data path and thinking of the
controller in terms of control signals from the data path. Work is also
being on the simulations of the data path to ensure that instructions
are operating as expected. At this point in time, the project is behind
in terms of the projected timeline but moving along at a fair rate. It
is definitely clear at this point that more work needs to be done at a
faster rate each week to ensure we have a fully operational design.

In this week the final touches were put on the data path and memory
system. Currently the work is being done on the controller. This
includes the design of the state diagrams as well as VHDL code. The
original design of the controller was going to be in microcode but
seeing as we know how well the instructions operate, it was simpler to
design the whole controller was one big state machine. The controller
was then being tested to interact with the memory system and data path.
The first thing that was done was the reset to ensure that the processor
correctly reads the program start address from the last two bytes in
ROM. The controller was then finalized and all instructions were tested.
This was done by putting the respective opcodes and operands into ROM to
ensure that their operation was correct as expected. At this point in
the semester there is not much time left to keep working on this project
but everything is going smoothly and assuming that there are minimal
problems with the design elements moving forward, such as the bootloader
and serial communications, the project will be completed on time.

The assembler is currently being finalized to work with the bootloader.
The bootloader will be a modified version of the Boots program but
written to operate on this microprocessor. Details are being worked out
to have successful implementation of serial communication as well. These
are the last few components needed to be finished in the projected
timeline. In the next week, the bootloader and assembler were finished
up to the point of serial communications. There was errors and issues
when trying to implement the BxCom VHDL component code within our memory
system. A couple weeks went by which set us behind in order to get a
working implementation of our microprocessor. At that point, not enough
time was put into getting the serial communication port completed on
time by the time that it was to interface it with peripheral devices. We
reached the time of the presentation where a simulation was only to be
shown of the project.

In order to implement this project, a budget was made to project the
cost of our project. Originally, the projected cost of our project was
zero dollars. The reason for that was that the University of Hartford
already had a lot of the tools and programs necessary for this project
to be completed. The cost of the PCB design was expected but because we
decided that time was an issue, we did not have to make that purchase.
The other expected cost was based on whether or not we wanted to buy our
own FPGA board. On average, the cost comes out to \$140. Even then, it
is not a necessary purchase, but one we made for the reason that there
was more space on the board for our microprocessor program.

Chapter 6: Detailed Design
==========================

In designing a microprocessor there are many design elements that make
up the entire project. Listed below are all the design elements that
were implemented on the FPGA microprocessor.

**Design Elements**

[]{#_Toc514031689 .anchor}Table . Design Elements (not simulation
elements)

![](media/image10.png){width="6.5in" height="1.0611111111111111in"}

Instruction Set Architecture
----------------------------

In order to design a proper instruction, set the architectural decisions
were used to form an understanding of all the registers that were
allowed to be used by the user. By looking at the available registers,
it is fairly simple to make the instructions that will correspond to
those registers accordingly. In addition, the architectural decisions
are what forms the actual instructions to be included in the set. The
registers that can be used by the user are listed below.

[]{#_Toc514031690 .anchor}Table . Registers Seen by User

![](media/image11.png){width="5.395833333333333in"
height="2.1979166666666665in"}

With the knowledge of all these registers and what they represented it
was possible to start deciding what instructions would be included to
the actual instruction set. This was also determined by using the
instruction set simulator as a guide and tool in the design. The
instruction set was also designed using the known addressing modes such
as, inherent, immediate, direct, indexed, and stack. The final thing
that was taken into consideration was the fact that no branch type
instructions were going to be used and instead all types of branching
would just be replaced with jumps. This decision was made to simplify
the actual instruction set and limit redundancy in the actual
microprocessor.

To start off with the design the first instructions to be included were
the load and store instructions. These were first written to have
immediate data and then would have another instruction that could cover
the use case for the other addressing modes. After that more
instructions were added such as addition and subtraction of the
accumulators. Not only with immediate and direct data but also some
inherent instructions that would add the two accumulators together or
increment or decrement them as necessary. The other instructions that
were included were based off of test programs that were programmed using
the instruction set simulator.

The instruction set simulator was extremely useful in learning about
what was necessary to include in the processor, as well as what was
necessary to exclude. The test programs that were written typically
would include basic operations that would be expected to be carried out
by the processor to carry out the use case. Then these instructions were
added to the list of the actual instruction set. It was determined to be
complete once all the basic test programs ran and there were no other
complications to it. The set was also slimmed down from any excess or
unnecessary instructions just to make sure that the implementation
process would go as smoothly as possible.

Listed in Appendix A are all the instructions that were implemented into
the microprocessor. In total there are approximately 60 instructions.
This makes the instruction set simple enough to use and understand,
while complex enough to do especially cool things with the
microprocessor, such as talking to peripheral devices.

Encoding Scheme
---------------

Encoding is the process of taking the assembly level language
instructions and converting them to something that the processor can
read and understand and then execute. The encoding for this particular
microprocessor was chosen to be fairly arbitrarily. This means that the
encoding scheme is not necessarily something that can be figured out by
hand but rather something that corresponds to a pattern relative to the
design of the microprocessor. The encoding is specifically designed in a
way that it is easy to see what addressing mode the instruction mode
belongs to but not what it is actually doing unless looking it up in the
datasheet.

The encoding scheme is as follows. For the first 4-bits, a value is
chosen that is consistent among all the same instructions within an
addressing mode. For example, this means that all the instructions that
correspond to something such as inherent addressing will start with a 0.
That zero of course refers to a hexadecimal value because this is how
all the values would be read from the microprocessor and also how data
would be encoded to from the assembly files. In table \# all the
addressing modes are listed were there associated encoding scheme for
the first 4-bits of the encoded value.

[]{#_Toc514031691 .anchor}Table . Encoding of First 4-bits and
Associated Addressing Mode

![](media/image12.png){width="4.006201881014873in"
height="1.767442038495188in"}

The next 4-bits of the encoding scheme were the individual instruction
within the specific addressing mode that was being covered. This leaves
only 16 possible instructions for each addressing mode. Although this
doesn’t leave a lot of room for many more instructions per addressing
mode, it covers all the possible instructions that were covered in this
instruction set. If more instructions need to be added there is plenty
of room in 8-bits to encode more instructions for 8-bits there are 256
possible instructions that could be included in the instruction set. The
encoding for each individual instruction is shown in Appendix A in both
binary and hexadecimal.

Data Path Design
----------------

The data path is one of the most important design elements within the
microprocessor. It defines the way that data moves around within the
actual processor and where instructions will be allowed to operate at
any given point. The data path also contains the arithmetic logic unit.
This device is what allows the mathematical operations of the processor
to be performed as well as any logical operators between its inputs.

The initial design of the data path was created by placing all the
possible registers that were to be used onto a diagram. This diagram was
then connected up as logically as possible without any code being
written. After an initial revision was made there were instruction
traces written for the instructions to ensure that they would be able to
operate. If the instruction was not able to operate correctly then the
design was iterated and revised to accommodate new changes.

Initial revisions for the data path can be seen in Appendix B and C.
These revisions were far off from what the actual design would end up
being, however they give a good example of where the data paths design
started and the methodology for improving it. Many of the core
components were there but the design itself needed improvement before it
would be able to cover all the instructions necessary. With the third
revision of the data path, that can be seen in Appendix D, the actual
design was starting to shape up to be usable for most if not all
instructions. To simplify the actual look of the data path a block
diagram is shown below.

![C:\\Users\\Brice\\Documents\\Capstone\\datapath\_blockdiagram.jpg](media/image13.jpeg){width="4.906976159230096in"
height="6.237760279965005in"}

[]{#_Toc514031709 .anchor}Figure . Data Path Block Diagram

Each one of the blocks in the block diagram were then converted into the
corresponding VHDL code and connected together to form an actual data
path that could be used for simulation and testing. In the figure below
the entire final schematic for the data path and in the sections below,
the code will be shown and explained to demonstrate the design of the
data path.

![C:\\Users\\Brice\\Downloads\\dpath.PNG](media/image14.png){width="5.6636297025371825in"
height="5.732557961504812in"}

[]{#_Toc514031710 .anchor}Figure . Final Schematic for Data Path

### *8-bit Register VHDL Design*

The 8-bit registers within the data path are the 2 general purpose
accumulators, the instruction register, the code condition register, and
both din1 and din2. To design the 8-bit registers it was necessary to
have a load enable, this input would make it so that the register could
only be loaded with 8-bit data when the enable is high. The 8-bit
register is also synchronous so data can only be written or read if the
clock signal is high. The last thing to do inside of this make the
output irrelevant when the register is not being used. This is because
all registers and data on the data path is happening simultaneously.

![](media/image15.png){width="2.686046587926509in"
height="2.919051837270341in"}

[]{#_Toc514031711 .anchor}Figure . VHDL 8-bit General Purpose Register

The code shown in above is what is used for the A, B, and CCR registers
however the din1 and din2 registers needed to be designed slightly
differently to accommodate other instructions. In din1 and din2 there is
an additional bit for the load signals. This makes it so that the value
that is being inputted into din1 or din2 can change. The specific input
is labeled adx\_din because it refers to an address value that would
need to be loaded into din1 and din2 from within the data path and not
from the memory system, which is where the data normally comes from. The
figure below shows how this was implemented in VHDL.

![](media/image16.png){width="2.348837489063867in"
height="3.104498031496063in"}

[]{#_Toc514031712 .anchor}Figure . VHDL din1 and din2 Registers

### *16-bit Register VHDL Design*

The 16-bit registers within the data path are the index register, the
stack pointer register, and the program counter. The index and stack
pointer registers were designed fairly similar to the 8-bit general
purpose registers and the only thing that was changed was there increase
in length to 16-bits. Everything else behaved similarly such as the load
enable and a synchronous load. This can be shown in figure below.

![](media/image17.png){width="3.64546697287839in"
height="3.4069772528433946in"}

[]{#_Toc514031713 .anchor}Figure . 16-bit General Purpose Register

The program counter followed the same idea as this code however it
needed to be a bit different to account for the incrementing that would
be done at each fetch. The program counter is 16-bits and the ALU is
8-bits. This meant that something would need to be done in order for the
16-bit value to add an 8-bit value without leaving the program counter
in an invalid state, something that should never happen during a
processors operation. The solution was to use another register to hold
the low byte while the upper byte was added to any possible carry. These
were then concurrently placed into the 16-bit register through two 8-bit
inputs, one for the high byte and one for the low byte. This is shown in
the figure below.

![](media/image18.png){width="3.5930227471566054in"
height="3.6015179352580926in"}

[]{#_Toc514031714 .anchor}Figure . PC Register VHDL

### *8-bit ALU VHDL Design*

The ALU was designed in a way so that all the simple 8-bit operations
could be performed with no complications. Below in table 4 is all the
possible outputs that could come from the ALU and what flags are set in
accordance with those operations. A and B refer to the inputs A and B of
the ALU and not the Accumulators A and B. Also, there is no Invert B
because the ALU only needs to invert values from the A input.

[]{#_Toc514031692 .anchor}Table . Operations Supported by ALU

![](media/image19.png){width="5.0in" height="1.65625in"}

These basic operations would allow for all the instructions to be
supported and would provide an easy enough design challenge in
constructing an ALU that could do all of this and support the necessary
flags. The ALU is set up in a way where all the operations are
controlled by a select of 3 bit, this is because there are only 8
operations in the ALU. The final two bits being used in the load select
between the outputs of the ALU. Those outputs correspond to the
Accumulators, the Hold Low Register, and the Code Condition Register.
The figure below shows exactly how this was implemented in VHDL.

![](media/image20.png){width="6.5in" height="6.716666666666667in"}

[]{#_Toc514031715 .anchor}Figure . ALU VHDL Code

In general, because there were no complex mathematical instructions
included in the instruction set, writing the VHDL for this ALU was far
easier than if it were to have multiply or divide or something else of
that nature. Leaving the ALU in this state leaves a lot of room for
improvement and many more instructions could be implemented fairly
easily if they were to be included in the ALU.

### *Multiplexer VHDL Design*

Multiplexers are an essential part of the data path design. They allow
for signals to be selected from the data bus to be input into another
module such as the ALU or to be put out to the memory system. The
multiplexers in this design currently are the ALU A input, the ALU B
input, the Data output, and the address output. The reason why these
multiplexers are necessary is because they provide a way for the
controller to select between the variety of signals that are available
at any given point in the data path. Below are each of the multiplexers
and which signals they control.

**ALU MUX A**

The ALU needs two separate distinct inputs when calculating any
operation otherwise it will have an invalid output. This multiplexer is
the A input and contains all the signals that would be necessary for the
different operations. It contains a lot of signals because of the way
that the ALU is set up. This multiplexer only deals with 8-bits as
inputs and outputs so it was necessary for any 16-bit values to split
them up into a higher byte and a lower byte. The default setting for
this multiplexer is zero when nothing is being used or loaded from it.
The borrow value is also set within it so that if the subtraction of a
value were to cause the value to turn negative, the borrow value could
be used in the next operation. Below shows the VHDL code that describes
how this multiplexer operates.

![](media/image21.png){width="5.302083333333333in"
height="3.4167016622922133in"}

[]{#_Toc514031716 .anchor}Figure . ALU MUX A VHDL Code

The signals that this multiplexer handles are as follows; din1, A, B,
CCR, cin1, din2, SPH, SPL, XL, and XH. These were determined through
extensive testing of each instruction.

**ALU MUX B**

This multiplexer is the B input of the ALU and contains all the other
signals that are necessary when performing mathematical operations. Its
function is fairly similar to that of the A multiplexer and it contains
some of the same signals. This multiplexer also splits up the 16-bit
data values into their corresponding 8-bit high and low value, just like
the A multiplexer. The default setting for the multiplexer is to output
zero. Below shows the VHDL code that describes how this multiplexer
operates.

![](media/image22.png){width="4.918604549431321in"
height="3.02998687664042in"}

[]{#_Toc514031717 .anchor}Figure . ALU MUX B VHDL Code

The signals that this multiplexer handles are somewhat redundant to
those in A but this is to allow many different varieties of inputs to be
used in the ALU. The signals are as follows; A, B, XH, XL, PCH, PCL,
SPH, SPL, din1, cin1.

**DOUT MUX**

The data out multiplexer is used to output data from certain registers
to the memory system. One thing to note about it is the separation of
16-bit signals into 8-bit bytes. This is done because the memory system
can only read and write 8-bit values. One drawback that this will cause
is that it will take more clock cycles to output a 16-bit value. The
benefit is that everything is still supported as expected and because
this processor is not performance oriented then the drawback is not bad
at all. The data output sends a default signal of zero when nothing is
being loaded into it. Below shows the description of the DOUT
multiplexer in VHDL.

![](media/image23.png){width="3.837209098862642in"
height="2.1084306649168854in"}

[]{#_Toc514031718 .anchor}Figure . DoutMux VHDL Code

The signals that were chosen to be output from DOUT are for specific
reasons. The A and B are chosen because that is the nature of a load
store architecture. The CCR was chosen as a way to store the code
condition registers flag values if necessary. The Program Counter was a
necessary output because it would store the return value for any
subroutine. The index register was chosen to store the value of the
index register in memory as was din1 for any values that came into the
din1 that needed to be sent back out right away or if the value was an
offset.

**ADX MUX**

The address multiplexer contains all the possible addressed that would
be sent out to either the ROM or RAM. The values that are sent to the
read are the addresses that contain the program start address, which is
always located at 0xFFFE and 0xFFFF. The other addresses are all things
that would be read or written to when considering the data in memory.
The only thing of note is the address that will always contain the
return address for a subroutine. These values are 0x7FFE and 0x7FFF. The
reason for using specific values for the return address is so that
things such as the stack can be reused as seen fit by the programmer
throughout the programming of the microprocessor.

![](media/image24.png){width="3.6992979002624673in"
height="2.2093022747156605in"}

[]{#_Toc514031719 .anchor}Figure . ADX MUX VHDL Code

The other signals that the address multiplexer handles are the index
address, the stack address, the program counter address, and DIN. DIN is
the concatenation of din1 and din2, these two registers contain
temporary addresses that need to be used for retrieving data from the
memory system. One such example is an offset value and the index
register. The other signals present are used as necessary from the
instruction that is being used.

### *Hold Low Register and Borrow Out Register VHDL Design*

One problem that arose while designing the data path was the invalid
state of the program counter that was caused when incremented because
the program counter was 16-bits and the ALU was 8-bits. The solution to
this problem proved to be very interesting and useful for other values
that would be calculated with the 16-bit registers. The Hold Low
register stores the value of the lower byte of any 16-bit value that is
being used within the ALU. This prevents invalid states because the
16-bit registers can then be loaded with both the high byte, from the
output of the ALU, and the low byte, from the output of Hold Low. The
Hold Low register is basically the same type of register as all other
8-bit general purpose registers with a load and clear for all of its use
case. Below is the code that describes the Hold Low register.

![](media/image25.png){width="3.2558136482939632in"
height="4.067674978127734in"}

[]{#_Toc514031720 .anchor}Figure . Hold Low Register VHDL Code

Another thing that needed to be accounted for with the 16-bit values was
the possibility for borrows and carries to be used in the addition or
subtraction of a number. In order to solve this problem, a simple flip
flop was used to obtain the value of the flag for carry or borrow and it
was attached as an input into the ALU’s multiplexer inputs. Below shows
the code for this simple flip-flop with a clear signal.

![](media/image26.png){width="2.722354549431321in"
height="2.779069335083115in"}

[]{#_Toc514031721 .anchor}Figure . Borrow or Carry Out VHDL Code

### *Clear Signal*

One thing to note about all the VHDL modules within the data path is
that they all have a signal to clear them. This was used so that there
would never be an invalid state within any of the modules at startup of
the microprocessor.

Memory System Design
--------------------

The memory system is another one of the most critical items when
designing a microprocessor. It contains the values of all the
instructions that are being used in any program as well as providing a
location for other data items to be read and written to. The memory
system used in this microprocessor has synchronous read and synchronous
write. This means that there needs to be a clock signal in order for the
microprocessor to read or write data from the memory system. The way
that addresses were differentiated within the memory system was by using
enables that corresponded to the address of the device that was being
used. In addition to this, there would be a manual signal from the
controller that would state whether there was reading or writing being
done. Below shows the code for the address enabler within the memory
system. This adx\_en also determines what the addressable spaces of the
memory are.

![](media/image27.png){width="3.4302329396325457in"
height="2.5095286526684166in"}

[]{#_Toc514031722 .anchor}Figure . adx\_en VHDL Code

### *Addressable Memory Addresses*

With the current values that are within the adx\_en the addressable
space for each memory device can be seen below in table 5.

[]{#_Toc514031693 .anchor}Table . Addressable Memory Addresses

![](media/image28.png){width="6.7395395888014in"
height="1.0697681539807524in"}

The only reserved memory locations that should never be replaced are
0xFFFE, 0xFFFF, 0x7FFE, and 0x7FFF. This is because they contain the
value for the program start address and the return address of the
subroutines respectively.

### ROM VHDL Design

The ROM Is read only memory, in the microprocessor read only is handled
by an assortment of enables that only allow the ROM memory device to be
read and it allows no data to be written to it. This is fairly simple to
implement in VHDL because the device itself just needs to only have an
input for the read enable signal and from the address enable. Once both
of these signals are high the output can be read from the device during
a low clock phase. One other thing that the ROM uses is a look up table,
this table contains all the hexadecimal values of the opcodes and
operands for the processors operation. Below is the description in VHDL.

![](media/image29.png){width="3.710890201224847in"
height="5.1046511373578305in"}

[]{#_Toc514031723 .anchor}Figure . ROM VHDL Code

The figure above shows how the output of the ROM can only show a value
when it is the device being read and also that it cannot replace any
values internally because there is no input for 8-bit data.

### RAM Design 

The RAM is random access memory, this memory is volatile meaning that it
will reset every time the microprocessor system runs. It can be read and
written to without any changes to the code. This is in contrast to the
ROM which will save its state every time the processor is re-started.
The RAM is designed by using the same type of enables as the ROM however
it allows the use of a write signal as well. The read and write signals
are never both active at the same time because this would provide an
invalid state to the memory and nothing would work properly. Below is
the VHDL code that describes how the RAM is actually implemented in this
design.

![](media/image30.png){width="3.6395352143482063in"
height="5.200567585301838in"}

[]{#_Toc514031724 .anchor}Figure . RAM VHDL Code

This design for RAM instantiates 2\^14 bytes and uses an array to store
the data as it comes in from the data path. This method makes it
volatile which is how the RAM is supposed to behave. In general, the
output code and enable code is very similar to that of the ROM. The RAM
is set to 0 when the clear signal is set and this is to make sure that
nothing starts with an invalid state within the RAM.

Controller Design
-----------------

Designing the controller was definitely one of the most challenging
aspects of the entire microprocessor design. This is because it controls
all the necessary enables throughout the processor that allow everything
to run as expected. All instructions are handled using the controller
because it sends the enables that actually cause the instruction to
perform as it was described in the instruction set architecture.

The design for the controller ended up being a giant state machine.
Using this method, it was fairly simple to work on each piece of the
controller to end up with a fully functional device. The controller
takes a few inputs from the data path such as the status flags within
the code condition register and then it sends out the corresponding
signals as necessary. The controller also takes the input from the
instruction register so that it can determine what instruction is
currently fetched and what needs to be sent out. The controller can’t be
included due to the length of the code but will be attached in the
appendix.

The current design of the controller is as follows. First the system is
reset. This causes the controller to look for the program start address,
which is located at the last 2 bytes of addressable memory. Then that
value is loaded into the program counter. Once that is done the first
actual byte of information can be read from ROM using a fetch and
increment. The fetch retrieves the value from memory over 2 clock cycles
and the increment adds 1 to the program counter. The first byte should
always be an instruction unless coded improperly. This instruction is
placed into the instruction register. Once there, the controller can
check the value and determine what needs to be done next. If the
instruction was inherent then the work then the next state involves
whatever work is being done, otherwise the memory system is read again
to retrieve the next value from the ROM. This cycle is known as the
fetch-execute cycle and it is found everywhere in the world of
computing.

Currently the design has no pipelining built in but it could be
implemented if performance was more of a critical issue. There are also
other performance increases that can be done to the controller but
seeing as the performance was never the goal of the use case, it has
been forgone so that the controller could be fully operable in time.

### *Controller Signals*

The controller signals contain all the enables that are sent to both the
data path and the memory system. The signals are as follows;
load\_en\_out(13:0), muxAsel\_out(3:0), muxBsel\_out(3:0),
DoutMuxSel\_out(3:0), adx\_sel\_out(3:0), alu\_sel\_out(4:0), and
rw\_en\_out(1:0). Below explains what each bit of every signal
corresponds to.

[]{#_Toc514031694 .anchor}Table . Load Enables and Device

![](media/image31.png){width="6.569767060367454in"
height="2.9883716097987754in"}

For the devices that have more than one load enable, the table below
shows what those extra signals do.

[]{#_Toc514031695 .anchor}Table . Din and CCR Load Enable Outputs

![](media/image32.png){width="6.616280621172353in"
height="2.034884076990376in"}

[]{#_Toc514031696 .anchor}Table . ALU MUX A Control Signal I/O

![](media/image33.png){width="6.639535214348206in"
height="2.4534886264216973in"}

[]{#_Toc514031697 .anchor}Table . ALU MUX B Control Signal I/O

![](media/image34.png){width="6.604650043744532in"
height="2.4418602362204727in"}

[]{#_Toc514031698 .anchor}Table . Dout MUX Control Signal I/O

![](media/image35.png){width="6.6046511373578305in"
height="2.0465113735783027in"}

[]{#_Toc514031699 .anchor}Table . ADX Select Control Signal I/O

![](media/image36.png){width="6.627906824146982in"
height="2.0232556867891516in"}

[]{#_Toc514031700 .anchor}Table . ALU Control Signal I/O

![](media/image37.png){width="6.686047681539807in"
height="2.116279527559055in"}

The last signal is the read and write signal where the second bit refers
to read and the first bit refers to write. This corresponds with the
actual naming convention to make it easy and both of them cannot be
enabled together otherwise an invalid state would be present.

Serial Communications Device Design
-----------------------------------

The serial communications device is crucial to our project because it is
what allows us to communicate with the microprocessor. This would
involve sending one bit at a time, sequentially, over a computer bus.
The BxCom serial communication port was originally developed for the
Sys08 processor to be used with the Boots program. There is no direct
support for hardware handshaking but there are simple parallel inputs
and outputs. The transmitter component within the device is
double-buffered. This allows a character to be inserted while a prior
character is being transmitted. Within the BxCom device are a series of
signals. The system signals that are write-bus related, are ena, ax, dw,
and cb. The ena signal is what enables the device using positive logic.
The ax bit are the address bits which writes the data to the device. The
rw bit is for read-write select where 10 equals read, 01 equals write,
and 00 equals no action. The last signal is the cb or clock bus signal.
The original BxCom device uses the clock bus signal to concatenate the
clock, clock phase, and clear bits. However, for our system we kept the
signals separate. The System signals that return the read-bus are dx,
dy, irq, and ira. The dx bit returns the data bus input and the dy bit
returns the data bus output. The irq and ira bits are the interrupt
request output and interrupt acknowledge input were originally used in
the BxCom device but in this project, they were not needed or necessary.
The next set of signals are the serial data and parallel data interface
signals. RxD and TxD represent the serial communications port received
and transmitted data. The PDI and PDO signals represent the parallel
ports, one of which receives data or an input and the other sends data
or an output.

The BxCom device uses its ax and rw signals to refer to specific
register locations. The device itself has 4 registers: STPX or the
Status register and punch commands, CHRS or Configuration and soft-reset
register, RDTD or Serial Communications receive and transmit data
register, and PDIO or Parallel input and output registers. The status
register indicates that the transmit is ready for a character, the
receiver has a character, a break pattern was detected, and the
transmitter is in idle state. The configuration register writes to
configure the device, however if written to the two least significant
bits, it will cause the device to have a software reset. The serial
communications receive and transmit data register returns the received
byte data register value using a read. A write command causes a byte to
be transmitted. The parallel input and output registers is where reading
returns parallel data. The parallel input data is first sampled with a
write to the status register. Writing sends out parallel data and is
sampled with a write to the status register as well.

Assembler
---------

The assembler was designed as an afterthought to the original design of
the microprocessor. Its purpose was to make programming the processor
much faster. The assembler is written entirely in Python and it is used
to assemble the custom assembly level language instructions into machine
code that can be read by the processor. The file that it assembles the
code into is of the s19 format. This format was primarily used because
the bootloader takes in files of that format as well as the RomTools
tool that was developed by our technical advisor.

To design the assembler the essential parsing was taken out of the
instruction set simulator and converted so that it could encode the
values as necessary in the assembler. Then any extra information data
had to be handled as well so this was added to the assembler’s
abilities. Finally, the assembler took the encoded string and would
write to an s19 file all the data that was necessary for an s19 file
format such as the length and the checksum. This would ensure that the
code could be read into the processor using the bootloader or using rom
tools.

In general, the assembler is a quick and dirty approach to assembly
however because it works and improves the speed of programming it is
definitely useful for the project. The future of this assembler could
have many more features and be improved significantly in compile time.
For now, it is a useful tool that was designed to improve the projects
use case and usefulness.

The code for the assembler is attached within the appendix but below is
some example code and the format in which it is output for the
bootloader or ROM to use for the programs execution.

![](media/image38.png){width="1.7808902012248469in"
height="3.9583333333333335in"}

[]{#_Toc514031725 .anchor}Figure . Assembly Language Test Code

![](media/image39.png){width="6.5in" height="0.4444444444444444in"}

[]{#_Toc514031726 .anchor}Figure . Compiled S19 Assembly File

Using RomTools the file is converted into a ROM look up table to be used
by the memory system which is shown in the figure below.

![](media/image40.png){width="4.875in" height="3.6640627734033244in"}

[]{#_Toc514031727 .anchor}Figure . Compiled Assembly Loaded into ROM LUT

Bootloader
----------

The bootloader was a necessary component in the design of the
microprocessor system because it allows the use case to be satisfied.
The bootloaders purpose is to allow code to be uploaded to the board on
the fly and executed. The specific code that is uploaded is through the
serial communications device and the type of file that is accepted is
s19. The main benefit in using a bootloader is that the speed of
programming the microprocessor is greatly increased. The reason for this
is that when code is loaded onto the FPGA it takes a long time to
synthesize and load all the hardware descriptions onto the board. While
using a bootloader that entire process is skipped and so code can be
directly loaded an executed after a one-time upload of the hardware
descriptions with the bootloader loaded into ROM.

The bootloader was written as a translation from a precious bootloader
design that was provided by our technical advisor. This code had to be
functionally the same in order to achieve the same output goals however
the code was written for a different architecture entirely. Some things
in particular that needed to be overcome were branches and branch
clears, as well as immediate move instructions and some other specific
pedantic items. The code for the bootloader is attached in the appendix.
It is currently written for the custom microprocessor architecture that
is used in this project.

Test Assembly Programs
----------------------

In order to verify the status of the use case for the microprocessor
test programs were written to ensure that the final result would be in
line with the design goals. This meant including many small test
programs during the design phase to ensure the operation was as
expected. Some simple programs that were written for example was
multiplication via repeated addition. Another example was a program that
prints hello world via an ASCII string. Using both of these as an
example it was possible to see that the final processor was on the right
track to give the results that were wanted.

The test assembly programs were also implemented onto the board and
fully simulated to ensure the operation was also as expected. This is
discussed more in the Testing Methodology and Results Section.

Implementation on Spartan 6
---------------------------

Implementation was done using the tools currently available within the
ISE Design Suite. It refers to the entire hardware description of the
device being synthesized and uploaded to the FPGA board. Doing this was
essential to the completion of the microprocessor system. It would also
allow the processor to finally be used outside of a simulation.
Implementation is successful when the code is able to transfer properly
without any errors.

Chapter 7: Testing Methodology and Results
==========================================

In any large-scale project, it is a necessity to keep testing and
incrementing design as the project moves forward. For this
microprocessor system, testing was done for each and every component
individually and then as the blocks grew bigger the testing would be
done for the larger schematic. Once everything was put together the
final tests that were run ensured that the operation of all the
instructions was as expected.

VHDL Testing
------------

The first type of testing that was done was through VHDL testbenches. In
this testing phase the VHDL module of each individual block was tested
by itself to make sure that the code was giving valid outputs and no
errors from any of the possible inputs that would be given. In all of
the testbenches that were written a clock signal was given and set to
oscillate at some arbitrary value such as 50ns. Then the testbench would
begin with a clear signal set to high as if to simulate the state in
which the processor would begin following reset.

Testbenches are an extremely powerful tool to use when simulating
integrated circuits and all of the tools within ISE make it simple and
easy to generate testbenches for the respective module that is being
simulated.

Inputs were handled in the testbench by giving the signals specific
values at certain periods of time. The one thing that always need to be
considered was the state of the clock. This is mainly because the
modules within the processor all operate on a rising clock edge. If the
code were to be set before the clock edge than it wouldn’t give the
expected output. This would become an even greater issue when the timing
becomes a greater challenge as is with the memory system.

All of the simulations were done using the tool known as ISIM. ISIM
provides a nice graphical user interface that can display a variety of
wave signals from all the digital logic that is contained within the
simulation. Depending on which module was being tested, signals would be
added to the wave window to show what the outputs and inputs were at any
given point within the simulation. ISIM also gives the user the ability
to run the simulation for any period of time which is convenient when
trying to locate a specific error or bug that is found during testing.

### *Memory System Testing*

The memory system was not particularly difficult to test because it had
many of the same test signals as previous modules before it. However,
after combining it with the data path, writing testbenches required that
the timing be perfect to simulate the actual reading or writing of data
from the data path to the memory system and vice versa.

While the memory system was contained in its own top-level module it was
much easier to design a test bench that would just test the signals and
outputs that would be expected from the memory system. Knowing that
everything worked on its own, would make the process of connecting it to
the controller and data path a much better experience.

### *Controller Testing*

The controller was one of the most critical components in the entire
design of the microprocessor and so naturally it utilized the most
amount of testing. The testing process for it was iterative for each
instruction that was handled. Before any instructions were set to be
used a reset state had to be implemented. This process involved writing
the code for the controller that would handle the reset, then running
the simulation testbench in ISIM and checking the output of waves that
should be set. Following reset the program counter was supposed to be
loaded with the last two bytes of memory. If this process did not work
then the code was modified and the simulation was run again.

Once the reset was set and working then the actual instruction handling
code could be written in the controller. This code would do one thing
but would go through many states. That is why simulating it in ISIM was
necessary. The simulation would allow almost a sort of debugging to see
what the current state was as well as the next state the controller was
headed to.

Through this iterative process the entire controller was able to be
designed and fully operational. It was definitely more time consuming
then writing all the code at once but it also ensured that the
controller was working properly which would mean that the entire
microprocessor system was working properly. Shown below are all the
signals that were checked after each run of the simulation.

![C:\\Users\\Brice\\Documents\\Capstone\\signalsforsim.PNG](media/image41.png){width="2.4767443132108484in"
height="4.589280402449694in"}

[]{#_Toc514031728 .anchor}Figure . Signals Checked after each Simulation
Increment

The controller simulations were considered finished when every single
simulation was functional for every type of instruction.

Assembly Code Testing
---------------------

The idea behind assembly code testing was to make sure that when the
board was finally implemented that all the simulation outputs obtained
would match their real-world counterparts. In general, the testing for
this was much easier than writing a test bench because it would involve
all the tools and components that were already built.

The code was written in assembly, then it was put through the assembler
to make an s19 file. With this file, RomTools was utilized to make a ROM
look up table that would contain all the necessary instructions and
data. Finally, a test bench could be made with the basics of just a
clock signal and a reset signal. This test bench would be simulated and
all the output waves would be observed till the completion of the
assembly code and it could be compared against the instruction set
simulator to make sure that the output of the program matched that of
the simulator. With all of this done, the simulation and testing portion
of the project was complete.

Example Simulation of Load A Immediate
--------------------------------------

This is an example of the simulation that shows accumulator A being
loaded with 8-bit immediate data.

![](media/image42.png){width="6.779069335083115in"
height="3.4822397200349955in"}

[]{#_Toc514031729 .anchor}Figure . Load A Immediate Simulation

In the first portion of the wave window shown above the reset signal is
sent to high and the processor fetches the program start address from
the last two bytes in ROM (fffe and ffff seen at signal adx\[15:0\]) and
loads it into the program counter (seen at signal PC with fc00).

Next, the processor fetches the first byte located at the address of the
program counter and loads it into the instruction register (seen at time
380ns at signal irout\[7:0\] = 10). The value in the instruction
register is now 0x10. This corresponds to a Load A Immediate
instruction.

The processor now knows it needs to fetch another byte from memory so it
increments and fetches the next byte (seen at time 500ns PC = fc01).

The value that is retrieved can be seen in the signal din1 as 0x06. This
value is then loaded into A (seen at time 800ns) and the program counter
can be incremented once again.

This is the fetch execute cycle within the custom microprocessor and
just one example of its functionality.

Chapter 8: Conclusion and Future Work
=====================================

This project proved to be one of the greatest undertakings as an
undergraduate within the University of Hartford. It was also the most
fun and exhilarating experiences that could be had as a computer
engineering student.

The end result was less than what was expected of the use case but the
microprocessor system was still an extremely useful learning tool and
step into what is possible in the world of embedded computing.

The reason that the project didn’t satisfy our use case is because the
FPGA was never implemented and tested with a peripheral device. This is
partially due to the time constraint of the semester and also because
the serial communications device was not finished in time.

The total percentage of completion was about 90% with the last 10% being
the peripheral device communications and testing on a physical FPGA.

The final cost was over \$150 and it should have been more considering
there should have been devices purchased but none of that happened as
time went on and the communications device was at a standstill.

In the future, plenty can be done to this project to improve. For one,
there are plenty of optimizations that can be made within the data path
and the controller. In the data path many extra unused load signals
could be removed as well as improvements to the multiplexer inputs to
only the necessary data. In the controller, optimization can be made to
the fetch and execute cycle as well as pipelining to improve speed and
overall usability. Other things could be including more instructions to
be supported as well as expanding the ALU to allow for more operations.

Lastly the project is extremely close to where it originally set out to
be and getting the processor to work with peripheral devices shouldn’t
be too hard from where the project is currently at.

All code for this processor will be released under a liberal and
permissive open source license such as MIT. This will allow any person
in the future to modify the processor and make improvements as seen fit.

Appendix
========

Appendix A: Instruction Set Architecture and Encoding Documentation
-------------------------------------------------------------------

![](media/image43.png){width="6.427083333333333in"
height="8.332397200349956in"}

![](media/image44.png){width="6.627725284339458in"
height="8.864583333333334in"}

Appendix B: Data Path Revision 1.0
----------------------------------

![](media/image45.png){width="6.739583333333333in"
height="8.575365266841645in"}

![](media/image46.png){width="6.941445756780403in" height="8.90625in"}

Appendix C: Data Path Revision 2.0
----------------------------------

![](media/image47.png){width="6.647133639545057in" height="8.625in"}

Appendix D: Data Path Revision 3.0
----------------------------------

![](media/image48.png){width="6.385416666666667in"
height="8.657761373578303in"}

Appendix E: Controller VHDL Code
--------------------------------

![](media/image49.png){width="5.802326115485564in"
height="8.667224409448819in"}

![](media/image50.png){width="6.017684820647419in"
height="9.083536745406825in"}

![](media/image51.png){width="5.679505686789152in"
height="9.11007983377078in"}

![](media/image52.png){width="6.102228783902012in"
height="9.052061461067366in"}

![](media/image53.png){width="6.116278433945757in"
height="9.085222003499563in"}

![](media/image54.png){width="6.213663604549431in"
height="9.14150043744532in"}

![](media/image55.png){width="6.211724628171479in"
height="9.057682633420823in"}

![](media/image56.png){width="6.173207567804025in"
height="9.12505358705162in"}

![](media/image57.png){width="6.211240157480315in"
height="9.100485564304462in"}

![](media/image58.png){width="5.77446741032371in"
height="9.086853674540682in"}

![](media/image59.png){width="5.734625984251968in"
height="9.11920494313211in"}

![](media/image60.png){width="6.177567804024497in"
height="9.09510498687664in"}

![](media/image61.png){width="5.8631299212598424in"
height="9.127679352580927in"}

![](media/image62.png){width="5.88469050743657in"
height="9.121270778652669in"}

![](media/image63.png){width="6.003391294838146in"
height="9.162380796150481in"}

![](media/image64.png){width="6.087936351706037in"
height="9.151543088363955in"}

Appendix F: Assembler Code
--------------------------

![](media/image65.png){width="5.15625in" height="8.634001531058617in"}

![](media/image66.png){width="6.114583333333333in"
height="9.12113188976378in"}

![](media/image67.png){width="5.822916666666667in"
height="9.115788495188102in"}

![](media/image68.png){width="6.166666666666667in"
height="8.981884295713035in"}

Appendix G: Bootloader Code
---------------------------

![](media/image69.png){width="5.677083333333333in"
height="8.730869422572178in"}

![](media/image70.png){width="5.645833333333333in"
height="8.976468722659668in"}

![](media/image71.png){width="5.8106977252843395in" height="9.03125in"}

![](media/image72.png){width="5.947870734908136in"
height="9.061429352580927in"}

![](media/image73.png){width="5.90625in" height="8.333284120734907in"}
