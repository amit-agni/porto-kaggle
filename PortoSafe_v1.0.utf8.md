
<!-- rnb-text-begin -->

---
title: "Porto Safe "
output: html_notebook
---
Objective : is to predict the probability that a driver will initiate an auto insurance claim in the next year.
Data Description : Features that belong to similar groupings are tagged as such in the feature names (e.g., ind, reg, car, calc). 
Feature names include the postfix bin to indicate binary features and cat to indicate categorical features. Features without these designations are either continuous or ordinal.
Values of -1 indicate that the feature was missing from the observation. The target columns signifies whether or not a claim was filed for that policy holder.

First will setup the environment and load the data

<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-output-begin eyJkYXRhIjoiRXJyb3IgaW4gZmlsZShmaWxlLCBcInJ0XCIpIDogY2Fubm90IG9wZW4gdGhlIGNvbm5lY3Rpb25cbiJ9 -->

```
Error in file(file, "rt") : cannot open the connection
```



<!-- rnb-output-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->


Check whether train and test names are same. 

<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxuZGVzY3JpYmUoZGF0YXNldCxJUVI9VFJVRSwgcXVhbnQgPSBjKDAuMjUsMC43NSkpXG5cbmBgYCJ9 -->

```r
describe(dataset,IQR=TRUE, quant = c(0.25,0.75))

```

<!-- rnb-source-end -->

