<!--
lab:
  title: 'Lab 00: Environment Setup — Zava Corporation AI Agent Infrastructure'
  description: Before any security configuration can begin, the Zava Corporation environment must be fully provisioned. In this lab, MOD Administrator will configure the Microsoft Entra ID tenant, enable Microsoft Copilot Studio, register the security group required for agent authoring, create the three AI agents that serve as governance targets throughout the entire course, connect each agent to its designated SharePoint knowledge source, and upload the sample business documents that simulate Zava's live data environment.
  duration: 30 minutes
  level: 300
  islab: true
  primarytopics:
    - Microsoft Copilot
    - Microsoft Entra
    - Microsoft Entra ID
    - Microsoft Copilot Studio
-->

# Lab 00: Environment Setup — Zava Corporation AI Agent Infrastructure

## Introduction

**Zava Corporation** is a mid-sized financial services and HR consulting firm operating across the UK and EU. Zava manages sensitive employee records, client financial data, and third-party vendor contracts. The organisation has recently deployed AI agents across its HR, Finance, and IT Support functions to improve operational efficiency.

Before any security configuration can begin, the Zava Corporation environment must be fully provisioned. In this lab, **MOD Administrator** will configure the Microsoft Entra ID tenant, enable Microsoft Copilot Studio, register the security group required for agent authoring, create the three AI agents that serve as governance targets throughout the entire course, connect each agent to its designated SharePoint knowledge source, and upload the sample business documents that simulate Zava's live data environment.

Every subsequent lab depends on the agents, identities, and files created here. Complete all three exercises in order before proceeding to Lab 01.


   >[!Note] In a real-world environment, the responsibilities outlined here would be distributed across multiple personas — such as developers, IT administrators, security administrators, and compliance officers — each operating with scoped permissions aligned to the principles of least privilege and Zero Trust. However, due to time and environment constraints, this lab does not replicate that separation of duties. All setup and configuration tasks will be performed by a single persona: the MOD Administrator, who holds the Global Administrator role in Microsoft.


### Objectives

- Create a role-assignable security group in Microsoft Entra ID and assign it the Privileged Role Administrator role.
- Enable the **copilotagentsecurity** group as the authorised Copilot Studio Authors group in Power Platform Admin Center.
- Enable Entra Agent Identity for Copilot Studio at the environment level.
- Connect SharePoint as a data source in the Power Apps maker portal.
- Create three Copilot Studio agents: Zava HR Assistant, Zava Finance Agent, and Zava IT Support Agent.
- Connect each agent to its designated SharePoint knowledge source.
- Publish each agent and share it with the appropriate lab users.
- Upload Zava sample business documents to the HR and Finance SharePoint sites.
- Verify that all three agents appear as Active in the Microsoft Agent 365 Agent Registry.


---

## Lab Duration

Estimated time: **30 minutes**

---

## Exercise 1: Configure Entra ID and Enable Copilot Studio Authors

### Task 1: Sign In and Configure Multi-Factor Authentication

1. Open a browser and navigate to +++https://entra.microsoft.com+++.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image1.png)

1. On the sign-in page, enter the **MOD Administrator** credentials from the **Resources** tab of your lab environment.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image2.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image3.png)

1. If prompted with a **Keep your account secure** window, select **Next**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image4.png)

1. Follow the on-screen prompts to set up the Microsoft Authenticator app.

    >[!Note] On your mobile device, open the Authenticator app, select **+** in the top-right corner, select **Work or school account**, and then select **Scan a QR code**. Scan the QR code displayed on screen.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image5.png)

1. Complete all remaining prompts to finish the Authenticator setup.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image6.png)

1. If asked **Stay signed in?**, select **Yes**.

1. On the Microsoft Entra admin center welcome screen, select **Get Started**.



### Task 2: Create the copilotagentsecurity Security Group

1. In the Microsoft Entra admin center, in the left navigation pane, expand **Entra ID**. Under **Entra ID**, select **Groups**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image7.png)

1. On the **Overview** page, select **New group**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image8.png)

1. On the **New Group** page, configure the following fields:

    - **Group type:** Select **Security**.
    - **Group name:** Enter `copilotagentsecurity`.
    - **Microsoft Entra roles can be assigned to the group:** Select **Yes**. If this option is not visible, skip this field and continue.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image9.png)


1. Under **Owners**, select **No owners selected**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image10.png)

1. On the **Add owners** panel, search for and select **MOD Administrator**. Choose **Select** to confirm the owner.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image11.png)

