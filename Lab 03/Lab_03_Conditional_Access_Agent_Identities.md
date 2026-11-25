<!--
lab:
  title: 'Lab 03: Conditional Access for Zava Agent Identities'
  description: In this lab, you created a custom security attribute set named AgentAttributes with an AgentApprovalStatus attribute containing five predefined governance values. You assigned the HRApproved approval status to the Zava HR Assistant, establishing a structured agent classification model in Entra ID. You created the Zava - Block Unapproved Agent Identities Conditional Access policy targeting all agent identities and excluding those with approved attribute values. You used the What If tool in Report-only mode to validate that an approved agent is correctly excluded from the block policy, then switched the policy to enforcement mode. You created the Zava - Block High Risk Agent Identities policy using Entra ID Protection agent risk signals and set it to Report-only pending risk signal generation. Patti Fernandes invoked the Zava HR Assistant to generate sign-in events, which you then investigated in the Service principal sign-in logs filtered by agent type. Zava's agent identities are now governed by Zero Trust Conditional Access controls.
  duration: 30 minutes
  level: 200
  islab: true
-->

# Lab 03: Conditional Access for Zava Agent Identities

## Introduction

Zava's CISO has mandated that only reviewed and approved AI agents may access company resources. Any agent that has not been through the governance review process must be blocked automatically. Additionally, if any agent identity shows signs of compromise — such as anomalous token acquisition behaviour — it must be blocked immediately without manual intervention.

MOD Administrator will implement both controls using Conditional Access for Agent Identities (Preview). Patti Fernandes will validate that policy evaluation is visible in sign-in logs. This lab establishes the Zava agent governance baseline that all subsequent security labs build upon.

Conditional Access for Agent Identities is a preview capability in Microsoft Entra ID that extends Zero Trust controls to AI agents. MOD Administrator will create custom security attributes to classify the approval status of each Zava agent, build a Conditional Access policy that blocks all unapproved agent identities from accessing organisational resources, and create a second policy that blocks any agent identity exhibiting high-risk behaviour based on Entra ID Protection signals. The policies will first be validated in Report-only mode before being switched to enforcement. Patti Fernandes will investigate agent sign-in events to confirm Conditional Access policy evaluation.



### Objectives

- Create a custom security attribute set and approval status attribute for agent classification.
- Assign approval status attributes to all three Zava agent identities.
- Create a Conditional Access policy that blocks all unapproved agent identities.
- Validate the policy scope using the What If tool to confirm an untagged agent would be blocked.
- Switch the policy to enforcement mode.
- Create a second Conditional Access policy that blocks high-risk agent identities.
- Generate agent sign-in events by invoking the Zava HR Assistant as Patti Fernandes.
- Investigate Conditional Access policy evaluation in agent identity sign-in logs.




## Lab Duration

Estimated time: **30 minutes**



## Exercise 1: Create Custom Security Attributes for Agent Governance

### Task 1: Assign the Attribute Definition Administrator Role

1. Open a browser and navigate to +++https://entra.microsoft.com+++. Sign in with **MOD Administrator** credentials if prompted. Under **Entra ID**, select **Roles & admins**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image1.png)

1. In the search bar, enter `Attribute Definition Administrator`.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image2.png)

1. Select **Attribute Definition Administrator** by selecting its name. Do not select the checkbox.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image3.png)

1. On the **Attribute Definition Administrator** page, select **+ Add assignments**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image4.png)

1. On the **Add assignments** panel, select **No members selected**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image5.png)

1. Search for and select **MOD Administrator**. Choose **Select** to confirm.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image6.png)

1. Select **Next**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image7.png)

1. Under **Assignment type**, select **Active**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image8.png)

1. In the activation panel, enter a justification — `Lab 03 custom security attribute configuration`.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image9.png)

1. Uncheck **Permanently assigned** and set the duration to **1 hour**. Select **Assign**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image10.png)

