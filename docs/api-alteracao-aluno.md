# API Dados Cadastrais - Criacao/Alteracao

## Informacoes Gerais

- **Nome**: API Dados Cadastrais - Criacao/Alteracao
- **Nome Curto**: Alteracao de Aluno
- **Versao**: 1.1 (24/11/2025)
- **Metodos HTTP**: PATCH (alteracao), POST (inclusao)
- **Formato**: JSON
- **Autenticacao**: Token exclusivo

## Objetivo

Permitir a insercao, consulta e atualizacao de dados cadastrais dos estudantes no sistema academico Prime, garantindo seguranca, integridade e rastreabilidade das informacoes.

## Regras de Negocio Principais

### Identificacao
- **CPF** e o identificador unico e **obrigatorio** em todas as requisicoes
- Se o CPF ja existir: atualiza o cadastro (PATCH)
- Se o CPF nao existir: cria novo cadastro (POST)

### Campos Imutaveis
- **CPF** e **MATRICULA** nao podem ser alterados

### Rastreabilidade
- Toda alteracao registra historico com:
  - Data/hora da modificacao
  - Usuario responsavel
  - Campos modificados
  - Valores anteriores e novos

### Alteracoes
- Refletidas imediatamente no banco de dados
- Validacao de campos obrigatorios no momento da requisicao

---

## Campos da API

### Dados Basicos

| Campo | Tipo | Obrigatorio | Observacoes |
|-------|------|-------------|-------------|
| CPF | String | **Sim** | 11 digitos, preservar zeros a esquerda |
| MATRICULA | String | Nao | Codigo unico do aluno (nao pode ser alterado) |
| NOME | String | Nao | Max 100 caracteres, permite acentuacao |
| NOME_SOCIAL | String | Nao | Max 100 caracteres |
| DATA_NASCIMENTO | Data | Nao | Formato ISO 8601 (AAAA-MM-DD) |
| GENERO | String | Nao | "Masculino" ou "Feminino" |
| ESTADO_CIVIL | String | Nao | Codigo da tabela Prime (1-8) |
| RACA | String | Nao | Codigo da tabela tipo_raca (0-6) |
| NACIONALIDADE | String | Nao | 1-Brasileira, 2-Naturalizado, 3-Estrangeiro |
| NECESSIDADE_ESPECIAL | String | Nao | Tabela de necessidades especiais |

### Documentos

| Campo | Tipo | Obrigatorio | Observacoes |
|-------|------|-------------|-------------|
| RG/RNM/CRNM | String | Nao | Pode conter digitos e tracos |
| RG_ORGAO | String | Nao | Tabela padrao de referencia |
| RG_UF | String | Nao | Sigla da UF |
| RG_DATA_EMISSAO | Data | Nao | Formato ISO 8601 |
| DOC_MILITAR | String | Nao | Numeros, letras e hifens |
| DATA_EMISSAO_DOC_MILITAR | Data | Nao | Formato ISO 8601 |
| TITULO_ELEITOR | String | Nao | 12 digitos |
| ZONA_ELEITORAL | String | Nao | 3 digitos |
| SECAO_ELEITORAL | String | Nao | 4 digitos |
| CTPS | String | Nao | Pode conter zeros a esquerda |
| CTPS_SERIE | String | Nao | 4-5 digitos |

### Certidao de Nascimento

| Campo | Tipo | Obrigatorio | Observacoes |
|-------|------|-------------|-------------|
| CERTIDAO_NASCIMENTO | String | Nao | Formato varia conforme cartorio |
| LIVRO_CERTIDAO_NASC | String | Nao | Identificador do livro |
| FOLHA_CERTIDAO_NASC | String | Nao | Pode ter zero a esquerda |
| UF_CERTIDAO_NASC | String | Nao | Sigla da UF |
| MUNICIPIO_CERTIDAO_NASC | String | Nao | Nome do municipio |
| DISTRITO_CERTIDAO_NASC | String | Nao | Se aplicavel |

### Naturalidade

| Campo | Tipo | Obrigatorio | Observacoes |
|-------|------|-------------|-------------|
| NATURAL_CIDADE | String | Nao | Permite acentuacao |
| NATURAL_UF | String | Nao | Sigla da UF |
| PAIS | String | Nao | Tabela padrao |
| COD_INEP_PAIS | String | Nao | Codigo INEP |

### Genitores

| Campo | Tipo | Obrigatorio | Observacoes |
|-------|------|-------------|-------------|
| NOME_MAE | String | Nao | Max 100 caracteres |
| CPF_MAE | String | Nao | 11 digitos |
| NOME_PAI | String | Nao | Max 100 caracteres |
| CPF_PAI | String | Nao | 11 digitos |
| GereroGenitor | String | Nao | Genero do pai |
| GeneroGenitora | String | Nao | Genero da mae |

### Responsavel Legal

| Campo | Tipo | Obrigatorio | Observacoes |
|-------|------|-------------|-------------|
| NOME_RESP | String | Nao | Max 100 caracteres |
| CPF_RESP | String | Nao | 11 digitos |
| DATA_NASCIMENTO_RESP | Data | Nao | Formato ISO 8601 |
| EMAIL_RESP | String | Nao | Formato valido |
| GRAU_PARENTESCO_RESP | String | Nao | - |

