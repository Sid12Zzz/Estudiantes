# ADR 002: Isolamento Multi-Tenant em Banco Compartilhado com Chaves UUIDv4

* **Status:** Aceito
* **Data:** 2026-11-26

## Contexto
Múltiplos professores utilizarão a mesma instância da aplicação e o mesmo banco de dados gratuito (PostgreSQL), mas cada docente deve gerenciar exclusivamente seus próprios alunos e conteúdos, sem vazamento de dados entre turmas (conformidade com a LGPD).

## Decisão
1. **Isolamento Lógico (Shared Database, Shared Schema):** Todas as tabelas de domínio vinculam-se hierarquicamente ao `professor_id`. Toda consulta no backend exige filtragem obrigatória pelo identificador do professor extraído do contexto de segurança (JWT), nunca confiando apenas em parâmetros enviados pela URL.
2. **Chaves Primárias em UUIDv4:** Em vez de chaves inteiras sequenciais (`1, 2, 3`), todas as entidades utilizam `UUID`, mitigando ataques de enumeração de recursos (IDOR).

## Consequências
* **Positivas:** Viabiliza o uso de uma única instância gratuita de banco PostgreSQL para dezenas de professores e elimina ataques triviais de enumeração de IDs.
* **Negativas:** Exige rigor nos testes de integração (`MockMvc`) para garantir que nenhum endpoint omita a cláusula de filtragem por tenant/matrícula.