1. Confirm the assignment appears in the list under **Active assignments**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image11.png)

1. Navigate back to **Roles & admins**.

1. In the search bar, enter `Attribute Assignment Administrator` and repeat the steps to assign the role to **MOD Administrator**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image12.png)

1. Select the **MOD Administrator** account icon in the top-right corner of the page. Select **Sign out**.

1. Sign back in to `https://entra.microsoft.com` with **MOD Administrator** credentials.

    >[!Note] The Attribute Definition Administrator role grants permissions to create and manage custom security attribute definitions. This role is intentionally excluded from Global Administrator to enforce separation of duties. A fresh sign-in is required for the new role assignment to take effect.

    


### Task 2: Create the AgentAttributes Attribute Set

1. In the left navigation pane, expand **Entra ID** and select **Custom security attributes**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image13.png)

1. On the **Custom security attributes** page, select **+ Add attribute set**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image14.png)

1. On the **Add attribute set** panel, in the **Attribute set name** field, enter `AgentAttributes`.

1. In the **Description** field, enter `Attribute set for classifying AI agent approval and governance status.`

1. In the **Maximum number of attributes** field, leave the default value.

1. Select **Add** to create the attribute set.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image15.png)

1. Confirm that **AgentAttributes** appears in the attribute set list.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image16.png)

    


### Task 3: Create the AgentApprovalStatus Attribute

1. On the **Custom security attributes** page, select **AgentAttributes** to open the attribute set.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image17.png)

1. On the **AgentAttributes** page, select **+ Add attribute**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image18.png)

1. On the **Add attribute** panel, configure the following fields:

    - **Attribute name:** Enter `AgentApprovalStatus`.
    - **Description:** Enter `Tracks the approval status of each AI agent identity in the Zava governance review process.`
    - **Data type:** Select **String**.
    - **Allow multiple values to be assigned:** Select **Yes**.
    - **Only allow predefined values to be assigned:** Select **Yes**.


1. Under **Predefined values**, select **+ Add value**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image19.png)

1. In the value field, enter `New`. Then select **Add**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image20.png)

1. Select **+ Add value**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image21.png)

1. In the value field, enter `In_Review`. Then select **Add**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image22.png)

1. Select **+ Add value**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image23.png)

1. In the value field, enter `HR_Approved`. Then select **Add**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image24.png)

1. Select **+ Add value**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image25.png)

1. In the value field, enter `Finance_Approved`. Then select **Add**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image26.png)

1. Select **+ Add value**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image27.png)

1. In the value field, enter `IT_Approved`. Then select **Add**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image28.png)

1. Select **Save**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image29.png)

1. Confirm that **AgentApprovalStatus** appears in the attributes list under **AgentAttributes**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image30.png)

    


### Task 4: Assign HR_Approved to the Zava HR Assistant

1. In the left navigation pane, select **Agent ID**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image31.png)

1. On the **All agent identities (Preview)** page, select **Zava HR Assistant (Microsoft Copilot Studio)**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image32.png)

1. On the **Overview (Preview)** page, in the left sub-navigation, select **Custom security attributes (Preview)**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image33.png)

1. On the **Custom security attributes** page, select **+ Add assignment**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image34.png)

1. On the **Add custom security attribute assignment** panel, configure the following:

    - **Attribute set:** Select **AgentAttributes**.
    - **Attribute:** Select **AgentApprovalStatus**.
    - **Assigned values:** Select **Add value** > **HR_Approved** and select **Save**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image35.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image36.png)


1. Select **Save** to apply the assignment.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image37.png)

1. Confirm that **AgentApprovalStatus** appears with the value **HR_Approved** on the custom security attributes page.

1. Similarly assign the following attributes to respective agents.

    - **Zava Finance Agent (Microsoft Copilot Studio)**: HR_Approved
    - **Zava HR Assistant (Microsoft Copilot Studio)**: HR_Approved
    


