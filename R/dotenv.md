# dotenv 패키지로 credential 정보 분리하기

Github 등에 작업물을 올릴 때, 코드 내에 민감한 정보(DB 계정 정보 등)가 들어있다면 곤란하다. 회사 Github repos에 업로드 되어있던 Credential 정보들이 있었기 때문에, 이를 코드로부터 분리하는 방법을 찾아보았다. 여러 방법이 있겠지만, R에서는 `dotenv` 패키지를 사용하면, Credential 정보를 `.env` 파일로 분리하여 환경 변수로 빼내어 사용할 수 있다.


아래와 같이 DB 계정 정보가 담긴 DB connection 코드가 있다고 해보자. 이 코드가 고스란히 Github에 노출되어선 곤란하다.

```r
my_connect <- dbConnect(
  host = "jsang_host_adress", 
  user = "jsang",
  password = "12345678"
)
```

`.env`라는 이름으로 아래와 같이 계정 정보가 담긴 텍스트 파일을 만들고, `.gitignore` 파일에 추가해둔다.
(참고로 아래 텍스트 파일을 생성할 때 = 앞뒤로 띄어쓰기가 있으면 안된다.)

```
HOST="jsang_host_adress", 
USER="jsang",
PW="12345678"
```

이 후 `dotenv` 패키지의 `load_dot_env()` 함수를 사용하면, `Sys.getenv()`를 통해 환경 변수를 불러올 수 있다.

```r
library(dotenv)

load_dot_env(".env")

Sys.getenv("HOST")
# [1] "jsang_host_adress"
Sys.getenv("USER")
# [1] "jsang"
Sys.getenv("PW")
# [1] "12345678"
```

이제 이를 활용해 DB connection에서 계정 정보를 분리한다.

```r
my_connect <- dbConnect(
  host = Sys.getenv("HOST"), 
  user = Sys.getenv("USER"),
  password = Sys.getenv("PW")
)
```

## 참고 자료
[https://towardsdatascience.com/using-dotenv-to-hide-sensitive-information-in-r-8b878fa72020](https://towardsdatascience.com/using-dotenv-to-hide-sensitive-information-in-r-8b878fa72020)