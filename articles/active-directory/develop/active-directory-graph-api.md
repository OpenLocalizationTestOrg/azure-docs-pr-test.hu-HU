---
title: Active Directory Graph API aaaAzure |} Microsoft Docs
description: "A Graph API, amely lehetővé teszi a programozott hello egy áttekintése és a gyors üzembe helyezési útmutatója tooAzure AD REST API-végpontokon keresztül férnek hozzá."
services: active-directory
documentationcenter: 
author: viv-liu
manager: mbaldwin
editor: mbaldwin
ms.assetid: 5471ad74-20b3-44df-a2b5-43cde2c0a045
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/27/2017
ms.author: viviali
ms.custom: aaddev
ms.openlocfilehash: cde1dd86b0ca1dc24a5b46dc578b6245ba98751f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-graph-api"></a>Azure Active Directory – Graph API
> [!IMPORTANT]
> Határozottan javasoljuk, hogy használjon [Microsoft Graph](https://graph.microsoft.io/) helyett az Azure AD Graph API tooaccess Azure Active Directory-erőforrásokra. A fejlesztési energiáinkat mostantól a Microsoft Graph-ra koncentráljuk, az Azure AD Graph API-hoz nem tervezünk további fejlesztéseket. Nagyon korlátozott számú forgatókönyvek, amelyek az Azure AD Graph API továbbra is megfelelő lehet; További információkért lásd: hello [Microsoft Graph vagy hello Azure AD Graph](https://dev.office.com/blogs/microsoft-graph-or-azure-ad-graph) hello Office fejlesztői központban a blogbejegyzést.
> 
> 

hello Azure Active Directory Graph API-val programozott hozzáférés tooAzure AD REST API-végpontokon keresztül biztosít. Alkalmazások használhatják hello Graph API tooperform létrehozása, olvasása, frissítése és Törlés (CRUD) típusú műveletek directory adatok és objektumok. Például hello Graph API támogatja a következő gyakori műveletek a felhasználói objektum hello:

* Új felhasználó létrehozása a könyvtárban található
* A felhasználó részletes tulajdonságait, például saját csoport
* A felhasználó tulajdonságai, például a hely és a telefonszámot, vagy módosítania kell a jelszavát
* Ellenőrizze a felhasználói csoport tagságát a szerepköralapú hozzáférés
* Egy felhasználói fiókot, vagy a teljes törlése

Továbbá toouser objektumok más objektumok, például a csoportok és alkalmazások hasonló műveleteket hajthat végre. Graph API hello alkalmazás könyvtárban regisztrálni kell az Azure ad-vel és toocall hello tooallow hozzáférés toohello directory konfigurálva. Ez általában egy felhasználó vagy rendszergazda hozzájárulási folyamat keresztül érhető el.

toobegin használatával hello Azure Active Directory Graph API, lásd: hello [Graph API gyors üzembe helyezési útmutató](active-directory-graph-api-quickstart.md), vagy a nézet hello [interaktív Graph API-referenciadokumentáció](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).

## <a name="features"></a>Szolgáltatások
hello Graph API hello a következő szolgáltatásokat nyújtja:

* **REST API-végpontok**: hello Graph API-val egy olyan szabványos HTTP-kérelmek használatával elért végpontokat áll RESTful szolgáltatás. Graph API hello kérések és válaszok XML- vagy Javascript Object Notation (JSON) tartalom típusokat támogatja. További információkért lásd: [az Azure AD Graph REST API-referencia](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).
* **Az Azure AD hitelesítési**: minden kérelem toohello Graph API-val egy JSON webes jogkivonat (JWT) hello engedélyezési hello kérelem fejlécében hozzáfűzésével hitelesíteni kell. Ez a token szerzi így token-végpont egy kérelem tooAzure AD meg, és adja meg érvényes hitelesítő adatokat. Hello engedélyezési kód engedélyezése folyamat tooacquire egy grafikonon token toocall hello vagy hello OAuth 2.0 ügyfél-hitelesítő adatok folyamata is használhatja. További információ [OAuth 2.0, az Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx).
* **Szerepkör alapú engedélyezési (RBAC)**: biztonsági csoportok állnak használt tooperform RBAC a hello Graph API-val. Például ha toodetermine azt szeretné, hogy a felhasználó rendelkezik-e hozzáférési tooa adott erőforrás, hello alkalmazás meghívhatja hello [csoporttagság ellenőrzése (tranzitív)](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/groups-operations#FunctionsandactionsongroupsCheckmembershipinaspecificgrouptransitive) művelet, amely igaz vagy hamis értéket ad vissza.
* **Különbözeti lekérdezés**: Ha toocheck között két időszakok anélkül, hogy toomake könyvtár változása lekérdezések toohello Graph API-t gyakran használják az, hogy különbözeti kérést. A kérelem típusa csak hello módosítások hello előző különbözeti lekérdezés kérelem és a jelenlegi kérelem hello között adja vissza. További információkért lásd: [az Azure AD Graph API különbözeti lekérdezés](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-differential-query).
* **Címtárbővítmények**: Ha egy alkalmazás, amelyet a directory-objektumok tooread vagy írási egyedi tulajdonságok fejleszt, regisztrálhat és bővítmény értékek használata hello Graph API segítségével. Például ha az alkalmazás egy Skype-azonosító tulajdonságot minden olyan felhasználóhoz, hello új tulajdonság hello directory regisztrálhatja, és a minden felhasználói objektum elérhető lesz. További információkért lásd: [az Azure AD Graph API Directory-séma bővítményei](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions).
* **Védi-engedélyhatókörök**: AAD Graph API-val tesz elérhetővé, amelyek lehetővé teszik az adatok a biztonságos/átadni kívánt hozzájárult e hozzáférés tooAAD, és támogatja az ügyfél típusú alkalmazás, beleértve a különböző engedélyhatókörök:
  
  * egy felhasználói felületet, amely delegált adta rendelkező toodata engedélyezési keresztül elérhető hello bejelentkezett felhasználónak (delegált)
  * használó alkalmazás-szerepkörön alapuló hozzáférés-vezérlő megadásához például szolgáltatás/démon ügyfelek (app-szerepkörök)
    
    Mindkét meghatalmazott, és alkalmazás szerepkör engedélyhatókörök hello Graph API által elérhetővé tett jogosultság jelöl, és regisztrációs Alkalmazásengedélyek keresztül ügyfélalkalmazások kérhető [hello Azure-portálon szolgáltatásai](https://portal.azure.com). Ügyfelek ellenőrizheti hello engedélyhatókörök toothem kap hello hatókör ("szolgáltatáskapcsolódási pont") jogcím delegált jogosultságokkal sikeresen telepítették hello hozzáférési jogkivonatot kapott vizsgálatával, és az Alkalmazásengedélyek szerepkör jogcím hello szerepkörök ("szerepkörök"). További információ [az Azure AD Graph API-Engedélyhatókörök](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes).

## <a name="scenarios"></a>Forgatókönyvek
hello Graph API lehetővé teszi, hogy az alkalmazás-forgatókönyvek. a következő forgatókönyvek hello leggyakoribb hello:

* **Üzletági (Egybérlős) alkalmazás**: Ebben a forgatókönyvben egy vállalati fejlesztői működik egy olyan szervezet, amely olyan Office 365-előfizetéssel rendelkezik. hello fejlesztői fejleszt, amely együttműködik az Azure AD tooperform feladatok például hozzárendelése a licenc tooa felhasználó webalkalmazás. Ez a feladat hozzáférés toohello Graph API-val, így hello fejlesztői hello egybérlős alkalmazás regisztrálása az Azure ad-ben, és konfigurálja az olvasási és írási engedéllyel hello Graph API szükséges. Majd az alkalmazás hello konfigurált toouse vagy saját hitelesítő adatait, vagy azokat hello jelenleg bejelentkezési felhasználói tooacquire token toocall hello Graph API-val.
* **(Több-Bérlős) szolgáltatás alkalmazás**: Ebben a forgatókönyvben egy független szoftverszállító (ISV) által fejlesztett üzemeltetett több-bérlős webalkalmazás, amely más Azure AD használó szervezetek felhasználói felügyeleti funkciókat biztosít. Ezek a szolgáltatások hozzáférés toodirectory objektumhoz igényel, ezért az így hello alkalmazás toocall hello Graph API-val. hello fejlesztői hello alkalmazás regisztrálása az Azure ad-ben, konfigurálja toorequire olvasási és írási engedéllyel a Graph API hello és majd lehetővé teszi, hogy külső hozzáférés úgy, hogy más szervezetek is hozzájárul a címtár toouse hello alkalmazás. Amikor egy felhasználó egy másik szervezet toohello alkalmazás hello először hitelesíti, megtekinthető a hozzájárulási párbeszédpanel hello alkalmazás hello engedélyekkel.  Hozzájárulási adjon hello alkalmazás megadása a kért engedélyek toohello Graph API hello felhasználói könyvtárba. Hello hozzájárulási keretrendszerre további információkért lásd: [hello hozzájárulás keretrendszer áttekintése](active-directory-integrating-applications.md).

## <a name="see-also"></a>Lásd még:
[Az Azure AD Graph API gyors üzembe helyezési útmutató](active-directory-graph-api-quickstart.md)

[AD Graph REST-dokumentáció](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)

[Az Azure Active Directory fejlesztői útmutatója](active-directory-developers-guide.md)

