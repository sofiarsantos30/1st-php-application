Login e Logout - Funções
==============================

O objetivo de criar `funções` é para `facilitar/automatizar/delegar` um determinado processo, com isso fazemos um reaproveitamento de código e nos tornamos muito mais produtivos.

## User Story #1
Dentro do ficheiro `url.php` que está na pasta `functions`, crie a função abaixo.

*Leia com calma todos os comentários para entender melhor o que esta função faz*

<span style="color: #ef5350; font-size: 0.9rem">*Digite o código abaixo linha a linha para praticar*</span>

```php
<?php

/**
 * Esta função faz um redirecionamento
 * de acordo com os valores do array associativo
 * passados como parâmetro.
 * 
 * Notem o parâmetro -----> $values = [] em ---> url_redirect($values = []) {....
 * Isto significa que se nenhum valor for passado como parâmetro para
 * esta função, então ele assume um array vazio como valor padrão
 * para esta variável.
 * 
 * @param $values array
 */
function url_redirect($values = []) {
    // A função "http_build_query" converte um array associativo em uma query string
    // Exemplo: 
    // Este array associativo --------> ['route' => 'home']
    // Vai se transformar nesta query string ------> route=home
    
    // Leia mais sobre a função "http_build_query" no site oficial do PHP.
    // https://www.php.net/manual/en/function.http-build-query.php
    $buildQueryString = http_build_query($values);

    // a função "header" modifica o cabeçalho de um pedido/resposta do servidor, ou seja,
    // uma destas modificações é responder com um redirecionamento como mostro no exemplo abaixo.

    // Leia mais sobre a função "header" no site oficial do PHP.
    // https://www.php.net/manual/en/function.header
    header('Location: http://localhost/?' . $buildQueryString);

    /*
     * Se a variávei $values tiver um array associativo com o este conteúdo ----> ['route' => 'dashboard']
     * então iremos ser redirecionado para o endereço ---> http://localhost/?route=dashboard
     * 
     * Iremos sempre ser redirecionado, isto só depende do conteúdo passado na nossa variavel $values.
     */
    
    // Este "exit" diz para o PHP parar toda a execução do código
    // isto porque não queremos exibir nenhum conteúdo enquanto o redirecionamento é feito.
    exit; 
}
```

---

## User Story #2
Dentro do ficheiro `message.php` que está na pasta `functions`, crie a função abaixo.

*Leia com calma todos os comentários para entender melhor o que esta função faz*

<span style="color: #ef5350; font-size: 0.9rem">*Digite o código abaixo linha a linha para praticar*</span>

```php
<?php

/**
 * Esta função exibe uma mensagem temporária, ou seja, se eu pedir 
 * para exibir uma mensagem temporária e página for refrescada logo em seguida,
 * então esta mensagem vai desaparecer.
 * 
 * Notem o parâmetro -----> $message = '' em ---> function flash_message($message = '') {....
 * Isto significa que se nenhum valor for passado como parâmetro para
 * esta função, então ele assume uma string vazia como valor padrão
 * para esta variável.
 * 
 * @param $message string
 * @return void
 */
function set_flash_message($message = '') {
  // Iremos usar a super global $_SESSION para fazer este "truque"
  // Vamos gravar a nossa mensagem dentro de uma chave n super global $_SESSION
  $_SESSION['flash_message'] = $message;

  // A função "strtotime" retorna um conjunto de números que chamamos de "TIMESTAMP"

  // Por exemplo, este valor "1658587240" que representa o TIMESTAMP, seu eu converter para uma DATA/HORA
  // vou ter o seguinte resultado: 2022-07-23 15:40:40
  // Mais exemplos
  // 1. 1658587240 ----> 2022-07-23 15:40:40
  // 2. 1658587241 ----> 2022-07-23 15:40:41
  // 3. 1658587242 ----> 2022-07-23 15:40:42

  // Resumindo, o TIMESTAMP é um conjunto de números (1658587240)
  // e que a cada segundo o seu valor vai sendo incrementado em 1.

  // Para saber mais sobre a história do TIMESTAMP, vocês podem consultar o link abaixo, é super interessante.
  // https://treinamento24.com/library/lecture/read/873865-como-funciona-o-timestamp
  // https://hkotsubo.github.io/blog/2019-05-02/o-que-e-timestamp

  // Para receber o TIMESTAMP da função "strtotime", temos que pedir para retornar o TIMESTAMP de algum momento no tempo.
  // O momento que queremos é o "agora + 1 segundos no futuro" ou seja o "now + 10 sec" ----> strtotime('now + 1 sec')

  // Isto significa que a nossa mensagem só ficará prensente por 10 segundos.
  $timestampNowPlus2Sec = strtotime('now + 1 sec');
  $_SESSION['flash_message_timestamp'] = $timestampNowPlus2Sec;
}
```

