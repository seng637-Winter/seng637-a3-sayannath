**SENG 637 - Dependability and Reliability of Software Systems**

**Lab. Report #3 – Code Coverage, Adequacy Criteria and Test Case Correlation**

| Group \#:      |     |
| -------------- | --- |
| Student Names: |     |
|                |     |
|                |     |
|                |     |

(Note that some labs require individual reports while others require one report
for each group. Please see each lab document for details.)

# 1 Introduction

In this assignment 3, we will first look at different tools used to measure how well our software testing covers the code. We'll explore various tools recommended for checking these coverage measurements. Then, we will learn how to check coverage ourselves manually.

Next, we will see how to use a specific coverage tool called EclEmma. We will use it to make our test cases from Assignment 2 better.

# 2 Manual data-flow coverage calculations

### 1. `DataUtilities.calculateColumnTotal(Values2D data, int column)`
### Data flow graph

![calculateColumnTotal(Values2D data, int column)](media/seng637_assignment3_dfd_calculateColumnTotal.drawio.png)
### Defs, Uses, and DU-pairs

|               |                                |
| ------------- | ------------------------------ |
| **defs**:     | def(1) = {data, column}        |
|               | def(3) = {total, rowCount, r}  |
|               | def(5) = {n}                   |
|               | def(6) = {total}               |
|               | def(7) = {r}                   |
|               | def(8) = {r2}                  |
|               | def(9) = {n}                   |
|               | def(10) = {total}              |
|               | def(11) = {r2}                 |
| **uses**:     | use(2) = {data}      |
|               | use(3) = {data}      |
|               | use(4) = {rowCount, r}      |
|               | use(5) = {data, r, column, n}              |
|               | use(6) = {total, n}              |
|               | use(7) = {r}              |
|               | use(8) = {r2, rowCount}              |
|               | use(9) = {data, r2, column, n}              |
|               | use(10) = {total, n}              |
|               | use(11) = {r2}              |
|               | use(12) = {total}              |
| **du-pairs**: | data: (1, 2), (1, 3), (1, 5), (1, 9) |
|               | column: (1, 5), (1, 9)         |
|               | total: (3, 6), (3, 10), (3, 12), (6, 6), (6, 12), (10, 10), (10, 12)         |
|               | rowCount: (3, 4), (3, 8)        |
|               | r: (3, 4), (3, 5), (3, 7), (7, 7), (7, 4)       |
|               | n: (5, 5), (5, 6), (9, 9), (9, 10)       |
|               | r2: (8, 8), (8, 9), (8, 11), (11, 11), (11, 8) |

### DU-pairs covered in test cases
| Test case | Execution path | DU-pairs covered |
|---|---|--|
|`testCalculateColumnTotalOfFirstColumn`|[1, 2, 3, 4, 5, 6, 7, 8, 12]|(1, 2), (1, 3), (1, 5), (1, 9), (3, 6), (3, 12), (6, 6), (6, 12), (3, 4), (3, 5), (3, 7), (7, 7), (7, 4), (5, 5), (5, 6), (8, 8), (8, 12)|
|`testCalculateColumnTotalOfSecondColumn`|[1, 2, 3, 4, 5, 6, 7, 8, 12]|(1, 2), (1, 3), (1, 5), (1, 9), (3, 6), (3, 12), (6, 6), (6, 12), (3, 4), (3, 5), (3, 7), (7, 7), (7, 4), (5, 5), (5, 6), (8, 8), (8, 12)|
|`testCalculateColumnTotalWhenPositiveBoundaryExists`|[1, 2, 3, 4, 5, 6, 7, 8, 12]|(1, 2), (1, 3), (1, 5), (1, 9), (3, 6), (3, 12), (6, 6), (6, 12), (3, 4), (3, 5), (3, 7), (7, 7), (7, 4), (5, 5), (5, 6), (8, 8), (8, 12)|
|`testCalculateColumnTotalWhenNegativeExists`|[1, 2, 3, 4, 5, 6, 7, 8, 12]|(1, 2), (1, 3), (1, 5), (1, 9), (3, 6), (3, 12), (6, 6), (6, 12), (3, 4), (3, 5), (3, 7), (7, 7), (7, 4), (5, 5), (5, 6), (8, 8), (8, 12)|
|`testCalculateColumnTotalWhenTheIndexBecomesTooLarge`|[1, 2, 3, 4, 5, 6, 7, 8, 12]|(1, 2), (1, 3), (1, 5), (1, 9), (3, 6), (3, 12), (6, 6), (6, 12), (3, 4), (3, 5), (3, 7), (7, 7), (7, 4), (5, 5), (5, 6), (8, 8), (8, 12)|
|`testCalculateColumnTotalWhenTheIndexBecomesTooLarge`|[1, 2, 3, 4, 5, 6, 7, 8, 12]|(1, 2), (1, 3), (1, 5), (1, 9), (3, 6), (3, 12), (6, 6), (6, 12), (3, 4), (3, 5), (3, 7), (7, 7), (7, 4), (5, 5), (5, 6), (8, 8), (8, 12)|
|`testCalculateColumnTotalAllNonNull`|[1, 2, 3, 4, 5, 6, 7, 8, 12]|(1, 2), (1, 3), (1, 5), (1, 9), (3, 6), (3, 12), (6, 6), (6, 12), (3, 4), (3, 5), (3, 7), (7, 7), (7, 4), (5, 5), (5, 6), (8, 8), (8, 12)|
|`testCalculateColumnTotalWithNulls`|[1, 2, 3, 4, 5, 6, 7, 8, 12]|(1, 2), (1, 3), (1, 5), (1, 9), (3, 6), (3, 12), (6, 6), (6, 12), (3, 4), (3, 5), (3, 7), (7, 7), (7, 4), (5, 5), (5, 6), (8, 8), (8, 12)|

