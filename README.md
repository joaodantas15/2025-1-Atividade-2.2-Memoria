# S.O. 2025.1 - Atividade 2.02 - Gestão de memória

## Informações gerais

- **Objetivo do repositório**: Repositório para atividade avaliativa dos alunos
- **Assunto**: Gestão de memória
- **Público alvo**: alunos da disciplina de SO (Sistemas Operacionais) do curso de TADS (Superior em Tecnologia em Análise e Desenvolvimento de Sistemas) no CNAT-IFRN (Instituto Federal de Educação, Ciência e Tecnologia do Rio Grande do Norte - Campus Natal-Central).
- disciplina: **SO** [Sistemas Operacionais](https://github.com/sistemas-operacionais/)
- professor: [Leonardo A. Minora](https://github.com/leonardo-minora)
- Repositótio do aluno: https://github.com/joaodantas15/2025-1-Atividade-2.2-Memoria
- Aluno: João Pedro Dantas Magalhães

## Tarefas do aluno
1. Fork desse repositório e atualizar a linha 10 com o nome e link do github
2. Ler a descrição da atividade
3. Montar a resposta no final deste arqivo, no tópico **Resposta**

---

## 1. Descrição da atividade
### 1.1. Objetivo
Praticar os conceitos de alocação de memória (best-fit), memória virtual e desfragmentação em um sistema com memória limitada.

---

### 1.2. Contexto
Um computador possui apenas **64 KB de RAM** e um **disco rígido para memória virtual**. O sistema operacional deve gerenciar 5 processos com tamanhos diferentes, cuja soma ultrapassa a capacidade da RAM.

#### 1.2.1. Processos iniciais

| Processo | Tamanho (KB) |
|----------|-------------|
| P1       | 20          |
| P2       | 15          |
| P3       | 25          |
| P4       | 10          |
| P5       | 18          |
| **Total**| **88 KB**   |

- **Memória RAM**: 64 KB (contígua, inicialmente vazia).  
- **Memória Virtual (Disco)**: Espaço ilimitado para paginação.

#### 1.2.2. Alocação Inicial com Best-Fit
Os alunos devem simular a alocação dos processos na RAM usando o algoritmo **best-fit**.  
- A memória RAM será representada como um bloco contíguo (ex: `[0KB - 64KB]`).  
- Devem alocar os processos nos menores espaços livres que atendam ao seu tamanho.  

**Alocação inicial**:  
1. P1 (20 KB) → Ocupa [0-20].  
2. P2 (15 KB) → Ocupa [20-15].  
3. _continuar a partir daqui_

#### 1.2.3. Simular Memória Virtual (Paginação)
- Os processos não alocados na RAM devem ser "paginados" no disco.  
- Criar uma tabela de páginas indicando quais partes estão na RAM e quais estão no disco.  

#### 1.2.4. Desfragmentação da RAM
- Desfragmentar a RAM para liberar espaço contíguo.
- Após desfragmentação (compactação), verificar quais processos podem ser alocado.  

### 1.3. Questões para Reflexão
1. Best-fit foi mais eficiente que first-fit ou worst-fit neste cenário?  
2. Como a memória virtual evitou um deadlock?  
3. Qual o impacto da desfragmentação no desempenho do sistema?  

---

## Resposta

### 1. Alocação Inicial com Best-Fit

O algoritmo de alocação Best-Fit (Melhor Encaixe) examina todos os blocos de memória livres e aloca o processo no menor bloco que seja grande o suficiente para contê-lo. A simulação abaixo descreve a alocação sequencial dos processos na memória RAM de 64 KB.

Cenário:

RAM: 64 KB

Processos: P1(20KB), P2(15KB), P3(25KB), P4(10KB), P5(18KB)

Passo a Passo da Alocação:

Estado Inicial: A memória é um único bloco livre.

[ Bloco Livre: 64 KB ]

Alocação de P1 (20 KB):

O único bloco de 64 KB é selecionado.

Estado da RAM:

[ Ocupado por P1: 20 KB ]

[ Bloco Livre: 44 KB ] (64 - 20)

Alocação de P2 (15 KB):

O único bloco livre de 44 KB é selecionado.

Estado da RAM:

[ P1: 20 KB ] [ Ocupado por P2: 15 KB ]

[ Bloco Livre: 29 KB ] (44 - 15)

Alocação de P3 (25 KB):

O único bloco livre de 29 KB é selecionado.

Estado da RAM:

[ P1: 20 KB ] [ P2: 15 KB ] [ Ocupado por P3: 25 KB ]

[ Bloco Livre: 4 KB ] (29 - 25)

Tentativa de Alocação de P4 (10 KB):

O único bloco livre disponível tem 4 KB.

Como 10 KB > 4 KB, a alocação falha. P4 não pode ser carregado na RAM.

Tentativa de Alocação de P5 (18 KB):

O único bloco livre disponível ainda é o de 4 KB.

Como 18 KB > 4 KB, a alocação também falha. P5 não pode ser carregado na RAM.

Resumo da Alocação Inicial:
A memória RAM ficou ocupada pelos três primeiros processos, resultando em um pequeno fragmento de espaço livre.



**Endereço na RAM	**  |  ** Conteúdo	**       |   ** Tamanho**
0 KB - 19 KB	        Processo             P1	20 KB
20 KB - 34 KB       	Processo P2	         15 KB
35 KB - 59 KB       	Processo P3	         25 KB
60 KB - 63           KB	Espaço Livre	     4 KB
Não alocados         	P4, P5               	-


### 2. Simular Memória Virtual (Paginação)
Os processos que não puderam ser alocados na RAM (P4 e P5) são mantidos em uma área de armazenamento secundário (disco rígido) pelo mecanismo de memória virtual. O sistema operacional mantém uma tabela para rastrear a localização de cada processo.

Tabela de página simplificada d sistema:

**Processo	 **           ** Localização **                    ** 	Endereço / Status na Memória**
P1                        	RAM	                               Ocupa o espaço de memória [0-19]
P2                        	RAM	                               Ocupa o espaço de memória [20-34]
P3	                        RAM	                               Ocupa o espaço de memória [35-59]
P4	                        Disco (Virtual)	                   Paginado. Aguardando 10 KB livres
P5	                        Disco (Virtual)	                   Paginado. Aguardando 18 KB livres

### 3. Desfragmentação da RAM

A desfragmentação (ou compactação) reorganiza os blocos de memória ocupados para consolidar os espaços livres em um único bloco contíguo. Para que seu efeito seja visível, simulamos um cenário onde um processo intermediário termina.

Cenário Hipotético: O Processo P2 (15 KB) encerra sua execução.

Estado Fragmentado (Pós-Término de P2):

O espaço de P2 se torna um "buraco" no meio da memória.

Layout: [ P1 (20KB) ] [ LIVRE (15KB) ] [ P3 (25KB) ] [ LIVRE (4KB) ]

Temos um total de 19 KB livres, mas o maior bloco contínuo é de apenas 15 KB, o que ainda não é suficiente para alocar P5 (18 KB).

Execução da Desfragmentação (Compactação):

O Sistema Operacional move o bloco de P3 para junto de P1, unificando os espaços livres.

Layout Resultante: [ P1 (20KB) ] [ P3 (25KB) ] [ LIVRE (19KB) ]

Agora temos um bloco único e contíguo de 19 KB.

Nova Tentativa de Alocação:

O S.O. verifica os processos em memória virtual (P4 de 10 KB e P5 de 18 KB) para alocação no novo espaço de 19 KB.

Ambos cabem. Aplicando a regra Best-Fit:

Se alocar P4, o espaço restante é: 19 - 10 = 9 KB.

Se alocar P5, o espaço restante é: 19 - 18 = 1 KB.

O S.O. escolhe P5, pois ele é o "melhor encaixe", minimizando o espaço livre restante (fragmentação interna).

Estado Final da RAM: P5 é carregado do disco para a RAM.

[ P1 (20KB) ] [ P3 (25KB) ] [ P5 (18KB) ] [ LIVRE (1KB) ]



 ### 4. Questões para Reflexão

** a) Best-fit foi mais eficiente que first-fit ou worst-fit neste cenário?**

No cenário de alocação inicial descrito, não houve diferença de eficiência entre os algoritmos Best-Fit, First-Fit e Worst-Fit. A razão para isso é que a memória estava inicialmente vazia, e a cada etapa de alocação (para P1, P2 e P3) havia sempre um único bloco de memória livre. Como não havia escolha entre diferentes blocos livres, os três algoritmos teriam tomado a mesma decisão, produzindo um layout de memória idêntico. A diferença de eficiência entre eles só se manifesta em um sistema com múltiplos "buracos" de memória de tamanhos variados.

**b) Como a memória virtual evitou um deadlock?**

