# Introduction
There's an interesting video on finding the [average shadow of a cube](https://www.youtube.com/watch?v=ltLUadnCyi0). The video has a simple problem, a brute force solution, and finally an elegant closed form solution. Let's find a problem with a similar situation and work through the steps.

# Problem
Say that we are starting a collection and want at least one of every type. There's a uniform probability of acquiring any such item. For n unique items, what is the expected number of samples until at least one of each collectible is obtained?

# Brute Force
First, let's write a program to try it out. When one of each collectible is chosen, return the number of samples to get there. Repeat this process to approximate a true solution. This code can be seen below. Note that the collectibles will be represented as unique integer values. And the collection is represented as a set.

```c++
static constexpr int kNumSamples = 1000000000;

double arithmetic_mean(const std::vector<int> &all_num_samples) {
    const int64_t sum =
        std::accumulate(all_num_samples.begin(), all_num_samples.end(),
                        static_cast<int64_t>(0));
    return sum / static_cast<double>(all_num_samples.size());
}

int compute_num_samples_to_fill_at_least_one_bin(const int num_bins) {
    static std::default_random_engine random_engine;
    int samples = 0;
    std::unordered_set<int> bins;
    std::uniform_int_distribution uniform_dist(0, num_bins - 1);
    while (static_cast<int>(bins.size()) < num_bins) {
        const int new_bin = uniform_dist(random_engine);
        bins.insert(new_bin);
        ++samples;
    }
    return samples;
}

double compute_num_samples_to_fill_at_least_one_bin_mean(const int num_bins) {
    std::vector<int> num_samples;
    for (int sample = 0; sample < kNumSamples; ++sample) {
        num_samples.push_back(
            compute_num_samples_to_fill_at_least_one_bin(num_bins));
    }
    return arithmetic_mean(num_samples);
}
```

Running a program over a range of values for some time may yield a plot like below.

![bruteforceplot](/assets/images/2022/prob_mean_vs_bins.png "Brute Force Mean vs. Bins")

That looks promising. There's clearly a trend, but it isn't quite linear.

# Steps Towards an Analytical Solution
## Visualizing Every Brute Force Outcome
Below is a visualization of the various outcomes at each sample. Note that the shading level groups results with the same sample count. The area of the shading represents the probability of the result. Also, note that some of the results don't terminate.

![sampletree0](/assets/images/2022/prob_sample_tree_0.png "Brute Force Sample Tree")

50% of the time it takes two samples to complete the set. 25% it takes three turns. And 25% it takes four or more samples.

## Bounding the Long Tail
The space can be bounded by using the geometric series. The following equation computes the expected number of counts, when only one collectible is obtained and there are a total of two collectibles.

![boundedequation](/assets/images/2022/prob_bounded_equations.png "Bounded Equations")

This equation is the same as determining the expected number of flips until heads on a fair two sided coin. As an equation, it might look like the following.

![expectedvalue](/assets/images/2022/prob_expected_value.png "Expected Num Flips Fair Coin")

With the bounds, the chart looks like below.

![sampletree1](/assets/images/2022/prob_sample_tree_1.png "Bounded Sample Tree")

This chart is greatly simplified. After collecting unique item zero, there are two more expected samples to complete our collection. There's a similar situation after collecting unique item one. So for two unique items, three samples are expected to complete our collection.

## Observing Symmetry
The final step to achieving an analytical solution is to observe the symmetry among all the paths. It turns out that the order that the collectibles doesn't really matter. What really matters is the number of unique collectibles obtained. Therefore, the sample chart isn't a tree, but a list as shown below.

![sampletree2](/assets/images/2022/prob_sample_tree_2.png "Sample List")

Furthermore, the order that the unique collectible is obtained doesn't matter. Every step can be computed independently. Then, the final result can be added together in any order.

If we set the number of bins to n, we can say that the following equation computes the expected number of samples until all of them are chosen at least once.

![equationfinal](/assets/images/2022/prob_equation_final.png "Equation Final")

The previous equation is pleasing because it can be represented with only a few characters. Also, it can be computed accurately and efficiently for larger numbers.

# Representing the Equation in Code

The previous equation can be represented as the following C++ code.

```c++
double compute_num_samples_to_fill_at_least_one_bin_analytic(
    const int num_bins) {
    double output = 0.0;
    for (int i = 1; i <= num_bins; ++i) {
        output += static_cast<double>(num_bins) / i;
    }
    return output;
}
```

In newer versions of C++ it may be possible to use ranges to express this in a more functional way. For example, there's supposed to be a `sum` function in [C++23](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2020/p2214r0.html). Unfortunately, as of writing this, it doesn't exist in GCC. ranges v3 can get us pretty close. See below.

```c++
double compute_num_samples_to_fill_at_least_one_bin_analytic_ranges(
    const int num_bins) {
    auto n_div_i = [num_bins](const int i) {
        return static_cast<double>(num_bins) / i;
    };
    return ::ranges::accumulate(
        std::views::iota(1, num_bins + 1) | std::views::transform(n_div_i),
        0.0);
}
```

