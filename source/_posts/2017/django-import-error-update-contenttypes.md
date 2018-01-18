---
title: 'ImportError: cannot import name `update_contenttypes`'
date: 2017-01-15 19:13:45
tags: [python, django]
categories:
- Programming
- Python
---

# ImportError: cannot import name 'update_contenttypes'

장고에서 update_contenttypes 임포트 에러 발생시

django.contrib.contenttype.`apps.py` 를 수정합니다.

django `1.11` 버전에서 생기는 오류 인것으로 보입니다.

```python
from .management import (
    inject_rename_contenttypes_operations,
    # update_contenttypes,
    create_contenttypes,
)
```

기존의 `update_contenttypes` 대신 `create_contenttypes`로 수정합니다.


```python
def ready(self):
    pre_migrate.connect(inject_rename_contenttypes_operations, sender=self)
    # post_migrate.connect(update_contenttypes)
    post_migrate.connect(create_contenttypes)
    checks.register(check_generic_foreign_keys, checks.Tags.models)
```

마찬가지로, `update_contenttypes` 대신 `create_contenttypes`로 수정합니다.



## 출처
- https://code.djangoproject.com/ticket/28092#no1
