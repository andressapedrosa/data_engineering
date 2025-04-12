# Projeto de Engenharia de Dados
## Criando um pipeline de dados relacionados a suicídios cometidos no Brasil entre 2010 e 2019

O suicídio é um problema grave de saúde pública que afeta milhões de pessoas em todo o mundo. No Brasil, como em muitos outros países, a Classificação Internacional de Doenças (CID) é utilizada para registrar e categorizar as causas de morte, incluindo suicídios. A CID-10, em particular, dedica uma seção específica às causas externas de morbidade e mortalidade, onde os suicídios são classificados sob os códigos X600 a X850. Esses códigos são essenciais para identificar e monitorar os casos de suicídio, permitindo que os profissionais de saúde e pesquisadores analisem tendências e desenvolvam estratégias de prevenção.

Através da CID-10, é possível identificar que uma pessoa cometeu suicídio ao verificar se o código de causa de morte está dentro do grupo X600-X850, que inclui lesões autoprovocadas intencionalmente, como suicídio e tentativas de suicídio. Essa informação é crucial para a vigilância epidemiológica e para a implementação de políticas públicas eficazes na prevenção do suicídio.

Ao correlacionar dados de suicídio com a CID-10 é possível realizar uma análise mais aprofundada dos padrões e tendências de suicídio, ajudando a melhorar a compreensão e a resposta a esse problema de saúde pública.

## Linhagem dos Dados
O DATASUS disponibiliza publicamente dados relacionados a suicídios cometidos no Brasil. Estes podem ser encontrados em:
-  https://opendatasus.saude.gov.br/dataset/sim  

Com base nestas informações, foi disponibilizado no Kaggle, um novo dataset contendo a série histórica dos suicídios cometidos no Brasil entre os anos de 2010 e 2019.
- https://www.kaggle.com/datasets/psicodata/dados-de-suicidios-entre-2010-e-2019

Importante mencionar que os dados de suicídio disponibilizados no Kaggle **não possuem dados nulos**: já houve a **tratativa dos dados faltantes**. Foi realizada a **substituição dos valores nulos pelo valor 'NA'**.

Este dataset faz referência ao CID, informando em 2 de suas colunas o respectivo código. Como abordado anteriormente, o CID se refere a um Diagnóstico que, para este cenárop, foi utilizado para definir a causa do óbito do indivíduo. 

Porém, para fins de análise, a disponibilização somente do código CID não é suficiente para compreensão dos resultados encontrados e respectivas tomadas de decisão. Se faz necessária a descrição do Diagnóstico e, esta informação pode ser encontrada na seguinte referência:
- http://www2.datasus.gov.br/cid10/V2008/cid10.htm

Para este presente trabalho serão utilizados os seguintes datasets:

- dados de suicídio entre 2010 e 2019
  - https://www.kaggle.com/datasets/psicodata/dados-de-suicidios-entre-2010-e-2019
    - suicidios_2010_a_2019.csv
- dados do CID
  - http://www2.datasus.gov.br/cid10/V2008/cid10.htm
    - http://www2.datasus.gov.br/cid10/V2008/downloads/CID10CSV.zip
      - CID-10-SUBCATEGORIAS.CSV

## Catálogo de Dados

### Entidade 'Ocorrência' 

