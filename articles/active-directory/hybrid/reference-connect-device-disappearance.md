---
title: 'Understanding Azure AD Connect 1.4.xx.x and device disappearance | Microsoft Docs'
description: This document describes an issue that arises with version 1.4.xx.x of Azure AD Connect
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/25/2019
ms.subservice: hybrid
ms.author: billmath
---

# Understanding Azure AD Connect 1.4.xx.x and device disappearance
With version 1.4.xx.x of Azure AD Connect, some customers may see some or all of their Windows devices disappear from Azure AD. This is not a cause for concern, as these device identities are not used by Azure AD during conditional access authorization. This change won't delete any Windows devices that were correctly registered with Azure AD for Hybrid Azure AD Join.

If you see the deletion of device objects in Azure AD exceeding the Export Deletion Threshold, it is advised that the customer allow the deletions to go through. [How To: allow deletes to flow when they exceed the deletion threshold](how-to-connect-sync-feature-prevent-accidental-deletes.md)

## Background
Windows devices registered as Hybrid Azure AD Joined are represented in Azure AD as device objects. These device objects can be used for conditional access. Windows 10 devices are synced to the cloud via Azure AD Connect, down level Windows devices are registered directly using either AD FS or seamless single sign-on.

## Windows 10 devices
Only Windows 10 devices with a specific userCertificate attribute value configured by Hybrid Azure AD Join are supposed to be synced to the cloud by Azure AD Connect. In previous versions of Azure AD Connect this requirement was not rigorously enforced, resulting in unnecessary device objects in Azure AD. Such devices in Azure AD always stayed in the “pending” state because these devices were not intended to be registered with Azure AD. 

This version of Azure AD Connect will only sync Windows 10 devices that are correctly configured to be Hybrid Azure AD Joined. Windows 10 device objects without the Azure AD join specific userCertificate will be removed from Azure AD.

## Down-Level Windows devices
Azure AD Connect should never be syncing [down-level Windows devices](../devices/hybrid-azuread-join-plan.md#windows-down-level-devices). Any devices in Azure AD previously synced incorrectly will now be deleted from Azure AD. If Azure AD Connect is attempting to delete [down-level Windows devices](../devices/hybrid-azuread-join-plan.md#windows-down-level-devices), then the device is not the one that was created by the [Microsoft Workplace Join for non-Windows 10 computers MSI](https://www.microsoft.com/download/details.aspx?id=53554) and it is not able to be consumed by any other Azure AD feature.

Some customers may need to revisit [How To: Plan your hybrid Azure Active Directory join implementation](../devices/hybrid-azuread-join-plan.md) to get their Windows devices registered correctly and ensure that such devices can fully participate in device-based conditional access. 

## Next Steps
- [Azure AD Connect Version history](reference-connect-version-history.md)
