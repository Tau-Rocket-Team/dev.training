# Desafio Iniciante 01 — FizzBuzz com um Toque

**Dificuldade:** Iniciante  
**Tempo estimado:** 20–30 minutos  
**Habilidades praticadas:** Loops, condicionais, funções, código limpo

---

## Contexto

FizzBuzz é um exercício de programação clássico. Esta versão estende o original para torná-lo mais interessante e praticar a escrita de código limpo e bem estruturado.

---

## Enunciado do Problema

Escreva uma função `fizzbuzz(n)` que receba um inteiro positivo `n` e retorne uma **lista de strings** para todos os números de 1 a `n` (inclusive), seguindo estas regras:

- Se o número for divisível por **3**, retorne `"Fizz"`.
- Se o número for divisível por **5**, retorne `"Buzz"`.
- Se o número for divisível por **3 e 5**, retorne `"FizzBuzz"`.
- Se o número for divisível por **7**, **acrescente** `"Rocket"` ao resultado obtido até agora (ex.: `"Rocket"`, `"FizzRocket"`, `"BuzzRocket"`, `"FizzBuzzRocket"`).
- Caso contrário, retorne o número como string.

### Exemplo

```
fizzbuzz(20) → [
  "1", "2", "Fizz", "4", "Buzz", "Fizz", "Rocket", "8", "Fizz",
  "Buzz", "11", "Fizz", "13", "Rocket", "FizzBuzz", "16", "17",
  "Fizz", "19", "Buzz"
]
```

---

## Requisitos

1. Implemente `fizzbuzz(n: int) -> list[str]` em Python (ou na linguagem de sua escolha).
2. A função deve tratar casos extremos:
   - `n = 0` → retorne uma lista vazia.
   - `n` negativo → lance um `ValueError` com uma mensagem descritiva.
3. Escreva pelo menos **5 testes unitários** cobrindo diferentes cenários.
4. Seu código deve estar devidamente formatado e com lint antes da submissão.

---

## Bônus

- Aceite um parâmetro opcional `rules` que é uma lista de tuplas `(divisor, label)`, tornando a função totalmente configurável. As regras padrão do FizzBuzz devem ser o padrão.

```python
# Exemplo de uso com regras customizadas
fizzbuzz(15, rules=[(3, "Fizz"), (5, "Buzz"), (7, "Rocket")])
```

---

## Submissão

1. Crie um branch: `feat/challenge-01-<your-github-username>`.
2. Coloque sua solução em: `activities/submissions/<your-github-username>/challenge_01/solution.py`.
3. Coloque seus testes em: `activities/submissions/<your-github-username>/challenge_01/test_solution.py`.
4. Abra um pull request e solicite seu mentor como revisor.

Veja [`activities/submissions/README.md`](../submissions/README.md) para instruções completas de submissão.
