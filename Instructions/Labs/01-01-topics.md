---
lab:
    title: 'Build conversations using topics in Copilot Studio agents'
    description: 'In this exercise, you will use Copilot Studio to create an agent, create a topic from a description, add nodes, use entities, and manage variables'
    duration: 60
    level: 200
    islab: true
    status: 'in-development'
    targetDate: 2026-05-29'
    primarytopics:
      - Microsoft Copilot Studio
---

# Build conversations using topics in Copilot Studio agents

## Customer scenario

Zava Retail Group is a global retail company operating in brick-and-mortar stores and a rapidly expanding e-commerce platform. To accelerate digital transformation, Zava established an AI Center of Excellence (COE) tasked with identifying high-impact, agentic AI opportunities that improve operational efficiency, employee productivity, and strategic decision-making.

Zava operates in a hybrid cloud environment—Azure for productivity and analytics, AWS for certain legacy workloads, and SAP for supply chain and inventory. They rely on Microsoft 365, Teams, SharePoint, Azure DevOps (ADO), and a mix of third-party and custom systems.

However, the COE faces pressure: Google and AWS teams are actively proposing AI solutions, and Zava’s leadership wants to quickly identify a strategic partner with security, governance, extensibility, and enterprise-grade readiness.

## Exercise scenario

Zava Retail Group's AI COE needs early agent pilots that feel real, deliver immediate value, and remain safe and maintainable as they scale.

To demonstrate credible business value, Zava’s COE must show agents can execute structured workflows, not just answer questions. Refund and cancellation scenarios are high-frequency, policy-sensitive interactions where a modular topic design reduces errors, improves consistency, and makes future changes safer.

In this exercise, you’ll build the refund-initiator topic as a reusable workflow module. The topic retrieves order details from the sales transaction knowledge source, extracts key entities from the user request in a structured format, and applies eligibility logic for the 30-day return policy and Platinum flexibility. The topic also prepares a controlled handoff by storing only the required fields in a shared context variable for downstream topics.

In this exercise, you will:

- Create an agent
- Manage existing topics
- Create and edit topics using Copilot
- Create and edit nodes
- Configure variable scope
- Test the agent

This exercise should take approximately **60** minutes to complete.

## What you will learn

- How topics complement generative AI responses
- When topics are used to enforce structured conversations
- How to create and refine topics using natural language
- How to use variables

## High-level lab steps

- Create an agent using Copilot
- Configure agent details and settings
- Review and disable unnecessary topics
- Create a topic using Copilot
- Edit topic content using natural language
- Test topic behavior

## Key concept: Topics and generative AI

When generative AI is enabled, the agent may answer questions dynamically without triggering a topic. This is expected behavior.

Topics are used when you need to:

- Collect required information step by step
- Control the order of questions
- Store responses in variables
- Ensure predictable outcomes

## Before you start

