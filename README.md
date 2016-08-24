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

### Create

#### SUCESSO

**Padrão aceito:**

- Código: 201 - Criado

#### ERRO

E quando o erro no backend é de validação, você acha melhor retornar:

a) 403 - Proibido
b) 406 - Não Aceitável
c) 409 - Conflito
d) 412 - Pré-condição falhou
e) 422 - Entidade improcessável 

> Eu voto no 409.

**EU** voto no 409 pois esse código diz:

> Indica que a solicitação não pôde ser processada por causa do conflito no pedido, como um conflito de edição .

Eu acredito que esse **conflito** no pedido pode se dar pela não validação de seus valores, por isso gera um conflito com o que esperamos, o que acham?

**Padrão sugerido:**

- Código: 409 - Conflito

### Read

#### SUCESSO

- Retorna 1 lista vazia: 204

#### ERRO

### Update

#### SUCESSO

#### ERRO

### Delete

#### SUCESSO

#### ERRO
