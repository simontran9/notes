# Asymptotic analysis

## Worst case time complexity

### Iterative algorithms

1. Calculate the total cost by summing the costs of statements where:  
    - $basic \ operation = 1$ 
    - $loop = loop \ body \ cost * number \ of \ iterations$, then express it in terms of the input $n$
3. Apply Bachmann-Landau notation simplification rules in order
   1. combine same complexity classes
   2. drop non-dominant terms
   3. ignore multiplicative constants

$\therefore t_{worst \ case}(n) \in O(f(n))$, where $f(n)$ is one of the following
| Growth Type   | Function       |
|---------------|----------------|
| Constant      | $1$            |
| Logarithmic   | $\log n$       |
| Linear        | $n$            |
| Linearithmic  | $n \log n$     |
| Quadratic     | $n^2$          |
| Cubic         | $n^3$          |
| Exponential   | $2^n$          |
| Factorial     | $n!$           |

### Recursive algorithms (TO DO)

## Worst case space complexity

**Remark** Ignore the space used by the input to an algorithm

### Iterative algorithms

1. Calculate the total cost by summing the costs of data:  
    - $variable = 1$ 
    - $ADT \ data \ structure = n$
3. Apply Bachmann-Landau notation simplification rules in order
   1. combine same complexity classes
   2. drop non-dominant terms
   3. ignore multiplicative constants

$\therefore t_{worst \ case}(n) \in O(f(n))$, where $f(n)$ is one of the following
| Growth Type   | Function       |
|---------------|----------------|
| Constant      | $1$            |
| Logarithmic   | $\log n$       |
| Linear        | $n$            |
| Linearithmic  | $n \log n$     |
| Quadratic     | $n^2$          |
| Cubic         | $n^3$          |
| Exponential   | $2^n$          |
| Factorial     | $n!$           |

### Recursive algorithms (TO DO)


