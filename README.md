# Projeto de Fonte de tensão
Esse projeto descreve uma fonte de tensão que recebe como entrada 110v de tensão alternada, e produz como saída 3 a 12v de corrente continua. Ele foi criado para a disciplina SSC0180 - Eletrônica para Computação, ministrada pelo professor Eduardo do Valle Simões.

## Especificações
 A fonte espera como entrada uma tensão alternada efetiva de 127v com frequência de 60hz, como provida em diversas redes pelo país. Ela gera uma tensão regulável que vai de 3v até 12v, e provê um mínimo de 100mA. 
 
## Peças Utilizadas

| Nome | Especificação Básica | Valor |
| :---: | :---: | :---: |
| [transformador] | Entrada: 110V-220V / Saída: 20V 200 mA | R$35,00 |
| [Capacitor] | 470uF / 25V | R$0,43 |
| [Diodo] | 50V / 1A | R$0,14 |
| [zener] | 13V / 1W | R$0.20 |
| [potenciometro] | 10KΩ | R$1.61 |
| [transistor] |  hfe = 110 | R$0.17 |
| [resistor5k] | 5KΩ | R$0.10 |
| LED | verde |  R$1.00 |

## Fonte
Para efeitos de cálculo, consideramos a carga máxima como 150mA, de modo a dar margem de segurança.  
Primeiramente a fonte reduz a tensão de entrada que possui <a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;\bg_white&space;127\times\sqrt{2}\approx180" target="_blank"><img src="https://latex.codecogs.com/png.latex?\inline&space;\bg_white&space;127\times\sqrt{2}\approx180" title="127\times\sqrt{2}\approx180" /></a> de pico para uma tensão com 20v de pico, por meio de um [transformador]. Ele foi escolhido do [catálogo](http://www.transformadoreslider.com.br/catalogo/catalogo.pdf) por ser capaz de realizar a trasnformação desejada e aguentar 150mA de corrente.

