---
title: "Módosítsa a számlázás az Azure RemoteApp |} Microsoft Docs"
description: "Megtudhatja, hogyan állítsa le az Azure RemoteApp kiszámlázott."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 8f94da9a-7848-4ddc-b7b7-d9c280ccf4d7
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: mbaldwin
ms.openlocfilehash: 32fc673eeef01e80c73375bf264206beea8cfbe5
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/29/2017
---
# <a name="migrate-from-azure-remoteapp-to-mycloudit"></a>Az Azure RemoteApp MyCloudIT áttelepítése 

**Jelenleg használja a Microsoft Azure RemoteApp?** : MyCloudIT beépített egy automatizált eszköz, miközben továbbra is futtassa a Microsoft Azure áttelepítése az Azure RemoteApp (ARA) a következő gyűjtemény(ek) készleteit szinkronizálja a MyCloudIT kezelési platformot.

**Az Azure Resource Manager portal előnyeit**: a MyCloudIT platformra befejezett áttelepítés lehetővé teszi, hogy azonnali hozzáférés az Azure új Azure Resource Manager portálhoz. Ezen a portálon az új képességeket tartalmazza, és a Microsoft Azure által kínált innovációinak épül többek között a központi telepítés biztosításához a virtuális gépek méretét a hozzáférést az üzleti igényeinek támogatására.

**Annak biztosítása érdekében az igényeinek megfelelő megoldás párhuzamos vizsgálat**: A MyCloudIT áttelepítési eszköz kezdeményezheti az áttelepítési folyamat és a vizsgálati párhuzamos igénybe a jelenlegi ARA felhasználók továbbra is használja az ARA épül.  A felhasználók az ARA marad az áttelepítést, és tesztelési be nem fejeződik.  Az áttelepítési eszköz kezeléséhez a tipikus ARA gyűjtemény épül.  Ha úgy érzi, hogy rendelkezik-e egy egyedi, nem szabványos forgatókönyv, lépjen kapcsolatba velünk a [ sales@conexlink.com ](mailto:sales@conexlink.com) , elősegítve ezzel az áttelepítést egy szabott tervet biztosítunk is biztosíthatja.

**Asztali,--szolgáltatás képességek**: vegye figyelembe, hogy MyCloudIT nem csak a kezelői rendszeresen RemoteApp képességeket kínál, de a teljes asztali –--szolgáltatásként is kínálunk lehetőségeket biztosít a azonos költség nélkül bármely minimális felhasználó havonta követelmények.

## <a name="what-we-will-do-for-you"></a>Milyen műveleteket végezzük el az Ön

MyCloudIT automatizálja az áttelepítés az Azure RemoteApp sablon a klasszikus Azure portál, az előfizetés az Azure Resource Manager portálon a rendszerről való automatikus áttelepítéshez eszközzel az előfizetéséhez.  

> [!NOTE]
> Az Azure RemoteApp sablon ennek az eredeti Azure RemoteApp központi telepítés egy Azure-régióban kell maradniuk.  Ha Azure-régiókat, vagy az Azure-előfizetések módosítsa az áttelepítés során van szüksége, lépjen kapcsolatba velünk további útmutatást nyújt a [ sales@conexlink.com ](mailto:sales@conexlink.com).

Olvassa el alább az automatizált áttelepítési folyamat a MyCloudIT áttelepítési eszközzel a részletes információk:

1. Az áttelepítési eszköz ellenőrzi az aktuális előfizetés(ek) meglévő ARA követelményszabályait.  
2. Válassza ki az áttelepítendő egyszerre egy ARA gyűjteményt.  Ha több gyűjteményt, az eszköz többször is lefuthat.
3. Lehetősége van a felhasználói profil lemezeket (UPD) átmásolása az új központi telepítést, így vagy örökölt adatainak beolvasása, vagy manuálisan a felhasználóiprofil leképezése az új központi telepítés. Ha a felhasználóiprofil másolja, rendszer mentése a felhasználóiprofil és egy szövegfájlt, amely UPD rendeli minden egyes felhasználók nevét tartalmazza.  A felhasználóiprofil a RDSMGMT kiszolgálón megosztásra kerülnek `F:\Shares\LegacyUPD` és a megosztáson keresztül elérhetővé tehető `\\RDSmgmt\LegacyUPD`. 
4. Az áttelepítés az aktuális ARA üzembe helyezéshez szükséges állásidő nélkül.  De bármely módosításai a felhasználóiprofil (az ARA) a másolás után, ha ezeket a módosításokat nem megjelennek a felhasználóiprofil tárolódnak az Azure Resource Manager portal. 
5. Ha további virtuális gépek például a tartományvezérlők és a fájlkiszolgálók a klasszikus Azure virtuális hálózat fog kapcsolatot létesítünk a meglévő klasszikus Azure virtuális hálózatra és az új virtuális hálózat nem létrehozni, az új Azure-erőforrás között hálózatokból Manager portál.
6. Az automatizált megoldást csak főkiszolgálójával hálózatokból a meglévő klasszikus Azure virtuális hálózatra és az új virtuális hálózat között, ha a meglévő ARA központi telepítés egy hibrid telepítésben; tehát, akkor vannak amely a hitelesítéshez a Windows Server Active Directory tartományvezérlő meglévő klasszikus virtuális hálózaton. Ha a gyűjtemény hálózatokból nem létesítünk, de hálózatokból van szüksége, lépjen kapcsolatba velünk, [ sales@conexlink.com ](mailto:sales@conexlink.com) , és a Microsoft örömmel hálózatokból további költségek nélkül konfigurálja.
7. Az automatizált megoldást biztosítja az új virtuális hálózat annak érdekében, hogy az új központi telepítési csatlakozni tud-e a meglévő tartományvezérlőt a klasszikus virtuális hálózatot az Azure DNS-konfiguráció frissült.
8. Az automatizált megoldást fogja ellenőrizze, hogy egyetlen IP-címütközés azt az új virtuális hálózat létrehozása és egy meglévő Windows Server Active Directory-kiszolgáló rendelkező központi telepítések a Vnetben társviszony létesítéséhez.
9. Csak az Azure AD használ a hitelesítéshez, ha a MyCloudIT hozzon létre egy új Windows Server Active Directory-tartományhoz, és a meglévő Azure AD-példányt és az új Windows Server Active Directory-tartomány által létrehozott között felhasználók szinkronizálása az Azure AD Connect használatával MyCloudIT.
10. Használatakor a Windows Server Active Directory-tartomány ARA felhasználók hitelesítésére, az automatizált megoldást fognak kapcsolódni az új MyCloudIT központi telepítés a meglévő Windows Server Active Directory tartományvezérlő keresztül Vnetben társviszony-létesítés.
11. Azure Active Directory tartományi szolgáltatások a hitelesítéshez használ, akkor át azt, de lépjen kapcsolatba velünk, egyéni áttelepítési terv létrehozhatjuk meg.  Kérjük, küldjön egy e-mailek [ sales@conexlink.com ](mailto:sales@conexlink.com). 
12. Miután kell áttelepíteni a gyűjtemény a rendszer meggyőződött róla, elhelyezkedik vissza, és közben az automatizált megoldást áttelepíti az ARA gyűjtemény és a felhasználói Profillemezek (opcionális) az új távoli alkalmazások megoldás vezérelt MyCloudIT enyhíteni.
13. Ha a telepítés befejeződött, azt újra közzéteszi az ARA, és a feladás egy vagy több központi telepítési lesz további alkalmazások közzétételéhez fizetős közzétett ugyanazon alkalmazásokat.

