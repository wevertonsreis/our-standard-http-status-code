# Our standard http status code

Nosso padrão de resposta de códigos HTTP via API REST.

Como não existe muito material sobre esse assunto em pt-BR, vamos criar o nosso. Esse padrão está aberto **SEMPRE** a sugestões, caso a sugestão seja devidamente explicada ela será votada pelos contribuidores, até a data, do projeto.

*ps: Lembrando que esse padrão é para ser utilizado em **TODOS** os sistemas web da Webschool.*

## CRUD

Iniciamente vamos definir os códigos de retorno para as funções básicas do CRUD.

Para isso precisamos definir alguns padrões, por exemplo:

> Quando você pede 1 listagem de usuários e a API responde com 1 *Array* vazio, você retornará?

- **a)** ![204](https://http.cat/204)
- **b)** ![404](https://http.cat/404)

**IMHO** devemos retornar `204`, pois achamos a entidade a ser buscada, `Usuário`, porém o mesmo veio vazio ou `No content`. E só retornaria `404` caso não existisse a entidade `Usuário` ou não existisse o `Usuário` específico, que veremos mais a frente.

Devemos sempre ter na mente que **QUALQUER código que inicie com `4` será um código de erro**.

Por isso agora eu lhe pergunto:

> Se você tem Usuários, porém para aquela busca o resultado veio **VAZIO**. Isso é um erro?

> Acho que não né? Então pronto!

ps: **Atualizado**

Achei 1 material que diz exatamente isso sobre o 204: [http://www.vinaysahni.com/best-practices-for-a-pragmatic-restful-api#http-status](http://www.vinaysahni.com/best-practices-for-a-pragmatic-restful-api#http-status)

> 204 - Response to a successful request that won't be returning a body (like a DELETE request)

Porém perceba que **para nós** o `DELETE` irá retornar 1 JSON também, então vamos utilizar esse código como:

> Retornando 1 corpo/body vazio.

Perceba que é **APENAS** o corpo de dados, pois podemos ter outras informações que virão juntamente com o JSON da resposta, logo mais chegaremos nisso.

Além disso sabemos que a resposta sempre será **ou SUCESSO ou ERRO**, porém além disso ela poder ter mais nuances, por exemplo o retorno do **ERRO**, vamos dividir em:

- Não encontrou entidade: 404
- Encontrou mas não aplicou a ação: 204 ???

Aposto que você percebeu **claramente** que encontrar mas não aplicar a ação não é semanticamente 1 erro da requisição, porém para nós será 1 tipo de erro de sucesso.

Será 1 erro pois poderemos tratar no Frontend diferentemente do que tratamos o retorno com `200`.

> O que acham?

### Create

Função que **CRIA** uma entidade seguindo o padrão:

- url: /api/entidade
- method: POST

#### Create SUCESSO

**Padrão aceito:**

- Código: 201 - Criado

#### Create ERRO

Iremos ter 2 formas de erro para o `Create`:

- Não encontrou a entidade a ser criada.
- Encontrou porém os dados não validaram.

##### Não encontrou a ENTIDADE

Quando não encontra a entidade a ser criada já sabemos que é: `404`

![404](https://http.cat/404)

E quando o erro no backend é de validação, você acha melhor retornar:

- a) 403 - Proibido
- b) 406 - Não Aceitável
- c) 409 - Conflito
- d) 412 - Pré-condição falhou
- e) 422 - Entidade improcessável 

> Eu voto no 409.

**EU** voto no 409 pois esse código diz:

> Indica que a solicitação não pôde ser processada por causa do conflito no pedido, como um conflito de edição .

Eu acredito que esse **conflito** no pedido pode se dar pela não validação de seus valores, por isso gera um conflito com o que esperamos, o que acham?


##### Atualização Create ERRO

O [Wendell](https://github.com/WendellAdriel) me chamou a antenção para o `422` e depois eu também li o mesmo aqui [http://www.vinaysahni.com/best-practices-for-a-pragmatic-restful-api#http-status](http://www.vinaysahni.com/best-practices-for-a-pragmatic-restful-api#http-status):

> 422 Unprocessable Entity - Used for validation errors

**Padrão sugerido/aceito:**

- Código: 422 - Entidade improcessável 

![422](https://http.cat/422)

### Read

Função que **LÊ/BUSCA** entidade(s) seguindo o padrão:

- url: /api/entidade
- method: GET

- url: /api/entidade/:id
- method: GET

- url: /api/entidade/campo1/valor1/campo2/valor2
- method: GET

- url: /api/entidade?id=
- method: GET

- url: /api/entidade?=campo1=valor1&campo2=valor2
- method: GET


#### SUCESSO

- Retorna 1 lista vazia: 204
- Retorna 1 lista não vazia: 200

##### retorno vazio

**Padrão sugerido:**

- Código: 204 - No Content

![204](https://http.cat/204)

##### retorno não vazio

**Padrão sugerido:**

- Código: 200 - Ok

![200](https://http.cat/200)

#### ERRO

##### Não encontrou a ENTIDADE

Quando não encontra a entidade a ser criada já sabemos que é: `404`

![404](https://http.cat/404)

### Update

No `Update` teremos as seguintes situações:

- Não encontra a entidade para alterar: 404
- Encontra porém não valida os dados: 409
- Encontra e altera a entidade, retornando a entidade modificada: 200
- Encontra e não altera a entidade, retornando a entidade antiga: 204

> Mas por que retornar `204` quando a alteração não modifica em nada a entidade?

**IMHO**, acredito que sabermos se algum valor foi realmente alterado no backend ou a entidade não precisou modificar nada é de suma importância para o Frontend. Pois se devolvermos apenas `200` nos 2 casos o desenvolvedor acreditará que modificou algo mesmo se esse não precisasse, logo se o Frontend receber um `204` ele saberá que não adiantará enviar os mesmos dados pois a entidade no backend já está com aquela conformação dos dados/valores.

#### SUCESSO

#### ERRO

##### Não encontrou a ENTIDADE

Quando não encontra a entidade a ser criada já sabemos que é: `404`

![404](https://http.cat/404)

### Delete

#### SUCESSO

#### ERRO

##### Não encontrou a ENTIDADE

Quando não encontra a entidade a ser criada já sabemos que é: `404`

![404](https://http.cat/404)
