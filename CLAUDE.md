# Projeto: Mapeamento APIs x Base Unificada

## Visao Geral

Este projeto e uma ferramenta visual para mapear e validar os campos necessarios para integracao com **multiplas APIs** do sistema PRIME, comparando com os campos disponiveis na **Base Unificada** (banco de dados interno).

### APIs Suportadas

| Nome Curto | Nome Completo | Descricao |
|------------|---------------|-----------|
| **Emissao Diploma** | API 2a Via de Diploma e Historico Legado | Emissao de segunda via de diplomas e historicos academicos |
| **Alteracao de Aluno** | API Dados Cadastrais - Criacao/Alteracao | Insercao, consulta e atualizacao de dados cadastrais dos estudantes |

## Arquivos do Projeto

- `index.html` - Aplicacao React single-file com a tabela de mapeamento
- `CLAUDE.md` - Este arquivo de contexto
- `docs/api-campos.md` - Documentacao dos campos da API Emissao Diploma
- `docs/schema-banco.md` - Schema do banco de dados Base Unificada

## Tecnologias

- React 18 (via CDN)
- Babel Standalone (transpilacao JSX)
- Tailwind CSS (via CDN)
- Single HTML file (sem build process)

---

## Regras de Status

| Status | Cor | Label | Significado |
|--------|-----|-------|-------------|
| **green** | Verde | OK | Campo existe na Base Unificada e mapeia corretamente |
| **yellow** | Amarelo | Parcial | Campo existe mas precisa de tratamento/parse/derivacao |
| **red** | Vermelho | Faltando | Campo obrigatorio que NAO existe na Base Unificada |
| **gray** | Cinza | N/O | Campo nao obrigatorio que nao existe na base |
| **review** | Azul | Analisar | Campo que precisa de verificacao manual |

### Regra para definir status:
- Se o campo **existe** na Base Unificada → `green` (OK)
- Se o campo **existe parcialmente** ou precisa de transformacao → `yellow` (Parcial)
- Se o campo **NAO existe** e e **obrigatorio** → `red` (Faltando)
- Se o campo **NAO existe** e e **NAO obrigatorio** → `gray` (N/O)
- Se o campo **precisa de analise manual** para determinar existencia/mapeamento → `review` (Analisar)

---

## Regras de Obrigatoriedade (API Emissao Diploma)

| Simbolo | Significado |
|---------|-------------|
| `*` | Obrigatorio apenas para brasileiros |
| `**` | Nao obrigatorio quando tipo e RG |
| `***` | Obrigatorio se desejar informar forma de ingresso |
| `****` | Obrigatorio se informar atividade complementar |
| `*****` | Obrigatorio se desejar informar dados do ENADE |
| `******` | Obrigatorio desde que nao esteja em tramitacao |
| `*******` | Obrigatorio se desejar informar dados do Polo (EAD) |
| `********` | Obrig. Diploma/Hist. Formado, nao obrig. hist. parcial |
| `*********` | Obrig. se houver renovacao do reconhecimento |
| `**********` | Obrig. se credenciamento vencido ou em tramitacao |

### API Alteracao de Aluno
- Apenas o campo **CPF** e obrigatorio
- Todos os demais campos sao opcionais para alteracao parcial
- CPF e MATRICULA nao podem ser alterados

---

## Estrutura de Dados da Tabela

Cada registro possui:
```javascript
{
  api: "String",          // Nome da API (ex: "Emissao Diploma", "Alteracao de Aluno")
  categoria: "String",    // Agrupamento (ex: "Dados do Aluno", "Endereco")
  campo: "String",        // Nome do campo na API
  descricao: "String",    // Descricao do campo
  tipo: "String",         // Tipo de dado (String, Integer, Data ISO 8601, etc.)
  obrigatorio: "String",  // Sim, Nao, Cond.*, etc.
  baseUnif: "String",     // Nome do campo na Base Unificada ou "—"
  tabela: "String",       // Tabela de origem ou "—"
  exemplo: "String",      // Valor de exemplo
  status: "String",       // green, yellow, red, gray, review
  obs: "String"           // Observacoes/notas
}
```

---

## Regras para Observacoes

A coluna de observacoes deve conter APENAS:
- **Duvidas** - Marcadas com "Confirmar com..."
- **Condicionais** - Ex: "Obrigatorio para brasileiros"
- **Notas tecnicas** - Ex: "Max 100 caracteres", "Formato: AAAA-MM-DD"
- **DE-PARA** - Quando precisa transformacao de valores

**NAO incluir:**
- Textos genericos como "Preenchimento manual obrigatorio"
- Informacoes obvias ou redundantes

---

## Categorias de Campos

