[Início - RAP130 - Crie Apps SAP Fiori com ABAP Cloud e SAP Joule for Developers no Visual Studio Code](../../README.md)

# Exercício 5: Gerar ABAP Unit Tests 💎

## Introdução

No exercício anterior, você adicionou uma validação de backend para verificar IDs de cliente (_veja o [Exercício 4](../ex04/README.md)_).

Neste exercício, você vai gerar ABAP unit tests para a helper class **`ZCL_TRAVEL_HELPER_###`**, corrigir eventuais problemas e executar os testes, tudo em um único passo.

### Exercícios

- [5.1 - Gerar e Executar Unit Tests](#exercício-51-gerar-e-executar-unit-tests-)
- [Resumo](#resumo)

> ℹ️ **Lembrete**: Não se esqueça de substituir todas as ocorrências do placeholder **`###`** pelo seu ID do grupo nos passos do exercício abaixo.

---

## Exercício 5.1: Gerar e Executar Unit Tests 💎
[^Topo da página](#)

> Use o seu coding agent em Agent mode para gerar unit tests para **`ZCL_TRAVEL_HELPER_###`**, corrigir eventuais problemas e executar os testes até que todos passem.

> ⚠ **Aviso sobre resultados gerados por IA** ⚠
> Esteja ciente de que os resultados gerados por IA na descrição deste exercício podem diferir dos seus. **Sempre revise o código de teste gerado antes de executá-lo.**

<details>
  <summary>🔵 Clique para expandir!</summary>

1. Abra o **GitHub Copilot Chat** em **Agent mode**.

   > 💡 **Dica (GitHub Copilot)**: Você pode começar no **Plan mode** para visualizar as alterações que o GitHub Copilot pretende fazer e depois mudar para o **Agent mode** para executá-las.

2. Digite o seguinte prompt:

   ```
   Gere unit tests para o método validate_customer da classe ZCL_TRAVEL_HELPER_###. Corrija quaisquer problemas e execute os unit tests até que todos passem.
   ```

   > ℹ️ Substitua `###` pelo seu ID do grupo no prompt antes de enviá-lo.

3. O seu coding agent vai criar a classe de teste, gerar os métodos de teste, corrigir eventuais problemas e executar os testes de forma iterativa até que todos passem. Durante esse processo ele vai chamar as MCP tools **`abap_activate-objects`** e **`abap_run_unit_tests`**; permita essas chamadas quando solicitado.

4. Revise os resultados dos testes no painel **Test Results**.

   ![Copilot gerando e executando unit tests](images/ex05_generate_unit_tests.gif)

5. Opcionalmente, confirme executando os unit tests manualmente: abra o **Command Palette** (**`Ctrl+Shift+P`** / **`Cmd+Shift+P`**) e selecione **"ABAP: Run Unit Tests"**.

   ![Executando unit tests pelo Command Palette](images/ex05_run_unit_tests.gif)

6. O seu código deve ficar parecido com este:
   
   > ℹ️ Não se esqueça de substituir **`###`** pelo seu *ID do grupo* atribuído ou pelo sufixo escolhido.

   <details>

      ```ABAP
        
      CLASS ltc_validate_customer DEFINITION FINAL FOR TESTING
         DURATION SHORT
         RISK LEVEL HARMLESS.

         PRIVATE SECTION.
            CLASS-DATA: mo_osql_env TYPE REF TO if_osql_test_environment.
            DATA:        mo_cut      TYPE REF TO zcl_travel_helper_###.

            CLASS-METHODS:
               class_setup,
               class_teardown.
            METHODS:
               setup,
               customer_exists     FOR TESTING,
               customer_not_exists FOR TESTING.

      ENDCLASS.

      CLASS ltc_validate_customer IMPLEMENTATION.

         METHOD class_setup.
            mo_osql_env = cl_osql_test_environment=>create(
               i_dependency_list = VALUE #( ( '/DMO/CUSTOMER' ) ) ).
         ENDMETHOD.

         METHOD class_teardown.
            mo_osql_env->destroy( ).
         ENDMETHOD.

         METHOD setup.
            mo_osql_env->clear_doubles( ).
            mo_cut = NEW zcl_travel_helper_###( ).
         ENDMETHOD.

         METHOD customer_exists.
            " Arrange: insere um cliente na tabela simulada (mock)
            DATA lt_customers TYPE TABLE OF /dmo/customer.
            APPEND VALUE #( customer_id = '000001' ) TO lt_customers.
            mo_osql_env->insert_test_data( lt_customers ).

            " Act
            DATA(lv_result) = mo_cut->validate_customer( iv_customer_id = '000001' ).

            " Assert
            cl_abap_unit_assert=>assert_equals(
               act = lv_result
               exp = abap_true
               msg = 'Customer 000001 should be found' ).
         ENDMETHOD.

         METHOD customer_not_exists.
            " Arrange: a tabela permanece vazia

            " Act
            DATA(lv_result) = mo_cut->validate_customer( iv_customer_id = '999999' ).

            " Assert
            cl_abap_unit_assert=>assert_equals(
               act = lv_result
               exp = abap_false
               msg = 'Customer 999999 should not be found' ).
         ENDMETHOD.

      ENDCLASS.
      ```
   </details>
</details>

---

## Resumo
[^Topo da página](#)

Parabéns! Você concluiu todos os exercícios deste workshop.

Neste exercício final, você:
- Usou o seu coding agent em Agent mode para gerar unit tests para `ZCL_TRAVEL_HELPER_###` em um único passo
- Deixou o seu coding agent corrigir eventuais problemas e executar os testes de forma iterativa até que todos passassem

Você pode continuar com os outros exercícios opcionais:
- **[Exercício 6: Depurar Código ABAP no Visual Studio Code](../ex06/README.md)**
- **[Exercício 7: Criar um Custom Agent](../ex07/README.md)**

---

### O que você construiu neste workshop

Ao longo do RAP130, você:

1. ✅ Configurou o Visual Studio Code com a extensão ADT e conectou a um sistema ABAP
2. ✅ Habilitou e configurou o ADT MCP Server com o seu coding agent
3. ✅ Gerou um app Fiori RAP transacional completo por meio de chamadas de MCP tools conduzidas por IA
4. ✅ Explorou e ajustou os artefatos gerados no Visual Studio Code
5. ✅ Publicou o serviço e visualizou o app SAP Fiori elements no navegador
6. ✅ Adicionou uma validação
7. ✅ Gerou e executou ABAP unit tests 

**[↑ Voltar ao início do tutorial](../../README.md)**

---
