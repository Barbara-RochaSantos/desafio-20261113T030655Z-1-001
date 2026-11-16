# Declaração de Uso de Inteligência Artificial

## 1. Ferramenta e Modelo Utilizados

* **Ferramenta:** Google Gemini (interface web)
* **Modelo:** Gemini 4 Hydrogen
* **Link da conversa:** [Tutorando Iniciante em Python](https://gemini.google.com/app/43c6392951593a72)

---

## 2. Divisão de Responsabilidades

### O que a IA fez
* Atuou exclusivamente como **tutora técnica**, sem fornecer o código pronto da solução.
* Estruturou o plano de ação inicial e traduziu conceitos de Java/JUnit para o ecossistema Python/pytest.
* Explicou o funcionamento de tipos nativos (`Decimal`, `date`, `timedelta`) e armadilhas de precisão binária com `float`.
* Orientou sobre atalhos de navegação no VS Code e leitura de *tracebacks* no `pytest` (`-x --tb=short`).

### O que eu fiz
* Configurei o ambiente virtual (`.venv`) e instalei as dependências do projeto.
* Executei a suíte de testes e naveguei pelos arquivos de código-fonte e testes.
* Investiguei as causas raiz dos bugs, implementei todas as correções e validei os resultados finais.
* Estruturei o histórico de commits no Git, criei os artefatos de documentação e publiquei o repositório no GitHub.

---

## 3. Histórico Completo da Interação

<details>
<summary><strong>Clique para expandir a transcrição original (sem alterações)</strong></summary>

```text
Tutorando Iniciante em Python
[https://gemini.google.com/app/43c6392951593a72](https://gemini.google.com/app/43c6392951593a72)

User prompt: Preciso de ajuda em uma determinada atividade. Neste caso, quero que me guie pela execução atuando como um tutor. Não quero que apenas me dê código pronto, você pode mostrar códigos porém não quero que de forma indiscriminada sem que eu possa entender o que está sendo feito. Quero que parta da premissa que sou iniciante, tendo noções de fundamentos de programação e vivência breve em python o que significa que você terá que me explicas as estruturas utilizadas. A atividade é a seguinte enviada pelas imagens. No campo de testes, estou aprendendo testes em Java, então pode ser que eu consiga associar alguma coisa. Roteirize a atividade, explique - a, prioritariamente você é um dev senior me explicando a tarefa como estagiário.
Response: Bem-vindo ao time! Senta aí, pega um café, porque essa tarefa que caiu na nossa mão é um clássico do dia a dia de engenharia de software: manutenção e correção de bugs em código legado com suporte de testes automatizados.
Como você já mexe com testes em Java (provavelmente JUnit), você já tem meio caminho andado. A lógica de testar entradas e saídas é exatamente a mesma, só muda a sintaxe.
Vou quebrar essa demanda em um roteiro de trabalho para você não se perder, explicar o contexto do projeto e te dar o primeiro passo prático.
1. O Contexto do Problema (O que estamos resolvendo?)
Temos um CLI (Command Line Interface — um programa que roda direto no terminal, sem interface gráfica) feito em Python.
Entrada: Dois arquivos CSV — um com reservas de hotéis (reservas_hoteis.csv) e outro com taxas de câmbio (taxas_cambio.csv).
Processamento: O programa deve ler esses dados, converter moedas para Real (BRL), filtrar o que realmente foi faturado, calcular diárias e gerar rankings.
Saída: Um arquivo report.json consolidado.
O problema: O sistema já existe, mas está cheio de bugs nas regras de negócio e entrega números errados. Nosso trabalho é investigar, consertar o código sem quebrar o contrato de entrada/saída, e fazer a suíte de testes ficar 100% verde.
2. Roteiro de Execução (Seu Plano de Batalha)
Não tente ler todos os arquivos de uma vez para sair alterando código. Vamos trabalhar por etapas:
Setup do Ambiente (Preparar a bancada): Criar o ambiente virtual, instalar as dependências e rodar o projeto pela primeira vez.
Rodar os Testes (O Diagnóstico): Executar o pytest. No Java você usa JUnit; aqui usamos o pytest. Ele vai cuspir vários erros — e cada teste falhando é uma pista exata de um bug.
Mapeamento das Regras de Negócio: Ler o código em src/onfly_report/ comparando linha a linha com as regras do README (ex: uso de Decimal em vez de float, filtro de status CONFIRMED, cálculo de noites, ordenação).
Correção Incremental (Um bug por vez): Consertar um problema, rodar o pytest e ver o número de falhas diminuir.
Validação na Amostra: Rodar o CLI usando o arquivo exemplo-validacao.csv e conferir se o JSON gerado bate exatamente com a tabela de referência da documentação.
Documentação e Entrega: Preencher o entrega/README-ENTREGA.md (e a pasta uso-de-ia/, já que você está usando um assistente para te tutorar — fique atento a essa regra na seção Entrega!).
3. Traduzindo os Conceitos (Java → Python)
Para você não estranhar o ambiente:
venv (Ambiente Virtual): No Java você tem o Maven/Gradle gerenciando dependências por projeto. No Python, se você instalar bibliotecas direto no sistema, vira uma bagunça. O venv cria uma "caixinha isolada" na pasta do projeto para instalar só o que precisamos.
requirements-dev.txt: É o equivalente simplificado do seu pom.xml ou build.gradle.
PYTHONPATH=src: Diz ao Python onde procurar o código-fonte raiz do projeto na hora de executar.
pytest -q: O -q significa quiet (silencioso). Ele oculta textos longos e mostra de forma limpa quais testes passaram (.) e quais falharam (F).
Seu Primeiro Passo (Mão na Massa)
Abra o terminal na pasta raiz do projeto e execute os comandos da etapa de setup (se você estiver no Windows, o comando de ativar o ambiente muda um pouquinho — me avise se for o caso):
python3.11 -m venv .venv
source .venv/bin/activate
pip install -r requirements-dev.txt


Em seguida, rode a suíte de testes para vermos o tamanho do estrago:
pytest -q


Sua missão agora: Rode esses comandos e me mande aqui o resultado do pytest -q junto com a lista de arquivos que existem dentro da pasta src/onfly_report/ e tests/. A partir daí, vamos analisar o primeiro bug juntos!

User prompt: Ok. Já rodei a suite de testes e estou vendo 5 erros. A estrutura do projeto tá bem diferente do que costumo lidar na faculdade (bem mais complexa), e além disso eu usei o vs code quando estava vendo JavaScript. No Python eu utilizei o jupyter notebook, mais voltado para dados não para desenvolvimento, até mesmo essa estrutura que eu percebo de orientação a objetos em python pra mim é nova embora já soubesse da possibilidade. Estou meio perdida nos arquivos de projeto e um pouco na ide também. Faz algum sentido fazer casting pra float, depois pra string e depois pra decimal? porque pelo que eu suponho, vai dar erro de precisão nisso, estou correta?
Response: Excelente instinto! Você já matou um dos bugs clássicos do projeto de primeira.
Antes de responder à sua dúvida técnica (que está certíssima), deixa eu te acalmar sobre o VS Code e a estrutura do projeto.
1. Sobrevivendo ao VS Code e ao Projeto Profissional
Na faculdade ou no Jupyter, tudo costuma ficar num arquivo só. No mundo real, dividimos o código em pastas e arquivos menores para cada um ter uma responsabilidade única.
No VS Code, você só precisa de três atalhos/ferramentas agora:
Explorer (barra lateral esquerda): Para navegar entre src/onfly_report/ (onde fica o código que você vai editar) e tests/ (onde ficam os testes — só leitura).
Terminal integrado (Ctrl + '): Onde você roda o pytest -q.
Busca global (Ctrl + Shift + F): Se você vir o nome de uma função ou variável e não souber onde ela foi criada, aperte isso e digite o nome. O VS Code acha na hora.
2. Sobre o Casting (float → str → Decimal)
Você está 100% correta na sua suposição! Não faz o menor sentido e causa erro de precisão.
Olha o porquê: O computador guarda números float em formato binário (base 2), e alguns números decimais simples (como 0.1 ou 0.2) viram dízimas periódicas em binário.
Se você fizer float("0.1"), na memória do computador ele já virou algo como 0.1000000000000000055511151231257827021181583404541015625.
Quando você converte esse float sujo para str e depois para Decimal, você está congelando o lixo de precisão dentro do Decimal. O estrago já foi feito na primeira conversão!
Como deve ser feito: Quando lemos um CSV, tudo vem originalmente como str (texto), por exemplo "150.50". Você deve converter direto de str para Decimal:
from decimal import Decimal

valor = Decimal("150.50") # Limpo, exato, sem passar por float!


Isso bate exatamente com a regra de negócio na seção Exatidão do README: "Use Decimal, nunca float, no dinheiro."
3. Nosso Próximo Passo
Para eu conseguir te guiar sem te dar respostas prontas e te explicando as estruturas de Python que aparecerem (classes, funções, dicionários, etc.):
Liste aqui os nomes dos arquivos que estão dentro de src/onfly_report/ e dentro de tests/.
Cole aqui a saída do erro do primeiro teste que falhou no pytest (pode rodar pytest -k "nome_do_primeiro_teste" ou só pytest sem o -q para ver o detalhe da primeira falha).
Assim que você me mandar os nomes dos arquivos e o primeiro erro, vamos abrir o arquivo correspondente e destrinchar a sintaxe juntos!

User prompt: Eu corrigi este erro e um de lógica que estava neste erro, dois testes passaram dos 5. é normal?
Response: Sim, é totalmente normal — e é um ótimo sinal! Você acabou de matar 40% dos problemas de uma vez.
Na engenharia de software, isso acontece o tempo todo porque os testes validam comportamentos diferentes que dependem da mesma base. Por exemplo:
Um teste pode estar verificando especificamente se os valores financeiros não têm erro de centavos (precisão do Decimal).
Outro teste pode estar validando o relatório final completo (como a amostra de validação), que só passou agora porque o cálculo monetário estava impedindo o total geral de bater.
Quando você conserta a fundação (como a conversão de moeda ou um filtro básico), vários testes que dependiam daquela peça passam em efeito dominó.
E agora, vamos para os 3 que faltam! Como você está se adaptando ao código em Python e à estrutura do projeto, me mande aqui o output do pytest mostrando apenas os 3 erros restantes (ou o log do primeiro deles).
Se você também tiver alguma dúvida sobre alguma sintaxe específica de Python que viu nesses arquivos (como dataclass, def, list comprehensions ou dicionários), pode colar o trecho aqui que eu traduzo para o "mental model" do Java para você!

User prompt: Eu gostaria de saber se é tão complicado identificar qual arquivo está o erro no pytest desse jeito. Eu estou indo arquivo por arquivo para tentar localizar alguma coisa relacionada do teste, tem alguma outra forma ou é mais ou menos isso mesmo?
Response: Não precisa sair caçando arquivo por arquivo no escuro, não! No dia a dia, se a gente fizer isso em projetos grandes, não entrega nada. O próprio pytest te entrega o mapa da mina se você souber ler o traceback (rastreamento de pilha) e usar as flags certas.
No Java (JUnit), quando um teste falha, você olha o Stack Trace para ver qual classe e linha lançaram o AssertionError. No Python é a mesma coisa, só que de cabeça para baixo.
1. Como fazer o pytest te dar o caminho exato
Em vez de rodar pytest -q (que esconde os detalhes porque -q significa quiet), rode este comando no terminal:
pytest -x --tb=short


-x (exitfirst): Faz o pytest parar imediatamente no primeiro erro que encontrar. Assim seu terminal não fica poluído com 3 falhas gigantes ao mesmo tempo — você foca em um bug por vez.
--tb=short (traceback short): Mostra o caminho exato dos arquivos e o número das linhas sem encher a tela de código desnecessário.
2. Como ler a saída (O "Mapa da Mina")
Quando você rodar isso, o Python vai te mostrar um bloco parecido com este exemplo:
tests/test_report.py:42: in test_calculo_noites
    assert resultado["total_room_nights"] == 28
E   AssertionError: assert 32 == 28


Leia exatamente nesta ordem:
A primeira linha (tests/test_report.py:42): Diz qual arquivo de teste falhou e em qual linha.
O nome da função (test_calculo_noites): Diz qual regra de negócio quebrou.
A linha com E AssertionError: Mostra o que o código gerou (32) versus o que o teste esperava (28).
3. Como achar o arquivo do código (src/) rapidinho no VS Code
Agora que você viu que falhou test_calculo_noites:
Abra o arquivo tests/test_report.py na linha 42 (você pode segurar Ctrl no Windows/Linux ou Cmd no Mac e clicar direto no caminho tests/test_report.py:42 dentro do terminal do VS Code que ele abre o arquivo na linha exata!).
Veja qual função do projeto o teste está chamando (ex: generate_report(...) ou calculate_nights(...)).
Com o cursor em cima do nome dessa função no VS Code, aperte F12 (ou clique com o botão direito → Go to Definition). O VS Code pula direto para o arquivo dentro de src/onfly_report/ onde aquela função foi escrita.
Roda aí o pytest -x --tb=short e cola aqui o resultado desse próximo erro que apareceu! Vamos usar esse "mapa" para ir direto ao ponto.

User prompt: operações com objetos do tipo data em python retorna que tipo de objeto?
Response: Quando você subtrai duas datas em Python (data_fim - data_inicio), o resultado é um objeto do tipo timedelta (do módulo datetime), que representa uma duração (um intervalo de tempo), e não uma data ou um número inteiro puro.
No Java (java.time), isso equivale à diferença entre um LocalDate (a data no calendário) e um Period ou Duration (o intervalo entre duas datas).
Como extrair os dias desse objeto
Como o timedelta é um objeto complexo (que guarda dias, segundos e microssegundos), você não pode multiplicá-lo diretamente pela quantidade de quartos (rooms) sem extrair o número inteiro antes. Para pegar apenas a quantidade inteira de dias, você acessa o atributo .days:
from datetime import date

checkin = date(2025, 1, 10)
checkout = date(2025, 1, 13)

diferenca = checkout - checkin  # Retorna: datetime.timedelta(days=3)
dias = diferenca.days           # Retorna: 3 (tipo int)


Isso conecta direto com a fórmula que está na seção Noites da especificação:
room_nights = (checkout_date - checkin_date).days * rooms


Dica de Sênior para ficar de olho no código: Se o código original estiver tentando somar + 1 nessa diferença (tornando-a inclusiva, como se check-in dia 10 e check-out dia 13 fossem 4 noites em vez de 3), ou se estiver subtraindo na ordem invertida (checkin - checkout, o que daria um número negativo), você já sabe onde está o bug!
Encontrou algo parecido com isso no cálculo de noites aí no código?

User prompt: quais atributos tem o objeto timedelta?
Response: Internamente, o objeto timedelta guarda apenas três atributos (todos do tipo inteiro, int), porque qualquer intervalo de tempo no Python é normalizado nessas três unidades:
.days: Quantidade de dias inteiros (pode ser positivo ou negativo).
.seconds: Quantidade de segundos restantes dentro de um dia (vai de 0 até 86399, que é 23h59m59s).
.microseconds: Quantidade de microssegundos restantes dentro de um segundo (vai de 0 até 999999).
⚠️ Pegadinha clássica de iniciante (e de código bugado!)
Repare que não existem os atributos .hours ou .minutes no timedelta. Se você tentar acessar diferenca.hours, o Python lança um AttributeError.
Outra pegadinha comum é confundir .seconds com .total_seconds():
.seconds (atributo): Olha só o que sobrou além dos dias inteiros. Se a diferença for de 2 dias exatos, diferenca.days vale 2 e diferenca.seconds vale 0!
.total_seconds() (método — tem parênteses): Converte a duração inteira (dias + segundos + microssegundos) para segundos totais (retorna um float). No exemplo de 2 dias exatos, diferenca.total_seconds() retorna 172800.0.
No caso do nosso projeto de reservas de hotel, como check-in e check-out são datas civis (date, no formato YYYY-MM-DD, sem horas), os atributos .seconds e .microseconds serão sempre 0, e você só vai precisar do .days.
Você está investigando algum bug específico no cálculo das datas ou das noites agora?