# Report 

<!-- Your text goes here. Remember to check the result of your CI to see whether 
the final PDF rendered correctly! -->

## Overview
Enhanced with capabilities for multiplication, division of both positive and negative numbers along with flag management, my Extended Single Cycle CPU offers advanced computational functionalities. Maintaining simplicity, my CPU retains the base ISA by leveraging the 4th and 8th bits of the ALU opcode.
The following table shows the syntax of MULTIPLICATION and DIVISION: 

| EXTENSION | DESCRIPTION            | SYNTAX          |
|-----------|------------------------|-----------------|
| MULTIPLY  | multiplies two numbers | mult rd, ra, rb |
| DIVIDE    | divides two numbers    | div rd, ra, rb  |

## ISA Changes

## Microarchitecture Changes
A two-bit control signal ALUOPEX was added to the control unit allowing for the extensions. A corresponding input signal was added in the ALU too allowing the ALU to distinguish. I included an additional UNDEF 1-bit output in the ALU to indicate when a number is divided by zero and also used it as a flag. The ALU underwent significant changes with the integration of multiplication and division functionalities. I added multiplier and divider components to the ALU and integrated with the ALUOPEX signal, to distinguish between multiplication, division, and other ALU operations while keeping the 2-bit ALUOP signal intact. ALUOPEX is used as a selector bit in a multiplexer. The flags of multiplication and division were integrated with the previous flags using a mulitplexer again. 

## Analysis and Tradeoffs
# Key benefits of my changes
Maintaining the multiplier and divider components within the ALU itself minimizes complications, as they are seamlessly integrated as integral components of the ALU, this also directly allows the modification of flags using a multiplexer. ALUOPEX was utilized to prevent alterations to the control unit, eliminating the need to consider the last three most significant bits of the opcode to identify the operation being performed. There is a drawback explained in the next point but that drawback minimises the use of another register which is very expensive. 

# Limitations/ Tradeoffs of my changes
The mulitiplication in my case is capable of multiplying only two 8-bit numbers (both positive and negative) generating an overflow flag otherwise. This has been done so that the result does not exceed 16-bit which would result in storing issues. To keep division in line with multiuplication, 8-bits are taken into consideration. The quotient (least significant 8-bits) and remainder (most signicant 8-bits) are therefore both combined and stored in a 16-bit result. If a negative number is input directly, it sets the most significant bit to high leading to set the overflow flag (this is by default). 

## Testing 

