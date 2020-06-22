---
layout: post
title:  "Como Recebi Pagamentos do Exterior como Pessoa Física"
date:   2020-06-21 22:15:00 -0300
categories: jekyll update
---

Em Fevereiro deste ano, me inscrevi no processo seletivo de uma empresa alemã.
Fui aprovada e a ideia seria mudar pra lá dentro de poucos meses. No começo de
Março, pedi demissão da empresa onde eu trabalhava ao mesmo tempo que a pandemia
se agravou ao redor do mundo (timing bem louco, eu sei). Obviamente, precisei
rever os meu planos.

A alternativa encontrada foi começar a trabalhar remotamente para a nova empresa
e atuar num formato de prestação de serviços (gerando uma fatura comercial
mensalmente).

Eu que até então só tinha atuado em em regime CLT, fiquei bastante confusa. Mas
no final deu tudo certo e acredito que relatar essa experiência aqui pode ser
útil para alguém que se encontre em situação parecida.

Consultei alguns amigos que já trabalham em regime PJ pro exterior, mas no meu
caso, que nem empresa tinha, abrir uma no meio da quarentena não era uma opção
viável, então procurei me informar sobre quais eram as minhas possibilidades
para receber pagamento do exterior como pessoa física.

## Conta virtual e envio de cobrança
Abri uma conta na
[Payoneer](https://www.payoneer.com/raf/?rid=4341FAC8-0423-40AB-9388-F5D1EEE452F6)
(e se você abrir sua conta usando esse mesmo link aí, tanto eu quanto você
ganhamos 25 USD cada um se você passar a receber pagamentos por lá também).
Ali criei uma conta virtual que aceita tanto Dólar quanto Euro. O sistema também
me permite fazer cobranças, bastando para isso anexar o _invoice_ - fatura
comercial, o template dá pra caçar na internet... eu usei um dos vários
modelos disponibilizados pela Microsoft [aqui](https://templates.office.com/en-us/invoices).

Depois de anexar o invoice (contendo os dados bancários da minha conta virtual
da Payoneer), uso a funcionalidade deles para o envio de solicitação de
pagamento para o e-mail do departamento de contabilidade da empresa. Assim que o
pagamento é recebido, a Payoneer me notifica por e-mail e pelo app mobile. Eu
posso optar por manter o dinheiro lá ou também por transferir para uma conta
bancária local. Para transferir, eles cobram uma taxa de até 2% do valor da taxa
de câmbio no momento da transação. Pela praticidade que a plataforma fornece,
achei um preço bem justo. Usei o sistema de transferência com o Banco do Brasil
e com a Nuconta. Zero problemas.

## Carnê Leão
O próximo passo é fazer a declaração dessa renda, dado que ao receber o
pagamento desta forma, os impostos ainda não foram pagos. Ao receber quantias do
exterior, sem imposto direto na fonte, a declaração deve ser feita mensalmente
através do [Carnê Leão](http://receita.economia.gov.br/orientacao/tributaria/pagamentos-e-parcelamentos/pagamento-do-imposto-de-renda-de-pessoa-fisica/carne-leao). Se o valor declarado ultrapassar R$ 1.903,98 será
necessário pagar os impostos sobre ele, que pode variar entre 7,5% e 27,5%
do total.

O [Blog do Edivaldo Brito](https://www.edivaldobrito.com.br) contém instruções
passo a passo da instalação do carnê leão no Linux, recomendo: [Como instalar o Carnê Leão no Linux](https://www.edivaldobrito.com.br/carne-leao-no-linux/)

Para o preenchimento do Carnê Leão, eu sugiro o vídeo [Preencher Carnê Leão com
Salário no Exterior](https://www.youtube.com/watch?v=L5rcZ7xI1oQ) da [Fernanda
Ellis](https://www.fernandaellis.com/).

E detalhando um pouco mais a regra mencionada no vídeo sobre a conversão
aplicada ao valor declarado: caso o valor recebido não esteja em dólares dos
EUA, ele deve primeiramente ser convertido em USD utilizando a [plataforma de
conversão do Banco Central](https://www.bcb.gov.br/conversao) selecionando a
data do recebimento no campo `Data da cotação`. Então, deve ser convertido em
reais, informando desta vez a data do último dia útil da primeira quinzena do
mês anterior ao recebimento. O resultado desta conversão corresponde ao valor a
ser declarado.

Após o preenchimento, podemos gerar o DARF (Documento De Arrecadação de
Receitas Federais), entretanto, a versão gerada pelo software não contém código
de barras, o que impossibilita o seu pagamento por meios digitais. Para gerar o
DARF Online, uso o
[Sicalcweb](http://servicos.receita.fazenda.gov.br/Servicos/sicalcweb/default.asp?TipTributo=1&FormaPagto=1) (Programa para Cálculo e Impressão de Darf On Line) acessando a aba "Pagamento". Lá eu informo os dados a serem
pagos conforme o DARF previamente gerado pelo Carnê Leão e gero a versão que
pode ser paga digitalmente. O comprovante pode ser conferido poucos dias depois
no [Portal e-CAC](https://cav.receita.fazenda.gov.br/).

É isso. Espero ter ajudado, ainda estou aprendendo bastante sobre esse processo
também. :)
