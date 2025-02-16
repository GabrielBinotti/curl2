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
curl_setopt($ch, CURLOPT_SSL_VERIFYHOST, false);
```

### ___Usando Certificados___
Para garantir maior segurança nas requisições, pode especificar um arquivo CA.
```php
curl_setopt($ch, CURLOPT_CAINFO, "/caminho/do/certificado/ca-cert.pem");
```

Se precisar usar um certificado cliente.
```php
curl_setopt($ch, CURLOPT_SSLCERT, "/caminho/do/certificado/cert.pem");
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

- 
### ___Configuração de Cookies___








### ___Autenticações___





### ___Configurando o Header___
















