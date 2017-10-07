---
title: "csoportkezelés az Azure Active Directory aaaPowerShell példák |} Microsoft Docs"
description: "Ezen a lapon a PowerShell-példák toohelp biztosít az Azure Active Directoryban csoportok kezelése"
keywords: "Az Azure Active Directory, Azure Active Directory PowerShell, csoportok, csoportok kezelése"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 7a5023dc-2727-4c25-8254-b531fc3244ac
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: curtand
ms.reviewer: rodejo
ms.openlocfilehash: ba049babc436e99a290f20899b3a87bcfa811d9e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-version-2-cmdlets-for-group-management"></a>Csoportok kezelése az Azure Active Directory 2-es parancsmagok
> [!div class="op_single_selector"]
> * [Azure Portal](active-directory-groups-create-azure-portal.md)
> * [klasszikus Azure portál](active-directory-accessmanagement-manage-groups.md)
> * [PowerShell](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)
>
>

Ebben a cikkben található példák bemutatják, hogyan toouse PowerShell toomanage a csoportok az Azure Active Directory (Azure AD).  Azt is megtudhatja, hogyan tooget állítsa be a hello Azure AD PowerShell modult. Először meg kell [hello Azure AD PowerShell-modul letöltése](https://www.powershellgallery.com/packages/AzureAD/).

## <a name="installing-hello-azure-ad-powershell-module"></a>Hello Azure AD PowerShell-modul telepítése
tooinstall hello Azure AD PowerShell modult, a következő parancsok hello használata:

    PS C:\Windows\system32> install-module azuread

amely modul hello tooverify lett telepítve, a következő parancs hello használata:

    PS C:\Windows\system32> get-module azuread

    ModuleType Version      Name                                ExportedCommands
    ---------- ---------    ----                                ----------------
    Binary     2.0.0.115    azuread                      {Add-AzureADAdministrati...}

Most elindíthatja hello modul hello-parancsmagokkal. A hello Azure AD modulban hello parancsmagok teljes leírása, tekintse meg az toohello online dokumentációját [Azure Active Directory PowerShell 2-es verzió](/powershell/azure/install-adv2?view=azureadps-2.0).

## <a name="connecting-toohello-directory"></a>Csatlakozás toohello könyvtár
Azure AD PowerShell-parancsmagok használatával csoportok kezelése előtt a PowerShell-munkamenet toohello directory kívánt toomanage kell csatlakoztatni. A következő parancs hello használata:

    PS C:\Windows\system32> Connect-AzureAD

hello parancsmag kéri a hello hitelesítő adatok toouse tooaccess szeretné, hogy a címtárban. A jelen példában használjuk karen@drumkit.onmicrosoft.com tooaccess hello bemutató könyvtár. hello parancsmag ad vissza egy megerősítő tooshow hello munkamenet sikeresen csatlakoztatva lett tooyour könyvtár:

    Account                       Environment Tenant
    -------                       ----------- ------
    Karen@drumkit.onmicrosoft.com AzureCloud  85b5ff1e-0402-400c-9e3c-0f…

Most elindíthatja a címtárban hello AzureAD parancsmagok toomanage csoportok használatával.

## <a name="retrieving-groups"></a>Csoportok beolvasása
meglévő csoportok tooretrieve a könyvtárból, használhatja a Get-AzureADGroups parancsmag hello. minden tooretrieve csoportok hello könyvtárban, paraméterek nélkül használja a hello parancsmagot:

    PS C:\Windows\system32> get-azureadgroup

hello parancsmag az összes csoport hello csatlakoztatott címtárhoz adja vissza.

Hello - objectID paraméter tooretrieve is használhat, amelynek hello csoport objectID megadott adott csoporthoz:

    PS C:\Windows\system32> get-azureadgroup -ObjectId e29bae11-4ac0-450c-bc37-6dae8f3da61b

hello parancsmag most hello csoport, amelynek objectID megegyezik-e hello hello paraméter megadott adja vissza:

    DeletionTimeStamp            :
    ObjectId                     : e29bae11-4ac0-450c-bc37-6dae8f3da61b
    ObjectType                   : Group
    Description                  :
    DirSyncEnabled               :
    DisplayName                  : Pacific NW Support
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 9bb4139b-60a1-434a-8c0d-7c1f8eee2df9
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

Kereshet egy adott csoport használatával hello - szűrő paraméter. Ez a paraméter egy ODATA szűrőfeltétel vesz igénybe, és adja vissza az összes csoport szűrőnek hello, mint például a következő hello:

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

> [!NOTE] 
> hello AzureAD PowerShell-parancsmagok hello szabványos OData-lekérdezés végrehajtása. További információkért lásd: **$filter** a [OData rendszer lekérdezési lehetőségek hello OData-végpont használatával](https://msdn.microsoft.com/library/gg309461.aspx#BKMK_filter).

## <a name="creating-groups"></a>Csoportok létrehozása
a címtárban, használja a New-AzureADGroup hello parancsmagot az új csoport toocreate. Ez a parancsmag "Marketing" nevű új biztonsági csoportot hoz létre:

    PS C:\Windows\system32> New-AzureADGroup -Description "Marketing" -DisplayName "Marketing" -MailEnabled $false -SecurityEnabled $true -MailNickName "Marketing"

## <a name="updating-groups"></a>Csoportok frissítése
tooupdate egy meglévő csoportot, használja a Set-AzureADGroup hello parancsmagot. Ebben a példában a Microsoft épp módosított hello DisplayName tulajdonságát hello csoport "Intune-rendszergazdák." Először igazolnia fogjuk megtalálni hello csoport hello Get-AzureADGroup parancsmag és a szűrő hello DisplayName attribútum használatával:

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

A következő azt épp módosított hello leírás toohello új tulajdonságérték "Intune eszköz-rendszergazdák":

    PS C:\Windows\system32> Set-AzureADGroup -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -Description "Intune Device Administrators"

Most ismét találtunk hello csoport, ha látható hello Description tulajdonságot frissített tooreflect van hello új érték:

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Device Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

## <a name="deleting-groups"></a>Csoportok törlése
a címtárban lévő toodelete csoportok parancsmaggal hello Remove-AzureADGroup az alábbiak szerint:

    PS C:\Windows\system32> Remove-AzureADGroup -ObjectId b11ca53e-07cc-455d-9a89-1fe3ab24566b

## <a name="managing-members-of-groups"></a>A csoportok tagjai kezelése
Ha tooadd új tagok tooa csoport van szüksége, használja az Add-AzureADGroupMember hello parancsmagot. Ez a parancs az előző példában hello használtuk tag toohello az Intune-rendszergazdák csoport hozzáadása:

    PS C:\Windows\system32> Add-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -RefObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

hello - ObjectId paraméter hello ObjectID a hello csoport toowhich szeretnénk tooadd tagja, és hello - RefObjectId hello ObjectID hello felhasználó szeretnénk tooadd tag toohello csoportként.

tooget hello meglévő egy csoport tagjai, parancsmaggal hello Get-AzureADGroupMember, ebben a példában látható módon:

    PS C:\Windows\system32> Get-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df

    DeletionTimeStamp ObjectId                             ObjectType
    ----------------- --------                             ----------
                          72cd4bbd-2594-40a2-935c-016f3cfeeeea User
                          8120cc36-64b4-4080-a9e8-23aa98e8b34f User

tooremove hello tag korábban hozzáadott toohello csoport, használja a Remove-AzureADGroupMember hello parancsmagot, ahogy az itt látható:

    PS C:\Windows\system32> Remove-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -MemberId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

tooverify hello csoport membership(s) egy olyan felhasználó hello válassza-AzureADGroupIdsUserIsMemberOf parancsmag használható. Ez a parancsmag paramétereit hello hello felhasználó mely toocheck hello csoporttagságok ObjectId tesz és mely toocheck hello tagságát csoportok listája. csoportok listájának hello megadott formában hello "Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck" típusú összetett változó, úgy azt először hozzon létre egy változót ezzel a típussal kell lennie:

    PS C:\Windows\system32> $g = new-object Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck

A következő általunk értékeket a hello csoportazonosítókat toocheck hello attribútum "Csoportazonosítókat" a komplex változó:

    PS C:\Windows\system32> $g.GroupIds = "b11ca53e-07cc-455d-9a89-1fe3ab24566b", "31f1ff6c-d48c-4f8a-b2e1-abca7fd399df"

Mostantól Ha azt szeretné, hogy toocheck hello csoporttagságait elleni $g hello csoportok ObjectID 72cd4bbd-2594-40a2-935c-016f3cfeeeea rendelkező felhasználó, érhetjük el:

    PS C:\Windows\system32> Select-AzureADGroupIdsUserIsMemberOf -ObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea -GroupIdsForMembershipCheck $g

    OdataMetadata                                                                                                 Value
    -------------                                                                                                  -----
    https://graph.windows.net/85b5ff1e-0402-400c-9e3c-0f9e965325d1/$metadata#Collection(Edm.String)             {31f1ff6c-d48c-4f8a-b2e1-abca7fd399df}


hello visszaadott érték, amelynek tagja a felhasználói csoportok listája. A metódus toocheck névjegyek, csoportok vagy Szolgáltatásnevekről tagsági csoportok használatával válassza AzureADGroupIdsContactIsMemberOf, válasszon-AzureADGroupIdsGroupIsMemberOf adott listáját is alkalmazhat vagy SELECT-AzureADGroupIdsServicePrincipalIsMemberOf

## <a name="managing-owners-of-groups"></a>Csoport tulajdonosainak kezelése
tooadd tulajdonosok tooa csoport, használja a hello Add-AzureADGroupOwner parancsmagot:

    PS C:\Windows\system32> Add-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -RefObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

hello - ObjectId paraméter hello ObjectID a hello csoport toowhich szeretnénk tooadd egy olyan tulajdonost, és hello - RefObjectId hello ObjectID hello felhasználó hello csoport tulajdonos szeretnénk tooadd.

egy csoport tulajdonosainak tooretrieve hello hello Get-AzureADGroupOwner parancsmagokat használhatja:

    PS C:\Windows\system32> Get-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df

hello parancsmag hello megadott csoport tulajdonosainak hello listáját adja vissza:

    DeletionTimeStamp ObjectId                             ObjectType
    ----------------- --------                             ----------
                          e831b3fd-77c9-49c7-9fca-de43e109ef67 User

Ha azt szeretné, hogy tooremove tulajdonost egy csoportból, használja a hello Remove-AzureADGroupOwner parancsmagot:

    PS C:\Windows\system32> remove-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -OwnerId e831b3fd-77c9-49c7-9fca-de43e109ef67

## <a name="reserved-aliases"></a>Fenntartott aliasok 
Egy csoport jön létre, amikor bizonyos végpontokkal hello végfelhasználói toospecify egy mailNickname vagy alias toobe hello címre küldi hello csoport részeként. A következő magas szintű jogosultsággal rendelkező e-mail-aliasok hello csoportok csak az Azure AD globális rendszergazda hozhatók létre. 
  
* visszaélés 
* Rendszergazda 
* Rendszergazda 
* hostmaster 
* majordomo 
* postamester 
* legfelső szintű 
* Biztonságos 
* Biztonsági 
* SSL-rendszergazda 
* gazdáját 

## <a name="next-steps"></a>Következő lépések
További Azure Active Directory PowerShell dokumentációját a található [Azure Active Directory-modul parancsmagokkal](/powershell/azure/install-adv2?view=azureadps-2.0).

* [Hozzáférés tooresources kezelése az Azure Active Directoryval](active-directory-manage-groups.md)
* [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md)
