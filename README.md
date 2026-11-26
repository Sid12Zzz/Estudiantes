# Plataforma LMS Multi-Tenant (Projeto Acadêmico & Portfólio)

Sistema de Gestão de Aprendizagem (*Learning Management System*) 24/7 desenvolvido para permitir que múltiplos professores gerenciem seus próprios alunos, distribuam videoaulas organizadas por módulos, disponibilizem apostilas em PDF e gerem quizzes avaliativos com auxílio de Inteligência Artificial.

A arquitetura foi projetada com **isolamento lógico de dados por professor (Multi-tenancy)**, **cibersegurança orientada ao padrão OWASP** e **custo operacional zero**.

---

## Arquitetura e Stack Tecnológica

| Camada | Tecnologias | Hospedagem (Free Tier) |
| :--- | :--- | :--- |
| **Frontend** | React, TypeScript, Vite, Tailwind CSS | Cloudflare Pages |
| **Backend** | Java 21, Spring Boot 3, Spring Security, Spring Data JPA, Flyway | Oracle Cloud (Always Free) / Render |
| **Banco de Dados** | PostgreSQL | Neon / Supabase |
| **Armazenamento** | Object Storage compatível com API S3 (PDFs privados) | Cloudflare R2 / Supabase Storage |
| **Integrações** | Google Gemini API (geração de quizzes), Feeds RSS (atualidades) | Serverless / Scheduled Jobs |

---

## Pilares de Cibersegurança Implementados

* **Isolamento Multi-Tenant e Prevenção de IDOR:** Todas as consultas ao banco validam obrigatoriamente o vínculo entre o `aluno_id`, `professor_id` e as matrículas ativas extraídas do token autenticado, impedindo acesso cruzado entre turmas.
* **Autenticação Blindada:** Senhas armazenadas com hash **BCrypt** (fator de custo 12) e tokens **JWT** trafegando exclusivamente via cookies `HttpOnly`, `Secure` e `SameSite=Strict` (mitigando ataques XSS e CSRF).
* **Proteção de Arquivos (Presigned URLs):** Os PDFs ficam armazenados em bucket privado. O acesso ocorre apenas via URLs temporárias assinadas pelo backend com expiração de 5 minutos após validação da matrícula.
* **Rate Limiting & Validação:** Proteção contra força bruta e esgotamento de cota de IA utilizando *Bucket4j*, validação de *magic bytes* no upload de arquivos e headers de segurança (`CSP`, `X-Frame-Options`, `CORS` restrito).

---

## Estrutura do Repositório

```text
/
├── backend/          # API RESTful em Java 21 + Spring Boot
├── frontend/         # Aplicação SPA em React + TypeScript
├── docs/             # Documentação de Engenharia de Software
│   ├── adr/          # Architecture Decision Records
│   └── diagrams/     # DER (Banco de Dados) e Fluxos de Segurança
└── docker-compose.yml
