<!--
lab:
  title: 'Lab 04: Microsoft Purview — Sensitivity Labels and DLP for Copilot'
  description: In this lab, MOD Administrator will enable sensitivity label support for SharePoint and OneDrive, build Zava's label taxonomy using a label group and child labels, configure auto-labelling for financial data, publish labels to users, and create a DLP policy that prevents Microsoft 365 Copilot from processing labelled content. Adele Vance will test whether Copilot is blocked from surfacing labelled content. Patti Fernandes will verify the audit trail.
  duration: 30 minutes
  level: 200
  islab: true
  primarytopics:
    - Microsoft 365
    - Microsoft 365 Copilot
    - Microsoft Purview
-->

# Lab 04: Microsoft Purview — Sensitivity Labels and DLP for Copilot

## Introduction

Zava's information security team has identified that the Zava Finance Agent and Zava HR Assistant can retrieve and surface content from SharePoint without any awareness of how sensitive that content is. The CISO has mandated that all sensitive HR and financial documents must be labelled before the end of Day 2, and that Microsoft 365 Copilot must be prevented from processing documents labelled as confidential HR data.

Zava Corporation handles sensitive employee records, financial data, and vendor contracts across SharePoint sites that are now connected to AI agents. Without sensitivity labels and Data Loss Prevention policies, these agents can surface protected content to any user who asks — regardless of their access rights or data handling obligations.

In this lab, MOD Administrator will enable sensitivity label support for SharePoint and OneDrive, build Zava's label taxonomy using a label group and child labels, configure auto-labelling for financial data, publish labels to users, and create a DLP policy that prevents Microsoft 365 Copilot from processing labelled content. Adele Vance will test whether Copilot is blocked from surfacing labelled content. Patti Fernandes will verify the audit trail.



### Objectives

- Enable sensitivity label co-authoring support for SharePoint and OneDrive.
- Create a Zava label group and two child labels for HR and Financial data.
- Configure an auto-labelling policy to automatically apply the Financial Data label to content containing financial sensitive information types.
- Publish both labels to all Zava users.
- Apply the HR Data label manually to Zava HR SharePoint files.
- Create a DLP policy targeting the Microsoft 365 Copilot location to block processing of HR-labelled content.
- Test DLP enforcement as Adele Vance via Microsoft 365 Copilot Chat.
- Investigate the DLP match audit event as Patti Fernandes in Purview Audit.




## Lab Duration

Estimated time: **30 minutes**



## Exercise 1: Enable Sensitivity Label Support for SharePoint and OneDrive

### Task 1: Enable Co-Authoring for Files with Sensitivity Labels

1. Open a browser and navigate to +++https://purview.microsoft.com+++. Sign in with **MOD Administrator** credentials if prompted. In the left navigation pane, select **Settings**.

1. Under **Settings**, select **Information Protection**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image1.png)

1. On the **Information Protection settings** page, select the **Co-authoring for files with sensitivity labels** tab.

1. Select the checkbox for **Turn on co-authoring for files with sensitivity labels**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image2.png)

1. Select **Apply** at the bottom of the page.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image3.png)

    >[!Note] Enabling co-authoring also activates sensitivity label support for files stored in SharePoint and OneDrive. This is a prerequisite for applying labels to SharePoint-hosted files and for Defender for Cloud Apps to scan files for label metadata. Without this setting, the Sensitivity button will not appear in Office for the web.

    


## Exercise 2: Create the Zava Sensitivity Label Taxonomy

### Task 1: Create the Zava-Confidential Label Group

1. In the left navigation pane of the Microsoft Purview portal, select **Solutions**. Select **Information Protection**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image4.png)

1. In the left sub-navigation, select **Sensitivity labels**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image5.png)

1. If asked, select **Get started** for migrating to the new label scheme.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image6.png)

1. Select **Migrate** on the fly-out pane and then **Confirm migration**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image7.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image8.png)

1. On the **Sensitivity labels** page, select **+ Create** and then select **Label group**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image9.png)

1. On the **New label group** configuration page, on the **Provide basic details for this label group** step, enter the following:

    - **Name:** `Zava-Confidential`
    - **Display name:** `Zava-Confidential`
    - **Description for users:** `Use this label group for all Zava confidential content requiring restricted handling.`
    - **Description for admins:** `Zava confidential label group. Contains child labels for HR and Financial data classifications.`


