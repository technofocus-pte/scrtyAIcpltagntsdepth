<!--
lab:
  title: 'Lab 01: Agent Discovery and Registry Management'
  description: In this lab, you explored the Agent 365 Overview dashboard and reviewed key governance metrics for the Zava tenant. You inspected all three Zava agents in the Agent Registry, reviewing their metadata, host products, and knowledge sources. You approved the Zava IT Support Agent submission, published the Zava HR Assistant through Teams Admin Center, then blocked and unblocked the Zava HR Assistant to verify that lifecycle controls function correctly. You exported the agent inventory to a CSV file to confirm audit trail capability, and used the ownerless agent filter to check for governance gaps in agent ownership.
  duration: 20 minutes
  level: 300
  islab: true
-->

# Lab 01: Agent Discovery and Registry Management

## Introduction

Zava's CISO has asked the security team to confirm that all deployed AI agents are visible, governed, and accounted for before any security policy work begins. Patti Fernandes, Zava's Security Admin and SOC Analyst, will use the Agent Registry in Microsoft Entra ID to inspect the three Zava agents, test lifecycle controls, and identify any governance gaps. The Defender XDR workspace and Defender for Cloud Apps connector will also be initialised so that agent activity data begins flowing before moving further in the security journey.

This lab introduces the Microsoft 365 Admin Center Agent Registry as the primary tool for agent discovery, lifecycle management, and governance. Patti will explore the registry, review agent metadata, take lifecycle actions, identify ownerless agents, and complete the prerequisite configuration tasks required for Day 2 security labs — including Purview Audit verification, Defender XDR provisioning, and Defender for Cloud Apps initialisation.



### Objectives

- Explore the Agent 365 Overview dashboard and interpret key metrics.
- Inspect all three Zava agents in the Agent Registry and review their metadata.
- Block and unblock the Zava HR Assistant to validate lifecycle controls.
- Export the agent inventory to confirm audit trail capability.
- Identify ownerless agents using the registry dashboard filter.
- Verify that Purview Audit is active and run a baseline audit log search.
- Provision Microsoft Defender XDR by signing in to the Defender portal.
- Configure Defender for Cloud Apps organisation details and connect the Microsoft 365 app connector.




## Lab Duration

Estimated time: **20 minutes**



## Exercise 1: Explore the Agent 365 Overview and Agent Registry

### Task 1: Access the Agent 365 Overview Page

1. Open a browser and navigate to +++https://admin.cloud.microsoft/+++. Sign in with **MOD Administrator** credentials if prompted.

1. In the left navigation pane, expand **Agents**, and then select **Overview**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2001/media/image1.png)

1. On the **Agent Overview** page, locate the following metrics and note their current values:

    - **Agent Registry** — total count of agents in the tenant.
    - **Active users in Copilot** — unique users who interacted with an agent in the last 30 days.
    - **Pending requests for agents** — open requests to add specific agents.
    - **Agents without owners** — agents whose owner has left the company.
    - **Agent analytics** — agents by creators, top platforms used to build agents, and active users in Copilot over time.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2001/media/image2.png)

    >[!Note] In a freshly configured environment, active user counts and agents without owners may show zero. This is expected. The metrics will populate as agents are used throughout the course.

    


### Task 2: Inspect and Approve the Zava Agents in the Agent Registry

1. In the left navigation pane, select **Agents**. Select **All agents**. Then select the **Requests** tab.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2001/media/image3.png)

1. In the agent list, locate **Zava IT Support Agent** and select the vertical **...** next to the name.

1. From the two options, you can either **Reject submission** or **Publish to store**. For now, select **Publish to store**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2001/media/image4.png)

1. On the **Publish new agent** flow, under **Select users or groups who can install the agent**, select **All users**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2001/media/image5.png)

1. Under **Select users or groups who will have the agent pre-installed (optional)**, select **Specific users/groups**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2001/media/image6.png)

1. In the **Specific users/groups** search box, search for `Adele Vance` and select her from the dropdown.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2001/media/image7.png)

1. Add **Patti Fernandes** as well.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2001/media/image8.png)

1. Then select **Next**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2001/media/image9.png)

1. On **Apply security template**, select **Next**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2001/media/image10.png)

1. On **Review permissions**, select **Next**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2001/media/image11.png)

1. Select the **Registry** tab, search for `Zava IT Support Agent`, select it, and note the information in the details pane.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2001/media/image12.png)

    


### Task 3: Approve an Agent in Teams Admin Center

