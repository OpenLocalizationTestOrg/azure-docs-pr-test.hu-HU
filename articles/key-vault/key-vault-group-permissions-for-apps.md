---
title: "Számos alkalmazás egy az Azure key vault elérésére engedélyt |} Microsoft Docs"
description: "Megtudhatja, hogyan számos alkalmazás kulcstároló elérésére engedélyt szeretne megadni"
services: key-vault
documentationcenter: 
author: amitbapat
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 785d4e40-fb7b-485a-8cbc-d9c8c87708e6
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/01/2016
ms.author: ambapat
ms.openlocfilehash: f58b633de2e4b5702ff2df9b3722662b09510200
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="grant-permission-to-many-applications-to-access-a-key-vault"></a><span data-ttu-id="b9027-103">Számos alkalmazás kulcstároló elérésére engedélyt</span><span class="sxs-lookup"><span data-stu-id="b9027-103">Grant permission to many applications to access a key vault</span></span>

## <a name="q-i-have-several-over-16-applications-that-need-to-access-a-key-vault-since-key-vault-only-allows-16-access-control-entries-how-can-i-achieve-that"></a><span data-ttu-id="b9027-104">K: van több kulcstároló eléréséhez szükséges (több mint 16) alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="b9027-104">Q: I have several (over 16) applications that need to access a key vault.</span></span> <span data-ttu-id="b9027-105">Mivel Key Vault csak lehetővé teszi, hogy a hozzáférés-vezérlési bejegyzés 16, hogyan szeretnék érhető el, amely?</span><span class="sxs-lookup"><span data-stu-id="b9027-105">Since Key Vault only allows 16 access control entries, how can I achieve that?</span></span>

<span data-ttu-id="b9027-106">Key Vault hozzáférés-vezérlési házirend csak a 16 bejegyzések támogatja.</span><span class="sxs-lookup"><span data-stu-id="b9027-106">Key Vault access control policy only supports 16 entries.</span></span> <span data-ttu-id="b9027-107">Azonban létrehozhat egy Azure Active Directory biztonsági csoport.</span><span class="sxs-lookup"><span data-stu-id="b9027-107">However you can create an Azure Active Directory security group.</span></span> <span data-ttu-id="b9027-108">A társított szolgáltatás rendszerbiztonsági tagok hozzáadása ehhez a biztonsági csoporthoz, és majd a hozzáférési jogot a Key Vault biztonsági csoport.</span><span class="sxs-lookup"><span data-stu-id="b9027-108">Add all the associated service principals to this security group and then grant access to this security group to Key Vault.</span></span>

<span data-ttu-id="b9027-109">Az alábbiakban az Előfeltételek:</span><span class="sxs-lookup"><span data-stu-id="b9027-109">Here are the pre-requisites:</span></span>
* <span data-ttu-id="b9027-110">[Azure Active Directory V2 PowerShell-modul telepítéséhez](https://www.powershellgallery.com/packages/AzureAD/2.0.0.30).</span><span class="sxs-lookup"><span data-stu-id="b9027-110">[Install Azure Active Directory V2 PowerShell module](https://www.powershellgallery.com/packages/AzureAD/2.0.0.30).</span></span>
* <span data-ttu-id="b9027-111">[Telepítse az Azure PowerShellt](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="b9027-111">[Install Azure PowerShell](/powershell/azure/overview).</span></span>
* <span data-ttu-id="b9027-112">A következő parancsokat, a csoportokat az Azure Active Directory-bérlő létrehozása/szerkesztése engedélyekre van szükség.</span><span class="sxs-lookup"><span data-stu-id="b9027-112">To run the following commands, you need permissions to create/edit groups in the Azure Active Directory tenant.</span></span> <span data-ttu-id="b9027-113">Ha nem rendelkezik engedélyekkel, szükség lehet az Azure Active Directory-rendszergazdától.</span><span class="sxs-lookup"><span data-stu-id="b9027-113">If you don't have permissions, you may need to contact your Azure Active Directory administrator.</span></span>

<span data-ttu-id="b9027-114">Most futtassa a következő parancsokat a PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b9027-114">Now run the following commands in PowerShell.</span></span>

```powershell
# Connect to Azure AD 
Connect-AzureAD 
 
# Create Azure Active Directory Security Group 
$aadGroup = New-AzureADGroup -Description "Contoso App Group" -DisplayName "ContosoAppGroup" -MailEnabled 0 -MailNickName none -SecurityEnabled 1 
 
# Find and add your applications (ServicePrincipal ObjectID) as members to this group 
$spn = Get-AzureADServicePrincipal –SearchString "ContosoApp1" 
Add-AzureADGroupMember –ObjectId $aadGroup.ObjectId -RefObjectId $spn.ObjectId 
 
# You can add several members to this group, in this fashion. 
 
# Set the Key Vault ACLs 
Set-AzureRmKeyVaultAccessPolicy –VaultName ContosoVault –ObjectId $aadGroup.ObjectId -PermissionToKeys all –PermissionToSecrets all –PermissionToCertificates all 
 
# Of course you can adjust the permissions as required 
```

<span data-ttu-id="b9027-115">Ha szeretne egy másik engedélycsoportja a szükséges engedélyek az alkalmazások azon csoportját, az ilyen alkalmazások külön Azure Active Directory biztonsági csoport létrehozása.</span><span class="sxs-lookup"><span data-stu-id="b9027-115">If you need to grant a different set of permissions to a group of applications, create a separate Azure Active Directory security group for such applications.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b9027-116">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b9027-116">Next steps</span></span>

<span data-ttu-id="b9027-117">További tudnivalók a [a kulcstartót biztonságos](key-vault-secure-your-key-vault.md).</span><span class="sxs-lookup"><span data-stu-id="b9027-117">Learn more about how to [Secure your key vault](key-vault-secure-your-key-vault.md).</span></span>
