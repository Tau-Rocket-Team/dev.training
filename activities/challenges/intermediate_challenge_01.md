# Desafio Intermediário 01 — Parser de Pacotes de Telemetria

**Dificuldade:** Intermediário  
**Tempo estimado:** 2–3 horas  
**Habilidades praticadas:** Parsing de dados binários, implementação de protocolo, POO, tratamento de erros, testes

---

## Contexto

Computadores de voo transmitem dados de telemetria como pacotes binários por um link serial. Conseguir analisar esses pacotes de forma confiável — e tratar dados corrompidos ou malformados de forma elegante — é uma habilidade essencial para os times de aviônica e estação terrestre.

---

## Enunciado do Problema

Implemente um **parser de pacotes de telemetria** para o seguinte protocolo binário:

### Formato do Pacote

```
┌──────┬──────────┬────────┬───────────────┬──────┐
│ 0xAA │ MSG_TYPE │ LENGTH │    PAYLOAD    │ CRC8 │
│  1B  │    1B    │   2B   │  0–255 bytes  │  1B  │
└──────┴──────────┴────────┴───────────────┴──────┘
```

- **Byte de sincronização** (`0xAA`): Sempre o primeiro byte. Se não estiver presente, avance até encontrá-lo.
- **MSG_TYPE** (1 byte): Identifica o formato do payload (veja tabela abaixo).
- **LENGTH** (2 bytes, little-endian): Número de bytes no payload.
- **PAYLOAD**: Dados de comprimento variável interpretados de acordo com MSG_TYPE.
- **CRC8** (1 byte): Calculado sobre `MSG_TYPE + LENGTH + PAYLOAD` usando o polinômio `0x07`.

### Tipos de Mensagem

| MSG_TYPE | Nome | Formato do Payload |
|---|---|---|
| `0x01` | IMU | `ax(f32), ay(f32), az(f32), gx(f32), gy(f32), gz(f32)` — 24 bytes |
| `0x02` | BARO | `pressure(f32), temperature(f32)` — 8 bytes |
| `0x03` | GPS | `lat(f64), lon(f64), alt(f32)` — 20 bytes |

Todos os valores multi-byte são **little-endian**.

### Interface Necessária

```python
from dataclasses import dataclass

@dataclass
class IMUFrame:
    ax: float; ay: float; az: float
    gx: float; gy: float; gz: float

@dataclass
class BaroFrame:
    pressure: float
    temperature: float

@dataclass
class GPSFrame:
    lat: float; lon: float; alt: float

class TelemetryParser:
    def parse(self, data: bytes) -> list:
        """
        Parse a byte stream containing one or more telemetry packets.

        Returns a list of decoded frame objects (IMUFrame, BaroFrame, GPSFrame).
        Silently skips packets with invalid CRC or unknown MSG_TYPE.
        Logs a warning for each skipped packet.
        """
```

---

## Requisitos

1. Implemente `TelemetryParser` com o método `parse(data)`.
2. Implemente `crc8(data: bytes) -> int` — CRC-8 com polinômio `0x07`.
3. Trate casos extremos:
   - Stream começa no meio de um pacote (procure `0xAA`).
   - Múltiplos pacotes em uma única chamada.
   - Pacote truncado no final do buffer.
   - CRC inválido — pule e continue.
4. Escreva testes unitários para:
   - Parsing de cada tipo de mensagem corretamente.
   - Validação de CRC (válido e inválido).
   - Tratamento de ressincronização no meio do stream.
   - Entradas vazias / truncadas.

---

## Bônus

- Adicione uma classe `TelemetrySerializer` que faz o inverso: recebe um objeto de frame e retorna os bytes corretamente formados.
- Verifique que `parse(serialize(frame)) == [frame]` para todos os tipos de frame em seus testes.

---

## Submissão

1. Crie um branch: `feat/int-challenge-01-<your-github-username>`.
2. Coloque sua solução em: `activities/submissions/<your-github-username>/int_challenge_01/`.
3. Inclua um `README.md` na pasta da sua submissão explicando sua abordagem e quaisquer decisões de design.
4. Abra um pull request e solicite seu mentor como revisor.
