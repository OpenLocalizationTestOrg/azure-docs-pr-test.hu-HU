---
title: "az Azure Active Directory biztonságicsoport-alapú licenckezeléshez aaaPowerShell példák |} Microsoft Docs"
description: "Az Azure Active Directory biztonságicsoport-alapú licencelési PowerShell-forgatókönyvek"
services: active-directory
keywords: "Az Azure AD-licencelés"
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/05/2017
ms.author: curtand
ms.openlocfilehash: 31eeab0a34c35e80849a4cd11f5447a30b7c04be
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="powershell-examples-for-group-based-licensing-in-azure-ad"></a>PowerShell-példák az Azure Active Directory biztonságicsoport-alapú licenckezeléshez

Csoportalapú licenckezelési teljes funkciók is elérhetővé hello [Azure-portálon](https://portal.azure.com), és a PowerShell-támogatás jelenleg korlátozott. Van azonban néhány hasznos feladatok használatával hello meglévő végrehajtható [MSOnline PowerShell-parancsmagok](https://docs.microsoft.com/powershell/msonline/v1/azureactivedirectory). Ez a dokumentum lehetőségekről példákat.

> [!NOTE]
> Parancsmagok futtatása előtt győződjön meg arról, hogy először csatlakoztatja tooyour bérlői hello futtatásával `Connect-MsolService` parancsmag.

## <a name="view-product-licenses-assigned-tooa-group"></a>Licencek hozzárendelt tooa csoport megtekintése
A [Get-MsolGroup](/powershell/module/msonline/get-msolgroup?view=azureadps-1.0) parancsmag használt tooretrieve hello csoportobjektum tehető, és ellenőrizze a hello *licencek* tulajdonság: felsorolja az összes termék licenccel jelenleg toohello csoport.
```
(Get-MsolGroup -ObjectId 99c4216a-56de-42c4-a4ac-e411cd8c7c41).Licenses
| Select SkuPartNumber
```
Kimenet:
```
SkuPartNumber
-------------
ENTERPRISEPREMIUM
EMSPREMIUM
```

> [!NOTE]
> hello adata korlátozott tooproduct (SKU) adatokat. Már nem lehetséges toolist service-csomagokról hello hello licenc le van tiltva.

## <a name="get-all-groups-with-licenses"></a>-Licenccel rendelkező összes csoport beolvasása

Hello a következő parancs futtatásával bármely licenccel rendelkező összes csoport található:
```
Get-MsolGroup | Where {$_.Licenses}
```
További részletek megjelenítése kapcsolatos milyen termékek vannak hozzárendelve:
```
Get-MsolGroup | Where {$_.Licenses} | Select `
    ObjectId, `
    DisplayName, `
    @{Name="Licenses";Expression={$_.Licenses | Select -ExpandProperty SkuPartNumber}}
```

Kimenet:
```
ObjectId                             DisplayName              Licenses
--------                             -----------              --------
7023a314-6148-4d7b-b33f-6c775572879a EMS E5 – Licensed users  EMSPREMIUM
cf41f428-3b45-490b-b69f-a349c8a4c38e PowerBi - Licensed users POWER\_BI\_STANDARD
962f7189-59d9-4a29-983f-556ae56f19a5 O365 E3 - Licensed users ENTERPRISEPACK
c2652d63-9161-439b-b74e-fcd8228a7074 EMSandOffice             {ENTERPRISEPREMIUM,EMSPREMIUM}
```

## <a name="get-statistics-for-groups-with-licenses"></a>Megtekintheti a statisztikákat a licenccel rendelkező csoportok
A licenccel rendelkező csoportok alapszintű statisztikákat is tud jelentéseket. Hello az alábbi példa felsorolja a Microsoft hello teljes felhasználószám, hello hello csoport és a felhasználók, akiknél licencek nem rendelhető hello csoport hello száma már hozzárendelt licenccel rendelkező felhasználók száma.

```
#get all groups with licenses
Get-MsolGroup -All | Where {$_.Licenses}  | Foreach {
    $groupId = $_.ObjectId;
    $groupName = $_.DisplayName;
    $groupLicenses = $_.Licenses | Select -ExpandProperty SkuPartNumber
    $totalCount = 0;
    $licenseAssignedCount = 0;
    $licenseErrorCount = 0;

    Get-MsolGroupMember -All -GroupObjectId $groupId |
    #get full info about each user in hello group
    Get-MsolUser -ObjectId {$_.ObjectId} |
    Foreach {
        $user = $_;
        $totalCount++

        #check if any licenses are assigned via this group
        if($user.Licenses | ? {$_.GroupsAssigningLicense -ieq $groupId })
        {
            $licenseAssignedCount++
        }
        #check if user has any licenses that failed toobe assigned from this group
        if ($user.IndirectLicenseErrors | ? {$_.ReferencedObjectId -ieq $groupId })
        {
            $licenseErrorCount++
        }     
    }

    #aggregate results for this group
    New-Object Object |
                    Add-Member -NotePropertyName GroupName -NotePropertyValue $groupName -PassThru |
                    Add-Member -NotePropertyName GroupId -NotePropertyValue $groupId -PassThru |
                    Add-Member -NotePropertyName GroupLicenses -NotePropertyValue $groupLicenses -PassThru |
                    Add-Member -NotePropertyName TotalUserCount -NotePropertyValue $totalCount -PassThru |
                    Add-Member -NotePropertyName LicensedUserCount -NotePropertyValue $licenseAssignedCount -PassThru |
                    Add-Member -NotePropertyName LicenseErrorCount -NotePropertyValue $licenseErrorCount -PassThru

    } | Format-Table
```


Kimenet:
```
GroupName         GroupId                              GroupLicenses       TotalUserCount LicensedUserCount LicenseErrorCount
---------         -------                              -------------       -------------- ----------------- -----------------
Dynamics Licen... 9160c903-9f91-4597-8f79-22b6c47eafbf AAD_PREMIUM_P2                   0                 0                 0
O365 E5 - base... 055dcca3-fb75-4398-a1b8-f8c6f4c24e65 ENTERPRISEPREMIUM                2                 2                 0
O365 E5 - extr... 6b14a1fe-c3a9-4786-9ee4-3a2bb54dcb8e ENTERPRISEPREMIUM                3                 3                 0
EMS E5 - all s... 7023a314-6148-4d7b-b33f-6c775572879a EMSPREMIUM                       2                 2                 0
PowerBi - Lice... cf41f428-3b45-490b-b69f-a349c8a4c38e POWER_BI_STANDARD                2                 2                 0
O365 E3 - all ... 962f7189-59d9-4a29-983f-556ae56f19a5 ENTERPRISEPACK                   2                 2                 0
O365 E5 - EXO     102fb8f4-bbe7-462b-83ff-2145e7cdd6ed ENTERPRISEPREMIUM                1                 1                 0
Access tooOffi... 11151866-5419-4d93-9141-0603bbf78b42 STANDARDPACK                     4                 3                 1
```

## <a name="get-all-groups-with-license-errors"></a>Az összes csoport első licenc hibákkal
Néhány felhasználó, akinek licencek nem rendelhető tartalmazó toofind csoportokat:
```
Get-MsolGroup -HasLicenseErrorsOnly $true
```
Kimenet:
```
ObjectId                             DisplayName             GroupType Description
--------                             -----------             --------- -----------
11151866-5419-4d93-9141-0603bbf78b42 Access tooOffice 365 E1 Security  Users who should have E1 licenses
```
## <a name="get-all-users-with-license-errors-in-a-group"></a>Minden felhasználó licenc hibákkal csoport lekérése

A megadott egy csoportot, amely tartalmazza az egyes kapcsolódó hibák licenceléséhez most listázhatja azokat a hibák által érintett összes felhasználó. Egy jser túl lehet más csoportokhoz, a hibák. Azonban a következő példa azt korlátozza eredmények csak tooerrors vonatkozó toohello csoport ellenőrzésével a **ReferencedObjectId** tulajdonság az egyes **IndirectLicenseError** hello felhasználói bevitelének.

```
#a sample group with errors
$groupId = '11151866-5419-4d93-9141-0603bbf78b42'

#get all user members of hello group
Get-MsolGroupMember -All -GroupObjectId $groupId |
    #get full information about user objects
    Get-MsolUser -ObjectId {$_.ObjectId} |
    #filter out users without license errors and users with licenense errors from other groups
    Where {$_.IndirectLicenseErrors -and $_.IndirectLicenseErrors.ReferencedObjectId -eq $groupId} |
    #display id, name and error detail. Note: we are filtering out license errors from other groups
    Select ObjectId, `
           DisplayName, `
           @{Name="LicenseError";Expression={$_.IndirectLicenseErrors | Where {$_.ReferencedObjectId -eq $groupId} | Select -ExpandProperty Error}}
```

Kimenet:
```
ObjectId                             DisplayName      License Error
--------                             -----------      ------------
6d325baf-22b7-46fa-a2fc-a2500613ca15 Catherine Gibson MutuallyExclusiveViolation
```
## <a name="get-all-users-with-license-errors-in-hello-entire-tenant"></a>Minden felhasználó licenc hibákkal hello teljes bérlő lekérése

egy vagy több csoportok, a következő parancsfájl hello licenc hibák rendelkező felhasználók használható toolist. Ez a parancsfájl több sorban is megjelennek, felhasználónkénti Licenchiba, amely lehetővé teszi egy tooclearly azonosítása minden egyes hibához hello forrását.

> [!NOTE]
> Ez a parancsfájl hello bérlő számára, amely nem feltétlenül optimális nagy bérlők lévő összes felhasználó számára.

```
Get-MsolUser -All | Where {$_.IndirectLicenseErrors } | % {   
    $user = $_;
    $user.IndirectLicenseErrors | % {
            New-Object Object |
                Add-Member -NotePropertyName UserName -NotePropertyValue $user.DisplayName -PassThru |
                Add-Member -NotePropertyName UserId -NotePropertyValue $user.ObjectId -PassThru |
                Add-Member -NotePropertyName GroupId -NotePropertyValue $_.ReferencedObjectId -PassThru |
                Add-Member -NotePropertyName LicenseError -NotePropertyValue $_.Error -PassThru
        }
    }  
```

Kimenet:

```
UserName         UserId                               GroupId                              LicenseError
--------         ------                               -------                              ------------
Anna Bergman     0d0fd16d-872d-4e87-b0fb-83c610db12bc 7946137d-b00d-4336-975e-b1b81b0666d0 MutuallyExclusiveViolation
Catherine Gibson 6d325baf-22b7-46fa-a2fc-a2500613ca15 f2503e79-0edc-4253-8bed-3e158366466b CountViolation
Catherine Gibson 6d325baf-22b7-46fa-a2fc-a2500613ca15 11151866-5419-4d93-9141-0603bbf78b42 MutuallyExclusiveViolation
Drew Fogarty     f2af28fc-db0b-4909-873d-ddd2ab1fd58c 1ebd5028-6092-41d0-9668-129a3c471332 MutuallyExclusiveViolation
```

Itt található, amely csak a csoportok licenc hibás keresésre hello parancsfájl egy másik verziója. Előfordulhat, hogy több optimalizálva toohave várt forgatókönyvek problémás néhány csoporthoz.

```
Get-MsolUser -All | Where {$_.IndirectLicenseErrors } | % {   
    $user = $_;
    $user.IndirectLicenseErrors | % {
            New-Object Object |
                Add-Member -NotePropertyName UserName -NotePropertyValue $user.DisplayName -PassThru |
                Add-Member -NotePropertyName UserId -NotePropertyValue $user.ObjectId -PassThru |
                Add-Member -NotePropertyName GroupId -NotePropertyValue $_.ReferencedObjectId -PassThru |
                Add-Member -NotePropertyName LicenseError -NotePropertyValue $_.Error -PassThru
        }
    }
```

## <a name="check-if-user-license-is-assigned-directly-or-inherited-from-a-group"></a>Ha felhasználói licenc hozzárendelése közvetlenül vagy öröklés útján egy csoporttól származik ellenőrzése

Olyan felhasználói objektum esetén lehetséges toocheck egy adott termék licence van hozzárendelve, a csoportból, vagy ha közvetlenül van hozzárendelve.

az alábbi hello két minta funkciók lehet használni egy adott felhasználó tooanalyze hello címhozzárendelés típusát:
```
#Returns TRUE if hello user has hello license assigned directly
function UserHasLicenseAssignedDirectly
{
    Param([Microsoft.Online.Administration.User]$user, [string]$skuId)

    foreach($license in $user.Licenses)
    {
        #we look for hello specific license SKU in all licenses assigned toohello user
        if ($license.AccountSkuId -ieq $skuId)
        {
            #GroupsAssigningLicense contains a collection of IDs of objects assigning hello license
            #This could be a group object or a user object (contrary toowhat hello name suggests)
            #If hello collection is empty, this means hello license is assigned directly - this is hello case for users who have never been licensed via groups in hello past
            if ($license.GroupsAssigningLicense.Count -eq 0)
            {
                return $true
            }

            #If hello collection contains hello ID of hello user object, this means hello license is assigned directly
            #Note: hello license may also be assigned through one or more groups in addition toobeing assigned directly
            foreach ($assignmentSource in $license.GroupsAssigningLicense)
            {
                if ($assignmentSource -ieq $user.ObjectId)
                {
                    return $true
                }
            }
            return $false
        }
    }
    return $false
}
#Returns TRUE if hello user is inheriting hello license from a group
function UserHasLicenseAssignedFromGroup
{
    Param([Microsoft.Online.Administration.User]$user, [string]$skuId)

    foreach($license in $user.Licenses)
    {
        #we look for hello specific license SKU in all licenses assigned toohello user
        if ($license.AccountSkuId -ieq $skuId)
        {
            #GroupsAssigningLicense contains a collection of IDs of objects assigning hello license
            #This could be a group object or a user object (contrary toowhat hello name suggests)
            foreach ($assignmentSource in $license.GroupsAssigningLicense)
            {
                #If hello collection contains at least one ID not matching hello user ID this means that hello license is inherited from a group.
                #Note: hello license may also be assigned directly in addition toobeing inherited
                if ($assignmentSource -ine $user.ObjectId)
                {
                    return $true
                }
            }
            return $false
        }
    }
    return $false
}
```

Ez a parancsfájl végrehajtása olyan feladatokat a hello bérlői hello SKU-ID használja bemenetként szereplő minden felhasználó – ebben a példában azt is hello licencet *Enterprise Mobility + Security*, amely a bérlői azonosítóvaljelölt*contoso:EMS*:
```
#hello license SKU we are interested in. use Msol-GetAccountSku toosee a list of all identifiers in your tenant
$skuId = "contoso:EMS"

#find all users that have hello SKU license assigned
Get-MsolUser -All | where {$_.isLicensed -eq $true -and $_.Licenses.AccountSKUID -eq $skuId} | select `
    ObjectId, `
    @{Name="SkuId";Expression={$skuId}}, `
    @{Name="AssignedDirectly";Expression={(UserHasLicenseAssignedDirectly $_ $skuId)}}, `
    @{Name="AssignedFromGroup";Expression={(UserHasLicenseAssignedFromGroup $_ $skuId)}}
```

Kimenet:
```
ObjectId                             SkuId       AssignedDirectly AssignedFromGroup
--------                             -----       ---------------- -----------------
157870f6-e050-4b3c-ad5e-0f0a377c8f4d contoso:EMS             True             False
1f3174e2-ee9d-49e9-b917-e8d84650f895 contoso:EMS            False              True
240622ac-b9b8-4d50-94e2-dad19a3bf4b5 contoso:EMS             True              True
```

## <a name="remove-direct-licenses-for-users-with-group-licenses"></a>Távolítsa el a csoport-licenccel rendelkező felhasználók közvetlen licenceinek
hello parancsfájl célja a felhasználók, akik már ugyanaz a licenc hello örökölhet; csoport közvetlen licencek szükségtelen tooremove például részeként egy [tér át, toogroup-alapú licencelési](https://docs.microsoft.com/azure/active-directory/active-directory-licensing-group-migration-azure-portal).
> [!NOTE]
> Fontos toofirst ellenőrizze, hogy hello eltávolított közvetlen licencek toobe további szolgáltatás szolgáltatásokat, mint hello örökölt licencek nem engedélyezi. Ellenkező esetben hello közvetlen licenc eltávolítása előfordulhat, hogy tiltsa le a hozzáférési tooservices és a felhasználók adatait. Jelenleg nincs lehetséges toocheck melyik szolgáltatás be legyen kapcsolva keresztül örökölt licencek vs közvetlen Powershellen keresztül. Hello parancsfájlban azt határozza meg tudjuk szolgáltatások minimális szintű hello csoportokból öröklődtek és ellenőrizzük ezzel szemben.

```
#BEGIN: Helper functions used by hello script

#Returns TRUE if hello user has hello license assigned directly
function UserHasLicenseAssignedDirectly
{
    Param([Microsoft.Online.Administration.User]$user, [string]$skuId)

    $license = GetUserLicense $user $skuId

    if ($license -ne $null)
    {
        #GroupsAssigningLicense contains a collection of IDs of objects assigning hello license
        #This could be a group object or a user object (contrary toowhat hello name suggests)
        #If hello collection is empty, this means hello license is assigned directly - this is hello case for users who have never been licensed via groups in hello past
        if ($license.GroupsAssigningLicense.Count -eq 0)
        {
            return $true
        }

        #If hello collection contains hello ID of hello user object, this means hello license is assigned directly
        #Note: hello license may also be assigned through one or more groups in addition toobeing assigned directly
        foreach ($assignmentSource in $license.GroupsAssigningLicense)
        {
            if ($assignmentSource -ieq $user.ObjectId)
            {
                return $true
            }
        }
        return $false
    }
    return $false
}
#Returns TRUE if hello user is inheriting hello license from a specific group
function UserHasLicenseAssignedFromThisGroup
{
    Param([Microsoft.Online.Administration.User]$user, [string]$skuId, [Guid]$groupId)

    $license = GetUserLicense $user $skuId

    if ($license -ne $null)
    {
        #GroupsAssigningLicense contains a collection of IDs of objects assigning hello license
        #This could be a group object or a user object (contrary toowhat hello name suggests)
        foreach ($assignmentSource in $license.GroupsAssigningLicense)
        {
            #If hello collection contains at least one ID not matching hello user ID this means that hello license is inherited from a group.
            #Note: hello license may also be assigned directly in addition toobeing inherited
            if ($assignmentSource -ieq $groupId)
            {
                return $true
            }
        }
        return $false
    }
    return $false
}

#Returns hello license object corresponding toohello skuId. Returns NULL if not found
function GetUserLicense
{
    Param([Microsoft.Online.Administration.User]$user, [string]$skuId, [Guid]$groupId)
    #we look for hello specific license SKU in all licenses assigned toohello user
    foreach($license in $user.Licenses)
    {
        if ($license.AccountSkuId -ieq $skuId)
        {
            return $license
        }
    }
    return $null
}

#produces a list of disabled service plan names for a set of plans we want tooleave enabled
function GetDisabledPlansForSKU
{
    Param([string]$skuId, [string[]]$enabledPlans)

    $allPlans = Get-MsolAccountSku | where {$_.AccountSkuId -ieq $skuId} | Select -ExpandProperty ServiceStatus | Where {$_.ProvisioningStatus -ieq "PendingActivation"} | Select -ExpandProperty ServicePlan | Select -ExpandProperty ServiceName
    $disabledPlans = $allPlans | Where {$enabledPlans -inotcontains $_}

    return $disabledPlans
}

function GetUnexpectedEnabledPlansForUser
{
    Param([Microsoft.Online.Administration.User]$user, [string]$skuId, [string[]]$expectedDisabledPlans)

    $license = GetUserLicense $user $skuId

    $extraPlans = @();

    if($license -ne $null)
    {
        $userDisabledPlans = $license.ServiceStatus | where {$_.ProvisioningStatus -ieq "Disabled"} | Select -ExpandProperty ServicePlan | Select -ExpandProperty ServiceName

        $extraPlans = $expectedDisabledPlans | where {$userDisabledPlans -notcontains $_}
    }
    return $extraPlans
}
#END: helper functions

#BEGIN: executing hello script
#hello group toobe processed
$groupId = "48ca647b-7e4d-41e5-aa66-40cab1e19101"

#license toobe removed - Office 365 E3
$skuId = "contoso:ENTERPRISEPACK"

#minimum set of service plans we know are inherited from groups - we want toomake sure that there aren't any users who have more services enabled
#which could mean that they may lose access after we remove direct licenses
$servicePlansFromGroups = ("EXCHANGE_S_ENTERPRISE", "SHAREPOINTENTERPRISE", "OFFICESUBSCRIPTION")

$expectedDisabledPlans = GetDisabledPlansForSKU $skuId $servicePlansFromGroups

#process all members in hello group
Get-MsolGroupMember -All -GroupObjectId $groupId |
    #get full info about each user in hello group
    Get-MsolUser -ObjectId {$_.ObjectId} |
    Foreach {
        $user = $_;
        $operationResult = "";

        #check if Direct license exists on hello user
        if (UserHasLicenseAssignedDirectly $user $skuId)
        {
            #check if hello license is assigned from this group, as expected
            if (UserHasLicenseAssignedFromThisGroup $user $skuId $groupId)
            {
                #check if there are any extra plans we didn't expect - we are being extra careful not tooremove unexpected services
                $extraPlans = GetUnexpectedEnabledPlansForUser $user $skuId $expectedDisabledPlans
                if ($extraPlans.Count -gt 0)
                {
                    $operationResult = "User has extra plans that may be lost - license removal was skipped. Extra plans: $extraPlans"
                }
                else
                {
                    #remove hello direct license from user
                    Set-MsolUserLicense -ObjectId $user.ObjectId -RemoveLicenses $skuId
                    $operationResult = "Removed direct license from user."   
                }

            }
            else
            {
                $operationResult = "User does not inherit this license from this group. License removal was skipped."
            }
        }
        else
        {
            $operationResult = "User has no direct license tooremove. Skipping."
        }

        #format output
        New-Object Object |
                    Add-Member -NotePropertyName UserId -NotePropertyValue $user.ObjectId -PassThru |
                    Add-Member -NotePropertyName OperationResult -NotePropertyValue $operationResult -PassThru
    } | Format-Table
#END: executing hello script
```

Kimenet:
```
UserId                               OperationResult                                                                                
------                               ---------------                                                                                
7c7f860f-700a-462a-826c-f50633931362 Removed direct license from user.                                                              
0ddacdd5-0364-477d-9e4b-07eb6cdbc8ea User has extra plans that may be lost - license removal was skipped. Extra plans: SHAREPOINTWAC
aadbe4da-c4b5-4d84-800a-9400f31d7371 User has no direct license tooremove. Skipping.                                                
```

## <a name="next-steps"></a>Következő lépések

hello funkcióval kapcsolatban további csoportok, a licenc-kezelő beállítása toolearn hello következő lásd:

* [Mi az a csoport-alapú licencelése az Azure Active Directoryban?](active-directory-licensing-whatis-azure-portal.md)
* [Az Azure Active Directoryban licencek tooa csoportok átjáróalhálózathoz való hozzárendelése](active-directory-licensing-group-assignment-azure-portal.md)
* [Majd azonosítani és megoldani az Azure Active Directory csoport licenc problémák](active-directory-licensing-group-problem-resolution-azure-portal.md)
* [Hogyan toomigrate személy licenccel rendelkező felhasználók toogroup alapú licencelése az Azure Active Directoryban](active-directory-licensing-group-migration-azure-portal.md)
* [Az Azure Active Directory biztonságicsoport-alapú licencelési további helyzeteket is](active-directory-licensing-group-advanced.md)
