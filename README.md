# Agenda Polo — Plataforma de Agendamentos

> Estudo de caso técnico. O código-fonte e os dados permanecem privados por confidencialidade e propriedade intelectual.

**O problema:** organizar a sala de reuniões por mensagens dificulta consultar horários e gera reservas concorrentes.

**A solução:** uma agenda interna com acesso restrito, onde a equipe consulta, cria, edita e cancela reservas dentro do horário operacional — com prevenção de conflitos garantida em três camadas.

**Stack:** Next.js 15 · React 19 · TypeScript · Prisma 6 · PostgreSQL · NextAuth · Tailwind CSS · Railway

**Uso:** em uso pela equipe da Polo Negócios Imobiliários (Uberlândia, MG) desde Junho/2026, com 4 usuários ativos.

**Status:** em produção, em evolução contínua.

---

## Origem do projeto

A aplicação partiu do projeto educacional [06-ignite-call, da Rocketseat Education](https://github.com/rocketseat-education/06-ignite-call) — um fluxo público de agendamentos com Next.js, autenticação, disponibilidade configurável e Google Calendar. A base forneceu estruturas de autenticação, persistência e manipulação de datas.

O README oficial declara licença MIT, mas na verificação deste estudo o arquivo `LICENSE` não estava presente e o GitHub não detectava licença. A origem é reconhecida explicitamente e nenhum código é reproduzido aqui.

### O que foi transformado

- Fluxo público de agendamentos substituído por agenda interna de sala
- Stack atualizada (Next.js, React, Prisma) e migração para PostgreSQL
- Integração com Google Calendar removida; OAuth mantido apenas no login
- Camada visual original trocada por Tailwind CSS, Radix UI e Schedule-X
- Identidade visual da empresa, responsividade e tema claro/escuro
- Telas legadas redirecionadas para o fluxo atual

### O que foi construído do zero

- Reservas internas com criação, edição, cancelamento e opção de privacidade
- Controle de acesso por allowlist de e-mails, estado ativo da conta e papéis `USER` / `ADMIN`
- Área "Minhas Reservas", gestão administrativa de usuários e painel de métricas
- Restrição de horário operacional (06:00–20:00) e prevenção de conflitos no banco
- Notificações por e-mail via SMTP
- Deploy na Railway com health check

---

## Funcionalidades

- **Acesso autenticado** — Google OAuth, allowlist e bloqueio de contas inativas
- **Agenda centralizada** — Schedule-X com visões de dia, semana e mês
- **Gestão de reservas** — criação, alteração e cancelamento lógico
- **Privacidade** — título e observações de reservas privadas ficam ocultos para os demais, sem esconder a ocupação da sala
- **Proteção contra conflitos** — validação na interface, no servidor e constraint no PostgreSQL
- **Administração** — papéis, ativação de contas, gestão de usuários e indicadores
- **Notificações** — avisos de criação, alteração e cancelamento quando o SMTP está configurado

Fora do escopo atual: página pública para visitantes, disponibilidade individual, sincronização com Google Calendar, Google Meet automático e reagendamento por link externo.

---

## Tecnologias

**Frontend** — Next.js 15 (Pages Router), React 19, TypeScript, Tailwind CSS, Schedule-X, Radix UI, React Query
**Formulários** — React Hook Form, Zod, Day.js
**Backend** — API Routes, Prisma 6, PostgreSQL
**Autenticação** — NextAuth v4, Google OAuth, Nodemailer
**Infraestrutura** — Docker para banco local, Railway com build, migração e health check

---

## Decisão técnica em destaque: conflito tratado em três camadas

Uma agenda compartilhada tem uma condição de corrida óbvia: dois usuários reservando o mesmo horário simultaneamente. Validar só na interface não resolve, e validar só no servidor ainda deixa janela entre a checagem e a escrita.

A solução combina as três: a interface impede a seleção, o servidor valida a requisição e o PostgreSQL rejeita a escrita por constraint. A última camada é a que garante correção sob concorrência real.

---

## Desafios

- Transformar uma base educacional sem preservar fluxos incompatíveis com a operação
- Modelar regras de reserva e impedir sobreposição em acessos concorrentes
- Separar informação privada em uma agenda compartilhada
- Atualizar a stack e personalizar a interface sem perder consistência funcional

---

## Segurança e privacidade

OAuth, rotas protegidas, validação com Zod, autorização por sessão, papéis, allowlist e segredos fora do código público. Esses controles reduzem riscos, sem representar garantia absoluta.

---

## Minha participação

Análise da base existente, adaptação ao contexto da empresa e evolução até a agenda interna atual. O trabalho abrangeu fluxo, stack, identidade visual, regras de reserva e conflito, controle de acesso, administração, notificações e implantação. A aplicação não é apresentada como criada integralmente do zero.

---

**Gabriel Souza** — [LinkedIn](https://www.linkedin.com/in/gabrielbsouza16/) · [GitHub](https://github.com/GabrielSouza160709)
