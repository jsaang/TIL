[클린한 코드를 작성하는 Tip에 대한 포스팅](https://www.r-bloggers.com/2021/03/5-tips-for-writing-clean-r-code-leave-your-code-reviewer-commentless/)에서, 패키지를 작성하는 것이 아니더라도 함수를 짤 땐 `roxygen2` 패키지를 이용하라는 팁을 보았다. 확실히 해당 패키지에서 제공하는 포맷대로 함수에 대한 주석을 작성해두면, 여러모로 편리할 것 같아 사용법을 가볍게 정리해둔다.

사용법은 아주 간단한데, 먼저 아래 포맷대로 `#'`을 이용해 함수 주석을 작성한 후, `roxygen2::roxygenise()`를 실행하면 끝이다.
실제로 좀 사용해보고 팁 등을 더 기록해두려고 한다.

```r
#' Add together two numbers
#'
#' @param x A number
#' @param y A number
#' @return The sum of \code{x} and \code{y}
#' @examples
#' add(1, 1)
#' add(10, 1)
add <- function(x, y) {
  x + y
}
```

### References
- [https://cran.r-project.org/web/packages/roxygen2/vignettes/roxygen2.html](https://cran.r-project.org/web/packages/roxygen2/vignettes/roxygen2.html)