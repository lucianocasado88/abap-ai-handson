[Início - RAP130 - Crie Apps SAP Fiori com ABAP Cloud e SAP Joule for Developers no Visual Studio Code](../../README.md)

# Exercício 6: Depurar Código ABAP no Visual Studio Code _(Opcional)_

## Introdução

Neste exercício, você vai aprender a depurar (debug) código ABAP de backend diretamente do **Visual Studio Code**, usando o ABAP debugger integrado. Você vai definir breakpoints no behavior pool **`ZBP_R_TRAVEL###`**, disparar o debugger a partir do Fiori elements App Preview e inspecionar variáveis e call stacks, tudo sem sair do Visual Studio Code.

### Exercícios

- [6.1 - Entender o Debugger do Visual Studio Code para ABAP](#exercício-61-entender-o-debugger-do-visual-studio-code-para-abap)
- [6.2 - Definir Breakpoints na classe de implementação](#exercício-62-definir-breakpoints-na-classe-de-implementação)
- [6.3 - Disparar e Anexar o Debugger](#exercício-63-disparar-e-anexar-o-debugger)
- [6.4 - Inspecionar Variáveis e Percorrer o Código](#exercício-64-inspecionar-variáveis-e-percorrer-o-código)
- [6.5 - Usar a Watch View e o Call Stack](#exercício-65-usar-a-watch-view-e-o-call-stack)
- [Resumo](#resumo)

> ℹ️ **Lembrete**: Não se esqueça de substituir todas as ocorrências do placeholder **`###`** pelo seu ID do grupo nos passos do exercício abaixo.

---

## Sobre o ABAP Debugger no Visual Studio Code

<details>
  <summary>Clique para expandir!</summary>

A extensão ADT for Visual Studio Code traz o **ABAP debugger** completo para dentro do Visual Studio Code. O debugger se conecta ao backend ABAP e oferece:

- **Breakpoints** definidos diretamente no editor do Visual Studio Code (clique na margem (gutter) ao lado do número da linha)
- Controles **Step Into / Step Over / Step Return / Continue** na Debug toolbar
- Painel **Variables**: inspecione variáveis locais, parâmetros e field symbols em tempo de execução
- **Watch**: monitore variáveis ou expressões específicas enquanto percorre o código
- **Call Stack**: veja a hierarquia completa de chamadas e salte para qualquer frame
- **Logpoints**: imprima uma mensagem sem interromper a execução (sem precisar modificar o código)

O debugger funciona com todos os tipos de objeto ABAP suportados pelo ADT for Visual Studio Code.

> **Leitura adicional**: [Documentação de Run and Debug do Visual Studio Code](https://code.visualstudio.com/docs/editor/debugging)

Aqui está um exemplo de como é o debug no Visual Studio Code. Os passos abaixo vão guiar você por esse fluxo.

![Visão geral de uma sessão de debug](images/ex06_debug.gif)

</details>

---

## Exercício 6.1: Entender o Debugger do Visual Studio Code para ABAP
[^Topo da página](#)

> Familiarize-se com a visão **Run & Debug** e seus componentes antes de definir o seu primeiro breakpoint.

<details>
  <summary>🔵 Clique para expandir!</summary>

1. Abra a visão **Run & Debug** clicando no ícone de bug/play na **Activity Bar** (borda esquerda do Visual Studio Code), ou pressione **`Ctrl+Shift+D`** (macOS: **`Cmd+Shift+D`**).

2. Revise os painéis visíveis nesta visão:

   | Painel | Finalidade |
   |--------|------------|
   | **Variables** | Mostra variáveis locais, parâmetros e seus valores atuais em um breakpoint |
   | **Watch** | Monitora expressões ou nomes de variáveis que você definir |
   | **Call Stack** | Mostra a hierarquia de chamadas; clique em qualquer frame para saltar para aquele ponto do código |
   | **Breakpoints** | Lista todos os breakpoints definidos no seu workspace; habilite/desabilite cada um individualmente |

3. A **Debug toolbar** aparece no topo do editor quando uma sessão de debug está ativa:

   | Botão | Atalho | Ação |
   |-------|--------|------|
   | Continue | **`F5`** | Executa até o próximo breakpoint |
   | Step Over | **`F10`** | Executa a linha atual e avança para a próxima |
   | Step Into | **`F11`** | Entra em um método chamado |
   | Step Return | **`Shift+F11`** | Executa até o método atual retornar |
   | Stop | **`Shift+F5`** | Encerra a sessão de debug |

   Você pode encontrar mais informações [aqui](https://code.visualstudio.com/docs/debugtest/debugging)

</details>

---

## Exercício 6.2: Definir Breakpoints na classe de implementação
[^Topo da página](#)

> Defina breakpoints no método `validateCustomer` de **`ZBP_R_TRAVEL###`** para poder inspecionar a lógica de validação em tempo de execução.

<details>
  <summary>🔵 Clique para expandir!</summary>

1. Abra o behavior pool **`ZBP_R_TRAVEL###`** usando **`Ctrl+Shift+A`**.

2. Navegue até a implementação do método **`validateCustomer`**. Você pode usar a visão **Outline** (barra lateral esquerda) para ir direto a ele, ou usar **`Ctrl+Shift+O`** para ir a um símbolo no arquivo.

3. **Defina um breakpoint** na primeira linha executável dentro de `validateCustomer` (por exemplo, a instrução `READ ENTITIES`):
   - Clique na **margem (gutter)**, a faixa estreita à esquerda dos números de linha.
   - Um ponto vermelho aparece, indicando que o breakpoint foi definido.

   ![Breakpoint definido na margem (gutter)](images/ex06_set_breakpoint.png)

4. Defina um segundo breakpoint algumas linhas depois, por exemplo na linha `LOOP AT travels`, para poder percorrer o loop passo a passo.

5. Verifique se ambos os breakpoints aparecem na seção **Breakpoints** da visão **Run & Debug**.

   > ℹ️ **Dica**: Você pode **desabilitar** um breakpoint temporariamente clicando no ponto vermelho (ele fica cinza). Assim você o mantém sem parar a execução toda vez. Clique com o botão direito em um breakpoint para ver mais opções (Edit Condition, Add Logpoint etc.).

   > ℹ️ **Logpoints**: Clique com o botão direito na margem (gutter) e selecione **Add Logpoint** para imprimir uma mensagem no Debug Console sem interromper a execução. Útil para rastrear valores sem interromper o fluxo do app.

</details>

---

## Exercício 6.3: Disparar e Anexar o Debugger
[^Topo da página](#)

> Inicie uma sessão de debug e dispare o seu breakpoint usando o app Travel no navegador.

<details>
  <summary>🔵 Clique para expandir!</summary>


### Disparar os breakpoints a partir do app Fiori

4. Abra (ou atualize) o **Fiori elements App Preview**:
   - Abra o service binding **`ZUI_TRAVEL_O4###`** via **`Ctrl+Shift+A`**
   - Selecione a entidade **Travel** e clique em **Preview**

5. No app, clique em **Create** para iniciar um novo registro de viagem.

6. Preencha os campos, incluindo um **Customer ID** (use qualquer valor, válido ou não, por exemplo `000001`).

7. Clique em **Save**.

8. O backend ABAP executa `validateCustomer` ao salvar. O Visual Studio Code **vem automaticamente para o primeiro plano** e a execução é interrompida no seu primeiro breakpoint.


</details>

---

## Exercício 6.4: Inspecionar Variáveis e Percorrer o Código
[^Topo da página](#)

> Use o painel Variables e os controles de debug para inspecionar os dados em tempo de execução e percorrer a lógica de validação passo a passo.

<details>
  <summary>🔵 Clique para expandir!</summary>

1. Com a execução interrompida na linha `READ ENTITIES`, observe o painel **Variables** na visão Run & Debug.

   Você verá variáveis locais e parâmetros, incluindo:
   - `keys`: a tabela interna de chaves de Travel passadas para a validação
   - Quaisquer outros parâmetros do método

2. **Expanda** a variável `keys` clicando na seta. Inspecione as entradas de chave individuais (UUID etc.).

3. Pressione **`F10`** (Step Over) para executar a instrução `READ ENTITIES`.

4. Após o passo, a variável `travels` agora está preenchida. Expanda-a no painel Variables para ver as instâncias de Travel que foram lidas, incluindo o campo `CustomerID`.

5. Pressione **`F10`** novamente para passar pela instrução `SELECT` do cliente. Depois que ela for executada, inspecione a tabela interna `customers`: ela deve conter os registros de cliente correspondentes (ou estar vazia para um ID inválido).

6. Pressione **`F10`** para entrar no `LOOP AT travels`. A cada iteração, passe o mouse sobre `travel` no código: o Visual Studio Code mostra um tooltip com o valor atual dessa variável.

   > ℹ️ **Dica**: Você pode passar o mouse sobre **qualquer variável ou campo** no editor enquanto estiver em um breakpoint para ver o seu valor atual em tempo de execução em um tooltip.

7. Pressione **`F5`** (Continue) para retomar a execução e deixar o app concluir a operação de salvamento.

</details>

---

## Exercício 6.5: Usar a Watch View e o Call Stack
[^Topo da página](#)

> Adicione uma expressão Watch e inspecione o call stack para entender o contexto completo de execução.

<details>
  <summary>🔵 Clique para expandir!</summary>

### Adicionar uma expressão Watch

1. Dispare o breakpoint novamente (crie outro registro de Travel no app e clique em Create).

2. No painel **Variables** da visão Run & Debug, clique com o botão direito na variável que você deseja adicionar ao watch e selecione **Add to Watch**.

3. O painel Watch agora mostra a variável selecionada conforme a execução avança pelo loop, sem que você precise expandir a árvore de variáveis a cada vez.

   > ℹ️ **Dica**: Você pode adicionar qualquer campo ou expressão ABAP válida como entrada do Watch. É útil para monitorar campos muito aninhados.

### Inspecionar o call stack

5. Observe o painel **Call Stack**. Ele mostra a hierarquia completa de chamadas que levou a `validateCustomer`:
   - O disparo do framework RAP (determination/validation dispatcher)
   - O método `validateCustomer` do behavior pool
   - Quaisquer submétodos chamados

6. Clique em qualquer frame do Call Stack para saltar para aquele ponto no código: o editor navega automaticamente para o arquivo e a linha correspondentes.


### Encerrar a sessão de debug

7. Pressione **`Shift+F5`** (Stop) ou clique no botão vermelho **Stop** na Debug toolbar para encerrar a sessão de debug.

8. Os seus breakpoints continuam definidos para sessões futuras. Remova-os clicando nos pontos vermelhos na margem (gutter), ou use **"Remove All Breakpoints"** no menu de contexto do painel Breakpoints.

</details>

---

## Resumo
[^Topo da página](#)

Neste exercício, você aprendeu a:
- Navegar pela visão **Run & Debug** e entender seus painéis (Variables, Watch, Call Stack, Breakpoints)
- Definir **breakpoints** e **logpoints** diretamente na margem (gutter) do editor do Visual Studio Code
- Iniciar uma sessão de debug ABAP e **dispará-la a partir do Fiori elements App Preview**
- **Percorrer o código ABAP passo a passo** usando Step Over, Step Into e Step Return
- **Inspecionar valores em tempo de execução** no painel Variables e passando o mouse sobre variáveis no editor
- Usar **expressões Watch** para monitorar campos específicos ao longo das iterações de um loop
- Navegar pelo **Call Stack** para entender o contexto de execução

Essas habilidades de debug se aplicam a qualquer objeto ABAP do seu sistema, não apenas a behavior pools RAP. Use a mesma abordagem para classes ABAP, function modules e mais.

Você pode continuar com o outro exercício opcional:
- **[Exercício 7: Criar um Custom Agent](../ex07/README.md)**

**[↑ Voltar ao início do tutorial](../../README.md)**

---