A memória virtual evitou uma condição de paralisação do sistema por exaustão de recursos. Sem ela, o S.O. teria que rejeitar a execução dos processos P4 e P5, pois não havia RAM disponível. Isso não é um deadlock clássico (onde processos travam uns aos outros em um ciclo de espera), mas sim uma negação de serviço que impede o sistema de progredir com novas tarefas.

Ao utilizar a memória virtual, o sistema pode "paginar" os processos P4 e P5 para o disco, colocando-os em um estado de espera. Isso permite que o sistema continue operacional e produtivo com os processos residentes na RAM. Quando um processo na RAM termina (como P2 em nossa simulação), o S.O. pode então carregar um processo do disco, garantindo um fluxo de trabalho contínuo e uma maior taxa de utilização do processador.

**c) Qual o impacto da desfragmentação no desempenho do sistema?**

A desfragmentação possui um impacto duplo, representando um clássico trade-off (relação de custo-benefício) em sistemas operacionais:

Impacto Positivo (Benefício): O principal benefício é a mitigação da fragmentação externa. Ao consolidar múltiplos espaços livres pequenos em um único bloco grande, a desfragmentação permite que processos maiores, que antes não caberiam em nenhum dos "buracos" individuais, sejam alocados na RAM. Isso melhora a utilização da memória e aumenta o grau de multiprogramação do sistema.