1. Under **Members**, select **No members selected**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image12.png)

1. On the **Add members** panel, search for and select **MOD Administrator** and **Patti Fernandes**. Choose **Select** to confirm the members.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image13.png)

1. Under **Roles**, select **No roles selected**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image14.png)

1. On the **Select roles** panel, search for `Global admin`, select **Global Administrator**, and then choose **select**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image15.png)

1. Select **Create**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image16.png)

1. In the confirmation dialog, select **Yes**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image17.png)

1. Confirm that a success notification appears at the top of the page.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image18.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image19.png)


### Task 3: Enable Access Management for Azure Resources

1. In the left navigation pane of the Microsoft Entra admin center, expand **Entra ID**. Under **Entra ID**, select **Overview**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image20.png)

1. On the **Overview** page, select **Properties** from the top bar.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image21.png)

1. On the **Properties** page, locate the **Access management for Azure resources** toggle and set it to `Yes`.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image22.png)

1. Select **Manage security defaults**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image23.png)

1. On the **Security defaults** panel, under **Security defaults**, select **Enabled**. Select **Save**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image24.png)

1. Return to the **Properties** page and select **Save**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image25.png)


### Task 4: Assign the Privileged Role Administrator Role

1. In the left navigation pane of the Microsoft Entra admin center, expand **Entra ID** and select **Roles & admins**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image26.png)

1. On the **Roles and administrators** page, in the search bar, enter `privileged role admin`.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image27.png)

1. In the search results, select **Privileged Role Administrator** by selecting its name. Do not select the checkbox next to it.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image28.png)

1. On the **Privileged Role Administrator** page, select **+ Add assignments**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image29.png)

1. On the **Add assignments** panel, select **No members selected**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image30.png)

1. On the **Select members** panel, search for and select **copilotagentsecurity**. Choose **select** to confirm.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image31.png)

1. Select **Next**.

1. On the **Settings** step, under **Assignment type**, select **Active**. In the **Enter justification** field, enter `Successful lab completion`.

1. Select **Assign**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image32.png)

1. Confirm that the role assignment appears in the assignments list.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image33.png)


### Task 5: Configure Copilot Studio Authors in Power Platform Admin Center

1. Open a new browser tab and navigate to +++https://admin.powerplatform.microsoft.com+++.

1. In the left navigation pane, select **Manage**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image34.png)

1. Under **Manage**, select **Tenant Settings**. On the **Tenant Settings** page, locate and select **Copilot Studio Authors** from the list.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image134.png)

1. On the **Copilot Studio Authors** panel, select the **Edit** icon near security group.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image35.png)

1. In the search field, enter `copilotagentsecurity`. Select the **copilotagentsecurity** group from the results. Then select **Done**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image36.png)

1. Select **Save** to apply the setting.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image37.png)



### Task 6: Enable Entra Agent Identity for Copilot Studio

1. Remain in the Power Platform Admin Center at `https://admin.powerplatform.microsoft.com`. In the left navigation pane, select **Copilot**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image38.png)

1. On the **Copilot** page, select **Settings**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image39.png)

1. In the settings list, under the **Copilot Studio** section, select **Entra Agent Identity for Copilot Studio**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image40.png)

1. On the **Entra Agent Identity for Copilot Studio** panel, select the **Dev One** environment from the environment list. Select **Edit setting**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image41.png)

1. On the setting panel, select **On**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image42.png)

1. Select **Save**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image43.png)

1. After saving, close the panel.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image44.png)

    >[!Note] Enabling Entra Agent Identity allows Copilot Studio agents to be automatically assigned a unique identity in Microsoft Entra ID. This is required for identity governance, Conditional Access, and Defender for Cloud Apps integration in later labs.



### Task 7: Add a SharePoint Connection in the Power Apps Maker Portal

1. Open a new browser tab and navigate to +++https://make.powerapps.com+++ and sign in with **MOD Administrator** credentials if prompted.

1. If prompted, on the **Welcome to Power Apps** screen, select **United States** and then select **Get started**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image45.png)

1. In the top-right corner, confirm that the **Dev One** environment is selected in the environment switcher. If not, select the environment switcher and select **Dev One**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image46.png)

1. In the left navigation bar, expand **More** and select **Connections**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image47.png)

1. On the **Connections** page, select **+ New connection**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image48.png)

1. In the connector search bar, enter `SharePoint`. Select **SharePoint** from the list of available connectors.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image49.png)

1. On the **SharePoint** connection panel, select **Connect directly (cloud services)**. Select **Create**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image50.png)

