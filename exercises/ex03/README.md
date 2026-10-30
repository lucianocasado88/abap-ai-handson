[Início - RAP130 - Crie Apps SAP Fiori com ABAP Cloud e SAP Joule for Developers no Visual Studio Code](../../README.md)

# Exercício 3: Publicar e Visualizar o App Travel

## Introdução

No exercício anterior, você gerou um UI service RAP transacional completo para Travel e Booking usando o seu coding agent e as ADT MCP tools (_veja o [Exercício 2](../ex02/README.md)_).

Neste exercício, você vai publicar o endpoint local de serviço do seu service binding OData V4 e abrir o **SAP Fiori elements App Preview** em um navegador para ver o app Travel em funcionamento.

### Exercícios

- [3.1 - Publicar o Service Binding](#exercício-31-publicar-o-service-binding)
- [3.2 - Visualizar o App Travel no Navegador](#exercício-32-visualizar-o-app-travel-no-navegador)
- [3.3 - Criar a classe ABAP auxiliar](#exercício-33-criar-a-classe-abap-auxiliar)
- [Resumo e Próximo Exercício](#resumo-e-próximo-exercício)

> ℹ️ **Lembrete**: Não se esqueça de substituir todas as ocorrências do placeholder **`###`** pelo seu ID do grupo nos passos do exercício abaixo.

---

## Exercício 3.1: Publicar o Service Binding
[^Topo da página](#)

> Publique o endpoint local de serviço do service binding **`ZUI_TRAVEL_O4###`** para tornar o OData service disponível para preview e consumo.

<details>
  <summary>🔵 Clique para expandir!</summary>

1. Abra o service binding usando **`Ctrl+Shift+A`** e pesquise por **`ZUI_TRAVEL_O4###`**. Selecione a entrada do tipo **Service Binding** (SRVB).

2. Clique no botão **Publish** na barra de ferramentas do editor (área superior do editor do service binding).

   > ℹ️ A ação "Publish" registra um endpoint local de serviço que permite que o preview do Fiori elements e consumidores externos descubram o serviço.

   ![Service binding publicado](images/ex03_publish_service_binding.png)

4. Aguarde a conclusão da publicação. 

   > ✅ Você deve ver na parte inferior a mensagem _Publish/unpublish action in progress_.

</details>

---

## Exercício 3.2: Visualizar o App Travel no Navegador
[^Topo da página](#)

> Abra o SAP Fiori elements App Preview para a entidade Travel.

<details>
  <summary>🔵 Clique para expandir!</summary>

1. Abra o seu service binding **`ZUI_TRAVEL_O4###`** e clique no botão **Preview**.

   ![Preview do service binding](images/ex03_service_binding.png)

2. Selecione a entidade `Travel` e clique em **Open** na caixa de diálogo.
   
   ![Selecionar entidade](images/ex03_select_entity.png)

   Uma janela do navegador é aberta com o **SAP Fiori elements App Preview** da sua aplicação Travel.

2. Tente criar uma nova entidade `Travel`:
   - Clique em **Create** e preencha alguns campos: **Destination**, **Begin Date**, **End Date**
   - Preencha **Agency ID** e **Customer ID** (use quaisquer IDs válidos dos dados de referência de voo DMO, por exemplo `070001` para Agency e `000001` para Customer)
   - Clique em **Save**

   > ✅ Se você conseguir criar e salvar um registro de Travel, o app está funcionando corretamente!

</details>

---

## Exercício 3.3: Criar a classe ABAP auxiliar
[^Topo da página](#)

> Crie uma classe auxiliar (helper class) para popular as tabelas de banco de dados de Travel e Booking com dados de demonstração do ABAP Flight Reference Scenario.

<details>
  <summary>🔵 Clique para expandir!</summary>

Carregar dados de demonstração torna o app mais interessante de usar nos demais exercícios.

1. Abra o **Command Palette** (**`Ctrl+Shift+P`**) e digite `ABAP: Create new ABAP Object`, ou use **`Ctrl+Shift+Alt+N`** (macOS: **`Cmd+Option+Shift+N`**), para criar uma nova classe ABAP.

   - Package: **`ZRAP130_AI_###`**
   - Name: **`ZCL_TRAVEL_HELPER_###`**
   - Description: **`Travel helper class ###`**
  
   Você pode deixar em branco os campos `Superclass` e `Interface`. A classe **`ZCL_TRAVEL_HELPER_###`** deve ter sido criada com sucesso

   ![Criar classe ABAP](images/ex03_create_abap_class.png)


2. Cole o código a seguir na classe, substituindo **`###`** pelo seu ID do grupo:

   ```ABAP
   CLASS zcl_travel_helper_### DEFINITION
     PUBLIC
     FINAL
     CREATE PUBLIC.

     PUBLIC SECTION.
       METHODS: validate_customer
                  IMPORTING iv_customer_id TYPE /dmo/customer_id
                  RETURNING VALUE(rv_exists) TYPE abap_bool.

     PROTECTED SECTION.
     PRIVATE SECTION.
   ENDCLASS.

   CLASS zcl_travel_helper_### IMPLEMENTATION.

     METHOD validate_customer.
       SELECT SINGLE
         FROM /dmo/customer
         FIELDS @abap_true AS line_exists
         WHERE customer_id = @iv_customer_id
         INTO @rv_exists.
     ENDMETHOD.

   ENDCLASS.
   ```

3. Salve (**`Ctrl+S`**) e ative (**`Ctrl+F3`**) a classe.

</details>

---

## Resumo e Próximo Exercício
[^Topo da página](#)

Agora que você:
- Publicou o endpoint local de serviço do service binding **`ZUI_TRAVEL_O4###`**
- Abriu e testou o SAP Fiori elements App Preview no navegador
- Criou um registro de viagem de teste para verificar que o app está funcionando
- Criou uma helper class para uso futuro

Você pode continuar com o próximo exercício: **[Exercício 4: Adicionar uma Validação](../ex04/README.md)**

---
