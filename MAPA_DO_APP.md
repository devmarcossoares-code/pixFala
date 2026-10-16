# Mapa do app PixFala

Use este mapa para localizar o ponto de entrada de uma tarefa. Siga imports e dependências diretamente relacionadas; o mapa não substitui a leitura do código nem justifica abrir o repositório inteiro.

## Rotas e telas

As rotas Expo Router ficam em `app/` (não em `src/app/`). Código compartilhado fica em `components/`, `services/`, `utils/` e `constants/`.

| Área | Ponto de entrada | Dependências mais prováveis |
|---|---|---|
| Navegação global | `app/_layout.tsx` | `app/(tabs)/_layout.tsx` |
| Início, monitoramento e cobrança | `app/(tabs)/index.tsx` | `components/PermissionSetupModal.tsx`, `components/StatusToggle.tsx`, `components/PixChargeGenerator.tsx`, `services/bankNotificationListener.ts` |
| Configurações, chave Pix, fala e produtos | `app/(tabs)/configuracoes.tsx` | `components/PixChargeSettings.tsx`, `services/pixChargeService.ts`, `services/speechSettingsService.ts`, `services/savedProductsService.ts` |
| Relatórios | `app/(tabs)/relatorio.tsx` | `services/pixStorage.ts`; PDF via `expo-print`/`expo-sharing` |
| Turbine seu Negócio | `app/(tabs)/turbine.tsx` | Confira imports da tela antes de alterar |
| Tema e bancos | `constants/theme.ts`, `constants/banks.ts` | Componentes/serviços que consomem os valores |

## Fluxos que cruzam módulos

- **Recebimento Pix e fala:** `services/bankNotificationListener.ts` coordena `services/bankSelection.ts`, `utils/bankPixParser.ts`, `services/ttsService.ts` e `services/pixStorage.ts`, além do módulo Android. Para persistência, deduplicação, fallback e ordem do fluxo, consulte a seção de notificações em `ESPECIFICACAO_TECNICA_PIXFALA.md`.
- **Cobrança Pix:** `services/pixChargeService.ts`, `components/PixChargeGenerator.tsx`, `components/PixChargeSettings.tsx`, `services/savedProductsService.ts` e `utils/pixFormatter.ts`.
- **Configuração da fala:** `services/speechSettingsService.ts`, `services/ttsService.ts` e `utils/ttsFormatter.ts`; alterações de comportamento Android podem envolver também a fila nativa.
- **Relatórios:** `services/pixStorage.ts` e `app/(tabs)/relatorio.tsx`; exportação PDF usa `expo-print`/`expo-sharing`.

## Módulo Expo local para Android

O listener de notificações e a fila de fala são específicos do Android. Ao alterar a integração, verifique o contrato TypeScript, o módulo Kotlin, a configuração do plugin e seus consumidores JavaScript em conjunto.

- Serviço de notificações: `modules/nubank-notification-listener/android/src/main/java/expo/modules/nubanknotificationlistener/NubankNotificationListenerService.kt`
- Fila persistente/TTS: `modules/nubank-notification-listener/android/src/main/java/expo/modules/nubanknotificationlistener/PixSpeechQueue.kt`
- Ponte/API nativa: `modules/nubank-notification-listener/android/src/main/java/expo/modules/nubanknotificationlistener/NubankNotificationListenerModule.kt`
- Serviço foreground: `modules/nubank-notification-listener/android/src/main/java/expo/modules/nubanknotificationlistener/PixFalaForegroundService.kt`
- Tipos TypeScript: `modules/nubank-notification-listener/src/NubankNotificationListener.types.ts`
- Configuração do prebuild: `plugins/withNubankNotificationListener.js`

## Fonte de verdade

Este mapa é um índice, não uma especificação: nomes, responsabilidades e fluxos podem mudar. Confirme no código antes de implementar; mantenha o mapa atualizado se a estrutura mudar. A especificação técnica documenta detalhes comportamentais que não estão reproduzidos aqui.
