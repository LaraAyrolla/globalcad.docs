---
layout: default
title: Persistência de Dados
nav_order: 7
has_children: false
---

# Persistência de Dados
{: .no_toc }

<details open markdown="block">
  <summary>
    Índice
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

---

## Tabelas Associadas a Módulos

Conforme mencionado anteriormente, um módulo é composto por 1 (uma) tela Web, 1 (uma) tela Mobile e outras telas e componentes.

As telas de um módulo obtém e armazenam dados de/em algumas tabelas do banco de dados. Cada módulo utiliza sempre 7 (sete) tabelas, das quais 5 (cinco) são tabelas de sistema - possuem o mesmo formato para todos os módulos -, e 2 (duas) são tabelas de projeto. As 2 (duas) tabelas de projeto são:

- `VALUES`: <mark>Tabela de valores.</mark> Armazena os valores preenchidos pelos usuários nas telas do módulo.
- `DICTIONARIES`: <mark>Tabela de dicionários.</mark> Armazena dicionários que traduzem números em suas representações textuais. Ex.: `415 = Brasil`, `619 = Colômbia`.

---

## Tabela `VALUES`

A tabela `VALUES` persiste, no banco de dados, os valores preenchidos pelos usuários nas telas do módulo. Seu formato é:

<table>
  <tr>
    <th style="text-align:left">CODCONTRATO</th>
    <th style="text-align:left">CODCADASTRO</th>
    <th style="text-align:left">PARENT_CONTAINER_ITEMID</th>
    <th style="text-align:left">ID</th>
    <th style="text-align:left">PARENT_ID</th>
    <th style="text-align:left">REGISTRY_ORDER</th>
    <th style="text-align:left">KEY1..KEY*</th>
    <th style="text-align:left">STR1..STR*</th>
    <th style="text-align:left">INT1..INT*</th>
    <th style="text-align:left">BIGINT1..BIGINT*</th>
    <th style="text-align:left">FLOAT1..FLOAT*</th>
    <th style="text-align:left">DATETIME1..DATETIME*</th>
    <th style="text-align:left">LATLON1..LATLON*</th>
  </tr>
  <tr>
    <td>int</td>
    <td>int</td>
    <td>int</td>
    <td>long</td>
    <td>long</td>
    <td>int</td>
    <td>long</td>
    <td>string</td>
    <td>int</td>
    <td>long</td>
    <td>float</td>
    <td>DateTime</td>
    <td>double</td>
  </tr>
</table>

Cada linha da tabela `VALUES` representa um `container` de um `registro`. Um `registro` representa os dados preenchidos pelo usuário em uma tela do módulo. Por definição, um `registro` é composto por 1 ou mais `containers`, pois as telas do módulo podem possuir mais de um `container`. 

As 6 primeiras colunas da tabela `VALUES` são colunas de sistema, presentes na tabela `VALUES` de qualquer projeto criado na plataforma GlobalCad. As demais colunas são colunas de projeto.

| Coluna                | Tipo      | Descrição                                                        |
|:----------------------|:----------|:-----------------------------------------------------------------|
| `CODCONTRATO`         | `int`     | Identificador numérico do módulo ao qual a linha está associada. Em uma mesma infraestrutura Cloud, não são permitidos 2 módulos com o mesmo `CODCONTRATO`.
| `CODCADASTRO`         | `int`     | Identificador numérico do `registro`. Um `registro` pode ocupar mais de uma linha na tabela `VALUES` caso precise de mais de um `container` para ser inteiramente representado. Por essa razão, um mesmo valor de `CODCADASTRO` pode repetir-se mais de uma vez na tabela `VALUES`.
| `PARENT_CONTAINER_ITEMID`| `int`  | ID do item de tela considerado o pai do `container` representado pela linha. Indica, basicamente, o nível do `container`. `-1` = Nível raíz. Lembre-se: Um `registro` é composto por 1 ou mais `containers`.
| `ID`                  | `long`    | Identificador único do `container` representado pela linha no contexto do `registro` (`CODCADASTRO`). Um mesmo valor de `ID` nunca se repetirá para um mesmo `CODCADASTRO`. Lembre-se: Um `registro` é composto por 1 ou mais `containers`.
| `PARENT_ID`           | `long`    | Identificador único do `container-pai` do `container` representado pela linha.
| `REGISTRY_ORDER`      | `int`     | Ordem do `container` no `registro` (`CODCADASTRO`) em questão.
| `Demais Colunas`      | `vários`  | Colunas de projeto. Nessas colunas serão armazenados os valores preenchidos pelos usuários nas telas do módulo. As colunas do tipo `KEY*` são chaves-estrangeiras para a coluna `KEY_VALUE` da tabela `DICTIONARIES`.

A <mark>chave-primária</mark> da tabela `VALUES` é `CODCONTRATO`, `CODCADASTRO`, `ID`.

---

## Tabela `DICTIONARIES`

A tabela `DICTIONARIES` armazena dicionários que traduzem números em sua representação textual. Sua chave primária é `KEY_VALUE`.

<table>
  <tr>
    <th style="text-align:left">CODCONTRATO</th>
    <th style="text-align:left">DICTID</th>
    <th style="text-align:left">KEY_VALUE</th>
    <th style="text-align:left">KEY_TEXT</th>
    <th style="text-align:left">PARENT_KEY</th>
    <th style="text-align:left">DISPLAY_ORDER</th>
    <th style="text-align:left">KEY1..KEY*</th>
    <th style="text-align:left">STR1..STR*</th>
    <th style="text-align:left">INT1..INT*</th>
    <th style="text-align:left">BIGINT1..BIGINT*</th>
    <th style="text-align:left">FLOAT1..FLOAT*</th>
    <th style="text-align:left">DATETIME1..DATETIME*</th>
    <th style="text-align:left">LATLON1..LATLON*</th>
  </tr>
  <tr>
    <td>int</td>
    <td>int</td>
    <td>long</td>
    <td>string</td>
    <td>long</td>
    <td>int</td>
    <td>long</td>
    <td>string</td>
    <td>long</td>
    <td>float</td>
    <td>DateTime</td>
    <td>double</td>
  </tr>
