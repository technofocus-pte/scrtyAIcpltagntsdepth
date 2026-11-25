<!--
lab:
  title: 'Lab 05: Microsoft Defender — AI Agent Inventory and Threat Hunting'
  description: In this lab, MOD Administrator will enable Defender preview features, activate the Copilot Studio AI agent inventory, and connect it to Power Platform. Patti Fernandes will then explore the AI agent inventory, investigate Zava agent configurations, and run Advanced Hunting KQL queries to identify potential security risks across the Zava agent estate.
  duration: 60 minutes
  level: 300
  islab: true
  primarytopics:
    - Microsoft Defender
-->

# Lab 05: Microsoft Defender — AI Agent Inventory and Threat Hunting

## Introduction

Microsoft Defender for Cloud Apps provides a dedicated AI agent inventory that discovers all Copilot Studio custom agents in the tenant and exposes them for security investigation. Combined with the Advanced Hunting `AIAgentsInfo` table in Microsoft Defender XDR, the security team can query agent configurations, detect misconfigurations, identify governance gaps, and proactively hunt for risky agent behaviour — all without leaving the Defender portal.

In this lab, MOD Administrator will enable Defender preview features, activate the Copilot Studio AI agent inventory, and connect it to Power Platform. Patti Fernandes will then explore the AI agent inventory, investigate Zava agent configurations, and run Advanced Hunting KQL queries to identify potential security risks across the Zava agent estate.



## Scenario

Zava's SOC team has been asked to confirm that all deployed AI agents are visible in the Defender portal and that the security team has the tooling in place to hunt for misconfigured or risky agents. Patti Fernandes will use the AI agent inventory to review Zava agent properties — including authentication type, knowledge sources, and owner assignments — and run a series of community and custom KQL queries to surface any configuration risks. Any findings will be documented for the CISO review at the end of Day 2.



### Objectives

- Enable Microsoft Defender preview features for Cloud Apps, Defender for Cloud, and Defender XDR.
- Enable the Copilot Studio AI agent inventory in Defender for Cloud Apps settings.
- Complete the AI agent inventory onboarding in Power Platform Admin Center.
- Confirm the green Connected status in the Defender portal.
- Explore the AI agent inventory and review Zava agent details.
- Use Go hunt to open Advanced Hunting pre-filtered for a specific agent.
- Run community queries from the AI Agents folder to identify unauthenticated and misconfigured agents.
- Run a custom KQL query to review all Zava agent configurations in a single view.
- Review the Defender Alerts queue for Cloud Apps agent-related activity.




## Lab Duration

Estimated time: **60 minutes**



## Exercise 1: Enable Defender Preview Features

### Task 1: Enable Preview Features in Microsoft Defender XDR

1. Open a browser and navigate to +++https://security.microsoft.com+++.

1. Sign in with **MOD Administrator** credentials if prompted.

1. In the left navigation pane, select **System** > **Settings**.

1. On the **Settings** page, select **Microsoft Defender XDR**.

1. In the left sub-navigation, select **Preview features**.

1. On the **Preview features** page, set the **Preview features** toggle to **On**.

1. Select **Save preferences**.

1. Confirm that a success notification appears.

    


### Task 2: Enable Preview Features for Defender for Cloud Apps

1. In the left navigation pane, select **Settings**.

1. On the **Settings** page, select **Cloud Apps**.

1. Under **System**, select **Preview features**.

1. On the **Preview features** page, set the **Preview features** toggle to **On**.

1. Select **Save**.

1. Confirm that the setting is saved.

    >[!Note] Enabling preview features is required to access the Copilot Studio AI agent inventory and the `AIAgentsInfo` advanced hunting table. Without this setting, the AI Agents option will not appear under Assets in the Defender portal. -->
    


## Exercise 2: Enable the Copilot Studio AI Agent Inventory

### Task 1: Enable Copilot Studio AI Agents in Defender for Cloud Apps Settings

1. In the Microsoft Defender portal at `https://security.microsoft.com`, in the left navigation pane, select **Settings** under **System**.

1. On the **Settings** page, select **Cloud Apps**.

1. In the left sub-navigation, under **System**, select **Copilot Studio AI Agents**.

1. On the **Copilot Studio AI Agents** page, set the **Copilot Studio AI Agents** toggle to **On**.

1. Read the disclaimer that appears and select **I agree** or **Turn on** to confirm.

1. Note the **AI Agents Inventory** status indicator — it will initially show as pending or disconnected.

    >[!Note] Enabling this setting initiates the connection between Defender for Cloud Apps and Copilot Studio. The second step in Power Platform Admin Center must be completed before the green Connected status appears.

    