<!-- rnb-output-begin eyJkYXRhIjoiICAgICAgICAgICAgICAgdmFycyAgICAgICBuICAgICAgbWVhbiAgICAgICAgc2QgICBtaW4gICAgICAgIG1heCAgICAgIHJhbmdlICAgICBzZSAgICAgICBJUVIgICAgIFEwLjI1ICAgICAgUTAuNzVcbmlkICAgICAgICAgICAgICAgIDEgMTQ4ODAyOCA3NDQwMTMuNTAgNDI5NTU2LjgzICAwLjAwIDE0ODgwMjcuMDAgMTQ4ODAyNy4wMCAzNTIuMTQgNzQ0MDEzLjUwIDM3MjAwNi43NSAxMTE2MDIwLjI1XG5wc19pbmRfMDEgICAgICAgICAyIDE0ODgwMjggICAgICAxLjkwICAgICAgMS45OSAgMC4wMCAgICAgICA3LjAwICAgICAgIDcuMDAgICAwLjAwICAgICAgMy4wMCAgICAgIDAuMDAgICAgICAgMy4wMFxucHNfaW5kXzAyX2NhdCAgICAgMyAxNDg4MDI4ICAgICAgMS4zNiAgICAgIDAuNjYgLTEuMDAgICAgICAgNC4wMCAgICAgICA1LjAwICAgMC4wMCAgICAgIDEuMDAgICAgICAxLjAwICAgICAgIDIuMDBcbnBzX2luZF8wMyAgICAgICAgIDQgMTQ4ODAyOCAgICAgIDQuNDIgICAgICAyLjcwICAwLjAwICAgICAgMTEuMDAgICAgICAxMS4wMCAgIDAuMDAgICAgICA0LjAwICAgICAgMi4wMCAgICAgICA2LjAwXG5wc19pbmRfMDRfY2F0ICAgICA1IDE0ODgwMjggICAgICAwLjQyICAgICAgMC40OSAtMS4wMCAgICAgICAxLjAwICAgICAgIDIuMDAgICAwLjAwICAgICAgMS4wMCAgICAgIDAuMDAgICAgICAgMS4wMFxucHNfaW5kXzA1X2NhdCAgICAgNiAxNDg4MDI4ICAgICAgMC40MSAgICAgIDEuMzUgLTEuMDAgICAgICAgNi4wMCAgICAgICA3LjAwICAgMC4wMCAgICAgIDAuMDAgICAgICAwLjAwICAgICAgIDAuMDBcbnBzX2luZF8wNl9iaW4gICAgIDcgMTQ4ODAyOCAgICAgIDAuMzkgICAgICAwLjQ5ICAwLjAwICAgICAgIDEuMDAgICAgICAgMS4wMCAgIDAuMDAgICAgICAxLjAwICAgICAgMC4wMCAgICAgICAxLjAwXG5wc19pbmRfMDdfYmluICAgICA4IDE0ODgwMjggICAgICAwLjI2ICAgICAgMC40NCAgMC4wMCAgICAgICAxLjAwICAgICAgIDEuMDAgICAwLjAwICAgICAgMS4wMCAgICAgIDAuMDAgICAgICAgMS4wMFxucHNfaW5kXzA4X2JpbiAgICAgOSAxNDg4MDI4ICAgICAgMC4xNiAgICAgIDAuMzcgIDAuMDAgICAgICAgMS4wMCAgICAgICAxLjAwICAgMC4wMCAgICAgIDAuMDAgICAgICAwLjAwICAgICAgIDAuMDBcbnBzX2luZF8wOV9iaW4gICAgMTAgMTQ4ODAyOCAgICAgIDAuMTkgICAgICAwLjM5ICAwLjAwICAgICAgIDEuMDAgICAgICAgMS4wMCAgIDAuMDAgICAgICAwLjAwICAgICAgMC4wMCAgICAgICAwLjAwXG5wc19pbmRfMTBfYmluICAgIDExIDE0ODgwMjggICAgICAwLjAwICAgICAgMC4wMiAgMC4wMCAgICAgICAxLjAwICAgICAgIDEuMDAgICAwLjAwICAgICAgMC4wMCAgICAgIDAuMDAgICAgICAgMC4wMFxucHNfaW5kXzExX2JpbiAgICAxMiAxNDg4MDI4ICAgICAgMC4wMCAgICAgIDAuMDQgIDAuMDAgICAgICAgMS4wMCAgICAgICAxLjAwICAgMC4wMCAgICAgIDAuMDAgICAgICAwLjAwICAgICAgIDAuMDBcbnBzX2luZF8xMl9iaW4gICAgMTMgMTQ4ODAyOCAgICAgIDAuMDEgICAgICAwLjEwICAwLjAwICAgICAgIDEuMDAgICAgICAgMS4wMCAgIDAuMDAgICAgICAwLjAwICAgICAgMC4wMCAgICAgICAwLjAwXG5wc19pbmRfMTNfYmluICAgIDE0IDE0ODgwMjggICAgICAwLjAwICAgICAgMC4wMyAgMC4wMCAgICAgICAxLjAwICAgICAgIDEuMDAgICAwLjAwICAgICAgMC4wMCAgICAgIDAuMDAgICAgICAgMC4wMFxucHNfaW5kXzE0ICAgICAgICAxNSAxNDg4MDI4ICAgICAgMC4wMSAgICAgIDAuMTMgIDAuMDAgICAgICAgNC4wMCAgICAgICA0LjAwICAgMC4wMCAgICAgIDAuMDAgICAgICAwLjAwICAgICAgIDAuMDBcbnBzX2luZF8xNSAgICAgICAgMTYgMTQ4ODAyOCAgICAgIDcuMzAgICAgICAzLjU0ICAwLjAwICAgICAgMTMuMDAgICAgICAxMy4wMCAgIDAuMDAgICAgICA1LjAwICAgICAgNS4wMCAgICAgIDEwLjAwXG5wc19pbmRfMTZfYmluICAgIDE3IDE0ODgwMjggICAgICAwLjY2ICAgICAgMC40NyAgMC4wMCAgICAgICAxLjAwICAgICAgIDEuMDAgICAwLjAwICAgICAgMS4wMCAgICAgIDAuMDAgICAgICAgMS4wMFxucHNfaW5kXzE3X2JpbiAgICAxOCAxNDg4MDI4ICAgICAgMC4xMiAgICAgIDAuMzMgIDAuMDAgICAgICAgMS4wMCAgICAgICAxLjAwICAgMC4wMCAgICAgIDAuMDAgICAgICAwLjAwICAgICAgIDAuMDBcbnBzX2luZF8xOF9iaW4gICAgMTkgMTQ4ODAyOCAgICAgIDAuMTUgICAgICAwLjM2ICAwLjAwICAgICAgIDEuMDAgICAgICAgMS4wMCAgIDAuMDAgICAgICAwLjAwICAgICAgMC4wMCAgICAgICAwLjAwXG5wc19yZWdfMDEgICAgICAgIDIwIDE0ODgwMjggICAgICAwLjYxICAgICAgMC4yOSAgMC4wMCAgICAgICAwLjkwICAgICAgIDAuOTAgICAwLjAwICAgICAgMC41MCAgICAgIDAuNDAgICAgICAgMC45MFxucHNfcmVnXzAyICAgICAgICAyMSAxNDg4MDI4ICAgICAgMC40NCAgICAgIDAuNDAgIDAuMDAgICAgICAgMS44MCAgICAgICAxLjgwICAgMC4wMCAgICAgIDAuNDAgICAgICAwLjIwICAgICAgIDAuNjBcbnBzX3JlZ18wMyAgICAgICAgMjIgMTQ4ODAyOCAgICAgIDAuNTUgICAgICAwLjc5IC0xLjAwICAgICAgIDQuNDIgICAgICAgNS40MiAgIDAuMDAgICAgICAwLjQ4ICAgICAgMC41MiAgICAgICAxLjAwXG5wc19jYXJfMDFfY2F0ICAgIDIzIDE0ODgwMjggICAgICA4LjI5ICAgICAgMi41MSAtMS4wMCAgICAgIDExLjAwICAgICAgMTIuMDAgICAwLjAwICAgICAgNC4wMCAgICAgIDcuMDAgICAgICAxMS4wMFxucHNfY2FyXzAyX2NhdCAgICAyNCAxNDg4MDI4ICAgICAgMC44MyAgICAgIDAuMzggLTEuMDAgICAgICAgMS4wMCAgICAgICAyLjAwICAgMC4wMCAgICAgIDAuMDAgICAgICAxLjAwICAgICAgIDEuMDBcbnBzX2Nhcl8wM19jYXQgICAgMjUgMTQ4ODAyOCAgICAgLTAuNTAgICAgICAwLjc5IC0xLjAwICAgICAgIDEuMDAgICAgICAgMi4wMCAgIDAuMDAgICAgICAxLjAwICAgICAtMS4wMCAgICAgICAwLjAwXG5wc19jYXJfMDRfY2F0ICAgIDI2IDE0ODgwMjggICAgICAwLjczICAgICAgMi4xNSAgMC4wMCAgICAgICA5LjAwICAgICAgIDkuMDAgICAwLjAwICAgICAgMC4wMCAgICAgIDAuMDAgICAgICAgMC4wMFxucHNfY2FyXzA1X2NhdCAgICAyNyAxNDg4MDI4ICAgICAtMC4xNiAgICAgIDAuODQgLTEuMDAgICAgICAgMS4wMCAgICAgICAyLjAwICAgMC4wMCAgICAgIDIuMDAgICAgIC0xLjAwICAgICAgIDEuMDBcbnBzX2Nhcl8wNl9jYXQgICAgMjggMTQ4ODAyOCAgICAgIDYuNTYgICAgICA1LjUwICAwLjAwICAgICAgMTcuMDAgICAgICAxNy4wMCAgIDAuMDAgICAgIDEwLjAwICAgICAgMS4wMCAgICAgIDExLjAwXG5wc19jYXJfMDdfY2F0ICAgIDI5IDE0ODgwMjggICAgICAwLjkxICAgICAgMC4zNSAtMS4wMCAgICAgICAxLjAwICAgICAgIDIuMDAgICAwLjAwICAgICAgMC4wMCAgICAgIDEuMDAgICAgICAgMS4wMFxucHNfY2FyXzA4X2NhdCAgICAzMCAxNDg4MDI4ICAgICAgMC44MyAgICAgIDAuMzcgIDAuMDAgICAgICAgMS4wMCAgICAgICAxLjAwICAgMC4wMCAgICAgIDAuMDAgICAgICAxLjAwICAgICAgIDEuMDBcbnBzX2Nhcl8wOV9jYXQgICAgMzEgMTQ4ODAyOCAgICAgIDEuMzMgICAgICAwLjk4IC0xLjAwICAgICAgIDQuMDAgICAgICAgNS4wMCAgIDAuMDAgICAgICAyLjAwICAgICAgMC4wMCAgICAgICAyLjAwXG5wc19jYXJfMTBfY2F0ICAgIDMyIDE0ODgwMjggICAgICAwLjk5ICAgICAgMC4wOSAgMC4wMCAgICAgICAyLjAwICAgICAgIDIuMDAgICAwLjAwICAgICAgMC4wMCAgICAgIDEuMDAgICAgICAgMS4wMFxucHNfY2FyXzExX2NhdCAgICAzMyAxNDg4MDI4ICAgICA2Mi4yNiAgICAgMzMuMDEgIDEuMDAgICAgIDEwNC4wMCAgICAgMTAzLjAwICAgMC4wMyAgICAgNjIuMDAgICAgIDMyLjAwICAgICAgOTQuMDBcbnBzX2Nhcl8xMSAgICAgICAgMzQgMTQ4ODAyOCAgICAgIDIuMzUgICAgICAwLjgzIC0xLjAwICAgICAgIDMuMDAgICAgICAgNC4wMCAgIDAuMDAgICAgICAxLjAwICAgICAgMi4wMCAgICAgICAzLjAwXG5wc19jYXJfMTIgICAgICAgIDM1IDE0ODgwMjggICAgICAwLjM4ICAgICAgMC4wNiAtMS4wMCAgICAgICAxLjI2ICAgICAgIDIuMjYgICAwLjAwICAgICAgMC4wOCAgICAgIDAuMzIgICAgICAgMC40MFxucHNfY2FyXzEzICAgICAgICAzNiAxNDg4MDI4ICAgICAgMC44MSAgICAgIDAuMjIgIDAuMjUgICAgICAgNC4wMyAgICAgICAzLjc4ICAgMC4wMCAgICAgIDAuMjQgICAgICAwLjY3ICAgICAgIDAuOTFcbnBzX2Nhcl8xNCAgICAgICAgMzcgMTQ4ODAyOCAgICAgIDAuMjggICAgICAwLjM2IC0xLjAwICAgICAgIDAuNjQgICAgICAgMS42NCAgIDAuMDAgICAgICAwLjA2ICAgICAgMC4zMyAgICAgICAwLjQwXG5wc19jYXJfMTUgICAgICAgIDM4IDE0ODgwMjggICAgICAzLjA3ICAgICAgMC43MyAgMC4wMCAgICAgICAzLjc0ICAgICAgIDMuNzQgICAwLjAwICAgICAgMC43OCAgICAgIDIuODMgICAgICAgMy42MVxucHNfY2FsY18wMSAgICAgICAzOSAxNDg4MDI4ICAgICAgMC40NSAgICAgIDAuMjkgIDAuMDAgICAgICAgMC45MCAgICAgICAwLjkwICAgMC4wMCAgICAgIDAuNTAgICAgICAwLjIwICAgICAgIDAuNzBcbnBzX2NhbGNfMDIgICAgICAgNDAgMTQ4ODAyOCAgICAgIDAuNDUgICAgICAwLjI5ICAwLjAwICAgICAgIDAuOTAgICAgICAgMC45MCAgIDAuMDAgICAgICAwLjUwICAgICAgMC4yMCAgICAgICAwLjcwXG5wc19jYWxjXzAzICAgICAgIDQxIDE0ODgwMjggICAgICAwLjQ1ICAgICAgMC4yOSAgMC4wMCAgICAgICAwLjkwICAgICAgIDAuOTAgICAwLjAwICAgICAgMC41MCAgICAgIDAuMjAgICAgICAgMC43MFxucHNfY2FsY18wNCAgICAgICA0MiAxNDg4MDI4ICAgICAgMi4zNyAgICAgIDEuMTIgIDAuMDAgICAgICAgNS4wMCAgICAgICA1LjAwICAgMC4wMCAgICAgIDEuMDAgICAgICAyLjAwICAgICAgIDMuMDBcbnBzX2NhbGNfMDUgICAgICAgNDMgMTQ4ODAyOCAgICAgIDEuODkgICAgICAxLjE0ICAwLjAwICAgICAgIDYuMDAgICAgICAgNi4wMCAgIDAuMDAgICAgICAyLjAwICAgICAgMS4wMCAgICAgICAzLjAwXG5wc19jYWxjXzA2ICAgICAgIDQ0IDE0ODgwMjggICAgICA3LjY5ICAgICAgMS4zMyAgMC4wMCAgICAgIDEwLjAwICAgICAgMTAuMDAgICAwLjAwICAgICAgMi4wMCAgICAgIDcuMDAgICAgICAgOS4wMFxucHNfY2FsY18wNyAgICAgICA0NSAxNDg4MDI4ICAgICAgMy4wMSAgICAgIDEuNDEgIDAuMDAgICAgICAgOS4wMCAgICAgICA5LjAwICAgMC4wMCAgICAgIDIuMDAgICAgICAyLjAwICAgICAgIDQuMDBcbnBzX2NhbGNfMDggICAgICAgNDYgMTQ4ODAyOCAgICAgIDkuMjMgICAgICAxLjQ2ICAxLjAwICAgICAgMTIuMDAgICAgICAxMS4wMCAgIDAuMDAgICAgICAyLjAwICAgICAgOC4wMCAgICAgIDEwLjAwXG5wc19jYWxjXzA5ICAgICAgIDQ3IDE0ODgwMjggICAgICAyLjM0ICAgICAgMS4yNSAgMC4wMCAgICAgICA3LjAwICAgICAgIDcuMDAgICAwLjAwICAgICAgMi4wMCAgICAgIDEuMDAgICAgICAgMy4wMFxucHNfY2FsY18xMCAgICAgICA0OCAxNDg4MDI4ICAgICAgOC40NCAgICAgIDIuOTEgIDAuMDAgICAgICAyNS4wMCAgICAgIDI1LjAwICAgMC4wMCAgICAgIDQuMDAgICAgICA2LjAwICAgICAgMTAuMDBcbnBzX2NhbGNfMTEgICAgICAgNDkgMTQ4ODAyOCAgICAgIDUuNDQgICAgICAyLjMzICAwLjAwICAgICAgMjAuMDAgICAgICAyMC4wMCAgIDAuMDAgICAgICAzLjAwICAgICAgNC4wMCAgICAgICA3LjAwXG5wc19jYWxjXzEyICAgICAgIDUwIDE0ODgwMjggICAgICAxLjQ0ICAgICAgMS4yMCAgMC4wMCAgICAgIDExLjAwICAgICAgMTEuMDAgICAwLjAwICAgICAgMS4wMCAgICAgIDEuMDAgICAgICAgMi4wMFxucHNfY2FsY18xMyAgICAgICA1MSAxNDg4MDI4ICAgICAgMi44NyAgICAgIDEuNjkgIDAuMDAgICAgICAxNS4wMCAgICAgIDE1LjAwICAgMC4wMCAgICAgIDIuMDAgICAgICAyLjAwICAgICAgIDQuMDBcbnBzX2NhbGNfMTQgICAgICAgNTIgMTQ4ODAyOCAgICAgIDcuNTQgICAgICAyLjc1ICAwLjAwICAgICAgMjguMDAgICAgICAyOC4wMCAgIDAuMDAgICAgICAzLjAwICAgICAgNi4wMCAgICAgICA5LjAwXG5wc19jYWxjXzE1X2JpbiAgIDUzIDE0ODgwMjggICAgICAwLjEyICAgICAgMC4zMyAgMC4wMCAgICAgICAxLjAwICAgICAgIDEuMDAgICAwLjAwICAgICAgMC4wMCAgICAgIDAuMDAgICAgICAgMC4wMFxucHNfY2FsY18xNl9iaW4gICA1NCAxNDg4MDI4ICAgICAgMC42MyAgICAgIDAuNDggIDAuMDAgICAgICAgMS4wMCAgICAgICAxLjAwICAgMC4wMCAgICAgIDEuMDAgICAgICAwLjAwICAgICAgIDEuMDBcbnBzX2NhbGNfMTdfYmluICAgNTUgMTQ4ODAyOCAgICAgIDAuNTUgICAgICAwLjUwICAwLjAwICAgICAgIDEuMDAgICAgICAgMS4wMCAgIDAuMDAgICAgICAxLjAwICAgICAgMC4wMCAgICAgICAxLjAwXG5wc19jYWxjXzE4X2JpbiAgIDU2IDE0ODgwMjggICAgICAwLjI5ICAgICAgMC40NSAgMC4wMCAgICAgICAxLjAwICAgICAgIDEuMDAgICAwLjAwICAgICAgMS4wMCAgICAgIDAuMDAgICAgICAgMS4wMFxucHNfY2FsY18xOV9iaW4gICA1NyAxNDg4MDI4ICAgICAgMC4zNSAgICAgIDAuNDggIDAuMDAgICAgICAgMS4wMCAgICAgICAxLjAwICAgMC4wMCAgICAgIDEuMDAgICAgICAwLjAwICAgICAgIDEuMDBcbnBzX2NhbGNfMjBfYmluICAgNTggMTQ4ODAyOCAgICAgIDAuMTUgICAgICAwLjM2ICAwLjAwICAgICAgIDEuMDAgICAgICAgMS4wMCAgIDAuMDAgICAgICAwLjAwICAgICAgMC4wMCAgICAgICAwLjAwXG4ifQ== -->

