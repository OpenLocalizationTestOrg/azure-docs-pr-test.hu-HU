---
title: "Azure AD Connect szinkronizálása: technikai fogalmak |} Microsoft Docs"
description: "Ismerteti az Azure AD Connect szinkronizálási szolgáltatás hello műszaki elveit."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 731cfeb3-beaf-4d02-aef4-b02a8f99fd11
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: markvi;andkjell
ms.openlocfilehash: c6309bb9be462fb3d49c5b6ab302d4327ce4b7be
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-technical-concepts"></a>Az Azure AD Connect szinkronizálása: technikai kulcsfogalmak
Ez a cikk hello témakör összegzését [ismertetése architektúra](active-directory-aadconnectsync-technical-concepts.md).

Az Azure AD Connect szinkronizálási szolgáltatás egy teli metakönyvtár szinkronizálási platformra épít.
a következő szakaszok hello metakönyvtár szinkronizálási hello fogalmak vezethet.
MIIS ILM és FIM épít, hello Azure Active Directory szinkronizálási szolgáltatások platformot kínál a hello következő csatlakozó toodata adatforrások, adatforrások, valamint hello kiépítésének és megszüntetésének biztosítása identitások közötti szinkronizálása.

![Műszaki fogalmak](./media/active-directory-aadconnectsync-technical-concepts/scenario.png)

a következő szakaszok hello hello hello FIM szinkronizálási szolgáltatás aspektusainak a következő további részleteket biztosítanak:

* összekötő
* Attribútumfolyam
* Összekötőtér
* Metaverzum
* Kiépítés

## <a name="connector"></a>összekötő
hello kódú modulokat, amelyek egy csatlakoztatott könyvtárral használt toocommunicate nevezzük összekötők (korábbi nevén kezelőügynökök (MAs)).

Hello futtató Azure AD Connect szinkronizálási szolgáltatás van telepítve. hello összekötők hello ügynök nélkül képes tooconverse hello speciális ügynökök telepítése ahelyett távoli rendszer protokollok használatával adja meg. Ez azt jelenti, csökkentheti kockázat és üzembe helyezési idők, különösen akkor, ha kritikus fontosságú alkalmazások és rendszerek foglalkoznak.

A fenti hello kép hello összekötő azonos a hello kapcsolódási térbe, de magában foglalja a hello külső rendszer összes kommunikációt.

hello összekötő felelős összes importálása és exportálása funkció toohello rendszer és felszabadítja a fejlesztők számára nem szükséges toounderstand hogyan tooconnect tooeach rendszer natív módon deklaratív létesítési toocustomize adatátalakítást használatakor.

Importálja és exportálja csak fordulhat elő, amikor ütemezett, lehetővé teszi a további változások hello rendszer, mivel a módosítások nem terjesztése automatikusan toohello csatlakoztatott adatforrás a szigetelése. Ezenkívül a fejlesztők is hozzon létre kapcsolódás toovirtually minden adatforrás saját összekötőket.

## <a name="attribute-flow"></a>Attribútumfolyam
hello metaverse az összevont hello nézet az összes csatlakoztatott identitások a szomszédos összekötőterek. Hello a fenti ábrán az Attribútumfolyam kapcsolaton keresztül a bejövő és kimenő áramlás nyílhegy írja le. Attribútumfolyam hello folyamat másolása, vagy egy rendszer tooanother származó adatok átalakítását, és az összes attribútum adatfolyamok (bejövő vagy kimenő).

Attribútumfolyam között történik hello kapcsolódási térbe és hello metaverse kétirányúan ütemezett toorun szinkronizálási (teljes vagy különbözeti) műveletek esetén.

Attribútumfolyam csak akkor következik be, ezeket a szinkronizálások futtatásakor. Szinkronizálási szabályok attribútumfolyamok vannak definiálva. Ezek lehetnek bejövő (ISR hello képen látható a fenti) vagy kimenő (OSR hello képen látható a fenti).

