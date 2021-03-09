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

테스트는 `files > tests > expectations`의 계층적 구조로 이루어져 있다.

- **expectation**은 테스트의 가장 기본적인 단위이다. 올바른 값과 클래스를 반환했는가? 에러가 났을 때 올바르게 에러 메시지를 반환하는가? 등과 같이 예상되는 결과 값을 표현한다. `expect_`로 시작되는 함수를 사용한다.
- **test**는 여러 개의 expectation들을 하나의 함수에 대한 테스트 단위로 묶는다. 함수에 적용할 수 있는 여러가지 다양한 parameter 상황에 대해 일괄적으로 테스트를 시행한다. 이렇게 한 기능에 대해 테스트를 진행하기 때문에, **unit**이라고 부르기도 한다. 테스트는 `test_that` 함수를 사용하여 만들 수 있다.
- **file**은 연관있는 여러 test들을 모아 놓은 것이다. `context()` 함수로 사람이 읽을 수 있도록 이름을 붙여준다.

### Expectations
- `expect_`로 시작한다.
- 두개의 인자를 가진다. 첫번째 인자는 실제 결과값, 두번째 인자는 예상되는 결과값이다.
- 만약 실제 결과와 예상되는 결과가 일치하지 않는다면, testthat은 에러를 발생시킨다.

빈번하게 사용되는 `expect_` 함수들을 알아보자.
- equality를 판단할 때에는 `expect_equal()`이나 `expect_identical()`을 사용한다. `equal()` 함수는 단순 값의 동일함 여부만 판단하고, `identical()`은 값뿐만 아니라, 클래스까지 같아야 한다.(`identical()`이 훨씬 까다롭게 판단하는 것이다.)
- `expect_match()`는 정규 표현식을 이용해 문자열의 매칭 여부를 판단해준다. (`all`, `ignore.case = TRUE`, `fixed = TRUE` 등의 옵션을 사용할 수 있다.)
- `expect_match()`에서 파생된 네 가지 함수가 있다. 
  - `expect_output()`: about printed output
  - `expect_message()`: about message
  - `expect_warning()`: about warning
  - `expect_error()`: about error
- 원하는 작업을 수행하는 함수가 없다면 `expect_true()`, `expect_false()`를 사용하면 된다.

## Writing tests
각 테스트는 반드시 의미있는 이름이 있어야 하고, 한 단위의 기능을 다루어야 한다. 그래야 테스트가 실패했을 때, 무엇이 문제이며 코드의 어느 곳을 보아야 하는지 빠르게 파악할 수 있다. 한 테스트에 너무 많은 expectations를 넣는 것은 피하자.

### What to test
테스트를 작성할 땐 밸런스 게임을 해야한다. 테스트는 부주의하게 코드를 바꿀 가능성을 낮춰주지만, 목적에 맞게 코드를 수정하는 것도 어렵게한다. 다음 포인트들이 도움이 될 것이다.
- 함수의 external interface를 테스트하는 것에 집중하라. 뭔소리여.
- 하나의 행동을 오직 하나의 테스트에서 테스트하도록 해라. 그러면 나중에 하나의 행위에 대해 코드를 바꿨을 때, 하나의 테스트만 수정하면 된다.
- 작동할거라고 확신하는 간단한 코드에 대해선 굳이 테스트하지 말아라.
- 버그를 발견하면 테스트를 작성하는 습관을 들여라. test-first 마인드를 탑재하면 아주 좋다. 항상 테스트를 먼저 작성하고, 테스트를 통과하도록 코드를 짜는 습관을 들이는 것이다.

### Building your own testing tools

아래의 테스트 코드엔 중복이 많아 가독성과 관리의 용이성이 많이 떨어진다. 중복되는 행위들을 뽑아내어 새로운 함수로 만들어버리면 중복을 줄일 수 있다.

```r
library(lubridate)
#> 
#> Attaching package: 'lubridate'
#> The following objects are masked from 'package:base':
#> 
#>     date, intersect, setdiff, union
test_that("floor_date works for different units", {
  base <- as.POSIXct("2009-08-03 12:01:59.23", tz = "UTC")

  expect_equal(floor_date(base, "second"), 
    as.POSIXct("2009-08-03 12:01:59", tz = "UTC"))
  expect_equal(floor_date(base, "minute"), 
    as.POSIXct("2009-08-03 12:01:00", tz = "UTC"))
  expect_equal(floor_date(base, "hour"),   
    as.POSIXct("2009-08-03 12:00:00", tz = "UTC"))
  expect_equal(floor_date(base, "day"),    
    as.POSIXct("2009-08-03 00:00:00", tz = "UTC"))
  expect_equal(floor_date(base, "week"),   
    as.POSIXct("2009-08-02 00:00:00", tz = "UTC"))
  expect_equal(floor_date(base, "month"),  
    as.POSIXct("2009-08-01 00:00:00", tz = "UTC"))
  expect_equal(floor_date(base, "year"),   
    as.POSIXct("2009-01-01 00:00:00", tz = "UTC"))
})
#> Test passed 🥇
```

이제 실제값과 예상값이 한 줄에 딱 들어가기 때문에 비교하기 쉬워졌다.

```r
test_that("floor_date works for different units", {
  base <- as.POSIXct("2009-08-03 12:01:59.23", tz = "UTC")
  floor_base <- function(unit) floor_date(base, unit)
  as_time <- function(x) as.POSIXct(x, tz = "UTC")

  expect_equal(floor_base("second"), as_time("2009-08-03 12:01:59"))
  expect_equal(floor_base("minute"), as_time("2009-08-03 12:01:00"))
  expect_equal(floor_base("hour"),   as_time("2009-08-03 12:00:00"))
  expect_equal(floor_base("day"),    as_time("2009-08-03 00:00:00"))
  expect_equal(floor_base("week"),   as_time("2009-08-02 00:00:00"))
  expect_equal(floor_base("month"),  as_time("2009-08-01 00:00:00"))
  expect_equal(floor_base("year"),   as_time("2009-01-01 00:00:00"))
})
#> Test passed 🥳
```

non-standard evaluation을 이용하면 더욱 간소화할 수 있다.

```r
test_that("floor_date works for different units", {
  as_time <- function(x) as.POSIXct(x, tz = "UTC")
  expect_floor_equal <- function(unit, time) {
    eval(bquote(expect_equal(floor_date(base, .(unit)), as_time(.(time)))))
  }

  base <- as_time("2009-08-03 12:01:59.23")
  expect_floor_equal("second", "2009-08-03 12:01:59")
  expect_floor_equal("minute", "2009-08-03 12:01:00")
  expect_floor_equal("hour",   "2009-08-03 12:00:00")
  expect_floor_equal("day",    "2009-08-03 00:00:00")
  expect_floor_equal("week",   "2009-08-02 00:00:00")
  expect_floor_equal("month",  "2009-08-01 00:00:00")
  expect_floor_equal("year",   "2009-01-01 00:00:00")
})
#> Test passed 🎊
```

이렇게 `testthat` 패키지를 이용해 테스트 코드를 구성하는 법을 가볍게 정리해보았다. 대충 어떻게 쓰는지는 알았지만, 실제로 아직 사용해보지 못해 감이 잡히진 않는다. 좀 더 실제적인 적용 사례를 알아보기 위해 주요 패키지들의 테스트 코드를 뜯어보는 것도 좋은 공부가 될 것 같다.