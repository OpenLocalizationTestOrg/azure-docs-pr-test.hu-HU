---
title: "aaaGrant engedély toomany alkalmazások tooaccess egy az Azure key vault |} Microsoft Docs"
description: "Ismerje meg, hogyan toogrant engedély toomany alkalmazások tooaccess egy kulcsot tároló"
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
ms.openlocfilehash: 5258149f939856f91b3848fc50399e58e5894f0d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="grant-permission-toomany-applications-tooaccess-a-key-vault"></a><span data-ttu-id="c9e59-103">Adjon engedélye toomany alkalmazások tooaccess kulcstároló</span><span class="sxs-lookup"><span data-stu-id="c9e59-103">Grant permission toomany applications tooaccess a key vault</span></span>

## <a name="q-i-have-several-over-16-applications-that-need-tooaccess-a-key-vault-since-key-vault-only-allows-16-access-control-entries-how-can-i-achieve-that"></a><span data-ttu-id="c9e59-104">K: van több kulcstároló tooaccess (több mint 16) alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="c9e59-104">Q: I have several (over 16) applications that need tooaccess a key vault.</span></span> <span data-ttu-id="c9e59-105">Mivel Key Vault csak lehetővé teszi, hogy a hozzáférés-vezérlési bejegyzés 16, hogyan szeretnék érhető el, amely?</span><span class="sxs-lookup"><span data-stu-id="c9e59-105">Since Key Vault only allows 16 access control entries, how can I achieve that?</span></span>

<span data-ttu-id="c9e59-106">Key Vault hozzáférés-vezérlési házirend csak a 16 bejegyzések támogatja.</span><span class="sxs-lookup"><span data-stu-id="c9e59-106">Key Vault access control policy only supports 16 entries.</span></span> <span data-ttu-id="c9e59-107">Azonban létrehozhat egy Azure Active Directory biztonsági csoport.</span><span class="sxs-lookup"><span data-stu-id="c9e59-107">However you can create an Azure Active Directory security group.</span></span> <span data-ttu-id="c9e59-108">Adja hozzá az összes hello társított szolgáltatás rendszerbiztonsági tagok toothis biztonsági csoport, és adjon hozzáférést toothis biztonsági csoport tooKey tárolóban.</span><span class="sxs-lookup"><span data-stu-id="c9e59-108">Add all hello associated service principals toothis security group and then grant access toothis security group tooKey Vault.</span></span>

<span data-ttu-id="c9e59-109">Az alábbiakban hello Előfeltételek:</span><span class="sxs-lookup"><span data-stu-id="c9e59-109">Here are hello pre-requisites:</span></span>
* <span data-ttu-id="c9e59-110">[Azure Active Directory V2 PowerShell-modul telepítéséhez](https://www.powershellgallery.com/packages/AzureAD/2.0.0.30).</span><span class="sxs-lookup"><span data-stu-id="c9e59-110">[Install Azure Active Directory V2 PowerShell module](https://www.powershellgallery.com/packages/AzureAD/2.0.0.30).</span></span>
* <span data-ttu-id="c9e59-111">[Telepítse az Azure PowerShellt](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="c9e59-111">[Install Azure PowerShell](/powershell/azure/overview).</span></span>
* <span data-ttu-id="c9e59-112">toorun hello következő parancsokat, engedélyek toocreate/szerkesztése csoportok hello Azure Active Directory-bérlő van szüksége.</span><span class="sxs-lookup"><span data-stu-id="c9e59-112">toorun hello following commands, you need permissions toocreate/edit groups in hello Azure Active Directory tenant.</span></span> <span data-ttu-id="c9e59-113">Ha nem rendelkezik engedélyekkel, szükség lehet toocontact az Azure Active Directory-rendszergazda.</span><span class="sxs-lookup"><span data-stu-id="c9e59-113">If you don't have permissions, you may need toocontact your Azure Active Directory administrator.</span></span>

<span data-ttu-id="c9e59-114">Most futtassa a következő PowerShell-parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="c9e59-114">Now run hello following commands in PowerShell.</span></span>

```powershell
# Connect tooAzure AD 
Connect-AzureAD 
 
# Create Azure Active Directory Security Group 
$aadGroup = New-AzureADGroup -Description "Contoso App Group" -DisplayName "ContosoAppGroup" -MailEnabled 0 -MailNickName none -SecurityEnabled 1 
 
# Find and add your applications (ServicePrincipal ObjectID) as members toothis group 
$spn = Get-AzureADServicePrincipal –SearchString "ContosoApp1" 
Add-AzureADGroupMember –ObjectId $aadGroup.ObjectId -RefObjectId $spn.ObjectId 
 
# You can add several members toothis group, in this fashion. 
 
# Set hello Key Vault ACLs 
Set-AzureRmKeyVaultAccessPolicy –VaultName ContosoVault –ObjectId $aadGroup.ObjectId -PermissionToKeys all –PermissionToSecrets all –PermissionToCertificates all 
 
# Of course you can adjust hello permissions as required 
```

<span data-ttu-id="c9e59-115">Ha toogrant engedélyek tooa az alkalmazások azon csoportját különböző szabálykészleteket van szüksége, az ilyen alkalmazások külön Azure Active Directory biztonsági csoport létrehozása.</span><span class="sxs-lookup"><span data-stu-id="c9e59-115">If you need toogrant a different set of permissions tooa group of applications, create a separate Azure Active Directory security group for such applications.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c9e59-116">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c9e59-116">Next steps</span></span>

<span data-ttu-id="c9e59-117">További tudnivalók túl[a kulcstartót biztonságos](key-vault-secure-your-key-vault.md).</span><span class="sxs-lookup"><span data-stu-id="c9e59-117">Learn more about how too[Secure your key vault](key-vault-secure-your-key-vault.md).</span></span>
