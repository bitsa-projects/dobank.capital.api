# Integração API  Dobank

Bem vindo à integração mais fácil e prática do mercado!

A integração com a API da Dobank foi desenvolvida para ser o mais simples possível. 

Usamos apenas enpoints POST. 
A autenticação é feita com o envio  do token como variável post. 



1. Acesse sua conta Dobank;
2. Vá em Configurações -> API's Dobank;
3. Consulte as taxas para Pix e Btc;
4. Informe a URL do seu webhook para receber as informações do retorno;
5. Copie o token já gerado (você pode trocar se desejar);
6. implemente  a integração seguindo os modelos abaixo.

## Recebimento

Exemplo em  python/requests:

    import json
    import requests
    url = 'https://dobank.capital/api/'
    endpoint = 'recebimento'

    token = '<SEUTOKEN>'
    valor = 1.00 # R$ 1,00
    metodo = 'pix' # ou 'btc' para bitcoin

    payload = {
        "token": token,
        "amount": valor,
        "method_code":metodo,#or "btc"
    }
    #print(str(user))
    r = requests.post(url+endpoint, data=payload)

    #print(r.url, r.content, r.status_code)
    a = r.json()
    print(a)
    #adicione o seu código para registrar as informações no banco de dados

Exemplo em php:

    <?php
    function curl_post($url_endpoint,$payload){
        $ch = curl_init();
        curl_setopt($ch, CURLOPT_URL, $url_endpoint);
        curl_setopt($ch, CURLOPT_POST, true);
        curl_setopt($ch, CURLOPT_POSTFIELDS, http_build_query($payload)); 
        curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
        // curl_setopt($ch, CURLOPT_TIMEOUT, 20);

        $response = curl_exec($ch);
        curl_close($ch);
        return json_decode($response, true);
      }

      $httpHeader = [];
      $token = '<SEUTOKEN>';

      $valor = 100.00; // R$ 1,00
      $metodo = 'btc'; // pix ou 'btc' para bitcoin
      $payload = [
        "token" => $token,
        "amount" => $valor,
        "method_code" => $metodo
      ];
      $url = 'https://dobank.capital/api/';
      $endpoint = 'recebimento';


      $result = curl_post($url.$endpoint,$payload);
      echo $result.'\n';
      ?>

Exemplo em javascript:

    let data = {
        token: '<SEUTOKEN>',
        method_code: "pix", //ou btc 
        amount:5.00 
      }

      fetch('https://dobank.capital/api/recebimento', {
        method: "POST",
        body: JSON.stringify(data),
        headers: {"Content-type": "application/json; charset=UTF-8"}
      })
      .then(response => response.json()) 
      .then(json => console.log(json))
      .catch(err => console.log(err));


