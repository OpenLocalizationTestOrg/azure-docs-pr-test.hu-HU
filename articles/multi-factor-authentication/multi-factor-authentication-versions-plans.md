---
title: "aaaAzure MFA-verziók és fogyasztói tervek |} Microsoft Docs"
description: "Hello multi-factor Authentication ügyfél és hello különböző módszereket és verzióra vonatkozó információk. Minden egyes felhasználási terv adatait"
keywords: 
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: kgremban
ms.openlocfilehash: 4914747e435531b9f950356d23aa386f3d9585d2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooget-azure-multi-factor-authentication"></a>Hogyan tooget Azure multi-factor Authentication

Tooprotecting ismét a fiókok, kétlépéses ellenőrzést kell szabványos a szervezetben. Ez a szolgáltatás különösen fontos a privilegizált hozzáférés tooresources rendszergazdai fiókok. Emiatt a Microsoft biztosít az alapszintű kétlépéses ellenőrzés szolgáltatások tooOffice 365 és Azure rendszergazdák. Ha tooupgrade hello szolgáltatások a rendszergazdák, vagy szeretné bővíteni a kétlépéses ellenőrzés toohello rest a felhasználók, az Azure multi-factor Authentication is vásárolhat. 

Ez a cikk foglalkozik hello különbség hello különböző példányainak verzióiban érhető el teljes körű Azure MFA verzió tooadministrators és hello ismerteti, és határozza meg, mely funkciók érhetők el az egyes. Ha készen áll toodeploy hello Azure MFA ajánlatot befejeződik, hello későbbi szakaszokban magában foglalja az végrehajtási beállítások, és hogyan számítja ki a Microsoft a felhasználását.

