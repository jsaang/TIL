## Problem
- `glue` 함수를 이용해 텍스트를 생성하려 하는데, 할당된 변수 중 일부가 NA 값이다.
- 이 때 NA인 값은 아예 결과에 나타나지 않게 하고 싶다.("")

## Solution
- `.na` 옵션을 사용한다.
- `.na = ""`로 세팅하면, NA인 변수 값을 표기하지 않는다.
- Defalut로는 `.na = "NA"`로 되어있다.

```r
a <- "apple"
b <- "banana"
c <- NA_character_

glue::glue("{a} + {b} = {c}")
# apple + banana = NA

glue::glue("{a} + {b} = {c}", .na = "")
# apple + banana = 
```