---
title: "az Azure Analysis Services aaaAuthentication és a felhasználói engedélyek |} Microsoft Docs"
description: "További tudnivalók az Azure Analysis Services hitelesítés és a felhasználó engedélyeit."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: dd49fd59eec1f68dfe8a0fe373fa612ac46de4e1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="authentication-and-user-permissions"></a>Hitelesítés és a felhasználói engedélyek
Az Azure Analysis Services az Azure Active Directory (Azure AD) identitás- és felhasználói hitelesítés. Bármely felhasználó létrehozása, kezelése vagy csatlakozás tooan Azure Analysis Services kiszolgáló rendelkeznie kell érvényes felhasználói azonosítót egy [Azure AD-bérlő](../active-directory/active-directory-administer.md) hello az ugyanahhoz az előfizetéshez.

Támogatja az Azure Analysis Services [Azure AD B2B együttműködés](../active-directory/active-directory-b2b-what-is-azure-ad-b2b.md). A B2B a szervezeten kívüli felhasználók más Azure AD-címtár felhasználói Vendég meghívható. A vendégek lehet egy másik Azure AD-bérlő címtár vagy bármilyen érvényes e-mail címet. Egyszer meghívott, és hello felhasználói elfogadás hello meghívó által küldött e-mailek, az Azure-ból hello felhasználói identitás kerül toohello bérlő címtárát. Ezeket az identitásokat majd felveheti toosecurity csoportok vagy egy kiszolgálói rendszergazda vagy az adatbázis-szerepkör tagjai.

![Az Azure Analysis Services hitelesítési architektúra](./media/analysis-services-manage-users/aas-manage-users-arch.png)

## <a name="authentication"></a>Authentication
Minden ügyfél-alkalmazások és eszközök használata egy vagy több hello Analysis Services [klienskódtárak](analysis-services-data-providers.md) (AMO-MSOLAP, ADOMD) tooconnect tooa kiszolgáló. 

A három ügyfélcsoport kódtárakat mind az Azure AD interaktív folyamat és a nem interaktív hitelesítés támogatásához. nem interaktív kétféleképpen hello, Active Directory-jelszó és az Active Directory integrált hitelesítési módszerek használatával AMOMD és MSOLAP-alkalmazásokban használható. A két módszerről soha nem eredményez előugró párbeszédpanelen.

