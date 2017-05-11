# MATH.H

 | Include    | #include `<math.h>`                                                                                              |                                   
 | -----------------------------------------------------------------------------------------------------------------------------                                   
 | Header     | [{z88dk}/include/_DEVELOPMENT/sccz80/math.h](https://raw.githubusercontent.com/z88dk/z88dk/master/include/_DEVELOPMENT/sccz80/math.h) |             
 | | [{z88dk}/include/_DEVELOPMENT/sdcc/math.h](https://raw.githubusercontent.com/z88dk/z88dk/master/include/_DEVELOPMENT/sdcc/math.h) |                            
 | Source     | [{z88dk}/libsrc/_DEVELOPMENT/math/float](https://raw.githubusercontent.com/z88dk/z88dk/master/libsrc/_DEVELOPMENT/math/float/)                     |

.... not all math libs supply all functions ....

# CLASSIFICATION

## int fpclassify(real-floating x)

## int isfinite(real-floating x)

## int isinf(real-floating x)

## int isnan(real-floating x)

## int isnormal(real-floating x)

## int signbit(real-floating x)

# TRIGONOMETRIC FUNCTIONS

## double acos(double x), float acosf(float x)

## double asin(double x), float asinf(float x)

## double atan(double x), float atanf(float x)

## double atan2(double y, double x), float atan2f(float y, float x)

## double cos(double x), float cosf(float x)

## double sin(double x), float sinf(float x)

## double tan(double x), float tanf(float x)

# HYPERBOLIC FUNCTIONS

## double acosh(double x), float acoshf(float x)

## double asinh(double x), float asinhf(float x)

## double atanh(double x), float atanhf(float x)

## double cosh(double x), float coshf(float x)

## double sinh(double x), float sinhf(float x)

## double tanh(double x), float tanhf(float x)

# EXPONENTIAL AND LOGARITHMIC FUNCTIONS

## double exp(double x), float expf(float x)

## double exp2(double x), float exp2f(float x)

## double expm1(double x), float expm1f(float x)

## double frexp(double value, int *exp), float frexpf(float value, int *exp)

## int ilogb(double x), int ilogbf(float x)

## double ldexp(double x, int exp), float ldexpf(float x, int exp)

## double log(double x), float logf(float x)

## double log10(double x), float log10f(float x)

## double log1p(double x), float log1pf(float x)

## double log2(double x), float log2f(float x)

## double logb(double x), float logbf(float x)

## double modf(double value, double *iptr), float modff(float value, float *iptr)

## double scalbn(double x, int n), float scalbnf(float x, int n)

# POWER AND ABSOLUTE VALUE FUNCTIONS

## double cbrt(double x), float cbrtf(float x)

## double fabs(double x), float fabsf(float x)

## double hypot(double x, double y), float hypotf(float x, float y)

## double pow(double x, double y), float powf(float x, float y)

## double sqrt(double x), float sqrtf(float x)

# ERROR AND GAMMA FUNCTIONS

## double erf(double x), float erff(float x)

## double erfc(double x), float erfcf(float x)

## double lgamma(double x), float lgammaf(float x)

## double tgamma(double x), float tgammaf(float x)

# NEAREST INTEGER FUNCTIONS

## double ceil(double x), float ceilf(float x)

## double floor(double x), float floorf(float x)

## double nearbyint(double x), float nearbyintf(float x)

## double rint(double x), float rintf(float x)

## long lrint(double x), long lrintf(float x)

## double round(double x), float roundf(float x)

## long lround(double x), long lroundf(float x)

## double trunc(double x), float truncf(float x)

# REMAINDER FUNCTIONS

## double fmod(double x, double y), float fmodf(float x, float y)

## double remainder(double x, double y), float remainderf(float x, float y)

## double remquo(double x, double y, int *quo), float remquof(float x, float y, int *quo)

# MANIPULATION FUNCTIONS

## double copysign(double x, double y), float copysignf(float x, float y)

## double nan(const char *tagp), float nanf(const char *tagp)

## double nextafter(double x, double y), float nextafterf(float x, float y)

## double nexttoward(double x, long double y), float nexttowardf(float x, long double y)

# MAXIMUM, MINIMUM AND POSITIVE DIFFERENCE FUNCTIONS

## double fdim(double x, double y), float fdimf(float x, float y)

## double fmax(double x, double y), float fmaxf(float x, float y)

## double fmin(double x, double y), float fminf(float x, float y)

# FLOATING MULTIPLY-ADD

## double fma(double x, double y, double z), float fmaf(float x, float y, float z)

# COMPARISONS

## int isgreater(real-floating x, real-floating y)

## int isgreaterequal(real-floating x, real-floating y)

## int isless(real-floating x, real-floating y)

## int islessequal(real-floating x, real-floating y)

## int islessgreater(real-floating x, real-floating y)

## int isunordered(real-floating x, real-floating y)

