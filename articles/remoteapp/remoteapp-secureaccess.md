---
title: "aaaSecuring hozzáférés tooAzure RemoteApp, megalapozása |} Microsoft Docs"
description: "Ismerje meg, hogy mennyire vannak biztonságban hozzáférés tooAzure RemoteApp feltételes hozzáférés az Azure Active Directory használatával"
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
ms.openlocfilehash: 98dfe69e2f5fa5014b6eb3157e01f131c287134d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="securing-access-tooazure-remoteapp-and-beyond"></a>Biztonságossá tétele a hozzáférés tooAzure RemoteApp, és túl
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.
> 
> 

Hogyan állíthat be a rendszergazda egy biztonságos hozzáférést csatorna hello végfelhasználói Azure RemoteApp-től kezdődő, és egy biztonságos erőforrás, például az SQL-adatbázis vagy egy másik alkalmazás háttér-végződő áttekintést nyújtunk a cikkben. hello célja, hogy csak a jogosult felhasználók értekezlet hello szükséges feltételek távoli alkalmazáshoz hozzáférhetnek, és biztonságos háttér-hello, amely csak elérése szabályozott hello Azure RemoteApp környezetben, és nem más helyről toomake.

Üdvözöljük a rendszergazdákat, toolook kell 3 főbb területet van:

![Az Azure RemoteApp feltételes hozzáférés kapcsolatos szempontok](./media/remoteapp-secureaccess/ra-conditionalenvironment.png)

Olvassa el az adatokat és a válaszok toothese kérdéseket tesz fel.

## <a name="who-can-access-hello-collection"></a>Hello gyűjtemény hozzáférő felhasználók?
hello rendszergazda úgy dönt, hogy hello felhasználók által elérhető távoli alkalmazások hello gyűjteményben. Használhatja az Azure Active Directory (Azure AD) a munkahelyi vagy iskolai fiók (korábbi nevén, "munkahelyi és iskolai fiókok") vagy Microsoft-fiókok (pl. @outlook.com). A legtöbb vállalati forgatókönyv részeként használja az Azure AD-fiókok; ezeket, használhatja a feltételes hozzáférési funkciókat később tárgyalja, és hello csak choice tartományhoz gyűjtemények is. hello rest hello cikk feltételezi, hogy az Azure AD-fiókok az Azure RemoteApp használja.

**Mi azt hajtotta végre:**

Az Azure AD fiókok toocontrol hozzáférés tooAzure RemoteApp biztosítanak számunkra két dolgot használatával:

1. Mindig tudja hozzáférő felhasználók hello alkalmazások azt közzétett, és bármely vissza-végpontok azon alkalmazások hozzáférési csatlakoznak.
2. Azt szabályozza, hogy hello alapjául szolgáló az Azure AD, így hozhatunk létre és delete felhasználói fiókok, set jelszóházirendeket, használja a multi-factor authentication stb. 

## <a name="how-is-hello-collection-accessed-from-where"></a>Hogyan érhető el hello gyűjtemény? Honnan?
Gyakran rendszergazdáknak is érdemes toodefine házirendek nyilvános internetre környezetben, például az Azure RemoteApp eléréséhez. Például, hogy hello vállalati hálózaton kívüli hello környezete hozzáférő felhasználók hozzáférhetnek toouse többtényezős hitelesítés (MFA) toogain; tooensure kívánják. vagy lehet, hogy azok le kell tiltani regisztrálását.

Az Azure RemoteApp rendszergazdái hello funkció érhető el prémium szintű Azure AD tooset feltételes hozzáférési szabályzatokkal az Azure RemoteApp környezetben. Szolgáltatást is alkalmazhatja gazdag jelentések és riasztási szolgáltatásokat toomonitor hogyan hello környezet használatban van.

### <a name="how-tooset-up-conditional-access-for-azure-remoteapp"></a>Hogyan tooset feltételes hozzáférés beállítása az Azure RemoteApp
Folyamatban, amelyet toowalk egy forgatókönyv példáján keresztül – hello Azure RemoteApp rendszergazda tooblock access toohello környezet hello vállalati hálózaton kívüli felhasználók esetén.

> [!NOTE]
> Feltételezzük, hogy frissítette az Azure AD toohello prémium csomagban és a létrehozott legalább egy Azure RemoteApp-gyűjteményt.
> 
> 

