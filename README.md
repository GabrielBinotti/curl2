# cURL

É uma biblioteca que permite fazer requisições HTTP para se comunicar com servidores
e APIs.

## Inicializando o cURL

Para começar a usar precisamos inicializar uma sessão cURL.

```php
$ch = curl_init();
```

## Configurando as opções

Após inicializado é necessario configurar as opções para efeturar uma requisição utilizando o comando `curl_setopt()`.

### ___URL___
```php
curl_setopt($ch, CURLOPT_URL, "URL");
```

### ___Método da Requisição___

- GET - (Default)
```php
curl_setopt($ch, CURLOPT_HTTPGET, true);
```

- POST
```php
curl_setopt($ch, CURLOPT_POST, true);
```

- PUT
```php
curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "PUT");
```

- DELETE
```php
curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "DELETE");
```

### ___Enviando Dados___

Para enviar dados no corpo da requisição, use a opção `curlopt_postfields()`.

```php
// Simulando um formulario
$data = array('chave1' => 'valor1', 'chave2' => 'valor2');
curl_setopt($ch, CURLOPT_POSTFIELDS, http_build_query($data));
```

```php
// Enviando em formato JSON
$jsonData = json_encode(array('chave1' => 'valor1', 'chave2' => 'valor2'));
curl_setopt($ch, CURLOPT_POSTFIELDS, $jsonData);
```

### ___Recebendo Respostas___

Para capturar as respostas de qualquer requisição, use `curlopt_resturtransfer()`. Isso faz com que o retorno seja uma string
em vez de imprimir na tela.

```php
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
```

### ___Configuração de Tempo e Redirecionamento___

Para definir o tempo máximo de execução da requisição em segundos, use `curlopt_timeout()`.
```php
curl_setopt($ch, CURLOPT_TIMEOUT, 10);
```

Para definir o tempo máximo para estabelecer uma conexão, use `curlopt_connecttimeout()`.
```php
curl_setopt($ch, CURLOPT_CONNECTTIMEOUT, 5);
```

Seguir redirecionamentos HTTP automaticamente, use `curlopt_followlocation()`;
```php
curl_setopt($ch, CURLOPT_FOLLOWLOCATION, true);
```

### ___Verificação de SSL___
Por padrão o cURL ele verifica o certificado SSL do servidor, em ambiente de produção, pode desativar essa verificação.

```php
curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, false);
curl_setopt($ch, CURLOPT_SSL_VERIFYHOST, false); // (1 ou 2)
```

### ___Usando Certificados___
Para garantir maior segurança nas requisições, pode especificar um arquivo CA.
```php
curl_setopt($ch, CURLOPT_CAINFO, "/caminho/do/certificado/ca-cert.crt");
```

Se precisar usar um certificado cliente.
```php
curl_setopt($ch, CURLOPT_SSLCERT, "/caminho/do/certificado/cert.crt");
curl_setopt($ch, CURLOPT_SSLKEY, "/caminho/da/chave/private.key");
```

- ___Criando certificados no Linux___

1.1 Criar uma chave privada para a CA.
```bash
openssl genrsa -out ca.key 4096
```

- `genrsa` -> Gera uma chave privada.
- `-out ca.key` -> Salva a chve no arquivo.
- `4096` -> Define o tamanho da chave.

1.2 Criar um certificado de CA
```bash
openssl req -new -x509 -days 3650 -key ca.key -out ca.crt
```
- `req -new -x509` → Gera um novo certificado no formato X.509.
- `-days 3650` → Certificado válido por 10 anos.
- `-key ca.key` → Usa a chave privada da CA.
- `-out ca.crt` → Salva o certificado da CA.

Durante a execução , você será solicitado a preencher informações como:
- __Country Name (C):__ BR
- __State or Province Name (ST):__ São Paulo
- __Organization Name (O):__ Minha Empresa
- __Common Name (CN):__ Minha CA

2.1 Criar uma chave privada para o cliente
```bash
openssl genrsa -out cliente.key 2048
```

2.2 Criar uma requisição de certificado (CSR - Certificate Signing Request)
```bash
openssl req -new -key cliente.key -out cliente.csr
```

- `-new` → Gera uma nova requisição de certificado.
- `-key cliente.key` → Usa a chave privada do cliente.
- `-out cliente.csr` → Salva o arquivo de requisição.

2.3 Assiar o certificado do cliente com CA

```bash
openssl x509 -req -in cliente.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out cliente.crt -days 365
```

- `-req` → Indica que será usado um CSR.
- `-in cliente.csr` → Usa a requisição do cliente.
- `-CA ca.crt` → Assina o certificado usando a CA.
- `-CAkey ca.key` → Usa a chave da CA para assinar.
- `-CAcreateserial` → Cria um número serial para o certificado.
- `-out cliente.crt` → Gera o certificado assinado.
- `-days 365` → Define validade de 1 ano.


