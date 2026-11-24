# Revisão de segurança — claude-mem v16.1.3

Revisão feita sobre o código-fonte do arquivo `claude-mem-main.zip`
(~4.000 arquivos, licença Apache-2.0). Foi uma leitura dirigida dos pontos
de risco, não uma auditoria linha a linha.

## Veredito

**Seguro para uso pessoal, com a configuração padrão.** Os dados ficam no seu
computador; nada é enviado para servidores do projeto a menos que você ative
recursos pagos/opcionais.

## O que ele faz no seu computador

| Ponto | O que encontrei | Risco |
|---|---|---|
| Hooks do Claude Code | Roda comandos `claude-mem` no início da sessão, a cada mensagem e após cada ferramenta usada (`plugin/hooks.json`) | Baixo — é assim que ele captura o contexto. Lê tudo o que o Claude faz. |
| Servidor local (worker) | Escuta só em `127.0.0.1` (`CLAUDE_MEM_WORKER_HOST`) | Baixo — não fica exposto na rede. |
| Banco de dados | SQLite em `~/.claude-mem/` | Baixo — local; quem acessa seu usuário acessa as memórias. |
| Resumos por IA | Padrão `CLAUDE_MEM_PROVIDER = claude`, modelo `haiku` | Médio — trechos das sessões vão para a Anthropic (mesmo destino do Claude Code). Consome sua cota. |
| Sincronização na nuvem | `sync.cmem.ai` / Supabase, **desligada por padrão** (`CLAUDE_MEM_CLOUD_SYNC_HUB_URL` vazio) | Alto *se ativada* — enviaria suas memórias a terceiros. Mantenha desligada. |
| TeamBrain / Pro | `cmem.ai`, **desligado por padrão** | Mesmo caso acima. |
| Provedores alternativos | DeepSeek, OpenRouter, Gemini, iFlytek (China), etc. | Alto *se escolhidos* — seus dados iriam para esses serviços. Fique com Claude. |
| Telemetria | `setup-funnel.ts` não faz chamadas de rede | Nenhum risco encontrado. |
| Script de instalação | Não há `postinstall` no `package.json`; instala Bun e uv se faltarem | Baixo, mas baixa programas da internet — normal. |

## Recomendações

1. Use `settings.recomendado.json` (sync e TeamBrain vazios = desligados).
2. Escolha o provedor **Claude** na instalação.
3. Envolva dados sensíveis em `<private>...</private>`.
4. Não aceite ofertas de "Pro"/sync se não quiser seus dados na nuvem deles.
5. Se trabalhar com dados de clientes, adicione a pasta em
   `CLAUDE_MEM_EXCLUDED_PROJECTS`.