1. Select **Next**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image10.png)

1. On the **Review your settings and finish** page, select **Create label group**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image11.png)

1. On the **Your label group was created** page, select **Done**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image12.png)

1. Confirm that **Zava-Confidential** appears in the label list.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image13.png)

    


### Task 2: Create the HR Data Child Label

1. On the **Sensitivity labels** page, locate the **Zava-Confidential** label group. Select the vertical ellipsis (**…**) next to **Zava-Confidential**. Select **+ Create label in group** from the dropdown menu.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image14.png)

1. On the **Provide basic details for this label** page, enter the following:

    - **Name:** `HR-Data`
    - **Display name:** `HR-Data`
    - **Description for users:** `Apply this label to documents containing Zava employee data including personnel files, payroll records, sick leave reports, and termination documentation.`
    - **Description for admins:** `Child label of Zava-Confidential. Used to classify HR documents on the Zava HR SharePoint site. Triggers DLP enforcement in Microsoft 365 Copilot.`


1. Select **Next**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image15.png)

1. On the **Define the scope for this label** page, select **Files** and **Emails**. Ensure **Meetings** is deselected. Select **Next**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image16.png)

1. On the **Choose protection settings for labeled items** page, select **Apply content marking**. Select **Next**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image17.png)

1. On the **Content marking** page, set the **Content marking** toggle to **On**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image18.png)

1. Select the checkbox for **Add a header**. Select the edit icon below to **Add a header**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image19.png)

1. In the **Header text** field, enter `ZAVA CONFIDENTIAL — HR DATA`. Select **Save**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image20.png)

1. Select the checkbox for **Add a footer**. Select the edit icon below to **Add a footer**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image21.png)

1. In the **Footer text** field, enter `Restricted — Zava HR use only`. Select **Save**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image22.png)

1. Select **Next**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image23.png)

1. On the **Auto-labeling for files and emails** page, select **Next**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image24.png)

1. On the **Define protection settings for groups and sites** page, select **Next**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image25.png)

1. On the **Review your settings and finish** page, select **Create label**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image26.png)

1. On the **Your sensitivity label was created** page, select **Don't create a policy yet**. Select **Done**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image27.png)

    


### Task 3: Create the Financial Data Child Label with Auto-Labelling

1. On the **Sensitivity labels** page, locate the **Zava-Confidential** label group. Select the vertical ellipsis (**…**) next to **Zava-Confidential**. Select **+ Create label in group** from the dropdown menu.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image28.png)

1. On the **Provide basic details for this label** page, enter the following:

    - **Name:** `Financial-Data`
    - **Display name:** `Financial-Data`
    - **Description for users:** `Apply this label to documents containing Zava financial data including invoices, budgets, expense reports, credit card numbers, or bank account information.`
    - **Description for admins:** `Child label of Zava-Confidential. Used to classify financial documents on the Zava Finance SharePoint site. Configured with auto-labelling for credit card numbers, ABA routing numbers, and SWIFT codes.`


1. Select **Next**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image29.png)

1. On the **Define the scope for this label** page, select **Files** and **Emails**. Ensure **Meetings** is deselected. Select **Next**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image30.png)

1. On the **Choose protection settings for labeled items** page, select **Apply content marking**. Select **Next**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image31.png)

1. On the **Content marking** page, set the **Content marking** toggle to **On**.

1. Select the checkbox for **Add a footer**. Select the edit icon next to **Add a footer**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image32.png)

1. In the **Footer text** field, enter `Restricted — Zava Finance use only`. Select **Save**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image33.png)

1. Select **Next**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image34.png)

1. On the **Auto-labeling for files and emails** page, set the **Auto-labeling for files and emails** toggle to **On**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image35.png)

1. Under **Detect content that matches these conditions**, select **+ Add condition**. Select **Content contains**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image36.png)

1. In the **Content contains** section, select **Add**. Select **Sensitive info types**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image37.png)

1. On the **Sensitive info types** flyout panel, search for and select the following sensitive info types:

    - `Credit Card Number`
    - `ABA Routing Number`
    - `SWIFT Code`


1. Select **Add** to confirm the selection.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image38.png)

1. Select **Next**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image39.png)

