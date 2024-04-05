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

Se você ainda não tem Python ou não sabe nada sobre Python, segue o link de um tutorial de como baixar o python e iniciar na linguagem. [Python Org Tutorial](https://www.python.org/about/gettingstarted/)

Com o python já instalado na sua máquina vai ser necessário que você entre no cmd do seu computador e baixe a bibliteoca Flask ou a extensão flask-RESTful. 

### Windows
```
pip install Flask
```
```
pip install flask-restful
```

### Linux (Ubuntu)

```
sudo apt-get install python3-flask
```

```
sudo apt-get install python3-flask-restful
```

Depois disso nós começamos a criar a nossa API, ela vai ser um sistema de consulta de cadastro de um banco fictício, a base de dados do sistema vai ser um dicionário em python e nós vamos implementar o metódo Read do CRUD para poder interagir com a base de dados.

**OBS:** O dicionário pode ser substítuido por qualquer banco de dados, mas ele servirá para o nosso exemplo.

```Python
from flask import Flask, make_response
import time
import json

base_dados = {
  1:{
    "name": "José",
    "idade": 53,
    "estado": "RJ",
    "saldo": 1200.00
  },
  2:{
    "name": "Maria",
    "idade": 25,
    "estado": "SP",
    "saldo": 15.50
  },
  3:{
    "name": "Alfredo",
    "idade": 80,
    "estado": "SP",
    "saldo": 80000.00
  },
  4:{
    "name": "Zeri",
    "idade": 18,
    "estado": "MG",
    "saldo": 275.36
  },
  5:{
    "name": "Marcelo",
    "idade": 34,
    "estado": "RS",
    "saldo": 15760.89
  },
}

app = Flask(__name__)

@app.route('/usuarios/<id>')
def searchUser(id):
  if base_dados.get(int(id)):
    response = make_response(json.dumps(base_dados[int(id)], indent=3))
    response.headers['Content-Type'] = 'application/json; charset=utf-8'
    time.sleep(5)
    return response
  time.sleep(5)
  return "Esse id não existe na base de dados"

@app.route('/usuarios')
def getAllUsers():
  response = make_response(json.dumps(base_dados, indent=3))
  response.headers['Content-Type'] = 'application/json; charset=utf-8'
  time.sleep(5)
  return response

if __name__ == "__main__":
  app.run(debug=True)
```

Agora vamos implementar na primeira rota, "/usuarios/id", o HTTP-Cache-Control através dos headers da resposta do servidor.

```Python
# A resposta dessa url da API vai ser guardada no cache do navegador por 300 segundos, 5 minutos
@app.route('/usuarios/<id>')
def searchUser(id):
  if base_dados.get(int(id)):
    response = make_response(json.dumps(base_dados[int(id)], indent=3))
    response.headers['Content-Type'] = 'application/json; charset=utf-8'
    # Implementando o HTTP-Cache-Control
    response.headers['Cache-Control'] = 'private, max-age=300, must-revalidate' # Guarda a informação no cache por 5 minutos e revalida antes de enviá-la novamente caso o tempo de validade tenha expirado
    time.sleep(5) # O time sleep serve para demonstrar de uma maneira mais tangível a diferença no tempo de resposta da API e do Cache do navegador
    return response
  time.sleep(5)
  return "Esse id não existe na base de dados"
```

O código completo fica assim


```Python
from flask import Flask, make_response
import time
import json

base_dados = {
  1:{
    "name": "José",
    "idade": 53,
    "estado": "RJ",
    "saldo": 1200.00
  },
  2:{
    "name": "Maria",
    "idade": 25,
    "estado": "SP",
    "saldo": 15.50
  },
  3:{
    "name": "Alfredo",
    "idade": 80,
    "estado": "SP",
    "saldo": 80000.00
  },
  4:{
    "name": "Zeri",
    "idade": 18,
    "estado": "MG",
    "saldo": 275.36
  },
  5:{
    "name": "Marcelo",
    "idade": 34,
    "estado": "RS",
    "saldo": 15760.89
  },
}

app = Flask(__name__)

# A resposta dessa url da API vai ser guardada no cache do navegador por 300 segundos, 5 minutos
@app.route('/usuarios/<id>')
def searchUser(id):
  if base_dados.get(int(id)):
    response = make_response(json.dumps(base_dados[int(id)], indent=3))
    response.headers['Content-Type'] = 'application/json; charset=utf-8'
    # Implementando o HTTP-Cache-Control
    response.headers['Cache-Control'] = 'private, max-age=300, must-revalidate' # Guarda a informação no cache por 5 minutos e revalida antes de enviá-la novamente caso o tempo de validade tenha expirado
    time.sleep(5) # O time sleep serve para demonstrar de uma maneira mais tangível a diferença no tempo de resposta da API e do Cache do navegador
    return response
  time.sleep(5)
  return "Esse id não existe na base de dados"

# A resposta dessa url da API não vai ser gaurdada no cache do navegador
@app.route('/usuarios')
def getAllUsers():
  response = make_response(json.dumps(base_dados, indent=3))
  response.headers['Content-Type'] = 'application/json; charset=utf-8'
  time.sleep(5)
  return response

if __name__ == "__main__":
  app.run(debug=True)
```


Agora, com a API pronta, basta colocar ela no ar.

**OBS:** Verifique se você está acessando a pasta aonde foi salvo o arquivo.py com a API

### Windows

CMD

```
python <nome_do_arquivo>.py
```

### Linux (Ubuntu)

SHELL

```
python3 <nome_do_arquivo>.py
```

Então com tudo pronto e a API no ar, vamos verificar se o caching da resposta da requisição está funcionando.

Como é possível observar nas imagens, a primeira requisição demorou 5 segundos para ser processada, pois a requisição acessou a API, processou os dados e deu a resposta para o navegador.
Já a segunda requisição demorou alugns milisegundos, já que a resposta da API já estava salva no cache do navegador, além disso, é possivel observar que o navegador não recebeu nenhuma transferência de dados e sim que a informação veio do cache.

### Requisição Número 1

![Requisição Número 1](https://github.com/MeTets/Tutorial_caching_Flask/assets/90905651/9dcb7995-0341-49dc-8b07-8cc89686bca7)

### Requisição Número 2

![Exemplo_req_resp_com_cache](https://github.com/MeTets/Tutorial_caching_Flask/assets/90905651/c55acfcf-192b-4f41-a80b-046a890f118a)

Já no lado do servidor é possível obervar que a API só processou uma requisição e não duas.

**OBS:** A primeira requisição que deu erro 404 foi processada na rota errada, a requisição que estamos olhando é a GET /usuarios/1

![Requsicoes_no_servidor](https://github.com/MeTets/Tutorial_caching_Flask/assets/90905651/28d4c090-94ce-49d6-9d87-bd17e263154f)

**PRONTO!** Agora você sabe implementar o armazenamento em cache de uma aplicação REST usando o HTTP-Cache-Control.

Essa pequena adição no seu código vai melhorar a performance e a latência da sua API. Entretanto, alem dos pontos positivos também existem pontos negativos, então vamos fazer um resumo desses pontos sobre caching com Http-Cache-Control.

### Positivos 
1. Diminui o consumo da API e do processamento de dados.
2. Diminui o tempo de respota para o cliente.
4. Permite que as inforamções sejam enviadas para o cliente mesmo sem conexão com internet ou caso a API fique fora do ar.

### Negativos
1. Os dados retornados para o cliente podem ser inconsistentes (no nosso exemplo, caso a base de dados fosse atualizada enquanto o cache daquela requisição ainda não expirou, a informação retornada para o cliente estará ultrapassda).

