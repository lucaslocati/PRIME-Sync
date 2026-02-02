# API 2ª Via de Diploma e Histórico Legado

## Informações Gerais

- **Finalidade**: Envio de dados acadêmicos para emissão de 2ª via de diploma e histórico escolar
- **Sistema destino**: PRIME (MEC)
- **Formato**: JSON
- **Encoding**: UTF-8

---

## Estrutura do JSON

```json
{
  "alunoID": "String",
  "alunoNome": "String",
  "alunoNomeSocial": "String",
  "alunoSexo": "String (M|F)",
  "alunoDataNascimento": "Data ISO 8601",
  "alunoNacionalidade": "String",
  "alunoPaisNascimento": "String",
  "alunoMunicipioNascimentoCodigoIBGE": "String",
  "alunoMunicipioNascimentoDescricao": "String",
  "alunoUFNascimento": "String (2 chars)",
  "alunoCPF": "String (11 dígitos)",
  "alunoRG": {
    "tipo": "String (RG|RNM|CRNM)",
    "numero": "String",
    "uf": "String",
    "orgaoExpedidor": "String"
  },
  "genitores": [
    {
      "genitorNome": "String",
      "genitorSexo": "String (M|F)"
    }
  ],
  "ingresso": {
    "alunoDataIngresso": "Data ISO 8601",
    "alunoFormaIngresso": "String"
  },
  "conclusao": {
    "alunoDataConclusao": "Data ISO 8601",
    "alunoPeriodoConclusao": "String (ex: 2024/2)",
    "alunoDataColacao": "Data ISO 8601"
  },
  "diploma": {
    "livroRegistro": "String",
    "numeroFolha": "String",
    "numeroSequencia": "String",
    "numeroRegistro": "String",
    "dataRegistro": "Data ISO 8601",
    "processo": "String",
    "dataExpedicao": "Data ISO 8601"
  },
  "historico": [
    {
      "idDisciplina": "String",
      "disciplina": "String",
      "nota": "String",
      "periodo": "String",
      "cargaHoraria": "String",
      "resultado": "String",
      "ciclo": "Integer",
      "percentualFrequencia": "Decimal",
      "docente": {
        "docenteNome": "String",
        "docenteTitulacao": "String",
        "docenteCpf": "String"
      }
    }
  ],
  "atividadeComplementar": [
    {
      "codigo": "String",
      "dataInicio": "Data ISO 8601",
      "dataFim": "Data ISO 8601",
      "descricao": "String",
      "tipo": "String",
      "chHoraRelogio": "String",
      "docenteAprovador": {
        "docenteNome": "String",
        "docenteTitulacao": "String"
      }
    }
  ],
  "estagio": {
    "codigoUnidadeCurricular": "String",
    "dataInicioEstagio": "Data ISO 8601",
    "dataFimEstagio": "Data ISO 8601",
    "cargaHorariaEstagio": "String",
    "docenteOrientador": {
      "docenteNome": "String",
      "docenteTitulacao": "String"
    }
  },
  "situacaoAluno": {
    "alunoCargaHorariaIntgralizada": "Integer",
    "alunoSituacao": "String",
    "alunoDataEmissaoHistorico": "Data ISO 8601",
    "alunoHoraEmissaoHistorico": "String (HH:MM:SS)"
  },
  "enade": {
    "situacao": "String",
    "condicao": "String",
    "motivo": "String",
    "edicao": "Integer",
    "dataProva": "Data ISO 8601"
  },
  "curso": {
    "cursoDescricao": "String",
    "cursoEMEC": "String",
    "cursoHabilitacao": "String",
    "cursoModalidade": "String",
    "cursoTituloConferido": "String",
    "cursoGrauConferido": "String",
    "cursoCargaHorariaTotal": "Integer",
    "cursoDataHabilitacao": "String",
    "autorizacao": { /* ... */ },
    "reconhecimento": { /* ... */ },
    "renovacaoReconhecimento": { /* ... */ },
    "endereco": { /* ... */ }
  },
  "emissora": {
    "nome": "String",
    "codigoEMEC": "String",
    "cnpj": "String (14 dígitos)",
    "endereco": { /* ... */ },
    "credenciamento": { /* ... */ },
    "recredenciamento": { /* ... */ },
    "renovacaoRecredenciamento": { /* ... */ }
  },
  "polo": {
    "poloNome": "String",
    "poloCodigoMEC": "String",
    "endereco": { /* ... */ }
  }
}
```

---

## Tipos de Dados

| Tipo | Formato | Exemplo |
|------|---------|---------|
| String | Texto livre | "João Silva" |
| Integer | Número inteiro | 3200 |
| Decimal | Número com decimais | 75.50 |
| Data ISO 8601 | AAAA-MM-DD | "2024-12-31" |
| Array | Lista de objetos | [...] |
| Object | Objeto aninhado | {...} |

---

## Valores Esperados

### alunoSexo / genitorSexo
- `M` - Masculino
- `F` - Feminino

### alunoRG.tipo
- `RG` - Registro Geral
- `RNM` - Registro Nacional Migratório
- `CRNM` - Carteira de Registro Nacional Migratório

### ingresso.alunoFormaIngresso
- Vestibular
- ENEM
- Transferência
- Portador de Diploma
- Outros

### historico[].resultado
- Aprovado
- Reprovado
- Dispensado
- Trancado

### situacaoAluno.alunoSituacao
- Aprovado
- Reprovado
- Em Curso
- Trancado
- Abandonou

### enade.situacao
- Habilitado
- Não Habilitado
- Irregular

### enade.condicao
- Ingressante
- Concluinte

### curso.cursoModalidade
- Presencial
- EAD

### curso.cursoGrauConferido
- Bacharelado
- Licenciatura
- Tecnólogo

### docente.docenteTitulacao
- Graduação
- Especialização
- Mestrado
- Doutorado
- Pós-Doutorado

---

## Estrutura de Atos Legais

Usado em: autorizacao, reconhecimento, renovacaoReconhecimento, credenciamento, recredenciamento

```json
{
  "tipo": "String (ex: Resolução, Portaria, Decreto)",
  "numero": "String (ex: 330/2019)",
  "data": "Data ISO 8601",
  "dataPublicacao": "Data ISO 8601",
  "veiculoPublicacao": "String",
  "secaoPublicacao": "String",
  "paginaPublicacao": "String",
  "numeroDOU": "String"
}
```

### Campos de Tramitação (quando em processo)
```json
{
  "tramitacaoNumeroProcesso": "String",
  "tramitacaoTipoProcesso": "String",
  "tramitacaoDataCadastro": "Data ISO 8601",
  "tramitacaoDataProtocolo": "Data ISO 8601"
}
```

---

## Estrutura de Endereço

Usado em: curso.endereco, emissora.endereco, polo.endereco

```json
{
  "logradouro": "String",
  "numero": "String",
  "complemento": "String",
  "bairro": "String",
  "municipioCodigoIBGE": "String",
  "municipioDescricao": "String",
  "uf": "String (2 chars)",
  "cep": "String (00000-000)"
}
```

---

## Validações Importantes

1. **CPF**: 11 dígitos, sem formatação
2. **CNPJ**: 14 dígitos, sem formatação
3. **CEP**: Formato 00000-000
4. **Datas**: Sempre ISO 8601 (AAAA-MM-DD)
5. **Hora**: Formato HH:MM:SS
6. **UF**: Sempre 2 caracteres maiúsculos
7. **Código IBGE**: Código oficial do município
8. **alunoNome**: Máximo 100 caracteres
9. **alunoNomeSocial**: Máximo 100 caracteres