```
               vars       n      mean        sd   min        max      range     se       IQR     Q0.25      Q0.75
id                1 1488028 744013.50 429556.83  0.00 1488027.00 1488027.00 352.14 744013.50 372006.75 1116020.25
ps_ind_01         2 1488028      1.90      1.99  0.00       7.00       7.00   0.00      3.00      0.00       3.00
ps_ind_02_cat     3 1488028      1.36      0.66 -1.00       4.00       5.00   0.00      1.00      1.00       2.00
ps_ind_03         4 1488028      4.42      2.70  0.00      11.00      11.00   0.00      4.00      2.00       6.00
ps_ind_04_cat     5 1488028      0.42      0.49 -1.00       1.00       2.00   0.00      1.00      0.00       1.00
ps_ind_05_cat     6 1488028      0.41      1.35 -1.00       6.00       7.00   0.00      0.00      0.00       0.00
ps_ind_06_bin     7 1488028      0.39      0.49  0.00       1.00       1.00   0.00      1.00      0.00       1.00
ps_ind_07_bin     8 1488028      0.26      0.44  0.00       1.00       1.00   0.00      1.00      0.00       1.00
ps_ind_08_bin     9 1488028      0.16      0.37  0.00       1.00       1.00   0.00      0.00      0.00       0.00
ps_ind_09_bin    10 1488028      0.19      0.39  0.00       1.00       1.00   0.00      0.00      0.00       0.00
ps_ind_10_bin    11 1488028      0.00      0.02  0.00       1.00       1.00   0.00      0.00      0.00       0.00
ps_ind_11_bin    12 1488028      0.00      0.04  0.00       1.00       1.00   0.00      0.00      0.00       0.00
ps_ind_12_bin    13 1488028      0.01      0.10  0.00       1.00       1.00   0.00      0.00      0.00       0.00
ps_ind_13_bin    14 1488028      0.00      0.03  0.00       1.00       1.00   0.00      0.00      0.00       0.00
ps_ind_14        15 1488028      0.01      0.13  0.00       4.00       4.00   0.00      0.00      0.00       0.00
ps_ind_15        16 1488028      7.30      3.54  0.00      13.00      13.00   0.00      5.00      5.00      10.00
ps_ind_16_bin    17 1488028      0.66      0.47  0.00       1.00       1.00   0.00      1.00      0.00       1.00
ps_ind_17_bin    18 1488028      0.12      0.33  0.00       1.00       1.00   0.00      0.00      0.00       0.00
ps_ind_18_bin    19 1488028      0.15      0.36  0.00       1.00       1.00   0.00      0.00      0.00       0.00
ps_reg_01        20 1488028      0.61      0.29  0.00       0.90       0.90   0.00      0.50      0.40       0.90
ps_reg_02        21 1488028      0.44      0.40  0.00       1.80       1.80   0.00      0.40      0.20       0.60
ps_reg_03        22 1488028      0.55      0.79 -1.00       4.42       5.42   0.00      0.48      0.52       1.00
ps_car_01_cat    23 1488028      8.29      2.51 -1.00      11.00      12.00   0.00      4.00      7.00      11.00
ps_car_02_cat    24 1488028      0.83      0.38 -1.00       1.00       2.00   0.00      0.00      1.00       1.00
ps_car_03_cat    25 1488028     -0.50      0.79 -1.00       1.00       2.00   0.00      1.00     -1.00       0.00
ps_car_04_cat    26 1488028      0.73      2.15  0.00       9.00       9.00   0.00      0.00      0.00       0.00
ps_car_05_cat    27 1488028     -0.16      0.84 -1.00       1.00       2.00   0.00      2.00     -1.00       1.00
ps_car_06_cat    28 1488028      6.56      5.50  0.00      17.00      17.00   0.00     10.00      1.00      11.00
ps_car_07_cat    29 1488028      0.91      0.35 -1.00       1.00       2.00   0.00      0.00      1.00       1.00
ps_car_08_cat    30 1488028      0.83      0.37  0.00       1.00       1.00   0.00      0.00      1.00       1.00
ps_car_09_cat    31 1488028      1.33      0.98 -1.00       4.00       5.00   0.00      2.00      0.00       2.00
ps_car_10_cat    32 1488028      0.99      0.09  0.00       2.00       2.00   0.00      0.00      1.00       1.00
ps_car_11_cat    33 1488028     62.26     33.01  1.00     104.00     103.00   0.03     62.00     32.00      94.00
ps_car_11        34 1488028      2.35      0.83 -1.00       3.00       4.00   0.00      1.00      2.00       3.00
ps_car_12        35 1488028      0.38      0.06 -1.00       1.26       2.26   0.00      0.08      0.32       0.40
ps_car_13        36 1488028      0.81      0.22  0.25       4.03       3.78   0.00      0.24      0.67       0.91
ps_car_14        37 1488028      0.28      0.36 -1.00       0.64       1.64   0.00      0.06      0.33       0.40
ps_car_15        38 1488028      3.07      0.73  0.00       3.74       3.74   0.00      0.78      2.83       3.61
ps_calc_01       39 1488028      0.45      0.29  0.00       0.90       0.90   0.00      0.50      0.20       0.70
ps_calc_02       40 1488028      0.45      0.29  0.00       0.90       0.90   0.00      0.50      0.20       0.70
ps_calc_03       41 1488028      0.45      0.29  0.00       0.90       0.90   0.00      0.50      0.20       0.70
ps_calc_04       42 1488028      2.37      1.12  0.00       5.00       5.00   0.00      1.00      2.00       3.00
ps_calc_05       43 1488028      1.89      1.14  0.00       6.00       6.00   0.00      2.00      1.00       3.00
ps_calc_06       44 1488028      7.69      1.33  0.00      10.00      10.00   0.00      2.00      7.00       9.00
ps_calc_07       45 1488028      3.01      1.41  0.00       9.00       9.00   0.00      2.00      2.00       4.00
ps_calc_08       46 1488028      9.23      1.46  1.00      12.00      11.00   0.00      2.00      8.00      10.00
ps_calc_09       47 1488028      2.34      1.25  0.00       7.00       7.00   0.00      2.00      1.00       3.00
ps_calc_10       48 1488028      8.44      2.91  0.00      25.00      25.00   0.00      4.00      6.00      10.00
ps_calc_11       49 1488028      5.44      2.33  0.00      20.00      20.00   0.00      3.00      4.00       7.00
ps_calc_12       50 1488028      1.44      1.20  0.00      11.00      11.00   0.00      1.00      1.00       2.00
ps_calc_13       51 1488028      2.87      1.69  0.00      15.00      15.00   0.00      2.00      2.00       4.00
ps_calc_14       52 1488028      7.54      2.75  0.00      28.00      28.00   0.00      3.00      6.00       9.00
ps_calc_15_bin   53 1488028      0.12      0.33  0.00       1.00       1.00   0.00      0.00      0.00       0.00
ps_calc_16_bin   54 1488028      0.63      0.48  0.00       1.00       1.00   0.00      1.00      0.00       1.00
ps_calc_17_bin   55 1488028      0.55      0.50  0.00       1.00       1.00   0.00      1.00      0.00       1.00
ps_calc_18_bin   56 1488028      0.29      0.45  0.00       1.00       1.00   0.00      1.00      0.00       1.00
ps_calc_19_bin   57 1488028      0.35      0.48  0.00       1.00       1.00   0.00      1.00      0.00       1.00
ps_calc_20_bin   58 1488028      0.15      0.36  0.00       1.00       1.00   0.00      0.00      0.00       0.00
```



