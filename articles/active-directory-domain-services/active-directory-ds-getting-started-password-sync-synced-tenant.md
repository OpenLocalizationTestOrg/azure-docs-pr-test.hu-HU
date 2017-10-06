---
title: "Azure AD tartományi szolgáltatások: Jelszavak szinkronizálásának engedélyezése | Microsoft Docs"
description: "Első lépések az Azure Active Directory tartományi szolgáltatások használatával"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 8731f2b2-661c-4f3d-adba-2c9e06344537
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/30/2017
ms.author: maheshu
ms.openlocfilehash: a7a6ee0f83d3d9bdaf236717efb39155a26934e5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-password-synchronization-tooazure-active-directory-domain-services"></a>Jelszó-szinkronizálás tooAzure Active Directory tartományi szolgáltatások engedélyezése
Az előző feladatokban engedélyezte az Active Directory Domain Servicest az Azure Active Directory (Azure AD) bérlő számára. hello következő feladata NT LAN Manager-(NTLM-) és a Kerberos-hitelesítés tooAzure AD tartományi szolgáltatások a hitelesítő kivonatok tooenable szinkronizálás szükséges. Miután beállította a hitelesítő adatok szinkronizálása, felhasználók bejelentkezhetnek toohello által kezelt tartomány a vállalati hitelesítő adataikkal.

hello lépései eltérőek csak felhőalapú felhasználói fiókok és felhasználói fiókok, amelyek alapján az Azure AD Connect használatával a helyszíni címtárral szinkronizálja. Ha az Azure AD-bérlő rendelkezik a felhő csak a felhasználók és a felhasználó számára a helyszíni Active Directory, tooperform két lépést kell.

<br>

> [!div class="op_single_selector"]
> * **Csak felhőalapú felhasználói fiókokhoz**: [szinkronizálja a jelszavakat, csak felhőalapú felhasználói fiókok tooyour felügyelt tartomány számára](active-directory-ds-getting-started-password-sync.md)
> * **A helyszíni felhasználói fiókok**: [szinkronizálja a jelszavakat a felhasználói fiókok szinkronizálása a helyszíni AD tooyour felügyelt tartományhoz](active-directory-ds-getting-started-password-sync-synced-tenant.md)
>
>

<br>

## <a name="task-5-enable-password-synchronization-tooyour-managed-domain-for-user-accounts-synced-with-your-on-premises-ad"></a>5. feladat: jelszó szinkronizálási tooyour kezelt tartományi felhasználói fiókok szinkronizálása megtörtént-e a helyszíni engedélyezése AD
A szinkronizált Azure AD-bérlő toosynchronize van beállítva a szervezete helyszíni címtár Azure AD Connect használatával. Alapértelmezés szerint az Azure AD Connect nem szinkronizálja az NTLM és Kerberos hitelesítő kivonatok tooAzure AD. toouse Azure AD tartományi szolgáltatásokat, tooconfigure az Azure AD Connect toosynchronize szükséges hitelesítő kivonatokat NTLM és Kerberos hitelesítéshez szükséges. a lépéseket követve hello hello szükséges hitelesítő kivonatokat a helyszíni címtár tooyour Azure AD-bérlővel való szinkronizálásának engedélyezése.

> [!NOTE]
> Ha a szervezete helyszíni címtárat, a futnia kell a szinkronizált felhasználói fiókok a NTLM és Kerberos-kivonatok rendelés toouse hello felügyelt tartomány szinkronizálásának engedélyezése. A szinkronizált felhasználói fiók pedig egy fiókot, amelyet a helyszíni címtár jött létre, és szinkronizálva az Azure AD Connect használatával tooyour Azure AD-bérlő.
>
>

### <a name="install-or-update-azure-ad-connect"></a>Azure AD Connect telepítése vagy frissítése
Telepítse a legújabb ajánlott hello kiadásban az Azure AD Connect olyan tartományhoz csatlakoztatott számítógép. Ha az Azure AD Connect a telepítő egy meglévő példányát, tooupdate kell azt az Azure AD Connect toouse hello letöltéséhez. ismert problémák/hibák, lehetséges, hogy már rögzített, tooavoid biztosítja, hogy mindig hello Azure AD Connect legújabb verzióját.

**[Az Azure AD Connect letöltése](http://www.microsoft.com/download/details.aspx?id=47594)**

Ajánlott verzió: **1.1.553.0** – közzététel dátuma: 2017. június 27.

> [!WARNING]
> Telepítenie kell a hello legújabb ajánlott az Azure AD Connect tooenable hello örökölt jelszó (NTLM és Kerberos hitelesítéshez szükséges) hitelesítő toosynchronize tooyour az Azure AD bérlő kiadásában. Ez a funkció nem érhető el a korábbi kiadásokban az Azure AD Connect vagy hello örökölt DirSync eszközzel.
>
>

Az Azure AD Connect telepítési útmutatás a hello következő érhető el – a cikk [Ismerkedés az Azure AD Connect](../active-directory/active-directory-aadconnect.md)

### <a name="enable-synchronization-of-ntlm-and-kerberos-credential-hashes-tooazure-ad"></a>Az NTLM és Kerberos hitelesítő kivonatok tooAzure AD szinkronizálásának engedélyezése
Hajtsa végre a következő PowerShell-parancsfájlt minden AD-erdőben, tooforce teljes jelszó-szinkronizálás, a hello, és lehetővé teszi minden helyi felhasználói hitelesítő kivonatok toosync tooyour az Azure AD bérlői. Ez a parancsfájl lehetővé teszi, hogy az NTLM/Kerberos hitelesítési toobe szinkronizált tooyour az Azure AD-bérlő szükséges hello hitelesítő kivonatokat.

```
$adConnector = "<CASE SENSITIVE AD CONNECTOR NAME>"  
$azureadConnector = "<CASE SENSITIVE AZURE AD CONNECTOR NAME>"  
Import-Module adsync  
$c = Get-ADSyncConnector -Name $adConnector  
$p = New-Object Microsoft.IdentityManagement.PowerShell.ObjectModel.ConfigurationParameter "Microsoft.Synchronize.ForceFullPasswordSync", String, ConnectorGlobal, $null, $null, $null
$p.Value = 1  
$c.GlobalParameters.Remove($p.Name)  
$c.GlobalParameters.Add($p)  
$c = Add-ADSyncConnector -Connector $c  
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $azureadConnector -Enable $false   
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $azureadConnector -Enable $true  
```

A címtár hello méretétől függően (felhasználók, száma, csoportok stb), a hitelesítő adatok szinkronizálását csak tooAzure AD időt vesz igénybe. hello jelszavak használhatóvá válnak az Azure AD tartományi szolgáltatások által kezelt tartomány hello hello hitelesítő kivonatokat szinkronizálta tooAzure AD követően rövid időn belül.

<br>

## <a name="related-content"></a>Kapcsolódó tartalom
* [Engedélyezze a jelszó-szinkronizálás tooAAD tartományi szolgáltatásokra kizárólag felhőalapú Azure AD-címtár](active-directory-ds-getting-started-password-sync.md)
* [Azure AD tartományi szolgáltatások által kezelt tartomány felügyelete](active-directory-ds-admin-guide-administer-domain.md)
* [Csatlakozás egy Windows virtuális gép tooan Azure AD tartományi szolgáltatások által felügyelt tartományokhoz](active-directory-ds-admin-guide-join-windows-vm.md)
* [Csatlakozás a Red Hat Enterprise Linux virtuális gép tooan Azure AD tartományi szolgáltatások által felügyelt tartományokhoz](active-directory-ds-admin-guide-join-rhel-linux-vm.md)