1. When prompted, sign in with **MOD Administrator** credentials to authorise the connection and select **Allow access**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image51.png)

1. Confirm that the SharePoint connection appears in the **Connections** list with a status of **Connected**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image52.png)


## Exercise 2: Create the Zava Copilot Studio Agents

In this exercise, MOD Administrator creates all three Zava agents in Microsoft Copilot Studio. Each agent is configured with a name, description, instructions, and a SharePoint knowledge source. After publishing, each agent is shared with the designated lab user accounts. These agents serve as the live governance targets in Labs 01 through 07.


### Task 1: Create the Zava HR Assistant

1. Open a new browser tab and navigate to +++https://copilotstudio.microsoft.com+++. Sign in with **MOD Administrator** credentials if prompted.

1. On the **Welcome** screen, locate the environment switcher in the top-right corner of the page.

1. If the current environment is not **Dev One**, select the environment switcher and select **Dev One** from the dropdown list.

    >[!Important] If Copilot Studio does not load or does not show the option to select an **Environment** as in the screenshot below, follow these steps.
    >
    > Open `https://admin.powerplatform.microsoft.com/`. Select **Manage** > **Environments** > **Dev One** and copy the value of the **Environment ID**.
    >
    > Navigate back to the Copilot Studio tab and open `https://copilotstudio.microsoft.com/environments/<EnvironmentID>` (replacing `<EnvironmentID>` with the value copied above).

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image53.png)

1. In the left navigation pane, select **Agents**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image54.png)

1. On the **Create an agent** page, select **Create blank agent**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image55.png)

1. On the agent configuration page, select **Edit** under **Details**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image56.png)

1. In the **Name** field, enter `Zava HR Assistant`.

1. In the **Description** field, enter `An AI assistant that helps Zava employees find HR policies, benefits information, and employee procedures.`

1. Select **Save**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image57.png)

1. In the **Instructions** field, select **Edit** and enter the following, then select **Save**.

    `You are the Zava HR Assistant. Answer questions using only the information available in the Zava HR SharePoint knowledge base. Do not speculate or provide information outside the knowledge base. Always respond professionally.`

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image58.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image59.png)

1. On the agent configuration page, locate the **Knowledge** section. Select **+ Add knowledge**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image60.png)

1. On the **Add knowledge** panel, select **SharePoint**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image61.png)

1. In the **SharePoint URL** field, enter the SharePoint HR site URL in the following format and select **Add**:

    `https://[TenantPrefix].sharepoint.com/sites/HR`

    >[!Note] Replace `[TenantPrefix]` with your tenant prefix found on the **Resources** tab of your lab environment.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image62.png)

1. Select **Add to agent** to connect the SharePoint site as the knowledge source.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image63.png)

1. In the top-right corner of the agent configuration page, select **Publish**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image64.png)

1. In the confirmation dialog, select **Publish** to confirm.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image65.png)

1. On the agent configuration page, locate the **Channels** tab on the top section (select **+2** if it is not directly visible).

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image67.png)

1. Select **Microsoft 365 Copilot and Microsoft Teams** to add them as channels.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image68.png)

1. Then select **Add channel**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image69.png)

1. Select **Availability options**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image71.png)

1. On the **Microsoft 365 Copilot and Microsoft Teams** page, select **Show to everyone in my org**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image72.png)

1. Select **Submit to org catalog**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image73.png)

1. On the **Give everyone access to this agent?** confirmation dialog, select **Yes**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image74.png)

1. You will be redirected to **Show in Teams app store for org** and see a notification: **Your agent is submitted and waiting for approval from your Teams admin**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image75.png)



### Task 2: Create the Zava Finance Agent

1. In the left navigation pane, select **Agents**. Then select **Create blank agent**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image76.png)

1. On the **Agent** page, select **Edit** under **Details**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image77.png)

1. On the agent configuration page, in the **Name** field, enter `Zava Finance Agent`.

1. In the **Description** field, enter `An AI assistant that helps Zava finance team members retrieve budget information, invoice data, and financial reports.` Then select **Save**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image78.png)

1. In the **Instructions** field, select **Edit**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image79.png)

1. Enter the following and select **Save**.

    `You are the Zava Finance Agent. Answer questions using only the information in the Zava Finance SharePoint knowledge base. Do not share financial data with users who have not been granted access to the Finance SharePoint site. Always respond professionally and flag any requests for data outside your knowledge base.`

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image80.png)

