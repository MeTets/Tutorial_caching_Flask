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

### Implementando caching em uma API REST Flask - Client-Side Caching
O método utilizado no tutorial para implementar o caching na API Flask vai ser o HTTP-Cache Control. Esse método é um dos mais simples e rápidos de serem implementados e ele se encaixa no tipo Cliet-side Caching e na estreatégia Read-Through. Nesse caso o browser vai ficar responsável por armazenar as informações no seu prórpio cache de acordo com as regras passadas através dos headers das requisições e das respostas. As regras que podem ser passadas através do header "Cache-Control" das requisiçóes são:

1. **max-age** -> Define o tempo máximo que a informação pode ficar armazeanda no cache no navegador antes de expirar.
2. **public/private** -> Define quais servidores podem armazenar as infomrações da requisição em cache. **Public**: Todos os intermediários + usuário final \ **Private**: Usuário final
3. **must-revalidate** -> Define que após o fim da validade, as infomrações em cache devem ser revalidadas antes de serem enviadas para o usuário final
4. **no-cache** -> Define que as informações do cache devem ser atualizadas a cada reutilização da informação.
5. **no-store** -> Define que a informações da requisições não devem ser armazenadas no cache do navegador.

MÃOS A OBRA!

1. Passo - Constriur uma api em Python Flask

Se você ainda não tem Python, segue o link de um tutorial de como baixar o python e iniciar na linguagem. [Python Org Tutorial](https://www.python.org/about/gettingstarted/)

Com o python já instalado na sua máquina vai ser necessário que você baixe o python
```
pip install Flask
```

Depois disso nós começamos a criar a nossa aplicação

```Python
import requests
from flask import Flask

app = Flask('__name__')

@app.route('/')
def doSomething():
  return "Hello World"
```
