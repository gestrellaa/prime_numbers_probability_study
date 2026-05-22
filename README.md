# Author
Eduardo Matte Zanardo dos Santos

# Introduction

The intention [1] of this article is to analyse [2] the efficiency of algorithms that answear [3] the following questions:<br>

1. "Given a number N, what is the probability of a prime number to be randomly chosen in the interval [4] [1,N]?"<br>
2. "How does the function of this probability depending on N look like?"<br>

In order to do that, the trivial solution is implemented as well as the optmized one, both [5] of them will be explained and analyzed [2] in the following sections.

[1] "intention" -> "aim"/"purpose"; idiomatic <br>
[2] "analyse", "analyze" -> "analyze"/"analyzed" American or "analyse"/"analysed" British English; opt for one, but don't use both (eu sugiro o americano até pq tu está usando "optmize" depois  <br>
[3] "answear" -> "answer"; spelling)<br>
[4] "of a prime number to be randomly chosen in the interval" -> "that a prime number will be randomly chosen in the interval"; phrasing)<br>
[5] "one. Both of them..." punctuation (em inglês a minha impressão é que eles quebram mais as frases do que a gente, evitando o que eles chamam de "comma splice"). <br>


# Trivial Solution

### Algorithm

The idea of the trivial solution is to iterate over all numbers from 2 to N and, for each of them, check whether it is prime. If so, a counter variable is incremented. So, at the end of all iterations, the division of this counter by N will return the probability of a randomly chosen number in this range to be [1] prime.

To find out whether a number X is prime, X shall be divided by all numbers from 2 until its integer half or until the rest of the division is zero [2]. It is not necessary to divide X by any number greater than its half because the greatest divisor of a number, without considering the number itself, is its half.

[1] "to be prime" -> "being prime", idiomatic<br>
[2] "rest" -> "remainder", mathematical terminology

### Pseudocode

The whole algorithm can be written by this: [1]

```text
    function is_prime(n):
        if n == 1:
            return False
        for i in [2, n//2]:
            if n mod i == 0:
                return False
        return True

    primes_count <- 0
    for i in [1, N]:
        if is_prime(i):
            primes_count++
    
    probability <- primes_count / N
```
[1] "by this" -> "as follows", idiomatic

### Complexity Analysis

This algorithm iterates over all numbers in the interval between 1 and N and, for each of them, which will be called by auxiliar here for better understanding, iterates, in the worst case scenario, over aux divided by 2 [1]. This leads to a complexity of O(N*aux/2), which can be generalized for O(n²). In other words, this algorithms [2] has a problem of scalability, because it is very slow when applied to a high value of N.

[1] Not sure I understand the second half of the sentence about the auxiliar. To me it was a little ambiguous what it meant, but I think a small tweak like this can be useful:
    "..." -> "This algorithm iterates over all numbers in the interval between 1 and N. For each number, which will be called auxiliar here for clarity, it iterates, in the worst-case scenario, up to auxiliar / 2 times." <br>
[2] "this algorithms" -> "this algorithm", grammar

# Optimized Solution

### Algorithm

To understand the optimized algorithm, it is necessary to keep in mind the principle that, in a multiplication, the order of the factors does not change the product. The idea of this algorithm consists in, instead of counting how many numbers between 1 and N are prime, saving all the non prime numbers [1] between 1 and N, and then divide N [2] minus the count of non prime numbers by N, using the complementary logic.

In order to save all this [3] non prime numbers, the algorithm iterates over all the numbers between 2 and the integer square root of N, that will be called by i, and, for each i, it is made a new iteration [4] over all the numbers between i and the integer division of N by i. The products of the multiplication of i and each of these numbers are added to the non prime numbers list, which is initialized with only the number 1, if and only if this product is not in the list yet.

However, it is very important to understand why it is possible to start the second iteration by the number itself, eg: [5] 4\*4, 4\*5, 4\*6 and so on. As mentioned in the first paragraph, the order of the factors does not change the product. Because of that, using the same example, it is not necessary to do 4\*2 and 4\*3, because 2\*4 and 3\*4 were already calculated in the iterations of 2 and 3. For the same reason, the first iteration can be stopped at the integer square root of N because, if the second iteration is started by the same current number of first iteration, the greatest number that can be multiplied by itself without exceeding N is the integer square root of N.

After applying all these multiplications and saving them in a list, this list must contain all the non prime numbers between 1 and N, as all combinations of multiplications that return a number in this interval were considered. Because of that, it is reasonable to conclude that the counting of non prime numbers can be defined as N minus the length of the list mentioned and the probability is this subtraction divided by N.

[1] "non prime numbers" -> "non-prime numbers" (melhor fazer contrl+f aqui), idiomatic (eles também dizem "non-linear equation", "non-zero value", "non-trivial solution"<br>
[2] "divide" -> "dividing", parallelism <br>
[3] "this" -> "these", spelling<br>
[4] "it is made a new iteration" -> "it makes a new iteration", grammar
[5] "eg:" -> "e.g.,", spelling (bem útil e fica bonito quando alguém sabe usar)<br>

### Pseudocode

The whole algorithm can be written as following [1]:

```text
    non_primes <- [1]
    for n in [2, integer(√N)]:
        for aux in [n, N//n]:
            product <- n * aux
            if product not in non_primes:
                non_primes <- non_primes + [product]
    
    probability <- (N-length(non_primes)) / N
```

[1] "as following" -> "as follows", idiomatic

### Complexity Analysis

This algorithm iterates over all numbers between 2 and the integer square root of N and, for each of these numbers, called by auxiliar for better understanding, iterates over all numbers between auxiliar and the integer division of N by auxiliar. In other words, it can be described as O((N√N)/aux - aux√N), which can be generalized for O(n√n). 

# Probabilities Function

In order to obtain the shape of the probabilities function, all the results of the optimized algorithm from N going from 2 untill 5000 and plotted in a line graph. It is show in the file `results.png`.

# Results

To see in real world the difference between these two algorithms [1], both of them was [2] executed for all numbers between 2 and 5000 and the time each of them took to run was saved in a list. In the end of the experiment [3], while the optimized algorithm was executing instantly, the trivial one was taking 0.02 seconds. The greatest number chosen was 5000 because, after this limit, the trivial algorithm was taking so much time and the experiment would be much slower. All the graphs can be found in the file `results.png`.

All of the experiments were executed in an Intel I7 processor.

[1] "To see in real world the difference between these two algorithms" -> "To see the difference between these algorithms in the real world"
[2] "was" -> "were" ("both of them were" pois é plural, para ser singular poderia ser "each of them was"
[3] "In the end of the experiment" -> "At the end of the experiment", phrasing 

# Conclusion

This study has implemented successfully a more scalable algorithm for the problem of the probability of a random number chosen between 1 and a given N to be prime. Besides that, the code succeds in providing the shape of the equation of these probabilities in function of N.