### Task 2: Complete Onboarding in Power Platform Admin Center

1. Open a new browser tab and navigate to +++https://admin.powerplatform.microsoft.com+++.

1. Sign in with **MOD Administrator** credentials if prompted.

1. In the left navigation pane, select **Security**.

1. Under **Security**, select **Threat Protection**.

1. On the **Threat Protection** page, locate **Microsoft Defender - Copilot Studio AI Agents**.

1. Select **Microsoft Defender - Copilot Studio AI Agents** to open its settings.

1. Set the **Enable Microsoft Defender - Copilot Studio AI Agents** toggle to **On**.

1. Select **Save** to apply the setting.

    


### Task 3: Confirm Connected Status in the Defender Portal

1. Return to the **MOD Administrator** browser session at `https://security.microsoft.com`.

1. In the left navigation pane, select **Settings**.

1. On the **Settings** page, select **Cloud Apps**.

1. Under **System**, select **Copilot Studio AI Agents**.

1. On the **Copilot Studio AI Agents** page, check the **AI Agents Inventory** status indicator.

1. Confirm that a green **Connected** indicator is displayed.

    >[!Note] It can take up to 30 minutes for the initial connection status to update after completing both onboarding steps. If the indicator has not turned green yet, proceed to Exercise 3 and return to verify the status after completing the Advanced Hunting exercises. The inventory data may take additional time to fully populate depending on the size of the environment.

    


## Exercise 3: Explore the AI Agent Inventory

### Task 1: Access the AI Agents Inventory

1. In the Microsoft Defender portal at `https://security.microsoft.com`, in the left navigation pane, select **Assets**.

1. Under **Assets**, select **AI Agents**.

    >[!Note] If **AI Agents** is not visible under Assets, confirm that preview features were enabled in Exercise 1 and that the inventory connection completed in Exercise 2. Wait up to 30 minutes after completing Exercise 2 before retrying.

1. On the **AI Agents** page, review the full list of agents discovered in the Zava tenant.

1. In the **Platform** filter, select **Copilot Studio** to filter the view to Copilot Studio custom agents only.

1. Confirm that the following three agents appear in the inventory:

    | Agent Name | Status | Platform |
    ||||
    | Zava HR Assistant | Published | Copilot Studio |
    | Zava Finance Agent | Published | Copilot Studio |
    | Zava IT Support Agent | Published | Copilot Studio |

    


### Task 2: Review the Zava HR Assistant Agent Details

1. On the **AI Agents** page, select **Zava HR Assistant** to open its details panel.

1. On the details panel, review and note the following fields:

    - **Agent name**
    - **Status**
    - **Creator**
    - **Owner UPNs**
    - **Authentication type**
    - **Access control policy**
    - **Knowledge sources**
    - **Last modified**
    - **Last published**


1. Under **Knowledge sources**, confirm that the Zava HR SharePoint site is listed.

1. Under **Authentication type**, note the current value.

    >[!Note] If **Authentication type** shows **None**, this indicates the agent does not require users to authenticate before interacting with it. This is a significant security risk in an enterprise environment and will appear in the Advanced Hunting community query results in Exercise 4. If the authentication type shows **Microsoft**, the agent is correctly configured to require Entra ID authentication.

1. Under **Access control policy**, note the current value and confirm whether access is restricted to specific groups or open to all users.

    


### Task 3: Use Go Hunt to Open Advanced Hunting for the Zava HR Assistant

1. On the **Zava HR Assistant** details panel, locate the **Go hunt** button or link.

1. Select **Go hunt**.

1. Confirm that the browser navigates to **Investigation & response > Hunting > Advanced hunting** with a pre-populated query scoped to the Zava HR Assistant agent.

1. Review the pre-populated query to understand its structure.

1. Select **Run query** to execute it.

1. Review the results returned in the query output panel.

1. Note the columns returned, including `AIAgentName`, `AgentStatus`, `CreatorAccountUpn`, `KnowledgeDetails`, and `UserAuthenticationType`.

    


## Exercise 4: Run Advanced Hunting Queries Against AIAgentsInfo

### Task 1: Sign In to the Defender Portal as Patti Fernandes

1. Open a new **InPrivate** or **Incognito** browser window.

1. Navigate to +++https://security.microsoft.com+++.

1. Sign in with **Patti Fernandes** credentials from the **Resources** tab.

1. In the left navigation pane, select **Investigation & response**.

1. Under **Investigation & response**, select **Hunting**.

1. Select **Advanced hunting**.

    


### Task 2: Run the Agents with No Authentication Community Query

1. On the **Advanced hunting** page, select the **Queries** tab.

1. In the queries panel, select **Community queries**.

