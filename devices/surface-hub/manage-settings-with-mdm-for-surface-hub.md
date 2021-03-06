---
title: Manage settings with an MDM provider (Surface Hub)
description: Microsoft Surface Hub provides an enterprise management solution to help IT administrators manage policies and business applications on these devices using a mobile device management (MDM) solution.
ms.assetid: 18EB8464-6E22-479D-B0C3-21C4ADD168FE
keywords: mobile device management, MDM, manage policies
ms.prod: w10
ms.mktglfcycl: manage
ms.sitesec: library
ms.pagetype: surfacehub, mobility
author: TrudyHa
localizationpriority: medium
---

# Manage settings with an MDM provider (Surface Hub)

Surface Hub and other Windows 10 devices allow IT administrators to manage settings and policies using a mobile device management (MDM) provider. A built-in management component communicates with the management server, so there is no need to install additional clients on the device. For more information, see [Windows 10 mobile device management](https://msdn.microsoft.com/library/windows/hardware/dn914769.aspx).

Surface Hub has been validated with Microsoft’s first-party MDM providers:
- On-premises MDM with System Center Configuration Manager (beginning in version 1602)
- Hybrid MDM with System Center Configuration Manager and Microsoft Intune
- Microsoft Intune standalone

You can also manage Surface Hubs using any third-party MDM provider that can communicate with Windows 10 using the MDM protocol.

## <a href="" id="enroll-into-mdm"></a>Enroll a Surface Hub into MDM
You can enroll your Surface Hubs using automatic, bulk, or manual enrollment.

> [!NOTE]
> You can join your Surface Hub to Azure Active Directory (Azure AD) to manage admin groups on the device. However, Surface Hub does not currently support automatic enrollment to Microsoft Intune through Azure AD join. If your organization automatically enrolls Azure AD joined devices into Intune, you must disable this policy for Surface Hub before joining the device to Azure AD.

**To disable automatic enrollment for Microsoft Intune**
1. In the [Azure classic portal](https://manage.windowsazure.com/), navigate to the **Active Directory** node and select your directory. 
2. Click the **Applications** tab, then click **Microsoft Intune**.
3. Under **Manage devices for these users**, click **Groups**. 
4. Click **Select Groups**, then select the groups of users you want to automatically enroll into Intune. Do not include accounts that are used to enroll Surface Hubs into Intune. 5. Click the checkmark button, then click **Save**.

### Bulk enrollment
**To configure bulk enrollment**
- Surface Hub supports the [Provisioning CSP](https://msdn.microsoft.com/library/windows/hardware/mt203665.aspx) for bulk enrollment into MDM. For more information, see [Windows 10 bulk enrollment](https://msdn.microsoft.com/library/windows/hardware/mt613115.aspx).<br>
--OR--
- If you have an on-premises System Center Configuration Manager infrastructure, see [How to bulk enroll devices with On-premises Mobile Device Management in System Center Configuration Manager](https://technet.microsoft.com/library/mt627898.aspx).

### Manual enrollment
You can manually enroll with an MDM using the **Settings** app on your Surface Hub. 

**To configure manual enrollment**
1. From your Surface Hub, open **Settings**.
2. Type the device admin credentials when prompted.
3. Select **This device**, and navigate to **Device management**.
4. Under **Device management**, select **+ Device management**.
5. Follow the instructions in the dialog to connect to your MDM provider.

## Manage Surface Hub settings with MDM

You can use MDM to manage some [Surface Hub CSP settings](#supported-surface-hub-csp-settings)<!---, and some [Windows 10 settings](#supported-windows-10-settings)-->. Depending on the MDM provider that you use, you may set these settings using a built-in user interface, or by deploying custom SyncML. Microsoft Intune and System Center Configuration Manager provide built-in experiences to help create policy templates for Surface Hub. Refer to documentation from your MDM provider to learn how to create and deploy SyncML.

### Supported Surface Hub CSP settings

You can configure the Surface Hub settings in the following table using MDM. The table also tells if the setting is supported with Microsoft Intune, System Center Configuration Manager, or SyncML.

For more information, see [SurfaceHub configuration service provider](https://msdn.microsoft.com/library/windows/hardware/mt608323.aspx). 

| Setting              | Node in the SurfaceHub CSP         | Supported with<br>Intune? | Supported with<br>Configuration Manager? | Supported with<br>SyncML? |
| -------------------- | ---------------------------------- | ------------------------- | ---------------------------------------- | ------------------------- |
| Maintenance hours | MaintenanceHoursSimple/Hours/StartTime <br> MaintenanceHoursSimple/Hours/Duration | Yes | Yes | Yes |
| Automatically turn on the screen using motion sensors | InBoxApps/Welcome/AutoWakeScreen | Yes | Yes | Yes |
| Require a pin for wireless projection | InBoxApps/WirelessProjection/PINRequired | Yes | Yes | Yes |
| Enable wireless projection | InBoxApps/WirelessProjection/Enabled | Yes | Yes.<br> Use a custom setting. | Yes |
| Miracast channel to use for wireless projection | InBoxApps/WirelessProjection/Channel | Yes | Yes.<br> Use a custom setting. | Yes |
| Connect to your Operations Management Suite workspace | MOMAgent/WorkspaceID <br> MOMAgent/WorkspaceKey | Yes | Yes.<br> Use a custom setting. | Yes |
| Welcome screen background image | InBoxApps/Welcome/CurrentBackgroundPath | Yes | Yes.<br> Use a custom setting. | Yes |
| Meeting information displayed on the welcome screen | InBoxApps/Welcome/MeetingInfoOption | Yes | Yes.<br> Use a custom setting. | Yes |
| Friendly name for wireless projection | Properties/FriendlyName | Yes. <br> Use a custom policy. | Yes.<br> Use a custom setting. | Yes |
| Device account, including password rotation | DeviceAccount/*`<name_of_policy>`* <br> See [SurfaceHub CSP](https://msdn.microsoft.com/library/windows/hardware/mt608323.aspx). | No | No | Yes |



## Example: Manage Surface Hub settings with Micosoft Intune

You can use Microsoft Intune to manage Surface Hub settings. 

**To create a configuration policy from a template**

You'll use the **Windows 10 Team general configuration policy** as the template.

1. On the [Intune management portal](https://manage.microsoft.com), sign in with your Intune administrator account.
2. On the left-hand navigation menu, click **Policy**.
3. In the Overview page, click **Add Policy**.
4. On **Select a template for the new policy**, expand **Windows**, select **General Configuration (Windows 10 Team and later)**, and then click **Create Policy**.
5. Configure your policy, then click **Save Policy**
6. When prompted, click **Yes** to deploy your new policy to a user or device group. For more information, see [Use groups to manage users and devices in Microsoft Intune](https://docs.microsoft.com/intune/deploy-use/use-groups-to-manage-users-and-devices-with-microsoft-intune).

**To create a custom configuration policy**

You’ll need to create a custom policy to manage settings that are not available in the template. 

1. On the [Intune management portal](https://manage.microsoft.com), sign in with your Intune administrator account.
2. On the left-hand navigation menu, click **Policy**.
3. In the Overview page, click **Add Policy**.
4. On **Select a template for the new policy**, expand **Windows**, select **Custom Configuration (Windows 10 Desktop and Mobile and later)**, and then click **Create Policy**.
5. Type a name and optional description for the policy.
6. Under OMA-URI Settings, click **Add**.
7. Complete the form to create a new setting, and then click **OK**.
8. Repeat Steps 6 and 7 for each setting you want to configure with this policy.
9. Once you're done, click **Save Policy** and deploy it to a user or device group.

## Example: Manage Surface Hub settings with System Center Configuration Manager
System Center Configuration Manager supports managing modern devices that do not require the Configuration Manager client to manage them, including Surface Hub. If you already use System Center Configuration Manager to manage other devices in your organization, you can continue to use the Configuration Manager console as your single location for managing Surface Hubs.

> [!NOTE]
> These instructions are based on the current branch of System Center Configuration Manager.

**To create a configuration item for Surface Hub settings**

1. On the **Assets and Compliance** workspace of the Configuration Manager console, click **Overview** > **Compliance Settings** > **Configuration Items**.
2. On the **Home** tab, in the **Create** group, click **Create Configuration Item**.
3. On the **General** page of the Create Configuration Item Wizard, specify a name and optional description for the configuration item.
4. Under **Specify the type of configuration item that you want to create**, select **Windows 8.1 and Windows 10**.
5. Click **Categories** if you create and assign categories to help you search and filter configuration items in the Configuration Manager console.
6. On the **Supported Platforms** page, select **Windows 10** > **All Windows 10 Team and higher**. Unselect the other Windows platforms. 
7. On the **Device Settings** page, under **Device settings groups**, select **Windows 10 Team**.
8. On the **Windows 10 Team** page, configure the settings you require.
9. You'll need to create custom settings to manage settings that are not available in the Windows 10 Team page. On the **Device Settings** page, select the check box **Configure additional settings that are not in the default setting groups**.
10. On the **Additional Settings** page, click **Add**.
11. On the **Browse Settings** dialog, click **Create Setting**.
12. On the **Create Setting** dialog, under the **General** tab, specify a name and optional description for the custom setting.
13. Under **Setting type**, select **OMA URI**.
14. Complete the form to create a new setting, and then click **OK**.
15. On the **Browse Settings** dialog, under **Available settings**, select the new setting you created, and then click **Select**.
16. On the **Create Rule** dialog, complete the form to specify a rule for the setting, and then click **OK**.
17. Repeat Steps 10 to 16 for each custom setting you want to add to the configuration item.
18. Once you're done, on the **Browse Settings** dialog, click **Close**.
19. Complete the wizard. <br> You can view the new configuration item in the **Configuration Items** node of the **Assets and Compliance** workspace.

For more information, see [Create configuration items for Windows 8.1 and Windows 10 devices managed without the System Center Configuration Manager client](https://docs.microsoft.com/sccm/compliance/deploy-use/create-configuration-items-for-windows-8.1-and-windows-10-devices-managed-without-the-client).

## Related topics

[Manage Microsoft Surface Hub](manage-surface-hub.md)

[Microsoft Surface Hub administrator's guide](surface-hub-administrators-guide.md)

 

 





