# Submissões

Este diretório é onde os desenvolvedores enviam seus desafios e tarefas concluídos.

---

## Estrutura de Diretórios

Cada desenvolvedor tem seu próprio subdiretório nomeado após seu nome de usuário do GitHub:

```
submissions/
├── README.md               ← Este arquivo
├── fernando-dorneles/
│   ├── challenge_01/
│   │   ├── solution.js
│   │   └── test_solution.js
│   └── int_challenge_01/
│       ├── solution.js
│       ├── test_solution.js
│       └── README.md
└── john-doe/
    └── challenge_01/
        ├── solution.js
        └── test_solution.js
```

---

## Instruções de Submissão

### Passo 1: Crie Seu Diretório

Se o seu diretório `submissions/<seu-github-username>/` ainda não existir, crie-o:

```bash
mkdir -p activities/submissions/<your-github-username>
```

### Passo 2: Adicione Sua Solução

Coloque seus arquivos no subdiretório correto conforme especificado pelo desafio:

```bash
# Exemplo para o desafio 01
mkdir -p activities/submissions/<your-github-username>/challenge_01
# Adicione solution.js e test_solution.js
```

### Passo 3: Execute os Testes e o Lint Localmente

Sempre verifique se seu trabalho passa em todos os critérios antes de realizar o `push`:

```bash
cd activities/submissions/<your-github-username>/challenge_01

# Instalar as dependências (caso seja a primeira vez)
npm install

# Executar os testes com Vitest
npx vitest run

# Verificar regras de código (Lint)
npx eslint .

# Verificar a formatação (Prettier)
npx prettier --check .
```

### Passo 4: Abra um Pull Request

1. Faça push do seu branch.
2. Abra um PR contra `main` usando o [template de pull request](../../templates/pull_request_template.md).
3. Nomeie o PR: `submission: <challenge-name> — <your-github-username>`.
4. Solicite seu mentor como revisor.

---

## Critérios de Revisão

Os mentores avaliarão as submissões com base em:

| Critério | Descrição |
|---|---|
| **Correção** | A solução produz a saída correta para todos os casos? |
| **Testes** | Os casos extremos estão cobertos? Todos os testes passam? |
| **Qualidade do código** | O código é legível, bem nomeado e devidamente documentado? |
| **Estilo** | Passa no lint sem avisos? |
| **Design** | A solução é bem estruturada e fácil de estender? |
