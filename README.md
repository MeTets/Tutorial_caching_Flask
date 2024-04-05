## Tutorial: Como utilizar o caching de requisições para aumentar a velocidade da sua API REST
Tutorial de como utilizar o cache de dados em uma aplicação Python-Flask

```Python
import requests
from flask import Flask

app = Flask('__name__')

@app.route('/')
def doSomething():
  return "Hello World"
```
