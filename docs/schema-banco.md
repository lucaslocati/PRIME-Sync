# Schema do Banco de Dados - Base Unificada

## Informações Gerais

- **Schema**: BASE_UNIF
- **Banco**: SQL Server
- **Finalidade**: Armazenamento unificado de dados acadêmicos

---

## Tabelas

### ALUNO

Dados básicos do aluno.

| Campo | Tipo | Descrição | Mapeado para API |
|-------|------|-----------|------------------|
| COD_ALUNO | VARCHAR | Código/Matrícula do aluno | alunoID |
| NOME_ALUNO | VARCHAR | Nome completo | alunoNome |

---

### ALUNO_CURSO

Dados do aluno em relação ao curso (matrícula).

| Campo | Tipo | Descrição | Mapeado para API |
|-------|------|-----------|------------------|
| ID | INT | ID interno da matrícula | — |
| COD_ALUNO | VARCHAR | Código do aluno | alunoID |
| NOME_ALUNO | VARCHAR | Nome do aluno | alunoNome |
| NOME_CURSO | VARCHAR | Nome do curso | — |
| COD_ALUNO_CURSO | VARCHAR | Código da matrícula | — |
| DT_NASCIMENTO | DATE | Data de nascimento | alunoDataNascimento |
| NACIONALIDADE | VARCHAR | Nacionalidade | alunoNacionalidade |
| UF | CHAR(2) | UF (verificar se é nascimento ou endereço) | alunoUFNascimento (parcial) |
| CPF | VARCHAR | CPF do aluno | alunoCPF |
| TIPO_DOC_IDENT | VARCHAR | Tipo do documento | alunoRG.tipo |
| NUM_RG | VARCHAR | Número do RG | alunoRG.numero |
| ORGAO_EXP | VARCHAR | Órgão expedidor | alunoRG.orgaoExpedidor |
| DT_INGRESSO | DATE | Data de ingresso | ingresso.alunoDataIngresso |
| FORMA_INGRESSO | VARCHAR | Forma de ingresso | ingresso.alunoFormaIngresso |
| DATA_CONCLUSAO | DATE | Data de conclusão | conclusao.alunoDataConclusao |
| DT_COLACAO_GRAU | DATE | Data da colação | conclusao.alunoDataColacao |
| DT_EXP_DIPLOMA | DATE | Data expedição diploma | diploma.dataExpedicao |
| OBS_ENADE | VARCHAR | Observações ENADE | enade.situacao / enade.motivo |
| RECONHECIMENTO | VARCHAR | Reconhecimento do curso | — |
| INFO_INGRESSO | VARCHAR | Informações de ingresso | — |
| COL_ENS_MEDIO | VARCHAR | Colégio do ensino médio | — |
| ESTADO_ENS_MEDIO | VARCHAR | Estado do ensino médio | — |
| CDDE_ENS_MEDIO | VARCHAR | Cidade do ensino médio | — |
| PAIS_ENS_MEDIO | VARCHAR | País do ensino médio | — |
| ANO_CONC_ENS_MEDIO | INT | Ano conclusão ensino médio | — |
| SIT_AFASTAMENTO | VARCHAR | Situação de afastamento | — |
| DATA_AFASTAMENTO | DATE | Data de afastamento | — |
| CAMPUS | VARCHAR | Campus | — |
| TIPO_HISTORICO | VARCHAR | Tipo de histórico | — |

**Campos NÃO utilizados pela API:**
- ID
- COD_ALUNO_CURSO
- RECONHECIMENTO
- INFO_INGRESSO
- COL_ENS_MEDIO
- ESTADO_ENS_MEDIO
- CDDE_ENS_MEDIO
- PAIS_ENS_MEDIO
- ANO_CONC_ENS_MEDIO
- SIT_AFASTAMENTO
- DATA_AFASTAMENTO
- CAMPUS
- TIPO_HISTORICO

---

### CURSO

Dados do curso.

| Campo | Tipo | Descrição | Mapeado para API |
|-------|------|-----------|------------------|
| COD_CURSO | VARCHAR | Código do curso | — |
| NOME_CURSO | VARCHAR | Nome do curso | curso.cursoDescricao |
| RECONHECIMENTO | VARCHAR | Dados de reconhecimento | — |

**Campos NÃO utilizados pela API:**
- COD_CURSO
- RECONHECIMENTO

---

### HISTORICO_ESCOLAR

Disciplinas cursadas pelo aluno.

