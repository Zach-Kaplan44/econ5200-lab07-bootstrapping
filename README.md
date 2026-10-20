# econ5200-lab07-bootstrapping
## Diagnosing Broken Confidence Intervals

### Objective
I tested when bootstrap and parametric confidence intervals fail, and whether choosing the right resampling scheme fixes them.

### Methodology
- Bootstrapped the median of 32,561 integer ages and examined why the resulting standard error came out as 0 and why BCa returned `nan` with a warning instead of an interval.
- Ran 400 simulated experiments on skewed data with n = 50, building nominal 95% parametric and bootstrap intervals for both the mean and the median, and recorded how often each interval contained the true value.
- Bootstrapped wages from 25 firms two ways: first treating every worker as independent, then resampling whole firms, and compared interval width and coverage between the two.
- Used a `bootstrap_utils.py` module containing percentile, i.i.d., and cluster bootstrap functions.
- Had an AI write a coverage function, revised my prompt once to specify the seeding rule, and checked its output against my own coverage numbers.

### Key Findings
- **Degenerate bootstrap:** With 32,561 integer ages, the bootstrap median had a standard error of 0, and BCa could not produce an interval, returning `nan` with a warning.
- **Coverage on skewed data:** For the mean, both methods came in slightly under 95% (parametric 92.5%, bootstrap 92.0%). For the median, the parametric interval failed badly at 66.8%, while the bootstrap held at 92.8%.
- **Clustered data:** Treating workers from 25 firms as independent produced an interval 5.7x too narrow, with only 19.5% coverage. Resampling whole firms brought coverage up to 91.0%.
- **AI-written code check:** After I specified the seeding rule in my prompt, the AI's coverage function matched my results exactly on both arms: 19.5% vs. 19.5% and 91.0% vs. 91.0%.
