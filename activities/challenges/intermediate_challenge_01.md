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
* **CRC-8** (1 byte): Valor de verificação calculado sobre os bytes `MSG_TYPE`, `LENGTH` e `PAYLOAD`, na ordem de transmissão. O cálculo inicia com `0x00` e, para cada byte, aplica operações de XOR e deslocamento de bits utilizando o valor `0x07`. O resultado final (1 byte) é anexado ao final do pacote e utilizado para detecção de erros na transmissão.
### Tipos de Mensagem

| MSG_TYPE | Nome | Formato do Payload                                                |
|----------|------|-------------------------------------------------------------------|
|  `0x01`  | IMU  | `ax(f32), ay(f32), az(f32), gx(f32), gy(f32), gz(f32)` — 24 bytes |
|  `0x02`  | BARO | `pressure(f32), temperature(f32)` — 8 bytes                       |
|  `0x03`  | GPS  | `lat(f64), lon(f64), alt(f32)` — 20 bytes                         |

Todos os valores multi-byte são **little-endian**.

### Interface Necessária

```javascript
class IMUFrame {
  /**
   * @param {number} ax
   * @param {number} ay
   * @param {number} az
   * @param {number} gx
   * @param {number} gy
   * @param {number} gz
   */
  constructor(ax, ay, az, gx, gy, gz) {
    this.ax = ax;
    this.ay = ay;
    this.az = az;
    this.gx = gx;
    this.gy = gy;
    this.gz = gz;
  }

  createImuPayload(ax, ay, az, gx, gy, gz) {
    // 1. Aloca um bloco de memória de exatamente 24 bytes
    const buffer = new ArrayBuffer(24);

    // 2. Cria uma interface (DataView) para manipular essa memória
    const view = new DataView(buffer);

    // 3. Escreve A (float32 ocupa 4 bytes * 3 valores = 12 bytes) a partir da posição 0.
    // O terceiro argumento 'true' garante a formatação little-endian.
    view.setFloat32(0, ax, true);
    view.setFloat32(4, ay, true);
    view.setFloat32(8, az, true);

    // 4. Escreve a G (float32 ocupa 4 bytes * 3 valores = 12 bytes) a partir da posição 12.
    view.setFloat32(12, gx, true);
    view.setFloat32(16, gy, true);
    view.setFloat32(20, gz, true);

    // 5. Converte para o array de bytes padrão
    const payloadBytes = new Uint8Array(buffer);

    // 6. Gera a visualização em Hexadecimal para debug
    const hexVisualizacao = Array.from(payloadBytes)
    .map(byte => '0x' + byte.toString(16).padStart(2, '0').toUpperCase())
    .join('');

    console.log(`[DEBUG] Payload Imu: ${hexVisualizacao}`);

    // 7. Retorna os bytes binários reais para o sistema usar
    return payloadBytes;
  }
}

class BaroFrame {
  /**
   * @param {number} pressure
   * @param {number} temperature
   */
  constructor(pressure, temperature) {
    this.pressure = pressure;
    this.temperature = temperature;
  }

  createBaroPayload(pressure, temperature) {
    // 1. Aloca um bloco de memória de exatamente 8 bytes
    const buffer = new ArrayBuffer(8);

    // 2. Cria uma interface (DataView) para manipular essa memória
    const view = new DataView(buffer);

    // 3. Escreve a pressão (float32 ocupa 4 bytes) a partir da posição 0.
    // O terceiro argumento 'true' garante a formatação little-endian.
    view.setFloat32(0, pressure, true);

    // 4. Escreve a temperatura (float32 ocupa 4 bytes) a partir da posição 4.
    view.setFloat32(4, temperature, true);

    // 5. Converte para o array de bytes padrão
    const payloadBytes = new Uint8Array(buffer);

    // 6. Gera a visualização em Hexadecimal para debug
    const hexVisualizacao = Array.from(payloadBytes)
    .map(byte => '0x' + byte.toString(16).padStart(2, '0').toUpperCase())
    .join('');

    console.log(`[DEBUG] Payload Baro: ${hexVisualizacao}`);

    // 7. Retorna os bytes binários reais para o sistema usar
    return payloadBytes;
  }
}

class GPSFrame {
  /**
   * @param {number} lat
   * @param {number} lon
   * @param {number} alt
   */
  constructor(lat, lon, alt) {
    this.lat = lat;
    this.lon = lon;
    this.alt = alt;
  }

  createGpsPayload(lat, lon, alt) {
    // 1. Aloca um bloco de memória de exatamente 20 bytes
    const buffer = new ArrayBuffer(20);

    // 2. Cria uma interface (DataView) para manipular essa memória
    const view = new DataView(buffer);

    // 3. Escreve a latitude (float32 ocupa 8 bytes) a partir da posição 0.
    // O terceiro argumento 'true' garante a formatação little-endian.
    view.setFloat64(0, lat, true);

    // 4. Escreve a longitude (float64 ocupa 8 bytes) a partir da posição 8.
    view.setFloat64(8, lon, true);

    // 5. Escreve a longitude (float64 ocupa 4 bytes) a partir da posição 16.
    view.setFloat32(16, alt, true);

    // 6. Converte para o array de bytes padrão
    const payloadBytes = new Uint8Array(buffer);

    // 7. Gera a visualização em Hexadecimal para debug
    const hexVisualizacao = Array.from(payloadBytes)
    .map(byte => '0x' + byte.toString(16).padStart(2, '0').toUpperCase())
    .join('');

    console.log(`[DEBUG] Payload Gps: ${hexVisualizacao}`);

    // 8. Retorna os bytes binários reais para o sistema usar
    return payloadBytes;
  }
}

class TelemetryParser {
  /**
   * Analisa um fluxo de bytes contendo um ou mais pacotes de telemetria.
   *
   * Retorna uma lista de objetos de frame decodificados (IMUFrame, BaroFrame, GPSFrame).
   * Ignora silenciosamente pacotes com CRC inválido ou MSG_TYPE desconhecido.
   * Registra um aviso (warning) para cada pacote ignorado.
   *
   * @param {Uint8Array} data - O fluxo de bytes a ser analisado.
   * @returns {Array<IMUFrame|BaroFrame|GPSFrame>} Lista contendo os frames decodificados.
   */
  parse(data) {
    console.log(data);
    
    return [];
  }
}
```

