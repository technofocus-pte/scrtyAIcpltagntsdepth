<!--
lab:
  title: 'Lab 06: DSPM — Oversharing Assessment and Remediation'
  description: MOD Administrator will launch a custom data risk assessment against the Zava HR and Finance SharePoint sites, activate DSPM one-click policies, and use the Objectives dashboard to drive remediation. Adele Vance will generate realistic Copilot interaction signals referencing sensitive labelled files. Patti Fernandes will investigate the AI activities in DSPM Activity Explorer and review the oversharing findings from the assessment.
  duration: 25 minutes
  level: 300
  islab: true
-->

# Lab 06: DSPM — Oversharing Assessment and Remediation

## Introduction

Microsoft Purview Data Security Posture Management is the unified front door for discovering, protecting, and investigating sensitive data risks across Zava's digital estate — including AI apps, agents, SharePoint sites, and user interactions. Unlike the classic DSPM for AI experience, the new DSPM combines traditional data security posture with AI observability into a single solution, organised around outcome-based security objectives.

In this lab, the data risk assessment scan is initiated at the very start of Day 3 before any other work begins, so results are available by the time learners reach Exercise 4. Signal generation exercises create realistic Copilot interaction events referencing sensitive Zava files. MOD Administrator a nd Patti Fernandes then use DSPM Objectives, one-click policies, assessment results, and the Activity Explorer to investigate and remediate oversharing risks across the Zava agent environment.



## Scenario

Zava's CISO has received a concern from the compliance team: the HR Assistant and Finance Agent may be surfacing sensitive employee and financial records to users who should not have access to that data. The security team needs to understand the full scope of data exposure, activate posture management policies, and apply remediation controls before the end of Day 3.

MOD Administrator will launch a custom data risk assessment against the Zava HR and Finance SharePoint sites, activate DSPM one-click policies, and use the Objectives dashboard to drive remediation. Adele Vance will generate realistic Copilot interaction signals referencing sensitive labelled files. Patti Fernandes will investigate the AI activities in DSPM Activity Explorer and review the oversharing findings from the assessment.



### Objectives

- Initiate a custom DSPM data risk assessment against Zava HR and Finance SharePoint sites at the start of Day 3.
- Generate realistic Microsoft 365 Copilot interaction signals referencing sensitive labelled files as Adele Vance.
- Navigate the new DSPM experience and review the Posture dashboard.
- Activate DSPM one-click policies for risky AI usage detection and sensitive data protection.
- Review DSPM Objectives for oversharing and Copilot data exposure.
- Review data risk assessment results and apply remediation actions.
- Investigate Zava agent activity and sensitive data access in the Apps and agents dashboard.
- Review AI interaction events in Activity Explorer filtered to Adele Vance.
- Apply SharePoint Restricted Content Discovery to the Zava HR site.




## Lab Duration

Estimated time: **25 minutes**



>[!Important] Complete Task 1 of Exercise 1 before anything else on Day 3.** The data risk assessment scan can take 30–60 minutes to complete. It must be started first so results are available when you reach Exercise 4. Do not proceed to Exercise 2 until Task 1 of Exercise 1 is complete.



## Exercise 1: Initiate the Data Risk Assessment

### Task 1: Register an Entra App

1. Navigate to +++https://entra.microsoft.com+++. Sign in with **MOD Administrator** credentials if prompted.

1. In the left navigation pane, select **App registrations** > **+ New registration**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2006/media/image1.png)

1. Configure the following:

    - **Name:** `Purview DSPM Item Level Scan`
    - **Supported account types:** Select **Single tenant only - Contoso**


1. Select **Register**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2006/media/image2.png)

1. On the app registration overview page, copy and note the **Application (client) ID**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2006/media/image3.png)

1. In the left sub-navigation, select **API permissions**. Select **+ Add a permission**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2006/media/image4.png)

1. Select **Microsoft Graph**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2006/media/image5.png)

1. Select **Application permissions**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2006/media/image6.png)

1. Search for and add the following permissions:

    - `Application.Read.All`
    - `Directory.Read.All`
    - `Files.ReadWrite.All`
    - `SensitivityLabels.Read.All`
    - `Sites.ReadWrite.All`
    - `User.Read.All`
    - `SensitivityLabel.Read`