Ügyfél alkalmazások, például az Excel és a Power BI Desktopban, és a például az SSMS és az SSDT telepítése hello hello tárak frissítésekor legújabb verzióját toohello legújabb verzióját. A Power BI Desktop, az SSMS és az SSDT havi frissítése. Excel [és az Office 365 frissítése](https://support.office.com/en-us/article/When-do-I-get-the-newest-features-in-Office-2016-for-Office-365-da36192c-58b9-4bc9-8d51-bb6eed468516). Az Office 365 frissítéseit ritkábban, és egyes szervezetek hello késleltetett csatorna használatára, jelentése frissítések elhalasztott toothree hónap fel.

 Hello ügyfélalkalmazás vagy az eszköz, amellyel függően eltérő lehet a hello típusú hitelesítés, és hogyan jelentkezzen be. Minden alkalmazás támogathatja a különböző szolgáltatások toocloud szolgáltatások, például az Azure Analysis Services csatlakozni.


### <a name="sql-server-management-studio-ssms"></a>SQL Server Management Studio (SSMS)
Az Azure Analysis Services-kiszolgálók támogatják az érkező kapcsolatokat [SSMS V17.1](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) és újabb Windows-hitelesítést, Active Directory jelszavas hitelesítést és az Active Directory univerzális hitelesítés használatával. Általában ajánlott Active Directory univerzális hitelesítést használni, mert:

*  Interaktív és a nem interaktív hitelesítési módszert támogat.

*  Azure B2B vendégfelhasználók hello Azure AS bérlői a meghívott támogatja. Tooa kiszolgálóhoz kapcsolódáskor a vendégfelhasználók kell választania az Active Directory univerzális hitelesítése, toohello kiszolgálóhoz kapcsolódáskor.

*  Támogatja a többtényezős hitelesítés (MFA). Az Azure MFA segít a biztonságos működés érdekében hozzáférés toodata és alkalmazások ellenőrzési lehetőségek közül: telefonhívás, szöveges üzenetet, intelligens kártyák PIN-kód és az értesítést a mobilalkalmazásban. Az Azure ad-val interaktív MFA érvényesítéshez előugró párbeszédpanelen eredményezhet.

### <a name="sql-server-data-tools-ssdt"></a>SQL Server Data Tools (SSDT)
Az SSDT tooAzure Analysis Services hitelesítéssel Active Directory univerzális MFA-támogatással rendelkező kapcsolatot. Felhasználók az első telepítéskor hello tooAzure rákérdezéses toosign közé tartozik a szervezeti azonosítójával (e-mail). Felhasználók tooAzure azok üzembe hello kiszolgálón kiszolgáló-rendszergazdai jogosultságokkal rendelkező fiókkal kell bejelentkeznie. Amikor első alkalommal jelentkezik be tooAzure hello, egy token van hozzárendelve. Az SSDT gyorsítótárazza a hello token memórián belüli a jövőbeli újrakapcsolódások száma.

### <a name="power-bi-desktop"></a>A Power BI Desktop
A Power BI Desktop tooAzure Analysis Services-hitelesítéssel Active Directory univerzális MFA-támogatással rendelkező kapcsolatot. Felhasználók tooAzure hello első csatlakozáskor a kért toosign közé tartozik a szervezeti azonosítójával (e-mail). Felhasználók egy olyan fiókkal, amely megtalálható a rendszergazda vagy az adatbázis-szerepkör tooAzure be kell jelentkeznie.

### <a name="excel"></a>Excel
Excel felhasználók kapcsolódhatnak tooa kiszolgáló a Windows-fiókot, egy szervezet azonosítója (e-mail címet) vagy egy külső e-mail címet. Az Azure AD hello vendégfelhasználóként egy külső e-mail identitások léteznie kell.

## <a name="user-permissions"></a>Felhasználói engedélyek

**Kiszolgáló-Rendszergazdák** adott tooan Azure Analysis Services szolgáltatások kiszolgálópéldánya vannak. Csatlakozáshoz például az Azure portál, az SSMS és az SSDT tooperform feladatokat, például adatbázisok hozzáadása és felhasználói szerepkörök kezelése. Alapértelmezés szerint a hello felhasználó által létrehozott hello server Analysis Services-kiszolgáló rendszergazdai automatikusan hozzáadja. Más rendszergazdák számára az Azure portál vagy a szolgáltatáshoz az SSMS használatával lehet hozzáadni. Kiszolgáló-rendszergazdák fiókkal kell rendelkeznie a hello hello Azure AD-bérlőben ugyanahhoz az előfizetéshez. több, lásd: toolearn [kiszolgáló-rendszergazdák kezelése](analysis-services-server-admins.md). 


**Adatbázis-felhasználók** toomodel adatbázisok csatlakoztatása a ügyfélalkalmazások, például Excel vagy a Power BI használatával. Felhasználók toodatabase szerepkörök hozzá kell adni. Adatbázis-szerepkörök definiálása a rendszergazda, a folyamat vagy olvasási engedéllyel az adatbázis. Fontos toounderstand adatbázis-felhasználók rendszergazdai jogosultságokkal rendelkező szerepkör rendszergazdái eltér. Azonban alapértelmezés szerint rendszergazdái egyaránt adatbázis-rendszergazdák. több, lásd: toolearn [adatbázis-szerepkörök és a felhasználók kezelése](analysis-services-database-users.md).

**Az Azure erőforrás-tulajdonosok**. Erőforrás-tulajdonosok egy Azure-előfizetés az erőforrások kezelése. Erőforrás-tulajdonosok használatával adhat hozzá az Azure AD felhasználói identitások tooOwner vagy közreműködői szerepkört egy előfizetésen belül **hozzáférés-vezérlés** Azure-portálon, vagy az Azure Resource Manager-sablonok. 

![Hozzáférés-vezérlés az Azure portálon](./media/analysis-services-manage-users/aas-manage-users-rbac.png)

Ezen a szinten szerepkörök toousers vagy tooperform hello portálon vagy az Azure Resource Manager-sablonok segítségével elvégezhető feladatokhoz szükséges fiókokat alkalmazni. több, lásd: toolearn [szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-what-is.md). 


## <a name="database-roles"></a>Adatbázis-szerepkörök

 Egy táblázatos modell meghatározott szerepköröket használják, adatbázis-szerepkörök. Ez azt jelenti, hogy hello szerepkörök az Azure Active Directory-felhasználók álló tagok tartalmaznak, és biztonsági csoportokat, amelyek konkrét engedélyeket, amelyek meghatározzák a hello művelet tagokkal rendelkezik egy modelladatbázissal vehet igénybe. Egy adatbázis-szerepkör külön objektumként hello adatbázis létrejön, és csak toohello adatbázist, amelyben ez a szerepkör létrehozása vonatkozik.   
  
 Alapértelmezés szerint amikor létrehoz egy táblázatos modell projekt hello jelentésmodell-projekt nem rendelkezik egyetlen szerepkörhöz sem. Szerepkörök hello SSDT szerepkörkezelő párbeszédpanel használatával határozható meg. Szerepkörök modell projekt tervezése során meghatározott, amikor azok alkalmazott csak toohello modell a munkaterület-adatbázis. Hello modell telepítésekor hello azonos szerepkörök olyan telepített alkalmazott toohello modell. A modell központi telepítése után server és adatbázis-rendszergazdák kezelhetik szerepkörök és tagok SSMS használatával. több, lásd: toolearn [adatbázis-szerepkörök és a felhasználók kezelése](analysis-services-database-users.md).
  


## <a name="next-steps"></a>Következő lépések

[Hozzáférés tooresources kezelése az Azure Active Directoryval](../active-directory/active-directory-manage-groups.md)   
[Adatbázis-szerepkörök és a felhasználók kezelése](analysis-services-database-users.md)  
[A kiszolgálók rendszergazdáinak kezelése](analysis-services-server-admins.md)  
[Szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-what-is.md)  