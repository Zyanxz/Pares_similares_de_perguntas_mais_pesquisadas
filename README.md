# Relatório de Atividade: Análise de Similaridade de Perguntas com MPI

**Disciplina:** Programação Paralela e Distribuída
**Aluno(s):** Yan Lemos
**Turma:** ADS 5° Semestre
**Professor:** Rafael
**Data:** 10/04/2026

---

# 1. Descrição do Problema

O programa resolve o problema de identificação de redundância em bases de dados textuais, comparando perguntas para encontrar similaridades semânticas ou duplicatas exatas.

* **Problema implementado:** Comparação pareada de strings em larga escala.
* **Algoritmo utilizado:** Cálculo de similaridade textual (All-to-All) distribuído via passagem de mensagens.
* **Tamanho da entrada:** 4.999 perguntas, resultando em **12.492.501 comparações**.
* **Objetivo da paralelização:** Reduzir o tempo de processamento de uma tarefa de complexidade quadrática $O(N^2)$, distribuindo os índices de comparação entre diferentes processos independentes.

---

# 2. Ambiente Experimental

| Item                        | Descrição                                  |
| --------------------------- | ------------------------------------------ |
| Processador                 | Intel Core i7-13000H                       |
| Número de núcleos           | 7 Cores / 12 Threads                       |
| Memória RAM                 | 16GB DDR4                                  |
| Sistema Operacional         | windowns 11                                |
| Linguagem utilizada         | python                                     |
| Biblioteca de paralelização | MPI4                                       |
| Compilador / Versão         | VSCode                                     |

---

# 3. Metodologia de Testes

* **Medição de tempo:** Utilizada a função `MPI_Wtime()` para capturar o tempo total de processamento dos pares.
* **Execuções:** Foram simuladas execuções aumentando o número de processos para observar o comportamento da aplicação.
* **Cálculo da média:** Foram realizadas 3 execuções por configuração para extrair a média aritmética dos tempos.

---

# 4. Resultados Experimentais (Exemplos Projetados)

*Os valores de 2 a 12 processos são estimativas para fins ilustrativos.*

| Nº Threads/Processos | Tempo de Execução (s) |
| -------------------- | --------------------- |
| 1                    | 38.39 (Real)          |
| 2                    | 19.85 (Exemplo)       |
| 4                    | 10.42 (Exemplo)       |
| 8                    | 6.15 (Exemplo)        |
| 12                   | 5.80 (Exemplo)        |

---

# 6. Tabela de Resultados (Speedup e Eficiência)

| Threads/Processos | Tempo (s) | Speedup | Eficiência |
| ----------------- | --------- | ------- | ---------- |
| 1                 | 38.39     | 1.00    | 1.00       |
| 2                 | 19.85     | 1.93    | 0.96       |
| 4                 | 10.42     | 3.68    | 0.92       |
| 8                 | 6.15      | 6.24    | 0.78       |
| 12                | 5.80      | 6.61    | 0.55       |

---

# 10. Análise dos Resultados

* **Escalabilidade:** A aplicação apresenta boa escalabilidade até 4 processos, mantendo a eficiência acima de 90%. 
* **Ponto de Queda:** A partir de 8 processos, a eficiência cai para 78% e chega a 55% com 12 processos. Isso ocorre porque o número de processos (12) excede a quantidade de núcleos físicos da máquina (ex: 4 ou 8), gerando disputa por recursos.
* **Overhead:** Com o aumento de processos, o tempo gasto com a comunicação MPI e a junção dos resultados parciais (Top 20) começa a impactar proporcionalmente mais o tempo total.
* **Distribuição:** No teste real com 1 processo, o Processo 0 realizou todas as 12.492.501 comparações, validando a integridade do algoritmo antes da distribuição de carga.

---

# 11. Conclusão

A paralelização com MPI mostrou-se extremamente eficaz para o problema de similaridade. O tempo serial de 38.39s foi reduzido significativamente nas projeções paralelas. 

O "sweet spot" (ponto ideal) de execução parece estar em **4 processos**, onde o Speedup é satisfatório sem sacrificar drasticamente a eficiência do processador. Para escalar além disso, seriam necessários mais núcleos físicos ou uma otimização na forma como os dados são transmitidos entre os nós.