1. Az Azure portálon kattintson a hello **Active Directory** fülre. Kattintson a kívánt tooconfigure hello könyvtár.
   
   Ne feledje: Feltételes hozzáférés egy tulajdonság, a címtár- és nem az Azure RemoteApp, az összes konfigurációs hello könyvtár szintjén történik. Ez azt is jelenti toobe hello directory rendszergazda toomake módosítások van szüksége.
2. Kattintson a **alkalmazások**, és kattintson a **Microsoft Azure RemoteApp** tooset feltételes hozzáférés. Vegye figyelembe, hogy beállíthat feltételes hozzáférés minden "szolgáltatott szoftver" alkalmazáshoz a címtárban külön-külön.
   ![Feltételes hozzáférés beállítása az Azure RemoteApp](./media/remoteapp-secureaccess/ra-conditionalaccessscreen.png)
3. A hello **konfigurálása** lapon **engedélyezése hozzáférési szabályok** tooON.
   ![Az Azure RemoteApp hozzáférési szabályok engedélyezése](./media/remoteapp-secureaccess/ra-enableaccessrules.png)
4. Most már konfigurálhatja különböző szabályok, és válassza ki, akik tooapply azokat:
   
   1. Válasszon **letiltja a hozzáférést, ha nem munkahelyi** toocompletely a felhasználók megakadályozása az Azure RemoteApp eléréséhez megadott hello hálózati környezeten kívül.
   2. Kattintson a hello lehetőség alatt toodefine hello IP-címtartományok a "megbízható hálózatot alkotó." Minden kívül azokat, a program elutasítja.
5. Tesztelje a konfigurációt hello Azure RemoteApp-ügyfelet a megadott hello tartományon kívül eső IP-cím indításával. Miután jelentkezik be az Azure AD hitelesítő adatait egy ehhez hasonló üzenetet kell megjelennie:

![A hozzáférés megtagadásáról szóló tooAzure RemoteApp](./media/remoteapp-secureaccess/ra-accessdenied.png)

### <a name="future-conditional-access-features"></a>Jövőbeli feltételes hozzáférési funkciókat
hello Azure Active Directory csapat dolgozik a feltételes hozzáférés új képessége. A rendszergazdák képes toocreate szabályok hálózati helye alapján szabályok túl új típusú lesz. A nyilvános előzetes verzió új funkcióinak hello hamarosan elérhetővé kell válnia.

### <a name="how-toomonitor-access-tooazure-remoteapp"></a>Hogyan érik el toomonitor tooAzure RemoteApp
Feltételes hozzáférés mellett a nagy szolgáltatás toouse hello Azure Active Directory Premium jelentéskészítési funkciójának. Jelentések toomonitor ki fér hozzá a környezetben használja, és bármilyen gyanús tevékenységek észlelése.

Például láthatja a hello hello-felhasználók neveit, ki fért hozzá az Azure RemoteApp hányszor tevékenységük, és mikor.

1. Az Azure portálon kattintson **Active Directory**, majd kattintson a címtárban.
2. Nyissa meg toohello **jelentések** fülre.
3. A jelentések hello listájában válassza ki **Alkalmazáshasználat** alatt **integrált alkalmazások**.
   
   Összesített statisztikai adatokat láthatja az Azure RemoteApp. 
   ![Azure RemoteApp-hozzáférés összesített statisztikák](./media/remoteapp-secureaccess/ra-accessstats.png)
4. Kattintson a hello alkalmazás tooreveal elérése az Azure RemoteApp felhasználói adatokat.
   ![Azure RemoteApp felhasználói hozzáférés statisztikái](./media/remoteapp-secureaccess/ra-userstats.png)

### <a name="summary"></a>Összefoglalás
Az Azure Active Directory Premium állíthatja be a hozzáférési szabályok tooAzure RemoteApp (és más szoftverek elérhető Azure AD szolgáltatás alkalmazásként). Szabályok jelenleg korlátozott toonetwork helyét alapján házirendek, de a jövőbeli hello kiterjesztése vállalati kezelésének tooother szempontját.

Prémium szintű Azure AD is lehetővé teszi a jelentéskészítési és figyelési képességek, amelyek további hello vezérlő Üdvözöljük a rendszergazdákat rendelkezik-e az Azure RemoteApp környezetben keresztül.