1. Select **Add permissions**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2006/media/image7.png)

1. Select **Grant admin consent for Contoso**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2006/media/image8.png)

1. Select **Yes** to confirm.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2006/media/image9.png)

1. Navigate to **Certificates & secrets** > **+ New client secret** > set expiry to **6 months** > **Add**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2006/media/image10.png)

1. Copy the **Value**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2006/media/image11.png)

1. Save the values, as they can only be copied once and will be needed in the next task.

    


### Task 2: Run a Custom Data Risk Assessment Against Zava SharePoint Sites

1. Open a browser and navigate to +++https://purview.microsoft.com+++. Sign in with **MOD Administrator** credentials if prompted.

1. In the left navigation pane, select **Solutions** > **DSPM **.

    >[!Note] Do not select **DSPM for AI (classic)** or **Data Security Posture Management (classic)**. The new experience is labelled **DSPM** and is a separate entry in the Solutions menu.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2006/media/image12.png)

1. On the **DSPM** landing page, if prompted to complete initial setup tasks, select **Get started** and accept any required configuration to enable the solution. Allow the setup to complete before continuing.

1. In the left sub-navigation, select **Discover**. Under **Discover**, select **Data risk assessments**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2006/media/image13.png)

1. On the **Item-level scan not setup** notification, select **Setup connection**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2006/media/image14.png)

1. On the **Client Secret** tab, enter the **Application ID** and **Client secret** value copied in Task 1. Then select **Authenticate**. Once successful, select **Save**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2006/media/image15.png)

1. On the **Data risk assessments** page, select **+ Create custom assessment**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2006/media/image16.png)

1. On the **Basic details** panel, configure the following:

    - **Assessment name:** Enter `Zava SharePoint Oversharing Assessment`.
    - **Description:** Enter `Custom assessment to identify potentially overshared sensitive items across Zava HR and Finance SharePoint sites.`


1. Select **Next**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2006/media/image17.png)

1. On the **Select scan level**, choose **Item-level**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2006/media/image18.png)

1. Select **Next** until you reach **Add data sources to assess**. Next to **SharePoint**, select **Scope sites**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2006/media/image19.png)

1. In the SharePoint site selector, select **Include** > **From all sites**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2006/media/image20.png)

1. Search for and select the following two sites:

    - `HR`
    - `Operations`


1. Select **Done** twice to confirm the site selection.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2006/media/image21.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2006/media/image22.png)

1. Select **Next**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2006/media/image23.png)

1. Select **Save and Run**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2006/media/image24.png)

1. Select **Done**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2006/media/image25.png)

1. Confirm that the assessment appears in the **Data risk assessments** list with a status of **In progress** or **Queued**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2006/media/image26.png)

    >[!Note] The assessment will take 30–60 minutes to complete depending on the number of items in the selected SharePoint sites. Proceed immediately to Exercise 2. You will return to review the results in Exercise 4.

    


## Exercise 2: Generate Copilot Interaction Signals

In this exercise, Adele Vance generates realistic Microsoft 365 Copilot interaction events that reference sensitive labelled files across the Zava HR and Finance SharePoint sites. These interactions will surface in the DSPM Activity Explorer and audit logs, creating the investigation data used in Exercises 5 and Lab 07.

### Task 1: Generate HR Data Interaction Signals as Adele Vance

1. Open a new **InPrivate** or **Incognito** browser window. Navigate to +++https://copilot.microsoft.com+++. Sign in with **Adele Vance** credentials from the **Resources** tab. Complete the authentication steps if necessary.

1. From the navigation, select **All agents** > **Zava HR Assistant** > **Add**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2006/media/image27.png)

1. In the input field, enter the following prompt:

    `Summarise the contents of Zava_Employee_Records.xlsx from the HR SharePoint site`

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2006/media/image28.png)

1. Wait for the response and note what Copilot returns.

1. Enter the following second prompt:

    `Find all employee salary information across Zava HR documents`

1. Wait for the response.

1. Enter the following third prompt:

    `What does the Zava payroll report for Q1 2025 contain?`

1. Wait for the response.

    


