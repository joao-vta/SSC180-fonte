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

Em seguida, usamos um diodo zener para regular a tensão, removendo a oscilação da onda descrita acima. Ele gera uma tensão de referência, porém não pode receber corrente demais. Tivemos que usar então um transistor bipolar NPN para controlar o fluxo de corrente. Como o transistor consome 0.7V, o zener precisa de ao mínimo 12 + 0.7 = 12.7V. O valor comercial mais próxmo é 13v, portanto esse foi o [zener] que usamos.

O zener tem um limite de potência baixo, portanto o colocamos em série com um resitor de 1KΩ. Ele precisa de ao menos 13V para gerar a tensão de referência, portanto o resistor não pode consumir mais que 18.6 - 13 = 5.6V no pico e 15 - 13 = 2V no mínimo.

[circuito](http://falstad.com/circuit/circuitjs.html?ctz=CQAgjCAMB0l3BWcA2aAOMB2ALGXyEw1sESQFJyRsBmcgUwFowwAoAdxEezRAE5kKfoMgcuPEACZJ2IdNmiAKnJkhksgVGpRoYSHy4wwyEzWJ9JYBJkmZ9OmhbCT92OHwrqR0Pr7HrhcGwNEVYAN3FeeUjAyghkOLRKZJ0EMU1jSk1JBFDObNypQpy88AsUOPLNUU4wKsES2P9ZaLwW1VEAYyLBAobC5Ng4bExwNGgZBBoaMBpkGmsCaZ04Nk5GzRoXJs4tykaDgdYAE0DMoJCpEGP6ADMAQwBXABsAFxOzhIupVUlru6ebw+bR+sjqf3OfxuDxe71OIIy5Uh-xhQN2202xB2ID2PRxWMaogATjiMYJcec4vBWDRZAAvegAO3oROYdAgYGgmDwC0kGAsGHgkiYo32Q3gEslWNm4FYdNJ+0KFK+fwZzNZs1Yr2oxgq1CSeriXDw0GQfGwXgEU0gU2QfxgyDQmAEJjsaF8jvmUjgKMB704PDiwX1WVKgcCZkuNRDeNxhNYJPDWEE4cJICSrCwskjepzLl48XgOl8FElZYgACV6ABnACW1de90ZnXoYnwerclXtCfx7WzBIGRTgYhz52VyVYAHsimoFNR9AZtjB7VccjjWEA) temporario

[zener]: https://www.baudaeletronica.com.br/diodo-zener-1n4743-13v-1w.html
[transformador]: https://produto.mercadolivre.com.br/MLB-1337849996-transformador-terminais-110v220v-saida-20v-20v-200ma-_JM#position=7&search_layout=stack&type=item&tracking_id=9352b21c-9685-440a-ba31-5a1ffce72d90
[diodo]: https://www.baudaeletronica.com.br/diodo-1n4001.html
[capacitor]: https://www.baudaeletronica.com.br/capacitor-eletrolitico-470uf-25v.html