</table>

Cada linha da tabela representa um `item de dicionário`. Tipicamente, esses itens são apresentados como alternativas em componentes do tipo `dropdown`, `autofilltextbox`, dentre outros, mas também são usados para outras finalidades. As 6 primeiras colunas são colunas de sistema, presentes na tabela `DICTIONARIES` de qualquer projeto criado na plataforma GlobalCad. As demais colunas são colunas de projeto.

| Coluna                | Tipo      | Descrição                                                        |
|:----------------------|:----------|:-----------------------------------------------------------------|
| `CODCONTRATO`         | `int`     | Identificador numérico do módulo ao qual a linha está associada. Em uma mesma infraestrutura Cloud, não são permitidos 2 módulos com o mesmo `CODCONTRATO`.
| `DICTID`             | `int`     | Identificador numérico do dicionário dentro do contexto de um `CODCONTRATO`.
| `KEY_VALUE`          | `long`    | <mark>Chave-primária.</mark> ID único do registro.
| `KEY_TEXT`           | `string`  | Representação textual do registro. Um mesmo `KEY_TEXT` não se repete para um mesmo `ID` e `PARENT_ID`.
| `PARENT_KEY`         | `long`    | ID único do registro-pai do registro. Por exemplo: Suponhando a existência de 2 dicionários, um para o registro de países e outro para o registro de estados, podemos ter, na `DICTIONARIES`, um registro do estado `Minas Gerais` cujo pai é um registro do país `Brasil`.<br/><br/>`PARENT_KEY` é chave estrangeira para a coluna `KEY_VALUE` da própria tabela `DICTIONARIES`.
| `DISPLAY_ORDER`      | `int`     | Ordem de exibição do `item de dicionário`. O primeiro registro a ser exibido é o que contém menor número, enquanto o último é o que contém maior número. Quando o campo contém `null`, os `itens de dicionário` são ordenados alfabeticamente.
| `Demais Colunas`      | `vários`  | Colunas de projeto. Nessas colunas serão armazenadas propriedades de cada `item de dicionário`. Por exemplo: Um item de dicionário que represente o país `Brasil` pode ter propriedades diversas como, por exemplo, IDH, População, Produto Interno Bruto, etc. As colunas do tipo `KEY*` são chaves-estrangeiras para a coluna `KEY_VALUE` da tabela `DICTIONARIES`.

---

## Exemplo de Persistência

Suponhamos que você tenha usado o Form Designer.xlsx para criar uma tela que visa coletar dados de pessoas entrevistadas, como demonstrado abaixo.

Form Designer.xlsx
{: .label .label-green }

<div class="code-example">

<span style="white-space: nowrap">Nome: <input disabled value="" />&nbsp;&nbsp;>> Configurado para salvar o valor na VALUES.STR1</span>
<br/>
<span style="white-space: nowrap">Idade: <input disabled value="" />&nbsp;&nbsp;>> Configurado para salvar o valor na VALUES.INT1</span>
<br/>
<span style="white-space: nowrap">País: <select disabled><option value="Brasil">Brasil</option></select>&nbsp;&nbsp;>> Configurado para salvar o valor na VALUES.KEY1</span>

</div>

Imaginemos que, após o lançamento do módulo, um usuário crie um novo registro e o preencha com as seguintes informações:

<div class="code-example">

Nome: <input disabled value="Juliana Souza" />
<br/>
Idade: <input disabled value="32" />
<br/>
País: <select disabled>
        <option value="Brasil">Brasil</option>
      </select>

</div>

Ao salvar o registro, será criado um novo `item de dicionário` na tabela de dicionários (`DICTIONARIES`) chamado `Brasil`:

<table>
  <tr>
    <th style="text-align:left">CODCONTRATO</th>
    <th style="text-align:left">DICTID</th>
    <th style="text-align:left">KEY_VALUE</th>
    <th style="text-align:left">KEY_TEXT</th>
    <th style="text-align:left">PARENT_KEY</th>
  </tr>
  <tr>
    <td>1000</td>
    <td>1</td>
    <td><b>415</b></td>
    <td>Brasil</td>
    <td>null</td>
  </tr>
</table>

Além de produzir um registro na tabela de dicionários, a operação de salvamento também criará um novo registro na tabela de valores (`VALUES`):

<table>
  <tr>
    <th style="text-align:left">CODCONTRATO</th>
    <th style="text-align:left">CODCADASTRO</th>
    <th style="text-align:left">PARENT_CONTAINER_ITEMID</th>
    <th style="text-align:left">ID</th>
    <th style="text-align:left">PARENT_ID</th>
    <th style="text-align:left">STR1</th>
    <th style="text-align:left">INT1</th>
    <th style="text-align:left">KEY1</th>
  </tr>
  <tr>
    <td>1000</td>
    <td>459</td>
    <td>-1</td>
    <td>202102171819509710</td>
    <td>-1</td>
    <td>Juliana Souza</td>
    <td>32</td>
    <td><b>415</b></td>
  </tr>
</table>

O campo `KEY1` da tabela de valores (`VALUES`) aponta para o país `Brasil` (`KEY_VALUE = 415` na tabela `DICTIONARIES`).
