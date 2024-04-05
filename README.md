# Tutorial: Como utilizar o caching de requisições para aumentar a velocidade da sua API REST

### O que é "Caching"? 
O Caching é o armazenamento inteligente dos dados consumidos por uma aplicação ou sistema. O Caching armazena as informações em forma de bloco em um local de armazenamento mais rápido, mais próximo do usuário final e com um maior nível de hierarquia, esse local de armazento é conhecido como Cache e ele está presente em navegadores web, banco de dados, computadores, aplicações etc.
O Caching é utilizado porque ele fornece uma maior eficiência no uso de recursos e dados, além de aumentar a velocidade e reduzir a latência da aplicação ou sistema. 

### Diferentes estratégias e tipos de Cache

Quando o assunto é Caching existem diversos tipos disponíveis para os desenvolvedores, cada tipo de armazenamento em cache tem os seus benefícios e malefícios, além de diferentes formas de implementação e níveis de habilidade necessários. Dentre os tipos disponíveis existem:

- Server-side Caching
- Distributed Caching
- Client-side Caching
- In-memory Caching

Além dos diferentes tipos, existem também diferentes estratégias de implementação do caching. Entre eles estão:

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
