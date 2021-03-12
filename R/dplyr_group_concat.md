## Problem
- SQL의 group_concat 함수와 같은 역할을 dplyr에선 어떻게 구현할 수 있을까?
- 즉, 아래와 같은 As-is 데이터 형태를 어떻게 To-be 형태로 바꿀 수 있을까?

```r
## As-is

#  A tibble: 5 x 2
#   key    value 
#   <chr>  <chr> 
# 1 fruit  apple 
# 2 fruit  banana
# 3 fruit  orange
# 4 animal dog   
# 5 animal cat   

## To-be

# # A tibble: 2 x 2
#   key    values               
#   <chr>  <chr>                
# 1 animal dog, cat             
# 2 fruit  apple, banana, orange
```

## Solution
- `group_by()` 함수와 `paste()` 함수의 `collapse` 옵션을 조합하면 쉽게 할 수 있다!!
  
```r
ex_data %>%
  group_by(key) %>%
  summarise(values = paste(value, collapse = ", "))
```