### ___Configuração de Cookies___

Para definir um cookie na requisição, use `curlopt_cookie()`.
```php
curl_setopt($ch, CURLOPT_COOKIE, "nome=Gabriel; token=abc123");
```

Salvar em um arquivo, use `curlopt_cookiejar()`.
```php
curl_setopt($ch, CURLOPT_COOKIEJAR, "cookies.txt");
```

Para utilizar os cookies de um arquivo, use `curlopt_cookiefile()`.
```php
curl_setopt($ch, CURLOPT_COOKIEFILE, "cookies.txt");
```

### ___Autenticações___

No cURL é possivel configurar diferentes formas de autenticações. Aqui estão algumas delas.

1. JWT - é um método comum de autenticações em APIs.
```php
$headers = [
    "Authorization: Bearer SEU_TOKEN_JWT_AQUI",
    "Content-Type: application/json"
];

curl_setopt($ch, CURLOPT_HTTPHEADER, $headers);
```

2. Basic Auth - Envia as credenciais (usuario:senha) codificados em Base64.

___Exemplo manual___
```php
$usuario = "meu_usuario";
$senha = "minha_senha";
$auth = base64_encode("$usuario:$senha");

$headers = [
    "Authorization: Basic $auth"
];

curl_setopt($ch, CURLOPT_HTTPHEADER, $headers);
```

___Exemplo Automático___
```php
curl_setopt($ch, CURLOPT_USERPWD, "meu_usuario:minha_senha");
curl_setopt($ch, CURLOPT_HTTPAUTH, CURLAUTH_BASIC);
```

3. Digest Auth -> é mais segura que Basic Auth, pois não envia a senha diretamente.
```php
curl_setopt($ch, CURLOPT_USERPWD, "meu_usuario:minha_senha");
curl_setopt($ch, CURLOPT_HTTPAUTH, CURLAUTH_DIGEST);
```

4. OAuth2.0 - Usar o Access Token.
```php

$headers = [
    "Authorization: Bearer SEU_ACCESS_TOKEN"
];

curl_setopt($ch, CURLOPT_HTTPHEADER, $headers);
```


### ___Configurando o Header___

No header podemos ter diversas configurações (Envio e Recebimento).
Para configurar diferentes tipos de resposta use o `Accept`.
```php
$headers = [
    "Accept: application/json", // Aceita JSON
    "Accept: application/xml",  // Aceita XML
    "Accept: */*"               // Aceita qualquer tipo de resposta
];

curl_setopt($ch, CURLOPT_HTTPHEADER, $headers);
```

Para enviar dados em diferentes formatos utilize o `Content-Type`.

- JSON
```php
$dados = json_encode([
    "nome" => "Gabriel",
    "idade" => 25
]);

$headers = [
    "Content-Type: application/json"
];

curl_setopt($ch, CURLOPT_POST, true);
curl_setopt($ch, CURLOPT_POSTFIELDS, $dados);
curl_setopt($ch, CURLOPT_HTTPHEADER, $headers);
```

- Como formulário
```php
$dados = http_build_query([
    "nome" => "Gabriel",
    "idade" => 25
]);
$headers = [
    "Content-Type: application/x-www-form-urlencoded"
];
curl_setopt($ch, CURLOPT_POST, true);
curl_setopt($ch, CURLOPT_POSTFIELDS, $dados);
curl_setopt($ch, CURLOPT_HTTPHEADER, $headers)
```

- Enviar dados + arquivos
```php
$dados = [
    "nome" => "Gabriel",
    "foto" => new CURLFile("caminho/da/imagem.jpg") // Envia o arquivo
];

$headers = [
    "Content-Type: multipart/form-data"
];

curl_setopt($ch, CURLOPT_POST, true);
curl_setopt($ch, CURLOPT_POSTFIELDS, $dados);
curl_setopt($ch, CURLOPT_HTTPHEADER, $headers);
```

- Enviar XML
```php
$xml = '<?xml version="1.0" encoding="UTF-8"?>
<usuario>
    <nome>Gabriel</nome>
    <idade>25</idade>
</usuario>';

$headers = [
    "Content-Type: application/xml"
];

curl_setopt($ch, CURLOPT_POST, true);
curl_setopt($ch, CURLOPT_POSTFIELDS, $xml);
curl_setopt($ch, CURLOPT_HTTPHEADER, $headers);

```

- Receber um arquivo especifico
```php
$headers = [
    "Accept: application/pdf" // Aceita apenas arquivos PDF
];

curl_setopt($ch, CURLOPT_HTTPHEADER, $headers);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);

$arquivo = curl_exec($ch);
file_put_contents("arquivo_baixado.pdf", $arquivo);
```

#### _Parametros da HEADER `accept` ou `Content-Type`_

`application/json`

`application/xml`

`application/javascript`

`application/pdf`

`image/jpeg`

`image/png`

`text/plain`

`text/html`

`multipart/form-data`
