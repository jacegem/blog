---
title: Postgresql 버전 확인
date: 2017-03-03
tags: [Postgresql]
categories:
- Application
- Database
---

# Postgresql 버전 확인

pgAdmin Query 창에서 아래 커맨드를 실행.

```sql
select version();
```

결과는 아래와 같이 나온다.

```sh
"PostgreSQL 9.5.3, compiled by Visual C++ build 1800, 64-bit"
```

## 출처

- https://www.postgresql.org/message-id/a2de01dd0808280738u32f76cffgaf5c740d12fff763@mail.gmail.com