1. On the agent configuration page, locate the **Knowledge** section. Select **+ Add knowledge**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image81.png)

1. On the **Add knowledge** panel, select **SharePoint**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image82.png)

1. In the **SharePoint URL** field, enter the SharePoint Finance site URL in the following format:

    `https://[TenantPrefix].sharepoint.com/sites/Operations`

    >[!Note] Replace `[TenantPrefix]` with your tenant prefix from the **Resources** tab.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image83.png)

1. Select **Add** to connect the SharePoint site as the knowledge source.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image84.png)

1. Then select **Add to agent**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image85.png)

1. On the agent configuration page, locate the **Channels** tab on the top section (select **+2** if it is not directly visible).

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image86.png)

1. Select **Microsoft 365 Copilot and Microsoft Teams** to add them as channels.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image87.png)

1. Then select **Add channel**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image88.png)

1. In the **Ready to publish?** dialog, select **Publish**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image89.png)

1. Under **Agent preview**, select **Availability options**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image90.png)

1. Under **Decide who you want to show your agent to:**, select **Show to my teammates and shared users**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image91.png)

1. On the **Share "Zava Finance Agent" in Teams** panel, in the search field, enter `Patti Fernandes`. Select **Patti Fernandes** from the results.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image92.png)

1. Select **Patti Fernandes** again and on the right panel, select the **Editor** role.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image93.png)

1. In the search field, enter `Megan Bowen`. Select **Megan Bowen** from the results.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image94.png)

1. In the search field, enter `Alex Wilber`. Select **Alex Wilber** from the results.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image95.png)

1. Select **Update** to apply the sharing configuration.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image96.png)



### Task 3: Create the Zava IT Support Agent

1. In the left navigation pane, select **Agents**. Then select **Create blank agent**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image97.png)

1. On the **Agent** page, select **Edit** under **Details**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image98.png)

1. On the agent configuration page, in the **Name** field, enter `Zava IT Support Agent`.

1. In the **Description** field, enter `An AI assistant that helps Zava employees resolve common IT issues, submit support requests, and find IT policy documentation.` Then select **Save**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image99.png)

1. In the **Instructions** field, select **Edit**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image100.png)

1. Enter the following and select **Save**.

    `You are the Zava IT Support Agent. Help users with common IT questions using publicly available Microsoft support documentation and Zava IT policies. Do not access or share any sensitive financial or HR information. Escalate complex issues to the IT helpdesk.`

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image101.png)

1. On the agent configuration page, locate the **Knowledge** section. Select **+ Add knowledge**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image102.png)

1. On the **Add knowledge** panel, select **Public Website**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image103.png)

1. In the **URL** field, enter the following site URL. Select **Add** to connect the site as the knowledge source.

    `https://support.microsoft.com/`

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image104.png)

1. Then select **Add to agent**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image105.png)

1. In the top-right corner of the agent configuration page, select **Publish**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image106.png)

1. In the confirmation dialog, select **Publish** to confirm.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image107.png)

1. On the agent configuration page, locate the **Channels** tab on the top section (select **+2** if it is not directly visible).

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image108.png)

1. Select **Microsoft 365 Copilot and Microsoft Teams** to add them as channels.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image109.png)

1. Then select **Add channel**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image110.png)

1. Select **Availability options**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image111.png)

1. On the **Microsoft 365 Copilot and Microsoft Teams** page, select **Show to everyone in my org**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image112.png)

1. Select **Submit to org catalog**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image113.png)

1. On the **Give everyone access to this agent?** confirmation dialog, select **Yes**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image114.png)

1. You will be redirected to **Show in Teams app store for org** and see a notification: **Your agent is submitted and waiting for approval from your Teams admin**.



## Exercise 3: Upload Zava Knowledge Files to SharePoint

In this exercise, MOD Administrator uploads the Zava sample business documents to the SharePoint HR and Finance sites. These files contain the sensitive data — including employee PII, payroll records, credit card numbers, and financial forecasts — that will trigger security detections and DLP policy matches throughout Labs 04, 05, and 07.


### Task 1: Upload Files to the Zava HR SharePoint Site

1. Open a new browser tab and navigate to `https://[TenantPrefix].sharepoint.com/sites/HR`.

    >[!Note] Replace `[TenantPrefix]` with your tenant prefix from the **Resources** tab.

1. From the top navigation select **Benefits @ Contoso** and then from the left sub-navigation pane, select **Documents**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image115.png)

1. On the **Documents** page, select **Create or upload**. Then select **Files upload**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image116.png)

