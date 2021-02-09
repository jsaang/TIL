# NULL 값이 있는 list column Unnest 하기

### Problem
- `tidyr::unnest` 함수를 이용해 tibble의 list column을 펼치려 한다.
- 이 list column에는 중간중간 NULL이 섞여있다.
- 이 때 unnest 함수를 사용할 경우, NULL이 포함된 row는 날아가버린다.
- 하지만, NULL이 있는 row도 유지하며 unnest를 하고 싶다.

### Solution
- `keep_empty = TRUE` 옵션을 설정해주면, NULL이 있어도 유지해준다.

사실 NULL이 섞인 list column을 unnest하는 상황이 잘 생기진 않겠지만, 만약 `purrr::safely` 함수 등을 사용해 tibble에 result와 error column을 만들어 에러 관리를 하고 싶다면 이 옵션을 유용하게 사용할 수 있을 것이다. 간단한 예시를 하나 만들어보았다.

```r
# 나누기 함수를 custom function으로 정의하였다.
# stop 함수를 사용하여 분모에 0이 들어오면 error 발생시키게 하였다.
# 기본 나누기 함수(/)는 분모에 0이 들어가면 Inf 값을 반환한다.
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

tb_divided
# # A tibble: 2 x 5
#       x     y z                result    error     
#   <dbl> <dbl> <list>           <list>    <list>    
# 1     2     1 <named list [2]> <dbl [1]> <NULL>    
# 2     4     0 <named list [2]> <NULL>    <smplErrr>

# 분모에 0이 들어간 상황에선 result에 NULL이, error에 <smplErrr>가 들어간 것을 확인할 수 있다.
# 이 때 result 값을 눈으로 확인하기 위해선 unnest를 해주어야 한다.

tb_divided %>% unnest(result)
# # A tibble: 1 x 5
#       x     y z                result error 
#   <dbl> <dbl> <list>            <dbl> <list>
# 1     2     1 <named list [2]>      2 <NULL>

# 그냥 unnest를 하자, NULL이 들어있던 두번째 행이 사라져버렸다.
# error 관리를 하기 위함인데 이렇게 데이터가 날아가면 곤란하다.

tb_divided %>% unnest(result, keep_empty = TRUE)
# # A tibble: 2 x 5
#       x     y z                result error     
#   <dbl> <dbl> <list>            <dbl> <list>    
# 1     2     1 <named list [2]>      2 <NULL>    
# 2     4     0 <named list [2]>     NA <smplErrr>

# keep_empty = TRUE 옵션을 사용하면 위와 같이 NULL이 있던 row도 보존할 수 있다.
```