## Exercise 3: Explore the DSPM Posture Dashboard and Activate One-Click Policies

### Task 1: Review the DSPM Posture Dashboard

1. Return to the **MOD Administrator** session in the Microsoft Purview portal at `https://purview.microsoft.com`.

1. In the left navigation pane, select **Solutions** > **DSPM**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2006/media/image29.png)

1. On the **DSPM** landing page, review the **Posture** dashboard.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2006/media/image30.png)

1. Review the following sections and note their current values:

    - **Security Copilot suggested prompts** — note the prompt suggestions available.
    - **Top objectives to address** — note which objectives are listed as highest priority.
    - **Data use snapshot** — note the volume of sensitive data activity detected across the estate.
    - **30-day trending graph** — review whether the trend is improving or worsening.

    


### Task 2: Activate the Detect Risky AI Usage One-Click Policy

1. On the **DSPM** landing page, in the left sub-navigation, select **Tasks and actions**. Then select **Remediation actions**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2006/media/image31.png)

1. Select **Detect risky interactions in AI apps** to expand it.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2006/media/image32.png)

1. Select **Create Policy** to enable the **DSPM for AI - Detect risky AI usage** Insider Risk Management policy.

1. Confirm that the policy status updates to **On** or **Active**. Close the tab.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2006/media/image33.png)

    >[!Note] This Insider Risk Management policy detects risky prompts and responses in Microsoft 365 Copilot, agents, and other generative AI apps — including prompt injection attempts, accessing protected materials, and other high-risk interaction patterns. The Adele Vance interactions generated in Exercise 2 will be evaluated by this policy.

    


### Task 3: Activate the Sensitive Data Protection One-Click Policy

1. On the **Remediation actions** page, select **Safeguard sensitive data in Microsoft 365 Copilot interactions** to expand it.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2006/media/image34.png)

1. Select **Get started** to enable this DLP policy.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2006/media/image35.png)

1. On the data pane, select **view** next to **Sensitive info types**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2006/media/image36.png)

1. Select **Credit Card Number**. Then select **Add**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2006/media/image37.png)

1. Under **Actions**, select **Restrict user prompts from being processed**. Then select **Create policy**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2006/media/image38.png)

1. Return to the **Remediation actions** page and select **Safeguard sensitive data in Microsoft 365 Copilot interactions**. Then select **Get started**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2006/media/image39.png)

1. Select **Enforce policy** to enable the **Default DLP policy - Protect sensitive M365 Copilot interactions** policy.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2006/media/image40.png)

1. Confirm the policy is active.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2006/media/image41.png)

    


## Exercise 4: Review Data Risk Assessment Results and Apply Remediation

>[!Note] Assessments may take some time. You can return to this exercise at the end of the labs if the assessment is still in progress.

### Task 1: Return to the Data Risk Assessment Results

1. In the left sub-navigation, select **Discover**. Select **Data risk assessments**.

1. On the **Data risk assessments** page, locate **Zava SharePoint Oversharing Assessment**.

1. Confirm the status shows **Completed**. If the status still shows **In progress**, wait for it to complete before continuing.

1. Select **Zava SharePoint Oversharing Assessment** to open the results.

    


### Task 2: Review Overshared Items

1. On the assessment results page, select the **Items** tab.

1. Review the list of potentially overshared items found across the Zava HR and Finance SharePoint sites.

1. Note the following for each item:

    - **File name**
    - **Sensitivity label** — confirm that HR-Data labelled files appear.
    - **Sharing scope** — note whether items are shared with **Everyone**, **All authenticated users**, or specific groups.
    - **Sensitive info types detected**


1. Locate **Zava_Employee_Records.xlsx** in the results and select it.

1. Review the item detail panel — note the sensitive info types detected, sharing permissions, and label applied.

1. Close the item detail panel.

    


### Task 3: Apply Remediation — Restrict Access by Label

1. On the assessment results page, select the **Protect** tab.

1. Locate the **Restrict access by label** remediation action.

1. Select **Restrict access by label**.

1. On the remediation panel, confirm that **Zava-Confidential/HR-Data** is listed as the label to restrict.

