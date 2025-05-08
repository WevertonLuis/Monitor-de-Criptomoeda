![image](https://github.com/user-attachments/assets/16fcc3eb-f759-46cc-bdd1-471e0119e943)

![image](https://github.com/user-attachments/assets/5115a040-6339-44f8-92b5-f5de9d0be6df)

Esse app é um aplicativo Android que permite atualizar a cotação do Bitcoin
Clicando no botão de atualizar, o app traz a cotação mais recente do Bitcoin.

Como o funciona?

O app utiliza a API do Mercado Bitcoin para buscar a cotação atual da criptomoeda. 

As chamadas HTTP são feitas usando a biblioteca Retrofit.

As respostas JSON são convertidas em objetos Kotlin usando o Gson, uma poderosa biblioteca do Google para trabalhar com dados no formato JSON.

Modelo de resposta (TickerResponse)

A classe TickerResponse é responsável por modelar o payload da resposta da API.
Se a API for atualizada futuramente, é só adicionar novos campos nessa classe..

```kotlin

class TickerResponse(
        val ticker: Ticker
)

class Ticker(
        val high: String,
        val low: String,
        val vol: String,
        val last: String,
        val buy: String,
        val sell: String,
        val date: Long
)

```
Serviço de comunicação com a API

O serviço MercadoBitcoinService define a requisição que consulta a cotação atual do Bitcoin:

```kotlin

import retrofit2.Response
import retrofit2.http.GET
import wevertonluis.com.github.cryptomonitor.model.TickerResponse

interface MercadoBitcoinService {

    // Fazendo o uso do endpoint em cima da url base disponibilizada no arquivo MercadoBitcoinServiceFactory.kt
    @GET("api/BTC/ticker/")
    suspend fun getTicker(): Response<TickerResponse>
}

```

Atualizando a tela com dados em tempo real

Dentro da MainActivity, o app usa Coroutines (CoroutineScope) para fazer a chamada e atualizar a interface assim que clicar no botão atualiar.

Além disso, o app formata:

O valor da moeda com NumberFormat, usando o padrão monetário brasileiro:

```kotlin

 // formatando número
 val numberFormat = NumberFormat.getCurrencyInstance(Locale("pt", "BR"))
 lblValue.text = numberFormat.format(lastValue)

```

A data e hora da atualização:

```kotlin

//formatando data
val date = tickerResponse?.ticker?.date?.let { Date(it * 1000L) }
val sdf = SimpleDateFormat("dd/MM/yyyy HH:mm:ss", Locale.getDefault())

```