## Exercise 2: Create a Conditional Access Policy to Block Unapproved Agent Identities

### Task 1: Create the Policy and Configure Assignments

1. In the left navigation pane of the Microsoft Entra admin center, expand **Entra ID**, then select **Conditional Access**.

1. On the **Conditional Access** page, select **Policies**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image38.png)

1. On the **Policies** page, select **+ New policy**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image39.png)

1. On the **New Conditional Access policy** page, in the **Name** field, enter `Zava - Block Unapproved Agent Identities`.

1. Under **Assignments**, select **0 users or agents (Preview) selected** under **Users or agents**.

1. On the assignments panel, under **What does this policy apply to?**, select **Agents (Preview)**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image40.png)

1. Under **Include**, select **All agent identities (Preview)**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image41.png)

1. Under **Exclude**, select **Select agent identities based on attributes**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image42.png)

1. Set **Configure** to **Yes**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image43.png)

1. In the expression configuration, under **AgentAttributes**, select the attribute **AgentApprovalStatus**. Set **Operator** to **Contains**. Set **Value** to **HR_Approved**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image44.png)

1. Select **Done** to confirm the exclusion configuration.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image45.png)

1. Under **Target resources**, select **No target resources selected**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image46.png)

1. Under **Include**, select **All resources (formerly 'All cloud apps')**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image47.png)

1. Under **Access controls**, on the **Grant** panel, confirm that **Block access** is selected.

1. Under **Enable policy**, keep **Report-only**.

1. Select **Create** to save the policy.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image48.png)

    


### Task 2: Validate the Policy Using the What If Tool

1. On the policy page, select **What If** to open the Report-only impact view.

    >[!Note] The What If tool allows you to simulate whether a specific identity would be affected by this policy without enforcing it.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image49.png)

1. On the **What If** panel, under **User or workload identity**, select **Agent identities**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image50.png)

1. Select **Edit agent identity**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image51.png)

1. In the agent identity search field, search for and select **Zava Finance Agent (Microsoft Copilot Studio)**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image52.png)

1. Under **Target resource**, set **Select target type** to **Cloud apps**. Select **+ Select cloud app**.

1. In the search field, enter `Office 365 SharePoint Online`. Select **Office 365 SharePoint Online** from the results. Choose **Select** to confirm.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image53.png)

1. Select **What if** to run the simulation.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image54.png)

1. Review the results and confirm that the policy **Zava - Block Unapproved Agent Identities** shows as **Applied** — because the Zava Finance Agent is NOT excluded by the `HR_Approved` attribute.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image3541.png)

1. Return to the **Edit agent identity** link and change the agent to **Zava HR Assistant**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image55.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image56.png)

1. Select **What if** to run the simulation.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image57.png)

1. Review the results and confirm that the policy **Zava - Block Unapproved Agent Identities** shows as **Not applied** — because the Zava HR Assistant is excluded by the `HR_Approved` attribute.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image58.png)

1. Select **Close** to exit the What If panel.

    


### Task 3: Switch the Policy to Enforcement Mode (Read Only)

>[!Note] You will not be able to perform this task in the current environment because Security Defaults were enabled in Lab 00 to allow publishing of Copilot Studio agents.

1. On the **Zava - Block Unapproved Agent Identities** policy page, select **Edit**.

1. Under **Enable policy**, select **On**.

1. Select **Save** to apply the change.

1. On the **Policies** page, confirm that **Zava - Block Unapproved Agent Identities** shows a status of **On**.

    


## Exercise 3: Create a Conditional Access Policy to Block High-Risk Agent Identities

### Task 1: Create the Policy and Configure Assignments

1. On the **Conditional Access** page, select **+ Create new policy**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image59.png)

1. In the **Name** field, enter `Zava - Block High Risk Agent Identities`.

1. Under **Assignments**, select **0 users or agents (Preview) selected** under **Users or agents**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image60.png)

