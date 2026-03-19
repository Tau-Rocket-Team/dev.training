# Desafio Intermediário 02 — Máquina de Estado de Voo

**Dificuldade:** Intermediário  
**Tempo estimado:** 3–4 horas  
**Habilidades praticadas:** Máquinas de estado, POO, design orientado a eventos, testes

---

## Contexto

A lógica central de um computador de voo é uma **máquina de estados finitos (FSM)** que transita por fases bem definidas com base em eventos de sensores. Implementar isso corretamente — e verificar cada transição — é um trabalho crítico para a segurança.

---

## Enunciado do Problema

Implemente uma classe `FlightStateMachine` que modele o ciclo de vida de um voo de foguete.

### Estados

```
IDLE → ARMED → POWERED_ASCENT → COASTING → APOGEE → DESCENT → LANDED
                                                ↓
                                          (ao detectar)
```

| Estado | Descrição |
|---|---|
| `IDLE` | Pré-lançamento; aguardando comando de armamento |
| `ARMED` | Pronto para detectar decolagem |
| `POWERED_ASCENT` | Motor queimando; alta aceleração |
| `COASTING` | Motor esgotado; subindo por inércia |
| `APOGEE` | Altitude máxima detectada; implantar drogue |
| `DESCENT` | Descendo sob paraquedas |
| `LANDED` | Velocidade abaixo do limiar no solo |

### Eventos / Transições

| De | Evento | Para |
|---|---|---|
| `IDLE` | `arm()` chamado | `ARMED` |
| `ARMED` | Aceleração > 20 m/s² | `POWERED_ASCENT` |
| `POWERED_ASCENT` | Aceleração < 5 m/s² (burnout) | `COASTING` |
| `COASTING` | Velocidade vertical ≤ 0 (apogeu) | `APOGEE` |
| `APOGEE` | Automaticamente | `DESCENT` |
| `DESCENT` | Altitude < 50m E velocidade < 5 m/s | `LANDED` |

### Interface Necessária

```python
class FlightStateMachine:
    def __init__(self):
        self.state: str = "IDLE"
        self.events: list[tuple[str, str]] = []  # (timestamp, event_description)

    def arm(self) -> None:
        """Transition IDLE → ARMED. Raises RuntimeError if not in IDLE."""

    def update(self, accel_mps2: float, velocity_mps: float, altitude_m: float) -> None:
        """
        Feed sensor readings into the FSM. Triggers state transitions where
        conditions are met. Appends a record to self.events on each transition.
        """

    def is_terminal(self) -> bool:
        """Return True if the FSM has reached LANDED."""
```

### Exemplo de Uso

```python
fsm = FlightStateMachine()
fsm.arm()                           # IDLE → ARMED
fsm.update(25.0, 10.0, 5.0)        # ARMED → POWERED_ASCENT (accel > 20)
fsm.update(3.0, 150.0, 800.0)      # POWERED_ASCENT → COASTING (accel < 5)
fsm.update(3.0, 0.0, 1200.0)       # COASTING → APOGEE → DESCENT (vel ≤ 0)
fsm.update(3.0, -8.0, 30.0)        # DESCENT → LANDED (alt < 50 AND vel < 5)
print(fsm.state)                    # "LANDED"
print(fsm.is_terminal())            # True
```

---

## Requisitos

1. Implemente `FlightStateMachine` com todos os estados e transições.
2. Transições ilegais devem lançar `RuntimeError` com uma mensagem descritiva.
3. Cada transição de estado deve adicionar uma entrada em `self.events`.
4. Escreva testes unitários cobrindo:
   - Voo completo pelo caminho feliz de `IDLE` a `LANDED`.
   - Tentativas de transição ilegais.
   - Cada transição de estado individual.
   - Múltiplas atualizações de sensor que não acionam uma transição.

---

## Bônus

- Adicione um método `disarm()` que transita `ARMED → IDLE` (cenário de aborto).
- Adicione um método `reset()` que retorna a FSM para `IDLE` a partir de qualquer estado (recuperação segura após anomalia).
- Persista o log de eventos em um arquivo JSON.

---

## Submissão

1. Crie um branch: `feat/int-challenge-02-<your-github-username>`.
2. Coloque sua solução em: `activities/submissions/<your-github-username>/int_challenge_02/`.
3. Inclua um `README.md` na pasta da sua submissão explicando o design da sua máquina de estados.
4. Abra um pull request e solicite seu mentor como revisor.
