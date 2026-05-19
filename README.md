# SCAA - Sistema de Controle de Atividades Acadêmicas

## Descrição do Projeto

O **SCAA** é um sistema desenvolvido para gerenciar atividades acadêmicas não avaliativas de estudantes universitários. O sistema permite que estudantes cadastrem suas atividades (palestras, workshops, projetos de extensão, minicursos e iniciação científica) e que coordenadores validem essas atividades, registrando as horas acumuladas.

## Estrutura do Projeto

```
scaa/
├── index.html                 # Página principal (HTML5)
├── css/
│   └── style.css             # Estilos responsivos (CSS3)
├── js/
│   └── script.js             # Lógica da aplicação (JavaScript)
├── sql/
│   └── banco_scaa.sql        # Scripts de banco de dados (MySQL)
├── diagramas/
│   ├── diagrama_classes.mmd  # Diagrama UML de Classes (Mermaid)
│   ├── diagrama_classes.png  # Diagrama UML (Imagem PNG)
│   ├── diagrama_der.mmd      # Diagrama Entidade-Relacionamento (Mermaid)
│   └── diagrama_der.png      # Diagrama DER (Imagem PNG)
└── README.md                 # Este arquivo
```

## Tecnologias Utilizadas

### Front-end
- **HTML5**: Estrutura semântica e acessível
- **CSS3**: Responsivo com Flexbox e Media Queries
- **JavaScript**: Lógica de cadastro, validação e manipulação de dados
- **Fonte**: Poppins (Google Fonts)

### Back-end (Banco de Dados)
- **MySQL**: Sistema gerenciador de banco de dados relacional
- **SQL**: Scripts de criação de tabelas e consultas

### Diagramas
- **Mermaid.js**: Diagramas UML e Entidade-Relacionamento

## Passos de Desenvolvimento

### Passo 1: Análise Orientada a Objetos
O projeto foi modelado utilizando análise orientada a objetos, identificando as seguintes classes principais:

- **Estudante**: Gerencia dados do estudante e suas atividades
- **Coordenador**: Responsável por validar atividades
- **Atividade**: Representa uma atividade acadêmica
- **TipoAtividade**: Categoriza os tipos de atividades
- **Validacao**: Registra o histórico de validações
- **SistemaAcademico**: Orquestra todo o sistema

### Passo 2: Desenvolvimento em JavaScript
O front-end foi desenvolvido em JavaScript puro, mantendo os dados em memória (array) e oferecendo as seguintes funcionalidades:

- Cadastro de atividades com validação de campos
- Visualização do histórico de atividades
- Cálculo automático de horas acumuladas (apenas atividades aprovadas)
- Remoção de atividades
- Filtro por status (Aprovada, Pendente, Recusada)

### Passo 3: Modelagem de Dados
O modelo relacional foi desenvolvido seguindo a Segunda Forma Normal (2FN), com as seguintes tabelas:

- **estudante**: Dados dos estudantes
- **coordenador**: Dados dos coordenadores
- **tipo_atividade**: Tipos de atividades disponíveis
- **atividade**: Atividades cadastradas
- **validacao**: Histórico de validações

### Passo 4: Banco de Dados (SQL)
Scripts SQL foram criados para:

- Criar o banco de dados e tabelas
- Inserir dados de exemplo (3 estudantes, 5 atividades)
- Realizar consultas para listar atividades, calcular horas e visualizar pendências

### Passo 5: Desenvolvimento Responsivo
A interface foi desenvolvida com CSS3 responsivo, garantindo:

- Layout em duas colunas no desktop (formulário + lista)
- Layout em coluna única no mobile
- Flexbox para organização de elementos
- Media queries para diferentes tamanhos de tela
- Design minimalista e moderno

## Funcionalidades Principais

### Para Estudantes
- ✅ Cadastrar novas atividades
- ✅ Visualizar histórico de atividades
- ✅ Acompanhar total de horas acumuladas
- ✅ Remover atividades
- ✅ Filtrar atividades por status

### Para Coordenadores (Estrutura pronta para expansão)
- ✅ Visualizar atividades pendentes
- ✅ Aprovar/Recusar atividades
- ✅ Adicionar observações nas validações

## Dados de Exemplo

### Estudantes Cadastrados
1. **Débora Ester Silva** - Desenvolvimento Web
2. **Thiago Henrique Souza** - Sistemas de Informação
3. **Mariana Oliveira Costa** - Análise e Desenvolvimento de Sistemas

