# Projeto: Mapeamento API 2ª Via x Base Unificada

## Visão Geral

Este projeto é uma ferramenta visual para mapear e validar os campos necessários para integração com a **API 2ª Via de Diploma e Histórico Legado** do MEC/PRIME, comparando com os campos disponíveis na **Base Unificada** (banco de dados interno).

## Arquivos do Projeto

- `mapeamento-api.html` - Aplicação React single-file com a tabela de mapeamento
- `CLAUDE.md` - Este arquivo de contexto
- `docs/api-campos.md` - Documentação dos campos da API
- `docs/schema-banco.md` - Schema do banco de dados Base Unificada

## Tecnologias

- React 18 (via CDN)
- Babel Standalone (transpilação JSX)
- Tailwind CSS (via CDN)
- Single HTML file (sem build process)

---

## Regras de Status

| Status | Cor | Significado |
|--------|-----|-------------|
| **OK** | Verde | Campo existe na Base Unificada e mapeia corretamente |
| **Parcial** | Amarelo | Campo existe mas precisa de tratamento/parse/derivação |
| **Faltando** | Vermelho | Campo obrigatório que NÃO existe na Base Unificada |
| **N/A** | Cinza | Campo não obrigatório que não existe na base |

### Regra para definir status:
- Se o campo **existe** na Base Unificada → `green` (OK)
- Se o campo **existe parcialmente** ou precisa de transformação → `yellow` (Parcial)
- Se o campo **NÃO existe** e é **obrigatório** → `red` (Faltando)
- Se o campo **NÃO existe** e é **NÃO obrigatório** → `gray` (N/A)

---

## Regras de Obrigatoriedade (Legenda)

| Símbolo | Significado |
|---------|-------------|
| `*` | Obrigatório apenas para brasileiros |
| `**` | Não obrigatório quando tipo é RG |
| `***` | Obrigatório se desejar informar forma de ingresso |
| `****` | Obrigatório se informar atividade complementar |
| `*****` | Obrigatório se desejar informar dados do ENADE |
| `******` | Obrigatório desde que não esteja em tramitação |
| `*******` | Obrigatório se desejar informar dados do Polo (EAD) |
| `********` | Obrig. Diploma/Hist. Formado, não obrig. hist. parcial |
| `*********` | Obrig. se houver renovação do reconhecimento |
| `**********` | Obrig. se credenciamento vencido ou em tramitação |

---

## Estrutura de Dados da Tabela

Cada registro possui:
```javascript
{
  categoria: "String",      // Agrupamento (ex: "Dados do Aluno", "Curso")
  campo: "String",          // Nome do campo na API (notação dot: "alunoRG.tipo")
  descricao: "String",      // Descrição do campo
  tipo: "String",           // Tipo de dado (String, Integer, Data ISO 8601, etc.)
  obrigatorio: "String",    // Sim, Não, Cond.*, etc.
  baseUnif: "String",       // Nome do campo na Base Unificada ou "—"
  tabela: "String",         // Tabela de origem ou "—"
  exemplo: "String",        // Valor de exemplo
  status: "String",         // green, yellow, red, gray
  obs: "String"             // Observações/notas
}
```

---

## Regras para Observações

A coluna de observações deve conter APENAS:
- **Dúvidas** - Marcadas com "⚠️ Confirmar com..."
- **Condicionais** - Ex: "Obrigatório para brasileiros"
- **Notas técnicas** - Ex: "Máx 100 caracteres", "Formato: AAAA-MM-DD"
- **DE-PARA** - Quando precisa transformação de valores

**NÃO incluir:**
- Textos genéricos como "Preenchimento manual obrigatório"
- Informações óbvias ou redundantes

---

## Categorias de Campos

1. **Dados do Aluno** - Informações pessoais do estudante
2. **Genitores** - Dados dos pais (mínimo 1 obrigatório)
3. **Ingresso** - Data e forma de ingresso
4. **Conclusão** - Datas de conclusão e colação
5. **Diploma** - Dados de registro do diploma
6. **Histórico** - Array de disciplinas cursadas
7. **Ativ. Complementar** - Atividades extras
8. **Estágio** - Dados de estágio obrigatório
9. **Situação Aluno** - Carga horária e situação final
10. **ENADE** - Dados do exame nacional
11. **Curso** - Informações do curso
12. **Autorização Curso** - Ato de autorização
13. **Reconhecimento Curso** - Ato de reconhecimento
14. **Renov. Reconhec. Curso** - Renovação do reconhecimento
15. **Endereço Curso** - Localização do curso
16. **Emissora (IES)** - Dados da instituição
17. **Credenciamento IES** - Ato de credenciamento
18. **Recredenciamento IES** - Ato de recredenciamento
19. **Renov. Recred. IES** - Renovação do recredenciamento
20. **Polo (EAD)** - Dados do polo para cursos EAD

---

## Funcionalidades da Aplicação

1. **Filtros** - Por status e por categoria
2. **Estatísticas** - Contadores de cada status
3. **Export CSV** - Download com BOM para Excel
4. **Drag-to-scroll** - Navegação por arraste na tabela
5. **Header fixo** - Cabeçalho sticky ao rolar
6. **Seção de campos não utilizados** - Lista campos do banco não mapeados

---

## Campos Não Utilizados da Base Unificada

### ALUNO_CURSO (13 campos)
- ID, COD_ALUNO_CURSO, RECONHECIMENTO, INFO_INGRESSO
- COL_ENS_MEDIO, ESTADO_ENS_MEDIO, CDDE_ENS_MEDIO, PAIS_ENS_MEDIO
- ANO_CONC_ENS_MEDIO, SIT_AFASTAMENTO, DATA_AFASTAMENTO, CAMPUS, TIPO_HISTORICO

### CURSO (2 campos)
- COD_CURSO, RECONHECIMENTO

### HISTORICO_ESCOLAR (7 campos)
- ANO, HORA_AULA, HORAS_PRJ_COMUNITARIO, OPTATIVA
- HABILITACAO, TOTAL_HORAS_AULA, COD_DISCIPLINA

---

## Convenções de Código

- Nomes de campos API usam **camelCase** com notação de ponto para objetos aninhados
- Arrays são indicados com `[]` no nome (ex: `historico[]`, `genitores[]`)
- Campos da Base Unificada usam **UPPER_SNAKE_CASE**
- Datas sempre no formato **ISO 8601** (AAAA-MM-DD)
