---
title: "Az Azure Active Directory tartományi szolgáltatások: A SharePoint felhasználói profil szolgáltatás támogatásának engedélyezése |} Microsoft Docs"
description: "Azure Active Directory tartományi szolgáltatások felügyelt tartományok toosupport profilszinkronizálás SharePoint-kiszolgáló konfigurálása"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 938a5fbc-2dd1-4759-bcce-628a6e19ab9d
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
ms.openlocfilehash: 9de4f810380309e8f6436fc24412701645978f1b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-managed-domain-toosupport-profile-synchronization-for-sharepoint-server"></a>SharePoint-kiszolgáló egy felügyelt tartomány toosupport profil szinkronizálás konfigurálása
A SharePoint Server tartalmaz egy felhasználói profil szolgáltatást, a felhasználói profil szinkronizáláshoz használt. tooset mentése felhasználóiprofil-szolgáltatás hello, megfelelő engedélyeket kap az Active Directory-tartomány toobe kell. További információkért lásd: [Active Directory tartományi szolgáltatások engedélyeket kap a SharePoint Server 2013 profilszinkronizálás](https://technet.microsoft.com/library/hh296982.aspx).

Ez a cikk azt ismerteti, hogyan konfigurálhatja az Azure AD tartományi szolgáltatások felügyelt tartományok toodeploy hello SharePoint Server felhasználóiprofil-szinkronizálás szolgáltatás.

## <a name="hello-aad-dc-service-accounts-group"></a>hello "AAD DC-szolgáltatásfiókok" csoport
A biztonsági csoport neve "**AAD DC szolgáltatásfiókok**" hello "Felhasználók" szervezeti egységet a felügyelt tartomány belül érhető el. Láthatja, hogy ez a csoport hello **Active Directory – felhasználók és számítógépek** MMC beépülő modul felügyelt tartományra.

![Az AAD DC szolgáltatásfiókok biztonsági csoport](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts.png)

A biztonsági csoport tagjai jogosultsággal a következő delegált hello:
- hello "Directory változások replikálása" jogosultság hello legfelső szintű DSE hello a felügyelt tartomány.
- hello hello konfigurációelnevezési környezetében "Directory változások replikálása" jogosultság (cn = konfigurációtároló) hello a felügyelt tartomány.

Ez a biztonsági csoport tagja is hello beépített csoport **Windows 2000 előtti rendszerrel kompatibilis hozzáférés**.

![Az AAD DC szolgáltatásfiókok biztonsági csoport](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts-properties.png)


## <a name="enable-your-managed-domain-toosupport-sharepoint-server-user-profile-sync"></a>A felügyelt tartományra toosupport SharePoint Server felhasználóiprofil-szinkronizálás engedélyezése
A SharePoint felhasználói profil szinkronizálási toohello használt hello szolgáltatásfiók is hozzáadhat **AAD DC szolgáltatásfiókok** csoport. Ennek eredményeképpen a hello szinkronizálási fiók lekérdezi a megfelelő jogosultságokkal tooreplicate módosítások toohello könyvtár. Ez a konfigurációs lépés lehetővé teszi, hogy a SharePoint Server felhasználói profil szinkronizálási toowork megfelelően.

![AAD DC szolgáltatásfiókokat - tagok hozzáadása](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts-add-member.png)

![AAD DC szolgáltatásfiókokat - tagok hozzáadása](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts-add-member2.png)

## <a name="related-content"></a>Kapcsolódó tartalom
* [Technikai útmutató - támogatás Active Directory tartományi szolgáltatások engedélyeit a SharePoint Server 2013 profilszinkronizálás](https://technet.microsoft.com/library/hh296982.aspx)
