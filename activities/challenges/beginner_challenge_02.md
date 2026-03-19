# Desafio Iniciante 02 — Conversor de Temperatura

**Dificuldade:** Iniciante  
**Tempo estimado:** 30–45 minutos  
**Habilidades praticadas:** Funções, dicionários, validação de entrada, tratamento de erros

---

## Contexto

Conversões de temperatura são uma tarefa comum no mundo real. Foguetes lidam com temperaturas extremas, portanto a conversão precisa entre unidades é genuinamente importante no software aeroespacial.

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

```python
class TemperatureConverter:
    def convert(self, value: float, from_unit: str, to_unit: str) -> float:
        """
        Convert a temperature value from one unit to another.

        Args:
            value: The temperature value to convert.
            from_unit: Source unit — "C", "F", or "K".
            to_unit: Target unit — "C", "F", or "K".

        Returns:
            Converted temperature rounded to 4 decimal places.

        Raises:
            ValueError: If either unit is not "C", "F", or "K".
            ValueError: If the resulting temperature is below absolute zero.
        """
```

### Exemplo de Uso

```python
conv = TemperatureConverter()

conv.convert(100, "C", "F")   # → 212.0
conv.convert(32, "F", "C")    # → 0.0
conv.convert(0, "C", "K")     # → 273.15
conv.convert(0, "K", "C")     # → -273.15
conv.convert(-300, "C", "K")  # → lança ValueError (abaixo do zero absoluto)
```

---

## Requisitos

1. Implemente a classe `TemperatureConverter`.
2. Valide as entradas — lance `ValueError` com uma mensagem significativa para unidades inválidas ou temperaturas fisicamente impossíveis (abaixo de 0 K).
3. Escreva pelo menos **8 testes unitários** incluindo casos extremos (zero absoluto, conversão para a mesma unidade, entrada inválida).
4. **Não** use nenhuma biblioteca externa — apenas Python padrão.

---

## Bônus

- Adicione um método `convert_batch(values, from_unit, to_unit)` que aceite uma lista e retorne uma lista de valores convertidos.
- Adicione uma interface de linha de comando para que o script possa ser executado como: `python solution.py 100 C F`.

---

## Submissão

1. Crie um branch: `feat/challenge-02-<your-github-username>`.
2. Coloque sua solução em: `activities/submissions/<your-github-username>/challenge_02/solution.py`.
3. Coloque seus testes em: `activities/submissions/<your-github-username>/challenge_02/test_solution.py`.
4. Abra um pull request e solicite seu mentor como revisor.
