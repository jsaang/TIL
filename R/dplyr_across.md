dplyr 패키지가 1.0.0 버전으로 업데이트 되면서 많은 변화가 있었지만, 그 중에서도 `across()` 함수의 등장은 기억해두어야 한다. `across()`는 `summarise()`나 `mutate()` 같은 **data-masking** 함수들 내부에서 `select()`와 같은 방식으로 컬럼들을 선택할 수 있게 해준다. 이를 이용하면 여러 컬럼에 동일한 transformation을 적용할 수 있게 된다. (data-masking에 대해선 [이 문서](https://dplyr.tidyverse.org/reference/dplyr_data_masking.html?q=data%20masking)를 참고하자.)

기존에 존재하던 `mutate_if()`, `mutate_at()`, `mutate_all()` (또는 `summarise_`)와 같은 함수들은, 이제 `mutate()` 단일 함수와 `across()`의 조합으로 완전히 대체할 수 있게 되었다. tidyverse 팀에서도 `_if`, `_at`, `_all` 함수들을 supersede(점차 사라질) 함수로 지정하고, `across()`로 대체할 것을 권장하였으므로, 미리미리 공부해서 바꿔두는 편이 좋겠다.




## 참고 문서
- [https://dplyr.tidyverse.org/reference/across.html](https://dplyr.tidyverse.org/reference/across.html)
- [https://www.tidyverse.org/blog/2020/04/dplyr-1-0-0-colwise/](https://www.tidyverse.org/blog/2020/04/dplyr-1-0-0-colwise/)