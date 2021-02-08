# dplyr::mutate 안에서 purrr::pmap 사용하기

`purrr` 패키지의 `map` 관련 함수들은 가뜩이나 헷갈리는데,
`dplyr::mutate` 함수 안에서 사용할때는 써도써도 구글링을 하게 된다.
오늘 마침 업무하며 `pmap` 함수를 사용하여 한 번 정리해본다.

- `pmap` 함수는 복수개의 인자를 받는 함수의 iteration에 사용된다.
- `list(arg1, arg2, ...)`의 형태로 인자를 받아오고,
- `~function(..1, ..2, ...)`의 형태로 함수에 넣어준다.

예시를 만들어보자. 상황은 다음과 같다.
1. R에서 흔히 사용하는 tibble 데이터에서
2. 각 column 값을 인자로 사용하는 custom function을 정의하여
3. 이 결과 값을 `dplyr::mutate` 함수를 이용해 새 column으로 만들자!

```r
desktop <- tibble(
  name = c("kim", "park"),
  cpu = c("5700x", "i7-10900k"),
  gpu = c("gtx-1660s", "rtx-3080")
)

test_func <- function(x, y, z) {
  print(paste0("NAME: ", z))
  print(paste0("CPU: ", y))
  print(paste0("GPU: ", y))
  return(paste0(x, "'s desktop consists of ", y, " and ", z))
}

desktop %>%
  mutate(sentence = purrr::pmap(list(name, cpu, gpu), ~test_func(..1, ..2, ..3)))

# [1] "NAME: gtx-1660s"
# [1] "CPU: 5700x"
# [1] "GPU: 5700x"
# [1] "NAME: rtx-3080"
# [1] "CPU: i7-10900k"
# [1] "GPU: i7-10900k"
# # A tibble: 2 x 4
#   name  cpu       gpu       sentence 
#   <chr> <chr>     <chr>     <list>   
# 1 kim   5700x     gtx-1660s <chr [1]>
# 2 park  i7-10900k rtx-3080  <chr [1]>
```

