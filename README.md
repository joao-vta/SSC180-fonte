# Projeto de Fonte de tensão
Esse projeto descreve uma fonte de tensão que recebe como entrada 110v de tensão alternada, e produz como saída 3 a 12v de corrente continua. Ele foi criado para a disciplina SSC0180 - Eletrônica para Computação, ministrada pelo professor Eduardo do Valle Simões.

## Especificações
> A fonte espera como entrada uma tensão alternada efetiva de 127v com frequência de 60hz, como provida em diversas redes pelo país. Ela gera uma tensão regulavel que vai de 3v até 12v, e provê um mínimo de 100mA. 

## Coisas
Para efeitos de calculo, consideramos a carga máxima como 200mA, de modo a dar margem de segurança.  
A fonte primeiro reduz a tensão de entrada que possui 127 * raiz(2) = 180v de pico para uma tensão com 20v de pico, por meio de um [transformador](https://produto.mercadolivre.com.br/MLB-1337849996-transformador-terminais-110v220v-saida-20v-20v-200ma-_JM#position=6&search_layout=stack&type=item&tracking_id=f3292d6c-98bd-455b-b6ad-afc45fc8f0fd). Ele foi escolhido do [catalogo](http://www.transformadoreslider.com.br/catalogo/catalogo.pdf) por ser capaz de realizar a trasnformação desejada com 200mA de corrente.

![Imagen da Tensão](https://raw.githubusercontent.com/joao-vta/SSC180-fonte/main/imagens/tensaoAlternada127v.png)

Em seguida, a tensão é retificada por meio de uma ponte de diodos. Isso garante que a tensão que passe seja positiva. Os diodos tem que aguentar 200mA de corrente e 20V de tensão, atendidos pelo modelo [1N4001](https://www.baudaeletronica.com.br/diodo-1n4001.html).  
Cada diodo 1N4001 corta 0,7V, e a corrente passa por exatamente dois diodos a cada momento na ponto de diodo. Como consequência, a tensão de entrada de 20V de pico cai para 18,6V de pico.  

![Imagen da Tensão](https://raw.githubusercontent.com/joao-vta/SSC180-fonte/main/imagens/cmpletaRetificada.png)

A tensão então passa por um filtro capacitivo que reduz a a amplitude da onda. 

![Imagen da Tensão](https://raw.githubusercontent.com/joao-vta/SSC180-fonte/main/imagens/ripple.png)