## <a name="how-do-i-make-sure-my-secure-resource-is-accessible-only-from-my-azure-remoteapp-environment"></a>Hogyan tehetem meg arról, hogy a biztonságos erőforrás csak az Azure RemoteApp környezetben elérhető-e?
Ez a cikk korábbi szakaszokban azt kiemelt hozzáférési toohello Azure RemoteApp környezetben való biztosításával. A Microsoft hajtotta végre, amely hello felhasználók, akik hozzáférést kiválasztása és hozzáférési szabályok toofurther beállítása, hogyan használhatják a hello szolgáltatást.

Egy általános forgatókönyv Azure RemoteApp telepítésekhez, hogy a távoli alkalmazások hello kell toocommunicate háttér-erőforrással, például egy SQL-adatbázis. Ehhez az erőforráshoz üzemelteti a helyszíni (például a vállalati hálózat) vagy hello felhőben (például a Azure IaaS). A rendszergazdák gyakran érdemes toomake meg arról, hogy hello háttér-erőforrás csak elérhető Azure RemoteApp segítségével központilag telepített alkalmazások és nem például az alkalmazások közvetlenül a felhasználó számítógépen fut, valamint a nyilvános interneten keresztül fér hozzá. Az Azure RemoteApp gyakran előforduló, hello központilag felügyelt és biztonságos környezetben, és így hello csak elérési amelyen keresztül a felhasználók kell interakcióba hello háttér-erőforrás.

hello megoldás tooplace mindkét hello Azure RemoteApp környezetben, és biztonságos erőforrás hello hello azonos Azure virtuális hálózatot (VNET). Ha hello erőforrás egy másik helyen, akkor is a pont-pont VPN-kapcsolatot, például egy virtuális hálózat feszítőfa hello Azure adatközpontba toocreate és hello ügyfél helyszíni környezetben.

Az Azure RemoteApp a gyűjtemény központi telepítések, ahol megadhatja a saját virtuális hálózat két típusokat támogatja:

* A nem tartományhoz csatlakoztatott: hello alkalmazások lesz "sort a láthatáron" hello, más erőforrások hello virtuális hálózat. Például ez lehet használt tooconnect alkalmazások tooa SQL-adatbázis SQL-hitelesítést használó (alkalmazások hitelesítést közvetlenül hello adatbázison hello felhasználó)
* Tartományhoz csatlakozó: hello virtuális gépek Azure RemoteApp által használt hello VNET illesztett tooa tartományvezérlőjén. Ez akkor hasznos, ha hello alkalmazásokat kell tooauthenticate egy Windows rendszerbeli tartományvezérlőnek a rendelés tooget hozzáférés tooa háttér-erőforrás ellen.
  ![Az Azure Remoteappban egy tartományhoz csatlakoztatott gyűjteményekben](./media/remoteapp-secureaccess/ra-domainjoined.png)

### <a name="how-toocreate-a-secure-connection-between-azure-and-my-on-premises-environment"></a>Hogyan toocreate egy biztonságos kapcsolatot az Azure és a helyszíni környezetben
Csatlakozás az Azure és a helyszíni környezetben több konfigurációs lehetőség áll rendelkezésre. Egy jó hello beállításainak áttekintése itt áll rendelkezésre.

Az Azure RemoteApp kell tooconfigure a virtuális hálózat először, és azt használja a gyűjtemény hello létrehozási folyamata során. 

## <a name="hello-complete-solution"></a>hello teljes megoldás
az alábbi ábrán hello hello teljes megoldást mutat, ahol azt építenek egy biztonságos csatornát hello végfelhasználói, keresztül Azure RemoteApp (ARA), a hello háttér erőforrás.
![Azure RemoteApp biztonságos](./media/remoteapp-secureaccess/ra-secureoverview.png) az 1. lépés jelenleg kijelölt hello felhasználók és szabályozására, hogy az ARA hozzáférhetők hozzáférési szabályok létrehozása. A hello az alábbi példa azt csak hozzá lehessen férni azoknak a hello vállalati hálózatról. Nem megfelelő felhasználók nem fognak tudni tooaccess hello ARA környezet minden.
2. fázis azt hello háttér erőforrás csak keresztül hello virtuális hálózat és a VPN konfigurációs, amelyek azt szabályozzák van kitéve. Az Azure RemoteApp került hello azonos virtuális hálózaton. hello végeredménynek hello erőforrás csak elérése hello ARA-környezeten keresztül.

