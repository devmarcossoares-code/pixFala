# PixFala — instruções para agentes

## Como navegar no projeto

Em tarefas de código, consulte `MAPA_DO_APP.md` para localizar a área relevante; abra primeiro o ponto de entrada indicado e siga imports, consumidores e dependências diretamente relacionadas. Não faça varreduras amplas sem necessidade, mas não limite uma mudança a um único arquivo quando o fluxo atravessar telas, serviços ou código nativo.

Para detalhes de comportamento e fluxos, consulte `ESPECIFICACAO_TECNICA_PIXFALA.md` quando a tarefa exigir esse contexto. Se os documentos divergirem do código, confirme o comportamento no código e atualize a especificação quando a mudança afetar o que ela descreve.

## Manutenção da documentação

- Na mesma tarefa, atualize os arquivos Markdown pertinentes quando uma mudança alterar arquitetura, fluxos entre módulos, comportamento importante do usuário, persistência/dados, integração nativa/permissões, dependências ou comandos de desenvolvimento/build.
- Atualize `MAPA_DO_APP.md` quando caminhos, responsabilidades ou relações entre módulos mudarem; atualize `ESPECIFICACAO_TECNICA_PIXFALA.md` quando mudar comportamento ou detalhes técnicos ali documentados. Atualize ambos somente quando ambos forem afetados.
- Mudanças internas pequenas que não alterem comportamento nem arquitetura não exigem atualização de documentação.
- Não encerre uma mudança crucial sem atualizar a documentação relevante ou explicar claramente por que não há documento aplicável. Mantenha a documentação concisa, correta e sem duplicar conteúdo entre arquivos.

## Regras de implementação

- Este app usa Expo, React Native, TypeScript e Expo Router. As rotas ficam em `app/` (não em `src/app/`); código compartilhado fica em `components/`, `services/`, `utils/` e `constants/`.
- Antes de alterar APIs do Expo, EAS ou React Native, confira a versão instalada de `expo` em `package.json` e consulte `https://docs.expo.dev/versions/v<major>.0.0/`. Para tópicos não cobertos, use `https://docs.expo.dev/llms.txt` e siga para a documentação específica.
- Preserve compatibilidade multiplataforma na UI, mas reconheça que o listener de notificações e a fila nativa são específicos do Android.
- Se `ios/` e `android/` não existirem, são gerados pelo Continuous Native Generation. Configure comportamento nativo por `app.json` ou config plugins; não crie nem edite diretórios nativos gerados à mão.
- Para dependências Expo, use `npx expo install <pacote>`; para outras dependências, use npm. Este repositório usa `package-lock.json`; use `bunx` apenas se o projeto passar a usar Bun.
- Não execute `expo install --fix` como rotina: use-o somente se a validação indicar incompatibilidades de versões.

## Validação

- `npm run lint`
- `npx tsc --noEmit`
- `npx expo-doctor` quando a mudança envolver configuração ou dependências do Expo.

Rode lint e typecheck antes de concluir mudanças de código. Mudanças nativas Android também podem exigir validação em build de desenvolvimento e em dispositivo; não afirme que o comportamento do sistema foi validado apenas com checagens TypeScript.