### DU-pair coverage calculation per test case
| Variable (v) | Defined at node (n) | dcu(v, n) | dpu(v, n)        |
| -------- | --------------- | --------- | ---------------- |
| data     | 1               | {2, 3, 5} | {(2, 3), (2, ∅)}               |
| data     | 1               | {2, 3, 9} | {(2, 3), (2, ∅)}               |
| column   | 1               | {5}       | {}               |
| column   | 1               | {9}       | {}               |
| r        | 3               | {5, 7}    | {(4, 5), (4, 8)} |
| r        | 7               | {5, 7}    | {(4, 5), (4, 8)} |
| r2        | 8               | {9, 11}    | {(8, 9), (8, 12)} |
| r2        | 11               | {9, 11}    | {(8, 9), (8, 12)} |
| rowCount | 3               | {}        | {(4, 5), (4, 8)} |
| total    | 3               | {6, 12}    | {}               |
| total    | 6               | {6, 12}    | {}               |
| total    | 3               | {10, 12}    | {}               |
| total    | 10               | {10, 12}    | {}               |
| n        | 5               | {6}       | {(5, 6), (5, 7)} |
| n        | 9               | {10}       | {(9, 10), (9, 11)} |
|          | Total           | CU = 26   | PU = 18           |

### Findings: We have infeasible pairs which no test cases can cover!
```java
   public static double calculateColumnTotal(Values2D data, int column) {
        ParamChecks.nullNotPermitted(data, "data");
        double total = 0.0;
        int rowCount = data.getRowCount();
        for (int r = 0; r < rowCount; r++) {
            Number n = data.getValue(r, column);
            if (n != null) {
                total += n.doubleValue();
            }
        }

        // INFEASIBLE
        for (int r2 = 0; r2 > rowCount; r2++) {
            Number n = data.getValue(r2, column);
            if (n != null) {
                total += n.doubleValue();
            }
        }
        return total;
    }
```
The commented portion of the targetted method is infeasible meaning no test cases can cover this block. As we can see, when `r2 > rowCount` the loop becomes infinite. Hence, the DU-pairs from the graph connecting this block becomes infeasible.

**Infeasible pairs in the data flow graph of the method**: (8, 9), (8, 11), (11, 11), (11, 8), (9, 9), (9, 10), (10, 10), (10, 12), etc.

### 2. `Range.ccombine(Range range1, Range range2)`
### Data flow graph

![calculateColumnTotal(Values2D data, int column)](media/seng637_assignment3_dfd_rangeCombine.drawio.png)
### Defs, Uses, and DU-pairs

|               |                                |
| ------------- | ------------------------------ |
| **defs**:     | def(1) = {range1, range2}        |
|               | def(6) = {l, u}  |
| **uses**:     | use(2) = {range1}      |
|               | use(3) = {range2}      |
|               | use(4) = {range2}      |
|               | use(5) = {range1}              |
|               | use(6) = {range1, range2, l, u}              |
| **du-pairs**: | range1: (1, 2), (1, 5), (1, 6) |
|               | range2: (1, 3), (1, 4), (1, 6)         |
|               | l: (6, 6)         |
|               | u: (6, 6)        |

### DU-pairs covered in test cases
| Test case | Execution path | DU-pairs covered |
|---|---|--|
|`testCombineWithFirstRangeNull`|[1, 2, 3]|(1, 2), (1, 3)|
|`testCombineWithSecondRangeNull`|[1, 2, 4, 5]|(1, 2), (1, 4), (1, 5)|
|`testCombineWithValidRange`|[1, 2, 4, 6]|(1, 2), (1, 4), (1, 6), (6, 6)|
|`testCombineWithBoundaryValues`|[1, 2, 4, 6]|(1, 2), (1, 4), (1, 6), (6, 6)|
|`testCombineWithBothRangesNull`|[1, 2, 3]|(1, 2), (1, 3)|

### DU-pair coverage calculation per test case
| Variable (v) | Defined at node (n) | dcu(v, n) | dpu(v, n)        |
| -------- | --------------- | --------- | ---------------- |
| range1     | 1               | {5} | {(2, 3), (2, 4)}               |
| range1     | 1               | {6} | {(2, 3), (2, 4)}               |
| range2     | 1               | {3} | {(4, 5), (4, 6)}               |
| range2     | 1               | {6} | {(4, 5), (4, 6)}               |
| l     | 6               | {6} | {}               |
| u     | 6               | {6} | {}               |
|          | Total           | CU = 6   | PU = 8           |



# 3 A detailed description of the testing strategy for the new unit test

Text…

# 4 A high-level description of five selected test cases you have designed using coverage information, and how they have increased code coverage

Text…

# 5 A detailed report of the coverage achieved of each class and method (a screenshot from the code cover results in green and red color would suffice)

Text…

# 6 Pros and Cons of coverage tools used and Metrics you report

Text…

# 7 A comparison on the advantages and disadvantages of requirements-based test generation and coverage-based test generation.

Text…

# 8 A discussion on how the team work/effort was divided and managed

Text…

# 9 Any difficulties encountered, challenges overcome, and lessons learned from performing the lab

Text…

# 10 Comments/feedback on the lab itself

Text…