## Exemplo de retorno  Pix (JSON)

    {
    "txid": "1e156a2b9f94215f2a84c1c308d4db50",
    "copiaecola": "00020101021226910014br.gov.bcb.pix2569api.developer.dobank.capital\/v1\/p\/v2\/5fd4b3c59588412caeb6f8bfe1a2250f5204000053039865802BR5925Dobank Tecnologia em Paga6011So Paulo SP62070503***630439F0",
    "qrcode": "iVBORw0KGgoAAAANSUhEUgAAAPQAAAD0CAYAAACsLwv+AAAAAklEQVR4AewaftIAAA4USURBVO3BQW4sy7LgQDKh\/W+ZfYY+CiBRJd33o93M\/mGtdYWHtdY1HtZa13hYa13jYa11jYe11jUe1lrXeFhrXeNhrXWNh7XWNR7WWtd4WGtd42GtdY2HtdY1HtZa13hYa13jhw+p\/KWKN1Smik+onFRMKicVn1CZKt5QmSpOVH5TxTepTBVvqPylik88rLWu8bDWusbDWusaP3xZxTepvKEyVUwqb1ScVEwqb6icVEwqJypTxUnFpDJVnFScqEwVk8qkMlVMKlPFGypTxRsV36TyTQ9rrWs8rLWu8bDWusYPv0zljYo3VD5RMalMKicqU8WkMlWcqEwqU8WkMlWcqJxUfFPFGxUnFScqU8Wk8k0qb1T8poe11jUe1lrXeFhrXeOHy6mcqJxUTCqfUHmjYlI5UZkqpoo3VKaKN1TeUDmpeENlqphUpor\/yx7WWtd4WGtd42GtdY0fLlPxhsobFZPKpHJS8YbKVDGpnKj8JZWTikllqphUJpWTiknlpOImD2utazysta7xsNa6xg+\/rOJ\/WcWk8ptUpopJ5RMVb6hMFZPKpHJSMam8oXJScaLylyr+lzysta7xsNa6xsNa6xo\/fJnK\/xKVqWJSmSomlanipGJSmSomlaliUvmEylTxiYpJ5ZsqJpUTlaliUjlRmSpOVP6XPay1rvGw1rrGw1rrGj98qOL\/JyrfVDGpnKi8UfGGyhsVJxWTylTxhspfqvi\/5GGtdY2HtdY1HtZa1\/jhQypTxaTyTRVTxRsqU8VJxRsqU8WJyknFpHKi8omKE5U3Kr6pYlI5qZhUpooTlW+q+E0Pa61rPKy1rvGw1rrGD1+m8omKE5WpYlI5qXhD5ZtUpooTlaliUpkqTlSmikllqpgqJpWpYlKZKj6hMlWcqEwVJypTxaTyRsWkMlV808Na6xoPa61rPKy1rvHDH6uYVCaVk4pJZar4TRUnKpPKJyr+SypvqEwVk8o3qXxCZar4RMWk8pce1lrXeFhrXeNhrXWNH\/6YyhsVv0llqnhDZap4Q+WkYlJ5Q+WNiknlpOKNikllqnhDZaqYVCaVqWJSeaPipGJS+U0Pa61rPKy1rvGw1rrGD19WcaLyhspJxaTyRsU3qfymihOVT6h8QuWkYqqYVKaKSWWqmFSmijcqTlROVP5LD2utazysta7xsNa6hv3DF6m8UTGpTBVvqEwVk8pUMal8ouITKicVJypTxSdUpopJZao4UTmpmFTeqHhD5aTiEyonFd\/0sNa6xsNa6xoPa61r2D98QOUTFZPKN1V8k8pUMal8U8Wk8k0Vk8obFScqb1ScqLxRMalMFW+onFScqEwV3\/Sw1rrGw1rrGg9rrWvYP3yRyhsVb6hMFScqU8WkMlWcqEwVb6icVEwqU8WkMlVMKlPFJ1SmikllqphUpopJZao4UflNFScqU8V\/6WGtdY2HtdY1HtZa17B\/+CKVk4pJZaqYVKaKSWWqmFROKk5UpooTlZOKSeWk4g2VqWJS+UTFpDJVfJPKScWk8omKSeUTFX\/pYa11jYe11jUe1lrXsH\/4gMpUMalMFScqU8Wk8omKSeWk4n+ZylTxhspUMamcVEwqU8WkclLxhspU8QmVqWJSOak4UTmp+MTDWusaD2utazysta5h\/\/CLVE4qJpVPVEwqU8WJyhsVk8pUcaIyVUwqJxWTylQxqXyi4kTlpGJSeaPiROWkYlKZKk5UpopJZaqYVE4qPvGw1rrGw1rrGg9rrWv88Msq3qg4UfmEylRxUjGpfEJlqphUTiomlU9UvKEyVZxUTCpTxaQyVUwqv0llqpgqJpUTlZOKb3pYa13jYa11jYe11jXsHz6gclIxqZxUTCpvVJyovFHxCZWTihOVk4pJZao4UZkqJpWpYlKZKt5QOak4UZkqTlQ+UXGiMlVMKlPFNz2sta7xsNa6xsNa6xo\/fKjiExWTylQxqUwVk8pUMVWcqJyovFFxonJSMalMKm+oTBWfqJhUpopJ5Q2Vk4pJZao4qbjJw1rrGg9rrWs8rLWuYf\/wRSonFW+onFScqEwVk8pJxTepvFFxojJVTCpTxaTyRsWkMlW8oXJSMamcVHxCZaqYVKaKE5U3Kj7xsNa6xsNa6xoPa61r\/PBlFZPKicpUMVVMKicqJypTxaTymyomlROVk4qTiknlpGJSmVT+l1RMKicVf6niLz2sta7xsNa6xsNa6xr2Dx9QmSomlb9U8YbKVHGiMlW8oTJVTConFZPKX6qYVN6oOFE5qfgmld9UMalMFd\/0sNa6xsNa6xoPa61r2D\/8h1SmijdUTio+ofKbKk5U3qh4Q2WqmFSmihOVk4pJZaqYVKaKN1SmikllqnhDZap4Q2Wq+MTDWusaD2utazysta7xw5epfJPKVHFS8YbKGxWTylTxhspJxaTyhspUcaIyVUwqU8VU8UbFpPIJlW9SmSpOVP5LD2utazysta7xsNa6xg8fUpkqJpVPVLyh8k0VJxUnKm9UfFPFGxWTylRxonJSMalMFd9U8YmKNypOVH7Tw1rrGg9rrWs8rLWu8cMvqzhRmVQ+UfGGyonKScWkclJxojJVTBWTyqTyTRWfqJhUpoo3VD6hcqLyTSonFd\/0sNa6xsNa6xoPa61r\/PChihOVk4pJZaqYVD6h8omKk4o3VKaKb6o4UTlRmSpOKiaVqeINlaniDZVvqjhRmSomld\/0sNa6xsNa6xoPa61r2D98kcpfqphUpopJ5f+SijdUTireUJkqTlSmiknlL1W8oTJVvKHyRsU3Pay1rvGw1rrGw1rrGvYPf0hlqphUpopPqPymiknlpOJE5RMVk8pJxaQyVfwmlaliUpkqJpWp4kRlqphUpooTlaliUpkqftPDWusaD2utazysta7xw\/84lZOKSWWqOFE5qThRmSpOVN6oOFGZVN5QmSo+ofJGxaQyVUwqb6hMFZ9QmSomlf\/Sw1rrGg9rrWs8rLWu8cOXqZxUTConFZ9QOak4UZkqpopJZao4qZhUTlSmim9SmSpOVN6omFSmikllqviEyicqJpWp4kRlqvimh7XWNR7WWtd4WGtdw\/7hi1TeqDhRmSomld9UMalMFZPKVDGpnFR8k8pU8YbKVPEJlTcqJpWpYlKZKj6hMlWcqEwVk8pU8U0Pa61rPKy1rvGw1rrGDx9SmSomlaliUpkqpopJ5Y2KSeWbVP6SylRxUjGpnFRMFW+oTBUnFZPKScUbKlPFGxVvVLyhMlV84mGtdY2HtdY1HtZa1\/jhQxWTyidUTiomlaliUjmpeKNiUjlRmSpOVKaKb6r4hMonKiaVqWJSOal4Q+Wk4g2VqWJSOan4poe11jUe1lrXeFhrXcP+4QMqU8WkMlVMKicVb6hMFScq\/6WKE5XfVPFNKm9UTConFd+kMlVMKicVk8pUMamcVHziYa11jYe11jUe1lrXsH\/4RSpTxYnKf6niN6m8UfEJlaniROWNiknlExWTyhsVk8pUMamcVHyTyknFJx7WWtd4WGtd42GtdY0fPqQyVUwVJypTxYnKGxUnKn+pYlKZKk5UTipOVKaKb6o4UZkqJpWTihOVE5Wp4kTljYqTiknlmx7WWtd4WGtd42GtdY0ffpnKScWJyknFN6n8poqpYlKZKqaKT1RMKicVn1CZKiaVqeINlaniDZWTihOVSWWqmFSmim96WGtd42GtdY2HtdY1fvhjFW9UvKHyiYo3VE4q3qh4Q+UNlZOKE5U3KiaVqeITFZ+omFROVKaKSeWkYlKZKj7xsNa6xsNa6xoPa61r\/PBlKlPFpPKbKiaVk4pJ5RMVk8obFScqJxWfUJkqpopJZaqYVE5UpooTlTcqTlSmim9S+UsPa61rPKy1rvGw1rqG\/cMXqZxU\/CaVqeITKlPFpHJSMal8ouINlZOKN1ROKt5QmSomlZOKSeWk4kRlqphU3qj4Sw9rrWs8rLWu8bDWuob9wx9SmSpOVH5TxaQyVUwqU8WkMlV8QmWqmFS+qWJSmSreUJkqTlTeqHhD5S9VTConFZ94WGtd42GtdY2HtdY1fvhjFW9UnKi8UTGpTBW\/SWWqeEPlpOINlW9SeUNlqjhRmVSmik9UvKHyRsWk8k0Pa61rPKy1rvGw1rrGDx9S+UsVU8WkMlVMKlPFpHJSMal8QmWqOKmYVE5Upoo3Kk5UpopJ5aRiUpkqTipOVKaKSeVEZao4qThRmSq+6WGtdY2HtdY1HtZa1\/jhyyq+SeVEZao4qZhUpopJ5Y2KSeWkYlL5poo3VE4qpopJ5URlqnij4i9VfFPFpDJVfOJhrXWNh7XWNR7WWtf44ZepvFHxTSpTxRsVk8o3qUwVk8obKr9J5Y2KSeWkYlKZKiaVb1L5TSq\/6WGtdY2HtdY1HtZa1\/jhMipvVHyi4qTiRGVSmSomlU9UTCpTxaQyVbyhMlW8UTGpTBUnKm9UfJPKScU3Pay1rvGw1rrGw1rrGj9cruJEZaqYVKaKSeWkYlJ5Q2WqmFTeUPkmlaliqphU3qg4UTmpmFSmiknljYqTiknlNz2sta7xsNa6xsNa6xo\/\/LKK31QxqXxCZaqYVKaKSWVSOamYVN6o+EsqU8U3VUwqU8WkMlVMKicqU8WkcqIyVZxU\/KaHtdY1HtZa13hYa13jhy9T+Usqv0nlRGWqmFSmipOKSWVS+aaKSeWk4kTlpOJEZaqYVD5RMam8UXGi8kbFNz2sta7xsNa6xsNa6xr2D2utKzysta7xsNa6xsNa6xoPa61rPKy1rvGw1rrGw1rrGg9rrWs8rLWu8bDWusbDWusaD2utazysta7xsNa6xsNa6xr\/D8Gh2lCeYYCOAAAAAElFTkSuQmCC",
    "valorcobrado": 2.52,
    "valor": "2.00000000",
    "pix_end2end_id": "E22896431202204020232HQeZHKL64SD"
    }

