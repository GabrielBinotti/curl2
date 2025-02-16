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
