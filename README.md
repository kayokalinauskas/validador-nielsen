# Validador Nielsen

Aplicação web para importar, validar e explorar arquivos de vendas Nielsen em formato TXT posicional. O processamento acontece integralmente no navegador: o arquivo selecionado não é enviado a servidores, e os resultados ficam disponíveis imediatamente para consulta.

[![HTML5](https://img.shields.io/badge/HTML5-E34F26?logo=html5&logoColor=white)](https://developer.mozilla.org/pt-BR/docs/Web/HTML)
[![CSS3](https://img.shields.io/badge/CSS3-1572B6?logo=css3&logoColor=white)](https://developer.mozilla.org/pt-BR/docs/Web/CSS)
[![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?logo=javascript&logoColor=black)](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript)
[![Sem dependências](https://img.shields.io/badge/depend%C3%AAncias-zero-2ea44f)](#tecnologias-e-decisões-técnicas)

## Sobre o projeto

O Validador Nielsen transforma um arquivo de largura fixa em uma interface de análise simples e responsiva. A solução foi construída para reduzir o trabalho manual de conferência: ela separa registros válidos e inválidos, converte valores numéricos, calcula o total vendido e oferece recursos de busca e organização dos dados.

O projeto demonstra, em uma implementação compacta e sem frameworks, conhecimentos de manipulação de arquivos, parsing de dados, gerenciamento de estado, renderização eficiente do DOM e criação de interfaces orientadas à produtividade.

## Funcionalidades

- Importação de arquivos `.txt` por meio da API `FileReader`;
- parsing de registros posicionais de largura fixa;
- validação dos campos essenciais e separação das linhas inválidas;
- exibição do motivo da rejeição junto à linha original;
- filtro por loja e busca independente em cada coluna;
- ordenação crescente e decrescente pelos cabeçalhos;
- paginação com 20 registros por página;
- totalização dinâmica dos valores após a aplicação dos filtros;
- formatação monetária em real brasileiro (`pt-BR`);
- tabela com cabeçalho fixo e navegação horizontal em telas menores;
- processamento local, sem backend e sem envio do arquivo para serviços externos.

## Como executar

O projeto não exige instalação de pacotes ou processo de build.

1. Clone o repositório:

   ```bash
   git clone https://github.com/kayokalinauskas/validador-nielsen.git
   ```

2. Acesse a pasta do projeto:

   ```bash
   cd validador-nielsen
   ```

3. Abra o arquivo `index.html` em um navegador moderno.

Também é possível servir a pasta com uma extensão como Live Server ou com qualquer servidor HTTP estático.

## Como usar

1. Clique em **Arquivo Nielsen (.txt)** e selecione o arquivo de entrada.
2. Consulte os registros válidos na tabela.
3. Use o seletor para visualizar apenas uma loja específica.
4. Digite nos campos dos cabeçalhos para combinar filtros entre colunas.
5. Clique no nome de uma coluna para alternar sua ordenação.
6. Caso existam registros rejeitados, clique em **Ver Linhas Inválidas** para conferir o motivo e o conteúdo original.

O card de total é recalculado sobre o conjunto filtrado. Assim, ele apresenta o total geral, o total da loja selecionada ou o total correspondente à combinação atual de filtros.

## Formato esperado do arquivo

Cada linha representa um registro e é interpretada por posições fixas:

| Campo | Posições (base 1) | Tratamento |
| --- | ---: | --- |
| Loja | 1–10 | Remove espaços e zeros à esquerda |
| GTIN | 11–24 | Texto |
| Produto | 25–94 | Texto |
| Semana | 95–100 | Texto |
| Quantidade vendida | 101–109 | Divide o valor bruto por 1.000 |
| Valor vendido | 110–120 | Divide o valor bruto por 100 |
| Data inicial | 122–123 | Texto |
| Data final | 124–125 | Texto |

Um registro é considerado válido quando possui loja, GTIN e produto preenchidos e valor vendido maior que zero. Linhas vazias são ignoradas. A posição 121 não é utilizada pelo parser atual.

## Fluxo da aplicação

```text
Arquivo TXT
    │
    ▼
Leitura local com FileReader
    │
    ▼
Parsing posicional e conversão de valores
    │
    ├── Registro inválido ──► painel de inconsistências
    │
    └── Registro válido ───► filtros ─► ordenação ─► paginação
                                      │
                                      └────────────► total dinâmico
```

## Tecnologias e decisões técnicas

- **HTML5:** estrutura e controles nativos de seleção de arquivo;
- **CSS3:** layout responsivo, propriedades customizadas, tabela com cabeçalho fixo e estados visuais;
- **JavaScript (Vanilla):** parsing, validação, estado, filtros, ordenação, paginação e renderização;
- **APIs Web:** `FileReader`, `Intl.NumberFormat`, `Set` e `DocumentFragment`;
- **zero dependências:** entrega simples, sem instalação, compilação ou runtime no servidor;
- **processamento no cliente:** mantém o fluxo rápido e evita a transferência do arquivo analisado.

O estado da interface é centralizado em um único objeto. As etapas de filtro, ordenação, totalização e renderização são separadas em funções específicas, tornando o fluxo mais fácil de entender e evoluir. Na montagem da tabela, um `DocumentFragment` reduz alterações repetidas no DOM.

## Estrutura do projeto

```text
validador-nielsen/
├── index.html   # interface, estilos e lógica da aplicação
└── README.md    # documentação do projeto
```

## Possíveis evoluções

- adicionar testes automatizados para o parser e as regras de validação;
- permitir configurar o leiaute posicional para diferentes fornecedores;
- exportar os registros válidos e o relatório de inconsistências;
- processar arquivos muito grandes com Web Workers;
- separar HTML, CSS e JavaScript em módulos conforme o crescimento do projeto;
- aprimorar acessibilidade, mensagens de erro e suporte a teclado.

## Autor

Desenvolvido por [Kayo Kalinauskas](https://github.com/kayokalinauskas).

Se este projeto foi útil ou chamou sua atenção, considere deixar uma estrela no repositório.
