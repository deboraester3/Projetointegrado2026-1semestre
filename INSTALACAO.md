# Guia de Instalação - SCAA

## Pré-requisitos

- Navegador web moderno (Chrome, Firefox, Safari, Edge)
- MySQL Server (para usar o banco de dados)
- Editor de texto ou IDE (VS Code, Sublime Text, etc.)

## Instalação do Front-end

### Passo 1: Extrair os arquivos
Extraia os arquivos do projeto para uma pasta em seu computador.

### Passo 2: Abrir no navegador
1. Navegue até a pasta do projeto
2. Clique duas vezes no arquivo `index.html`
3. O sistema abrirá no seu navegador padrão

**Alternativa**: Use um servidor web local
```bash
# Com Python 3
python -m http.server 8000

# Com Node.js
npx http-server

# Com PHP
php -S localhost:8000
```

Então acesse: `http://localhost:8000`

## Instalação do Banco de Dados

### Passo 1: Conectar ao MySQL
```bash
mysql -u seu_usuario -p
```

### Passo 2: Executar o script SQL
```bash
source /caminho/para/banco_scaa.sql
```

Ou copie e cole o conteúdo do arquivo `sql/banco_scaa.sql` no MySQL Workbench ou phpMyAdmin.

### Passo 3: Verificar a instalação
```sql
USE scaa_db;
SHOW TABLES;
SELECT * FROM estudante;
```

## Estrutura de Diretórios

```
scaa/
├── index.html                      # Página principal
├── css/
│   └── style.css                  # Estilos
├── js/
│   └── script.js                  # Lógica
├── sql/
│   └── banco_scaa.sql             # Banco de dados
├── diagramas/
│   ├── diagrama_classes.mmd       # Diagrama UML (Mermaid)
│   ├── diagrama_classes.png       # Diagrama UML (PNG)
│   ├── diagrama_der.mmd           # Diagrama DER (Mermaid)
│   └── diagrama_der.png           # Diagrama DER (PNG)
├── README.md                       # Documentação principal
├── DOCUMENTACAO_TECNICA.md        # Documentação técnica
└── INSTALACAO.md                  # Este arquivo
```

## Usando o Sistema

### 1. Cadastrar uma Atividade
1. Preencha todos os campos do formulário
2. Clique em "Cadastrar Atividade"
3. A atividade aparecerá na tabela com status PENDENTE

### 2. Visualizar Atividades
- As atividades aparecem na tabela abaixo do formulário
- Use o filtro de status para visualizar apenas atividades específicas

### 3. Remover uma Atividade
1. Clique no botão "Remover" na linha da atividade
2. Confirme a remoção na caixa de diálogo
3. A atividade será removida da lista

### 4. Acompanhar Horas
- O total de horas acumuladas é exibido no card "Total de Horas"
- Apenas atividades com status APROVADA são contabilizadas

## Dados de Exemplo

O sistema vem com dados pré-carregados:

**Estudantes:**
- Débora Ester Silva
- Thiago Henrique Souza
- Mariana Oliveira Costa

**Atividades Iniciais:**
1. Palestra sobre inovação (4h) - APROVADA
2. Minicurso de JavaScript (8h) - RECUSADA

## Troubleshooting

### Problema: Página em branco
**Solução**: Verifique se o arquivo `index.html` está no mesmo diretório que as pastas `css` e `js`.

### Problema: Estilos não carregam
**Solução**: Verifique se o arquivo `css/style.css` existe e está no caminho correto.

### Problema: Funcionalidades não funcionam
**Solução**: Abra o console do navegador (F12) e verifique se há erros de JavaScript.

### Problema: Banco de dados não conecta
**Solução**: Verifique se o MySQL está rodando e se as credenciais estão corretas.

## Próximos Passos

1. **Integrar com Backend**: Conecte o front-end com um servidor backend
2. **Implementar Autenticação**: Adicione login de usuários
3. **Painel de Coordenador**: Crie interface para validação de atividades
4. **Relatórios**: Gere relatórios em PDF ou Excel
5. **Deploy**: Hospede em um servidor web

## Suporte

Para dúvidas ou problemas, consulte:
- `README.md` - Documentação geral
- `DOCUMENTACAO_TECNICA.md` - Detalhes técnicos
- Comentários no código-fonte
