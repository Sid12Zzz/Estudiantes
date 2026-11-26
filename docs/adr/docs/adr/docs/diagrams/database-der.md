# Diagrama Entidade-Relacionamento (DER)

Modelagem relacional do banco PostgreSQL projetada para isolamento multi-tenant, controle granular de matrículas por matéria e fluxo de revisão de quizzes gerados por IA.

```mermaid
erDiagram
    USUARIOS ||--o| PROFESSORES : "possui perfil"
    USUARIOS ||--o| ALUNOS : "possui perfil"
    PROFESSORES ||--o{ ALUNOS : "vincula"
    PROFESSORES ||--|{ MATERIAS : "gerencia"
    ALUNOS ||--o{ MATRICULAS : "possui"
    MATERIAS ||--o{ MATRICULAS : "autoriza"
    MATERIAS ||--o{ MODULOS : "organiza"
    MODULOS ||--o{ AULAS : "contem"
    AULAS ||--o| QUIZZES : "gera"
    QUIZZES ||--|{ QUESTOES : "composto por"
    ALUNOS ||--o{ TENTATIVAS_ALUNOS : "responde"
    QUIZZES ||--o{ TENTATIVAS_ALUNOS : "registra"

    USUARIOS {
        uuid id PK
        varchar nome
        varchar email UK
        varchar senha_hash
        varchar role "ADMIN | PROFESSOR | ALUNO"
        boolean ativo
        timestamp criado_em
    }

    PROFESSORES {
        uuid id PK
        uuid usuario_id FK
        varchar codigo_convite UK
    }

    ALUNOS {
        uuid id PK
        uuid usuario_id FK
        uuid professor_id FK
    }

    MATERIAS {
        uuid id PK
        uuid professor_id FK
        varchar nome "INGLES | MATEMATICA | PORTUGUES"
    }

    MATRICULAS {
        uuid id PK
        uuid aluno_id FK
        uuid materia_id FK
        timestamp liberado_em
    }

    MODULOS {
        uuid id PK
        uuid materia_id FK
        varchar titulo
        int ordem
    }

    AULAS {
        uuid id PK
        uuid modulo_id FK
        varchar titulo
        varchar youtube_video_id
        text texto_base
        varchar pdf_storage_key
        int ordem
    }

    QUIZZES {
        uuid id PK
        uuid aula_id FK
        varchar titulo
        varchar status "RASCUNHO | PUBLICADO"
    }

    QUESTOES {
        uuid id PK
        uuid quiz_id FK
        text enunciado
        jsonb alternativas
        char resposta_correta
        text explicacao
    }

    TENTATIVAS_ALUNOS {
        uuid id PK
        uuid aluno_id FK
        uuid quiz_id FK
        numeric nota
        timestamp realizado_em
    }