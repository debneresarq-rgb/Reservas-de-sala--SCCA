# SCC Room Booking

Sistema de reserva de salas do escritório SCC (Sala SCCA, Jardim Botânico e Lagoa).
Página única (`index.html`) hospedada no GitHub Pages, com login Google (`@scca.com.br`)
e criação de eventos no Google Agenda (os convites saem por e-mail pelo próprio Google).

## Estrutura do repositório

```
/
├── index.html   # o sistema inteiro (HTML + CSS + JavaScript + imagens embutidas)
└── README.md    # este guia
```

## Configuração (uma vez)

1. **Publicar no GitHub Pages**: Settings > Pages > *Deploy from a branch* > `main` / `(root)`.
   O endereço será `https://SEU-USUARIO.github.io/NOME-DO-REPOSITORIO/`.
2. **Google Cloud Console** (https://console.cloud.google.com), com a conta de administrador do Workspace:
   1. Crie um projeto.
   2. APIs e serviços > Biblioteca > ative **Google Calendar API**.
   3. Tela de consentimento OAuth > tipo de usuário **Interno**.
   4. Escopos: `calendar.events` e `calendar.freebusy` (além de openid, email e profile).
   5. Credenciais > Criar credenciais > **ID do cliente OAuth** > *Aplicativo da Web*.
   6. Em **Origens JavaScript autorizadas**, adicione `https://SEU-USUARIO.github.io`
      (somente o domínio, sem `/NOME-DO-REPOSITORIO` e sem barra no final).
3. **Colar o ID do cliente** no bloco `CONFIG` do `index.html` (campo `CLIENT_ID`) e fazer commit.
4. **Salas como recursos** (opcional, recomendado): Admin Console > Diretório > Edifícios e recursos.
   Cadastre as 3 salas e cole o e-mail de agenda de cada uma em `ROOM_CALENDARS`.
   Assim os horários ocupados valem para todos os colegas.

## Segurança

- Nunca coloque *client secret*, senhas ou chaves privadas no repositório. O `CLIENT_ID` é público por natureza.
- A restrição real ao domínio vem da tela de consentimento **Interna**, não do JavaScript.
- Sem `ROOM_CALENDARS`, cada pessoa só enxerga no sistema as reservas feitas no próprio navegador.