1. In the community queries search bar, enter `AI Agents` to locate the AI Agents query folder.

1. Select the **AI Agents** folder to expand it.

1. Select the query named **Agent with no authentication**.

1. Review the query in the query editor:

    ```kql
    AIAgentsInfo
    | summarize arg_max(Timestamp, *) by AIAgentId
    | where AgentStatus != "Deleted"
    | where UserAuthenticationType == "None"
    | project-reorder AgentCreationTime, AIAgentId, AIAgentName, AgentStatus, CreatorAccountUpn, OwnerAccountUpns
    ```

1. Select **Run query**.

1. Review the results.

1. If any Zava agents appear in the results, note their names and creator UPNs.

    >[!Note] Agents with no authentication are publicly accessible to any user who can reach the agent endpoint. In an enterprise environment, all agents should be configured to require Microsoft authentication. If the Zava agents appear here, this represents a configuration risk that should be flagged to the CISO. The recommended remediation is to update the agent's authentication settings in Copilot Studio.

    


### Task 3: Run the Hard-Coded Credentials Community Query

1. On the **Queries** tab, in the **AI Agents** community queries folder, select the query named **Hard-coded credentials in Topics or Actions**.

1. Review the query in the query editor:

    ```kql
    let suspicious_patterns = @"(AKIA[0-9A-Z]{16})|(AIza[0-9A-Za-z_\-]{35})|(xox[baprs]-[0-9a-zA-Z]{10,48})|(ghp_[A-Za-z0-9]{36,59})|(sk_(live|test)_[A-Za-z0-9]{24})|(SG\.[A-Za-z0-9]{22}\.[A-Za-z0-9]{43})|(\d{8}:[\w\-]{35})|(eyJ[A-Za-z0-9_\-]+\.[A-Za-z0-9_\-]+\.[A-Za-z0-9_\-]+)|(Authorization\s*:\s*Basic\s+[A-Za-z0-9=:+]+)|([A-Za-z]+:\/\/[^\/\s]+:[^\/\s]+@[^\/\s]+)";
    AIAgentsInfo
    | summarize arg_max(Timestamp, *) by AIAgentId
    | where AgentStatus != "Deleted"
    | mv-expand tool = AgentToolsDetails
    | mv-expand topic = AgentTopicsDetails
    | where isnotempty(tool) and isnotempty(topic)
    | where tool matches regex suspicious_patterns or topic matches regex suspicious_patterns
    | extend SuspiciousMatchTool = tool, SuspiciousMatchTopic = topic
    | project-reorder AgentCreationTime, AIAgentId, AIAgentName, AgentStatus, CreatorAccountUpn, OwnerAccountUpns, SuspiciousMatchTool, SuspiciousMatchTopic
    ```

1. Select **Run query**.

1. Review the results.

1. Confirm whether any of the three Zava agents have matched credential patterns in their topic or action configurations.

    >[!Note] In a correctly configured lab environment, no Zava agents should match this query. A result here would indicate that an agent's topic or action definition contains a hard-coded API key, token, or credential string — a critical security risk that requires immediate remediation by updating the agent configuration in Copilot Studio and rotating the exposed credential.

    


### Task 4: Run a Custom Zava Agent Configuration Review Query

1. On the **Advanced hunting** page, select the **New query** tab to open a blank query editor.

1. In the query editor, enter the following KQL query:

    ```kql
    AIAgentsInfo
    | summarize arg_max(Timestamp, *) by AIAgentId
    | where AgentStatus != "Deleted"
    | where AIAgentName has_any ("Zava HR Assistant", "Zava Finance Agent", "Zava IT Support Agent")
    | project
        AgentCreationTime,
        AIAgentName,
        AgentStatus,
        CreatorAccountUpn,
        OwnerAccountUpns,
        UserAuthenticationType,
        AccessControlPolicy,
        KnowledgeDetails,
        LastModifiedTime,
        LastModifiedByUpn,
        LastPublishedTime,
        LastPublishedByUpn
    | sort by AgentCreationTime asc
    ```

1. Select **Run query**.

1. Review the results returned for all three Zava agents.

1. For each agent, note the values in the following columns:

    - **UserAuthenticationType** — confirm all three show **Microsoft** (not **None**).
    - **AccessControlPolicy** — confirm the policy is not set to **Any** (which would allow any user in or outside the tenant).
    - **OwnerAccountUpns** — confirm that Patti Fernandes appears as owner of the Zava Finance Agent (assigned in Lab 02).
    - **KnowledgeDetails** — confirm the correct SharePoint site is listed for each agent.


1. Select **Save** to save the query.