Impacto Negativo (Custo): A desfragmentação é uma operação extremamente custosa em termos de desempenho. Ela exige que o sistema operacional pause a execução, copie grandes quantidades de dados de uma posição de memória para outra e atualize todas as referências de endereço dos processos movidos. Essa tarefa consome um tempo de CPU significativo e utiliza intensamente o barramento de memória, causando uma pausa notável na responsividade do sistema, durante a qual os processos ficam "congelados". Por esse motivo, é uma técnica usada com pouca frequência em sistemas modernos, que preferem mecanismos de paginação que não exigem memória contígua.

**d) Fragmentação Interna vs. Externa**
No nosso exercício, quando alocamos P3 (25 KB) no espaço livre de 29 KB, sobraram 4 KB de espaço entre os blocos. Chamamos isso de fragmentação externa. Agora, imagine um sistema operacional diferente que, para evitar a complexidade de gerenciar blocos de tamanhos variados, divide toda a RAM em "páginas" de tamanho fixo, por exemplo, de 4 KB.

Pergunta: Nesse sistema de páginas fixas, como o Processo P2 (15 KB) seria alocado? Quantas páginas ele ocuparia e haveria algum tipo de desperdício de memória? Que nome se dá a esse tipo de desperdício e como ele se diferencia da fragmentação externa que vimos?

Resposta:

Cálculo da Alocação: Para alocar o Processo P2 de 15 KB, o sistema precisa fornecer páginas de 4 KB até que todo o processo caiba na memória.

15 KB/4 KB por pagina=3.75 paginas

Como não é possível alocar uma fração de página, o sistema deve alocar 4 páginas inteiras.

Cálculo do Desperdício:

O espaço total alocado para o processo é: 4 páginas×4 KB/pagina =16 KB.

O processo, no entanto, só precisa de 15 KB.

O desperdício é a diferença: 16 KB (alocado)−15 KB (usado)=1 KB.

Nome e Diferença: Esse desperdício de 1 KB ocorre dentro da última página alocada para o processo. Ele é chamado de Fragmentação Interna.

Diferença Fundamental:

Fragmentação Externa (o que vimos na simulação): É o espaço livre que existe entre os blocos de memória alocados. São "buracos" que, somados, podem representar muita memória livre, mas que individualmente são pequenos demais para serem aproveitados por novos processos.

Fragmentação Interna (neste novo cenário): É o espaço desperdiçado dentro de um bloco/página de memória alocado. Ocorre porque um processo raramente tem um tamanho que é múltiplo exato do tamanho da página, então a última página alocada fica parcialmente vazia.

**e) Alternativas Modernas à Desfragmentação**
Vimos que a desfragmentação, apesar de eficaz para consolidar a memória, é uma operação muito custosa que causa pausas no sistema. Sistemas operacionais modernos como Windows, Linux e macOS praticamente nunca realizam uma desfragmentação completa da RAM.

Pergunta: Qual é a técnica fundamental que esses sistemas modernos utilizam para gerenciar a memória de forma que a fragmentação externa deixe de ser um problema crítico, eliminando a necessidade de realizar a compactação? Quais as vantagens dessa abordagem em relação ao modelo de alocação contígua que simulamos?

Resposta:

A técnica fundamental que os sistemas operacionais modernos utilizam é a alocação de memória não-contígua, principalmente através da Paginação.

Como Funciona a Paginação: Em vez de exigir que um processo inteiro ocupe um único bloco contínuo na RAM, o processo é dividido em várias "páginas" de tamanho fixo (ex: 4 KB). A memória RAM física também é dividida em "molduras de página" (frames) do mesmo tamanho. O sistema operacional pode então espalhar as páginas de um único processo por molduras de página que não estejam adjacentes na memória. Uma "Tabela de Páginas" para cada processo mantém o mapeamento de onde cada página está localizada fisicamente.

Vantagens sobre a Alocação Contígua:

Eliminação da Fragmentação Externa: Este é o maior benefício. Qualquer moldura de página livre na RAM pode ser usada para alocar uma página de um processo. Não importa mais se os espaços livres estão "juntos" ou "separados". Isso resolve o problema que a desfragmentação tentava corrigir.

Simplificação da Alocação: O gerenciador de memória não precisa mais procurar por um "buraco" de tamanho adequado (como no First-Fit ou Best-Fit). Ele simplesmente mantém uma lista de molduras de página livres e pega a primeira que estiver disponível.

Flexibilidade e Eficiência: Facilita o compartilhamento de memória (ex: bibliotecas do sistema) entre diferentes processos e é a base para uma implementação muito mais eficiente e granular da memória virtual. O sistema pode mover páginas individuais (e não o processo inteiro) entre a RAM e o disco.

Segurança: A Tabela de Páginas, em conjunto com o hardware (MMU), ajuda a isolar os espaços de endereço dos processos, impedindo que um processo acesse a memória de outro indevidamente.