1. Open a new browser tab and navigate to +++https://admin.teams.microsoft.com/+++ and log in using the MOD Administrator credentials.

1. From the left navigation under **Teams apps**, select **Manage apps**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2001/media/image13.png)

1. In the search bar, search for `Zava` and select **Zava HR Assistant**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2001/media/image14.png)

1. On the **Zava HR Assistant** page, select **Publish**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2001/media/image15.png)

1. On the confirmation dialog, select **Publish** again.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2001/media/image16.png)

    


### Task 4: Block and Unblock the Zava HR Assistant

1. Navigate back to `https://admin.cloud.microsoft/`. Sign in with **MOD Administrator** credentials if prompted.

1. On the **All agents** page, select the **Registry** tab, then search for and select **Zava HR Assistant** in the agent list.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2001/media/image17.png)

1. On the details panel, below the agent name, select **Block**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2001/media/image18.png)

1. On the **Block agent** pane, review the message confirming that blocking will prevent all users in the organisation from accessing the agent. Check the box next to **Block agent**. Select **Save**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2001/media/image19.png)

1. Confirm that **Zava HR Assistant** now displays a **Blocked** status.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2001/media/image20.png)

1. Below the agent name, select **Unblock**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2001/media/image21.png)

1. On the **Unblock agent** pane, select the **Unblock agent** checkbox. Select **Save**. Close the details panel.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2001/media/image22.png)

1. In the agent list, confirm that **Zava HR Assistant** now displays an **Active** status.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2001/media/image23.png)

    


### Task 5: Export the Agent Inventory

1. On the **Registry** tab, select **Export** on the toolbar above the agent list.

    >[!Note] If an **Export** button is not visible in the toolbar, select the ellipsis (**...**) menu in the toolbar to locate the export option.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2001/media/image24.png)

1. Confirm the download in the confirmation dialog. Wait for the export file to be generated and downloaded to your lab VM.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2001/media/image25.png)

1. Open the downloaded CSV file.

1. Confirm that the file contains rows for **Zava HR Assistant**, **Zava Finance Agent**, and **Zava IT Support Agent**.

1. Confirm that the following columns are present: agent name, publisher, creator, creation date, host products, and availability status.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2001/media/image26.png)

1. Close the CSV file.

    


### Task 6: Identify Ownerless Agents

1. On the **Registry** tab, select the **Missing an owner** card.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2001/media/image27.png)

1. Review the list of agents that are displayed after applying the ownerless filter.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2001/media/image28.png)

1. Note whether any of the three Zava agents appear in this filtered list.

    >[!Note] In a lab environment where agents were created by MOD Administrator, the agents may or may not appear as ownerless depending on how ownership is propagated from Copilot Studio. If no agents appear, this confirms that ownership was correctly assigned during creation. If agents appear, this represents a governance gap that would be addressed by reassigning ownership.

1. Select **Clear filter** or reset the filters to return to the full agent list.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2001/media/image29.png)

    


## Exercise 2: Prepare Purview Audit for Day 2

### Task 1: Verify Purview Audit Is Active

1. Open a new browser tab and navigate to +++https://purview.microsoft.com+++. Sign in with **MOD Administrator** credentials if prompted. Select **Get started**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2001/media/image30.png)

1. In the left navigation pane, select **Solutions**, then select **Audit**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2001/media/image31.png)

1. On the **Audit** page, check whether a banner appears prompting you to start recording user and admin activity.

    - If a banner is displayed, select **Start recording user and admin activity** to enable auditing.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2001/media/image32.png)

    - If no banner is displayed, auditing is already enabled. Proceed to the next step.


1. Configure the search with the following values:

    - **Start date:** Select today's date minus 3 days.
    - **End date:** Select today's date.
    - **Activities – friendly names:** Leave blank to search all activities.
    - **Users:** Leave blank.
    - **Record type:** Leave blank.


1. Select **Search**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2001/media/image33.png)

1. Wait for the search job to complete.

1. Review the results to confirm that audit records are being returned.

    >[!Note] If the search returns no results, this may indicate that no audited activities have occurred yet in the tenant, or that audit log ingestion requires additional time after initial provisioning. This is expected in a new lab environment. Audit records generated throughout this and subsequent labs will be searchable from Day 2 onwards.

    


## Exercise 3: Initialise Microsoft Defender XDR and Defender for Cloud Apps

### Task 1: Provision Microsoft Defender XDR

1. Open a new browser tab and navigate to +++https://security.microsoft.com+++. Sign in with **MOD Administrator** credentials if prompted.

