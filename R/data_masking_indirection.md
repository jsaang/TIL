# Data-masking indirection 활용

`dplyr` 함수들은 Data-masking을 활용해, 현재 사용하고 있는 데이터의 변수명들을 **직접** 참조할 수 있게 해준다. `diamonds[diamonds$x == 0 & diamonds$y == 0 & diamonds$z == 0, ]`가 아니라 `filter(diamonds, x == 0 & y == 0 & z == 0)`로 사용할 수 있게 해주는 것이다. 이런 방식은 매우 편리하지만, 사용하다보면 다소 불편한 점이 있었다. 바로 custom function을 만들 때와(dplyr 함수를 활용하여), character vector를 dplyr 함수의 인자로 사용하고 싶을 때였다. 그동안은 expression을 quote했다가 다시 unquote해주는 방식 등으로 우회하여 사용해왔으나, 훨씬 편리한 방법을 알게 되어 정리해둔다.

### 1. 함수의 인자로 변수명을 넣어주고 싶은 경우
`filter(df, {{ var }})`처럼 double culry brace로 변수명을 wrapping 해준다.
```r
dist_summary <- function(df, var) {
  df %>%
    summarise(n = n(), min = min({{ var }}), max = max({{ var }}))
}
mtcars %>% dist_summary(mpg)
mtcars %>% group_by(cyl) %>% dist_summary(mpg)
```

### 2. Character vetor를 컬럼명으로 사용하고 싶은 경우
`summarise(df, mean = mean(.data[[var]]))`처럼 `.data` 접두사를 사용한다.
```r
for (var in names(mtcars)) {
  mtcars %>% count(.data[[var]]) %>% print()
}

lapply(names(mtcars), function(var) mtcars %>% count(.data[[var]]))
```

### 참고 자료
[https://dplyr.tidyverse.org/dev/reference/dplyr_data_masking.html](https://dplyr.tidyverse.org/dev/reference/dplyr_data_masking.html)