<!-- rnb-output-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->



The non observed values are supposed to have -1. Lets check them, how many of them are there (in the whole train set)


<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxuXG5cbiNVc2UgYSBzbWFsbGVyIGRhdGFzZXQgZm9yIHRyaWFsaW5nIHRoZSBlbmQgdG8gZW5kIHJlZ3Jlc3Npb25cbiNJbXB1dGF0aW9uIHdpdGgga25uIHVzaW5nIHRoZSBvcmlnaW5hbCBzZXQgdGFraW5nIHZlcnkgbG9uZyB0aW1lXG4jU3BsaXQgYXJvdW5kIDEwMDAwIHJvd3NcblxuZGltKHRyYWluKVxuc3BsaXQ8LWNyZWF0ZURhdGFQYXJ0aXRpb24odHJhaW4kaWQsIHAgPSAwLjAxLCBsaXN0ID0gRkFMU0UpXG5kYXRhc2V0PC10cmFpbltzcGxpdCxdXG5kaW0oZGF0YXNldClcblxuI3ZhbDwtbXRjYXJzWy1zcGxpdCxdXG5cblxuXG5cbmRpbSh0cmFpbilcbiNBY3R1YWwgbWlzc2luZ1xuc2FwcGx5KHRyYWluLCBmdW5jdGlvbih4KSBzdW0oeCA9PSAtMSkgKVxuI1BlcmNlbnRhZ2VcbnNhcHBseSh0cmFpbiwgZnVuY3Rpb24oeCkgZm9ybWF0KDEwMCogKHN1bSh4ID09IC0xKSAvIDU5NTIxMiksIGRpZ2l0cyA9IDMpKVxuXG5gYGAifQ== -->