## <a name="post-migration-benefits"></a>POST áttelepítés előnyei

1. Azt adja meg a felügyeleti konzol, amely lehetővé teszi a távoli alkalmazások telepítés teljes életciklusának kezelését.
2. A virtuális gépek kezelése a portálról lehet.  Indítása / leállítása és átméretezése egyes virtuális gépeken, ha szükséges.
3. A felügyeleti konzol lehetővé teszi a felhasználók létrehozásához és kezeléséhez, vagy a kezelési portálon csoportosítja.
4. A felügyeleti konzol segítségével szinkronizálhatók a felhasználók és az Office 365 bejelentkezési azonos környezetet.
5. A felügyeleti konzol lehetővé teszi további távoli alkalmazás- és asztali gyűjtemények létrehozása ismétlődő felhasználót költségek, vagy a minimális felhasználói követelmények nélkül. 
6. A felügyeleti konzol lehetővé teszi a távoli alkalmazások új alkalmazás-közzététel.
7. A felügyeleti konzol lehetővé teszi a indításakor és leállásakor a távoli alkalmazások központi telepítés ütemezése, ha csak megoldásra van szüksége az adott időszakban.
8. A felügyeleti konzol lehetővé teszi a telepítése és konfigurálása az Azure Backup szolgáltatás ügynöke, amely egy dokumentumot a megőrzési előzménygyűjteményhez nyújt az ügyféladatok automatizálásához.
9. A felügyeleti konzol a központi telepítés teljesítménymutatók hozzáférést biztosít.  Biztosítja a potenciális szűk keresztmetszetek azonosítása további teljesítmény felügyeleti eszközök telepítése nélkül.
10. Ha több munkamenet-állomás, lesz az automatikus skálázás, hogy csak a munkamenet-állomásokat, amelyek kell futtatni futtatnak.
11. MyCloudIT a RDWeb átjárókiszolgálón keresztül MyCloudIT tartománynév hozzáférést biztosít.  Azt is lehetővé teszi újbóli hozzárendelését az URL-cím, egy egyéni URL-branding végfelhasználói.

## <a name="prerequisites-for-migration"></a>A Migrálással kapcsolatos előfeltételek

1. Az Azure-előfizetéshez, amelyen a jelenlegi Azure RemoteApp megoldás hozzáféréssel kell rendelkeznie.
2. Telepítse át a sablon és a létrehozása / módosítása a MyCloudIT új központi telepítés az előfizetésen belül kell adja meg a portál engedélyeket.
3. Ne feledje, hogy a távoli Azure-alkalmazásban korlátai miatt, egyes gyűjtemények csak áttelepíthető három alkalommal.  Ha szeretné áttelepíteni a gyűjtemény több mint három alkalommal, az Azure-ba, az Exportálás számának növeléséhez jegy ablakába, vagy lépjen velünk kapcsolatba és azt segítséget nyújt az ARA kérelem az Exportálás számának növeléséhez.

## <a name="mycloudit-billing"></a>MyCloudIT számlázási

Lásd: [MyCloudIT árképzési RemoteApp megoldások](https://mcitdocuments.blob.core.windows.net/terms/MyCloudIT_Pricing_Overview.pdf) (PDF) kapcsolatos információk előre jelezni, és a teljes Azure költségek kezelésére.

Ha további kérdései, lépjen kapcsolatba velünk a [ sales@conexlink.com ](mailto:sales@conexlink.com) vagy a teljes bemutató videó [Azure RemoteApp áttelepítési eszköz - MyCloudIT](https://www.youtube.com/watch?v=YQ_1F-JeeLM&t=482s). 

