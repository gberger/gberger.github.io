---
layout: post
title: "Falha de segurança na Ingresso.com vaza ingressos e informações pessoais"
permalink: /blog/10/falha-seguranca-ingresso-com/
ogimage: /img/posts/poc1.png
redirect_from:
  - /blog/10/
---

*Atualização em 17 de abril no final do post.*


Se você usa a [Ingresso.com](https://www.ingresso.com) para comprar ingressos de cinema, teatro e shows, saiba que você corre risco de não conseguir entrar no evento por ter tido seu ingresso roubado digitalmente. **Pior: seus dados pessoais como nome, CPF, RG estão expostos na internet.**

Isso é porque existe uma óbvia falha de segurança na Ingresso.com que permite que qualquer pessoa tenha acesso à lista de ingressos de outros usuários ao acessar uma página da web.

Essa página contém, em seu endereço, o ID do usuário em questão. Basta alterar esse número para algum outro para que, depois de algumas poucas tentativas, ache-se um outro cliente válido com ingressos disponíveis.

Aí, basta acessar o endereço web de um dos ingressos para poder imprimí-lo. E agora aproveite a sessão que o filme já vai começar! ...


## O criminoso em ação

Eu desenvolvi um pequeno programa que realiza o procedimento descrito acima, mostrando como é trivial abusar da falha.

Observe o resultado do programa depois de alguns poucos segundos:

<a href="/img/posts/poc1.png"><img src="/img/posts/poc1.png"></a>

O código censurado é o código do ingresso. Agora basta substituir o código no endereço de uma página de impressão de ingresso. Note que o nome e RG do comprador foi escondido, assim como a informação de que a entrada é "Meia Itaú" (o que também pode expor dados pessoais importantes), mas na página real estes dados estão expostos.

<a href="/img/posts/ingresso1.png"><img src="/img/posts/ingresso1.png"></a>

Caso o ingresso interesse ao criminoso, basta imprimí-lo e levar ao cinema, teatro ou show. Se ele chegar antes do cliente real, o código de barras será validado e ele terá acesso ao evento, enquanto o cliente ficará de fora e sofrerá constrangimento.

Observe que também é trivial para o criminoso alterar os dados pessoais na face do ingresso para os seus próprios, evitando que o funcionário responsável suspeite de sua má conduta.


## A falha é conhecida pela empresa


Em visita ao escritório da Ingresso.com, no Centro do Rio, em janeiro deste ano, **mostrei a falha a um funcionário de alto cargo na área de segurança da informação da empresa**. Ele comentou que a falha era conhecida havia meses, mas que estavam com problemas internos no processo de correção.

Depois de ter comunicado aos responsáveis sobre a falha e nada ter sido feito, acredito que é meu dever escrever este artigo para alertar os consumidores sobre este risco. Eu não usei os ingressos e os dados pessoais em meu benefício, assim como não divulguei a URL específica que dá acesso à lista de ingressos, pois meu objetivo não é causar danos à empresa, muito menos a seus clientes. 

Mas assim como eu tomei conhecimento da falha, criminosos também poderiam estar cientes dela, mesmo antes de eu ter publicado este artigo. Isto é: é muito provável que esta falha esteja sendo ativamente usada por criminosos.

Espero que com a divulgação desta informação a empresa se responsabilize pelo erro e corrija a falha. Espero também que ela investigue se houve abuso da falha por partes de criminosos, prezando o bem-estar de seus clientes.

---

Vale lembrar que em 2014, um [artigo foi publicado](http://agner.io/ingresso-com-como-nao-lidar-com-seguranca-da-informacao.html) detalhando duas falhas de segurança no site. A falha descrita aqui expõe os mesmos dados da falha descoberta por Marco Agner, mas o processo para acessá-los é um pouco diferente.


## Atualização em 17 de abril, 12:45

Desde ontem venho conversando com um desenvolvedor da Ingresso por email e agora fui
informado de que a falha de segurança foi corrigida.

Seguindo princípios de responsible disclosure, o código que eu criei, mencionado no texto,
está disponível em [meu GitHub](https://github.com/gberger/ingresso.hack). Lá você também
pode encontrar detalhes técnicos da falha.

Obrigado a todos que compartilharam, pois foi graças à ampla divulgação que conseguimos
chamar a atenção da empresa para o problema -- **coisa que uma visita ao escritório do diretor
de segurança da informação da empresa há 3 meses não conseguiu.**

Sinceramente espero que a Ingresso.com se responsabilize e divulgue nota prestando esclarecimento 
sobre o vazamento. Não fazê-lo é egoísta, irresponsável e antiético, além de ir contra os 
[princípios de privacidade](http://www.ingresso.com/rio-de-janeiro/home/institucional/privacidade-seguranca)
da empresa. Os clientes tem direito de saber que suas informações vazaram.
