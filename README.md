Cielo
=====

Integração com o Cielo e-Commerce webservice.

Por ora, a idéia é integrar somento no modelo BuyPageCielo onde a digitação dos dados do cartão será no ambiente da Cielo.
Possívelmente será expandido para o modelo BuyPageLoja onde digitação dos dados do cartão será no ambiente da Loja.

A integração pretende cobrir apenas compras por cartão de créditos sem parcelamento.

Pull requests são bem vindos \o\~

Modelo CieloBuyPage
-----

### Criando uma transação

	var order = new Order("12345", 4700.00m, DateTime.Now, "Goku e GokuSSJ");
	var paymentMethod = new PaymentMethod(CreditCard.MasterCard, PurchaseType.Credit);
	var options = new CreateTransactionOptions(AuthorizationType.AuthorizePassByAuthentication, capture: true);
	var createTransactionRequest = new CreateTransactionRequest(order, paymentMethod, options);
	
	var response = _cieloService.CreateTransaction(createTransactionRequest);
	
response contem a url para qual o cliente precisa ser redirecionado e o Tid (identificador único daquela transação) que usaremos mais tarde para verificar o status da transação.

### Verificando status da transação
		var tid = GetTid() // Recuperar o tid da transação (db,session,whatever)
		var checkTransactionRequest = new CheckTransactionRequest((tid);
		var response = _cieloService.CheckTransaction(checkTransactionRequest);

response dessa vez contem o Status da transação que pode conter os seguintes valores:

### Enum status

        Default = -1,
        Created = 0,
        InProgress = 1,
        Authenticated = 2,
        NotAuthenticated = 3,
        Authorized = 4,
        NotAuthorized = 5,
        Success = 6,
        Canceled = 9,
        AuthenticationProgress = 10,
        CancellationProgress = 12
		
Modelo LojaBuyPage
-----

Breve~