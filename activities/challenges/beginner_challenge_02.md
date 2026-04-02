# Desafio Iniciante 02 — Conversor de Temperatura

**Dificuldade:** Iniciante  
**Tempo estimado:** 30–45 minutos  
**Habilidades praticadas:** Funções, dicionários, validação de entrada, tratamento de erros

---

## Contexto

Conversões de temperatura são uma tarefa comum no mundo real. Foguetes lidam com temperaturas extremas, portanto a conversão precisa entre unidades é genuinamente importante no software aeroespacial.

---

## Pré-Requisitos
1. Ter o Node.js instalado (versão 20 ou superior). Veja [aqui](https://github.com/Tau-Rocket-Team/dev.docs/blob/main/docs/backend/nodejs.md) para instruções de instalação.
2. Ter o vitest instalado globalmente ou como dependência de desenvolvimento para rodar os testes. Veja [aqui](https://github.com/Tau-Rocket-Team/dev.docs/blob/main/docs/tests/vitest.md) para um guia completo sobre o Vitest.
3. Conhecimento básico de JavaScript ou TypeScript, incluindo loops, condicionais e funções. Veja [aqui](https://github.com/Tau-Rocket-Team/dev.docs/blob/main/docs/web/javascript.md)

---

## Enunciado do Problema

Crie uma classe `TemperatureConverter` que possa converter entre **Celsius (C)**, **Fahrenheit (F)** e **Kelvin (K)**.

### Fórmulas de Conversão

| De → Para | Fórmula |
|---|---|
| C → F | `F = (C × 9/5) + 32` |
| C → K | `K = C + 273.15` |
| F → C | `C = (F − 32) × 5/9` |
| F → K | `K = (F − 32) × 5/9 + 273.15` |
| K → C | `C = K − 273.15` |
| K → F | `F = (K − 273.15) × 9/5 + 32` |

### Interface da Classe

```javascript
class TemperatureConverter {
  /**
   * Converte um valor de temperatura de uma unidade para outra.
   *
   * @param {number} value - O valor da temperatura a ser convertido.
   * @param {string} fromUnit - Unidade de origem — "C", "F" ou "K".
   * @param {string} toUnit - Unidade de destino — "C", "F" ou "K".
   * @returns {number} Temperatura convertida arredondada para 4 casas decimais.
   * @throws {Error} Se qualquer uma das unidades não for "C", "F" ou "K".
   * @throws {Error} Se a temperatura resultante for inferior ao zero absoluto.
   */
  convert(value, fromUnit, toUnit) {
    // A lógica de conversão será inserida aqui
  }
}
```

### Exemplo de Uso

```javascript
const conv = new TemperatureConverter();

conv.convert(100, "C", "F")   // → 212.0
conv.convert(32, "F", "C")    // → 0.0
conv.convert(0, "C", "K")     // → 273.15
conv.convert(0, "K", "C")     // → -273.15
conv.convert(-300, "C", "K")  // → lança RangeError (abaixo do zero absoluto)
```

---

## Requisitos

1. Implemente a classe `TemperatureConverter`.
2. Valide as entradas — lance `RangeError` com uma mensagem significativa para unidades inválidas ou temperaturas fisicamente impossíveis (abaixo de 0 K).
3. Escreva pelo menos **8 testes unitários** incluindo casos extremos (zero absoluto, conversão para a mesma unidade, entrada inválida).
4. **Não** use nenhuma biblioteca externa — apenas JavaScript padrão.

---

## Bônus

- Adicione um método `convert_batch(values, from_unit, to_unit)` que aceite uma lista e retorne uma lista de valores convertidos.

---

## Submissão

1. Crie um branch: `feat/challenge-02-<your-github-username>`.
2. Coloque sua solução em: `activities/submissions/<your-github-username>/challenge_02/solution.py`.
3. Coloque seus testes em: `activities/submissions/<your-github-username>/challenge_02/test_solution.py`.
4. Abra um pull request e solicite seu mentor como revisor.
