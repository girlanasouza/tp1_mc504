# Simulador de Escalonamento de Processos
**Prazo de entrega:** 21/09/2026

**Entrega via Classroom**

-----------------------------
## Objetivo

O objetivo deste projeto é implementar um simulador de algoritmos de escalonamento de processos.

O simulador deverá implementar os seguintes algoritmos:

- FCFS (First-Come, First-Served);
- SJF (Shortest-Job-First), sem preempção;
- SRTF (Shortest-Remaining-Time-First), com preempção;
- Round-Robin;
- Prioridade, sem preempção;
- Prioridade, com preempção;
- Prioridade com Round-Robin para processos de mesma prioridade.

O simulador recebe uma lista de processos e um algoritmo de escalonamento, simula a execução dos processos e produz informações que podem ser utilizadas para construir um diagrama de Gantt, além do tempo médio de espera.

## Entrada

A lista de processos deve ser fornecida em um arquivo CSV.

Cada linha deve conter:

```text
nome,burst,chegada,prioridade
````

onde:

* `nome` é o identificador do processo;
* `burst` é a duração do burst de CPU do processo;
* `chegada` é o instante em que o processo chega ao sistema;
* `prioridade` é a prioridade do processo.

Os tempos são expressos em unidades de tempo (u.t.). As unidades não devem ser escritas no arquivo.

Exemplo:

```csv
nome,burst,chegada,prioridade
P1,8,0,2
P2,4,1,1
P3,2,2,3
P4,5,3,2
```

## Execução

O programa deve ser executado passando como parâmetros:

1. o nome do arquivo CSV;
2. o algoritmo de escalonamento;
3. o quantum, quando necessário.

Exemplos:

```bash
python scheduler.py processos.csv fcfs
```

```bash
python scheduler.py processos.csv sjf
```

```bash
python scheduler.py processos.csv srtf
```

```bash
python scheduler.py processos.csv rr 3
```

```bash
python scheduler.py processos.csv priority
```

```bash
python scheduler.py processos.csv priority-preemptive
```

```bash
python scheduler.py processos.csv priority-rr 3
```

Os nomes utilizados para os algoritmos devem ser exatamente os definidos pelo código inicial fornecido.

O parâmetro de quantum deve ser informado somente para os algoritmos que utilizam Round-Robin.

## Saída

O programa deve produzir:

1. o tempo médio de espera dos processos;
2. a sequência de eventos de escalonamento.

A saída deve seguir o formato:

```text
Tempo médio de espera: XX
t  Processo
0  P1
1  P2
3  P1
5  P3
...
```

Cada linha da segunda parte representa um **evento de escalonamento**: o instante em que o escalonador toma uma decisão e o processo que passa a ser executado a partir daquele instante.

Um evento deve ser registrado mesmo quando a decisão do escalonador for manter o mesmo processo em execução.

Por exemplo, em Round-Robin, se o quantum terminar e o mesmo processo for novamente selecionado, deve ser registrado um novo evento.

O custo de trocas de contexto e de eventos de escalonamento deve ser considerado zero.

## Processo Pidle

O simulador deve utilizar um processo virtual chamado `Pidle` quando não houver nenhum processo disponível para execução.

`Pidle` não faz parte da lista de processos de entrada.

Por exemplo, se um processo terminar no instante 5 e o próximo processo chegar somente no instante 8, a saída deverá indicar a execução de `Pidle` durante esse intervalo:

```text
5  Pidle
8  P2
```

## Regras de desempate

Quando houver mais de um processo que possa ser escolhido pelo algoritmo e os critérios relevantes forem iguais, deve ser escolhido o processo que aparece primeiro na lista de entrada.

A prioridade é representada por números inteiros. **Quanto menor o número, maior a prioridade.**

## Tempo de espera

O tempo de espera de um processo corresponde ao tempo total em que o processo permanece aguardando na fila de processos prontos.

O programa deverá calcular e apresentar o tempo médio de espera de todos os processos.

O tempo de execução de `Pidle` não é considerado tempo de espera de nenhum processo.

## Algoritmos

### FCFS

No algoritmo First-Come, First-Served, os processos são executados na ordem em que chegam ao sistema.

O algoritmo é não preemptivo.

### SJF

No algoritmo Shortest-Job-First, entre os processos disponíveis é escolhido aquele com menor duração de burst.

O algoritmo é não preemptivo.

### SRTF

No algoritmo Shortest-Remaining-Time-First, entre os processos disponíveis é escolhido aquele com menor tempo restante de execução.

O algoritmo é preemptivo.

### Round-Robin

No Round-Robin, cada processo recebe a CPU durante no máximo o intervalo definido pelo quantum.

Ao término do quantum, o escalonador toma uma nova decisão.

### Prioridade

No escalonamento por prioridade, o processo de maior prioridade é escolhido para execução.

Deve ser implementada uma versão não preemptiva e uma versão preemptiva.

### Prioridade com Round-Robin

Processos de mesma prioridade devem compartilhar a CPU utilizando Round-Robin.

O quantum utilizado deve ser informado na linha de comando.

## Casos de teste

Serão fornecidos alguns casos de teste contendo os arquivos de entrada e as respectivas saídas esperadas.

Os casos de teste poderão ser utilizados para verificar a implementação dos algoritmos.

Além dos casos fornecidos, cada aluno deverá criar casos de testes adicionais para todos os algoritmos implementados. Esses casos devem servir para verificar a correção da implementação e permitir uma análise do comportamento de cada algoritmo de escalonamento.

Os casos de teste adicionais devem incluir cenários relevantes e representativos, de forma a evidencia, quando aplicável:
- situações que o algoritmo apresenta bom desempenho;
- situações em que o algoritmo apresenta desempenho desfavorável;
- vantagens e desvantagens observadas em relação aos demais algoritmos;
- influência do quantum nos algoritmos que utilizam Round-Robin;
- cenários que evidenciem características conhecidas dos algoritmos, como convoy effect, maior tempo de espera, favorecimento de processos curtos, impacto das prioridades e possíveis situações de espera prolongada.

Não é necessário que cada caso de teste demonstre todos esses aspectos. O conjunto de testes deve, porém, permitir uma análise adequada das principais características de cada algoritmo.

### Teste 1 — FCFS
Entrada: `testes/inputs/fcfs_01.csv`

Comando para Execução: `python scheduler.py testes/inputs/fcfs_01.csv fcfs`

Saída esperada: `testes/expected/fcfs_01.out`

### Teste 2 — SRTF
Entrada: `testes/inputs/srtf_01.csv`

Comando para Execução: `python scheduler.py testes/inputs/srtf_01.csv srtf`

Saída esperada: `testes/expected/srtf_01.out`

### Teste 3 — Round-Robin, quantum 1
Entrada: `testes/inputs/rr_01.csv`

Comando para Execução: `python scheduler.py testes/inputs/rr_01.csv rr 1`

Saída esperada: `testes/expected/rr_01.out`


## Regras
- Esse trabalho é individual
- O código entregue deverá implementar todos os algoritmos solicitados e preservar a interface definida pelo código inicial fornecido.
- Todos os arquivos devem ser reunidos em um únivo `.zip`
- O nome do arquivo zip deve seguir o padrão:
    - `tp1_[nomedoaluno].zip`
    > **Atenção:** Substitua `[nomedoaluno]` pelo seu nome completo. **É obrigatório seguir esta estrutura de nome.**
- O relatório deve ter o nome:
    - `tp1.pdf`
- Não modifique este arquivo `Readme.md`.
- O envio deve ser realizado via **Classroom** até a data limite do projeto.



## Entrega
A entrega deve conter:
- [ ] Código fonte (sem execuáveis)
- [ ] Conjunto de testes, com entradas e saídas esperadas
    - [ ] Cenários que permitam observar características relevantes, desvantagens e limitações dos algoritmos
    - [ ] Casos de teste adicionais para todos os algoritmos

- [ ] Relatório breve (em PDF) contendo:
    - [ ] Visão geral do código e fluxo de execução do programa
    - [ ] Descrição da abordagem de implementação adotada, assim como detalhes das funções implementadas
    - [ ] Desafio encontrados
    - [ ] Explicação dos casos de teste adicionais elaborados
    - [ ] Análise do comportamento de cada algoritmo nos casos de teste
    - [ ] Discussão sobre as principais vantagens e desvantagens de cada algoritmo de escalonamento
    - [ ] Identificação e explicação de cenários interessantes observados durante os testes
    Comparação entre os algoritmos, destacando situações em que determinados algoritmos são mais ou menos adequados
    - [ ] Para os algoritmos que utilizam Round-Robin, análise do impacto da escolha do quantum no comportamento do escalonamento