![Imagem da Tensão](https://raw.githubusercontent.com/joao-vta/SSC180-fonte/main/imagens/tensaoAlternada127v.png)

Em seguida, a tensão é retificada por meio de uma ponte de diodos. Isso garante que a tensão que passe seja positiva, como demonstrado na animação a seguir: 

![GIF da ponte de diodos](https://i.pinimg.com/originals/b0/fe/d2/b0fed20dce97fc8b666fffbb4843afae.gif). 

Os diodos devem aguentar 150mA de corrente e 20V de tensão, atendidos pelo modelo [1N4001].  
Cada diodo 1N4001 corta 0,7V, e a corrente passa por exatamente dois diodos a cada momento na ponto de diodo. Como consequência, a tensão de entrada de 18V de pico cai para 18,6V de pico. Após a retificação, a onda possuirá esse formato:

![Forma de onda após ponte de diodos](https://raw.githubusercontent.com/joao-vta/SSC180-fonte/main/imagens/cmpletaRetificada.png)

A tensão então passa por um filtro capacitivo que reduz a amplitude da onda, e deixando o formato de onda mais estável. A razão entre os valores máximos e mínimos da onda após filtragem é definida como o valor de ripple.
Determinando a tensão mínima como sendo 15V, por segurança, achamos a capacitância:

![Forma de onda após filtro capacitivo](https://raw.githubusercontent.com/joao-vta/SSC180-fonte/main/imagens/ripple.png) ![Imagen das contas](https://raw.githubusercontent.com/joao-vta/SSC180-fonte/main/imagens/contasExp.png)

Como 472uF não é um valor comercial, escolhemos um [capacitor] de 470uF, próximo o bastante e capaz de aguentar a voltagem necessária.

Em seguida, usamos um diodo zener para regular a tensão, removendo a oscilação da onda descrita acima. Ele gera uma tensão de referência, porém não pode receber corrente demais. Tivemos que usar então um [transistor] bipolar NPN para controlar o fluxo de corrente. Como o transistor consome 0.7V, o zener precisa de ao mínimo 12 + 0.7 = 12.7V. O valor comercial mais próxmo é 13v, portanto esse foi o [zener] que usamos.

![Imagem do zener](https://raw.githubusercontent.com/joao-vta/SSC180-fonte/main/imagens/zener.png)

O zener tem um limite de potência baixo, portanto o colocamos em série com um resitor de 1KΩ. Ele precisa de ao menos 13V para gerar a tensão de referência, portanto o resistor não pode consumir mais que 18.6 - 13 = 5.6V no pico e 15 - 13 = 2V no mínimo.

Calculando a resistência para alcançar isso, temos 1K/x = 2/15 -> x = 7,5KΩ. Usamos um [potenciometro] para controlar quanta dessa tensão chega ao transitor. O menor valor comercial que atendeu isso foi 10KΩ.

Colocamos ao final um LED em paralelo com o capacitor para indicar o funcionamento efetivo da fonte. Ele ficou em série com um resitor de 5K para não queimar.

[circuito] resultante:

![Imagem do circuito](https://raw.githubusercontent.com/joao-vta/SSC180-fonte/main/imagens/circuito.png)


[circuito](http://falstad.com/circuit/circuitjs.html?ctz=CQAgjCAMB0l3BWcA2aAOMB2ALGXyEw1sESQFJyRsBmcgUwFowwAoAdxEezRAE5kKfoMgcuPEACZJ2IdNmiAKnJkhksgVGpRoYSHx1g++zDWR9M6sMlNokjSbDtpIaU30nI0aY5iZg6GD5gsXVhcGwNEVYAN3FeeXjwyghkFJctShgEMU1rSk1JBGjOQuKpVSKS8A8UFNrNUU4wxLxZRNEAYwqonqlyrNg4bExwNF1cTEgaSBtLGz5eGDg2TklMSXCaSQLogBNw-Ijezb36ADMAQwBXABsAF1YDvLTjvtOLm4ent9bao4+VzujwObUO-1egK+j0420oVRANGI-WiACdETstpijil4KwaLIAF70AB29FRzDoEDA0EweBoCEkGA8GHgkiYo3hQ3gPN5yIC4FYhIx8PKcLqUhAxLJFICrHu1GsEp4KVeKS4WHQUyRFjQkhoU0sOnWATAkmC2AtFDNdB2lDOQO+nBVbxdjTEbsESN6TWoGQR4oRonRLqwghdQZALg9SqO2DgNU2vu9EvFOLEiQR602QbEgfK2ZRUDERk2hVU7rWFcEhfd6NrNY2EsoFDgrGsOabAKbWZADuhjFu9FBmR0kFWIq2yPdWFkKYR8dV6r08B0SAASvQAM4ASy390uJM69FYAHsKmoFNR9AZlvBLOaaHwSDQzUg73ALARkEUtDnNjQrBAA) temporario

[LED]: https://lista.mercadolivre.com.br/led-verde-5mm
[resistor5k]: https://pt.aliexpress.com/item/1005002041218051.html?aem_p4p_detail=202107211533282465907643956900009483945
[transistor]: https://www.baudaeletronica.com.br/transistor-npn-bc548.html
[potenciometro]: https://www.baudaeletronica.com.br/potenciometro-linear-de-10k-10000.html
[zener]: https://www.baudaeletronica.com.br/diodo-zener-1n4743-13v-1w.html
[transformador]: https://produto.mercadolivre.com.br/MLB-1337849996-transformador-terminais-110v220v-saida-20v-20v-200ma-_JM#position=7&search_layout=stack&type=item&tracking_id=9352b21c-9685-440a-ba31-5a1ffce72d90
[diodo]: https://www.baudaeletronica.com.br/diodo-1n4001.html
[capacitor]: https://www.baudaeletronica.com.br/capacitor-eletrolitico-470uf-25v.html
