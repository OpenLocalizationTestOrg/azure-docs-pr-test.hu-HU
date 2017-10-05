---
title: "Az Azure Remoteapphoz, vagy azon kívül tárolt hozzáférés biztosítása |} Microsoft Docs"
description: "Ismerje meg, hogyan biztonságos hozzáférés az Azure RemoteApp segítségével a feltételes hozzáférés az Azure Active Directoryban"
services: remoteapp
documentationcenter: 
author: piotrci
manager: mbaldwin
ms.assetid: a19b0b09-ab26-4beb-80bb-33a46da39887
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 28ee4698515d11964e5371628d21dbc00687c861
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="securing-access-to-azure-remoteapp-and-beyond"></a>Hozzáférés az Azure Remoteapphoz, vagy azon kívül tárolt biztonsága
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. A részletekért olvassa el a [bejelentést](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

A rendszergazda hogyan állíthat be egy biztonságos hozzáférést csatornát a végfelhasználó Azure RemoteApp-től kezdődő, és egy biztonságos erőforrás, például az SQL-adatbázis vagy egy másik alkalmazás háttér-végződő áttekintést nyújtunk a cikkben. A cél, hogy győződjön meg arról, hogy a kívánt feltételeknek csak a jogosult felhasználók távoli alkalmazáshoz hozzáférhetnek, és a biztonságos háttér-csak elérése az Azure RemoteApp ellenőrzött környezetben, és nem más helyről.

Nincsenek 3 főbb területet, a rendszergazdának hozzá kell tekintse meg:

![Az Azure RemoteApp feltételes hozzáférés kapcsolatos szempontok](./media/remoteapp-secureaccess/ra-conditionalenvironment.png)

Olvassa el az információkat, valamint ezekre a kérdésekre adott válaszok.

## <a name="who-can-access-the-collection"></a>A gyűjtemény hozzáférő felhasználók?
A rendszergazda úgy dönt, hogy a felhasználók, a gyűjtemény távoli alkalmazáshoz hozzáférhetnek. Használhatja az Azure Active Directory (Azure AD) a munkahelyi vagy iskolai fiók (korábbi nevén, "munkahelyi és iskolai fiókok") vagy Microsoft-fiókok (pl. @outlook.com). A legtöbb vállalati forgatókönyv részeként használja az Azure AD-fiókok; segítségükkel feltételes hozzáférés szolgáltatások lejjebb és gyűjtemények tartományhoz egyetlen lehetősége is. A cikk többi feltételezi, hogy az Azure AD-fiókok az Azure RemoteApp használja.

**Mi azt hajtotta végre:**

Azure AD-fiókok használatával történő hozzáférés szabályozása érdekében az Azure RemoteApp biztosítanak számunkra két dolgot:

1. Mindig tudja ki férhet hozzá az alkalmazások a Microsoft közzétett és hozzáférés vissza-végpontok azon alkalmazások csatlakozni.
2. Az alapul szolgáló Azure AD azt szabályozzák, így hozhatunk létre és delete felhasználói fiókok, set jelszóházirendeket, használja a multi-factor authentication stb. 

## <a name="how-is-the-collection-accessed-from-where"></a>Hogyan érhető el a gyűjtemény? Honnan?
A rendszergazdák gyakran szeretne meghatározni házirendek nyilvános internetre környezetben, például az Azure RemoteApp eléréséhez. Például szeretnék biztosítja, hogy a környezete a vállalati hálózaton kívülről hozzáférő felhasználók többtényezős hitelesítés (MFA) segítségével hozzáférést; vagy lehet, hogy azok le kell tiltani regisztrálását.

Az Azure RemoteApp-rendszergazdák az Azure AD Premium keresztül elérhető funkciók beállításához használja az Azure RemoteApp-környezet feltételes hozzáférési szabályzatokat. Is használhatnak gazdag jelentéskészítési és riasztási szolgáltatásokat figyelésére, hogyan a környezet használatban van.

### <a name="how-to-set-up-conditional-access-for-azure-remoteapp"></a>Feltételes hozzáférés beállítása az Azure RemoteApp
Forgatókönyv – a rendszergazda letiltja a hozzáférést a környezetet, ha a felhasználók a vállalati hálózaton kívül vannak az Azure RemoteApp bízná fogjuk.

> [!NOTE]
> Feltételezzük, hogy az Azure AD prémium tarifacsomagra frissítette, és létrehozott legalább egy Azure RemoteApp-gyűjteményt.
> 
> 

1. Az Azure portálon kattintson a **Active Directory** fülre. Kattintson a konfigurálni kívánt könyvtár.
   
   Ne feledje: Feltételes hozzáférés egy tulajdonság, a címtár- és nem az Azure RemoteApp, így a könyvtár szintjén történik az összes konfigurációs. Ez azt is jelenti, rendszergazdának kell lennie a címtár módosítások végrehajtásához.
2. Kattintson a **alkalmazások**, és kattintson a **Microsoft Azure RemoteApp** feltételes hozzáférés beállítása. Vegye figyelembe, hogy beállíthat feltételes hozzáférés minden "szolgáltatott szoftver" alkalmazáshoz a címtárban külön-külön.
   ![Feltételes hozzáférés beállítása az Azure RemoteApp](./media/remoteapp-secureaccess/ra-conditionalaccessscreen.png)
3. Az a **konfigurálása** lapon **engedélyezése hozzáférési szabályok** ON értékre állítása.
   ![Az Azure RemoteApp hozzáférési szabályok engedélyezése](./media/remoteapp-secureaccess/ra-enableaccessrules.png)
4. Most már konfigurálhatja a különböző szabályok, és válassza ki azokat alkalmazni:
   
   1. Válasszon **letiltja a hozzáférést, ha nem munkahelyi** szeretné akadályozni, hogy felhasználók hozzáférését az Azure RemoteApp a megadott hálózati környezeten kívül.
   2. Kattintson a lenti lehetőséget adja meg az IP-címtartományok, a "megbízható hálózatot alkotó." Minden kívül azokat, a program elutasítja.
5. Tesztelje a konfigurációt a megadott tartományon kívül eső IP-cím az az Azure RemoteApp ügyfél elindításához. Miután jelentkezik be az Azure AD hitelesítő adatait egy ehhez hasonló üzenetet kell megjelennie:

![Az Azure Remoteapphoz hozzáférése megtagadva](./media/remoteapp-secureaccess/ra-accessdenied.png)

### <a name="future-conditional-access-features"></a>Jövőbeli feltételes hozzáférési funkciókat
Az Azure Active Directory csapat dolgozik a feltételes hozzáférés új képessége. A rendszergazdák tudják új típusú szabályok túl hálózati hely alapú szabályok létrehozását. Az új funkció nyilvános előnézete hamarosan elérhetővé kell válnia.

### <a name="how-to-monitor-access-to-azure-remoteapp"></a>Azure RemoteApp eléréséhez figyelése
Feltételes hozzáféréssel együtt használandó kiváló jellemzője az Azure Active Directory Premium jelentéskészítési funkciókat. Ki fér hozzá a környezet figyelése jelentésekkel, és bármilyen gyanús tevékenységek észlelése.

Például láthatja az Azure RemoteApp-tevékenységük azt, hogy hány alkalommal hozzáférő felhasználók nevét, és mikor.

1. Az Azure portálon kattintson **Active Directory**, majd kattintson a címtárban.
2. Lépjen a **jelentések** fülre.
3. Válassza ki a listáról a jelentések **Alkalmazáshasználat** alatt **integrált alkalmazások**.
   
   Összesített statisztikai adatokat láthatja az Azure RemoteApp. 
   ![Azure RemoteApp-hozzáférés összesített statisztikák](./media/remoteapp-secureaccess/ra-accessstats.png)
4. Kattintson az alkalmazás Azure RemoteApp hozzáférő felhasználók kapcsolatos információk megjelenítéséhez.
   ![Azure RemoteApp felhasználói hozzáférés statisztikái](./media/remoteapp-secureaccess/ra-userstats.png)

### <a name="summary"></a>Összefoglalás
Az Azure Active Directory Premium állíthat be a hozzáférési szabályok Azure RemoteApp (és más szoftverek elérhető Azure AD szolgáltatás alkalmazásként). Szabályok jelenleg korlátozott hálózati helye alapján házirendek, de más szempontokat enterprise Management a jövőben kerül sor.

Prémium szintű Azure AD is biztosít, jelentéskészítési és figyelési képességek, amelyek további a vezérlő a rendszergazda rendelkezik-e az Azure RemoteApp környezetben keresztül.

## <a name="how-do-i-make-sure-my-secure-resource-is-accessible-only-from-my-azure-remoteapp-environment"></a>Hogyan tehetem meg arról, hogy a biztonságos erőforrás csak az Azure RemoteApp környezetben elérhető-e?
Ez a cikk korábbi szakaszokban azt arra irányul, az Azure RemoteApp környezetben való hozzáférés biztosítása. Azt kell meghatározni, amely kiválasztásáról, a felhasználókat, akik hozzáférhetnek, és további vezérlő a hozzáférési szabályok beállítása, hogyan használhatják a szolgáltatás.

Egy általános forgatókönyv Azure RemoteApp telepítésekhez, hogy a távoli alkalmazások szeretne-e kommunikálni a háttér-erőforrás, például egy SQL-adatbázisban. Ehhez az erőforráshoz üzemelteti a helyszíni (például a vállalati hálózat) vagy a felhőben (például a Azure IaaS). A rendszergazdák gyakran szeretne győződjön meg arról, hogy a háttér-erőforrás csak érhető el az Azure RemoteApp segítségével központilag telepített alkalmazások és nem például az alkalmazások közvetlenül a felhasználó számítógépen fut, valamint a nyilvános interneten keresztül fér hozzá. Az Azure RemoteApp gyakran előforduló, a központilag felügyelt és biztonságos környezetben, és így a csak elérési utat, amelyen keresztül a felhasználók kell használják a háttér-erőforrás.

A megoldás, mind az Azure RemoteApp környezetben, és a biztonságos erőforrás az ugyanazon Azure virtuális hálózat (virtuális) helyezhető el. Ha az erőforrás egy másik helyen, akkor is a pont-pont VPN-kapcsolatot, például hozhat létre egy Vnetet az Azure-adatközpontban és az ügyfél helyszíni környezetben átfedés.

Az Azure RemoteApp a gyűjtemény központi telepítések, ahol megadhatja a saját virtuális hálózat két típusokat támogatja:

* A nem tartományhoz csatlakoztatott: az alkalmazások lesz "sort a láthatáron" az egyéb erőforrások a virtuális hálózat. Például ennek segítségével csatlakozzon az SQL-adatbázis SQL-hitelesítést használó alkalmazások (alkalmazások hitelesítéshez a felhasználó közvetlenül az adatbázison)
* Tartományhoz csatlakozó: az Azure RemoteApp által használt virtuális gépek tartományhoz csatlakoztatott virtuális tartományvezérlő. Ez akkor hasznos, ha az alkalmazásokat kell ahhoz, hogy a háttér-erőforráshoz való hozzáférést egy Windows rendszerbeli tartományvezérlőnek hitelesítése.
  ![Az Azure Remoteappban egy tartományhoz csatlakoztatott gyűjteményekben](./media/remoteapp-secureaccess/ra-domainjoined.png)

### <a name="how-to-create-a-secure-connection-between-azure-and-my-on-premises-environment"></a>Egy biztonságos kapcsolatot az Azure és a helyszíni környezet létrehozása
Csatlakozás az Azure és a helyszíni környezetben több konfigurációs lehetőség áll rendelkezésre. A beállítások egy jó áttekintése itt áll rendelkezésre.

Az Azure RemoteApp kell először konfigurálja a virtuális hálózat, és azt használja a gyűjtemény létrehozása közben. 

## <a name="the-complete-solution"></a>A teljes megoldás
Az alábbi ábra mutatja a teljes megoldás, ahol mi építettünk egy biztonságos csatornát a végfelhasználó keresztül Azure RemoteApp (ARA), a háttér-erőforrás a.
![Azure RemoteApp biztonságos](./media/remoteapp-secureaccess/ra-secureoverview.png) az 1. lépés jelenleg kijelölt a felhasználók és szabályozására, hogy az ARA hozzáférhetők hozzáférési szabályok létrehozása. Azt az alábbi példában csak a felhasználók számára a vállalati hálózathoz való hozzáférés engedélyezése. Nem megfelelő felhasználók nem fognak tudni az ARA környezet minden eléréséhez.
2. szakasz azt a háttérrendszer erőforráshoz csak a virtuális hálózat és a VPN konfigurációs, amelyek azt szabályozzák van kitéve. Az Azure RemoteApp ugyanazon virtuális ide lett helyezve. A záró eredménye, hogy az erőforrás csak a következőkkel érhetők el az ARA-környezetben.