## Exemplo de Fluxo de Bytes

```javascript
const exampleStream = new Uint8Array([
  // --- INÍCIO DO PACOTE ---
  0xAA,             // 1. SYNC (Sempre 0xAA)
  0x02,             // 2. MSG_TYPE (0x02 = BARO)
  0x08, 0x00,       // 3. LENGTH (8 bytes de payload, em little-endian)
  
  // --- PAYLOAD (8 bytes no total) ---
  0x00, 0x50, 0x7D, 0x44, // 4. Pressure: 1013.25 em float32 (IEEE 754, little-endian)
  0x00, 0x00, 0xCC, 0x41, // 5. Temperature: 25.5 em float32 (IEEE 754, little-endian)
  
  // --- FIM DO PACOTE ---
  0x66              // 6. CRC8 (Exemplo de valor calculado sobre MSG_TYPE + LENGTH + PAYLOAD)
]);
```

## Exemplos de Payloads
```javascript
  // --- PAYLOAD (8 bytes no total) ---
  0x00, 0x50, 0x7D, 0x44, // 4. Pressure: 1013.25 em float32 (IEEE 754, little-endian)
  0x00, 0x00, 0xCC, 0x41, // 5. Temperature: 25.5 em float32 (IEEE 754, little-endian)

  // --- PAYLOAD (24 bytes f32) ---
  0x00, 0x00, 0x00, 0x00, // 4. ax: 0.0 (f32, little-endian)
  0x00, 0x00, 0x00, 0x00, // 5. ay: 0.0 (f32, little-endian)
  0xC3, 0xF5, 0x1C, 0x41, // 6. az: 9.81 (f32, little-endian)
  0x00, 0x00, 0x00, 0x3F, // 7. gx: 0.5 (f32, little-endian)
  0x00, 0x00, 0x00, 0xBF, // 8. gy: -0.5 (f32, little-endian)
  0x00, 0x00, 0x00, 0x00, // 9. gz: 0.0 (f32, little-endian)

  // --- PAYLOAD (20 bytes no total) ---
  0x87, 0x16, 0xD9, 0x26, 0x9B, 0xAC, 0x3D, 0xC0, // 4. lat: -29.6842 (f64, little-endian)
  0xBA, 0x49, 0x0C, 0x02, 0x2B, 0xE7, 0x4A, 0xC0, // 5. lon: -53.8069 (f64, little-endian)
  0x00, 0x80, 0x16, 0x43,                         // 6. alt: 150.5 (f32, little-endian)
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
