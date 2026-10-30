[Início - RAP130 - Crie Apps SAP Fiori com ABAP Cloud e SAP Joule for Developers no Visual Studio Code](../../README.md)

# Exercício 4: Adicionar uma Validação

## Introdução

No exercício anterior, você gerou o app SAP Fiori usando o ADT MCP Server (_veja o [Exercício 3](../ex03/README.md)_).

Neste exercício, você vai usar **o seu coding agent em Agent mode** para definir e implementar uma **validação** de backend chamada **`validateCustomer`** em um único passo: tanto a declaração na behavior definition quanto a implementação no behavior pool serão feitas pelo seu coding agent.

### Exercícios

- [4.1 - Definir e Implementar a Validação](#exercício-41-definir-e-implementar-a-validação)
- [4.2 - Visualizar e Testar o App Travel Aprimorado](#exercício-42-visualizar-e-testar-o-app-travel-aprimorado)
- [Resumo e Próximo Exercício](#resumo-e-próximo-exercício)

> ℹ️ **Lembrete**: Não se esqueça de substituir todas as ocorrências do placeholder **`###`** pelo seu ID do grupo nos passos do exercício abaixo.

---

## Exercício 4.1: Definir e Implementar a Validação
[^Topo da página](#)

> Use o seu coding agent em Agent mode para declarar `validateCustomer` na behavior definition **`ZR_TRAVEL###`** e implementá-la em **`ZBP_R_TRAVEL###`** de uma só vez.

> ⚠ **Aviso sobre resultados gerados por IA** ⚠
> Esteja ciente de que os resultados gerados por IA na descrição deste exercício podem diferir dos seus. **Sempre revise o código gerado antes de ativar.**

<details>
  <summary>🔵 Clique para expandir!</summary>

1. Abra a behavior definition **`ZR_TRAVEL###`** e vá para o **GitHub Copilot Chat** em **Agent mode**.

   > 💡 **Dica (GitHub Copilot)**: Você pode começar no **Plan mode** para visualizar as alterações que o GitHub Copilot pretende fazer e depois mudar para o **Agent mode** para executá-las.

2. Digite o seguinte prompt:

   ```
   Adicione a validação validateCustomer. Na sua implementação, deve ser usado o método validate_customer da classe ABAP ZCL_TRAVEL_HELPER_###. Peça para revisar as alterações antes de prosseguir com a ativação
   ```

   > ℹ️ Substitua `###` pelo seu ID do grupo no prompt antes de enviá-lo.

3. O seu coding agent vai propor alterações tanto na behavior definition quanto na classe de implementação. **Revise as alterações com atenção** antes de confirmar.

4. Confirme a ativação quando ele solicitar.

   ![Copilot definindo e implementando a validação validateCustomer](images/ex04_add_validation.gif)

5. O seu código deve ficar parecido com este:
  
   > ℹ️ Não se esqueça de substituir **`###`** pelo seu *ID do grupo* atribuído ou pelo sufixo escolhido.

   <details>

      ```ABAP
         CLASS LHC_ZR_TRAVEL### DEFINITION INHERITING FROM CL_ABAP_BEHAVIOR_HANDLER.
            PRIVATE SECTION.
               METHODS:
                  GET_GLOBAL_AUTHORIZATIONS FOR GLOBAL AUTHORIZATION
                  IMPORTING
                     REQUEST requested_authorizations FOR Travel
                  RESULT result,
                  validateCustomer FOR VALIDATE ON SAVE
                  IMPORTING keys FOR Travel~validateCustomer.
         ENDCLASS.

         CLASS LHC_ZR_TRAVEL### IMPLEMENTATION.
            METHOD GET_GLOBAL_AUTHORIZATIONS.
            ENDMETHOD.

            METHOD validateCustomer.
               DATA(lo_travel_helper) = NEW zcl_travel_helper_###( ).

               READ ENTITIES OF zr_travel### IN LOCAL MODE
                  ENTITY Travel
                  FIELDS ( CustomerID )
                  WITH CORRESPONDING #( keys )
                  RESULT DATA(lt_travel).

               LOOP AT lt_travel INTO DATA(ls_travel).
                  IF lo_travel_helper->validate_customer( ls_travel-CustomerID ) = abap_false.
                  APPEND VALUE #( %tky = ls_travel-%tky ) TO failed-travel.
                  APPEND VALUE #(
                     %tky = ls_travel-%tky
                     %msg = new_message_with_text(
                        severity = if_abap_behv_message=>severity-error
                        text     = |Customer { ls_travel-CustomerID } does not exist| ) )
                     TO reported-travel.
                  ENDIF.
               ENDLOOP.
            ENDMETHOD.
         ENDCLASS.
      ```
   </details>

 6. **OPCIONALMENTE**, você também pode experimentar implementar a validação `validateCustomer` usando o **Joule Predictive Code Completion**. Apenas certifique-se de que a configuração dele está habilitada.

   ![Joule predictive code completion](/exercises/ex04/images/ex04_code_completion.gif)

   - Abra o **Command Palette** com **`Ctrl+Shift+P`** (macOS: **`Cmd+Shift+P`**)
   - Digite `Preferences: Open Settings (UI)` e selecione **Preferences: Open Settings...**.
   - Na barra de pesquisa, digite:

      ```
      adt code completion
      ```

   - Localize a configuração **"Adt > Joule > Editor: Predictive Code Completion"** e **habilite-a** marcando a caixa de seleção.

      ![Configuração do Joule predictive code completion](/exercises/ex04/images/ex04_code_completion_setting.png)

</details>

---

## Exercício 4.2: Visualizar e Testar o App Travel Aprimorado
[^Topo da página](#)

> Teste a validação `validateCustomer` no Fiori elements App Preview.

<details>
  <summary>🔵 Clique para expandir!</summary>

1. Abra o service binding **`ZUI_TRAVEL_O4###`** (use **`Ctrl+Shift+A`** para encontrá-lo), selecione a entidade **Travel** e clique em **Preview**.

2. No app, clique em **Create** para iniciar um novo registro de viagem.

3. No campo **Customer ID**, informe um **ID de cliente inválido**, por exemplo `999999`.

4. Clique em **Save**.

   > ✅ O app deve exibir uma mensagem de erro de validação: **"Customer 999999 does not exist"** (ou similar). O salvamento é rejeitado.

   ![Erro de validação para cliente inválido](images/ex04_customer_does_not_exist.png)

6. Agora corrija o **Customer ID** para um válido (por exemplo, `000001`) e clique em **Save** novamente.

   > ✅ O registro de viagem é salvo com sucesso.

</details>

---

## Resumo e Próximo Exercício
[^Topo da página](#)

Agora que você:
- Usou o seu coding agent em Agent mode para definir e implementar a validação `validateCustomer` em um único passo
- Testou a validação no Fiori elements App Preview, confirmando que IDs de cliente inválidos são rejeitados

você pode continuar com os exercícios opcionais:
- **[Exercício 5: Gerar ABAP Unit Tests](../ex05/README.md)**
- **[Exercício 6: Depurar Código ABAP no Visual Studio Code](../ex06/README.md)**

---
