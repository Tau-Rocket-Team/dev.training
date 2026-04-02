# Desafio Iniciante 01 — FizzBuzz com um Toque

**Dificuldade:** Iniciante  
**Tempo estimado:** 40–60 minutos  
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

## Pré-Requisitos
1. Ter o Node.js instalado (versão 20 ou superior). Veja [aqui](https://github.com/Tau-Rocket-Team/dev.docs/blob/main/docs/backend/nodejs.md) para instruções de instalação.
2. Ter o vitest instalado globalmente ou como dependência de desenvolvimento para rodar os testes. Veja [aqui](https://github.com/Tau-Rocket-Team/dev.docs/blob/main/docs/tests/vitest.md) para um guia completo sobre o Vitest.
3. Conhecimento básico de JavaScript ou TypeScript, incluindo loops, condicionais e funções. Veja [aqui](https://github.com/Tau-Rocket-Team/dev.docs/blob/main/docs/web/javascript.md)

---

## Requisitos

1. Implemente `fizzbuzz(n: int) -> list[str]` em JavaScript ou TypeScript.
   * Assinatura (TS): `fizzBuzz(n: number): string[]`
2. A função deve tratar casos extremos:
   - `n = 0` → retorne uma lista vazia.
   * `n` negativo → Lance um **`RangeError`** com uma mensagem descritiva (ex: "O valor de n deve ser positivo").
3.  Escreva pelo menos **5 testes unitários** utilizando **Jest** ou **Vitest**, cobrindo:
      * Entrada zero.
      * Entrada negativa (validação de exceção).
      * Múltiplos de 3 (Fizz).
      * Múltiplos de 5 (Buzz).
      * Múltiplos de ambos (FizzBuzz).
4. Seu código deve estar devidamente formatado e com lint antes da submissão.

---

## Submissão

1. Crie um branch: `feat/challenge-01-<your-github-username>`.
2. Coloque sua solução em: `activities/submissions/<your-github-username>/challenge_01/solution.js`.
3. Coloque seus testes em: `activities/submissions/<your-github-username>/challenge_01/test_solution.js`.
4. Abra um pull request e solicite seu mentor como revisor.

Veja [`activities/submissions/README.md`](../submissions/README.md) para instruções completas de submissão.