---

## User Story #3
Dentro do ficheiro `message.php` que está na pasta `functions`, crie a função abaixo.

*Leia com calma todos os comentários para entender melhor o que esta função faz*

<span style="color: #ef5350; font-size: 0.9rem">*Digite o código abaixo linha a linha para praticar*</span>

```php
/**
 * Esta função exibe uma mensagem mantida 1 segundos em uma sesssão no servidor.
 * Para que a mensagem seja exibida, a função 'set_flash_message' deverá ser
 * chamada antes.
 * 
 * @return void
 */
function get_flash_message() {
    // A condição abaixo testa se existe algum valor na chave 'flash_message'
    // da super global $_SESSION.
    // se não existir valores, retorna null (retorna vazio)
    if (empty($_SESSION['flash_message'])) {
        return null;
    }

    // ATENÇÃO: O código abaixo não será executado se o "return null" da condição acima
    // for executado, "return" siginfica "retorne e pare", não execute as linhas abaixo.

    // Iremos atribuir o valor da chave 'flash_message' que está na super global $_SESSION.
    // para dentro da variável $flashMessage
    $flashMessage = $_SESSION['flash_message'];

    // Iremos atribuir o conjunto de números que representa o TIMESTAMP deste exacto momento
    // dentro da variável $timestampNow
    $timestampNow = strtotime('now');

    // Iremos atribuir o valor da chave 'flash_message_timestamp' que está na super global $_SESSION.
    // para dentro da variável $timestampFlashMessage
    $timestampFlashMessage = $_SESSION['flash_message_timestamp'];

    // Se o TIMESTAMP que representa a hora atual for superior ao TIMESTAMP
    // que representa o tempo que a mensagem ficará mantida em sessão, então iremos removemos
    // as chaves 'flash_message' e 'flash_message_timestamp' de dentro da super global $_SESSION.

    // A função "unset" nos casos que iremos usar logo abaixp
    // remove a chave de dentro da super global $_SESSION.
        
    /*
     *  Exemplo
     * 
     *  Array
     *  (
     *      [is_authenticated] => true
     *      [flash_message] => "Minha mensagem de teste"
     *      [flash_message_timestamp] => 1658591365
     *  )
     * 
     * Depois de usar a função "unset"
     *
     * unset($_SESSION['flash_message']);
     * unset($_SESSION['flash_message_timestamp']);
     * 
     * Vamos ter o resultado abaixo, mas reparem que as 2 chaves 'flash_message' e 'flash_message_timestamp'
     * desapareceram, ficando apenas a chave 'is_authenticated'.
     * 
     *  Array
     *  (
     *      [is_authenticated] => true
     *  )
     */
    if ($timestampNow > $timestampFlashMessage) {
        // Removemos a chave 'flash_message' de dentro da super global $_SESSION.
        unset($_SESSION['flash_message']);
        // Removemos a chave 'flash_message_timestamp' de dentro da super global $_SESSION.
        unset($_SESSION['flash_message_timestamp']);
        // retornamos o valor null (vazio)
        return null;
    } else {
        // Se entrar neste else, então retornamos a mensagem que foi gravada em sessão no servidor
        // durante os 2 segundos.
        return $flashMessage;
    }
}
```

---

## User Story #4
Dentro do ficheiro `auth.php` que está na pasta `functions`, crie a função abaixo.

*Leia com calma todos os comentários para entender melhor o que esta função faz*

<span style="color: #ef5350; font-size: 0.9rem">*Digite o código abaixo linha a linha para praticar*</span>

```php
<?php

/**
 * Esta função tem como objetivo dizer se o utilizador
 * está ou não autenticado.
 * 
 * Para validar se o utilizador está ou não autenticado no site,
 * dentro da função, nós retornamos verdadeiro ou falso no seu retorno.
 * 
 * Como sabemos se o utilizador está ou não autenticado?
 * 
 * Caso a chave 'is_authenticated' esteja presente
 * na super global $_SESSION e tenha algum valor lá dentro, então
 * retornamos VERDADEIRO, senão, retornamos FAlSO.
 * 
 * @return bool
 */
function is_authenticated()
{
    if (empty($_SESSION['is_authenticated'])) {
        return false;
    } else {
        return true;
    }
}
```