Using GCC 11.2, the ranges solution can generate similar code to the raw loop in terms of instructions with O2: [godbolt](https://godbolt.org/#z:OYLghAFBqd5TKALEBjA9gEwKYFFMCWALugE4A0BIEAZgQDbYB2AhgLbYgDkAjF%2BTXRMiAZVQtGIHgBYBQogFUAztgAKAD24AGfgCsp5eiyahUAUgBMAIUtXyKxqiIEh1ZpgDC6egFc2TEAtydwAZAiZsADk/ACNsUikggAd0JWIXJi9ff0DyFLTnITCI6LY4hJ4gh2wnDJEiFlIiLL8AquxHQqZ6xqJiqNj4xPsGppacqtG%2B8IGyocqASnt0H1JUTi5LAGZw1F8cAGozLY9SY2BsAHoANy3Lpj94glRLllRUPx8jImwAOiQkkljrgzFoAILbXb7bBHE5nEzYJTA0EQ8GYFYxRgHDBsJI%2BH4AfQebAJSnYSUYSgJJAJdHo9AJLCIBMYLCUzKE2AJMXCjNY9AAns5UAT4RclBAUQdpdihOyDuEiAdidzwkoFkcAOw2cEyg4sfHoZUEwjXAkEWEAESOAFYrCqeUwkTbLRAME6lYqFRqzNqpXqZaRsERVkwDuymc8CeJ2ccPOifJjsMCIA61RrLgrjjqwXrfZbs/7pUGQ6QwyAQGLERW3h82F8mdhJbqA9L2ZgK9cCNgAO5KCsuBoQHjkZV%2BVVO2w8H2ajzhogdkBd3v9kBEeFKQSkNipk0EM0EJZFgNaX5aBaF8H5rhLejcG38AJcHTkdDcDy2WzhlZrGHbPjkEQ2g3ksADWIBbFsvyagAnFoAAc0gwQAbJqWwWFoFjITwNqGNw0j8GwIA2lo5BPi%2Bb5cPw/akUBz43uQcCwCgGA4PgxBkJQ1B0sw7AbABgjCGIEicDIcjCMoaiaPR5D6EERgmGgn7WPYHQ1F0bhMJ43itMRQShDMpTlMReSpOkQjjAENrJGZXT9EZQy4dUtRCD0Yw6Tk1mqZ0dRTPZgwJE5UyWXpIy9P5cyBUsSg/us3AJkmsq4viXIqmSuKUtS6C0gwDJMiy2BshyEQTnyEhCs8za5jK7ryl6aZOjOOZ6glWIrEQeJKsc1qnlol7VdKW4HBAXoWt1Bw8NmCqwh%2BWzWg1SJbFYRzWLYh5as1rYHO1nUrTYc3zpGIoxkQcatcmWy4Km46OuqByZgQ/V5pqBYtsWwahtt%2BKdf1163vej7Aa%2B77KctMWrOsK3ofwdE6AsSxIIVOAJBA/1cAR5BESRZFA5R1EgLRwHw%2BQ4Ekb80g2pqNo8MhkEwfByEwRY8F4VwWyAzJeOAUTjGIExyBoOguIMPEXFukLSQiwkwA8JUAgMD8pD9hAMRA46jQCtwAE4hwwgAPJMIKQM4Gw5ySDJhBBrU1zVjJ2DqDUKVa/wiodED9AEDEZykAKXg4M7gGkAQRG8AxNBGMASgAGrdj2etJMwAeCaI4iSGJyeSRoQP6COCmmKDhie/2sCsBwlbnIitzkDbCTrj4TCgcT6BJF0/ZcAAtAA6hI9AHF39vriwfftt15irdYGF93rFjUWpLkBBA7ghSOBklAFBj5OZmQeQEI6b3ZhnryOzldG5zQ7wYJ%2B%2BeFh%2BRZfwUX8ffm38Z07LBDomB9g/EMXeXAPjjTm3B1AM3bshaQBxgDvAmjwX4FhhofnHnYA47ESCkChiOA4XhhaMHQf%2BBYMMiZgWIqRP%2BGMsani2DweCMFYJYVptITUyEWbkX4FzGi3N6JLH5hAFiEspZixxJLXBIAZZyzpIrZWqsZLqx9gHHWzAiAGyNhbbApsTDmxfJbdS%2B5bYvnto7H4AdXZ/xfB7L2Gs/YbBfOuYOzsljhxYJHGOvZ46J1DuJFOIkpCyAzioLOMk5KGHOEpJBhcYjF1Rq%2BFuGQ25dx7n3TuA8zjDwXKPUGk927T1nj5Vwi8tLL2CFpCKr9TIFAyIU/eGQSnzG8jo1yD9si7zqfPM%2BNSKhhXck0%2B%2BN8153zfuDX8uR1zf3sazABrDgZcBAchMBECoGoBgXAhBBcUGEDQVDIIWD%2BG4M2QQzhcMEZIyGFE8CPAtCkPwoREASFfjMPORhNCWxkLWXQoAii3B8aEy4cQ25NppAWGkPBZ5yEtA8C2FoNCrN2bvLYZ8g5IFWYz1hVMwhPzq7xDSK4aQQA%3D). Take this measurement with a grain of salt. Ranges is new. There may be extra compile time overhead. In addition, there may be runtime overhead if parts don't get optimized. One advantage to ranges is that the operation could be parallelized.

# Conclusion
For a certain range of values on this particular problem, a linear fit model might be acceptable. The trend is a little harder to fit for large ranges. See the plot below for large numbers. Note that there is a log scale for both axes.

![finalplot](/assets/images/2022/prob_final_plot.png "Final Plot")

Computers are a great tool to hammer through a lot of math. Then, we can analyze those results to spot trends. Iterating on this process can produce elegant results. The result can feel [victorious!](https://www.youtube.com/watch?v=ZMovw9o9YCk)