>[!IMPORTANT]
>Ez a cikk célja toobe egy útmutató toohelp tisztában hello Azure multi-factor Authentication különböző módokon toobuy. Árak és számlázás kapcsolatos részleteket, mindig tekintse át toohello [multi-factor Authentication árképzést ismertető oldalra](https://azure.microsoft.com/pricing/details/multi-factor-authentication/).

## <a name="available-versions-of-azure-multi-factor-authentication"></a>Azure multi-factor Authentication elérhető verzió

hello következő táblázat a multi-factor authentication három verziója hello különbségei:

| Verzió | Leírás |
| --- | --- |
| Multi-Factor Authentication az Office 365-höz |Ebben a verzióban kizárólag az Office 365 alkalmazásokkal együttműködve és hello Office 365 portálon kezelt. A rendszergazdák [Office 365 erőforrásokat a kétlépéses ellenőrzéshez használttal biztonságos](https://support.office.com/article/Set-up-multi-factor-authentication-for-Office-365-users-8f0454b2-f51a-4d9c-bcde-2c48e41621c6). Ebben a verzióban az Office 365-előfizetéssel része. |
| A rendszergazdák az Azure többtényezős hitelesítést | Az Azure-bérlő globális rendszergazdái engedélyezheti a kétlépéses ellenőrzést, a globális rendszergazdai fiókokhoz, minden további költség nélkül.|
| Azure Multi-Factor Authentication | Gyakran hivatkozott tooas hello "teljes" verzió, az Azure multi-factor Authentication hello richest kezelési képességek kínál. További konfigurációs beállítások hello keresztül biztosít [a klasszikus Azure portálon](https://manage.windowsazure.com), speciális hibajelentési, és a helyszíni számos támogatása és a felhőalapú alkalmazásokhoz. Az Azure többtényezős hitelesítés az Azure Active Directory Premium (P1 és P2 tervek) és a nagyvállalati mobilitási + biztonsági (E3 és E5 tervek) része, és lehet telepíteni vagy [hello felhőben vagy helyszínen](multi-factor-authentication-get-started.md). |

## <a name="feature-comparison-of-versions"></a>Szolgáltatások összehasonlítása verziók
hello következő táblázat felsorolja hello szolgáltatások által biztosított hello Azure multi-factor Authentication különböző verzióiban.

> [!NOTE]
> Az összehasonlítási táblázat minden egyes multi-factor Authentication verziójának részét képező hello szolgáltatások ismerteti. Ha hello teljes Azure multi-factor Authentication szolgáltatással rendelkezik, néhány funkció nem áll rendelkezésre, attól függően, hogy használ-e [hello felhőben többtényezős hitelesítés vagy MFA helyszíni](multi-factor-authentication-get-started.md).


| Szolgáltatás | Multi-Factor Authentication az Office 365-höz | A rendszergazdák az Azure többtényezős hitelesítést | Azure Multi-Factor Authentication |
| --- |:---:|:---:|:---:|
| Az MFA Használatát a rendszergazdai fiókok védelme |● |● (csak a globális rendszergazdai fiókok esetében) |● |
| Második tényezőként mobilalkalmazás |● |● |● |
| Második tényezőként telefonhívás |● |● |● |
| Második tényezőként SMS |● |● |● |
| Alkalmazásjelszók az ügyfelek, amelyek nem támogatják a többtényezős hitelesítés |● |● |● |
| Az ellenőrzési módszereket rendszergazda általi szabályozásának |● |● |● |
| PIN-mód | | |● |
| Csalási riasztás | | |● |
| MFA-jelentések | | |● |
| Egyszeri mellőzés | | |● |
| Egyéni üdvözlések a telefonhívásokhoz | | |● |
| Egyéni Hívóazonosító a telefonhívások | | |● |
| Megbízható IP-címek | | |● |
| MFA megjegyzése megbízható eszközökön |● |● |● |
| MFA SDK | | |● (igényel multi-factor Auth provider és a teljes Azure-előfizetés) |
| Többtényezős hitelesítés a helyszíni alkalmazások | | |● |

## <a name="how-tooget-azure-multi-factor-authentication"></a>Hogyan tooget Azure multi-factor Authentication
Ha szeretné az Azure multi-factor Authentication által kínált hello összes funkcióját, többféle módon:

### <a name="option-1---mfa-licenses"></a>1 - MFA-licencet. lehetőséget

Azure multi-factor Authentication licencet vásárolt, és rendeljen hozzájuk tooyour felhasználók az Azure Active Directoryban. 

Ha ezt a beállítást használja, az Azure multi-factor Authentication hitelesítésszolgáltató csak akkor, ha licenccel nem rendelkező felhasználók is kell tooprovide kétlépéses ellenőrzést kell létrehoznia. Ellenkező esetben akkor előfordulhat, hogy számlázható kétszer.

### <a name="option-2---bundled-licenses-that-include-mfa"></a>Kötegelt beállítás 2 - licencet, amely tartalmazza az MFA

Az licencet, amely tartalmazza az Azure multi-factor Authentication hitelesítés, mint például az Azure Active Directory Premium (P1 vagy P2) vagy nagyvállalati mobilitási + biztonsági (E3 vagy E5), és rendeljen hozzájuk tooyour felhasználók az Azure Active Directoryban. 

Ha ezt a beállítást használja, az Azure multi-factor Authentication hitelesítésszolgáltató csak akkor, ha licenccel nem rendelkező felhasználók is kell tooprovide kétlépéses ellenőrzést kell létrehoznia. Ellenkező esetben akkor előfordulhat, hogy számlázható kétszer. 

### <a name="option-3---mfa-consumption-based-model"></a>MFA-fogyasztás alapján modell 3 - beállítás

Hozzon létre egy Azure multi-factor Authentication hitelesítésszolgáltató belül Azure-előfizetéssel. Az Azure MFA-szolgáltatók számlázása a nagyvállalati szerződés, Azure előfizetési vagy hitelkártya, mint minden más Azure-erőforrások Azure-erőforrások. Ezek a szolgáltatók csak a teljes Azure-előfizetések, korlátozás nélkül Azure előfizetések, amelyek a költségkeret $0 hozhatók létre. Korlátozott előfizetések aktiválásakor licencek, mint 1. és 2-beállításokban jönnek létre. 

Az Azure multi-factor Authentication hitelesítésszolgáltató használata esetén van elérhető, amely az Azure-előfizetéshez keresztül számlázása két használati modellekről:  

1. **Felhasználónként** - vállalatok szeretné, hogy az alkalmazottak, akik rendszeresen hitelesítési rögzített számú tooenable kétlépéses ellenőrzést. Egyes felhasználók számlázási hello száma az Azure AD-bérlő és/vagy az Azure MFA kiszolgáló a többtényezős hitelesítés használatára jogosult felhasználókat alapul. Felhasználók engedélyezve vannak-e a multi-factor Authentication mindkét Azure AD-ben és az Azure MFA kiszolgáló, és a tartomány sync (az Azure AD Connect) engedélyezve van, majd a Microsoft hello nagyobb mennyiségű felhasználók száma. Ha tartományi szinkronizálás nincs engedélyezve, akkor azt az összes olyan felhasználó, az Azure AD MFA engedélyezve hello sum száma és az Azure MFA kiszolgáló. Számlázási rendszer arányosított és jelentett toohello Commerce naponta. 

  > [!NOTE]
  > Számlázási 1. példa: jelenleg engedélyezve van a multi-factor Authentication 5000 felhasználója van. hello MFA rendszer elosztja ezt a számot 31 és jelentések 161.29 felhasználók napra vonatkozóan. Holnap engedélyezi 15 több felhasználó, így hello MFA rendszer 161.77 felhasználók jelentései adott napon. Számlázási ciklus hello hello végére szemben az Azure-előfizetéshez számlázott felhasználók teljes száma hello tooaround 5000 összeadja. 
  >
  > Számlázási 2. példa: rendelkezik licenccel rendelkező felhasználók és a felhasználók nélkül, így a felhasználói Azure MFA-szolgáltatóra toomake hello különbség fel kell. Nincsenek 4,500 nagyvállalati mobilitási + biztonsági licencek a tenant, de 5000 felhasználója van engedélyezve a többtényezős Hitelesítést. Az Azure-előfizetéshez fizetnie kell 500 felhasználónak, arányosítva, és naponta jelentett 16.13 felhasználóként. 

2. **Hitelesítésenként** - tooenable kétlépéses ellenőrzést szánt nagyszámú felhasználók, akik ritkán hitelesítési vállalatok számára. Számlázási hello Azure MFA felhőalapú szolgáltatás, függetlenül attól, hogy ezen ellenőrzések sikeres, vagy a rendszer megtagadja által fogadott kétlépéses ellenőrzés kérelmek száma hello alapul. A számlázási jelenik meg az Azure használati utasítás csomagokban 10 hitelesítést, és jelentett toohello Commerce rendszer naponta. 

  > [!NOTE]
  > Számlázási 3. példa: jelenleg hello Azure MFA szolgáltatással 3,105 kétlépéses ellenőrzés kérelem érkezett. Az Azure-előfizetéshez 310.5 hitelesítési csomagok lesz számlázva. 

Fontos fontos toonote is az Azure MFA licenccel rendelkezik, de továbbra is fizetnie első használat alapú konfigurálást. Ha beállította a hitelesítés Azure MFA-szolgáltatóra, kell fizetni összes kétlépéses ellenőrzés kérés még az licenccel rendelkező felhasználók végezhető el. Ha olyan tartományhoz, amely nem csatolt tooyour Azure AD-bérlő beállított felhasználói Azure MFA-szolgáltatóra, kell fizetni engedélyezett felhasználónkénti akkor is, ha a felhasználók Azure ad-val licenccel rendelkezik. 

## <a name="next-steps"></a>Következő lépések

- További árképzési részletekért lásd: [Azure MFA szolgáltatás díjszabása](https://azure.microsoft.com/pricing/details/multi-factor-authentication/).

- Válasszon e toodeploy Azure MFA [hello felhőalapú vagy helyszíni](multi-factor-authentication-get-started.md)
