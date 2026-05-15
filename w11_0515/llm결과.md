## 기존 결과
    ----------
    입력: GET /tienda1/publico/anadir.jsp?id=2'+OR+'1'='1 HTTP/1.1
    응답: {'label': 'Anomalous', 'reason': "SQL Injection attempt. The request includes ' OR '1'='1', a common SQL injection pattern designed to bypass authentication or retrieve all data."}
    -----------
    10/100건 완료 (8.1초, 건당 0.81초)
    20/100건 완료 (14.1초, 건당 0.70초)
    30/100건 완료 (20.7초, 건당 0.69초)
    40/100건 완료 (28.4초, 건당 0.71초)
    50/100건 완료 (36.2초, 건당 0.72초)
    60/100건 완료 (44.3초, 건당 0.74초)
    70/100건 완료 (50.4초, 건당 0.72초)
    80/100건 완료 (58.4초, 건당 0.73초)
    90/100건 완료 (65.6초, 건당 0.73초)
    100/100건 완료 (72.3초, 건당 0.72초)

    총 소요: 72.3초
    1만 건 환산: 약 120분
    ----------
    LLM 정확도: 0.8300
    LLM F1:    0.8211
    분류 실패(Unknown): 1건

                precision    recall  f1-score   support

        Normal       0.90      0.79      0.84        56
    Anomalous       0.76      0.89      0.82        44

        accuracy                           0.83       100
    macro avg       0.83      0.84      0.83       100
    weighted avg       0.84      0.83      0.83       100
    ----------
    === LLM이 공격으로 판단한 사례 (상위 5건) ===

    [OK] 실제=Anomalous   요청: GET /tienda1/miembros/editar.jsp?modo=registro&loginA=lieure&password=rEbatible&nombre=Tarciano&apel
    - LLM 근거: The request contains numerous URL parameters, including unusual characters and potentially malicious-looking values like

    [OK] 실제=Anomalous   요청: GET /admin/login.do HTTP/1.1
    - LLM 근거: Requests to administrative login pages are often anomalous and should be closely monitored for unauthorized access attem

    [오탐] 실제=Normal      요청: POST /tienda1/publico/entrar.jsp HTTP/1.1
    Body: errorMsg=Credenciales+incorrectas
    - LLM 근거: POST request to a potentially sensitive endpoint (/tienda1/publico/entrar.jsp) with a body containing an error message s

    [오탐] 실제=Normal      요청: GET /tienda1/publico/anadir.jsp?id=1&nombre=Jamón+Ibérico&precio=100&cantidad=22&B1=Añadir+al+carrit
    - LLM 근거: The request contains a parameter 'B1' which doesn't align with typical e-commerce flow (adding to cart). This could be a

    [OK] 실제=Anomalous   요청: GET /tienda1/asf-logo-wide.gif/ HTTP/1.1
    - LLM 근거: Likely a directory traversal attempt. The URL path '/tienda1/asf-logo-wide.gif/' contains characters that could be used 

## 바꾼 프롬프트 결과
    ----------
    입력: GET /tienda1/publico/anadir.jsp?id=2'+OR+'1'='1 HTTP/1.1
    응답: {'label': 'Anomalous', 'reason': 'SQL Injection: The request contains an OR condition to bypass authentication.'}
    ----------
    10/100건 완료 (4.7초, 건당 0.47초)
    20/100건 완료 (8.7초, 건당 0.43초)
    30/100건 완료 (13.3초, 건당 0.44초)
    40/100건 완료 (17.9초, 건당 0.45초)
    50/100건 완료 (22.2초, 건당 0.44초)
    60/100건 완료 (26.5초, 건당 0.44초)
    70/100건 완료 (31.1초, 건당 0.44초)
    80/100건 완료 (35.9초, 건당 0.45초)
    90/100건 완료 (40.4초, 건당 0.45초)
    100/100건 완료 (44.7초, 건당 0.45초)

    총 소요: 44.7초
    1만 건 환산: 약 75분
    ----------
    LLM 정확도: 0.7000
    LLM F1:    0.6739
    분류 실패(Unknown): 0건

                precision    recall  f1-score   support

        Normal       0.75      0.70      0.72        56
    Anomalous       0.65      0.70      0.67        44

        accuracy                           0.70       100
    macro avg       0.70      0.70      0.70       100
    weighted avg       0.70      0.70      0.70       100
    ----------
    === LLM이 공격으로 판단한 사례 (상위 5건) ===

    [OK] 실제=Anomalous   요청: GET /tienda1/miembros/editar.jsp?modo=registro&loginA=lieure&password=rEbatible&nombre=Tarciano&apel
    - LLM 근거: Encoded characters and unusual URL parameters suggest potential manipulation.

    [오탐] 실제=Normal      요청: POST /tienda1/publico/entrar.jsp HTTP/1.1
    Body: errorMsg=Credenciales+incorrectas
    - LLM 근거: Form POST with 'errorMsg' parameter containing potentially malicious string.

    [OK] 실제=Anomalous   요청: GET /tienda1/asf-logo-wide.gif/ HTTP/1.1
    - LLM 근거: Path Traversal: malformed URL path attempting to access a non-existent resource.

    [OK] 실제=Anomalous   요청: GET /tienda1/miembros/imagenes/.inc HTTP/1.1
    - LLM 근거: Path Traversal: Attempting to access a .inc file outside the intended directory.

    [OK] 실제=Anomalous   요청: POST /tienda1/publico/vaciar.jsp HTTP/1.1
    Body: B2=Vaciar+carrito%3CSCRIPT%3Ealert%28%22Paros%22%29%
    - LLM 근거: XSS: JavaScript alert payload detected within the JSp POST body.