## <a name="connected-system"></a>Csatlakoztatott rendszer
Csatlakoztatott rendszer (más néven csatlakoztatott címtárhoz) hivatkozik a távoli rendszer toohello Azure AD Connect szinkronizálási szolgáltatás tooand olvasása és írása az azonosító adatok tooand csatlakozott.

## <a name="connector-space"></a>Összekötőtér
Minden csatlakoztatott adatforrás hello objektumokat és attribútumokat hello kapcsolódási térbe szűrt részhalmazának jelzi.
Ez lehetővé teszi, hogy hello szinkronizálási szolgáltatás toooperate helyileg hello kell toocontact hello távoli rendszer nélkül hello objektumok szinkronizálásakor és interakció tooimports korlátozza, és csak exportálja.

Ha hello adatforrás és hello összekötő hello képességét tooprovide módosítások (különbözeti importálás) listáját, majd hello növekszik a tartomány működési hatékonyság jelentősen, csak hello legutóbbi lekérdezés óta történt változások ciklus lesznek cserélve. hello kapcsolódási térbe insulates hello csatlakoztatott adatforrás változások propagálása automatikusan igénylő hello összekötő ütemezésnek importálja és exportálja. A hozzáadott biztosítási biztosít, nyugodt maradhat afelől, tesztelési, megtekintés, vagy megerősítő hello következő frissítés közben.

## <a name="metaverse"></a>Metaverzum
hello metaverse az összevont hello nézet az összes csatlakoztatott identitások a szomszédos összekötőterek.

Identitások egymáshoz kapcsolódó és hatóság különböző attribútumok Attribútumfolyam-megfeleltetéseket importálási keresztül van hozzárendelve, hello központi metaverzum-objektum megkezdése több rendszer tooaggregate adatait. Az az objektum Attribútumfolyam leképezései továbbítani az információs toooutbound rendszerek.

Objektumok jönnek létre, amikor egy mérvadó rendszer hello metaverzumba projektek őket. Amint az összes kapcsolat törlődik, hello metaverzum-objektum törlődik.

Hello metaverzumban található objektumok közvetlenül nem szerkeszthető. Minden adat hello objektumban kell hozzájárult, Attribútumfolyam keresztül. hello metaverse tart fenn minden kapcsolódási térbe állandó összekötőt. Az összekötők újrakiértékelési nem igényel minden egyes szinkronizálás futtatása. Ez azt jelenti, hogy az Azure AD Connect szinkronizálási szolgáltatás toolocate hello egyező távoli objektum nincs minden alkalommal. Így nem szükséges költséges ügynökök tooprevent módosítások tooattributes általában felelős válaszoknak az összekapcsolása hello objektumok hello szükségességét.

Új adatforrásokat, amelyek egy folyamat rendelkezhetnek toobe felügyelt, az Azure AD Connect sync által használt igénylő elérésű, korábban létező objektumok felderítése meghívásakor egy illesztési szabály tooevaluate lehetséges jelöltek mely tooestablish a hivatkozást.
Miután hello kapcsolat létrejött, az értékelés indítást, és normál Attribútumfolyam hello távoli csatlakoztatott adatforrás és hello metaverse kerül sor.

## <a name="provisioning"></a>Kiépítés
Ha egy mérvadó forrás projektek hello metaverzumba összekötő terület új objektumot új objektumot képviselő egy alsóbb rétegbeli csatlakoztatott adatforrás egy másik összekötő is létrehozható.

Ez az eredendően kapcsolatot hoz létre, és Attribútumfolyam kétirányúan lépne.

Amikor egy szabály határozza meg, hogy egy új összekötő terület objektum kell-e létre toobe, kiépítés nevezik. Azonban ez a művelet csak akkor történik meg hello kapcsolódási térbe belül, mert az nem jelenik meg a csatlakoztatott adatforrás hello mindaddig, amíg az exportálás történik.

## <a name="additional-resources"></a>További források
* [Az Azure AD Connect-szinkronizálás: Szinkronizálási beállítások testreszabása](active-directory-aadconnectsync-whatis.md)
* [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md)

<!--Image references-->
[1]: ./media/active-directory-aadsync-technical-concepts/ic750598.png