Observações:

O "txid" é o número da transação que você deve registrar no seu banco de dados para vincular a este recebimento.

O "copiaecola" é o código copia e cola para você retornar ao seu cliente na sua interface. Quando o metódo é btc, o valor refere-se à carteira BTC para a qual deve ser enviado o "valor cobrado".

O "qrcode" é a imagem QRcode em base64 para você exibir ao seu cliente.

A moeda do "valorcobrado" muda de acordo como método  de recebimento. Se o metódo é pix,  o valor é em reais.
Se o  método é "btc", é o valor exato a ser transferido em BTC (bitcoin). 

O "valor" é o valor a ser creditado.

O "pix_end2end_id" é identificador do Banco Central para a transação pix específica



## Exemplo de retorno Bitcoin (JSON)

    {
    "txid": "3W4CZ9HZDDYF",
    "copiaecola": "39RHpydmiimMEQVEQesm8cFG46LNQsZJGM",
    "qrcode": "https:\/\/chart.googleapis.com\/chart?chs=300x300&cht=qr&chl=39RHpydmiimMEQVEQesm8cFG46LNQsZJGM&choe=UTF-8",
    "valorcobrado": 0.00048631,
    "valor": "50.00000000",
    "method_code": "btc"
    }

Note que o retorno do QRcode do bitcoin é uma url ao invés de uma imagem em base64 e o **valorcobrado** é em BTC.

