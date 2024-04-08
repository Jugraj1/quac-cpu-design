# Report 

<!-- Your text goes here. Remember to check the result of your CI to see whether 
the final PDF rendered correctly! -->

## Overview
Enhanced with capabilities for multiplication, division of both positive and negative numbers along with flag management, my Extended Single Cycle CPU offers advanced computational functionalities. Maintaining simplicity, my CPU retains the base ISA by leveraging the 4th and 8th bits of the machine code related to the ALU instructions.E
The following table shows the syntax of MULTIPLICATION and DIVISION: 

| EXTENSION | DESCRIPTION            | SYNTAX          | EXAMPLE MACHINE CODE           |
|-----------|------------------------|-----------------|--------------------------------|
| MULTIPLY  | multiplies two numbers | mult rd, ra, rb | 1xxx <cond> <rd> 0 <ra> 1 <rb> |
| DIVIDE    | divides two numbers    | div rd, ra, rb  | 1xxx <cond> <rd> 1 <ra> 0 <rb> |

## Microarchitecture Changes
The enhancement involved adding a two-bit control signal, ALUOPEX, to the control unit, facilitating extensions. A corresponding input signal was integrated into the ALU, enabling it to differentiate between operations. Additionally, an extra UNDEF 1-bit output was included in the ALU, serving as an indicator for division by zero and doubling as a flag. Extensive modifications were made to the ALU to accommodate the integration of multiplication and division functionalities. This included adding multiplier and divider components, integrating them with the ALUOPEX signal to discern between various operations while preserving the integrity of the 2-bit ALUOP signal. ALUOPEX functioned as a selector bit within a multiplexer. Furthermore, the multiplication and division flags were consolidated with the existing flags, excluding the UNDEFINED flag, using another multiplexer.

## Analysis and Tradeoffs
# Key benefits of my changes
In traditional CPU design, the flag register is typically kept separate from the main register file for streamlined operations. This architectural decision facilitates efficient writing to the flag register by directly connecting the ALU's output (RESULT) to the flag register (FL) in our case. Similarly, reading from the flag register is simplified through the use of a multiplexer, which selects and outputs the relevant flag based on the selector bit during read operations.
This separation optimizes the CPU's functionality and ease of use. Writing to the flag register is straightforward, requiring a direct connection from the ALU output. Reading from the flag register is also simplified, as the multiplexer efficiently handles the selection and output of the desired flag based on the read operation's requirements.
By segregating the flag register from the main register file, this design approach promotes operational efficiency and maintains a clear and organized structure within the CPU architecture.

Maintaining the multiplier and divider components within the ALU itself minimizes complications, as they are seamlessly integrated as integral components of the ALU, this also directly allows the modification of flags using a multiplexer. ALUOPEX was utilized to prevent alterations to the control unit, eliminating the need to consider the last three most significant bits of the opcode to identify the operation being performed. There is a drawback explained in the limitations/tradeoffs section but that drawback minimises the use of another register which is very expensive.
In the multiplier component, I employed an adder to monitor the out-of-bounds flag, which might initially appear costly but isn't, given that the critical path is determined by the three-layer adder itself. Furthermore, the multiplier gains efficiency through parallel addition, where in the individual layer of adders, each result is independent of others. So computation takes place like (a + b) + (c + d) and not like ((a+b)+c)+d. It's noteworthy that in each adder, the carry input bit is deliberately set to low due to the absence of any expected carry output. This decision aligns with the considerations discussed in the limitations/tradeoffs section, particularly in the context of working with an 8-bit system.
In the divider component, a separate component step_rem_compare was used in which the 1-bit output COMPARE directly corresponds to each bit of the quotient making the circuit much readable. 

# Limitations/ Tradeoffs of my changes
In my design, the multiplication operation is constrained to multiplying two 8-bit numbers, regardless of their sign, to mitigate the risk of overflow and potential storage complications associated with exceeding a 16-bit result. Similarly, division operations are standardized to operate on 8-bit inputs for consistency.

In division, the resulting quotient, reflected in the least significant 8 bits, and the remainder, captured in the most significant 8 bits, are merged and stored within a 16-bit format. Direct input of a negative number triggers the setting of the most significant bit to high, automatically signaling an overflow condition. However, careful handling ensures that when dealing with negative 8-bit numbers (i.e., when the 8 most significant bits are 0), the operation maintains its intended behavior.

Moreover, if a number exceeds 8 bits, the operation processes only the least significant 8 bits for multiplication or division and adjusts the corresponding flag accordingly.

## Testing 
So the program in demo.quac multiplies two numbers stored in R1 and R2. Storing in R1 and R2 is done through the MOVL instruction. Then in the same program, multiplication of negative numbers is demonstrated taking two's complement into account.