```r


#Use a smaller dataset for trialing the end to end regression
#Imputation with knn using the original set taking very long time
#Split around 10000 rows

dim(train)
split<-createDataPartition(train$id, p = 0.01, list = FALSE)
dataset<-train[split,]
dim(dataset)

#val<-mtcars[-split,]




dim(train)
#Actual missing
sapply(train, function(x) sum(x == -1) )
#Percentage
sapply(train, function(x) format(100* (sum(x == -1) / 595212), digits = 3))

```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->


There are quite a few null observations in 
ps_ind_05_cat 1%
ps_reg_03 18%
ps_car_03_cat 69%
ps_car_05_cat 45%
ps_car_07_cat 2%
ps_car_14 7%

These will have to be imputed. Rest are small percentages and can be ignored at this stage.
Source : https://www.analyticsvidhya.com/blog/2016/12/practical-guide-to-implement-machine-learning-with-caret-package-in-r-with-practice-problem/
Lets impute the sample dataset using kNN


<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxuXG4jQ29udmVydCB0aGUgbm9uLW9ic2VydmVkIHZhbHVlcyB0byBOQSAoYXMgcmVxdWlyZWQgYnkga05OIHByZVByb2Nlc3MpXG5kYXRhc2V0W2RhdGFzZXQgPT0gLTFdIDwtIE5BXG4jUmVjaGVjayB0aGUgcGVyY2VudGFnZXNcbnNhcHBseShkYXRhc2V0LCBmdW5jdGlvbih4KSBzdW0oaXMubmEoeCkpKVxuXG4jUmVtb3ZlIElkXG5kYXRhc2V0JGlkIDwtIE5VTExcbm5hbWVzKGRhdGFzZXQpXG5cblxuXG5cbiNJbXB1dGluZyBtaXNzaW5nIHZhbHVlcyB1c2luZyBLTk4uXG5cbiNJbXB1dGluZyBhbHNvIHNjYWxlcyB0aGUgZGF0YSwgc28gcmVtb3ZlIHRhcmdldCBmcm9tIGltcHV0YXRpb25cbnRlbXAgPC0gZGF0YXNldFssLTFdXG5cbnByZVByb2NWYWx1ZXMgPC0gcHJlUHJvY2Vzcyh0ZW1wLCBtZXRob2QgPSBjKFwia25uSW1wdXRlXCIpKVxuI2luc3RhbGwucGFja2FnZXMoXCJSQU5OXCIpXG5saWJyYXJ5KFJBTk4pXG5zdW0oaXMubmEoZGF0YXNldCkpXG50ZW1wIDwtIHByZWRpY3QocHJlUHJvY1ZhbHVlcywgdGVtcClcbnN1bShpcy5uYShkYXRhc2V0KSlcbiNbMV0gMFxuXG4jYWRkIHRoZSB0YXJnZXQgY29sdW1uIGJhY2ssIHJldGFpbiB0aGUgY29sdW1uIG5hbWVcblxubmFtZXMoZGF0YXNldClcbmRhdGFzZXQgPC0gY2JpbmQoZGF0YXNldFssMSxkcm9wID0gRkFMU0VdLHRlbXApXG5jb2xuYW1lcyhkYXRhc2V0WyxcImRhdGFzZXRbLDFdXCJdKSA8LSBcInRhcmdldFwiXG5cblxuXG5gYGAifQ== -->

