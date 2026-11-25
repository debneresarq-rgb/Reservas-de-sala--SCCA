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
      Para pedir menos permissão, dá para trocar `EVENTS_SCOPE` no `CONFIG` por `calendar.events.owned`
      (teste criar, editar e cancelar antes de adotar).
   5. Credenciais > Criar credenciais > **ID do cliente OAuth** > *Aplicativo da Web*.
   6. Em **Origens JavaScript autorizadas**, adicione `https://SEU-USUARIO.github.io`
      (somente o domínio, sem `/NOME-DO-REPOSITORIO` e sem barra no final).
3. **Colar o ID do cliente** no bloco `CONFIG` do `index.html` (campo `CLIENT_ID`) e fazer commit.
4. **Salas como recursos** (opcional, recomendado): Admin Console > Diretório > Edifícios e recursos.
   Cadastre as 3 salas e cole o e-mail de agenda de cada uma em `ROOM_CALENDARS`.
   Assim os horários ocupados valem para todos os colegas.

## Como achar o ID (e-mail) da agenda de cada sala

`ROOM_CALENDARS` precisa do **ID da agenda**, não do nome da sala (ex.: `c_1a2b3c...@resource.calendar.google.com`).
- **Admin Console** > Diretório > Edifícios e recursos > Recursos > abra a sala: o e-mail do recurso aparece nos detalhes.
- Ou no **Google Agenda** (do administrador): Configurações > na lista da esquerda, a sala > "Integrar agenda" > **ID da agenda**.
- Para os colegas verem o *título* das reuniões da sala (e não só "Ocupado"), a agenda da sala precisa estar
  compartilhada com a organização com "Ver todos os detalhes do evento" (Admin Console > Apps > Google Workspace > Agenda > Compartilhamento).

## Tags dos eventos

Todo evento criado leva uma tag entre colchetes no **título**, que aparece no assunto e no corpo do e-mail de convite:
`[escritorio]` (Sala SCCA Jardim Botânico e Sala SCCA Lagoa) e `[online]` (Sala SCCA). A tag também vai na descrição do evento.

## Como funciona o cancelamento

- Cancelar apaga o evento no Google Agenda (os convidados são avisados) e a reserva
  **continua na lista do site, com o selo "Cancelada"** (histórico preservado).
- Se o evento já tiver sido apagado direto no Google, o sistema apenas marca como cancelada.
- Os eventos criados pelo sistema levam uma marca interna (`sccApp`), para futuras
  sincronizações mexerem só neles.
- Cada pessoa autoriza o acesso no primeiro login. Se desmarcar a permissão do Agenda, o sistema pede que autorize de novo.

## Segurança

- Nunca coloque *client secret*, senhas ou chaves privadas no repositório. O `CLIENT_ID` é público por natureza.
- A restrição real ao domínio vem da tela de consentimento **Interna**, não do JavaScript.
- Sem `ROOM_CALENDARS`, cada pessoa só enxerga no sistema as reservas feitas no próprio navegador.