1. Under **What does this policy apply to?**, select **Agents (Preview)**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image61.png)

1. Under **Include**, select **All agent identities (Preview)**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image62.png)

1. Under **Target resources**, select **No target resources selected**, then select **All resources (formerly 'All cloud apps')**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image63.png)

1. Under **Conditions**, select **0 Conditions selected**. Then select **Not Configured** under **Agent Risk**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image64.png)

1. On the **Agent risk** panel, set **Configure** to **Yes**. Under **Configure agent risk levels needed for policy to be enforced**, select **High**. Select **Done** to confirm the condition.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image65.png)

1. Under **Access controls**, under **Grant**, make sure that **Block access** is selected.

1. Under **Enable policy**, select **Report-only**.

    >[!Note] This policy is set to Report-only because agent risk signals from Entra ID Protection require active agent usage over time before risk levels are generated. In a newly provisioned lab environment, no risk signals will be present yet. Report-only mode allows the policy to be evaluated against future sign-in events without blocking access prematurely. In a production environment, this policy would be switched to On once baseline risk signal data is established.

1. Select **Create** to save the policy.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image66.png)

1. On the **Policies** page, confirm that **Zava - Block High Risk Agent Identities** appears with a status of **Report-only**.

    


## Exercise 4: Generate Agent Sign-In Events and Investigate Conditional Access Policy Evaluation

### Task 1: Invoke the Zava HR Assistant

1. Open a new **InPrivate** or **Incognito** browser window.

1. Navigate to +++https://copilot.microsoft.com+++.

1. Sign in with **Patti Fernandes** credentials from the **Resources** tab. (You can use `pattif@TenantName` as your ID and the User Password from the Resources tab.)

1. In the Microsoft 365 Copilot chat interface, select **All agents** from the navigation. Search for and select **Zava HR Assistant**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image67.png)

1. Then select **Add**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image68.png)

1. In the chat input field, enter the following:

    `What is Zava's leave policy?`

1. Wait for the Zava HR Assistant to respond.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image69.png)

1. Enter a second message in the chat input field:

    `How do I submit a sick leave request?`

1. Wait for the response.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image70.png)

    >[!Note] These interactions generate agent sign-in events as the Zava HR Assistant authenticates to access its SharePoint knowledge source. These events will appear in the Entra sign-in logs and will have Conditional Access policy evaluation recorded against them.

1. Close the InPrivate browser window.

    


### Task 2: Investigate Agent Sign-In Logs in Entra

1. Return to the **MOD Administrator** browser session at `https://entra.microsoft.com`.

1. In the left navigation pane, expand **Entra ID** and select **Monitoring & health**. Select **Sign-in logs**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image71.png)

1. On the **Sign-in logs** page, select the **Service principal sign-ins** tab.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image72.png)

1. In the filter bar, select **+ Add filters**. Select **Is Agent** as the filter field.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image73.png)

1. Select **Yes** and then select **Apply** to apply the filter.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image74.png)

1. Review the sign-in entries returned in the filtered view.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2003/media/image75.png)

    


## Summary

In this lab, you created a custom security attribute set named `AgentAttributes` with an **AgentApprovalStatus** attribute containing five predefined governance values. You assigned the **HR_Approved** approval status to the Zava HR Assistant, establishing a structured agent classification model in Entra ID. You created the **Zava - Block Unapproved Agent Identities** Conditional Access policy targeting all agent identities and excluding those with approved attribute values. You used the What If tool in Report-only mode to validate that an approved agent is correctly excluded from the block policy, then switched the policy to enforcement mode. You created the **Zava - Block High Risk Agent Identities** policy using Entra ID Protection agent risk signals and set it to Report-only pending risk signal generation. Patti Fernandes invoked the Zava HR Assistant to generate sign-in events, which you then investigated in the Service principal sign-in logs filtered by agent type. Zava's agent identities are now governed by Zero Trust Conditional Access controls.
