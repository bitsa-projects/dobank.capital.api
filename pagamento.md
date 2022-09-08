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
			"beneficiaries": [
	      # Exemplo PIX
	      {
				"amount": 1.00,
				"pix_key_type": "CPF", # [EMAIL, CPF, CNPJ , TELEFONE , CHAVE_ALEATORIA]
				"pix_key": "012345678901"
			  }
	      ,
	      # Exemplo DOBANK
	      {
	      "amount": 1.00,
				"pix_key_type": "DOBANK",
				"pix_key": "001DB24555A7474"
	      },
	      # Exemplo DADOS_BANCARIOS
	      {
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



Retorno da api em json

        {
	"payer": {
		"name": "José da Couves",
		"email": "inventado@gmail.com",
		"cpf_cnpj": null,
		"amount": "1.00",
		"charge": null,
		"final_amount": "1.00",
		"current_balance": "5.09"
	},
	"beneficiaries": [{
		"beneficiary": {
			"account_name": "Maria das Couves",
			"pix_key_type": "CPF",
			"pix_key": "012345678901",
			"account_number": null,
			"bank_code": null,
			"amount": "1.00"
		}
	}]
	}
	
retorno da api com um exemplo de cada tipo:
	
	{
	"payer": {
		"name": "Alexandre",
		"email": "alexandremasbr@gmail.com",
		"cpf_cnpj": null,
		"amount": "1.00",
		"charge": null,
		"final_amount": "1.00",
		"current_balance": "3.09"
	},
	"beneficiaries": [{
		"beneficiary": {
			"account_name": "Alexandre Miguel de Andrade Souza",
			"pix_key_type": "CPF",
			"pix_key": "42137276291",
			"account_number": null,
			"bank_code": null,
			"amount": "1.00"
		}
	}, {
		"beneficiary": {
			"account_name": "Alexandre Miguel de Andrade Souza",
			"pix_key_type": "DADOS_BANCARIOS",
			"pix_key": "0",
			"account_number": "12743",
			"bank_code": 1,
			"amount": "1.00"
		}
	}, {
		"beneficiary": {
			"account_name": null,
			"pix_key_type": "DOBANK",
			"pix_key": "0001DB222241406010",
			"account_number": null,
			"bank_code": null,
			"amount": "1.00"
		}
	}]
	}
	
