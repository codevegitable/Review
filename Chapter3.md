9<script type="text/javascript" async
  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.7/MathJax.js?config=TeX-MML-AM_CHTML">
</script>
# Chapter 3
## Addition and Subtraction
Two number with different signs can not occur overflow while operating addition.

Two number with same signs can not occur overflow while operating subtraction.

When overflow occurs in unsigned number, overflow is often ignored.

How to detect? When result is different from the actual condition, then overflow occurs.
> For addition, two positive number's addition result is negative, or vice versa; for subtraction, positive number subtracting negative number get a negative number, or vice versa. 

ALU的实现懒得说了，直接看别人的吧(): <src href="https://xuan-insr.github.io/computer_organization/3_arithmetic/#311-1-bit-alu">https://xuan-insr.github.io/computer_organization/3_arithmetic/#311-1-bit-alu<src>

## Multiplication
懒得写了，做到这实验也做完了，应该会了()

> + Signed multiplication:
> First convert the multiplier and multiplicant into positive number and remember their sign, multiply the left 31 bits and finally calculate the sign.
> + Faster multiplication:
> As Moore's Law, the resources are enough to build faster multiplication hardware. It's possible to just add one bit use 64-bit ALU using 64 ALUs.(有点像超前进位加法器) Reflecting the pipelining.

### Booth's Algorithm
Divide a string of '1' to two strings that just exist one bit '1'.

> for example: 00111100 = 01000000 - 00000100

The advantage is that when we multiply two numbers, addition always take place in the condition that the bit '1'. So less 1 will result in less calculate.

剩下的具体操作看别人的吧：<src href="https://xuan-insr.github.io/computer_organization/3_arithmetic/#booths-algorithm">https://xuan-insr.github.io/computer_organization/3_arithmetic/#booths-algorithm<src>。简单来说就是：detect from right to left, while '10' subtract from left; while '01' add left.
> 10111100: 10111**10**0 - 00000100 -> (1**01**11100 + 00000100) - 00000100 = 11000000 - 00000100

## Division
没啥可说的，把被除数放余数寄存器里然后让除数和高32位相减，小于零不变，大于零高32位是减法结果，然后左移一位。结果高32位是余数，低32位是商。和乘法差不多。
> (闲的没事的翻译) put the divisor in the remainder register and then have the divisor and the high 32 bits subtracted, less than zero unchanged, greater than zero the high 32 bits are the result of the subtraction, and then shifted one bit to the left. The result is the remainder in the upper 32 bits and the quotient in the lower 32 bits. It's similar to multiplication.

## Floating Point
Use IEEE 754 to present the floating point number. A floating number is the format of $(-1)^S \times F \times 2^E$. And use a 32-bit register to explain:
> + single precision: sign(1-bit), exponent(8-bit), fraction(23-bit)
> + double precision: sign(1-bit), exponent(11-bit), fraction(20-bit)

The 'exponent' is the number E, it is the precision of a floating point number. The 'fraction' is the number F, it is the actual number in the floating point number.

For 'F', it should be the format '1.xxxxxxx'. Actually, F hide the bit '1' in front of the point, so the fraction can store more bits. The more bits in the fraction, the higher the precision of the floating-point number.

For 'E', it is actually the result of 'exponent - 127'(for single precision) or 'exponent - 1023'(for double precision). The more bits in the exponent, the greater the range of the floating-point number that can be stored.

So except the overflow, floating point number's calculation also exists underflow. It means the least range floating number can store. Overflow and underflow all depends on the exponent.

> some special conditions:
> + exponent = 0, fraction = 0: 0
> + exponent = 0, fraction = nonzero: denormalized number(非规格数)
> + exponent = 255 / 2047, fraction = 0: infinity(无穷)
> + exponent = 255 / 2047, fraction = nonzero: none(不是正常的数字), for example: 0/0, inf/inf

### Addition
实验课都做了，阶数向大的对齐，尾数右移，然后尾数相加，最后规格化。

### Multiplication
$\text{new exponent} = exponent_1 + exponent_2 - bias$

$\text{new fraction} = fraction_1 \times fraction_2$ (don't forget hidden bit)

$\text{new sign} = sign_1 \oplus sign_2$

### Accurate Arithmetic
