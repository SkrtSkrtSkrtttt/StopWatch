# StopWatch

Objec/ves:
In this experiment we will:
• Install the 7 segment numeric display and associated drive circuitry
• Install the tacAle switches
• Learn some more coding

Integer division:
The division of an integer by an integer may result in an integer result or a fracAonal
result. Some fracAons can be represented exactly but most must be approximated.
FloaAng point number representaAons take more space, require more Ame to process,
and can have troublesome accumulaAons of rounding errors. For instance may
not be exactly 1 due to rounding errors.
There are two mathemaAcal funcAons that are related to division: integer division
produces an integer quoAent and the modulo funcAon returns the remainder of a
division.
C is a typed language − numbers can be of an integral type (integers in all their various
flavors − char, int, long, etc.), or of a floaAng point type (float, double, and the like), or
other types. If the ‘/‘ division operator is applied with a floaAng point operand, the result
will be a floaAng point number. If, however, the ‘/‘ operator is used with two integral
arguments, the operator will perform integer division.
Integer division means that the dividend is divided by the divisor and any fracAonal part
is simply thrown away. 15/3 is 5 in integer division, but 16/3 and 17/3 are also 5 in
integer division.
Quantity Reference Value Description
⅓ D1 Red Display, Four Digit LED, Red
⅓ D1 Green Display, Four Digit LED, Green
⅓ D1 Yellow Display, Four Digit LED, Yellow
4 Q1-Q4 TRANS PNP 40V 0.2A TO92
3 SW1-SW3 SWITCH TACTILE SPST-NO 0.05A 12V
1
3 × 3
1
ESE 123 Laboratory 9
The modulo funcAon, denoted by the ‘%’ operator ignores the quoAent, but returns the
remainder. 15%3 is 0 because there is no remainder. 16%3 is 1, 17%3 is 2, and 18%3 is
back to 0.
Displaying decimal numbers using integer division and modulo:
If we want to display a binary number in decimal we need to make a series of
conversions. We first need to convert the binary number to a decimal number, and then
we need to look up which segments should be turned on to represent the number’s
glyph (the LED segments are turned on to represent the number). The la\er conversion
is handled by the display_digit funcAon.
If we have a four digit decimal number, say 6837, we can find the first decimal digit by
integer dividing by 1000 to get 6 and displaying the 6 in the thousands digit (digit 1 on
our four digit display). Assuming we had saved 6837 in an integer variable called
disp_int, this would look like:
display_digit (disp_int/1000, 1);
We then use the modulo funcAon to find the remainder − the hundreds, tens, and ones
and save it in the original variable. A_er this operaAon disp_int would contain 837:
disp_int = disp_int % 1000;
Now we can repeat the process for the next digit. We integer divide by 100 to get the
number of hundreds (8) and display this in digit 2:
display_digit (disp_int/100, 2);
And compute the remainder (37) − the hundreds are gone, the tens and ones remain:
disp_int = disp_int % 100;
The tens (3) are displayed next:
display_digit (disp_int/10, 3);
And then the ones (7):
display_digit (disp_int%10, 4);
Note that disp_int starts at 6837, but ends as 37. This display procedure destroys the
original value. If we wanted to retain the original value, we would copy it into a
temporary (scratch) variable.
Checking bu>ons:
Looking at the schemaAc diagram at the end of this document shows three bu\ons SW1
(labeled UP), SW2 (OK), and SW3 (DOWN). These are all connected to PORTE of the
microcontroller. We have set up the microcontroller to make these inputs with a weak
pull-up. This means that there is a relaAvely large value resistor connected internally to
2
ESE 123 Laboratory 9
+5V in the microcontroller so that the input will read as a logical 1 if nothing else is going
on.
However, pushing the bu\ons can change that. The bu\ons are configured to short the
input to ground thus forcing the input to a logical 0.
The switches are connected to the three least significant bits of PORTE: PE2 (DOWN),
PE1 (OK), and PE0 (UP). We wish to check the condiAon of one switch at a Ame and
ignore the other bits of PORTE.
A two input AND logical funcAon takes two bits in and generates one bit out. The output
bit is TRUE (logical 1) if both of the inputs are TRUE. If any input is FALSE (logical 0) the
output is FALSE.
The arithmeAc logic unit (ALU) on our chip is 8 bits wide. That means that it contains 8
separate AND gates. Each AND gate takes one input from one operand and the other
from the corresponding bit of the other operand.
Say we want to see if the UP bu\on is pressed. This is connected to PE0 − the least
significant bit of PORTE. If it is pressed PE0 will be a logical 0. If it is not pressed PE0 will
be a logical 1.
Say the bu\on is pressed (PORTE_IN bit 0 is low) and we perform this AND operaAon:
PORTE_IN 0b11011110
AND 0b00000001
0b00000000
The result is zero. Suppose the bu\on is the released. Now PORTE_IN bit 0 will be high
and we will get:
PORTE_IN 0b11011111
AND 0b00000001
0b00000001
This is called masking and it allows us to use the AND operaAon to ignore everything
except for one bit. If we were interested in isolaAng the OK bu\on (PORTE_IN bit 1) we
would AND PORTE_IN with 0b00000010 and the result would strictly depend on the OK
bu\on − we would get 0b00000000 if the bu\on was pressed and 0b00000010 if not...