1. On the **Microsoft Defender** portal welcome screen, review the provisioning message if displayed.

    >[!Note] Microsoft Defender XDR provisions automatically when an eligible admin visits the portal for the first time. If provisioning is in progress, a message will indicate the data centre location being used and an estimated completion time. Wait for provisioning to complete before continuing.

1. Once the portal loads fully, confirm that the left navigation pane displays the following sections: **Home**, **Incidents & alerts**, **Hunting**, **Threat intelligence**, **Assets**, **Identities**, **Endpoints**, **Email & collaboration**, **Cloud Apps**, and **Settings**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2001/media/image34.png)

1. Select **Home** to confirm the Defender XDR home dashboard loads without errors.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2001/media/image35.png)

    


### Task 2: Configure Defender for Cloud Apps Organisation Details

1. In the Microsoft Defender portal at `https://security.microsoft.com`, in the left navigation pane, select **Settings**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2001/media/image36.png)

1. On the **Settings** page, select **Cloud Apps**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2001/media/image37.png)

1. Select **Organisation details**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2001/media/image38.png)

1. On the **Organisation details** page, in the **Organisation display name** field, enter `Zava Corporation`.

1. In the **Environment name** field, enter `Dev One`.

1. In the **Managed domains** field, enter your tenant's primary domain in the following format:

    `[TenantPrefix].onmicrosoft.com`

    >[!Note] Replace **[TenantPrefix]** with your tenant prefix from the **Resources** tab. Adding managed domains ensures that internal users are correctly identified in Cloud Apps reports and alerts.

1. Select **Save**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2001/media/image39.png)

1. Confirm that a success notification appears confirming that the settings were saved.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2001/media/image40.png)

    


### Task 3: Enable File Monitoring in Defender for Cloud Apps

1. In the Microsoft Defender portal, in the left navigation pane, select **Settings**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2001/media/image36.png)

1. On the **Settings** page, select **Cloud Apps**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2001/media/image37.png)

1. Under **Information Protection**, select **Files**. On the **Files** page, select the **Enable file monitoring** checkbox. Select **Save**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2001/media/image46.png)

1. Confirm that a success notification appears confirming that file monitoring was enabled.

    


### Task 4: Connect the Microsoft 365 App Connector

1. In the Microsoft Defender portal, in the left navigation pane, select **Settings**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2001/media/image36.png)

1. On the **Settings** page, select **Cloud Apps**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2001/media/image37.png)

1. Under **Connected apps**, select **App Connectors**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2001/media/image41.png)

1. On the **App Connectors** page, select **+ Connect an app**.

1. In the app list, select **Microsoft 365**.

1. On the **Select Microsoft 365 components** page, confirm that all components are selected by default. If any component is deselected, select it to enable it.

1. Select **Connect Microsoft 365**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2001/media/image42.png)

1. Wait for the connection to complete. Then slect **Done**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2001/media/image43.png)

1. On the **App Connectors** page, confirm that **Microsoft 365** appears in the connectors list with a status of **Connected**.

1. On the **App Connectors** page, select the checkbox next to **Microsoft 365** and from the top options select **Connect Microsoft Azure Instance**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2001/media/image44.png)

1. Select **Connect Microsoft Azure**. Wait for the connection to complete.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2001/media/image45.png)

    >[!Note] After connecting, Defender for Cloud Apps begins scanning Microsoft 365 activity. Initial data from the past week will appear in the portal. The first full scan may take several hours depending on tenant size. This connector is required for activity monitoring, DLP policy enforcement, and alert generation in Day 2 and Day 3 labs.

    


## Summary

In this lab, you explored the Agent 365 Overview dashboard and reviewed key governance metrics for the Zava tenant. You inspected all three Zava agents in the Agent Registry, reviewing their metadata, host products, and knowledge sources. You approved the Zava IT Support Agent submission, published the Zava HR Assistant through Teams Admin Center, then blocked and unblocked the Zava HR Assistant to verify that lifecycle controls function correctly. You exported the agent inventory to a CSV file to confirm audit trail capability, and used the ownerless agent filter to check for governance gaps in agent ownership.

You then prepared the Day 2 monitoring infrastructure by verifying that Purview Audit is active and running a baseline audit log search. You provisioned Microsoft Defender XDR, configured Defender for Cloud Apps with Zava Corporation organisation details and managed domain, connected the Microsoft 365 app connector to begin activity data ingestion, and enabled file monitoring.

The Zava agent environment is now fully visible, governed, and ready for security policy configuration in Day 2.
