[Início - RAP130 - Crie Apps SAP Fiori com ABAP Cloud e SAP Joule for Developers no Visual Studio Code](../../README.md)

# Exercício 7: Criar um Custom Agent _(Opcional)_

## Introdução

Neste exercício, você vai criar um **custom agent** para adaptar o comportamento do seu coding agent ao desenvolvimento ABAP neste workshop. Um custom agent permite definir instruções específicas, convenções de nomenclatura, contexto e preferências de tools, para que o seu agente se comporte de forma consistente sem que você precise repetir a mesma orientação em cada prompt.

Para o desenvolvimento ABAP com o RAP130, um custom agent é útil para instruir o seu coding agent a:
- Sempre usar as ADT MCP tools nas operações ABAP
- Seguir as convenções de nomenclatura RAP e usar o pacote/ID do grupo corretos
- Manter o estilo do código ABAP consistente com as restrições do ABAP Cloud

> ℹ️ **Nota**: Os passos deste exercício diferem conforme o coding agent que você está usando:
> - **GitHub Copilot**: use o comando integrado do Visual Studio Code para criar o custom agent
> - **Outros coding agents** (por exemplo, Cursor, Cline etc.): crie manualmente um arquivo `agent.md`

A documentação oficial traz um [exemplo de custom agent para desenvolvimento ABAP](https://help.sap.com/docs/abap-cloud/abap-development-tools-for-visual-studio-code/agent-configuration?locale=en-US) que ajuda você a começar.

### Exercícios

- [7.1 - Criar um Custom Agent (GitHub Copilot)](#exercício-71-criar-um-custom-agent-github-copilot-)
- [7.2 - Criar um Custom Agent (Outros Coding Agents)](#exercício-72-criar-um-custom-agent-outros-coding-agents-)
- [Resumo](#resumo)

> ℹ️ **Lembrete**: Não se esqueça de substituir todas as ocorrências do placeholder **`###`** pelo seu ID do grupo nos passos do exercício abaixo.

---

## Exercício 7.1: Criar um Custom Agent (GitHub Copilot) 💎
[^Topo da página](#)

> Use o seletor de agentes do GitHub Copilot Chat para criar um custom agent para desenvolvimento ABAP.

<details>
  <summary>🔵 Clique para expandir!</summary>

1. Abra o **GitHub Copilot Chat** (**`Ctrl+Shift+I`** / **`Cmd+Shift+I`**).

2. Na barra de entrada do chat, clique no **ícone do seletor de agentes** (`</>`) para abrir o menu de agentes.

3. Selecione **"Configure Custom Agents..."** no final da lista. Em seguida, clique em **"+ Create new custom agent..."**

   ![Criando um custom agent no Visual Studio Code](images/ex7_create_custom_agent.gif)

4. O Visual Studio Code abre um novo arquivo `agent.md` para edição (armazenado em `.github/` no seu workspace).

5. Substitua o conteúdo padrão por instruções adaptadas a este workshop RAP130. Por exemplo:

   ```markdown
   # Custom Agent ABAP RAP130

   Você é um desenvolvedor ABAP construindo um app SAP Fiori elements transacional usando o framework RAP e o ADT MCP Server no Visual Studio Code.

   ## Regras
   - Sempre use as ADT MCP tools (`abap_generators-*`, `abap_creation-create_object`, `abap_activate-objects` etc.) para todas as operações de backend ABAP. Nunca gere código ABAP manualmente quando uma MCP tool puder fazer isso.
   - O sufixo do meu ID do grupo é `###`. Use esse sufixo em todos os nomes de artefatos (por exemplo, `ZRAP130_AI_###`, `ZTRAVEL###`, `ZR_TRAVEL###`).
   - Pacote para todos os objetos: `ZRAP130_AI_###`.
   ```

   > ℹ️ Substitua `###` pelo seu ID do grupo nas instruções do agente acima.

6. **Salve** o arquivo (**`Ctrl+S`** / **`Cmd+S`**).

7. Clique novamente no ícone do seletor de agentes (`</>`): o seu novo custom agent agora aparece na lista. Selecione-o para ativá-lo.

   > ✅ Depois de selecionado, o Copilot aplicará automaticamente as instruções do custom agent a todos os prompts.

</details>

---

## Exercício 7.2: Criar um Custom Agent (Outros Coding Agents) 💎
[^Topo da página](#)

> Crie manualmente um arquivo `agent.md` para personalizar o comportamento do seu coding agent (Cursor, Cline ou outros agentes compatíveis com MCP).

> ℹ️ **Sobre o `agent.md`**: o [`agent.md`](https://agents.md/) é um padrão aberto para definir o comportamento de agentes por meio de um arquivo Markdown. Veja [agents.md](https://agents.md/) para a especificação completa e exemplos.

<details>
  <summary>🔵 Clique para expandir!</summary>

1. No Visual Studio Code, crie um novo arquivo na raiz do seu workspace chamado **`agent.md`**.

   > ℹ️ **Dica**: Clique com o botão direito no painel Explorer, selecione **New File** e dê a ele o nome `agent.md`.

2. Adicione instruções adaptadas a este workshop RAP130. Por exemplo:

   ```markdown
   # Custom Agent ABAP RAP130

   Você é um desenvolvedor ABAP construindo um app SAP Fiori elements transacional usando o framework ABAP RESTful Application Programming Model e o ADT MCP Server no Visual Studio Code.

   ## Regras
   - Sempre use as ADT MCP tools (`abap_generators-*`, `abap_creation-create_object`, `abap_activate-objects` etc.) para todas as operações de backend ABAP. Nunca gere código ABAP manualmente quando uma MCP tool puder fazer isso.
   - O sufixo do meu ID do grupo é `###`. Use esse sufixo em todos os nomes de artefatos (por exemplo, `ZRAP130_AI_###`, `ZTRAVEL###`, `ZR_TRAVEL###`).
   - Pacote para todos os objetos: `ZRAP130_AI_###`.
   ```

   > ℹ️ Substitua `###` pelo seu ID do grupo nas instruções do agente acima.

3. **Salve** o arquivo (**`Ctrl+S`** / **`Cmd+S`**).

4. Abra o seu coding agent e verifique se ele carrega o arquivo `agent.md`. O mecanismo exato depende do seu agente; consulte a documentação dele para saber como ele carrega instruções personalizadas.

   > ✅ Depois de carregadas, o seu agente aplicará essas instruções automaticamente a todos os prompts.

</details>

---

## Resumo
[^Topo da página](#)

Neste exercício, você:
- Criou uma definição de custom agent para adaptar o comportamento do seu coding agent ao desenvolvimento ABAP RAP
- Configurou o agente com o seu ID do grupo, o nome do pacote, as convenções de nomenclatura e a preferência pelo uso das ADT MCP tools

Um custom agent economiza tempo em todos os exercícios: você não precisa mais repetir o contexto sobre o seu ID do grupo, o pacote ou as convenções RAP em cada prompt.

**[↑ Voltar ao início do tutorial](../../README.md)**

---
