# Api de pagamento Dobank

## Consulta de Saldo

Exemplo em python

    import json
    import requests
    url = 'https://dobank.capital/api/'
    endpoint = 'saldo'

    payload = {
      "token": 'seutoken',
    }

    r = requests.post(api_url+endpoint, data=payload)

    info = r.json()
    print(info)


  continua
