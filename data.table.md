Add new column conditional on other columns. Obviously, don't use `&&` instead of `&`. `&` is a vector operation, i.e., acts on all values whereas `&&` just acts on the first value.

```
> test_dt <- data.table(a = c(-1,0,1,2,-2), b=c(T,T,F,T,F))
> test_dt
    a     b
1: -1  TRUE
2:  0  TRUE
3:  1 FALSE
4:  2  TRUE
5: -2 FALSE
> test_dt[,type := if(b & a <= -1) {"down"} else if (b & a>= 1){"up"} else {"Not Significant"}]
Error in if (b & a <= -1) { : the condition has length > 1
```
Error because apparently the operation is performed on all values together. 

Solution: Use `by=1:nrow(test_dt)`. 

```
> test_dt[,type := if(b & a <= -1) {"down"} else if (b & a>= 1){"up"} else {"Not Significant"}, by = 1:nrow(test_dt)]
> test_dt
    a     b            type
1: -1  TRUE            down
2:  0  TRUE Not Significant
3:  1 FALSE Not Significant
4:  2  TRUE              up
5: -2 FALSE Not Significant
```

