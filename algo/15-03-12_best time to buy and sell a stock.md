# Q1 : Buy once and sell once

Simple DP. 

```cpp

  max_prices = max(prices[j] for i < j < n) 
  max_profit_after[i] = max(
      max_profit_after[i+1], // if not buy at i
      max_prices - prices[i] // if buy at i
      )

```


# Q2 : Buy and sell any number of times

## Method 1

For one buying/selling cicle:

When should we buy?

 - buy at i: v[i] < v[i+1]. 
   Suppose total profit of not buying at v[i] is ~p[i]
   Then ~p[i] + v[i+1] - v[i] > ~p[i].

When should we sell?
  
  - suppose we should sell at j. Then we know v[i] < v[j].
    Suppose at this time, the total profit is max_p. 
    Then max_p = v[i+1] - v[i] + (profit of selling at i+1 and buying again at i+1)
    So we can sell at j, which will achieve the same max profit.

## Method 2

Intuition:
  
  Considering [i..n). What is the max profit if we buy and sell any number of times in-between?
  
  - Suppose max_profit_after[i] equals to the value. max_profit_if_buying_at[i] is the max profit if we buy at i. max_profit_if_not_buying_at[i] is the max profit if we
  do not buy at i. Then apparently `max_profit_after[i] = std::max(max_profit_if_buying_at[i], max_profit_if_not_buying_at[i])`.

  - Also, max_profit_if_not_buying_at[i] = max_profit_after[i+1]

  - max_profit_if_buying_at[i] = max{max_profit_after[j] + prices[j] - prices[i] for i < j < n}. The time complexity to get the value is O(n-i). We optimize this with another
  dp array max_profit_if_sell_after[i] = max{max_profit_after[j] + prices[j], for i < j < n}. It means if fixing the buying point i, the max profit we can get if we ignore the buying money spent at i.
  Therefore, 
  
  ```cpp
  
  max_profit_if_buying_at[i] = max_profit_if_sell_after[i] - prices[i];

  ```
  
  - we can maintain the max_profit_if_sell_after[i] dynamically
  

So we have two dp arrays

1. max_profit_after[i]: maximum profit in [i..n) when we buy and sell any number of times in-between.
2. max_profit_if_sell_after[i]: For some fixed buying point k < i, the maximum profit in [i..n) if we sell the product after i(in [i..n)) without considering money spent
when buying at point k. Therefore, max_profit_if_sell_after[i] - prices[k] is the max profit we can get if we buy at point k.

Then we have:

```cpp
  max_profit_after[n-1] = 0 // we have only one buying/selling point in [n-1..n). So we can make at most 0 money.
  max_profit_if_sell_after[n-1] = prices[n-1] // max money we can get if we sell at n-1.

  max_profit_after[i] = max(
      max_profit_after[i+1],  // if we do not buy at i
      max_profit_if_sell_after[i+1] - prices[i] // if we buy at i
      );
  max_profit_if_sell_after[i] = max(
      max_profit_if_sell_after[i+1], // if we do not sell at i
      max_profit_after[i+1] + prices[i] // if we sell at i
      );

```

# Q3 Buy in at most two transactions


## method 1

This can be constructed based on Q1 and Q2

```cpp
max_profit_in_at_most_two_trans = max(max_profit_in_one_trans, max_profit_in_two_trans)

// max_profit_in_one_trans is solved in Q1.

max_profit_in_two_trans[i] = max(
  max_profit_in_two_trans[i+1], // if not buy at i
  max(max_profit_in_one_trans[j] + prices[j] - prices[i], for i < j < n) // if buy at i
  );
  
max_profit_if_sell_after[i] = max(max_profit_in_one_trans[j] + prices[j], for i < j < n)

```


# Q4 : Complete in at most k transactions

## method 1


```cpp

max_profit_in_trans[k][i] = max(
  max_profit_in_trans[k][i+1], // if not buy at i
  max_profit_if_sell_after[k-1][i] - prices[i] // if buy at i
  );

max_profit_if_sell_after[k][i] = max(
  max_profit_if_sell_after[k][i+1], // if not sell at i
  max_profit_in_trans[k-1][i] + prices[i] // if we sell at i
  );


```


