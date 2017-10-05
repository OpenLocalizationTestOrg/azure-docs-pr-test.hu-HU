---
title: "Az Azure MFA-verziók és felhasználási tervek |} Microsoft Docs"
description: "A multi-factor Authentication ügyfél és a különböző módszereket és verzióra vonatkozó információk. Minden egyes felhasználási terv adatait"
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
ms.openlocfilehash: 5adffb0d461503b57ff9152671c44716dd044b1e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-get-azure-multi-factor-authentication"></a>Azure multi-factor Authentication beszerzése

A fiókok védelmének ismét, kétlépéses ellenőrzést kell szabványos szervezeten belüli. Ez a szolgáltatás különösen fontos a rendszergazdai fiókok, amelyek privilegizált erőforrásokhoz való hozzáférést. Emiatt a Microsoft alapvető kétlépéses ellenőrzés funkciókat nyújtja a Office 365 és Azure rendszergazdák számára. Ha szeretné frissíteni a rendszergazdák tartozó szolgáltatásokat, vagy a felhasználók a többi kétlépéses ellenőrzés kiterjesztése, az Azure multi-factor Authentication is vásárolhat. 

Ez a cikk ismerteti a rendszergazdák, valamint a teljes Azure MFA-verziót verziók közötti különbséget ismerteti, és határozza meg, mely funkciók érhetők el az egyes. Ha a teljes Azure MFA ajánlat üzembe helyezésére, a későbbi fejezetek foglalkozik, végrehajtási beállítások, és hogyan számítja ki a Microsoft a felhasználását.

