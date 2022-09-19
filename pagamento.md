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

## Pagamento

Exemplo em python com um tipo de cada pagamento 

	import json
	import requests
	url = 'https://dev.dobank.capital/api/'
	endpoint = 'pagamento'

	token = '<SEUTOKEN>'
	valor = 1.00 # R$ 1,00


	payload = {

			"token": token,
			"lote": 1, #obrigatorio, seu número de controle
			"beneficiaries": [
	      # Exemplo PIX
	      {
	        "integration_id": 1, # seu número de controle de cada pagamento
		"amount": 1.00,
		"pix_key_type": "CPF", # [EMAIL, CPF, CNPJ , TELEFONE , CHAVE_ALEATORIA]
		"pix_key": "012345678901"
	      }
	      ,
	      # Exemplo DOBANK
	      {
	          "integration_id": 2, # seu número de controle de cada pagamento
		  "amount": 1.00,
	          "pix_key_type": "DOBANK", #Transferência de Dobank para Dobank
		  "pix_key": "001DB24555A7474" # Numero da conta do recebedor Dobank
	      },
	      # Exemplo DADOS_BANCARIOS
	      {
	      	"integration_id": 3, # seu número de controle de cada pagamento
		"amount": 1.00,
		"pix_key_type": "DADOS_BANCARIOS",
		"pix_key": "00000000000", # CPF/CNPJ do beneficiário
		"bank_code": "001", # Código do banco
		"agency": "0001", # Código da agência
		"account_number": "0000", # Número da conta
		"account_digit": "0", # Dígito da conta
		"account_type": "CONTA_CORRENTE", # Tipo da conta (CONTA_CORRENTE, CONTA_POUPANCA, CONTA_PAGAMENTO , CONTA_FACIL , ENTIDADES_PUBLICAS)
		"name": "Nome do beneficiário", # Nome do beneficiário
	      }
	    ]

	}
	r = requests.post(api_url+endpoint, json=payload)
	print(r.status_code,r.content)
	try:
	    info = r.json()
	    print(info)
	except Exception as e:
	    print(f"Error {e}")




	
retorno da api com um exemplo de cada tipo:
	
	{
	"payer": {
		"name": "João da Silva",
		"email": "joaodasilva@mail.com",
		"cpf_cnpj": null,
		"amount": "1.00",
		"charge": null,
		"final_amount": "1.00",
		"current_balance": "330.09"
	},
	"beneficiaries": [{
	     {
            "integration_id": 1,
            "trx": "F6YZ5M4CCMAC",
            "name": "José da Silveira",
            "pix_key_type": "CPF",
            "pix_key": "0000000000",
            "account_number": null,
            "bank_code": null,
            "agency": null,
            "amount": "1.00",
            "status": "APROVADO",
            "lote_original": 1,
            "created_at": "2022-09-11T22:14:30.000000Z",
            "updated_at": "2022-09-11T22:14:30.000000Z"
        }
	,{
		"beneficiary": {
            "integration_id": 2,
            "trx": "F6YZ5M4CCMAC",
            "name": null,
            "pix_key_type": "DOBANK",
            "pix_key": "001CC22145058457579",
            "account_number": null,
            "bank_code": null,
            "agency": null,
            "amount": "1.00",
            "status": "APROVADO",
            "lote_original": 1,
            "created_at": "2022-09-11T22:14:30.000000Z",
            "updated_at": "2022-09-11T22:14:30.000000Z"
		}
	},      {
            "integration_id": 3,
            "trx": "F6YZ5M4CCMAC",
            "name": null,
            "pix_key_type": "DADOS_BANCARIOS",
            "pix_key": "001CC22145058457579",
            "account_number": 12345,
            "bank_code": 1,
            "agency": 1,
            "amount": "1.00",
            "status": "APROVADO",
            "lote_original": 1,
            "created_at": "2022-09-11T22:14:30.000000Z",
            "updated_at": "2022-09-11T22:14:30.000000Z"
        }
	}]
	}
	
OBSERVAÇÃO:
A api rejeitará os lotes e pagamentos com ids já enviados. 


## consulta de Lotes

é possível consultar os pagamentos 1) por número de dias 2) por trxid e 3) por lote

1) exemplo em python de consulta por número de dias

	import json
	import requests
	import os
	from dotenv import load_dotenv

	load_dotenv()

	api_url = 'https://dobank.com.br/api'
	endpoint = 'pagamentos'
	token = '<SEUTOKEN>"

	payload = {
	  "token": token,
	  # "lote": "3",
	  # "trxid": "8EZSC1J3G65U",
	  "days": "30",
	}
	url = f'{api_url}{endpoint}'
	print(url,payload)
	r = requests.post(api_url+endpoint, data=payload)

	try:
	    info = r.json()
	    print(info)
	except Exception as e:
	    print(r.status_code,r.content)
	    print(f"Error {e}")

	
2) Consulta em python por transação
	
	
	import json
	import requests
	import os
	from dotenv import load_dotenv

	load_dotenv()

	api_url = 'https://dobank.com.br/api'
	endpoint = 'pagamentos'
	token = '<SEUTOKEN>"

	payload = {
	  "token": token,
	  #"lote": "3",
	  "trxid": "8EZSC1J3G65U",
	  #"days": "30",
	}
	url = f'{api_url}{endpoint}'
	print(url,payload)
	r = requests.post(api_url+endpoint, data=payload)

	try:
	    info = r.json()
	    print(info)
	except Exception as e:
	    print(r.status_code,r.content)
	    print(f"Error {e}")


3) Consulta em python por lote
	
	
	import json
	import requests
	import os
	from dotenv import load_dotenv

	load_dotenv()

	api_url = 'https://dobank.com.br/api'
	endpoint = 'pagamentos'
	token = '<SEUTOKEN>"

	payload = {
	  "token": token,
	  "lote": "3",
	  # "trxid": "8EZSC1J3G65U",
	  #"days": "30",
	}
	url = f'{api_url}{endpoint}'
	print(url,payload)
	r = requests.post(api_url+endpoint, data=payload)

	try:
	    info = r.json()
	    print(info)
	except Exception as e:
	    print(r.status_code,r.content)
	    print(f"Error {e}")


	
	
