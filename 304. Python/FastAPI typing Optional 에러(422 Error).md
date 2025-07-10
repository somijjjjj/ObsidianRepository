---
tistory: https://jn-silveryoung.tistory.com/16
tags:
  - publish/done
---
```python
from pydantic import BaseModel from typing import Optional, List

class DocumentTextAnalysisArgs(BaseModel):
	category_name: Optional[str] = None
	stop_pos_tags: Optional[list] = None
```

여기서 **`Optional`** 필드에 기본값 None을 지정했습니다. 이렇게 하면 만약 클라이언트에서 해당 필드를 요청에 포함하지 않으면 이 필드는 None 값으로 설정됩니다. 
즉, 클라이언트가 **`category_name`** 및 **`stop_pos_tags`** 필드를 요청에 포함하지 않더라도 이 필드는 기본적으로 None으로 설정합니다.
따라서 빈 요청이 들어와도 오류가 발생하지 않습니다.


```python
class DocumentTextAnalysisArgs(BaseModel):
	category_name: Optional[str]
	stop_pos_tags: Optional[list]
```

여기서 **`Optional`** 필드에 명시적인 기본값을 지정하지 않았습니다. 
이 경우, 클라이언트가 해당 필드를 요청에 포함하지 않으면 FastAPI는 이러한 필드를 **`None`** 값으로 설정하지 않습니다. 
따라서 클라이언트가 **`category_name`** 또는 **`stop_pos_tags`** 필드를 요청에 포함하지 않은 경우, 이러한 필드에 액세스하려고 하면 None이 아니라 NoneType에 액세스하려고 시도하게 되어 오류가 발생합니다.

따라서 첫 번째 코드처럼 작성하는 것이 안전합니다. 요청에서 해당 필드를 제공하지 않아도 기본적으로 `None`이 설정되므로 예기치 않은 오류를 방지할 수 있습니다.