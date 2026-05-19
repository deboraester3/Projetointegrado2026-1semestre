# Documentação Técnica - SCAA

## 1. Análise Orientada a Objetos

### 1.1 Contexto do Sistema
O Sistema de Controle de Atividades Acadêmicas (SCAA) foi desenvolvido para resolver o problema de desorganização na gestão de atividades complementares de estudantes. A universidade necessitava de um sistema centralizado que permitisse aos estudantes registrar suas atividades acadêmicas não avaliativas e aos coordenadores validar essas atividades de forma eficiente.

### 1.2 Atores Principais
- **Estudante**: Usuário que cadastra e acompanha suas atividades
- **Coordenador**: Responsável por validar as atividades dos estudantes
- **Sistema**: Gerencia dados e operações

### 1.3 Classes Identificadas

#### Classe: Estudante
```
Atributos:
- id: int (identificador único)
- nome: string
- email: string (único)
- curso: string

Métodos:
+ cadastrarAtividade(atividade: Atividade): void
+ visualizarHistorico(): Atividade[]
+ calcularTotalHoras(): float
+ removerAtividade(id: int): void
```

#### Classe: Coordenador
```
Atributos:
- id: int
- nome: string
- email: string (único)
- departamento: string

Métodos:
+ aprovarAtividade(id: int): void
+ recusarAtividade(id: int): void
+ visualizarPendencias(): Atividade[]
+ visualizarTodasAtividades(): Atividade[]
```

#### Classe: Atividade
```
Atributos:
- id: int
- estudante_id: int (chave estrangeira)
- tipo_id: int (chave estrangeira)
- data: date
- descricao: string
- carga_horaria: float
- status: string (PENDENTE, APROVADA, RECUSADA)
- data_criacao: datetime

Métodos:
+ getDetalhes(): string
+ atualizarStatus(novoStatus: string): void
+ validarDados(): boolean
```

#### Classe: TipoAtividade
```
Atributos:
- id: int
- nome: string (único)
- descricao: string

Métodos:
+ obterNome(): string
```

#### Classe: Validacao
```
Atributos:
- id: int
- atividade_id: int (chave estrangeira)
- coordenador_id: int (chave estrangeira)
- status: string
- data_validacao: datetime
- observacoes: string

Métodos:
+ registrarValidacao(status: string, observacoes: string): void
+ obterStatus(): string
```

#### Classe: SistemaAcademico
```
Atributos:
- estudantes: Estudante[]
- atividades: Atividade[]
- tipos: TipoAtividade[]
- validacoes: Validacao[]
- coordenadores: Coordenador[]

Métodos:
+ adicionarEstudante(estudante: Estudante): void
+ adicionarAtividade(atividade: Atividade): void
+ obterAtividadesPorEstudante(estudante_id: int): Atividade[]
+ obterAtividadesPendentes(): Atividade[]
+ calcularHorasTotaisPorEstudante(estudante_id: int): float
```

### 1.4 Relacionamentos
- **Estudante** → **Atividade** (1:N) - Um estudante pode ter várias atividades
- **Atividade** → **TipoAtividade** (N:1) - Várias atividades podem ser do mesmo tipo
- **Atividade** → **Validacao** (1:N) - Uma atividade pode ter várias validações
- **Coordenador** → **Validacao** (1:N) - Um coordenador realiza várias validações
- **SistemaAcademico** → Todas as classes (1:N) - O sistema gerencia todas as entidades

## 2. Modelagem de Dados (Relacional)

### 2.1 Tabelas e Campos

#### Tabela: estudante
| Campo | Tipo | Restrições |
|-------|------|-----------|
| id | INT | PRIMARY KEY, AUTO_INCREMENT |
| nome | VARCHAR(100) | NOT NULL |
| email | VARCHAR(100) | NOT NULL, UNIQUE |
| curso | VARCHAR(100) | NOT NULL |
| data_criacao | TIMESTAMP | DEFAULT CURRENT_TIMESTAMP |

#### Tabela: coordenador
| Campo | Tipo | Restrições |
|-------|------|-----------|
| id | INT | PRIMARY KEY, AUTO_INCREMENT |
| nome | VARCHAR(100) | NOT NULL |
| email | VARCHAR(100) | NOT NULL, UNIQUE |
| departamento | VARCHAR(100) | NOT NULL |
| data_criacao | TIMESTAMP | DEFAULT CURRENT_TIMESTAMP |

