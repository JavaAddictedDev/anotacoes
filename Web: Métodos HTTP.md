1. GET
   - Função: Recuperar um recurso específico do servidor.
   - Características:
     - Seguro: Não deve alterar o estado do servidor.
     - Idempotente: Múltiplas solicitações têm o mesmo efeito.
     - Cacheável: Respostas podem ser armazenadas em cache.
   - Corpo da solicitação: Não deve ter corpo.
   - Query String: Usada para passar parâmetros (ex: `/users?id=123`).
   - Exemplos:
     - Obter detalhes de um usuário: `GET /users/123`
     - Listar produtos: `GET /products`
     - Buscar por termo: `GET /search?q=smartphone`

2. POST
   - Função: Criar um novo recurso no servidor.
   - Características:
     - Não seguro: Altera o estado do servidor.
     - Não idempotente: Cada solicitação pode criar um novo recurso.
     - Geralmente não cacheável.
   - Corpo da solicitação: Contém os dados do novo recurso.
   - Exemplos:
     - Criar um novo usuário: `POST /users` (dados no corpo)
     - Enviar um formulário: `POST /submit-form`
     - Fazer login: `POST /login` (credenciais no corpo)

3. PUT
   - Função: Atualizar um recurso existente (ou criar, se não existir).
   - Características:
     - Não seguro: Altera o estado do servidor.
     - Idempotente: Múltiplas solicitações resultam no mesmo estado.
     - Geralmente não cacheável.
   - Corpo da solicitação: Contém a representação completa do recurso.
   - Exemplos:
     - Atualizar detalhes de usuário: `PUT /users/123` (todos os dados no corpo)
     - Substituir um documento: `PUT /documents/report.pdf`

4. PATCH
   - Função: Aplicar modificações parciais a um recurso.
   - Características:
     - Não seguro: Altera o estado do servidor.
     - Não idempotente: Cada patch pode alterar o estado de maneira diferente.
     - Geralmente não cacheável.
   - Corpo da solicitação: Contém apenas as mudanças a serem aplicadas.
   - Exemplos:
     - Atualizar o email: `PATCH /users/123` (corpo: `{"email": "new@example.com"}`)
     - Modificar um campo: `PATCH /posts/456` (corpo: `{"title": "Novo Título"}`)

5. DELETE
   - Função: Remover um recurso específico.
   - Características:
     - Não seguro: Altera o estado do servidor.
     - Idempotente: Deletar várias vezes tem o mesmo efeito.
     - Geralmente não cacheável.
   - Corpo da solicitação: Geralmente vazio, mas pode ter.
   - Exemplos:
     - Excluir um usuário: `DELETE /users/123`
     - Remover um comentário: `DELETE /posts/456/comments/789`

6. HEAD
   - Função: Obter apenas os cabeçalhos de um recurso, sem o corpo.
   - Características:
     - Seguro: Não altera o estado.
     - Idempotente: Múltiplas solicitações são iguais.
     - Cacheável: Como GET.
   - Uso: Verificar metadados sem transferir o recurso.
   - Exemplos:
     - Verificar se arquivo existe: `HEAD /files/large-file.zip`
     - Checar última modificação: `HEAD /articles/latest`

7. OPTIONS
   - Função: Descobrir quais métodos HTTP e operações são permitidos.
   - Características:
     - Seguro: Não altera o estado.
     - Idempotente: Múltiplas solicitações são iguais.
   - Resposta: Cabeçalho `Allow` lista métodos suportados.
   - CORS (Cross-Origin Resource Sharing): Usado em solicitações pré-voo.
   - Exemplos:
     - Checar métodos: `OPTIONS /api/posts` (resposta: `Allow: GET, POST, PUT, DELETE`)
     - CORS pré-voo: `OPTIONS /api/users` (com cabeçalhos CORS)

8. TRACE
   - Função: Diagnóstico de conexão, teste de loop-back.
   - Características:
     - Seguro: Não altera o estado.
     - Idempotente: Múltiplas solicitações são iguais.
   - Funcionamento: O servidor envia de volta a solicitação recebida.
   - Uso: Debug, ver se intermediários modificam a solicitação.
   - Preocupações de segurança: Muitas vezes desabilitado.

9. CONNECT
   - Função: Estabelecer um túnel para comunicação.
   - Uso principal: Em conexões SSL/TLS via proxy HTTP.
   - Funcionamento:
     1. Cliente pede ao proxy para conectar a um host.
     2. Proxy estabelece a conexão TCP.
     3. Proxy responde com 200 OK.
     4. Cliente e servidor trocam dados através do túnel.
   - Exemplo: `CONNECT server.example.com:443 HTTP/1.1`

Cada método tem seu propósito e semântica específicos. Em APIs RESTful, a escolha do método correto é crucial:
- GET para leitura
- POST para criação
- PUT/PATCH para atualização
- DELETE para remoção

Além disso, os códigos de status HTTP (200 OK, 201 Created, 404 Not Found, etc.) trabalham em conjunto com os métodos para fornecer mais contexto sobre o resultado da solicitação.
