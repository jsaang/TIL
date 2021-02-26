dplyr 패키지가 1.0.0 버전으로 업데이트 되면서 많은 변화가 있었지만, 그 중에서도 `across()` 함수의 등장은 기억해두어야 한다. `across()`는 `summarise()`나 `mutate()` 같은 **data-masking** 함수들 내부에서 `select()`와 같은 방식으로 컬럼들을 선택할 수 있게 해준다. 이를 이용하면 여러 컬럼에 동일한 transformation을 적용할 수 있게 된다. (data-masking에 대해선 [이 문서](https://dplyr.tidyverse.org/reference/dplyr_data_masking.html?q=data%20masking)를 참고하자.)

기존에 존재하던 `mutate_if()`, `mutate_at()`, `mutate_all()` (또는 `summarise_`)와 같은 함수들은, 이제 `mutate()` 단일 함수와 `across()`의 조합으로 완전히 대체할 수 있게 되었다. tidyverse 팀에서도 `_if`, `_at`, `_all` 함수들을 supersede(점차 사라질) 함수로 지정하고, `across()`로 대체할 것을 권장하였으므로, 미리미리 공부해서 바꿔두는 편이 좋겠다.

사용 방법은 아주 간단히는 아래와 같다.

1. `mutate()`, `summarise()` 내에서
2. `across()`의 첫번째 인자로, 컬럼들을 선택하고,
3. 두번째 인자로, 적용할 함수를 넣어준다.

먼저 컬럼을 어떤 식으로 선택할 수 있는지 살펴보자.

## 1. First argument: Column selection

이하의 방식들은 전부 같은 결과를 반환한다.
```r
library(dplyr)

iris <- iris %>% as_tibble()

# 컬럼 이름으로
iris %>%
  mutate(across(c(Sepal.Length, Sepal.Width), round))

# 컬림 위치로
iris %>%
  mutate(across(c(1, 2), round))

# column selection 함수로
iris %>%
  mutate(across(c(starts_with("Sepal")), round))

iris %>%
  mutate(across(where(is.double) & !starts_with("Petal"), round))

# `everything()` 함수를 이용하면 `mutate_all()`과 동일한 작업을 할 수 있다.
```

## 2. Second argument: Function

선택한 컬럼에 적용할 함수도 다양한 방식으로 넣어줄 수 있다.
```r
# 일반 함수
iris %>%
  group_by(Species) %>%
  summarise(across(starts_with("Sepal"), mean))
# # A tibble: 3 x 3
#   Species    Sepal.Length Sepal.Width
#   <fct>             <dbl>       <dbl>
# 1 setosa             5.01        3.43
# 2 versicolor         5.94        2.77
# 3 virginica          6.59        2.97

# purrr style 함수
iris %>%
  group_by(Species) %>%
  summarise(across(starts_with("Sepal"), ~mean(.x)))
# # A tibble: 3 x 3
#   Species    Sepal.Length Sepal.Width
#   <fct>             <dbl>       <dbl>
# 1 setosa             5.01        3.43
# 2 versicolor         5.94        2.77
# 3 virginica          6.59        2.97

# 함수 list
iris %>%
  group_by(Species) %>%
  summarise(across(starts_with("Sepal"), list(mean = mean, sd = sd)))
# # A tibble: 3 x 5
#   Species    Sepal.Length_mean Sepal.Length_sd Sepal.Width_mean Sepal.Width_sd
#   <fct>                  <dbl>           <dbl>            <dbl>          <dbl>
# 1 setosa                  5.01           0.352             3.43          0.379
# 2 versicolor              5.94           0.516             2.77          0.314
# 3 virginica               6.59           0.636             2.97          0.322
```

이외에도 `across()`는 `.names` argument 등을 이용해, 파생 컬럼의 이름을 세팅해줄 수 있다. 이는 참고 문서를 참조하자.

## 참고 문서
- [https://dplyr.tidyverse.org/reference/across.html](https://dplyr.tidyverse.org/reference/across.html)
- [https://www.tidyverse.org/blog/2020/04/dplyr-1-0-0-colwise/](https://www.tidyverse.org/blog/2020/04/dplyr-1-0-0-colwise/)