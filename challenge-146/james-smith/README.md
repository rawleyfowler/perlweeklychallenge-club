[< Previous 145](https://github.com/drbaggy/perlweeklychallenge-club/tree/master/challenge-145/james-smith) |
[Next 147 >](https://github.com/drbaggy/perlweeklychallenge-club/tree/master/challenge-147/james-smith)
# Perl Weekly Challenge #146

You can find more information about this weeks, and previous weeks challenges at:

  https://theweeklychallenge.org/

If you are not already doing the challenge - it is a good place to practise your
**perl** or **raku**. If it is not **perl** or **raku** you develop in - you can
submit solutions in whichever language you feel comfortable with.

You can find the solutions here on github at:

https://github.com/drbaggy/perlweeklychallenge-club/tree/master/challenge-146/james-smith

# Challenge 1 - 10001st Prime Number

***Write a script to generate the 10001st prime number.***

## The solution

We could use a Prime module, but finding primes is not that difficult so we will roll our own generator:

```perl
my($c,@p)=(5,3);
for(;@p<10000;$c+=2){
  ($_*$_>$c)?((push@p,$c),last):$c%$_||last for@p;
}
say$p[-1];
```

The crux of the code is in the `for @` line. This sees if a given odd number is prime.

We loop through all the primes up to and including the square root of the value we are checking.
If we don't find a prime factor by then we push the new value to the primes list, and go on to
try the next number. If we find a
factor we skip the rest of the loop and go on to try the next number.

We stop when we have 10,000 records in the array (as we don't include the prime number 2 in the list),
so the last element is the 10,001st prime.

# Challenge 2 - Curious Fraction Tree

***Can't really describe this - best to look at the image on the website at https://theweeklychallenge.org/blog/perl-weekly-challenge-146/.***

```
                                     1/1
                                      |
                  +-------------------+-------------------+
                 1/2                                     2/1
                  |                                       | 
        +---------+---------+                   +---------+---------+
        |                   |                   |                   |
       1/3                 3/2                 2/3                 3/1
        |                   |                   |                   |
   +----+----+         +----+----+         +----+----+         +----+----+
   |         |         |         |         |         |         |         |
  1/4       4/3       3/5       5/2       2/5       5/3       3/4       4/1
   |         |         |         |         |         |         |         |
 +-+-+     +-+-+     +-+-+     +-+-+     +-+-+     +-+-+     +-+-+     +-+-+
 |   |     |   |     |   |     |   |     |   |     |   |     |   |     |   |
1/5 5/4   4/7 7/3   3/8 8/5   5/7 7/2   2/7 7/5   5/8 8/3   3/7 7/4   4/5 5/1
```

For a given node `n/d` the children are `n/(n+d)` and `(n+d)/d`.

## The solution

We note that the left-child is always less than one, the right child is always greater than 1.

* To get the parent of the left child we note that `n+d = D` and `n = N`, so the parent denominator is `N-D` and numerator doesn't change
* To get the parent of a right child we note that `n = N+D` and `d = D`, so the parent numerator is `N-D` and denominator doesn't change.
* For all nodes the numerator/denominator are co-prime.

If it is a member of the tree we repeat until both `n` & `d` are 1. If the node is such that the numbers aren't coprime we eventually stop when 
We repeat this until we get to the top of the tree where both the denominator and numerator are less than 2. (In the tree is always 1/1) as all tree members have co-prime numerators and denominators. Other values end when the numerator is 0.

The `stringify` function just converts the tree into a single string (list of fractions) so we can test the tree code.

```perl
sub tree {
  my@tr=[my($n,$d)=@_];
  push@tr,$d>$n?[$n,$d-=$n]:[$n-=$d,$d]while$n*$d>1;
  \@tr;
}

sub stringify {
  "@{[map{join'/',@{$_}}@{$_[0]}]}";
}
```
