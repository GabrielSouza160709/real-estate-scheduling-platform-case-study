# Agenda Polo — Plataforma de Agendamentos

Agenda Polo é uma aplicação interna para reservar uma sala de reuniões da Polo Negócios Imobiliários, em Uberlândia, Minas Gerais. O sistema centraliza horários, evita sobreposições e oferece controles administrativos. O código permanece privado. A aplicação partiu de uma base educacional e foi transformada para as regras e a identidade da empresa.

> O código-fonte e os dados deste projeto permanecem privados por confidencialidade e propriedade intelectual. Este repositório apresenta somente um estudo de caso sobre a aplicação, suas adaptações e as decisões adotadas.

## Visão geral

Em uma operação imobiliária, organizar espaços por mensagens dificulta a consulta de horários e pode gerar reservas concorrentes.

A solução reúne a agenda da sala em uma interface restrita. Após entrar com uma conta Google autorizada, a equipe pode consultar, criar, editar e cancelar reservas dentro do horário operacional.

O projeto não sincroniza com Google Calendar. Google OAuth é usado somente no login; a antiga integração foi removida.

## Problema abordado

- Falta de uma visão única da ocupação da sala.
- Risco de reservas simultâneas.
- Trocas manuais para confirmar disponibilidade.
- Necessidade de limitar o acesso e diferenciar usuários comuns de administradores.
- Interface e regras adequadas à operação imobiliária.

## Objetivos do produto

- Centralizar a disponibilidade da sala.
- Permitir que usuários autorizados gerenciem suas reservas.
- Impedir sobreposição de horários também no banco de dados.
- Restringir reservas ao período operacional definido.
- Proteger detalhes de compromissos marcados como privados.
- Oferecer gestão e indicadores aos administradores.
- Manter uma experiência responsiva e alinhada à identidade da Polo.

## Origem e evolução do projeto

