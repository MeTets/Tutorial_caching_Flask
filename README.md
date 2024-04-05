# Tutorial: Como utilizar o caching de requisições para aumentar a velocidade da sua API REST

### Como utilizar o cache em uma Python-Flask API REST?

Quando o assunto é Caching existem diversos tipos dispoíveis para os desenvolvedores, cada tipo de armazenamento em cache tem os seus benefícios e malefícios, além de diferentes formas de implementação e níveis de habilidade necessários. Dentre os tipos disponíveis existem:

- Server-side Caching
- Distributed Caching
- Client-side Caching
- In-memory Caching

Além dos diferentes tipos, existem também diferentes estratégias de implementação do caching em uma API REST. Entre eles estão:

- Cache Aside
- Write-Through/Read-Through
- Write Behind


```Python
import requests
from flask import Flask

app = Flask('__name__')

@app.route('/')
def doSomething():
  return "Hello World"
```