1. On the **Define protection settings for groups and sites** page, select **Next**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image40.png)

1. On the **Review your settings and finish** page, select **Create label**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image41.png)

1. On the **Your sensitivity label was created** page, select **Automatically apply label to sensitive content**. Select **Done**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image42.png)

1. On the **Create auto-labeling policy** flyout page, select **Review policy**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image43.png)

    


### Task 4: Configure and Save the Financial Data Auto-Labelling Policy

1. On the **Name your auto-labeling policy** page, confirm the default name reflects the Financial-Data label, then select **Next**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image44.png)

1. On the **Choose a label to auto-apply** page, confirm that **Zava-Confidential/Financial-Data** is selected, then select **Next**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image45.png)

1. On the **Assign admin units** page, select **Next**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image46.png)

1. On the **Choose locations where you want to apply the label** page, select the following locations:

    - **Exchange email**
    - **SharePoint sites**
    - **OneDrive accounts**


1. Select **Next**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image47.png)

1. On the **Set up common or advanced rules** page, leave **Common rules** selected, then select **Next**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image48.png)

1. On the **Define rules for content in all locations** page, expand the **Financial-Data rule** to confirm that Credit Card Number, ABA Routing Number, and SWIFT Code are listed as conditions.

1. Select **Next**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image49.png)

1. On the **Additional label settings** page, select **Next**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image50.png)

1. On the **Decide if you want to test out the policy now or later** page, select **Run policy in simulation mode**.

1. Select the checkbox for **Automatically turn on policy if not modified after 7 days in simulation**.

1. Select **Next**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image51.png)

1. On the **Review and finish** page, select **Create policy**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image52.png)

1. On the **Your auto-labeling policy was created** page, select **Done**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image53.png)

    >[!Note] The auto-labeling policy will scan existing content in SharePoint, OneDrive, and Exchange in simulation mode. The `Zava_Expense_Report_Alex.xlsx`, `Zava_Payroll_Q1_2025.xlsx`, and `Zava_Invoice_Log.xlsx` files uploaded in Lab 00 contain credit card numbers and IBAN values and will be matched by this policy. After 7 days in simulation without modification, the policy will turn on automatically and begin applying the Financial-Data label to matched files.

    


## Exercise 3: Publish Sensitivity Labels to Zava Users

### Task 1: Publish the Zava-Confidential Labels

1. On the **Sensitivity labels** page, select **Publish labels**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image54.png)

1. On the **Choose sensitivity labels to publish** page, select **Choose sensitivity labels to publish**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image55.png)

1. On the **Sensitivity labels to publish** flyout panel, select the checkboxes for both of the following labels:

    - **Zava-Confidential/HR-Data**
    - **Zava-Confidential/Financial-Data**


1. Select **Add** at the bottom of the flyout panel.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image56.png)

1. Back on the **Choose sensitivity labels to publish** page, select **Next**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image57.png)

1. Then select **Next** until you reach the naming page.

1. On the **Name your policy** page, enter the following:

    - **Name:** `Zava-Confidential Label Policy`
    - **Description:** `Publishes Zava-Confidential HR-Data and Financial-Data labels to all Zava users for manual and auto-labelling of sensitive content.`


1. Select **Next**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image58.png)

1. On the **Review and finish** page, select **Submit**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image59.png)

1. On the **New policy created** page, select **Done**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image60.png)

    >[!Note] Label policy propagation can take up to 24 hours before the Sensitivity button appears in Office for the web for all users. In this lab, MOD Administrator will apply labels directly via the SharePoint document library sensitivity column in the next exercise, which does not depend on the Office app Sensitivity button.

    


## Exercise 4: Apply the HR-Data Label to Zava HR SharePoint Files

### Task 1: Apply Sensitivity Labels via SharePoint Document Library

1. Open a new browser tab and navigate to `https://[TenantName].sharepoint.com/sites/HR`.

    >[!Note] Replace `[TenantName]` with your tenant prefix from the **Resources** tab.

1. In the top navigation pane, select **HR** > **Benefits@Contoso**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image61.png)

1. Select **Documents** from the left navigation, and locate **Zava_Employee_Records.xlsx**. Select the checkbox to the left of **Zava_Employee_Records.xlsx** to select it.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image62.png)

1. In the toolbar above the document library, select **⋯** (More options) or the **Details** pane icon.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image63.png)

