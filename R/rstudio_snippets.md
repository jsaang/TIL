Rstudio에서 제공하는 **code snippets**라는 기능을 알게 되었다. `function`이나 `for` 함수 등에 적용할 수 있는 snippet이 있다는 건 알고 있었지만, 직접 만들어 추가할 수 있다는 것을 이제야 알게 되다니... 지금부터라도 유용하게 잘 써봐야겠다.

## Snippet 등록 방법
1. Rstudio IDE 설정 창에서 `Tools -> Global Options -> Code -> Edit Snippets`로 들어간다.
2. 원하는 Snippets을 템플릿 형식에 맞게 등록한다. [참고](https://jozef.io/r906-rstudio-snippets/)
3. R sciript 또는 console에서 Snippet 입력 후, `Shift + Tab`을 누르면 완성!

무궁무진하게 활용할 수 있겠지만, 나는 가장 먼저 아래 Snippet을 등록했다. `tryCatch`문을 사용할 때마다, 형식이 헷갈려서 구글링하게 되었었는데, 이제 1초만에 사용할 수 있다...!!!

```
snippet tryc
	tryCatch({
		${1}
	}, error = function(e) {
		message(sprintf("Error in %s: %s", deparse(e[["call"]]), e[["message"]]))
		${2}
	})
```