# Error message 추출하여 column으로 만들기

[NULL 값이 있는 list column unnset 하기](https://github.com/shk5660/TIL/blob/main/R/unnest_keeping_null.md)에 이어, safely 함수로 생성된 error message를 파싱하여 column으로 만들어 보려한다.

```r
divide <- function(x, y) {
  if(y == 0) stop("denominator cannot be zero.")
  return(x/y)
}
# safely 함수로 wrapping하여 result와 error를 각각 받을 수 있게 한다.
divide_safely <- purrr::safely(divide)

tb <- tibble(x = c(2, 4), y = c(1, 0))

tb_divided <- tb %>%
  mutate(
    z = purrr::map2(x, y, ~divide_safely(.x, .y)),
    result = purrr::map(z, "result"),
    error = purrr::map(z, "error")
  )
```

unnest 함수의 `keep_empty = TRUE` 인자를 사용하면, NULL이 있는 list column(result)을 행 손실없이 파싱할 수 있었다. error에도 동일하게 `unnest(., keep_empty = TRUE)`를 사용하면 되지 않을까?

```r
tb_divided %>% unnest(error, keep_empty = TRUE)
# Error: Input must be list of vectors
```

이상하게도 에러가 발생한다. 이는 error column의 element가 `simpleError`라는 클래스에 속해있기 때문이다.

```r
tb_divided$error[[2]] %>% class()
# [1] "simpleError" "error"       "condition"  
```

따라서 이 데이터를 unnest가 아닌 다른 방법을 사용하여, 에러 메시지만을 추출해보자.
먼저 알아야 할 것은 `.$message`를 사용하면 에러 메시지만을 뽑아낼 수 있다. 이를 map 함수와 함께 사용하면 손쉽게 목적을 달성할 수 있다.

```r
tb_divided %>%
  mutate(error_message = map_chr(error, ~ if_else(is.null(.x), NA_character_, .x$message)))
#   # A tibble: 2 x 6
#       x     y z                result    error      error_message              
#   <dbl> <dbl> <list>           <list>    <list>     <chr>                      
# 1     2     1 <named list [2]> <dbl [1]> <NULL>     NA                         
# 2     4     0 <named list [2]> <NULL>    <smplErrr> denominator cannot be zero.
```