#### Tabela: tipo_atividade
| Campo | Tipo | Restrições |
|-------|------|-----------|
| id | INT | PRIMARY KEY, AUTO_INCREMENT |
| nome | VARCHAR(50) | NOT NULL, UNIQUE |
| descricao | VARCHAR(255) | - |

#### Tabela: atividade
| Campo | Tipo | Restrições |
|-------|------|-----------|
| id | INT | PRIMARY KEY, AUTO_INCREMENT |
| estudante_id | INT | NOT NULL, FK(estudante) |
| tipo_id | INT | NOT NULL, FK(tipo_atividade) |
| data_realizacao | DATE | NOT NULL |
| descricao | TEXT | NOT NULL |
| carga_horaria | DECIMAL(5,2) | NOT NULL |
| status | ENUM | DEFAULT 'PENDENTE' |
| data_criacao | TIMESTAMP | DEFAULT CURRENT_TIMESTAMP |

#### Tabela: validacao
| Campo | Tipo | Restrições |
|-------|------|-----------|
| id | INT | PRIMARY KEY, AUTO_INCREMENT |
| atividade_id | INT | NOT NULL, FK(atividade) |
| coordenador_id | INT | NOT NULL, FK(coordenador) |
| status_anterior | ENUM | NOT NULL |
| status_novo | ENUM | NOT NULL |
| observacoes | TEXT | - |
| data_validacao | TIMESTAMP | DEFAULT CURRENT_TIMESTAMP |

### 2.2 Normalização
O modelo foi desenvolvido seguindo a **Segunda Forma Normal (2FN)**:
- Todas as tabelas possuem chave primária
- Não há dependências parciais
- Todos os atributos não-chave dependem totalmente da chave primária
- Relacionamentos estão bem definidos através de chaves estrangeiras

## 3. Estrutura HTML

### 3.1 Elementos Principais
```html
<header>          <!-- Cabeçalho com logo e informações do usuário -->
<main>
  <section>       <!-- Seção de formulário -->
    <form>        <!-- Formulário de cadastro -->
  </section>
  <section>       <!-- Seção de listagem -->
    <table>       <!-- Tabela de atividades -->
  </section>
</main>
<footer>          <!-- Rodapé -->
```

### 3.2 Formulário de Cadastro
- Campo: Título da Atividade (text, obrigatório)
- Campo: Tipo de Atividade (select, obrigatório)
- Campo: Data de Realização (date, obrigatório)
- Campo: Carga Horária (number, obrigatório)
- Campo: Descrição (textarea, obrigatório)
- Botão: Cadastrar Atividade (submit)

### 3.3 Tabela de Atividades
Colunas:
- Data (formatada DD/MM/YYYY)
- Título / Tipo
- Carga Horária
- Status (com badge de cor)
- Ações (Remover)

## 4. Estilos CSS

### 4.1 Variáveis de Design
```css
--primary-color: #1a365d;      /* Azul escuro */
--secondary-color: #3182ce;    /* Azul claro */
--bg-color: #f7fafc;           /* Cinza muito claro */
--card-bg: #ffffff;            /* Branco */
--text-main: #2d3748;          /* Texto principal */
--border-radius: 8px;
--box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05);
```

### 4.2 Componentes Estilizados
- **Header**: Fundo azul escuro, layout flexível
- **Cards**: Sombra suave, borda arredondada, espaçamento
- **Formulário**: Campos com foco interativo, botão com hover
- **Tabela**: Linhas com hover, badges de status coloridas
- **Responsividade**: Media queries para mobile (< 768px) e tablet (768px - 992px)

### 4.3 Layout Responsivo
- **Desktop**: Formulário (flex: 1) + Lista (flex: 2) lado a lado
- **Tablet**: Ajuste de espaçamento e tamanhos
- **Mobile**: Stack vertical, elementos ocupam 100% da largura

## 5. Lógica JavaScript

### 5.1 Estrutura de Dados
```javascript
const atividades = [
    {
        id: number,
        titulo: string,
        tipo: string,
        data: string (YYYY-MM-DD),
        cargaHoraria: number,
        descricao: string,
        status: string (PENDENTE|APROVADA|RECUSADA)
    }
];
```

