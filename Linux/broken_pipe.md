## Problem
터미널에서 ssh를 이용해 서버에 접속하여, 작업 시간이 제법 긴 스크립트를 하나 돌리고 있었는데,
`client_loop: send disconnect: Broken pipe`라는 메시지가 뜨며 서버와의 연결이 끊겨버렸다.

## Solution
1. 터미널에서 `nano ~/.ssh/config`를 입력한다.(text editor는 뭘 쓰든 상관없음)
2. 아래 내용을 config 파일에 넣어준다.
   ```
   Host *
   ServerAliveInterval 120
   TCPKeepAlive no
   ```

참조 : [https://may0301.tistory.com/10](https://may0301.tistory.com/10)