## html
Segue um exemplo de como mostrar o QRcode e o copiaecola em html

    <!DOCTYPE html>

    <div>
    <!-- url do code da carteira btc-->
    <img src="https:\/\/chart.googleapis.com\/chart?chs=300x300&cht=qr&chl=39RHpydmiimMEQVEQesm8cFG46LNQsZJGM&choe=UTF-8" >
    <!-- lembre de informar o valor em BTC a ser depositado >
    <p> Valor em btc a ser transferido  0.00048631 BTC</p>


    <!-- imagem png do pix em base64-->
    <img src="data:image/png;base64,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">
    <br>
    <button class="btn btn-success" onclick="copyPix();">Copiar Código PIX </button>



    </div>
    <script>
    function copyPix() {

    /* Copy the text inside the text field */
    navigator.clipboard.writeText('00020101021226910014br.gov.bcb.pix2569api.developer.dobank.capital/v1/p/v2/3694771894b9437fbfcee755d981b02f5204000053039865802BR5925Dobank Tecnologia em Paga6011So Paulo SP62070503***63048847');

    /* Alert the copied text */
    /*   alert("Pix copiado: 00020101021226910014br.gov.bcb.pix2569api.developer.dobank.capital/v1/p/v2/3694771894b9437fbfcee755d981b02f5204000053039865802BR5925Dobank Tecnologia em Paga6011So Paulo SP62070503***63048847" ); */
    }
    </script>

    </html>

