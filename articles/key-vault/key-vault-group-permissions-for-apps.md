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
# <a name="grant-permission-toomany-applications-tooaccess-a-key-vault"></a>Adjon engedélye toomany alkalmazások tooaccess kulcstároló

## <a name="q-i-have-several-over-16-applications-that-need-tooaccess-a-key-vault-since-key-vault-only-allows-16-access-control-entries-how-can-i-achieve-that"></a>K: van több kulcstároló tooaccess (több mint 16) alkalmazásokat. Mivel Key Vault csak lehetővé teszi, hogy a hozzáférés-vezérlési bejegyzés 16, hogyan szeretnék érhető el, amely?

Key Vault hozzáférés-vezérlési házirend csak a 16 bejegyzések támogatja. Azonban létrehozhat egy Azure Active Directory biztonsági csoport. Adja hozzá az összes hello társított szolgáltatás rendszerbiztonsági tagok toothis biztonsági csoport, és adjon hozzáférést toothis biztonsági csoport tooKey tárolóban.

Az alábbiakban hello Előfeltételek:
* [Azure Active Directory V2 PowerShell-modul telepítéséhez](https://www.powershellgallery.com/packages/AzureAD/2.0.0.30).
* [Telepítse az Azure PowerShellt](/powershell/azure/overview).
* toorun hello következő parancsokat, engedélyek toocreate/szerkesztése csoportok hello Azure Active Directory-bérlő van szüksége. Ha nem rendelkezik engedélyekkel, szükség lehet toocontact az Azure Active Directory-rendszergazda.

Most futtassa a következő PowerShell-parancsok hello.

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

Ha toogrant engedélyek tooa az alkalmazások azon csoportját különböző szabálykészleteket van szüksége, az ilyen alkalmazások külön Azure Active Directory biztonsági csoport létrehozása.

## <a name="next-steps"></a>Következő lépések

További tudnivalók túl[a kulcstartót biztonságos](key-vault-secure-your-key-vault.md).