```r

#Convert the non-observed values to NA (as required by kNN preProcess)
dataset[dataset == -1] <- NA
#Recheck the percentages
sapply(dataset, function(x) sum(is.na(x)))

#Remove Id
dataset$id <- NULL
names(dataset)




#Imputing missing values using KNN.

#Imputing also scales the data, so remove target from imputation
temp <- dataset[,-1]

preProcValues <- preProcess(temp, method = c("knnImpute"))
#install.packages("RANN")
library(RANN)
sum(is.na(dataset))
temp <- predict(preProcValues, temp)
sum(is.na(dataset))
#[1] 0

#add the target column back, retain the column name

names(dataset)
dataset <- cbind(dataset[,1,drop = FALSE],temp)
colnames(dataset[,"dataset[,1]"]) <- "target"

```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->


Set the parameters for caret


<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxubmFtZXMoZGF0YXNldClcbnN0cihkYXRhc2V0KVxuXG5jb250cm9sIDwtIHRyYWluQ29udHJvbChtZXRob2Q9XCJyZXBlYXRlZGN2XCIsIG51bWJlcj01LCByZXBlYXRzPTEpXG5zZWVkIDwtIDdcblxuY2xhc3Moc2VlZClcblxubWV0cmljIDwtIFwiQWNjdXJhY3lcIlxuXG4jU29tZSBtb2RlbHMgbmVlZHMgU2NhbGluZ1xucHJlUHJvY2Vzcz1jKFwiY2VudGVyXCIsIFwic2NhbGVcIilcblxuYGBgIn0= -->