| Nome da Coluna      | Tipo      | Descrição | Domínio |
|---------------------|-----------|-----------|---------------------|
| `#`     | Inteiro   | Identificação do registro (número sequencial)   | Valor mínimo: 1 </br> Valor máximo: 112.491  |
| `estado`           | Texto   | Estado brasileiro de registro do suicídio | RJ, SP, MG e demais siglas dos estados |
| `ano`            | Inteiro     | Ano de registro do suicídio | Valor mínimo: 2010 </br> Valor máximo: 2019 |
| `mes`            | Inteiro     | Mês de registro do suicídio | Valor mínimo: 1 </br> Valor máximo: 12 |
| `DTOBITO`         | Data     | Data do óbito no formato YYYY-MM-DD | Valor mínimo: 2010-01-01 </br> Valor máximo: 2019-12-31 |
| `DTNASC`           | Data  |  Data de nascimento no formato YYYY-MM-DD |
| `sexo`             | Texto  | Sexo registrado | Feminino, Masculino, NA |
| `RACACOR`          | Texto  | Raça ou cor de acordo com a classificação do IBGE | Amarela, Branca, Parda, Preta, Indígena, NA |
| `ASSISTMED`      | Texto     | Recebeu assistência médica? | Sim, Não, NA |
| `escmae`      | Texto     | Escolaridade registrada da mãe  | '1 a 3 anos', '4 a 7 anos', '8 a 11 anos', '12 e mais', 'Nenhuma', 'NA' |
| `ESTCIV`  | Texto     | Estado civil | Casado/a, Solteiro/a, 'União consensual', NA, 'Separado/a judicialmente', Viuvo/a |
| `esc`      | Texto     | Escolaridade (em anos) |  '1 a 3 anos', '4 a 7 anos', '8 a 11 anos', '12 e mais', 'Nenhuma', 'NA' |
| `OCUP`      | Texto     | Ocupação (profissão) | APOSENTADO/PENSIONISTA, ESTUDANTE, 'DONA DE CASA', dentre outros. |
| `CODMUNRES`      | Texto     | Município de residência | 'São Paulo', 'Porto Velho', 'Fortaleza', 'São Simão', 'Sobral', 'Sorocaba', 'Rio de Janeiro', 'São Gonçalo', dentre outros |
| `CAUSABAS`      | Texto     | Causa básica da declaração de óbito (de acordo com CID) |  X600, X700, X705, X850, dentre outros |
| `CAUSABAS_O`      | Texto     | Causa básica da morte (de acordo com CID) |  X600, X700, X705, X850, dentre outros |
| `LOCOCOR`      | Texto     | Local de ocorrência da morte |  Hospital, Domicílio, Outros, 'Via pública', 'Outro estabelecimento de saúde', NA |
| `CIRURGIA`      | Texto     | Indica se foi realizada cirurgia |  Sim, Não, NA |

### Entidade 'Diagnóstico' 

| Nome da Coluna      | Tipo      | Descrição | Domínio |
|---------------------|-----------|-----------|------------------|
| `SUBCAT` | Texto   | Código CID   | A000, X600, X700, X705, X850, W800        |
| `CLASSIF` | Texto   | Indica a categoria do CID   | em branco: não tem dupla classificação; </br> +: classificação por etiologia; </br> *: classificação por manifestação.        |
| `RESTRSEXO` | Texto   |  Indica se o CID só pode ser usado para homens ou mulheres   | em branco: pode ser utilizada em qualquer situação; </br> F: só deve ser utilizada para o sexo feminino; </br> M: só deve ser utilizada para o sexo masculino.        |
| `CAUSAOBITO` | Texto   |  Indica se o CID pode causar óbito   | em branco: não há restrição; </br> N: a subcategoria tem pouca probabilidade de causar óbito.         |
| `DESCRICAO` | Texto   | Descrição do CID   | 'Febre tifóide',  'Cólera não especificada',  'Botulismo', 'Lesão autoprovocada intencionalmente por enforcamento, estrangulamento e sufocação - residência', dentre outros     |
| `DESCRABREV` | Texto   | Descrição abreviada do CID   | 'A01.0 Febre tifoide', 'A00.9 Colera NE', 'A05.1 Botulismo', 'X70.0 Residencia', dentre outros    |
| `REFER` | Texto   | Referencia outro código CID, quando aplicável   | 'B02.2+, 'A18.8+', 'A23.-+', dentre outros        |
| `EXCLUIDOS` | Texto   | Indica códigos excluídos que agora se referem ao respectivo CID   | N975, Q352, S446, dentre outros   |
