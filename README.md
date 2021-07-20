# Projeto de Fonte de tensão
Esse projeto descreve uma fonte de tensão que recebe como entrada 110v de tensão alternada, e produz como saída 3 a 12v de corrente continua. Ele foi criado para a disciplina SSC0180 - Eletrônica para Computação, ministrada pelo professor Eduardo do Valle Simões.

## Especificações
 A fonte espera como entrada uma tensão alternada efetiva de 127v com frequência de 60hz, como provida em diversas redes pelo país. Ela gera uma tensão regulável que vai de 3v até 12v, e provê um mínimo de 100mA. 
 
## Peças Utilizadas

| Nome | Especificação Básica | Valor |
| :---: | :---: | :---: |
| [transformador] | Entrada: 110V-220V / Saída: 9V+9V 200 MA | R$28,99 |
| Capacitor | 470uF / 25V | R$0,43 |
| Diodo | 50V / 1A | R$0,14 |

## Fonte
Para efeitos de cálculo, consideramos a carga máxima como 150mA, de modo a dar margem de segurança.  
Primeiramente a fonte reduz a tensão de entrada que possui <a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;\bg_white&space;127\times\sqrt{2}\approx180" target="_blank"><img src="https://latex.codecogs.com/png.latex?\inline&space;\bg_white&space;127\times\sqrt{2}\approx180" title="127\times\sqrt{2}\approx180" /></a> de pico para uma tensão com 18v de pico, por meio de um [transformador](https://produto.mercadolivre.com.br/MLB-1251924135-transformador-trafo-99v-200ma-bivolt-eletronica-_JM#reco_item_pos=2&reco_backend=machinalis-seller-items-pdp&reco_backend_type=low_level&reco_client=vip-seller_items-above&reco_id=559de26e-339f-4593-91cf-1d10a5a540eb). Ele foi escolhido do [catálogo](http://www.transformadoreslider.com.br/catalogo/catalogo.pdf) por ser capaz de realizar a trasnformação desejada e aguentar 150mA de corrente.

![Imagem da Tensão](https://raw.githubusercontent.com/joao-vta/SSC180-fonte/main/imagens/tensaoAlternada127v.png)

Em seguida, a tensão é retificada por meio de uma ponte de diodos. Isso garante que a tensão que passe seja positiva, como demonstrado na animação a seguir: 

![GIF da ponte de diodos](https://i.pinimg.com/originals/b0/fe/d2/b0fed20dce97fc8b666fffbb4843afae.gif). 

Os diodos devem aguentar 150mA de corrente e 20V de tensão, atendidos pelo modelo [1N4001](https://www.baudaeletronica.com.br/diodo-1n4001.html).  
Cada diodo 1N4001 corta 0,7V, e a corrente passa por exatamente dois diodos a cada momento na ponto de diodo. Como consequência, a tensão de entrada de 18V de pico cai para 16,6V de pico. Após a retificação, a onda possuirá esse formato:

![Forma de onda após ponte de diodos](https://raw.githubusercontent.com/joao-vta/SSC180-fonte/main/imagens/cmpletaRetificada.png)

A tensão então passa por um filtro capacitivo que reduz a amplitude da onda, e deixando o formato de onda mais estável. A razão entre os valores máximos e mínimos da onda após filtragem é definida como o valor de ripple.
Determinando a tensão mínima como sendo 13V, por segurança, achamos a capacitância:

![Forma de onda após filtro capacitivo](https://raw.githubusercontent.com/joao-vta/SSC180-fonte/main/imagens/ripple.png) ![Imagen das contas](https://raw.githubusercontent.com/joao-vta/SSC180-fonte/main/imagens/contasExp.png)

Como 425uF não é um valor comercial, escolhemos um [capacitor](https://www.baudaeletronica.com.br/capacitor-eletrolitico-470uf-25v.html) de 470uF, capaz de aguentar a voltagem necessária.

[circuito](http://falstad.com/circuit/circuitjs.html?ctz=CQAgjCAMB0l3BWcA2aAOMB2ALGXyEw1sESQFJyRsBmcgUwFowwAoAdxEezRAE5kKfoMgcuPEACZJ2IdNmiAKnJkhksgVGpRoERjDzJpktGkg0aaGpOt8dcHAhq00CV30SS7MPr7HrhcGwNEVYAN3FeeUjAyghkOLMtShgEMU0wBMDJBFDOTRzBQqlcqDEwPkkUOMrY8tqC0s1RTgDovFlo0QAnEGtKTX6SkRAzVgBjYezS4pTYOGwclGhg9Uw+BEw3PhJITHtINk5iwckBvL6zqeLZ1gATQMy44OyQO-oAMwBDAFcAGwALvdHlkOlJVFV3t9-kCHmD2rUnlI3p9foDgWCMoispDUTCxENBsQ6pwhsVLJ1Sj0+sSibIkXF4KwAF40ymCCnVZFQtEAxjM+gAO3o3QJxKR+C5ogB1EyXJ4cSycS4YBgaEk6zQlQ2AjMNAgMEwh3VmGQeAwJDN+1VlB5+M4CqCskdzTELo5tIujvJxNuvUdWEE3qpo1EAHspPsApRsJBfFQDFoqhq+qwgA) (temporario)


[transformador]: https://produto.mercadolivre.com.br/MLB-1251924135-transformador-trafo-99v-200ma-bivolt-eletronica-_JM#reco_item_pos=2&reco_backend=machinalis-seller-items-pdp&reco_backend_type=low_level&reco_client=vip-seller_items-above&reco_id=559de26e-339f-4593-91cf-1d10a5a540eb