| Campo | Tipo | Descrição | Mapeado para API |
|-------|------|-----------|------------------|
| ID_DISCIPLINA | VARCHAR | ID único da disciplina | historico[].idDisciplina |
| COD_DISCIPLINA | VARCHAR | Código da disciplina | — |
| DISCIPLINA | VARCHAR | Nome da disciplina | historico[].disciplina |
| MEDIA_FINAL | DECIMAL | Nota/média final | historico[].nota |
| ANO_SEM_DISC | VARCHAR | Período (ex: 2024/1) | historico[].periodo |
| ANO | INT | Ano | — |
| HORA_RELOGIO | INT | Carga horária (hora relógio) | historico[].cargaHoraria |
| HORA_AULA | INT | Carga horária (hora aula) | — |
| RESULTADO | VARCHAR | Resultado (Aprovado, etc.) | historico[].resultado |
| PERC_FREQUENCIA | DECIMAL | Percentual de frequência | historico[].percentualFrequencia |
| PROFESSOR_TITULACAO | VARCHAR | Nome e titulação (texto único) | historico[].docente.docenteNome (parcial) |
| HORAS_ATIVIDADE | INT | Horas de atividade complementar | atividadeComplementar[].chHoraRelogio |
| HORAS_PRJ_COMUNITARIO | INT | Horas projeto comunitário | — |
| TOTAL_HORA_RELOGIO | INT | Total horas (todo histórico) | situacaoAluno.alunoCargaHorariaIntgralizada |
| TOTAL_HORAS_AULA | INT | Total horas aula | — |
| DESC_HABILITACAO | VARCHAR | Descrição da habilitação | curso.cursoHabilitacao |
| HABILITACAO | VARCHAR | Código habilitação | — |
| OPTATIVA | CHAR(1) | Flag optativa (S/N) | — |

**Campos NÃO utilizados pela API:**
- COD_DISCIPLINA
- ANO
- HORA_AULA
- HORAS_PRJ_COMUNITARIO
- TOTAL_HORAS_AULA
- HABILITACAO
- OPTATIVA

---

## Mapeamentos que Precisam de Tratamento

### 1. PROFESSOR_TITULACAO → docente.docenteNome + docente.docenteTitulacao

O campo `PROFESSOR_TITULACAO` contém nome e titulação em texto corrido. Exemplo:
```
"Prof. Dr. João Silva - Doutorado em Educação"
```

Precisa de **parse** para separar em:
- `docenteNome`: "João Silva"
- `docenteTitulacao`: "Doutorado"

### 2. UF → alunoUFNascimento

Verificar se o campo `UF` na tabela `ALUNO_CURSO` refere-se a:
- UF de nascimento (esperado pela API)
- UF de endereço atual

### 3. OBS_ENADE → enade.situacao + enade.motivo

O campo `OBS_ENADE` contém texto livre. Exemplo:
```
"Exame realizado em 29/06/97"
```

Precisa de **parse** para extrair:
- `situacao`: "Habilitado" ou "Não Habilitado"
- `motivo`: motivo se não habilitado

### 4. DATA_CONCLUSAO → conclusao.alunoPeriodoConclusao

O campo `alunoPeriodoConclusao` (ex: "2024/2") pode ser **derivado** de `DATA_CONCLUSAO`:
- Extrair ano e semestre da data
- Formato: AAAA/S (onde S = 1 para jan-jun, 2 para jul-dez)

---

## Campos Faltantes (não existem no banco)

### Críticos (obrigatórios)
- alunoSexo
- alunoPaisNascimento
- alunoMunicipioNascimentoCodigoIBGE
- alunoMunicipioNascimentoDescricao
- alunoRG.uf
- genitores[] (array completo)
- diploma.livroRegistro, numeroFolha, numeroSequencia, numeroRegistro, dataRegistro, processo
- historico[].ciclo
- estagio (estrutura completa)
- curso.cursoEMEC, cursoModalidade, cursoTituloConferido, cursoGrauConferido, cursoCargaHorariaTotal
- curso.autorizacao (estrutura completa)
- curso.reconhecimento (estrutura completa)
- curso.endereco (estrutura completa)
- emissora (estrutura completa)

### Condicionais
- Campos de tramitação (quando em processo)
- Campos de renovação (quando houver)
- Campos de polo (para EAD)
- Campos de ENADE detalhados

---

## Queries de Referência

### Buscar dados do aluno
```sql
SELECT
    ac.COD_ALUNO,
    ac.NOME_ALUNO,
    ac.DT_NASCIMENTO,
    ac.CPF,
    ac.TIPO_DOC_IDENT,
    ac.NUM_RG,
    ac.ORGAO_EXP,
    ac.NACIONALIDADE,
    ac.UF,
    ac.DT_INGRESSO,
    ac.FORMA_INGRESSO,
    ac.DATA_CONCLUSAO,
    ac.DT_COLACAO_GRAU,
    ac.DT_EXP_DIPLOMA,
    ac.OBS_ENADE,
    c.NOME_CURSO
FROM BASE_UNIF.ALUNO_CURSO ac
INNER JOIN BASE_UNIF.CURSO c ON ac.COD_CURSO = c.COD_CURSO
WHERE ac.COD_ALUNO = @matricula
```

### Buscar histórico escolar
```sql
SELECT
    ID_DISCIPLINA,
    DISCIPLINA,
    MEDIA_FINAL,
    ANO_SEM_DISC,
    HORA_RELOGIO,
    RESULTADO,
    PERC_FREQUENCIA,
    PROFESSOR_TITULACAO,
    HORAS_ATIVIDADE,
    DESC_HABILITACAO,
    TOTAL_HORA_RELOGIO
FROM BASE_UNIF.HISTORICO_ESCOLAR
WHERE COD_ALUNO = @matricula
ORDER BY ANO_SEM_DISC
```
