Retornar uma resposta HTTP completa e bem estruturada em PHP, incluindo status, headers, e corpo JSON, é fundamental para criar APIs robustas e profissionais. Aqui está um guia completo:

1. Definindo o Status HTTP:
   ```php
   http_response_code(200); // OK
   // Outros códigos comuns:
   // 201 - Created
   // 400 - Bad Request
   // 401 - Unauthorized
   // 403 - Forbidden
   // 404 - Not Found
   // 500 - Internal Server Error
   ```

2. Definindo Cabeçalhos:
   ```php
   // Content-Type
   header('Content-Type: application/json; charset=utf-8');

   // Controle de Cache
   header('Cache-Control: no-store, no-cache, must-revalidate');
   header('Pragma: no-cache');
   header('Expires: 0');

   // CORS (para permitir acesso cross-origin)
   header('Access-Control-Allow-Origin: *');
   header('Access-Control-Allow-Methods: GET, POST, PUT, DELETE, OPTIONS');
   header('Access-Control-Allow-Headers: Content-Type, Authorization');

   // Segurança
   header('X-Content-Type-Options: nosniff');
   header('X-Frame-Options: DENY');
   header('X-XSS-Protection: 1; mode=block');
   ```

3. Estruturando o Corpo da Resposta JSON:
   ```php
   $response = [
       'status' => [
           'code' => 200,
           'message' => 'OK'
       ],
       'data' => [
           'id' => 1,
           'name' => 'Alice',
           'email' => 'alice@example.com'
       ],
       'meta' => [
           'api_version' => '1.0',
           'request_id' => uniqid()
       ]
   ];

   echo json_encode($response, JSON_UNESCAPED_UNICODE | JSON_PRETTY_PRINT);
   ```

4. Lidando com Erros:
   ```php
   try {
       // Código que pode lançar uma exceção
       throw new Exception('Recurso não encontrado');
   } catch (Exception $e) {
       http_response_code(404);
       $response = [
           'status' => [
               'code' => 404,
               'message' => 'Not Found'
           ],
           'error' => [
               'message' => $e->getMessage(),
               'trace' => $e->getTrace()  // Em produção, evite expor o trace
           ]
       ];
       echo json_encode($response);
       exit();
   }
   ```

5. Paginação e Links:
   ```php
   $response = [
       'status' => ['code' => 200, 'message' => 'OK'],
       'data' => $users,  // Array de usuários
       'links' => [
           'self' => 'https://api.example.com/users?page=2',
           'first' => 'https://api.example.com/users?page=1',
           'prev' => 'https://api.example.com/users?page=1',
           'next' => 'https://api.example.com/users?page=3',
           'last' => 'https://api.example.com/users?page=5'
       ],
       'meta' => [
           'total_records' => 50,
           'records_per_page' => 10,
           'current_page' => 2
       ]
   ];
   ```

6. Validação e Erros de Formulário:
   ```php
   if (!validate_input($_POST)) {
       http_response_code(422);
       $response = [
           'status' => ['code' => 422, 'message' => 'Unprocessable Entity'],
           'errors' => [
               'name' => ['Required field is missing'],
               'email' => ['Invalid email format']
           ]
       ];
       echo json_encode($response);
       exit();
   }
   ```

7. Rate Limiting:
   ```php
   header('X-RateLimit-Limit: 100');
   header('X-RateLimit-Remaining: 50');
   header('X-RateLimit-Reset: ' . (time() + 3600));

   if ($rateLimitExceeded) {
       http_response_code(429);
       $response = [
           'status' => ['code' => 429, 'message' => 'Too Many Requests'],
           'error' => 'Rate limit exceeded. Try again in 1 hour.'
       ];
       echo json_encode($response);
       exit();
   }
   ```

8. Versionamento da API:
   ```php
   header('X-API-Version: 1.0');
   // E no URL: https://api.example.com/v1/users
   ```

9. Logging e Debugging:
   ```php
   $response['debug'] = [
       'query_time' => 0.0034,  // em segundos
       'memory_usage' => memory_get_usage(true)
   ];
   // Remova em produção!
   ```

10. Usando Classes para Estruturar Respostas:
    ```php
    class ApiResponse {
        public static function send($data, $code = 200) {
            http_response_code($code);
            header('Content-Type: application/json; charset=utf-8');
            
            $response = [
                'status' => [
                    'code' => $code,
                    'message' => self::getStatusMessage($code)
                ],
                'data' => $data
            ];
            
            echo json_encode($response, JSON_UNESCAPED_UNICODE);
            exit();
        }
        
        private static function getStatusMessage($code) {
            $messages = [
                200 => 'OK',
                201 => 'Created',
                // ...
            ];
            return $messages[$code] ?? 'Unknown';
        }
    }

    // Uso
    ApiResponse::send(['name' => 'Alice'], 201);
    ```

11. Autenticação e Autorização:
    ```php
    $token = apache_request_headers()['Authorization'] ?? '';
    
    if (!validateJwtToken($token)) {
        http_response_code(401);
        $response = [
            'status' => ['code' => 401, 'message' => 'Unauthorized'],
            'error' => 'Invalid or missing token'
        ];
        echo json_encode($response);
        exit();
    }
    ```

12. Compressão (opcional):
    ```php
    if (strpos($_SERVER['HTTP_ACCEPT_ENCODING'], 'gzip') !== false) {
        ob_start('ob_gzhandler');
        header('Content-Encoding: gzip');
    }
    ```

13. Tratando OPTIONS para CORS:
    ```php
    if ($_SERVER['REQUEST_METHOD'] == 'OPTIONS') {
        http_response_code(204); // No Content
        header('Access-Control-Allow-Origin: *');
        header('Access-Control-Allow-Methods: GET, POST, PUT, DELETE, OPTIONS');
        header('Access-Control-Allow-Headers: Content-Type, Authorization');
        exit();
    }
    ```

Ao seguir estas práticas, você garante que sua API PHP:
- Seja RESTful e siga os padrões HTTP.
- Forneça respostas JSON bem estruturadas e informativas.
- Lide corretamente com diversos cenários (sucesso, erro, paginação).
- Tenha cabeçalhos apropriados para segurança, cache e CORS.
- Seja versionada, escalável e fácil de manter.

Esta abordagem não apenas torna sua API mais profissional, mas também mais fácil de ser consumida por clientes, sejam eles navegadores, aplicativos móveis ou outros serviços.