1. Review the action — this will create or reference a DLP policy that restricts access to items carrying the HR-Data label.

1. Select **Apply** or **Confirm** to activate the remediation.

1. Confirm that the remediation action status updates to **Applied**.

    


### Task 4: Apply Remediation — Enable SharePoint Restricted Content Discovery

1. On the **Protect** tab, locate the **Restrict all items** or **Enable Restricted Content Discovery** remediation action.

1. Select the action to open the configuration panel.

1. Review the description — SharePoint Restricted Content Discovery prevents items in the selected site from being surfaced in Microsoft 365 Copilot responses for users who do not have explicit access.

1. Confirm that the scope is set to the **Zava HR SharePoint site**.

1. Select **Apply** or **Enable** to activate Restricted Content Discovery for the Zava HR site.

1. Confirm that the action status updates to **Applied**.

    >[!Note] SharePoint Restricted Content Discovery is one of the most effective controls available to prevent AI agents and Copilot from surfacing content from a SharePoint site to users who lack explicit permission. This differs from DLP — it operates at the site discovery level rather than at the content classification level.

    


## Exercise 5: Investigate Agent Activity and AI Interactions

### Task 1: Review the Apps and Agents Dashboard

1. In the left sub-navigation, select **Discover**.

1. Select **Apps and agents**.

1. On the **Apps and agents** dashboard, review the list of AI apps detected across the tenant.

1. Locate **Copilot Studio** in the platform filter and apply it to show only Copilot Studio agents.

1. Confirm that the three Zava agents appear in the dashboard.

1. Select **Zava HR Assistant** to open its agent details.

1. On the agent details panel, review the following:

    - **Sensitive data accessed** — types and volume of sensitive content the agent has referenced.
    - **Policy coverage** — which Purview policies are protecting data accessed by this agent.
    - **Users** — which users have interacted with this agent.


1. Close the agent details panel.

    


### Task 2: Investigate AI Activities in Activity Explorer

1. In the left sub-navigation, select **Discover**.

1. Select **Activity explorer**.

1. On the **Activity explorer** page, select the **AI activities** tab.

1. In the filter bar, select **User** and enter `Adele Vance`.

1. Select **Apply** to filter results to Adele's interactions.

1. Review the interaction events listed in the filtered view.

1. Select an interaction event that references a sensitive file — for example, one referencing `Zava_Employee_Records.xlsx` or `Zava_Payroll_Q1_2025.xlsx`.

1. On the event detail panel, review the following fields:

    - **Date and time**
    - **User**
    - **Activity type**
    - **AI app**
    - **File referenced**
    - **Sensitivity label on file**
    - **DLP rule matched** — if applicable


1. Note whether the DLP policy **Zava - Block HR Data in M365 Copilot** appears as matched for any of the HR-labelled file interactions.

1. Close the event detail panel.

1. Remove the user filter and apply a filter for **Sensitivity label** set to **Zava-Confidential/HR-Data**.

1. Review the results — these show all AI interactions across the tenant that involved a file carrying the HR-Data label.

    


## Summary

In this lab, you initiated a custom DSPM data risk assessment against the Zava HR and Finance SharePoint sites at the start of Day 3, ensuring results were available for investigation later in the lab. You registered an Entra app and configured the item-level scan connection required by DSPM. As Adele Vance, you generated three realistic Microsoft 365 Copilot interaction events referencing sensitive labelled files including employee records, payroll data, and financial projections — creating the AI activity signals needed for investigation throughout Day 3.

You explored the DSPM Posture dashboard and reviewed its key metrics, top objectives, and Security Copilot suggested prompts. You activated two one-click policies: the DSPM for AI risky AI usage Insider Risk Management policy and the sensitive info detection DLP policy for Copilot interactions. You reviewed the data risk assessment results, identified overshared sensitive items in the Zava HR and Finance sites, and applied two remediation actions: restricting access by the HR-Data sensitivity label and enabling SharePoint Restricted Content Discovery on the Zava HR site. Finally, as Patti Fernandes, you investigated Adele Vance's Copilot interaction events in the DSPM Activity Explorer AI activities tab, reviewing file references, sensitivity labels, and DLP match records — building the evidence base for the Day 3 compliance review.