6. Aguarde seu cliente efetuar o pagamento. 

7. Retorno do recebimento. 

### A) Você pode consultar os recebimentos dos últimos 2 dias:

Exemplo em python:    

    import json
    import requests
    url = 'https://dobank.capital/api/'
    endpoint = 'recebimentos'

    token = '<SEUTOKEN>'



    payload = {
        "token": token
    }
    #print(str(user))
    url = url+endpoint
    print(url, payload)
    r = requests.post(url, data=payload)

    # print(r.url, r.content, r.status_code)
    a = r.json()
    print(a)
    #adicione o seu código para registrar as informações no banco de dados

Exemplo em php:

    <?php
    function curl_post($url_endpoint,$payload){
    $ch = curl_init();
    curl_setopt($ch, CURLOPT_URL, $url_endpoint);
    curl_setopt($ch, CURLOPT_POST, true);
    curl_setopt($ch, CURLOPT_POSTFIELDS, http_build_query($payload)); 
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
    // curl_setopt($ch, CURLOPT_TIMEOUT, 20);
  
    $response = curl_exec($ch);
        curl_close($ch);
        return json_decode($response, true);
      }


      $token = '<SEUTOKEN>';

      //consulta
      $url = 'https://dobank.capital/api/';
      $endpoint = 'recebimentos';


      $result = curl_post($url.$endpoint,$payload);
      echo $result.'\n';
      $txid = "M434SEQ3NGS9"; //txid do retorno do recebimento
      $payload = [
        "token" => $token, 
      ]

      ?>

Exemplo em javascript:

    let data = {
        token: '<SEUTOKEN>',
    }

    fetch('https://dobank.capital/api/recebimentos', {
        method: "POST",
        body: JSON.stringify(data),
        headers: {"Content-type": "application/json; charset=UTF-8"}
      })
      .then(response => response.json()) 
      .then(json => console.log(json))
      .catch(err => console.log(err));



