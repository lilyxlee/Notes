# Notes
TechNotes
1. What is the difference between the float and integer data type when the size is the same?
float stores floating-point values, that is, values that have potential decimal places
int only stores integral values, that is, whole numbers
So while both are 32 bits wide, their use (and representation) is quite different. You cannot store 3.141 in an integer, but you can in a float.

Dissecting them both a little further:

In an integer, all bits are used to store the number value. This is (in Java and many computers too) done in the so-called two's complement. This basically means that you can represent the values of −231 to 231 − 1.

In a float, those 32 bits are divided between three distinct parts: The sign bit, the exponent and the mantissa. They are laid out as follows:

S EEEEEEEE MMMMMMMMMMMMMMMMMMMMMMM
There is a single bit that determines whether the number is negative or non-negative (zero is neither positive nor negative, but has the sign bit set to zero). Then there are eight bits of an exponent and 23 bits of mantissa. To get a useful number from that, (roughly) the following calculation is performed:

M × 2E

(There is more to it, but this should suffice for the purpose of this discussion)

The mantissa is in essence not much more than a 24-bit integer number. This gets multiplied by 2 to the power of the exponent part, which, roughly, is a number between −128 and 127.

Therefore you can accurately represent all numbers that would fit in a 24-bit integer but the numeric range is also much greater as larger exponents allow for larger values. For example, the maximum value for a float is around 3.4 × 1038 whereas int only allows values up to 2.1 × 109.

But that also means, since 32 bits only have 4.2 × 109 different states (which are all used to represent the values int can store), that at the larger end of float's numeric range the numbers are spaced wider apart (since there cannot be more unique float numbers than there are unique int numbers). You cannot represent some numbers exactly, then. For example, the number 2 × 1012 has a representation in float of 1,999,999,991,808. That might be close to 2,000,000,000,000 but it's not exact. Likewise, adding 1 to that number does not change it because 1 is too small to make a difference in the larger scales float is using there.

Similarly, you can also represent very small numbers (between 0 and 1) in a float but regardless of whether the numbers are very large or very small, float only has a precision of around 6 or 7 decimal digits. If you have large numbers those digits are at the start of the number (e.g. 4.51534 × 1035, which is nothing more than 451534 follows by 30 zeroes – and float cannot tell anything useful about whether those 30 digits are actually zeroes or something else), for very small numbers (e.g. 3.14159 × 10−27) they are at the far end of the number, way beyond the starting digits of 0.0000...