1. On the **Details** pane or right-click context menu, select the **Sensitivity** field and then select **Zava-Confidential/HR-Data** from the dropdown list.

1. Repeat steps 3 to 5 for the following files:

    - `Zava_Payroll_Q1_2025.xlsx`
    - `Zava_Sick_Leave_Report.xlsx`
    - `Zava_Termination_Checklist.docx`


1. Confirm that all four files show **Zava-Confidential/HR-Data** in the **Sensitivity** column.

    >[!Note] If the Sensitivity column is not visible in the document library, select **Add column** from the column header row and add the **Sensitivity** column. If the sensitivity label options do not appear yet due to propagation delay, wait 15–30 minutes and retry. Alternatively, open each file in Office for the web, select **Sensitivity** from the ribbon, and apply the label from within the document.

    


## Exercise 5: Create a DLP Policy for Microsoft 365 Copilot

### Task 1: Create the DLP Policy

1. Return to the Microsoft Purview portal at `https://purview.microsoft.com`. In the left navigation pane, select **Solutions**. Select **Data Loss Prevention**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image65.png)

1. In the left sub-navigation, select **Policies**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image66.png)

1. On the **Policies** page, select **+ Create policy**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image67.png)

1. For the **What info do you want to protect?** pane, select **Enterprise applications and devices**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image68.png)

1. On the **Start with a template or create a custom policy** page, select **Custom** under **Categories**. Select **Custom policy** under **Regulations**. Select **Next**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image69.png)

1. On the **Name your DLP policy** page, enter the following:

    - **Name:** `Zava - Block HR Data in M365 Copilot`
    - **Description:** `Prevents Microsoft 365 Copilot and Copilot Chat from processing or surfacing documents labelled as Zava-Confidential HR-Data.`


1. Select **Next**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image70.png)

1. On the **Assign admin units** page, select **Next**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image71.png)

1. On the **Choose locations to apply the policy** page, deselect all locations that are toggled on by default.

1. Locate **Microsoft 365 Copilot and Copilot Chat** in the locations list. Toggle **Microsoft 365 Copilot and Copilot Chat** to **On**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image72.png)

1. Confirm that all other locations remain toggled **Off**.

1. Select **Next**.

    >[!Note] The Microsoft 365 Copilot and Copilot Chat location applies DLP policy controls to interactions in Microsoft 365 Copilot Chat and Copilot-powered experiences. It does not apply to Copilot Studio custom agents accessed directly. The test in Exercise 6 will use M365 Copilot Chat at copilot.microsoft.com, not the Zava HR Assistant directly, to validate enforcement.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image73.png)

1. On the **Define policy settings** page, select **Create or customize advanced DLP rules**. Select **Next**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image74.png)

1. On the **Customize advanced DLP rules** page, select **+ Create rule**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image75.png)

1. On the **Create rule** panel, in the **Name** field, enter `Block Copilot access to HR-labelled content`.

1. In the **Description** field, enter `Blocks Microsoft 365 Copilot from processing files labelled Zava-Confidential/HR-Data.`

1. Under **Conditions**, select **+ Add condition**. Select **Content contains**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image76.png)

1. In the **Content contains** section, select **Add**. Select **Sensitivity labels**.

    <!--![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image77.png)-->

1. On the **Sensitivity labels** flyout panel, search for and select **Zava-Confidential/HR-Data**. Select **Add** to confirm.

    <!--![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image78.png)-->

1. Under **Actions**, select **+ Add an action**. Select **Restrict Copilot from processing contents**.

    <!--![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image79.png)-->

1. Under **Restrict Copilot from processing contents**, select the checkbox next to **Accessing knowledge sources**.

1. Select **Save** to save the rule.

    <!--![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image80.png)-->

1. Confirm that **Block Copilot access to HR-labelled content** appears in the rules list on the **Customize advanced DLP rules** page. Select **Next**.

    <!--![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image81.png)-->

1. On the **Policy mode** page, select **Turn the policy on immediately**. Select **Next**.

    ![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image82.png)-->

1. On the **Review and finish** page, review the policy configuration and select **Submit**.

    <!--![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image83.png)-->

1. On the **New policy created** page, select **Done**.

    <!--![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image84.png)-->

