# Guia de instalação do claude-mem (v16.1.3)

O claude-mem dá memória ao Claude Code entre sessões: o que você discutiu hoje
(plano de negócio, decisões, tarefas) aparece automaticamente na próxima conversa.

## 1. Pré-requisitos (no SEU computador)

- **Node.js 20 ou superior** — baixe em https://nodejs.org (versão LTS).
- **Claude Code** atualizado.
- Bun e uv são instalados automaticamente pelo instalador, se faltarem.

Confira o Node no terminal:

```bash
node --version
```

## 2. Instalar

```bash
npx claude-mem install
```

O instalador pergunta qual IA vai resumir suas sessões. Escolha **Claude**
(usa o mesmo login do seu Claude Code; nada vai para terceiros).

## 3. Aplicar a configuração recomendada (privacidade)

Copie `settings.recomendado.json` (desta pasta) para `~/.claude-mem/settings.json`
**mesclando** com o que já estiver lá (o arquivo é criado na primeira execução).
Ele garante que a sincronização na nuvem e o TeamBrain ficam desligados.

## 4. Usar

1. Abra o Claude Code na pasta do seu projeto (ex.: este repositório).
2. Trabalhe normalmente. O claude-mem grava observações sozinho.
3. Na próxima sessão, o contexto anterior já vem carregado.
4. Para ver as memórias no navegador: http://localhost (a porta aparece no terminal
   ao iniciar o worker).

**Dica:** qualquer coisa entre `<private>` e `</private>` nunca é gravada.
Use para senhas, CPF, dados bancários:

```
Meu CNPJ provisório é <private>00.000.000/0001-00</private>
```

## 5. Desinstalar

```bash
npx claude-mem uninstall
```

Seus dados ficam em `~/.claude-mem/` — apague a pasta se quiser remover tudo.