### 5.2 Funções Principais

#### cadastrarAtividade(e)
- Captura dados do formulário
- Valida campos obrigatórios
- Cria objeto de atividade
- Adiciona ao array
- Atualiza interface
- Limpa formulário

#### removerAtividade(id)
- Confirma remoção com usuário
- Remove do array
- Atualiza interface

#### renderizarAtividades()
- Filtra por status se necessário
- Ordena por data (mais recente primeiro)
- Cria linhas da tabela dinamicamente
- Formata datas
- Aplica classes CSS de status

#### atualizarEstatisticas()
- Calcula total de atividades
- Calcula total de horas (apenas aprovadas)
- Atualiza elementos DOM

### 5.3 Event Listeners
- `DOMContentLoaded`: Inicializa a aplicação
- `form#form-atividade submit`: Cadastra nova atividade
- `select#filtro-status change`: Filtra atividades

## 6. Consultas SQL

### 6.1 Listar todas as atividades com detalhes
```sql
SELECT 
    e.nome AS Nome_Estudante,
    ta.nome AS Tipo_Atividade,
    DATE_FORMAT(a.data_realizacao, '%d/%m/%Y') AS Data_Realizacao,
    a.status AS Status_Validacao
FROM 
    atividade a
JOIN 
    estudante e ON a.estudante_id = e.id
JOIN 
    tipo_atividade ta ON a.tipo_id = ta.id
ORDER BY 
    a.data_realizacao DESC;
```

### 6.2 Total de horas por estudante (aprovadas)
```sql
SELECT 
    e.nome AS Nome_Estudante,
    e.curso AS Curso,
    SUM(a.carga_horaria) AS Total_Horas_Aprovadas
FROM 
    estudante e
LEFT JOIN 
    atividade a ON e.id = a.estudante_id AND a.status = 'APROVADA'
GROUP BY 
    e.id, e.nome, e.curso
ORDER BY 
    Total_Horas_Aprovadas DESC;
```

### 6.3 Atividades pendentes de validação
```sql
SELECT 
    a.id AS ID_Atividade,
    e.nome AS Nome_Estudante,
    ta.nome AS Tipo_Atividade,
    a.descricao AS Descricao,
    a.carga_horaria AS Horas,
    DATE_FORMAT(a.data_realizacao, '%d/%m/%Y') AS Data
FROM 
    atividade a
JOIN 
    estudante e ON a.estudante_id = e.id
JOIN 
    tipo_atividade ta ON a.tipo_id = ta.id
WHERE 
    a.status = 'PENDENTE'
ORDER BY 
    a.data_realizacao ASC;
```

## 7. Fluxo de Funcionamento

### 7.1 Cadastro de Atividade
1. Estudante preenche formulário
2. JavaScript valida dados
3. Objeto é criado e adicionado ao array
4. Interface é atualizada
5. Atividade aparece na tabela com status PENDENTE

### 7.2 Visualização e Filtragem
1. Usuário seleciona filtro de status
2. JavaScript filtra o array
3. Tabela é renderizada com atividades filtradas
4. Estatísticas são atualizadas

### 7.3 Remoção de Atividade
1. Usuário clica em "Remover"
2. Confirmação é solicitada
3. Atividade é removida do array
4. Interface é atualizada

## 8. Integração com Banco de Dados

Para integrar o front-end com o banco de dados MySQL:

1. Criar um backend (PHP, Node.js, Python, etc.)
2. Implementar endpoints para:
   - POST /atividades (cadastrar)
   - GET /atividades (listar)
   - DELETE /atividades/:id (remover)
   - PUT /atividades/:id (atualizar status)
3. Modificar JavaScript para fazer requisições AJAX/Fetch
4. Validar dados no servidor antes de salvar

## 9. Considerações de Segurança

- Validar todos os dados no servidor
- Usar prepared statements para evitar SQL injection
- Implementar autenticação e autorização
- Sanitizar entrada do usuário
- Usar HTTPS em produção
- Implementar rate limiting

## 10. Possibilidades de Expansão

- Autenticação de usuários
- Painel administrativo para coordenadores
- Relatórios em PDF
- Exportação de dados em Excel
- Notificações por email
- Sistema de pontuação/ranking
- Integração com calendário
- Backup automático de dados