1. On the **Policies** page, confirm that **Zava - Block HR Data in M365 Copilot** appears in the list with a status of **On**.

    <!--![](https://raw.githubusercontent.com/technofocus-pte/scrtyAIcpltagntsdepth/refs/heads/main/Lab%2004/media/image85.png)-->

    >[!Note] DLP policy propagation to the Microsoft 365 Copilot location can take up to four hours. If the test in Exercise 6 does not produce a block immediately, this is expected. Proceed with the test, note the response, and if enforcement is not yet active, return to this test after completing Lab 05 or at the end of the day. The audit log event will confirm when enforcement first triggers.

    


## Exercise 6: Test DLP Enforcement via Microsoft 365 Copilot Chat

### Task 1: Attempt to Access HR-Labelled Content as Adele Vance

1. Open a new **InPrivate** or **Incognito** browser window.

1. Navigate to +++https://copilot.microsoft.com+++.

1. Sign in with **Adele Vance** credentials from the **Resources** tab.

1. In the Microsoft 365 Copilot Chat input field, enter the following prompt:

    `Summarise the contents of Zava_Employee_Records.xlsx`

1. Wait for the response.

1. Review the response carefully:

    - **If DLP is enforced:** Copilot will return a response indicating that it cannot access or share the content due to a data protection policy. A policy tip may be visible.
    - **If DLP propagation is still in progress:** Copilot may return a partial summary or a reference link to the file. Note the response and return to this step after completing Lab 05.


1. Enter a second prompt:

    `What employee salary information is in the HR SharePoint site?`

1. Review the response and note whether Copilot restricts or surfaces the content.

1. Close the InPrivate browser window.

    


## Exercise 7: Investigate the DLP Match Event in Purview Audit

### Task 1: Search for DLP Match Events as Patti Fernandes

1. Open a new **InPrivate** or **Incognito** browser window.

1. Navigate to +++https://purview.microsoft.com+++.

1. Sign in with **Patti Fernandes** credentials from the **Resources** tab.

1. In the left navigation pane, select **Solutions**.

1. Select **Audit**.

1. On the **Audit** page, select the **New Search** tab.

1. Configure the search with the following values:

    - **Start date:** Select today's date minus 1 day.
    - **End date:** Select today's date.
    - **Activities – friendly names:** Enter `DLP` and select **DLP rule matched** from the dropdown.
    - **Users:** Leave blank.


1. Select **Search**.

1. Wait for the search job to complete.

1. Review the results for any entries associated with **Adele Vance** and the **Zava - Block HR Data in M365 Copilot** policy.

1. If a match is found, select the entry to open the audit record detail panel.

1. On the detail panel, note the following fields:

    - **Date**
    - **User**
    - **Activity**
    - **Policy name**
    - **Rule name**
    - **Sensitivity label**
    - **Location**


1. Close the detail panel.

1. Close the InPrivate browser window.

    >[!Note] If no DLP match events appear in the audit log yet, this indicates that either the DLP policy has not yet propagated fully or that the test interaction in Exercise 6 did not trigger enforcement. DLP audit events for the Copilot location can take up to one hour to appear in the audit log after enforcement occurs. Return to this search after completing Lab 05 if results are not yet available.

    


## Summary

In this lab, you enabled sensitivity label co-authoring support in Microsoft Purview, activating label awareness for SharePoint and OneDrive files. You created the **Zava-Confidential** label group and two child labels — **HR-Data** and **Financial-Data** — with content markings to identify classified documents. You configured an auto-labelling policy for Financial-Data that detects credit card numbers, ABA routing numbers, and SWIFT codes in SharePoint, OneDrive, and Exchange, running in simulation mode with automatic enforcement after seven days. You published both labels to all Zava users via the **Zava-Confidential Label Policy**, and manually applied the **HR-Data** label to four sensitive HR documents in the Zava HR SharePoint site. You created the **Zava - Block HR Data in M365 Copilot** DLP policy targeting the Microsoft 365 Copilot and Copilot Chat location, blocking access to any content carrying the HR-Data label. Adele Vance tested whether Microsoft 365 Copilot could surface labelled HR content, and Patti Fernandes searched the Purview Audit log for the resulting DLP match event. Zava's sensitive data is now classified, and Microsoft 365 Copilot is governed by policy-based access controls.