```r
names(dataset)
str(dataset)

control <- trainControl(method="repeatedcv", number=5, repeats=1)
seed <- 7

class(seed)

metric <- "Accuracy"

#Some models needs Scaling
preProcess=c("center", "scale")

```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->


Now try the logistic regression 


<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxuIyBMb2dpc3RpYyBSZWdyZXNzaW9uXG4jZ2xtIGlzIGV4cGVjdGluZyB0YXJnZXQgdG8gYmUgZmFjdG9yIHRvIGRvIGNsYXNzaWZpY2F0aW9uXG4jU3RpbGwgZ2l2aW5nIDogcHJlZGljdGlvbiBmcm9tIGEgcmFuay1kZWZpY2llbnQgZml0IG1heSBiZSBtaXNsZWFkaW5nXG5kYXRhc2V0JHRhcmdldCA8LSBhcy5mYWN0b3IoZGF0YXNldCR0YXJnZXQpXG5cbnNldC5zZWVkKHNlZWQpXG5maXQuZ2xtIDwtIHRyYWluKHRhcmdldH4uLCBkYXRhPWRhdGFzZXQsIG1ldGhvZD1cImdsbVwiLCBtZXRyaWM9bWV0cmljLCB0ckNvbnRyb2w9Y29udHJvbClcbmBgYCJ9 -->

```r
# Logistic Regression
#glm is expecting target to be factor to do classification
#Still giving : prediction from a rank-deficient fit may be misleading
dataset$target <- as.factor(dataset$target)

set.seed(seed)
fit.glm <- train(target~., data=dataset, method="glm", metric=metric, trControl=control)
```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->


use only 2 variables

<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxuXG5uYW1lcyhkYXRhc2V0KVxuc3RyKGRhdGFzZXQpXG5maXQuZ2xtIDwtIHRyYWluKHRhcmdldH5wc19pbmRfMDErcHNfaW5kXzAzLCBkYXRhPWRhdGFzZXQsIG1ldGhvZD1cImdsbVwiLCBtZXRyaWM9bWV0cmljLCB0ckNvbnRyb2w9Y29udHJvbClcblxuYGBgIn0= -->

```r

names(dataset)
str(dataset)
fit.glm <- train(target~ps_ind_01+ps_ind_03, data=dataset, method="glm", metric=metric, trControl=control)

```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->










<!-- rnb-text-end -->

