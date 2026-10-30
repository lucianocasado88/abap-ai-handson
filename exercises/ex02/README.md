[Início - RAP130 - Crie Apps SAP Fiori com ABAP Cloud e SAP Joule for Developers no Visual Studio Code](../../README.md)

# Exercício 2: Gerar o App SAP Fiori via MCP Tools

## Introdução

No exercício anterior, você habilitou o ADT MCP Server e verificou que o seu coding agent consegue chamar as ADT MCP tools disponíveis (_veja o [Exercício 1](../ex01/README.md)_).

Neste exercício, você vai usar **o seu coding agent em agent mode** junto com as ADT MCP tools para:
1. Criar o seu pacote de desenvolvimento ABAP no ADT for Eclipse
2. Gerar um app SAP Fiori transacional completo (RAP business object Travel + Booking) usando o ABAP Cloud Generator, tudo conduzido por prompts em linguagem natural no chat do coding agent

### Exercícios

- [2.1 - Criar o Pacote ABAP](#exercício-21-criar-o-pacote-abap)
- [2.2 - Gerar o UI Service Transacional](#exercício-22-gerar-o-ui-service-transacional)
- [Resumo e Próximo Exercício](#resumo-e-próximo-exercício)

> ℹ️ **Lembrete**: Não se esqueça de substituir todas as ocorrências do placeholder **`###`** pelo seu ID do grupo nos passos do exercício abaixo.

---

## Sobre o ABAP Cloud Generator: App Transacional from Scratch

O **ABAP Cloud Generator: OData UI Service from Scratch** gera uma stack RAP completa a partir de uma descrição em linguagem natural do modelo de dados. Ele cria:

- Tabelas de banco de dados (uma por entidade)
- CDS base views (interface views)
- CDS projection views (consumption views)
- Metadata extensions (anotações de UI)
- Behavior definition 
- Classe de implementação do behavior
- Service definition
- Service binding

No [RAP120](https://github.com/SAP-samples/abap-platform-rap120) (Eclipse ADT), esse generator era acionado por meio de `Generate ABAP Repository Object > OData UI Service from Scratch`. No **RAP130**, o generator é exposto como uma MCP tool, chamada pelo seu coding agent a partir da janela de chat no Visual Studio Code.

> ⚠ **Aviso sobre resultados gerados por IA** ⚠
> Os nomes dos artefatos gerados, dos campos de banco de dados e de outros elementos podem diferir dos mostrados neste tutorial, pois são gerados por IA. **Sempre revise os artefatos gerados antes de continuar.**

---

## Exercício 2.1: Criar o Pacote ABAP
[^Topo da página](#)

> Crie o seu pacote do exercício **`ZSITFOR_AI_###`** no **ADT for Eclipse**. 

> ℹ️ **Em breve**: A criação de pacotes diretamente a partir do Visual Studio Code estará disponível em uma próxima versão da extensão ADT for Visual Studio Code.

<details>
  <summary>🔵 Clique para expandir!</summary>

1. Abra o **Eclipse ADT** e conecte ao seu sistema ABAP.

2. No **Project Explorer**, expanda a conexão com o seu sistema e clique com o botão direito em **`ZLOCAL`** → **New → ABAP Package**.

3. Preencha os detalhes do pacote:
   - **Name**: `ZSITFOR_AI_###` (substitua `###` pelo seu ID do grupo)
   - **Description**: `RAP AI Package ###`
   - **Superpackage**: `ZLOCAL`
   - **Package Type**: `Development`

4. Clique em **Next** (ou **Finish**) e atribua o pacote a uma transport request, se for solicitado.

5. Verifique se o pacote **`ZSITFOR_AI_###`** aparece no Project Explorer sob `ZLOCAL`.

6. Volte para o **Visual Studio Code**. Você pode adicionar o pacote criado ao seu workspace.

   Abra o **Command Palette** com **`Ctrl+Shift+P`** (macOS: **`Cmd+Shift+P`**), digite `>ABAP: Add Package as Folder to Workspace` e pressione **Enter**.

   ![Adicionar pacote ao workspace](images/ex02_add_package_workspace.png)

   Preencha com o nome do seu pacote `ZSITFOR_AI_###` e pressione **Enter**

   ![Adicionar pacote ao workspace](images/ex02_add_package_workspace_2.png)

   O workspace deve ficar assim: 
   
   ![Estrutura do workspace](images/ex02_workspace_overview.png)

</details>

---

## Exercício 2.2: Gerar o UI Service Transacional
[^Topo da página](#)

> Use as ADT MCP tools para gerar um OData UI service transacional RAP completo para as entidades Travel e Booking.

> ⚠ **Aviso sobre resultados gerados por IA** ⚠
> Esteja ciente de que os resultados gerados por IA na descrição deste exercício podem diferir dos seus. **Sempre revise o código e os artefatos gerados.**

<details>
  <summary>🔵 Clique para expandir!</summary>

### Passo 1: Descobrir os generators disponíveis

1. No **seu coding agent (Agent mode)**, digite:

   ```
   Liste todos os ABAP RAP generators disponíveis usando a MCP tool.
   ```

   ![Generators listados pela MCP tool](images/ex02_available_generators.png)

### Passo 2: Obter o schema do generator (opcional)

3. Para entender os parâmetros de entrada necessários, você pode perguntar ao seu coding agent:

   ```
   Obtenha o JSON schema do generator OData UI Service from Scratch.
   ```

4. Permita a chamada da tool `abap_generators-get_schema`. Revise o schema, que mostra quais campos o generator espera (nomes de entidades, listas de campos, sufixos etc.).

### Passo 3: Executar o generator

5. No **seu coding agent (Agent mode)**, copie e cole o prompt a seguir. Substitua **`###`** pelo seu ID do grupo:

   ```
   Gere um app SAP Fiori transacional para gerenciamento de viagens usando o ABAP RAP generator.

   Use a seguinte especificação:
   - Pacote: ZSITFOR_AI_###
   - Entidade 1: Travel, baseada na estrutura /DMO/TRAVEL_DATA
   - Entidade 2: Booking, baseada na estrutura /DMO/BOOKING_DATA (filha de Travel)
   - Todos os nomes de objetos gerados devem terminar com o sufixo "###"
   ```

6. O seu coding agent vai planejar a geração. Provavelmente ele vai chamar:
   - `abap_generators-get_schema`: para recuperar o schema de entrada do generator
   - `abap_generators-generate_objects`: para executar a geração

   **Permita cada chamada de tool** quando solicitado.

   > ℹ️ **Nota**: O generator pode levar um momento para executar. O progresso será exibido.

   ![Generator em execução via MCP tool](images/ex02_generator_running.png)

### Passo 4: Revisar os objetos gerados

7. Quando o generator terminar, o seu coding agent vai informar a lista de objetos gerados. Você deve ver entradas para:
   - Tabelas de banco de dados: **`ZTRAVEL###`**, **`ZTRAVEL_D###`**, **`ZBOOKING###`** e **`ZBOOKING_D###`**
   - CDS base views: **`ZR_TRAVEL###`**, **`ZR_BOOKING###`**
   - CDS projection views: **`ZC_TRAVEL###`**, **`ZC_BOOKING###`**
   - Metadata extensions: **`ZC_TRAVEL###`** (DDLX), **`ZC_BOOKING###`** (DDLX)
   - Behavior definition: **`ZR_TRAVEL###`** (BDEF)
   - Classe de implementação do behavior: **`ZBP_R_TRAVEL###`**
   - Classe de projection behavior: **`ZBP_C_TRAVEL###`** 
   - Service definition: **`ZUI_TRAVEL_04###`** (SRVD)
   - Service binding: **`ZUI_TRAVEL_O4###`** (SRVB)

   > ℹ️ **Nota**: Os nomes exatos dos artefatos podem diferir um pouco, pois são gerados por IA e podem variar.

   ![Saída do generator via MCP tool](images/ex02_generator_output.gif)

8. No **Explorer** do Visual Studio Code, pressione **F5** para atualizar o seu pacote **`ZSITFOR_AI_###`**. Você deve ver todos os objetos gerados listados nas respectivas pastas por tipo de objeto.

   ![Objetos gerados no Explorer do Visual Studio Code](images/ex02_generated_objects.gif)

### Passo 5: Ativar todos os objetos gerados

9. Ative todos os objetos inativos de uma só vez usando o atalho de teclado:
   - **`Ctrl+Shift+F3`** (macOS: **`Cmd+Shift+F3`**)

   Ou pela opção `Activate All` na barra de ferramentas inferior

   ![Ativar todos os objetos](images/ex02_activate_all.png)


10. Uma caixa de diálogo mostra todos os objetos inativos. Selecione todos e clique em **Activate**.

    > ⚠️ Se algum objeto falhar na ativação, verifique o painel **Problems** (**`Ctrl+Shift+M`**) para ver os detalhes do erro. Corrija os problemas e ative novamente.

    > ℹ️ **Dica**: Você também pode pedir ao seu coding agent para ativar via MCP: digite `Ative todos os objetos do pacote ZSITFOR_AI_### usando a MCP tool` e permita a chamada da tool `abap_activate-objects`.

</details>

---

## Resumo e Próximo Exercício
[^Topo da página](#)

Agora que você:
- Criou o pacote ABAP **`ZSITFOR_AI_###`** no ADT for Eclipse
- Gerou um UI service transacional RAP completo (Travel + Booking) via ADT MCP tools
- Ativou todos os objetos gerados

você pode continuar com o próximo exercício: **[Exercício 3: Publicar e Visualizar o App Travel](../ex03/README.md)**

---
