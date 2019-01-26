Do PHP ao Python

Um pouco da minha experiência na transição de PHP para Python

---

!['imagem ilustrativa'](https://adlermedrado.com.br/wp-content/uploads/2018/12/ad10e-1wlukTN8cgk7n2rJTOKlOkw.jpeg)  

#### Introdução

Em Dezembro de 2015 eu fui convidado a trabalhar no Olist e eu comecei a trabalhar efetivamente em Janeiro de 2016.  
A situação naquela época está mais ou menos descrita [neste post](https://engineering.olist.com/o-estado-da-tecnologia-na-olist-da18af46b284) do [osantana](https://medium.com/u/e8348ef249f3).

Era um desafio pois a equipe estava bem restrita, a plataforma era monolítica e de difícil manutenção, somado a isso o negócio do Olist em si não é algo trivial.

Quando comecei a trabalhar aqui existia uma versão da plataforma que posteriormente ficou conhecida como V1, era desenvolvida com PHP e tecnologias relacionadas.

O meu objetivo na época era o de manter e evoluir a plataforma num ritmo condizente com o crescimento da empresa. Um super desafio.

Posteriormente houve uma migração da nossa plataforma, denominada de V2 e escrita em Python.

Assim chegou a hora de eu me aventurar nessa linguagem e ecossistema.

O objetivo deste artigo é o de relatar como está sendo a minha experiência com o aprendizado do Python, relatos a respeito da migração podem e/ou poderão ser encontrados em outros artigos.

Não é meu objetivo fazer comparações entre as duas linguagens, sei que é difícil em posts deste tipo, mas meu objetivo não é gerar nenhum *flame war,* e sim, relatar como está sendo a experiência desse aprendizado.

#### O Começo

De Janeiro a Setembro de 2016 foi um período bem difícil, o sistema era instável, cada deploy era motivo de desespero (era proibido deploy às sextas-feiras), qualquer alteração no sistema poderia comprometer outras partes da plataforma, então, realmente não era um projeto fácil de ser mantido.

Aapesar de conhecer muito bem PHP e as tecnologias utilizadas no projeto confesso que não era algo trivial para mim, ajudar a mantê-lo.

#### Alguém falou em reescrever a plataforma? Em outra linguagem?

Me lembro da primeira vez em que escutei a sugestão de migrar a plataforma toda para Python.

!['imagem ilustrativa'](https://adlermedrado.com.br/wp-content/uploads/2018/12/11136-1nBXUASc3psXotxXkFl7HiQ.jpeg)  
_Fiquei mais ou menos assim na hora (sem os machucados)_

Isso foi pouco tempo depois de eu entrar na empresa e a minha reação foi a de: “*Putz, mal comecei na empresa e serei demitido*”, então na primeira oportunidade conversei com o [osantana](https://medium.com/u/e8348ef249f3), que me tranquilizou dizendo que isso não aconteceria, mas, da mesma forma como um *trigger* é disparado, eu comecei a ter em minha mente a idéia de que **precisaria aprender Python *ASAP***.

Mas o tempo foi passando e por motivos diversos eu comecei a estudar Python de verdade praticamente horas antes de *virarem a chave* da V1 para a V2, e me lembro da primeira reação que eu tive: “*Estou gostando desse negócio*”.

#### Vamos lá

Eu estou aprendendo Python da maneira que eu mais gosto de aprender as coisas: **Praticando**.

Nunca vou esquecer uma vez quando conversava com o Osvaldo sobre a estratégia para eu começar a trabalhar na V2, foi algo mais ou menos assim:

> “Imagina que um trem está passando e você se encontra do lado do trilho, quando o trem passar você se pendurará no trem e vai se virar para seguir a viagem com segurança”.

Como eu sou um cara que gosta de desafios, eu fiquei bem empolgado para começar.

#### A primeira vez a gente nunca esquece

A minha primeira tarefa na V2 foi no dia da virada e era a de criar um importador para os códigos de rastreios dos correios (esses códigos são usados para rastrear as encomendas no site dos correios).

*Me senti em casa* pois trabalhei bastante com essa integração com os correios na V1.

Pode ter sido algo psicológico, mas me ajudou a manter a tranquilidade no momento, afinal de contas eu não sabia nada de Python e estava trabalhando em algo importante para o funcionamento da plataforma.

A primeira coisa que eu precisei fazer foi configurar o ambiente, nesse momento eu percebi como as coisas em Python são feitas para serem simples, pois, basicamente eu precisei:

-   Instalar o PostgreSQL localmente (apt-get install …)
-   Configurar o Pyenv (git clone … )
-   Baixar os fontes do projeto (*git clone …)*
-   Criar um Virtualenv
-   Instalar Dependências (*pip install -r requirements.txt)*
-   Configurar o Heroku
-   Pronto

Eu precisei de poucos minutos para fazer isso, contando os downloads e etc., sem a necessidade de usar VM, Docker, nada.

Em princípio não era algo muito diferente do que faria com PHP, com exceção do pyenv, de qualquer forma outras características da plataforma me chamaram a atenção: O pip é muito mais rápido e simples de usar que o [composer](https://getcomposer.org/), o arquivo de configuração do pip é um arquivo *plain text,* o do composer é um JSON, e neste último aspecto, para mim é algo positivo pois eu detesto arquivos de configuração em JSON ou YAML.

### Com o projeto rodando localmente, vamos programar…

Tentando ser sucinto, vamos ao que interessa:

#### Estrutura de classes

A primeira coisa que eu precisei criar foi uma classe de *Model* para lidar com os códigos de rastreio, perguntei ao meu colega: — Onde crio os models? e a resposta: No models.py

!['imagem ilustrativa'](https://media.giphy.com/media/3o6wraGHbaesVVebjW/giphy.gif)  

Veja bem, eu trabalhei algum tempo com Java e muito tempo com PHP, e nessas linguagens, como em todas as outras existem convenções, logo eu estava acostumado a cada classe possuir um arquivo respectivo, por exemplo, em PHP as seguintes classes teriam um arquivo para cada:

> Pai.php

> `class Pai { ... }`

> Filho.php

> `class Filho extends Pai { ... }`

Em Python, seria um único arquivo com as classes declaradas nele:

> models.py

> `class Pai: pass`

> `class Filho(Pai): pass`

Na época eu perguntei a alguns amigos se isso é de fato uma regra em Python, e a resposta que tive foi a de que é possível também criar um arquivo para cada model, mas logo percebi que isso não é comum, da mesma forma que definir várias classes em um único arquivo em PHP também é possível, mas também não é comum.

Hoje familiarizado com esta convenção eu me sinto bem confortável, no final acho que a quantidade bem menor de arquivos acaba facilitando meu dia a dia.

#### Herança múltipla, traits e mixins

Python permite herança múltipla e PHP não, no entanto no PHP existem as traits que permitem fazer algo parecido, mas Python além da herança múltipla também possui mixins, ou seja, para quem não está acostumado, no começo é um pouco confuso entender os códigos pois a herança múltipla + mixins acabam complicando um pouco para quem não está acostumado.

A minha opinião é a de que tanto em PHP como em Python, o uso de mixins/traits acabam deixando o código mais complexo do que deveria e eu particularmente evito usá-los.

#### Debugging

O processo de debug em PHP para mim sempre foi simples, só que agora admito que ele deveria ser ainda mais simples.

Instalar o XDebug e configurar IDE/Editor/Whatever sempre foi uma tarefa trabalhosa, nem difícil e nem chata, mas trabalhosa, acho que por isso que a maioria dos desenvolvedores PHP que conheço ainda usam o artifício do

`print_r();die();`

no dia-a-dia, obviamente muitos debugam com PHP e XDebug, mas não são a maioria.

Em Python esse processo é inerente à linguagem, basta incluir em seu código:

`import pdb; pdb.set_trace()`

ou se tiver o *ipdb* em seu arquivo de dependências:

`import ipdb; ipdb.set_trace()`

E pronto, você já tem um break point na sua aplicação e pode debuga-la a vontade pelo terminal. Se quiser debugar pela sua IDE/Editor/Whatever também é bem simples, na verdade nunca precisei de nenhum passo adicional de instalação de dependências.

Fato é que hoje, desenvolvendo com Python eu não abro mão de um debugger, fiquei mal-acostumado.

#### Objetos

Em Python tudo é um objeto, então é possível sobrecarregar inclusive operadores matemáticos (*ex: +*) o que torna a linguagem bastante poderosa e flexível.

Uma coisa que eu senti diferença, é que em Python não há a necessidade de usar modificadores de acesso *public, protected e private*, na verdade essas palavras-chave não existem no Python, logo, na prática todos os métodos e propriedades são públicos, o que existe é uma convenção ([PEP-8](https://www.python.org/dev/peps/pep-0008/)) que determina que propriedades e métodos que iniciam com \_ ou \_\_ são *protected* e *private*, deixando a cargo dos desenvolvedores a responsabilidade de lidar com isso. Atualmente PHP oferece suporte aos modificadores de acesso, mas até o PHP 4 era comum seguirmos uma convenção parecida.

Para mim, não é problema.

#### Unicode

Python 3 já suporta Unicode por padrão, sem a necessidade de usar libs ou funções específicas para lidar com encoding.

Isso facilita bastante o trabalho porque nós temos certeza que uma função que transforma uma string, não retornará uma string com a codificação diferente da string original.

#### List Comprehensions

Este é um dos recursos que eu estou achando mais legal, pois permite criar e manipular listas (em PHP, o que mais se aproxima de uma lista do Python são os arrays com índices numéricos) de uma maneira simples e natural.

Eu tentarei ilustrar de uma forma bem simples porque esse recurso é muito legal.

Temos o seguinte código:

```
>>> resultados = []
>>> for x in range(20):
… resultados.append(x+x)
>>> resultados
[0, 2, 4, 6, 8, 10, 12, 14, 16, 18, 20, 22, 24, 26, 28, 30, 32, 34, 36, 38]
```

Esse código percorre um loop de 20 elementos, começando em 0 (zero) e armazena o resultado da soma do número por ele mesmo como um elemento da lista.

Usando list comprehensions, esse código pode ser reduzido a uma única linha:

```
resultados = [x+x for x in range(20)]
>>> resultados
[0, 2, 4, 6, 8, 10, 12, 14, 16, 18, 20, 22, 24, 26, 28, 30, 32, 34, 36, 38]
```

Legal né?

#### Shell Interativo

Outro recurso que ajuda bastante no dia-a-dia, é o shell interativo do Python (REPL), basta digitar *python* no terminal e pode-se testar código rapidamente.

O exemplo de list comprehensions eu fiz usando o shell interativo e hoje em dia, eu uso ele até quando preciso fazer alguma conta, dispensando o uso de uma calculadora. Me disseram que isso é coisa de quem está virando um *pythonista*.

!['imagem ilustrativa'](https://adlermedrado.com.br/wp-content/uploads/2018/12/f3662-1aZPMRPtyHWVHUfHJGBJlnw.png)  

_Exemplo de uso do Shell Interativo do Python_

#### Documentação

Buscar por documentação de funcionalidades no site <http://php.net> é mais simples que nos docs do Python, entretanto a documentação que consta no <https://docs.python.org> é completa e com bastante exemplos também.

Hoje, php.net e python.org estão ambos nas pontas dos meus dedos.

#### Código Pythonico

No início quando eu observava o pessoal conversando e falando sobre “Isso não está Pythonico” ou “Isso é legal, está Pythonico” eu me perguntava mas do que será que eles estão falando? *Esses devs python são esquisitos*.

No entanto depois de algum tempo eu cheguei à conclusão de que um código pythonico é o código idiomático, coeso, simples, legível e organizado e que isso faz parte da filosofia da linguagem.

Depois que entendi isso eu acho que estou aprendendo a escrever código melhor, até mesmo em outras linguagens.

O Zen do Python ajudou na minha compreensão:

***The Zen of Python, by Tim Peters***

-   Beautiful is better than ugly.
-   Explicit is better than implicit.
-   Simple is better than complex.
-   Complex is better than complicated.
-   Flat is better than nested.
-   Sparse is better than dense.
-   Readability counts.
-   Special cases aren’t special enough to break the rules.
-   Although practicality beats purity.
-   Errors should never pass silently.
-   Unless explicitly silenced.
-   In the face of ambiguity, refuse the temptation to guess.
-   There should be one — and preferably only one — obvious way to do it.
-   Although that way may not be obvious at first unless you’re Dutch.
-   Now is better than never.
-   Although never is often better than “right” now.
-   If the implementation is hard to explain, it’s a bad idea.
-   If the implementation is easy to explain, it may be a good idea.
-   Namespaces are one honking great idea — let’s do more of those!

No final, conciliar um código Pythonico com [S.O.L.I.D](https://en.wikipedia.org/wiki/SOLID_%28object-oriented_design%29) deixa ele bem legal :)

#### Framework

Nós usamos o Django Framework no Olist, portanto este é o único framework Python ao qual eu tivesse acesso até o momento.

Gostei do slogan:

> Django: The Web framework for perfectionists with deadlines

O Django é um framework bem simples de usar, basicamente basta instalar e começar.

Eu gosto da abordagem dele e infelizmente nesse ponto eu não consigo encontrar nenhum framework PHP que seja tão simples e intuitivo como ele.

Os frameworks PHP que eu já usei (Zend Framework, Symfony, Zend Expressive, entre outros (não, eu não usei o Laravel)), são muito baseados em configurações e mais configurações, o que particularmente eu não sou muito fã.

> Quero deixar claro: Não estou falando que os frameworks PHP são ruins, pelo contrário, são muito bons, principalmente o Zend Expressive, no entanto, para mim eles sofrem com configuração em excesso.

#### Annotations

Python tem suporte a annotations no core da linguagem, eu era acostumado com isso em Java e em PHP eu sempre usei com aquela sensação de estar fazendo gambiarra, usando docblocks para anotar meus objetos.

No final tudo funciona, mas eu gostei disso pois é recurso da linguagem, não de biblioteca externa.

### Considerações Finais

Sou fã de PHP e sempre serei, mas estou curtindo bastante essa fase *Python* na minha carreira.

Cada linguagem possui suas características, prós e contras e acho que é possível usar das boas práticas que conheço (e estou aprendendo, no caso de Python) em ambas plataformas.

A minha sorte também pode ser a de estar trabalhando em uma equipe de altíssimo nível, então conviver com esse pessoal, para mim, é um privilégio enorme.

Quando comecei trabalhar no Olist eu era um Desenvolvedor Sênior PHP e agora eu me sinto um Desenvolvedor Júnior Python.

E estou achando isso muito legal.

_Publicado Originalmente em 2018-06-08_

Tags: php, python 
