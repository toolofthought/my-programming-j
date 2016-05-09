#### notation for numerals ####
e r j p: 1e3, 2r3, 3j4

p, of the form XpY means X * pi ^ Y 

pi =: 1p1 	twopi =: 2p1 	2p_1
3.14159 	6.28319 	0.63662

Similarly, a numeral written with letter x, of the form XxY means X * e ^ Y where e is the familiar value 2.718281828....

    	e =: 1x1 	2x_1 	2 * e ^ _1
    2.71828 	0.735759 	0.735759

These p and x forms of numeral provide a convenient way of writing constants accurately without writing out many digits. Finally, we can write numerals with a base other than 10. For example the binary or base-2 number with binary digits 101 has the value 5 and can be written as 2b101.

   2b101 
5

The general scheme is that NbDDD.DDD is a numeral in number-base N with digits DDD.DDD . With bases larger than 10, we will need digits larger than 9, so we take letter 'a' as a digit with value 10, 'b' with value 11, and so on up to 'z' with value 35.

For example, letter 'f' has digit-value 15, so in hexadecimal (base 16) the numeral written 16bff has the value 255. The number-base N is given in decimal.

16bff 	(16 * 15) + 15
255 	255

One more example. 10b0.9 is evidently a base-10 number meaning "nine-tenths" and so, in base 20, 20b0.f means "fifteen twentieths"

   10b0.9 20b0.f
0.9 0.75