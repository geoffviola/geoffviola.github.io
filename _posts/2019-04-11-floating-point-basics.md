# Overview
C and C++ maintain a very minimal definition for what they call `float` and `double`. The [C++20](https://github.com/cplusplus/draft/raw/master/papers/n4810.pdf) draft doesn't go into a formal definition, but the [C18](https://web.archive.org/web/20181230041359if_/http://www.open-std.org/jtc1/sc22/wg14/www/abq/c17_updated_proposed_fdis.pdf#page=41) draft does. There is a standard for floating point types. It's IEEE 754 or IEC 60559. But C and C++ do not guarantee conformance to either standard. The C18 standard formalizes around the basics to be flexible about functionality and speed.

Unlike popular belief, `float` is not guaranteed to be a certain size. The [C18](https://web.archive.org/web/20181230041359if_/http://www.open-std.org/jtc1/sc22/wg14/www/abq/c17_updated_proposed_fdis.pdf#page=50) standard guarantees that
> The set of values of the type float is a subset of the set of values of the type double; the set of values of the type double is a subset of the set of values of the type long double.

In programmer terms, this roughly means
```
sizeof(float) <= sizeof(double) && sizeof(double) <= sizeof(long double)
```

It just so happens that the major compilers follow [similar guidelines on the same platforms](https://godbolt.org/#z:OYLghAFBqd5TKALEBjA9gEwKYFFMCWALugE4A0BIEAZugHZEDKqAhgDbYgCMAdAAwB2ACwBOAKzCATAGZR8haIAc5AFY9y7VvVCoApFIBCBw%2BQDO6AK6lUXAOQGZBeqnaWcAaj0yAwqjNEmOwEAEbeuHr8AIKOzq7u2F6%2BBOgBpNisALbhkTHR2AAeRNik9B4AbugEmB407OisRAD6BGZNwk0hAJ7FZhAAlN7G%2BUUlZZXVHphWIZwtbUqdPdh9gzLDUYXFpRVVNfU6TdOWs9jzTYvdvQNDuVtju5MHwEczc61N3ABsS9drG3dRjsJjU6g1mh8CNhUOJxKIbutAdtxnspm8zpDobD4f8kQ8QR5nq8Tu82lCYXCEQDos4iB5MqxnAMvIINh52R4CDQPBBhEkACLefkeMwEABe2HQNFo9Ua/X6LLZHOVYMa5w6VxWVNyyr0gkF0WVXJ5SgFQpF4sl0uOp3lip1yo5NtJF1%2BWtxho5eoNUSN3IgpqF5tFEqlEGeaJJ2DteqVjsJDBezoxCzdq1unvZ3od7ONEG%2BZpkwpDVvDicjtoVsZzjqJyfO3zT2szLJ9NbzAUwIBA9EsmRKBFQTWCmWIZm8PlVRHC3cxFNEVdZNZVsohZKxlI9vq9%2Bvb/s73d7/dIg%2BHBFHRHHvmTM5Ac%2Bxi7jjvr983Ge3Wd3LY7gUPfYHQ4jmOE4RjeMi4LO67zo%2By4cnW6LnOS2LNh%2BrY1jW6RENYZS4AAGgAkgAKk0TAAKo%2BD4uBMEw77enY/SaCAdjiHY5D0Ex/CsegTE%2BCYJgilYNiJI43CsUQHH0QxADWIDiPwjF2MIrGZLJ8nsXYnHkNxdisWYIDyeJGn0eQcCwCgGCZAADgQnAUFQEAWdZtloFoOhKPw8k0DZ2x6RAIQSeQITOKwpBdExonkBZ/aMAA8vQ7BhUZ5A4AyOicAFhDpKgRAEOUKwBYU0KWMU4WsbS2DsAFwQhKQIVdD4WD2BFRAnipdiiQxrm6HxRiaKEemQAx6CWTlDB6XYAC0nZCvoRgmNwggeBNMUyLpgm2DwnVMSxbEBdpBRKF8E1fHyrjaMAHhKAIAg8vgxBkF4sjcOQHgNVZNklI9MgyAqvFzUYYkSfK5BIBkOCkNQW2Kcpqm7Ul2m6fp5CGZxwMyXJCmrXDmkI8jQMmYgpnIGg6DvbZlDUI5H0Q8ASgyOQXnsD51D%2BUlQX0HVpWRaT0VEHFCUZdgqXAOlSWZdCOV5eNmmFagxVNWVjAVVVoS1aFDU4FzLXnqVnXnWgPWmNVA0DFpI0pPQ41TYEM2GwtS0rWt1gbdwUM7epONMQdR0nR4wCoKgl28DIt2ECQpBfc9r2k05n0ib9huA0ZwOg6w4OQ%2BQ6PyZV0PkCpwh8FIghfFIoj8F8Mj8KIgjiNwwjY1xTGIwZ%2BNExA5kx9TFMOZ3znlKglmWU05TcKITRSPwTQFCdDPeSUvms5p7Oc%2B1rFRdgsXxYlmkpedos7wQWWS/lSWy/LXPlTnmnVWr9WNVrrW65o%2BuzcYvXG/AQ3m2NIAAPQxVIcw60uCuwUu7PaXtfaZDMP3Coo9eAT1DvdCOIkXpvVjig2QUgE7/UMEnVGDFU7p1NjnJSedZJXUELIQQVcZBfDLlIYQh0G5aSbuYJGKNJKZ1hjnLGHtG46TxsnKGUhWL8NYYIzhwM8qkFFAwEAwggA). GCC, clang, and MSVC decide to use 32 bits and 64 bits for `float` and `double` respectively, when compiling for x86-64. There is an exception where  clang and GCC use 128 bits for `long double`, but MSVC uses 64 bits. Aside from that discrepancy, they all follow IEC 60559 by default as well.

# Rounding Error
When an arithmetic operation acts on two floating point values, rounding may occur. The rounding mode is [implementation defined](https://en.cppreference.com/w/cpp/types/climits/FLT_ROUNDS).

```
std::numeric_limits<float>::max() + std::numeric_limits<float>::epsilon();
```

The code above will round a value based on the rounding mode. On clang and x86\_64 and Mac OS, it returns the same value of `std::numeric_limits<float>::max()`. This rounding direction occurs, because the implementation specifies to round to the nearest value by default. More specifically, `FLT_ROUNDS` is set to `FE_TONEAREST`.

```
16777216.0f + 1.0f
```

The code above is intentionally on the edge of what a 32 bit IEC 60559 can represent. On some platforms the result of the addition is just 16777216.

Because of rounding errors, the order of operations can affect precision. Therefore, algebra's distributive rule and associative rule needs to be applied with care. It is generally a good practice to operate on high magnitude values separately from low magnitude values [before operating on them together](https://youtu.be/k12BJGSc2Nc?t=1415).

# Overflow
Overflow occurs when an operation goes beyond what the underlying data structure supports [without extraordinary roundoff error](https://web.archive.org/web/20181230041359if_/http://www.open-std.org/jtc1/sc22/wg14/www/abq/c17_updated_proposed_fdis.pdf#page=190). In the case of floating point, this can be done by doing arithmetic past the precision. The value will be `HUGE_VAL` or some variant based on the type.

```
std::numeric_limits<float>::max() * std::numeric_limits<float>::max();
```

In the example above, the result will overflow and return `HUGE_VALF`. On clang and x86\_64 and Mac OS, it returns infinity.

# Underflow
Underflow occurs when an operation goes below what the underlying data structure supports without extraordinary roundoff error. The result will have a magnitude less than or equal to the [smallest normalized positive number](https://web.archive.org/web/20181230041359if_/http://www.open-std.org/jtc1/sc22/wg14/www/abq/c17_updated_proposed_fdis.pdf#page=190) for that type.

```
std::cout << std::numeric_limits<float>::min() / 3.0f
```

The code above will cause underflow. `std::numeric_limits<float>::min()` returns the [minimum positive normalized value](https://en.cppreference.com/w/cpp/types/numeric_limits/min). After dividing that value by 3, the C18 standard states that the result must be in [0, `std::numeric_limits<float>::min()`]. On clang and x86\_64 and Mac OS, `std::numeric_limits<float>::min()` is 1.17549e-38 and the result is 3.91831e-39. This result is [expected](https://stackoverflow.com/a/17611400) for IEC 60559 conforming values.

# Optimization
Some of the implementation based behavior is configured based on flags. Most compilers have some form of fast math option that trades off speed for precision. Surprisingly, even with all the floating point operations, the compiler deems all the floating point types as [IEC 60559 compliant](https://godbolt.org/#z:OYLghAFBqd5TKALEBjA9gEwKYFFMCWALugE4A0BIEAZugHZEDKqAhgDbYgCMAdAAwB2ACwBOAKzCATAGZR8haIAc5AFY9y7VvVCoApFIBCBw%2BQDO6AK6lUXAOQGZBeqnaWcAaj0yAwqjNEmOwEAEbeuHr8AIKOzq7u2F6%2BBOgBpNisALbhkTHR2AAeRNik9B4AbugEmB407OisRAD6BGZNwk0hAJ7FZhAAlN7G%2BUUlZZXVHphWIZwtbUqdPdh9gzLDUYXFpRVVNfU6TdOWs9jzTYvdvQNDuVtju5MHwEczc61N3ABsS9drG3dRjsJjU6g1mh8CNhUOJxKIbutAdtxnspm8zpDobD4f8kQ8QR5nq8Tu82lCYXCEQDos4iB5MqxnAMvIINh52R4CDQPBBhEkACLefkeMwEABe2HQNFo9Ua/X6LLZHOVYMa5w6VxWVNyyr0gkF0WVXJ5SgFQpF4sl0uOp3lip1yo5NtJF1%2BWtxho5eoNUSN3IgpqF5tFEqlEGeaJJ2DteqVjsJDBezoxCzdq1unvZ3od7ONEG%2BZpkwpDVvDicjtoVsZzjqJyfO3zT2szLJ9NbzAUwIBA9EsmRKBFQTWCmWIZm8PlVRHC3cxFNEVdZNZVsohZKxlI9vq9%2Bvb/s73d7/dIg%2BHBFHRHHvmTM5Ac%2Bxi7jjvr983Ge3Wd3LY7gUPfYHQ4jmOE4RjeMi4LO67zo%2By4cnW6LnOS2LNh%2BrY1jW6RENYZS4AAGgAkgAKk0TAAKo%2BD4uBMEw77enY/SaCAdjiHY5D0Ex/CsegTE%2BCYJgilYNiJI43CsUQHH0QxADWIDiPwjF2MIrGZLJ8nsXYnHkNxdisWYIDyeJGn0eQcCwCgGCZAADgQnAUFQEAWdZtloFoOhKPw8k0DZ2x6RAIQSeQITOKwpBdExonkBZ/aMAA8vQ7BhUZ5A4AyOicAFhDpKgRAEOUKwBYU0KWMU4WsbS2DsAFwQhKQIVdD4WD2BFRAnipdiiQxrm6HxRiaKEemQAx6CWTlDB6XYAC0nZCvoRgmNwggeBNMUyEtNA0KwAQTQyRBILpgm2DwnVMSxbEBdpBRKF8E1fHyrjaMAHhKAIAg8vgxBkF4sjcOQHgNVZNklF9MjcAqvFzUYYkSfK5BIBkOCkNQx2KcpqlnUl2m6fp5CGZxMMyXJCkyKx6maZjOPQyZiCmcgaDoADtmUNQjmA4jwBKDI5BeewPnUP5SVBfQdWlZF9PRUQcUJRl2CpcA6VJZl0I5Xl42aYVqDFU1ZWMBVVWhLVoUNTgIsteepWdQ9aA9aY1UDQMWkjSk9DjVNgQzdbC1rfQ6ATTQlkTbSE0hJY3nOAH9CFKw2VrS1rCWdZOjbY0SBLTFG0BPt1iHaDCmnaTXFMZd123R4wCoKgT28KtEDvSQpDAz9f3005QOOFIYPW1DRkw3DrAI0j5AE/JlUo%2BQKnCHwUiCF8UiiPwXwyPwoiCOI3DCOjZNMVjBmUzTEDmc3rNMw5h/OeUqDx005TcKITRSPwTQFLdXPeSUvn85pgvC%2B1rFRdgsXxUSppFKD15bAIIFlZW%2BUkrq01iLcqI9NLVQNvVRqJtWrm00JbWaxheq23gENR2Y0mIAHoYpSA8CQv2IB05EEzkJI6ucSbnULiXTIZhz4VBvrwe%2Bb1CB1wbr9f6Ld66OBkB3CGhgu54wYr3fu9sR5KTHrJZ6ghZCCCXjIL4c8pDCCuhvAuOlzDY1xpJQeaMR7EwMVpLeFNu7IykMwjGtjTEwzyqQUUDAQDCCAA%3D). It's recommended to have unit tests in place to verify that any inaccuracies from these flags do not affect functionality.

Reducing the cache footprint can also increase speeds. For example, moving from a double to a float on a large dataset may half memory usage. Cache improvements could have a bigger impact than changing flags.

I'd recommend a bool or `std::optional` over Nan to signify that a floating point value is populated. Nan should be used to signify that the value is not populated to optimize for space. Those cases should be documented well. For example, ROS's [REP 117](http://www.ros.org/reps/rep-0117.html#abstract) uses nan to represent a bad measurement.

# Floating Point Exceptions

Floating point exceptions are meant to catch errors where they occur. C++11 standardized a few functions for setting [environment](https://en.cppreference.com/w/cpp/header/cfenv) bits and viewing them, after an error occurs. I prefer GCC's actual exceptions, because it fails at the lines specified. They can be enabled with [feenableexcept](https://linux.die.net/man/3/feenableexcept). On Mac or with clang, it may require implementing it. There is an implementation from [ardupilot](https://github.com/ArduPilot/ardupilot/blob/master/libraries/AP_Common/missing/fenv.h#L10).

```
// FE_INEXACT
std::numeric_limits<float>::max() + std::numeric_limits<float>::epsilon();
16777216.0f + 1.0f;

// FE_INEXACT | FE_OVERFLOW
std::numeric_limits<float>::max() * std::numeric_limits<float>::max();

// FE_UNDERFLOW
std::numeric_limits<float>::min() / 3.0f;
```

The code above will throw floating point exceptions depending on what flags are set in `feenableexcept`. Note that for `std::numeric_limits<float>::max() * std::numeric_limits<float>::max()` either `FE_INEXACT` or `FE_OVERFLOW` will enable a runtime exception when executed. It might be a good idea to run `feenableexcept(FE_ALL_EXCEPT)` and see what breaks.

# Comparisons

Equality on floats is tricky. Generally, an error margin is used for robustness. This error margin is often referred to as "epsilon" in mathematics. Sometimes C++'s `std::numeric_limits<T>::epsilon()` is appropriate. Sometimes some other small value is appropriate. It depends on the application. See the example below.

```
const bool close = std::fabs(a - b) < 1e-6;
```

The code above computes a difference between `a` and `b`. Then, it takes an absolute value and compares it to `1e-6`, which is our "epsilon."

Floating point values in a for loop have various issues. At each iteration, the floating point value may accumulate rounding errors. When the floating point value accumulates error, the comparison operation may be incorrect. Therefore, it is not recommended to use floating point values in a for loop. Clang analyzer checks for [this issue](https://clang-analyzer.llvm.org/available_checks.html#security_checkers).

Since there are so many tricky scenarios, some resort to integer and fixed point representations. For example, the [clipper](http://www.angusj.com/delphi/clipper.php) library uses integers to calculate various polygon operations.