### API Emissao Diploma
1. **Dados do Aluno** - Informacoes pessoais do estudante
2. **Genitores** - Dados dos pais (minimo 1 obrigatorio)
3. **Ingresso** - Data e forma de ingresso
4. **Conclusao** - Datas de conclusao e colacao
5. **Diploma** - Dados de registro do diploma
6. **Historico** - Array de disciplinas cursadas
7. **Ativ. Complementar** - Atividades extras
8. **Estagio** - Dados de estagio obrigatorio
9. **Situacao Aluno** - Carga horaria e situacao final
10. **ENADE** - Dados do exame nacional
11. **Curso** - Informacoes do curso
12. **Autorizacao Curso** - Ato de autorizacao
13. **Reconhecimento Curso** - Ato de reconhecimento
14. **Renov. Reconhec. Curso** - Renovacao do reconhecimento
15. **Endereco Curso** - Localizacao do curso
16. **Emissora (IES)** - Dados da instituicao
17. **Credenciamento IES** - Ato de credenciamento
18. **Recredenciamento IES** - Ato de recredenciamento
19. **Renov. Recred. IES** - Renovacao do recredenciamento
20. **Polo (EAD)** - Dados do polo para cursos EAD

### API Alteracao de Aluno
1. **Dados Basicos** - CPF, matricula, nome, genero, estado civil, raca, nacionalidade
2. **Documentos** - RG, titulo eleitor, CTPS, doc. militar
3. **Certidao Nascimento** - Dados da certidao de nascimento
4. **Naturalidade** - Cidade, UF e pais de nascimento
5. **Genitores** - Nome e CPF dos pais
6. **Responsavel Legal** - Dados do responsavel legal
7. **Responsavel Financeiro** - Dados do responsavel financeiro
8. **Endereco** - Endereco completo do estudante
9. **Contato** - Telefones e e-mail
10. **Ensino Medio** - Escola e ano de conclusao
11. **Profissao** - Dados profissionais
12. **Outros** - Campos auxiliares (chave integracao, usuario)

---

## Funcionalidades da Aplicacao

1. **Filtro por API** - Filtrar campos por API especifica
2. **Filtro por Status** - Por status de mapeamento
3. **Filtro por Categoria** - Por categoria de campos
4. **Busca** - Por nome do campo ou descricao
5. **Estatisticas** - Contadores de cada status
6. **Export CSV** - Download com BOM para Excel
7. **Drag-to-scroll** - Navegacao por arraste na tabela
8. **Header fixo** - Cabecalho sticky ao rolar
9. **Destaque de linhas** - Linhas com status "review" tem fundo azul claro
10. **Secao de campos nao utilizados** - Lista campos do banco nao mapeados

---

## Campos Mapeados entre APIs

Alguns campos existem em ambas as APIs. Eles sao **duplicados propositalmente** na tabela, pois podem ter obrigatoriedades diferentes:

| Campo Base Unificada | API Emissao Diploma | API Alteracao de Aluno |
|---------------------|---------------------|------------------------|
| CPF | alunoCPF (Obrig.) | CPF (Obrig.) |
| COD_ALUNO | alunoID (Obrig.) | MATRICULA (Nao) |
| NOME_ALUNO | alunoNome (Obrig.) | NOME (Nao) |
| DT_NASCIMENTO | alunoDataNascimento (Obrig.) | DATA_NASCIMENTO (Nao) |
| NACIONALIDADE | alunoNacionalidade (Obrig.) | NACIONALIDADE (Nao) |
| NUM_RG | alunoRG.numero (Obrig.) | RG/RNM/CRNM (Nao) |
| ORGAO_EXP | alunoRG.orgaoExpedidor (Nao) | RG_ORGAO (Nao) |
| UF | alunoUFNascimento (Parcial) | NATURAL_UF (Parcial) |
| COL_ENS_MEDIO | — | ESCOLA_ENS_MEDIO (OK) |
| ANO_CONC_ENS_MEDIO | — | ANO_CONCLUSAO_ENS_MEDIO (OK) |

---

## Campos Nao Utilizados da Base Unificada

### ALUNO_CURSO (11 campos)
- ID, COD_ALUNO_CURSO, RECONHECIMENTO, INFO_INGRESSO
- ESTADO_ENS_MEDIO, CDDE_ENS_MEDIO, PAIS_ENS_MEDIO
- SIT_AFASTAMENTO, DATA_AFASTAMENTO, CAMPUS, TIPO_HISTORICO

### CURSO (2 campos)
- COD_CURSO, RECONHECIMENTO

### HISTORICO_ESCOLAR (7 campos)
- ANO, HORA_AULA, HORAS_PRJ_COMUNITARIO, OPTATIVA
- HABILITACAO, TOTAL_HORAS_AULA, COD_DISCIPLINA

**Nota:** COL_ENS_MEDIO e ANO_CONC_ENS_MEDIO agora sao utilizados pela API Alteracao de Aluno.

---

## Convencoes de Codigo

- Nomes de campos da API Emissao Diploma usam **camelCase** com notacao de ponto para objetos aninhados
- Nomes de campos da API Alteracao de Aluno usam **UPPER_SNAKE_CASE**
- Arrays sao indicados com `[]` no nome (ex: `historico[]`, `genitores[]`)
- Campos da Base Unificada usam **UPPER_SNAKE_CASE**
- Datas sempre no formato **ISO 8601** (AAAA-MM-DD)

---

## Como Adicionar Nova API

1. Adicionar registros no array `initialData` com o campo `api` preenchido
2. Usar status `review` para campos que precisam de analise manual
3. Atualizar este CLAUDE.md com as novas categorias e regras
4. Atualizar a secao de campos nao utilizados se necessario