Exemplo de retorno:

    [
        {
            "trxid": "VNJ9BHNC4NW6",
            "method_code": "pix",
            "amount": "1.00000000",
            "amount_charged": "1.51200000",
            "status": "Aguardando Recebimento",
            "pix_end2end_id": "E22896431202204020232HQeZHKL64SD"
        },
        {
            "trxid": "SR4SHNC9B8D9",
            "method_code": "pix",
            "amount": "2.00000000",
            "amount_charged": "2.52400000",
            "status": "Aguardando Recebimento",
            "pix_end2end_id": "E22896431202204020232HQeZHKL64SD"
        }
    ]


### B) Para consultar um recebimento específico 

Exemplo em python:

    import json
    import requests
    url = 'https://dobank.capital/api/'
    endpoint = 'recebimentos'

    token = '<SEUTOKEN>'
    trxid = "VNJ9BHNC4NW5"

    payload = {
        "token": token, 
        "trxid": trxid
    }

    url = url+endpoint
    print(url, payload)
    r = requests.post(url, data=payload)

    # print(r.url, r.content, r.status_code)
    a = r.json()
    print(a)
    #adicione o seu código para registrar as informações no banco de dados

Exemplo em php:

    <?php
    function curl_post($url_endpoint,$payload){
    $ch = curl_init();
    curl_setopt($ch, CURLOPT_URL, $url_endpoint);
    curl_setopt($ch, CURLOPT_POST, true);
    curl_setopt($ch, CURLOPT_POSTFIELDS, http_build_query($payload)); 
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
    // curl_setopt($ch, CURLOPT_TIMEOUT, 20);
  
    $response = curl_exec($ch);
        curl_close($ch);
        return json_decode($response, true);
      }


      $token = '<SEUTOKEN>';

      //consulta
      $url = 'https://dobank.capital/api/';
      $endpoint = 'recebimentos';


      $result = curl_post($url.$endpoint,$payload);
      echo $result.'\n';
      $txid = "M434SEQ3NGS9"; //txid do retorno do recebimento
      $payload = [
        "token" => $token, 
      ]

      ?>
      
Exemplo em javascript:

     let data = {
        token: '<SEUTOKEN>',
        txid: 'XHDIFMASAII'
    }

    fetch('https://dobank.capital/api/recebimentos', {
        method: "POST",
        body: JSON.stringify(data),
        headers: {"Content-type": "application/json; charset=UTF-8"}
      })
      .then(response => response.json()) 
      .then(json => console.log(json))
      .catch(err => console.log(err));
      
      

 A única diferença entre a consulta dos recebimentos dos dois últimos dias e uma transação específica é informar o txid do recebimento. 


Exemplo de retorno de recebimento específico:



    [
        {
            "trxid": "VNJ9BHNC4NW6",
            "method_code": "pix",
            "amount": "1.00000000",
            "amount_charged": "1.51200000",
            "status": "Aguardando Recebimento",
            "pix_end2end_id": "E22896431202204020232HQeZHKL64SD"
        }
    ]


Exemplo de retorno de erro:

    {
        "error": "N\u00e3o foram encontrados registros"
    }


8. Webhook

Alternativamente, ao invês de ficar consultando periodicamente para checar se o recebimento foi feito, você pode configurar um webhook - um endpoint POST na sua aplicação -  para ser notificado quando ocorrer uma transação da API. ]

Exemplo:

    https://meusite.com.br/webhook_dobank



O webhook receberá uma mensagem no mesmo formato da consulta individual:

        [
            {
                "trxid": "VNJ9BHNC4NW6",
                "method_code": "pix",
                "amount": "1.00000000",
                "amount_charged": "1.51200000",
                "status": "Recebido",
                "pix_end2end_id": "E22896431202204020232HQeZHKL64SD"
            }
        ]

Recomendamos que o webhook seja configurado  para receber solicitações somente do domínio https://dobank.capital
