# Zeallot 패키지로 Variable assignment하기

파이썬에선 코드 한 줄로 복수 개의 변수에 값을 할당할 수 있다.
이러한 특성은 주로 복수 개의 값을 반환하는 함수의 결과물을 받아올 때, 유용하게 사용해왔다.
R에서도 Zeallot 패키지를 사용하면 이와 동일한 편리함을 누릴 수 있다!
```r
install.packages("zeallot")
library(zeallot)
```

`zeallot` 패키지는 `%<-%` 이런 요상한 모양의 할당 연산자를 쓸 수 있게 해준다.
사용법은 매우 간단하다. 파이썬과 거의 동일하다.

```r
c(a, b) %<-% c("apple", "banana") 
# > a
# [1] "apple"
# > b
# [1] "banana"
```

위 연산자는 `purrr::safely` 함수와 굉장히 좋은 시너지를 보여준다. `safely`로 함수를 래핑해 사용하면, 결과물로 result와 error라는 elements를 가진 리스트를 반환하는데, 이를 `%<-%`를 이용해 각각 변수로 받아올 수 있게 된다.(한방에!)

```r
safe_sum <- purrr::safely(sum)
safe_sum(1, 2, 3)
# $result
# [1] 6
# $error
# NULL

safe_sum("a", "b", "c")
# $result
# NULL
# $error
# <simpleError in .Primitive("sum")(..., na.rm = na.rm): invalid 'type' (character) of argument>

c(result, error) %<-% safe_sum(1, 2, 3)
# > result
# [1] 6
# > error
# NULL
```

# 참고자료
[zeallot github README](https://github.com/r-lib/zeallot)