### Tipos de Atividade
- Palestra
- Workshop
- Projeto de Extensão
- Minicurso
- Iniciação Científica

### Atividades de Exemplo
| Estudante | Tipo | Data | Horas | Status |
|-----------|------|------|-------|--------|
| Débora Ester Silva | Palestra | 2026-03-12 | 4h | APROVADA |
| Thiago Henrique Souza | Workshop | 2026-03-20 | 6h | PENDENTE |
| Mariana Oliveira Costa | Projeto de Extensão | 2026-04-05 | 12h | APROVADA |
| Débora Ester Silva | Minicurso | 2026-04-18 | 8h | RECUSADA |
| Thiago Henrique Souza | Iniciação Científica | 2026-05-02 | 10h | APROVADA |

## Como Usar

### 1. Executar o Front-end
Abra o arquivo `index.html` em um navegador web moderno (Chrome, Firefox, Safari, Edge).

### 2. Configurar o Banco de Dados
```bash
# Conecte ao MySQL
mysql -u seu_usuario -p

# Execute o script SQL
source /caminho/para/banco_scaa.sql
```

### 3. Consultas SQL Úteis

#### Listar todas as atividades com detalhes
```sql
SELECT 
    e.nome AS Nome_Estudante,
    ta.nome AS Tipo_Atividade,
    DATE_FORMAT(a.data_realizacao, '%d/%m/%Y') AS Data,
    a.status AS Status
FROM atividade a
JOIN estudante e ON a.estudante_id = e.id
JOIN tipo_atividade ta ON a.tipo_id = ta.id
ORDER BY a.data_realizacao DESC;
```

#### Total de horas por estudante (apenas aprovadas)
```sql
SELECT 
    e.nome AS Nome_Estudante,
    SUM(a.carga_horaria) AS Total_Horas
FROM estudante e
LEFT JOIN atividade a ON e.id = a.estudante_id AND a.status = 'APROVADA'
GROUP BY e.id, e.nome
ORDER BY Total_Horas DESC;
```

#### Atividades pendentes de validação
```sql
SELECT 
    a.id,
    e.nome,
    ta.nome AS Tipo,
    a.carga_horaria,
    a.data_realizacao
FROM atividade a
JOIN estudante e ON a.estudante_id = e.id
JOIN tipo_atividade ta ON a.tipo_id = ta.id
WHERE a.status = 'PENDENTE'
ORDER BY a.data_realizacao ASC;
```

## Design e Cores

### Paleta de Cores
- **Azul Escuro** (#1a365d): Cor primária (cabeçalho, títulos)
- **Azul Claro** (#3182ce): Cor secundária (botões, destaques)
- **Branco** (#ffffff): Fundo dos cards
- **Cinza Claro** (#f7fafc): Fundo geral

### Tipografia
- **Fonte Principal**: Poppins (Google Fonts)
- **Alternativa**: Arial, sans-serif

### Estilos de Status
- **Aprovada**: Verde (#48bb78)
- **Pendente**: Amarelo (#ecc94b)
- **Recusada**: Vermelho (#f56565)

## Responsividade

O sistema foi desenvolvido com abordagem **Mobile First** e é totalmente responsivo:

- **Desktop** (> 992px): Layout em duas colunas
- **Tablet** (768px - 992px): Transição gradual
- **Mobile** (< 768px): Layout em coluna única

## Diagramas

### Diagrama de Classes (UML)
Visualiza as relações entre as classes do sistema:
- Estudante, Coordenador, Atividade, TipoAtividade, Validacao, SistemaAcademico

### Diagrama Entidade-Relacionamento (DER)
Mostra a estrutura do banco de dados:
- Tabelas, campos, chaves primárias e relacionamentos

## Critérios de Avaliação

| Critério | Peso |
|----------|------|
| Execução da Proposta | 60% |
| Linguagem e Comunicação | 30% |
| Normalização | 10% |

## Notas Importantes

- O sistema atual mantém dados em memória (JavaScript). Para persistência, integre com o banco de dados MySQL.
- A validação de atividades por coordenadores pode ser expandida com uma interface administrativa.
- Considere adicionar autenticação de usuários para versões futuras.
- O sistema segue as normas ABNT para citações e referências.

## Autor

**Projeto Integrado - Inovação em Desenvolvimento Web**
Desenvolvido como parte da disciplina de Projeto Integrado

## Licença

Este projeto é fornecido para fins educacionais.
