주기적으로 작동하는 스크립트를 작성하며 매번 버그를 수정하는 것을 반복하다보니, 테스트 코드의 필요성을 절실히 느끼게 되었다. [해당 문서](https://r-pkgs.org/tests.html)를 번역/정리하며 `testthat` 패키지를 이용해 unit test를 하는 방법을 익히고 업무에 적용해보고자 한다.

## The goal of testing
> I started using automated tests because I discovered I was spending too much time re-fixing bugs that I’d already fixed before.

궁극적인 목적은 결국 시간을 아끼기 위함이다. 추가적으로 아래와 같은 이점을 누릴 수 있다.

- **버그가 적어진다.** 코드가 어떻게 작동해야만 하는지 이중으로 명시하기 때문이다.(실제 코드와 테스트 코드)
- **코드 구조가 더 나아진다.** 더 잘 디자인된 코드일 수록 테스트하기도 쉬운 법이다. 코드를 테스트하기 위해선, 어쩔 수 없이 코드에서 복잡한 부분들을 각각 작동하는 여러개의 함수들로 분리해야만 한다. 코드의 중복이 줄어들기에, 함수들은 테스트하고, 이해하고, 사용하기 편해진다.
- **재시작하기 쉬워진다.**(이 부분은 무슨 말인지 잘 이해가 안된다.) failing 테스트로 코딩을 마무리한다면, 다음에 다시 시작할 때 테스트가 무엇을 해야할지 잘 알려줄 것이다.
- **코드가 Robust해진다.** 작성한 코드의 주요 기능들에 대한 테스트가 갖춰져 있음을 인지한다면, 큰 두려움 없이 큰 변화를 시도할 수 있게 된다.

## Test structure
테스트 파일은 `test/testthat/` 디렉토리 안에, `test`로 시작하는 이름으로 생성해야 한다. 다음은 `stringr` 패키지 테스트 파일의 예시이다.
```r
context("String length")
library(stringr)

test_that("str_length is number of characters", {
  expect_equal(str_length("a"), 1)
  expect_equal(str_length("ab"), 2)
  expect_equal(str_length("abc"), 3)
})
#> Test passed 😀

test_that("str_length of factor is length of level", {
  expect_equal(str_length(factor("a")), 1)
  expect_equal(str_length(factor("ab")), 2)
  expect_equal(str_length(factor("abc")), 3)
})
#> Test passed 🎉

test_that("str_length of missing is missing", {
  expect_equal(str_length(NA), NA_integer_)
  expect_equal(str_length(c(NA, 1)), c(NA, 1))
  expect_equal(str_length("NA"), 2)
})
#> Test passed 🥇
```
