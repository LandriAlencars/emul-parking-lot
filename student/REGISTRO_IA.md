# Registro de uso de IA

Preencha durante o trabalho, não no fim. É o portão G4 da avaliação
([desafio](../docs/desafio.md#portões)): os dois alunos da dupla precisam ter
entradas. Quem não usou IA escreve isso aqui.

## Dupla

| Aluno | Matrícula | Partes que escreveu |
|---|---|---|
| Landri José de Alencar Sousa | _(preencher)_ | Trabalho individual: todo o `student/`, desenvolvido com o agente (veja abaixo) |
| _(sem dupla: trabalho solo)_ | | |

## Ferramentas

| Aluno | Ferramenta | Modelo/versão | Período de uso |
|---|---|---|---|
| Landri José de Alencar Sousa | Claude (app claude.ai, sessão de agente) | Claude Opus 5.5 | 2026-11-19 |

## Tarefas

### 2026-11-19: ambiente e leitura dos contratos

- **Aluno:** Landri José de Alencar Sousa
- **Requisito(s):** todos
- **O que pedi:** montar o ambiente e implementar o desafio de forma
  estruturada, para eu estudar depois.
- **O que a IA propôs:** instalou o workspace west (Zephyr v4.5.0, SDK 1.0.1,
  Renode 1.17.0 portátil) num ambiente Linux na nuvem; rodou o primeiro teste
  e conferiu a falha esperada do código inicial (`position_mdeg expected
  90000, observed 0`); leu desafio, especificação, protocolos, modelos e
  cenários.
- **O que aceitei, mudei ou rejeitei, e por quê:** aceitei. _(revisar)_
- **Como verifiquei:** `lab.py --headless normal` com o código inicial.
- **O que aprendi / o que ainda preciso entender:** _(preencher)_

### 2026-11-19: arquitetura e firmware completo

- **Aluno:** Landri José de Alencar Sousa
- **Requisito(s):** REQ-01 a REQ-13
- **O que pedi:** implementação completa.
- **O que a IA propôs:** separar a lógica do hardware: `hw.c` é a única camada
  que chama drivers; `lane.c` é uma máquina de estados usada pelas duas faixas;
  `adc_sampler.c` é o dono único do ADC4 (uma conversão assíncrona por vez,
  prazo de 100 ms); `line_parser.c` monta as linhas das três UARTs;
  `protocol.c` faz AT/P1; `tariff.c`, `sessions.c`; `display.c` + `font.c`
  (fonte 5x7 desenhada pelo agente) desenham via SPI numa thread de prioridade
  baixa; ISRs (EXTI e RX de UART) só acordam o laço de controle. Constantes
  centralizadas em `parking.h`. Decisões: abandono durante a identificação
  volta a IDLE sem assistência; abandono durante o pagamento pede assistência
  e cancela; espera de 50 ms na partida por causa do assentamento dos sensores
  no modelo.
- **O que aceitei, mudei ou rejeitei, e por quê:** _(preencher depois de
  estudar o GUIA_DE_ESTUDO.md)_
- **Como verifiquei:** 19/19 cenários públicos (`assess.py`, todos os
  cenários); fila de carros `assess.py --lot 500` com 17 perfis (captura que
  se recupera/falha/sem resposta, placa lida errada, pagamento
  atrasado/cancelado/malformado/sem resposta, hesitar, sair durante o
  pagamento, ficar no feixe, bloquear o fechamento): 30/30 veredictos.
  Problemas encontrados e corrigidos no caminho: macro errado para o canal do
  ADC (`adc_channel_setup_dt` devolvia `-ENOTSUP`), `uart_irq_update()`
  devolve `void` no Zephyr 4.5 e `SPI_OP_MODE_MASTER` está obsoleto.
- **O que aprendi / o que ainda preciso entender:** _(preencher)_

### 2026-11-19: testes de unidade, cobertura e análise estática

- **Aluno:** Landri José de Alencar Sousa
- **Requisito(s):** todos (qualidade)
- **O que pedi:** parte da implementação completa.
- **O que a IA propôs:** duas suítes ztest + FFF: `tests/logica` (tarifa,
  sessões, montador de linhas, protocolos, fonte, display e ADC com `hw.h`
  falso) e `tests/faixa` (máquina de estados com `hw.h` e `ui.h` falsos).
  Removeu o exemplo `tests/unit`. Simplificou condições redundantes e
  acrescentou 12 exclusões `LCOV_EXCL_LINE`, todas para falhas de driver que o
  Renode não produz (justificativas no próprio código e no guia).
- **O que aceitei, mudei ou rejeitei, e por quê:** _(preencher)_
- **Como verifiquei:** `firmware-coverage.py`: 100% de linhas e 98,8% de
  branches, com 19/19 cenários e as 2 suítes passando; `static-check.py`:
  0 achados.
- **O que aprendi / o que ainda preciso entender:** _(preencher)_

## Código de terceiros

Trechos copiados ou adaptados de outras fontes (documentação, exemplos do
Zephyr, fóruns), com origem e licença.

| Arquivo/trecho | Origem | Licença |
|---|---|---|
| Nenhum trecho copiado. As chamadas de API seguem a documentação e os cabeçalhos do Zephyr v4.5.0 (`drivers/gpio.h`, `adc.h`, `uart.h`, `spi.h`, `sys/ring_buffer.h`). A fonte 5x7 de `font.c` foi desenhada para este trabalho. | — | — |