1. In the **Save query** panel, enter the following:

    - **Query name:** `Zava Agent Configuration Review`
    - **Description:** `Reviews key security configuration properties for all three Zava Copilot Studio agents.`
    - **Folder:** Select **My queries**.


1. Select **Save** to confirm.

    


### Task 5: Run a Custom Query to Identify Agents with Broad Access Control

1. On the **Advanced hunting** page, select **New query**.

1. In the query editor, enter the following KQL query:

    ```kql
    AIAgentsInfo
    | summarize arg_max(Timestamp, *) by AIAgentId
    | where AgentStatus != "Deleted"
    | where AccessControlPolicy in ("Any", "Any (multitenant)")
    | project-reorder
        AgentCreationTime,
        AIAgentId,
        AIAgentName,
        AgentStatus,
        AccessControlPolicy,
        CreatorAccountUpn,
        OwnerAccountUpns
    ```

1. Select **Run query**.

1. Review the results.

1. Note whether any Zava agents or other tenant agents appear in the results.

    >[!Note] Agents with an Access Control Policy of **Any** are accessible to all users in the tenant without restriction. Agents with **Any (multitenant)** are accessible to users outside the organisation. Both configurations represent significant oversharing risks. In Day 3, the DSPM and oversharing lab will address how to remediate broad-access configurations across SharePoint and agent knowledge sources.

    


## Exercise 5: Review the Defender Alerts Queue for Agent-Related Activity

### Task 1: Filter the Alerts Queue by Cloud Apps Source

1. Remain signed in as **Patti Fernandes** in the Microsoft Defender portal.

1. In the left navigation pane, select **Incidents & alerts**.

1. Select **Alerts**.

1. On the **Alerts** page, select **Add filter**.

1. In the filter dropdown, select **Service source**.

1. Select **Microsoft Defender for Cloud Apps** as the filter value.

1. Select **Apply**.

1. Review the alerts returned in the filtered view.

1. If any alerts are present, select an alert to open its detail panel.

1. On the alert detail panel, review the following fields:

    - **Alert name**
    - **Severity**
    - **Status**
    - **Affected entity**
    - **Detection source**
    - **Activity log**


1. Close the alert detail panel.

    >[!Note] In a newly configured lab environment, the Cloud Apps alerts queue may be empty or contain only connector-related events. Agent-related alerts will begin appearing as the Zava agents are invoked, real-time protection signals are generated, and policy violations occur across Day 2 and Day 3 labs. This step establishes familiarity with the alerts queue that Patti will use for incident investigation in Day 3.

    


### Task 2: Check for Any Agent-Specific Incidents

1. In the left navigation pane, select **Incidents & alerts**.

1. Select **Incidents**.

1. On the **Incidents** page, in the search bar, enter `Zava`.

1. Review any incidents returned that reference Zava agent activity.

1. If an incident is present, select it to open the incident detail page.

1. On the incident detail page, review the **Alerts** tab to see all alerts grouped into the incident.

1. Review the **Evidence and response** tab to see affected entities.

1. Close the incident and return to the **Incidents** page.

    >[!Note] If no Zava-related incidents appear, this is expected at this stage of the course. Note the search and filter techniques demonstrated here — they will be used in Day 3 when active threat investigation tasks are introduced.

    


## Summary

In this lab, you enabled Microsoft Defender preview features for Defender XDR and Defender for Cloud Apps, which are required to access the Copilot Studio AI agent inventory and the `AIAgentsInfo` advanced hunting schema. You enabled the Copilot Studio AI agent inventory in Defender for Cloud Apps settings and completed the corresponding onboarding step in Power Platform Admin Center to establish the data connection. You confirmed the green Connected status in the Defender portal. You explored the AI agent inventory under Assets, reviewed the Zava HR Assistant agent details including its authentication type, access control policy, knowledge sources, and owner assignments, and used the Go hunt action to open Advanced Hunting pre-filtered for that agent.

As Patti Fernandes, you ran two community queries from the AI Agents folder — detecting agents with no authentication and agents with hard-coded credentials — and reviewed the results against the Zava agent estate. You ran a custom Zava Agent Configuration Review KQL query to surface key security properties for all three Zava agents in a single view, and saved it for future use. You ran a second custom query to identify agents with overly broad access control policies. Finally, you reviewed the Defender Alerts queue filtered by Cloud Apps source and checked the Incidents page for any Zava-related activity, establishing the investigation baseline for Day 3.

Day 2 is now complete. Zava's agents are governed by Conditional Access policies, sensitive data is classified with Purview labels and protected by DLP controls, and the security team has full visibility into agent configurations and activity through the Defender AI agent inventory and Advanced Hunting.