A aplicação partiu do projeto educacional [06-ignite-call, da Rocketseat Education](https://github.com/rocketseat-education/06-ignite-call), um fluxo público de agendamentos com Next.js, autenticação, disponibilidade configurável e Google Calendar. A base forneceu estruturas para autenticação, persistência e datas.

O README oficial declara licença MIT. Na verificação deste estudo, porém, o arquivo `LICENSE` referenciado não estava presente e o GitHub não detectava uma licença. A origem é reconhecida explicitamente; nenhum código é reproduzido aqui.

### Base inicial

A base incluía Next.js com Pages Router, TypeScript, rotas de servidor, Prisma, NextAuth, formulários validados, disponibilidade, página pública e integração com Google Calendar.

### Adaptações realizadas

- Substituição do fluxo público de agendamentos por uma agenda interna de sala.
- Atualização de Next.js, React e Prisma, com migração para PostgreSQL.
- Remoção da integração com Google Calendar, mantendo OAuth apenas para login.
- Troca da camada visual original por Tailwind CSS, componentes baseados em Radix UI e Schedule-X.
- Identidade visual da Polo, responsividade e tema claro/escuro.
- Redirecionamento das telas legadas para o fluxo atual da agenda.

### Funcionalidades adicionadas

- Reservas internas com criação, edição, cancelamento e opção de privacidade.
- Controle de acesso por lista de e-mails, estado ativo da conta e papéis `USER` e `ADMIN`.
- Área “Minhas Reservas”, gestão administrativa de usuários e painel de métricas.
- Restrição de horário entre 06:00 e 20:00 e prevenção de conflitos no banco.
- Notificações por e-mail quando o ambiente SMTP está configurado.
- Configuração de implantação na Railway e health check.

## Estado atual

### Implementado

Autenticação restrita; calendário em visões de dia, semana e mês; gestão de reservas; privacidade; prevenção de sobreposição; área pessoal; gestão de usuários; métricas; notificações SMTP opcionais; tema persistente e responsividade.

### Em desenvolvimento ou validação

O MVP foi validado localmente e possui configuração de implantação, mas não há evidência pública de operação contínua em produção. Segurança, e-mails e experiência em dispositivos seguem sujeitos a validação.

### Planejado

Não há um roadmap formal documentado. Próximas melhorias devem ser definidas a partir do uso real e das validações da equipe.

## Principais funcionalidades

- **Acesso autenticado:** login com Google OAuth, allowlist e bloqueio de contas inativas.
- **Agenda centralizada:** Schedule-X com diferentes visualizações.
- **Gestão de reservas:** criação, alteração e cancelamento lógico.
- **Privacidade:** título e observações de reservas privadas ficam ocultos para outros usuários.
- **Proteção contra conflitos:** validações na aplicação e restrição de exclusão no PostgreSQL.
- **Administração:** papéis, ativação de contas, usuários e indicadores.
- **Notificações:** avisos de criação, alteração e cancelamento quando o SMTP está disponível.

Não fazem parte do fluxo atual uma página pública para visitantes, configuração de disponibilidade individual, sincronização com Google Calendar, criação automática de Google Meet ou reagendamento por link externo.

## Fluxo principal

1. Um usuário previamente autorizado entra com sua conta Google.
2. O sistema verifica a allowlist e o estado da conta.
3. A agenda exibe as reservas e a situação atual da sala.
4. O usuário escolhe data, início, término, título, observações e nível de privacidade.
5. A aplicação valida os dados e o período operacional.
6. O banco impede a criação de uma reserva sobreposta.
7. A reserva passa a aparecer na agenda e na área do responsável.
8. Alterações ou cancelamentos atualizam a agenda e podem gerar notificações por e-mail.

## Tecnologias utilizadas

### Frontend

Next.js 15 com Pages Router, React 19, TypeScript, Tailwind CSS, Schedule-X, Radix UI e React Query.

### Interface e formulários

React Hook Form, Zod e Day.js, com tema claro e escuro.

### Backend e persistência

API Routes, Prisma 6 e PostgreSQL, com validação e restrição de sobreposição.

### Autenticação e comunicação

NextAuth v4, Google OAuth para login e Nodemailer para notificações SMTP opcionais.

### Infraestrutura

Docker para o banco local e configuração de build, migração e health check para Railway, sem afirmar produção ativa.

## Arquitetura conceitual

A interface e as rotas de servidor ficam no Next.js. A sessão identifica o usuário, rotas protegidas validam acesso e dados, e o Prisma persiste reservas no PostgreSQL. A agenda oculta detalhes privados conforme a permissão; operações administrativas permanecem separadas.

## Decisões de produto e engenharia

### Transformação de uma base existente

A base permitiu concentrar o trabalho na transformação para um fluxo interno e nas regras da empresa.

### Conflito tratado em mais de uma camada

Interface, servidor e PostgreSQL atuam em conjunto contra reservas simultâneas.

### Acesso restrito desde o login

OAuth, allowlist, estado ativo e papéis limitam o uso a pessoas autorizadas.

### Privacidade proporcional ao contexto

Reservas privadas ocultam conteúdo sem esconder a ocupação da sala.

## Segurança e privacidade

Os controles incluem OAuth, rotas protegidas, Zod, autorização por sessão, papéis, allowlist e segredos fora do código público. Tokens e credenciais permanecem privados. Esses controles reduzem riscos, sem representar garantia absoluta.

## Desafios do projeto

- Compreender e transformar uma base educacional sem preservar fluxos incompatíveis com a operação.
- Modelar regras de reserva e impedir sobreposições inclusive em acessos concorrentes.
- Separar informações privadas em uma agenda compartilhada.
- Atualizar a stack e personalizar a interface sem perder consistência funcional.

## Resultados e benefícios

O resultado organiza a consulta da sala e o registro de compromissos, reduz etapas manuais e a possibilidade de conflito, além de oferecer uma experiência consistente e uma visão administrativa da operação.

## Minha participação

Fui responsável por analisar a base, adaptá-la ao contexto da Polo e evoluí-la para uma agenda interna. O trabalho abrangeu fluxo, stack, identidade visual, reservas, regras de horário e conflito, acesso, administração, notificações e implantação. A aplicação não é apresentada como criada integralmente do zero.

## Status do projeto

**MVP funcional em evolução.** O fluxo principal foi implementado e validado localmente. A utilização contínua em produção não é afirmada neste estudo de caso por falta de evidência pública suficiente.

## Próximas etapas

Sem roadmap formal, os próximos passos são validar o uso, ampliar testes, observar notificações, revisar segurança e priorizar feedback real.

## Confidencialidade

Código, credenciais, dados de calendário e informações pessoais permanecem privados. Este repositório serve ao portfólio e não expõe detalhes operacionais. Imagens só poderão ser adicionadas após revisão e anonimização. A base foi reconhecida, sem publicação do histórico privado.

## Contato

[Gabriel Souza](https://www.linkedin.com/in/gabriel-souza-0071103a4/)
