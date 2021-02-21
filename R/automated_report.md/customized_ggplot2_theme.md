회사에서 고객사들을 위한 분석 자료들을 자주 만드는데, 이 때 그래프를 그릴 때 사용할 ggplot2 테마를 만들어두자는 이야기가 나왔었다. 이 때 아래 방법을 사용하여 만들 수 있을 것 같아 기록해둔다.

```r
theme_dtr <- function(...) {
  theme_classic(
    base_family = "AppleGothic"
  ) +
  theme(
    # Text
    axis.title = element_text(size = 20, family = "AppleGothic"),
    # Line colors
    axis.ticks = element_line(color = "#464646"),
    # Area colors
    legend_background = element_rect(fill = "#252525")
  ) +
  theme(
    ...
  )
}
```