>[!IMPORTANT]
>Ez a cikk arra szolgál, hogy egy útmutató, amelyekkel jobban megértheti az Azure multi-factor Authentication megvásárlása különböző módokon lehet. Árak és számlázás kapcsolatos részleteket, mindig tekintse át a [multi-factor Authentication árképzést ismertető oldalra](https://azure.microsoft.com/pricing/details/multi-factor-authentication/).

## <a name="available-versions-of-azure-multi-factor-authentication"></a>Azure multi-factor Authentication elérhető verzió

A következő táblázat ismerteti a multi-factor authentication három verziója közötti különbségek:

| Verzió | Leírás |
| --- | --- |
| Multi-Factor Authentication az Office 365-höz |Ebben a verzióban kizárólag az Office 365 alkalmazásokkal működik, és kezeli a Office 365 portálról. A rendszergazdák [Office 365 erőforrásokat a kétlépéses ellenőrzéshez használttal biztonságos](https://support.office.com/article/Set-up-multi-factor-authentication-for-Office-365-users-8f0454b2-f51a-4d9c-bcde-2c48e41621c6). Ebben a verzióban az Office 365-előfizetéssel része. |
| A rendszergazdák az Azure többtényezős hitelesítést | Az Azure-bérlő globális rendszergazdái engedélyezheti a kétlépéses ellenőrzést, a globális rendszergazdai fiókokhoz, minden további költség nélkül.|
| Azure Multi-Factor Authentication | Más néven a "teljes" verzió, Azure multi-factor Authentication kínál képességek richest készletét. További konfigurációs beállítások keresztül biztosít a [a klasszikus Azure portálon](https://manage.windowsazure.com), speciális hibajelentési, és a helyszíni számos támogatása és a felhőalapú alkalmazásokhoz. Az Azure többtényezős hitelesítés az Azure Active Directory Premium (P1 és P2 tervek) és a nagyvállalati mobilitási + biztonsági (E3 és E5 tervek) része, és lehet telepíteni vagy [a felhőben vagy helyszínen](multi-factor-authentication-get-started.md). |

## <a name="feature-comparison-of-versions"></a>Szolgáltatások összehasonlítása verziók
A következő táblázat felsorolja az Azure multi-factor Authentication különböző verzióiban elérhető szolgáltatásokat.

> [!NOTE]
> Az összehasonlítási táblázat ismerteti a multi-factor Authentication verziói részét képező szolgáltatásokat. Ha a teljes Azure multi-factor Authentication szolgáltatással rendelkezik, néhány funkció nem áll rendelkezésre, attól függően, hogy használ-e [multi-factor Authentication a felhőben vagy a multi-factor Authentication helyszíni](multi-factor-authentication-get-started.md).


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

## <a name="how-to-get-azure-multi-factor-authentication"></a>Azure multi-factor Authentication beszerzése
Ha szeretné, hogy a teljes Azure multi-factor Authentication által kínált funkciót közzétevő, többféle módon:

### <a name="option-1---mfa-licenses"></a>1 - MFA-licencet. lehetőséget

Azure multi-factor Authentication licencet vásárolt, majd rendelje hozzá őket a felhasználók az Azure Active Directoryban. 

Ha ezt a beállítást használja, az Azure multi-factor Authentication hitelesítésszolgáltató csak akkor, ha is meg kell adnia a kétlépéses ellenőrzést licenccel nem rendelkező felhasználók kell létrehoznia. Ellenkező esetben akkor előfordulhat, hogy számlázható kétszer.

### <a name="option-2---bundled-licenses-that-include-mfa"></a>Kötegelt beállítás 2 - licencet, amely tartalmazza az MFA

Az licencet, amely tartalmazza az Azure multi-factor Authentication hitelesítés, mint például az Azure Active Directory Premium (P1 vagy P2) vagy nagyvállalati mobilitási + biztonsági (E3 vagy E5), majd rendelje hozzá őket a felhasználók az Azure Active Directoryban. 

Ha ezt a beállítást használja, az Azure multi-factor Authentication hitelesítésszolgáltató csak akkor, ha is meg kell adnia a kétlépéses ellenőrzést licenccel nem rendelkező felhasználók kell létrehoznia. Ellenkező esetben akkor előfordulhat, hogy számlázható kétszer. 

### <a name="option-3---mfa-consumption-based-model"></a>MFA-fogyasztás alapján modell 3 - beállítás

Hozzon létre egy Azure multi-factor Authentication hitelesítésszolgáltató belül Azure-előfizetéssel. Az Azure MFA-szolgáltatók számlázása a nagyvállalati szerződés, Azure előfizetési vagy hitelkártya, mint minden más Azure-erőforrások Azure-erőforrások. Ezek a szolgáltatók csak a teljes Azure-előfizetések, korlátozás nélkül Azure előfizetések, amelyek a költségkeret $0 hozhatók létre. Korlátozott előfizetések aktiválásakor licencek, mint 1. és 2-beállításokban jönnek létre. 

Az Azure multi-factor Authentication hitelesítésszolgáltató használata esetén van elérhető, amely az Azure-előfizetéshez keresztül számlázása két használati modellekről:  

1. **Felhasználónként** – a vállalatok számára, amely engedélyezi a kétlépéses ellenőrzést alkalmazottak, akik rendszeresen hitelesítési rögzített száma. Egyes felhasználók számlázási az Azure AD-bérlő és/vagy az Azure MFA kiszolgáló a multi-factor Authentication engedélyezett felhasználók számát alapul. Felhasználók engedélyezve vannak-e a multi-factor Authentication mindkét Azure AD-ben és az Azure MFA kiszolgáló, és a tartomány sync (az Azure AD Connect) engedélyezve van, majd azt a nagyobb mennyiségű felhasználók száma. Ha a tartomány szinkronizálás nincs engedélyezve, akkor azt az összegük az összes olyan felhasználó, az Azure AD MFA engedélyezett száma és az Azure MFA kiszolgáló. Számlázási arányosítva és a kereskedelmi rendszer jelentett naponta. 

  > [!NOTE]
  > Számlázási 1. példa: jelenleg engedélyezve van a multi-factor Authentication 5000 felhasználója van. A multi-factor Authentication rendszer elosztja ezt a számot 31 és jelentések 161.29 felhasználók napra vonatkozóan. Holnap engedélyezi 15 további felhasználókat, így a multi-factor Authentication rendszer 161.77 felhasználók jelentései adott napon. Az elszámolási időszak végéig szemben az Azure-előfizetéshez számlázott felhasználók teljes száma ad legfeljebb 5000 körül. 
  >
  > Számlázási 2. példa: rendelkezik licenccel rendelkező felhasználók és a felhasználók nélkül, így egy felhasználói Azure MFA-szolgáltatóra különbség fel kell. Nincsenek 4,500 nagyvállalati mobilitási + biztonsági licencek a tenant, de 5000 felhasználója van engedélyezve a többtényezős Hitelesítést. Az Azure-előfizetéshez fizetnie kell 500 felhasználónak, arányosítva, és naponta jelentett 16.13 felhasználóként. 

2. **Hitelesítésenként** – a vállalatok számára, amely engedélyezi a felhasználók, akik ritkán hitelesítési nagyszámú kétlépéses ellenőrzést. Számlázási alapul fogadja az Azure MFA felhőalapú szolgáltatás, függetlenül attól, hogy ezen ellenőrzések sikeres legyen, vagy a rendszer megtagadja a kétlépéses ellenőrzés kérelmek számát jelenti. A számlázási jelenik meg az Azure használati utasítás csomagokban 10 hitelesítést, és a bejelentések a Commerce rendszer naponta. 

  > [!NOTE]
  > Számlázási 3. példa: jelenleg az Azure MFA szolgáltatással 3,105 kétlépéses ellenőrzés kérelem érkezett. Az Azure-előfizetéshez 310.5 hitelesítési csomagok lesz számlázva. 

Fontos megjegyezni, hogy is az Azure MFA licenccel rendelkezik, de továbbra is fizetnie első használat alapú konfigurálást. Ha beállította a hitelesítés Azure MFA-szolgáltatóra, kell fizetni összes kétlépéses ellenőrzés kérés még az licenccel rendelkező felhasználók végezhető el. Ha olyan tartományhoz, amely nem kapcsolódik az Azure AD-bérlő beállított felhasználói Azure MFA-szolgáltatóra, kell fizetni engedélyezett felhasználónkénti akkor is, ha a felhasználók Azure ad-val licenccel rendelkezik. 

## <a name="next-steps"></a>Következő lépések

- További árképzési részletekért lásd: [Azure MFA szolgáltatás díjszabása](https://azure.microsoft.com/pricing/details/multi-factor-authentication/).

- Válassza ki, hogy az Azure MFA telepítendő [a felhőben, vagy a helyszíni](multi-factor-authentication-get-started.md)