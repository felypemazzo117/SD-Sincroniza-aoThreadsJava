Sincronização de Threads em Java
# Atividade 01  
## Incluir Análise

O programa `MeuDadoThreadsJava` implementa o padrão Produtor-Consumidor utilizando threads, mas **sem sincronização**. A atividade consiste em executar o programa duas vezes, gerar logs distintos e comparar os resultados.

### Padrões Observados

- **Ordem de execução não garantida**  
  Em ambas as execuções, há intercalamento entre `Produtor` e `Consumidor`, mas sem controle. Em alguns momentos, o consumidor imprime valores antes do produtor armazená-los.

- **Repetição de valores**  
  O consumidor lê o mesmo valor várias vezes seguidas. Exemplo: `Consumidor: 9` aparece quatro vezes consecutivas. Isso ocorre porque o consumidor acessa o dado antes que o produtor o atualize.

- **Leitura antecipada**  
  O consumidor imprime valores antes da mensagem do produtor aparecer, indicando que acessou o dado antes da escrita.

- **Diferença entre execuções**  
  A ordem dos pares `Produtor`/`Consumidor` muda entre os logs. A quantidade de repetições também varia. Isso confirma o comportamento não determinístico do programa.

### Conclusão Técnica

- O programa não possui sincronização entre produtor e consumidor.
- Isso causa:
  - Leitura de dados antes da escrita
  - Repetição de valores
  - Ordem imprevisível de execução
- Para corrigir, recomenda-se:
  - Uso de `wait()` e `notify()` com blocos `synchronized`
  - Ou estruturas como `BlockingQueue` da biblioteca `java.util.concurrent`

  # Atividade 02  
## Incluir Análise

O programa `MeuDadoMonitorJava` simula o padrão Produtor-Consumidor utilizando threads com sincronização via monitor (`synchronized`). A atividade consiste em executar o programa duas vezes, gerar logs distintos e comparar os resultados.

---

### Análise Comparativa da Execução

#### Padrões Observados

- **Sincronização presente**  
  A execução mostra que o consumidor só acessa o dado após o produtor armazená-lo, evidenciado pelas mensagens sequenciais de "Armazenar Finalizando..." seguidas por "Carregar Finalizando..." e "Consumidor usando Monitor".

- **Ordem consistente**  
  Os valores seguem uma ordem crescente (0 a 4), sem repetições ou leituras fora de sequência. Isso indica que o monitor está funcionando corretamente para controlar o acesso ao dado compartilhado.

- **Ausência de leitura antecipada**  
  Ao contrário da versão sem sincronização, o consumidor não lê valores antes do produtor armazenar. Isso mostra que o controle de concorrência está bem implementado.

- **Execuções similares**  
  Mesmo com variações de tempo (por causa do `Thread.sleep` aleatório), as duas execuções geram logs com estrutura semelhante e valores consistentes, o que reforça a eficácia da sincronização.

---

### Conclusão Técnica

- O uso de `synchronized` e controle com flags (`Pronto`, `Ocupado`) garante que produtor e consumidor **não acessem o dado simultaneamente**.
- A sincronização evita:
  - Leitura de dados antes da escrita
  - Repetição de valores
  - Ordem imprevisível de execução
- O comportamento é **determinístico e seguro**, ideal para aplicações que exigem consistência entre threads.

---

# Atividade 03  
## Incluir Análise

Nesta atividade, foi executado o programa `MeuDadoEventJava`, que utiliza **eventos de sincronização** com `wait()` e `notify()` para coordenar o acesso entre produtor e consumidor. O objetivo é comparar os resultados dessa execução com os obtidos nas Atividades 01 (sem sincronização) e 02 (com monitor sincronizado).

---

### Análise Comparativa das Três Atividades

#### Atividade 01 — Sem Sincronização

- **Problemas observados:**
  - O consumidor acessa o dado antes do produtor armazenar.
  - Repetição de valores.
  - Ordem de execução imprevisível.
- **Comportamento:** Não determinístico e sujeito a condições de corrida.

#### Atividade 02 — Com Monitor (`synchronized`)

- **Melhorias observadas:**
  - O consumidor só acessa o dado após o produtor armazenar.
  - Ordem de execução consistente.
  - Sem repetição indevida de valores.
- **Comportamento:** Determinístico e seguro, com controle por flags (`Pronto`, `Ocupado`).

#### Atividade 03 — Com Eventos (`wait()` / `notify()`)

- **Resultados observados:**
  - O consumidor aguarda o produtor finalizar antes de acessar o dado.
  - O uso de `wait()` e `notify()` elimina a espera ativa (`while`), tornando o programa mais eficiente.
  - As mensagens de log mostram alternância clara entre produtor e consumidor, com valores sincronizados.
  - Em alguns momentos, há duplicação de mensagens (ex: `Consumidor usando Eventos: 13` aparece duas vezes), o que pode indicar múltiplas notificações ou chamadas concorrentes, mas sem quebra de ordem.

---

### Conclusão Técnica

- A evolução das atividades mostra os impactos da sincronização:
  - **Atividade 01:** Sem controle → comportamento caótico.
  - **Atividade 02:** Controle por monitor → comportamento previsível.
  - **Atividade 03:** Controle por eventos → comportamento eficiente e elegante.
- O uso de `wait()` e `notify()` é mais adequado para aplicações reais, pois evita loops de espera e melhora o desempenho geral do sistema.

---