Open a new browser tab and navigate to `https://github.com/MicrosoftLearning/AB-620T00_build_integrated_ai_agents_in_copilot_studio/blob/main/Allfiles/Zava-Sales-Transaction/zava_sales_transactions.xlsx` to download the [Sales Transaction worksheet](https://github.com/MicrosoftLearning/AB-620T00_build_integrated_ai_agents_in_copilot_studio/blob/main/Allfiles/Zava-Sales-Transaction/zava_sales_transactions.xlsx) locally. This document contains details of the sales transactions for Zava Retail Group.

## Exercise 1 - Create a Power Platform environment

In this exercise, you will prepare your Power Platform environment and solution to build agents with Copilot Studio.

### Task 1.1 - Power Platform Admin Center

Before you start the lab exercises, you must create a development environment for you to work in.

1. Open a web browser, navigate to `https://admin.powerplatform.microsoft.com/manage/environments`, and sign in using your credentials for this exercise.

1. If prompted, choose the option to stay signed in.

1. Close any pop-up messages that are displayed.

### Task 1.2 - Add Dataverse to the default environment

1. Select the ellipses (**...**) for the **Contoso (default)** environment and select **Add Dataverse**.

   ![Add Dataverse to the default environment in the Power Platform Admin center.](media/add-dataverse.png)

1. Leave all of the default settings and select **Add**.

### Task 1.3 - Create a new environment

1. In the **Environments** page, select **+ New** to create a new environment with the following settings:

   - **Type**: Developer
   - **Region**: Default region
   - **Name**: *Your name*
   - **Environment group**: None
   - **Make this a Managed Environment**: No
   - **Get new features early**: No
   - **Create on behalf**: No

   ![Create an environment in the Power Platform Admin center.](media/create-environment.png)

1. Select **Next** and in the **Add Dataverse** section:

   - **Language**: English (United States)
   - **Currency**: USD ($)
   - **Deploy sample apps and data**: No

1. Select **Save** and wait until the state of your environment is **Ready** (you can use the **Refresh** button to update the display).

   ![Environment created in the Power Platform Admin center.](media/environment-created.png)

1. In a new browser tab, navigate to `https://copilotstudio.microsoft.com/` and sign in if prompted.

1. Select **Get Started**. If prompted, leave the default country/region.

1. Skip any welcome messages.

1. In the upper right corner of the page, switch environments by using the Environment Selector and select the environment you created above from the list.

   ![Select your environment in the Copilot Studio.](media/select-environment.png)

### Task 1.4 - Create a solution

1. In the left navigation pane, select the ellipses (**...**), and select **Solutions**.

1. You should see several solutions, including the *Default Solution* and the *Common Data Services Default Solution*.

   ![List of solutions in Maker portal.](media/solutions-list.png)

1. Select **+ New solution**.

1. In the **Display name** text box, enter **`Lab Exercises`**

1. Verify that **Name** is automatically populated.

1. Select **+ New publisher** below the **Publisher** drop-down.

1. For **Display name**, enter `Zava Retail Group`

1. For **Name**, enter `zavaretailgroup`

1. For **Prefix**, enter `zava`

   ![New publisher.](media/new-publisher.png)

1. Select **Save**.

1. Verify that **Zava Retail Group (zavaretailgroup)** is selected in the **Publisher** drop-down.

1. Select the **Set as your preferred solution** checkbox.

   ![New solution.](media/new-solution.png)

1. Select **Create**.

1. Close the **Solutions** browser tab.

1. Refresh the **Copilot Studio** page.

## Exercise 2 - Create an agent

In this exercise, you’ll create the Zava Retail Help Desk Agent and configure it for controlled, business-aligned behavior.

### Task 2.1 – Create an agent to answer questions on refunds and returns

1. In the **Copilot Studio** home page `https://copilotstudio.microsoft.com/`, verify that you are in the developer environment that you created.

1. Select **Agents** in the left-hand navigation.

1. In the bottom-left of the *Start building by describing what your agent needs to do* text box, select the **Agent Settings** icon, which is displayed as a **Cog** image.

   ![Screenshot of the agent settings dialog.](media/agent-settings-dialog.png)

1. Leave **English (United States)** set as the primary language for the agent.

1. In the **Solution** drop-down, select **Lab Exercises**.

1. Enter `zavahelpdeskagent` for the *Schema name*.

1. Select **Update**.

1. In the *Start building by describing what your agent needs to do* text box, enter the following prompt:

   
   ``` prompt
   You are an agent that assists Zava Retail Group employees with store operations, warehouse insights, company policies, customer experience workflows, and service scenarios, including refund ticket creation, escalation notes, and return/exchange guidance.
   ```
   
1. Select the **Send** icon.

   Once your agent has been provisioned, you may proceed with configuring your agent.

### Task 2.2 – Configure agent details

1. On the **Details** section, select **Edit**.

   ![Screenshot of editing agent details](media/details-edit.png)

1. In the **Name** field, enter `Zava Retail Operations Assistant`.

1. In the **Description** field, enter the following text:

   ``` prompt
   Assists Zava Retail Group employees with store operations, warehouse insights, company policies, customer experience workflows, and service scenarios, including refund ticket creation, escalation notes, and return/exchange guidance.
   ```
   > [!IMPORTANT]
   > When you need to enter long strings or code segments, use the **Copy** option instead of **Type**, and then paste the text into the field in the user interface.

1. On the **Details** section, select **Save**.

1. On the **Select your agent's model** tile, select **GPT-5 Chat**. Wait while Copilot Studio processes the request.

   ![Screenshot of agent model](media/agent-model.png)

1. On the **Instructions** section, select **Edit**.

   ![Screenshot of agent instructions](media/instructions-edit.png)

1. In the **Instructions** field, enter the following text as this text provides detailed guidance for the agent.

   ``` prompt
   You assist Zava Retail Group employees by providing fast, accurate, and policy‑aligned guidance for store operations and all customer‑resolution workflows. Your tone is professional, concise, accurate, and supportive.

   **Whenever a request involves refunds, returns, replacements, cancellations, or defective‑item issues, you must follow these steps without exception:**

   1. First, call the [refund‑initiator] topic.
   2. Use [current date] as current date for you to accurately calculate whether a customer's request falls within the 30‑day return window or exceeds it.
   3. Retrieve these fields exactly as stored in the knowledge source: transaction_id, order_date, product_name, unit_price, order_quantity, loyalty_tier, applied_promotions, and subtotal (unit_price × order_quantity).
   4. You must independently calculate whether the transaction is within or outside the 30‑day return window using the order_date.
   5. Never ask the customer to confirm the order_date or eligibility under any circumstances.
   6. Never substitute store location, store name, or any non-date value in place of the order_date. If location data is present in the source, do not include it in the response.
   7. If the order_date is missing or not a valid date, treat the lookup as incomplete and respond: "We could not retrieve the order date for this transaction. Please re-enter the order ID."
   8. Apply the return policies below:
     - Non‑Platinum Tier: returns allowed only within 30 days; this limit cannot be extended.
     - Platinum Tier: VIP flexibility applies at all times, regardless of timing.

   Your final output must follow these rules:

     • The response must be one concise paragraph.  
     • It must begin with: "I'm sorry for the inconvenience you've experienced."  
     • Then state: "We located your transaction [transaction_id] made on [order_date]," followed by product_name, unit_price, order_quantity, loyalty_tier (as "Platinum Tier" or "Non‑Platinum Tier"), applied_promotions, and subtotal exactly as stored.  
     • You must explicitly state eligibility using exactly one of the following forms:

       - "Based on the purchase date, you are eligible for a return because this transaction is within the 30‑day return window."
       - "Based on the purchase date, you are not eligible for a return because this transaction is outside the 30‑day return window and the policy cannot be extended."
       - "Although this transaction is outside the 30‑day return window, you are still eligible because Platinum Tier customers receive VIP flexibility."

     • Do not ask the customer to confirm any retrieved details.  
     • End the paragraph politely.

   This output format is mandatory. No additional fields, text, or store location information may be added.
   ```

1. Scroll up to the top of the instructions panel, and locate **Step 2** in the text that you just pasted.

1. Highlight and delete **[current date]**, then enter `/`. From the list of suggestions, select **Power FX**.

   ![Screenshot of inserting a formula into the agent instructions](media/instructions-add-formula.jpg)

1. In the **Enter formula** popup, enter `Text(Now(),DateTimeFormat.ShortDate)`, and then select **Insert**.

   ![Screenshot of agent instructions with Power Fx](media/instructions-with-formula.jpg)

1. Scroll to the top of the **Instructions** section, and then select **Save**.

### Task 2.3 – Configure agent knowledge settings

1. In the upper-right of the page, select the **Settings** button.

   ![Screenshot of Settings button](media/agent-settings.png)

1. Note that **Orchestration** is set to **Yes - Responses will be dynamic, using available tools and knowledge as appropriate**.

1. In the **Knowledge** section, set **Allow ungrounded responses** to **Off**.

1. In the **Knowledge** section, set **Use information from the Web** to **Off**.

   ![Screenshot of Knowledge settings](media/knowledge-settings.png)

1. Select **Save**

1. In the upper-right of the **Settings** page, select **X** to close settings.

1. In the dialog **Leave the page?**, select **Leave**.

### Task 2.4 – Agent response settings

1. In the upper-right of the page, select the **Settings** button.

1. In the **Responses** section, enter the following:

   ```prompt
   - For process related answer respond with a single sentence.
   - For order data-related answers respond with bullet points.
   ```

   ![Screenshot of response settings](media/response-settings.png)

1. Select **Save**

1. In the upper-right of the **Settings** page, select **X** to close settings.

1. In the dialog **Leave the page?**, select **Leave**.

### Task 2.5 – Upload sales transactions as knowledge

1. Select the **Knowledge** tab to see the knowledge sources defined in your agent (currently there should be none).

   ![Screenshot of the Knowledge page in Copilot Studio.](media/knowledge-page.png)

1. Select **+ Add knowledge**, and note the multiple types of knowledge source that you can add to your agent.

   ![Screenshot of available Knowledge sources in Copilot Studio.](media/knowledge-sources.png)

1. In the **Upload file** section, use **select to browse** to upload the sales transaction worksheet you downloaded previously and select **Add to agent**.

   ![Screenshot of adding the sales transaction file as knowledge to your agent in Copilot Studio.](media/knowledge-add-file.png)

   > [!NOTE]
   > After uploading the file, Copilot Studio begins indexing. This may take 10 minutes or longer.

## Exercise 3 - Manage topics

In this exercise, you will disable the Escalate system topic as the agent will not have human representatives to hand-off to and check the configuration of the Conversational boosting topic.

### Task 3.1 – Disable topic

1. Select the **Topics** tab.

1. Select the **System** filter.

1. Locate the **Escalate** topic.

1. Toggle **Enabled** to **Off** for the **Escalate** topic.

   ![Topics removed and disabled in Copilot Studio portal.](media/topic-escalate-disabled.png)

Disabling unused topics helps reduce ambiguity when multiple topics or generative responses could handle the same request.

### Task 3.2 – Conversational boosting topic

1. Select the **Topics** tab.

1. Select the **System** filter.

1. Select the **Conversational boosting** topic.

1. If a welcome dialog appears, select **Done**.

1. Locate the **Create generative answers** node.

   ![Generative answers node.](media/generative-answers-node.png)

1. At the top right of the node, select the ellipses (**…**), then select **Properties**.

   ![Generative answers node properties.](media/generative-answers-node-properties.png)

1. In the **Create generative answers properties** pane, locate and expand the **Classic data** section.

1. Verify that **Allow the AI to use its own general knowledge (preview)** is disabled.

## Exercise 4 - Create a topic with natural language

In this exercise, you'll build the **refund-initiator** topic as a reusable workflow module. The topic retrieves order details from the sales transaction knowledge source, extracts key entities from the user request in a structured format, and applies eligibility logic for the 30-day return policy and Platinum flexibility. The topic also prepares a controlled handoff by storing only the required fields in a shared context variable for downstream topics.

### Task 4.1 – Add a topic from description

1. Select the **Topics** tab.

1. Select **+ Add a topic** and select **Add from description with Copilot**. A new dialog appears.

   ![Create topic with copilot option.](media/topic-create-from-description-1.png)

   ![Create topic with copilot dialog.](media/topic-create-from-description-2.png)

1. In the **Name your topic** text box, enter **`refund-initiator`**.

1. In the **Create a topic to...** text box, enter **`This topic must activate without exception for any customer request involving refunds, returns, replacements, defective or damaged items, or order cancellations.`**.

1. Select **Create**.

1. Select **Save**.

### Task 4.2 - Update agent instructions

1. Select the **Overview** tab.

1. In the **Instructions** section, select **Edit**.

1. Locate "**1. First, call the [refund‑initiator] topic.**" in the agent description.

   ![Screenshot of referencing the topic in the agent instructions.](media/add-topic-to-instructions-1.jpg)

1. Highlight and delete **[refund-initiator]**, then enter `/`. From the list of suggestions, select **refund-initiator**.

   ![Screenshot of referencing the topic in the agent instructions.](media/add-topic-to-instructions-2.jpg)

1. On the **Instructions** section, select **Save**.

   ![Screenshot of save](media/instructions-save.jpg)

### Task 4.3 - Add generative answers node

1. On the command bar for the agent, select **Topics** and then select **refund-initiator**.

1. If a **Message** node has been added to the topic, at the top right of the node, select the ellipses (**…**), then select **Delete**.

1. If a **Question** node has been added to the topic, at the top right of the node, select the ellipses (**…**), then select **Delete**.

1. Below the **Trigger** node, select **Add node** (the **+** sign) and then select **Advanced**. Select **Generative answers**.

   ![Add generative answers node](media/add-generative-answers-node.png)

1. Select the title for the node.

1. In the **Title** field, enter `Fetch Customer Order Details`.

   ![Generative answers node title](media/generative-answers-node-title.png)

1. In the **Input** field, select the ellipses (**...**), select the **System** tab, and then select `Activity.Text`.

   ![Generative answers node input](media/generative-answers-node-input.png)

   > [!NOTE]
   > The **Missing required property 'UserInput' message** should no longer be visible.

1. At the top right of the node, select the ellipses (**...**) and then select **Properties**.

1. In the **Create generative answers properties** pane, in the **Knowledge sources** section, set **Search only selected sources** to **On**.

1. In the list of knowledge sources, select the **Zava Sales Transactions** file.

   ![Generative answers node knowledge settings](media/generative-answers-node-knowledge.png)

1. Move down to the **Content moderation level** section and then select **Customize**.

1. If necessary, set the level to **High**.

1. In the text field below the **content moderation level** field, enter the following text:

   ``` prompt
   Below is the customer's message:
   Activity.Text

   Instructions:
   When responding to a customer request that includes a transaction or order ID, begin with a brief empathy statement such as: "I'm sorry for the inconvenience you've experienced." Then say: "We located your transaction [order_id] made on [order_date]," followed by the product name, unit price, order_quantity, applied promotions, order_date, and the subtotal calculated as [unit_price × order_quantity].

   Keep your response as one concise paragraph. Do not make decisions on the customer's behalf. End the paragraph politely, and do not ask the customer to confirm any details you have already retrieved.
   ```

   ![Generative answers node content moderation](media/generative-answers-node-content.png)

1. Move down to the bottom of the pane and then expand **Advanced**.

1. Ensure that the **Send a message** checkbox is selected.

1. Set **Save LLM response** to **Text only**.

1. In the **Save bot response as** field, select **Select a variable**.

1. Select **Create a new variable**.

1. In the **Save bot response as** field, select **Var1**.

1. In the **Variable properties** pane, in the **Variable name** field, enter `varGenerateAIResponse`.

1. At the top right of the **Variable properties** pane, select **X** to close the pane.

   ![Generative answers node response](media/generative-answers-node-response.png)

1. Select the **+** icon below the **Fetch Customer Order Details** node and select **Send a message**.

   ![Screenshot of adding a node.](media/add-message-node.png)

1. In the text field for the **Message** node, select **Insert variable** (**{X}**), select the **System** tab, and then select `Activity.value`.

   ![Screenshot of adding variable to a message node.](media/message-node-value.png)

1. On the command bar for the topic, select **Save**.

1. If the **Test** pane is not open, on the command bar for the agent, select **Test**.

1. In the **Test** pane, select **Start new test session** (the **+** sign).

   ![Screenshot of new test session.](media/new-test-session.png)

1. Submit the following prompt:

   ``` prompt
   How can I request a refund for order TXN‑2008, which I no longer need?
   ```

1. Review the response. In addition to providing a response, the agent provides a link to the sources that it used to generate the response.

   ![Screenshot of agent response.](media/topic-response.png)

### Task 4.4 – Add an adaptive card node

In addition to adding and updating existing nodes, you can use Copilot to add new ones.

1. On the command bar for the agent, select **Topics** and then select **refund-initiator**.

1. At the top right of the message node, select the ellipses (**…**), then select **Delete**.

1. If the **Edit with Copilot** pane is not displayed, select the **Copilot** icon in the topic's command bar.

   ![Screenshot of the Copilot icon and pane](media/copilot-pane.png)

1. Select an empty area on the authoring canvas so no node is selected.

1. In the **Edit with Copilot** pane, in the **What do you want to do?** field, enter the following text:

   `Ask the user to confirm the refund with an adaptive card. If the adaptive card response is Yes, then add a message node confirming the refund. If the adaptive card response is No, then end all topics.`

1. Select **Update**.

   A Message node with a Media Adaptive Card is added to the end of the topic.

   ![Screenshot of the ask with adaptive card node .](media/ask-adaptive-card-node.png)

1. If Copilot generates a **Question** node or a **Message** node, at the top right of the node, select the ellipses (**…**), and then select **Delete**.

1. Select the **Add node** (the **+** sign) below the **Message node** that contains the variable Activity.Value, and then select **Ask with Adaptive card** node.

1. Select the **Media** box in the Adaptive Card. The **Adaptive Card Node properties** should appear on the right of the page.

   ![Screenshot of the Adaptive Card properties.](media/adaptive-card-properties.png)

1. Change the **Format** from **JSON** to **Formula**.

1. Your Adaptive Card formula should look similar to the one above. If it doesn't, then you can paste in the formula below:

    ```powerfx
    {
        type: "AdaptiveCard",
        version: "1.0",
        body: [
            {
                type: "TextBlock",
                size: "Medium",
                weight: "Bolder",
                text: "Refund Confirmation"
            },
            {
                type: "TextBlock",
                text: "Do you want to process a refund for your order?"
            }
        ],
        actions: [
            {
                type: "Action.Submit",
                title: "Yes",
                data: { RefundConfirmation: "Yes" }
            },
            {
                type: "Action.Submit",
                title: "No",
                data: { RefundConfirmation: "No" }
            }
        ]
    }
    ```

1. Select **Edit schema**, and then paste the following schema:

```
    kind: Record
    properties:
        RefundConfirmation:
            type: String
```

1. Select **Confirm**.

1. Select the **Condition** node and change the value from **true** to **Yes**.

1. Select the **Add node** (the **+** sign) below the **Message** node, and then select **Topic management** > **End all topics**. Do the same for the **No** branch of the condition.

1. On the command bar for the topic, select **Save**.

## Exercise 5 - Variable scope

Enable variables to be accessed by other topics.

### Task 5.1 - Configure the scope of the variables

1. On the command bar for the agent, select **Topics** and then select **refund-initiator**.

1. Select **Variables** in the top bar to open the **Variables** pane (you may need to select **More** \> **Variables**).

1. Select and expand **Topic** variables.

   ![Screenshot of the variables pane.](media/variables-pane.png)

1. Select each of the topic variables in turn.

   ![Screenshot of the variables properties.](media/variables-properties.png)

1. Select **Global**.

1. Select **X** to close the variable pane.

1. Select **Save**.

## Exercise 6 - Test the agent

In this exercise, you will test topic routing and confirm the conversation follows the expected step-by-step flow.

### Task 6.1 - Test the Book Showing topic

1. Select the **Test** icon in the upper-right of the page to open the **Test** pane.

1. In the **Test** pane, select the ellipses (**...**) next to the variables **{x}** icon, and toggle **Show activity map when testing** to **On** and **Track between topics** to **Off**.

   ![Show activity map.](media/show-activity-map.png)

1. At the top of the **Test pane**, select the **Start new test session** icon **+**.

1. When the **Conversation Start** message appears, your agent starts a conversation. In response, enter the following prompt:

   `Provide a list of transaction ids and products for loyalty id ZAVA-10100`

1. The agent should respond with a list of three orders.

   ![Screenshot of the Conversation.](media/topic-conversation-1.png)

1. In response, enter the following text to trigger the topic:

   `Start a refund for TXN-2036`

1. Confirm `Yes` to the adaptive card.

   The agent responds with a confirmation message indicating the refund process has been submitted.

   ![Screenshot of the Conversation.](media/topic-conversation-2.png)

## Summary

In this lab, you created the refund-initiator topic and used nodes to enforce a structured, step-by-step interaction while generative AI remained enabled. You also configured variable scope so information collected can be used across topics.
