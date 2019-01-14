# awesome-bits [![Awesome](https://cdn.rawgit.com/sindresorhus/awesome/d7305f38d29fed78fa85652e3a63e154dd8e8829/media/badge.svg)](https://github.com/sindresorhus/awesome)

> A curated list of awesome bitwise operations and tricks
>
> Original - [Keon Kim](https://github.com/keonkim)

> Java version by [shenhaizhilong](https://github.com/shenhaizhilong)


## Integers
**Set n<sup>th</sup> bit**
```
x | (1<<n)
```
**Unset n<sup>th</sup> bit**
 ```
 x & ~(1<<n)
 ```
**Toggle n<sup>th</sup> bit**
```
x ^ (1<<n)
```
**Round up to the next power of two**
```
 int v; //only works if v is 32 bit
v--;
v |= v >> 1;
v |= v >> 2;
v |= v >> 4;
v |= v >> 8;
v |= v >> 16;
v++;
```

**Round up to the pre power of two/ highest one bit**
```
 int v; // only works if v is 32 bits

v |= v >> 1;
v |= v >> 2;
v |= v >> 4;
v |= v >> 8;
v |= v >> 16;
return v - (v >>> 1);
```

**Get the maximum integer**
```
int maxInt = ~(1 << 31);
int maxInt = (1 << 31) - 1;
int maxInt = (1 << -1) - 1;
```
**Get the minimum integer**
```
int minInt = 1 << 31;
int minInt = 1 << -1;
```
**Get the maximum long**
```
long maxLong = ((long)1 << 63) - 1;
```
**Multiply by 2**
```
n << 1; // n*2
```
**Divide by 2**
```
n >>> 1; // n/2
```
**Multiply by the m<sup>th</sup> power of 2**
```
n << m;
```
**Divide by the m<sup>th</sup> power of 2**
```
n >>> m;
```
**Check Equality**

<sub>*This is 35% faster in Javascript*</sub>
```
(a^b) == 0; // a == b
!(a^b) // use in an if
```
**Check if a number is odd**
```
(n & 1) == 1;
```
**Exchange (swap) two values**
```
//version 1
a ^= b;
b ^= a;
a ^= b;

//version 2
a = a ^ b ^ (b = a)
```
**Get the absolute value**
```
//version 1
x < 0 ? -x : x;

//version 2
(x ^ (x >> 31)) - (x >> 31);
```
**Get the max of two values**
```
b & ((a-b) >> 31) | a & (~(a-b) >> 31);
```
**Get the min of two values**
```
a & ((a-b) >> 31) | b & (~(a-b) >> 31);
```
**Check whether both numbers have the same sign**
```
(x ^ y) >= 0;
```
**Flip**
```
i = ~i; or i = i ^ -1;

```
**Flip the sign**
```
i = ~i + 1; // or
i = (i ^ -1) + 1; // i = -i
```
**Calculate 2<sup>n</sup>**
```
1 << n;
```
**Whether a number is power of 2**
```
n & (n - 1) == 0;
```
**Modulo 2<sup>n</sup> against m**
```
m & ((1 << n) - 1);
```
**Get the average**
```
(x + y) >> 1;
((x ^ y) >> 1) + (x & y);
```
**Get the m<sup>th</sup> bit of n (from low to high)**
```
(n >> (m-1)) & 1;
```
**Set the m<sup>th</sup> bit of n to 0 (from low to high)**
```
n & ~(1 << (m-1));
```
**Check if n<sup>th</sup> bit is set**
```
if (x & (1<<n)) {
  n-th bit is set
} else {
  n-th bit is not set
}
```
**Isolate (extract) the right-most 1 bit/lowest one bit**
```
x & (-x)
```
**Isolate (extract) the right-most 0 bit**
```
~x & (x+1)
```

**Set the right-most 0 bit to 1**
```
x | (x+1)
```

**n + 1**
```
since: -x = ~x +1; 
==> -x -1 = ~x ;
==> x +1 = -~x;

-~n
```
**n - 1**
```
-x = ~x +1;   -x -1 = ~x; replace x with -x; then  x -1 = ~(-x)
~-n
```
**Get the negative value of a number**
```
~n + 1;
(n ^ -1) + 1;
```
**`if (x == a) x = b; if (x == b) x = a;`**
```
x = a ^ b ^ x;
```
**Swap Adjacent bits**
```
((n & 10101010) >> 1) | ((n & 01010101) << 1)
```
**Different rightmost bit of numbers m & n**
```
(n^m)&-(n^m) // returns 2^x where x is the position of the differnet bit (0 based)
```
**Common rightmost bit of numbers m & n**
```
~(n^m)&(n^m)+1 // returns 2^x where x is the position of the common bit (0 based)
```
## Bit Count for int and long 
```

/**
     * https://stackoverflow.com/questions/22081738/how-does-this-algorithm-to-count-the-number-of-set-bits-in-a-32-bit-integer-work
     * https://codingforspeed.com/a-faster-approach-to-count-set-bits-in-a-32-bit-integer/
     *
     * @param n
     * @return
     */
 public static int bitCount(int n) {


        if (n == 0) return 0;
        int[] masks = {0x55555555, 0x33333333, 0x0f0f0f0f, 0x00ff00ff, 0x0000ffff};
        n = (n & masks[0]) + ((n >>> 1) & masks[0]);
        n = (n & masks[1]) + ((n >>> 2) & masks[1]);
        n = (n & masks[2]) + ((n >>> 4) & masks[2]);
        n = (n & masks[3]) + ((n >>> 8) & masks[3]);
        n = (n & masks[4]) + ((n >>> 16) & masks[4]);
        return n & 0x3f;
    }

    public static int bitCount2(int n) {

        int r = 0;
        while (n != 0)
        {
            r++;
            n = n&(n-1);
        }
        return r;
    }

    public static int bitCount2(long n) {

        int r = 0;
        while (n != 0)
        {
            r++;
            n = n&(n-1L);
        }
        return r;
    }
	
```
** Is Same sign**
```
public static boolean isSameSign(int a, int  b)
    {
        return (a^b)>=0;
    }
```

## Additional Resources

* [Bit Twiddling Hacks](https://graphics.stanford.edu/~seander/bithacks.html)
* [Floating Point Hacks](https://github.com/leegao/float-hacks)
* [Hacker's Delight](http://www.hackersdelight.org/)
* [The Bit Twiddler](http://bits.stephan-brumme.com/)