1. In the file picker, navigate to the **Lab Files** > **HR** folder on your lab VM desktop.

1. Select the following files and then select **Open** to upload them:

    | Filename | Contains |
    |---|---|
    | `Zava_HR_Policy_2024.docx` | Leave and disciplinary policy — no PII |
    | `Zava_Employee_Records.xlsx` | Employee IDs (format: ZVA123456), names, DOB, salary |
    | `Zava_Payroll_Q1_2025.xlsx` | Payroll data with credit card numbers in expense column |
    | `Zava_Onboarding_Guide.docx` | Standard onboarding content |
    | `Zava_Benefits_Summary.pdf` | Insurance and pension details |
    | `Zava_Org_Chart.docx` | Reporting lines and management structure |
    | `Zava_Termination_Checklist.docx` | Departing employee process with names and dates |
    | `Zava_Sick_Leave_Report.xlsx` | Employee names and illness reasons |

1. Wait for all 8 files to finish uploading.

1. On the **Documents** page, confirm that all 8 files appear in the document library.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image117.png)

1. Select **Zava_Employee_Records.xlsx** to open it.

1. Confirm that the file opens and displays employee data including employee IDs, names, and salary information.

1. Close the file and return to the **Documents** library.



### Task 2: Upload Files to the Zava Finance SharePoint Site

1. Open a new browser tab and navigate to `https://[TenantPrefix].sharepoint.com/sites/Operations`.

    >[!Note] Replace `[TenantPrefix]` with your tenant prefix from the **Resources** tab.

1. In the left navigation pane, select **Documents**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image118.png)

1. On the **Documents** page, select **Create or upload** > **Files upload**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image119.png)

1. In the file picker, navigate to the **Lab Files** > **Operations** folder on your lab VM desktop.

1. Select the following files and then select **Open** to upload them:

    | Filename | Contains |
    |---|---|
    | `Zava_Budget_2025.xlsx` | Department budgets and cost centres |
    | `Zava_Invoice_Log.xlsx` | Vendor invoices with IBAN and account numbers |
    | `Zava_Expense_Report_Alex.xlsx` | Alex Wilber's expenses with Visa credit card number |
    | `Zava_Audit_Report_2024.docx` | Internal audit findings — marked Confidential |
    | `Zava_Contracts_External.docx` | Third-party vendor contract — externally shared |
    | `Zava_Financial_Projections.xlsx` | Revenue forecasts with broad SharePoint permissions |

1. Wait for all 6 files to finish uploading.

1. On the **Documents** page, confirm that all 6 files appear in the document library.

1. Select **Zava_Expense_Report_Alex.xlsx** to open it.

1. Confirm that the file opens and displays expense data including credit card information.

1. Close the file and return to the **Documents** library.


### Task 3: Verify Agents in the Microsoft Agent 365 Agent Registry

1. Open a new browser tab and navigate to +++https://admin.cloud.microsoft/+++. Sign in with **MOD Administrator** credentials if prompted.

1. In the left navigation pane, select **Agents**. If this option is not visible, select **AI** and then select **Agents**. Then select **All agents**.

1. On this page, confirm that the following three agents appear in the list. You can search for `Zava` in the search box to filter the results.

    | Agent Name | Status | Publisher |
    |---|---|---|
    | Zava HR Assistant | Active | Default Publisher |
    | Zava Finance Agent | Active | Default Publisher |
    | Zava IT Support Agent | Active | Default Publisher |

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2000/media/image120.png)

    >[!Note] It may take up to 10 minutes after publishing in Copilot Studio for agents to appear in the Agent Registry. If the agents are not visible, wait 10 minutes and then refresh the page.


## Summary

In this lab, you completed the full environment baseline for the Zava Corporation AI security course. You created a role-assignable security group in the Microsoft Entra admin center, configured MOD Administrator as owner and member, assigned the Privileged Role Administrator role, and enabled the group as the authorised Copilot Studio Authors group in Power Platform Admin Center. You enabled Entra Agent Identity for Copilot Studio at the environment level, added a SharePoint connection in the Power Apps maker portal, and created three Copilot Studio agents — Zava HR Assistant, Zava Finance Agent, and Zava IT Support Agent — each connected to a designated knowledge source, published across Teams and Microsoft 365 channels. You uploaded 14 sample business documents containing realistic sensitive data across the Zava HR and Finance SharePoint sites, and verified that all three agents are registered and Active in the Microsoft Agent 365 Agent Registry. The environment is now fully prepared for security configuration in Labs 01 through 07.
