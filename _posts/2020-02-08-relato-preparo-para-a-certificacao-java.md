---
layout: post
title:  "Relato: Preparo para a certificação Java (OCA)"
date:   2020-02-07 22:15:00 -0300
categories: jekyll update
---

No ano passado, decidi tirar a certificação Java (OCA, Java SE 8 Programmer),
um ano ano depois de ter migrado pra essa linguagem (eu era Rubista até então).
Embora atualmente eu tenha contato cotidiano com Java, o preparo para a
certificação tem os seus macetes. Foram cerca de seis semanas estudando bastante
(bastante meeesmo) para este objetivo e achei que seria interessante
compartilhar os pontos mais relevantes aqui.

## Study Guide
O Study Guide da Sybex é a principal referência para quem pretende prestar a
certificação ([link para compra na
Amazon](https://www.amazon.com.br/OCA-Certified-Associate-Programmer-1Z0-808/)).
O conteúdo é excelente, muito didático e cobre todos os tópicos abordados no
exame. É super direcionado, por vezes acaba mencionando um outro item que
não cai no exame, mas que é útil no entendimento de algo que cai. Quando isso
acontece, os autores pontuam isso de forma bem explícita, para garantir que o
seu foco não seja totalmente investido em algo que não faz sentido naquele
momento.

O livro contém alguns simulados, tanto embutidos no livro físico quanto
disponíveis na [plataforma online](https://testbanks.wiley.com/) da Sybex.
Considero-os indispensáveis no preparo para o teste. Conforme recomendação dos
autores, o ideal é estudar bem cada conceito e fazer os simulados quando se
sentir minimamente preparado. Realizar os simulados repetidamente pode levar a
uma decoreba das respostas e uma falsa sensação de confiança. A plataforma online
também inclui mais 3 simulados bônus e um esqueminha de flashcards contendo
termos-chave e definições.

## Prática
Muitos dos conceitos abordados são exemplificados em porções de código que
constituem péssimas práticas. Embora sejam muito distantes do que se recomenda
em uma aplicação real, para o exame, é preciso ter um bom entendimento
do que é ou nao permitido pela linguagem.

Esse foi pra mim, um dos pontos mais difíceis durante os estudos. Praticar os
exemplos, compilando-os e executando-os, fazendo alterações e testando
diferentes cenários, foi fundamental para a minha compreensão desses tópicos.

#### Editor ao invés de IDE
No começo, eu praticava com o IntelliJ, a mesma IDE que uso para o meu dia-a-dia
no trabalho. No entanto, percebi que meu olho ainda estava pouco treinado para
capturar erros de sintaxe. Algumas questões apresentam um código de 20 linhas,
sem indentação e te pedem para identificar se o código compila ou não (e se não
compila, você deve identificar o motivo). Em questões assim, era fácil passar
batido se havia um `throw` onde o esperado era `throws` ou se as chaves não
estavam balanceadas. Troquei o IntelliJ pelo Vim e, pouco depois, identificar
esse tipo de erro se tornou algo bem natural. Ao me forçar a escrever o código
sem ajuda do autocomplete e sem os corretores da IDE, meus olhos se acostumaram
a prestar atenção nessas situações.

#### Jupyter
Na maioria das vezes, eu usava o editor e escrevia um programa inteiro, mas
eventualmente queria verificar algo bem pontual. Pra isso, decidi experimentar o
[IJava](https://github.com/SpencerPark/IJava), que é o kernel Jupyter para a
execução de código Java. Gostei também da possibilidade de ter os outputs de
porções de código registrados em arquivos para consulta de forma organizada sem
precisar executar tudo. Não diria que achei necessário, mas foi bem
interessante. Usei a imagem docker
[java-notebook](https://hub.docker.com/r/jbindinga/java-notebook), então não
precisei configurar/instalar nada muito específico.

## Enthuware
Depois de ler o livro da Sybex inteiro e passar algum tempo alternando entre os
simulados, correções, e prática de exercícios no computador, eu senti falta de
algo mais desafiador (cheguei no ponto em que eu não tinha certeza se estava
indo bem nos simulados porque eu realmente tinha aprendido ou porque tinha
memorizado as questões).

Pesquisando na internet, me deparei com a [enthuware.com](https://enthuware.com/),
uma plataforma que vende diferentes pacotes de simulados. Vi bastante gente
falando bem e comprei o pacote do OCA por 9,95 dólares.

Não me arrependi, valeu muito a pena. O pacote contém 7 simulados e mais de 600
questões. Achei que tem um teor bem mais difícil que os simulados do livro,
embora se baseie no mesmo tópicos. Confesso que não fiz todos, mas fiz o
suficiente para me sentir mais confiante para o dia do exame.

## A Prova
Quando o dia chegou, honestamente, achei que as questões reais não chegavam no
mesmo nível de dificuldade de boa parte das questões do Enthuware. Por conta
disso, senti que ter praticado com os simulados deles foi algo que me deixou bem
confortável.

Também achei que o formato das questões da prova foi bem mais parecido com as
questões do Enthuware do que com as questões do livro da Sybex (arrisco dizer
que uma ou outra questão era realmente idêntica).

Em termos de conceitos, não teve absolutamente nada que caiu no exame que não
estivesse coberto pelo livro. Embora o Enthuware forneça conteúdo teórico
relevante para cada questão dos simulados, não supre (e nem tem a intenção de
suprir) toda a bagagem teórica necessária. Entendo ambos como materiais
complementares.

Logo após a saída da prova, já recebi o e-mail da Oracle me informando que eu
tinha passado e alguns dias depois recebi minha [credencial digital](https://www.youracclaim.com/badges/903d0715-0d2f-481d-8ea4-8c0fe461c1c3/public_url) e o certificado.

Para finalizar, eu recomendaria a quem é estudante na reta final ou
recém-formado e pensa em tirar a certificação, que faça isso o quanto antes. Eu
não tenho dúvidas de que teria tido bem menos dificuldade se tivesse feito a
prova no meu primeiro ano de formada, quando o conteúdo teórico estava bem
mais fresco e eu tinha menos vícios do mercado.
