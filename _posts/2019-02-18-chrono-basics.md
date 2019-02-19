# Chrono

## Overview

I see `std::chrono` being rewritten in various projects. It is better to learn and understand its API than constantly learn new time APIs. The library is broken up into clocks which, provide time points, and durations.

## Clocks

There are basically two types of clocks: steady and system. `steady_clock` is monotonic. `system_clock` usually represents the wall clock time.

Use `steady_clock` to measure durations. It is also appropriate for performance analysis on certain operations.

Use `system_clock` to relate to a time or date for people. It is also useful for distributed systems. When using this clock's time, the programmer consider must consider that the time may go backwards.

The `now()` method returns a `time_point`. The `time_point` type is templated with the clock's type so that one clock's `time_point` will not interoperate by default with another's. `time_point`'s store the duration from a specified epoch. Note that `system_time` often uses [Unix Time](https://en.wikipedia.org/wiki/Unix_time), but the epoch is unspecified.

## C Compatability

`system_clock` also includes to and from `time_t` methods for C compatibility. Below is a reference table with similar C POSIX calls on [some systems](https://github.com/llvm-mirror/libcxx/blob/master/src/chrono.cpp#L79).

Clock | C++ Name | Monotonic | Usually Wall Time | POSIX |
--- | --- | --- | --- |
Steady | `std::chrono::steady_clock` | Yes | No | `clock_gettime(CLOCK_MONOTONIC, &tp)` |
System | `std::chrono::system_clock` | No | Yes | `clock_gettime(CLOCK_REALTIME, &tp)` |

## Duration

A `std::chrono:::duration` represents the difference between two time points. It consists of two parts: a representation and a period. The representation is the underlying type. For example, it could be a `struct`, `int`, or `double`. The value stored in this type is referred to as ticks. The period is the ratio from those ticks to seconds. Using seconds makes sense, because it is an SI unit.

Below is an example of using the clocks to create a duration.
```
const auto before = std::chrono::steady_clock::now();
foo();
const auto delta = std::chrono::steady_clock::now() - before;
const double seconds = std::chrono::duration<double>(delta).count();
std::cout << "foo took " << seconds << "s" << std::endl;
```
Note that `steady_clock` was used for durations, because the duration would always be positive. The `-` operator returned a duration. Then, `std::chrono::duration<double>` converted that representation to a duration with a double representation in seconds. `count()` converts from `duration<double, std::ratio<1>>` to the underlying type, which is just `double`. Double was used because iostream has an overload for it already.

C++14 introduced [`chrono_literals`](https://en.cppreference.com/w/cpp/chrono/operator%22%22ms). These allow a convenient way to type a literal as a duration.

```
using namespace std::chrono_literals;
const auto nanos = 10ns;
const auto micros = 10us;
```

Durations can be used to offset time points as well. See the code below.
```
const std::chrono::system_clock::time_point t1(1s);
const std::chrono::system_clock::time_point t2(t1 + 10ms);
const auto seconds = std::chrono::duration<double>(t2.time_since_epoch())
std::cout << "time_since_epoch = " << seconds.count() << std::end;
```
Here a time_point is constructed manually. Then, it is offset by 10 milliseconds to create a new time_point. Then, the time since the epoch is used to get the duration back out. Next, it is converted to seconds. Finally, it is converted to a double for printing. Note that `system_clock` is used, because the time since epoch value is usually relatable to other systems.

## Truncation And Casting
The various predefined durations are all [signed integers with some guarantee on their minimum bit length](https://en.cppreference.com/w/cpp/chrono/duration). Each predefined duration type covers +/- 292 years. That means going from a seconds to nanoseconds will not truncate, but the other way might.
```
const auto nanos = 100ns;
const auto seconds = std::chrono::duration_cast<std::chrono::seconds>(nanos);
const auto millis = 100ms;
const std::chrono::milliseconds micros = millis;
```
In the code above, `duration_cast` is used so that the compiler knows that the programmer accounted for the risk of truncation. Therefore, going from milliseconds to microseconds did not require a cast.

With chrono, conversion math is unnecessary. See the manual operations done below.
```
const int64_t s_0 = 100;
const int64_t n_0 = s_0 * static_cast<int64_t>(1e9);
const int64_t n_1 = 1234567891011;
const int64_t s_1 = n_1 / static_cast<int64_t>(1e9);
```
Instead, prefer to use the chrono duration types and the appropriate casting.
```
const auto s_0 = 100s;
const std::chrono::nanoseconds n_0 = s_0;
const auto n_1 = 1234567891011ns;
const auto s_1 = std::chrono::duration_cast<std::chrono::seconds>(n_1);
```
The multiplication and division are built into the type converter. Also, remember that `duration_cast` was only needed from nanoseconds to seconds, because of integer truncation.

## Floating Point Precision Error and Integer Overflow

Both integers and floating point types have limitations on the numbers that they can represent. Integers are straightforward, because they can only represent what is in their range. Their range is dependent on the number of bits. If arithmetic exceeds the limit, they overflow.

Floating point is more complicated because its resolution is dependent on its mantissa and the operation performed. Large floating point numbers can overwhelm the smaller number. For example, on some platforms `2e8f + 1.0f == 2e8f` is true.

Below is a table comparing the common representations and periods. For integer types, the seconds, days, and years are the time until a tick causes integer overflow. For floating point types, the seconds, days, and years are the time until the precision for a single tick is lost. This table assumes std::numeric_limits<float>::is_iec559 and std::numeric_limits<double>::is_iec559.

Representation | Period | Tick Duration | Seconds | Days | Years
--- | --- | --- | --- | ---
int32_t | std::ratio<1, 1000> | milliseconds | 2e6 | 25 | 0.07
int32_t | std::ratio<1, 1> | seconds | 2e9 | 2e4 | 68
int64_t | std::ratio<1, 1000000000> | nanoseconds | 9e9 | 1e5 | 292
float | std:ratio<1, 1> | seconds | 2e7 | 194 | 0.5
double | std::ratio<1, 1000000000> | nanoseconds | 5e6 | 52 | 0.1

For certain requirements floating point time representations may be acceptable. [A lot of devices are not designed to be turned on for long periods of time](https://arstechnica.com/information-technology/2015/05/boeing-787-dreamliners-contain-a-potentially-catastrophic-software-bug/). Floating point is easy to use, but is more complicated to account for error. If operations are accumulated, the error may accumulate over time as well. Also, some of the bits in a floating point number may be wasted on unnecessarily large or small number. This waste is common when storing SI units. Chrono makes fixed point arithmetic easier to use. For these reasons, it's best to consider using chrono's integer based durations by default.

## Conclusion

The chrono library follows a lot of good design principles. It clears up a lot of confusion by putting more logic in the type and conversion system. Chrono is not a general purpose units library. It will only handles 0 and 1 exponent units of time. Learning it will lead to good practices.