### Responsavel Financeiro

| Campo | Tipo | Obrigatorio | Observacoes |
|-------|------|-------------|-------------|
| RESP_FIN_MESMO_RESP | Boolean | Nao | "true" ou "false" |
| NOME_RESP_FIN | String | Nao | Max 100 caracteres |
| CPF_RESP_FIN | String | Nao | 11 digitos |
| DATA_NASCIMENTO_RESP_FIN | Data | Nao | Formato ISO 8601 |
| EMAIL_RESP_FIN | String | Nao | Formato valido |
| GRAU_PARENTESCO_RESP_FIN | String | Nao | - |

### Endereco

| Campo | Tipo | Obrigatorio | Observacoes |
|-------|------|-------------|-------------|
| ENDERECO | String | Nao | Nome da rua, avenida etc. |
| ENDERECO_NUMERO | String | Nao | Pode conter letras (ex: "123-A") |
| ENDERECO_COMPLEMENTO | String | Nao | - |
| ENDERECO_CEP | String | Nao | - |
| ENDERECO_BAIRRO | String | Nao | - |
| ENDERECO_CIDADE | String | Nao | - |
| ENDERECO_UF | String | Nao | Sigla da UF |
| ENDERECO_PAIS | String | Nao | - |
| COD_INEP_UF | String | Nao | Codigo INEP |
| COD_INEP_CIDADE | String | Nao | Codigo INEP |
| COD_INEP_PAIS | String | Nao | Codigo INEP |

### Contato

| Campo | Tipo | Obrigatorio | Observacoes |
|-------|------|-------------|-------------|
| TELEFONE_RESIDENCIAL | String | Nao | Formato: (XX) XXXX-XXXX |
| TELEFONE_PRINCIPAL | String | Nao | Formato: (XX) XXXX-XXXX |
| TELEFONE_COMERCIAL | String | Nao | Formato: (XX) XXXX-XXXX |
| CELULAR | String | Nao | Formato: (XX) 9XXXX-XXXX |
| EMAIL | String | Nao | - |

### Ensino Medio

| Campo | Tipo | Obrigatorio | Observacoes |
|-------|------|-------------|-------------|
| ESCOLA_ENS_MEDIO | String | Nao | Nome da instituicao |
| ANO_CONCLUSAO_ENS_MEDIO | Integer | Nao | Ano com 4 digitos (AAAA) |
| TIPO_ESCOLA | String | Nao | "Publica" ou "Privada" |
| COD_INEP_IE | String | Nao | Codigo INEP da escola |

### Profissao

| Campo | Tipo | Obrigatorio | Observacoes |
|-------|------|-------------|-------------|
| PROFISSAO | String | Nao | Tabela tipo_profissao |
| ID_PROFISSAO | String | Nao | - |
| LOCAL_DE_TRABALHO | String | Nao | - |

### Outros

| Campo | Tipo | Obrigatorio | Observacoes |
|-------|------|-------------|-------------|
| COD_EXPORTACAO_ORIGEM | String | Nao | Chave de integracao externa |
| USUARIO | String | Nao | Usuario do estudante |

---

## Tabelas de Referencia

### Estado Civil (ESTADO_CIVIL)
| Codigo | Descricao |
|--------|-----------|
| 1 | Casado |
| 2 | Divorciado |
| 3 | Marital |
| 4 | Separado |
| 5 | Separado Consensualmente |
| 6 | Separado Judicialmente |
| 7 | Solteiro |
| 8 | Uniao Estavel |

### Raca (RACA)
| Codigo | Descricao |
|--------|-----------|
| 0 | Nao Informado |
| 1 | Branca |
| 2 | Preta |
| 3 | Parda |
| 4 | Amarela |
| 5 | Indigena |
| 6 | Nao Dispoe da Info |

### Nacionalidade (NACIONALIDADE)
| Codigo | Descricao |
|--------|-----------|
| 1 | Brasileira |
| 2 | Brasileiro nascido no exterior ou Naturalizado |
| 3 | Estrangeiro |

---

## Codigos de Erro

| Codigo | Descricao | Causa |
|--------|-----------|-------|
| 400 | Bad Request | CPF nao informado na requisicao |
| 401 | Nao Autorizado | Token invalido ou ausente |
| - | CPF ja cadastrado | Tentativa de criar registro com CPF existente |

---

## Exemplo de Requisicao

```json
{
  "ACAO": "dados_cadastrais_manutencao",
  "dados_cadastrais_manutencao": [
    {
      "COD_EXPORTACAO_ORIGEM": "chave_integracao",
      "CPF": "12345678901",
      "MATRICULA": "19304438",
      "NOME": "Joao da Silva",
      "NOME_SOCIAL": "",
      "DATA_NASCIMENTO": "1990-05-15",
      "GENERO": "Masculino",
      "NACIONALIDADE": "1",
      "NOME_MAE": "Maria da Silva",
      "CPF_MAE": "98765432100",
      "ENDERECO": "Rua das Flores",
      "ENDERECO_NUMERO": "123",
      "ENDERECO_CEP": "01234-567",
      "ENDERECO_CIDADE": "Sao Paulo",
      "ENDERECO_UF": "SP",
      "EMAIL": "joao.silva@email.com",
      "CELULAR": "(11) 91234-5678"
    }
  ]
}
```
