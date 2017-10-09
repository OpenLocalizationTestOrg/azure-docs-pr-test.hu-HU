---
title: "mentése az Azure mikroszolgáltatások folyamatos integrációt aaaSet |} Microsoft Docs"
description: "Hogyan áttekintést kaphat tooset folyamatos integrációt és a Visual Studio Team Services (VSTS) használatával a Service Fabric-alkalmazás központi telepítését."
services: service-fabric
documentationcenter: na
author: mthalman-msft
manager: timlt
editor: 
ms.assetid: 3e8c2290-9e7a-456a-9b2c-db44d1b3988d
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 12/06/2016
ms.author: mthalman;mikhegn
ms.openlocfilehash: 041e081f4a42f379394f2d21f07db4f6de045b24
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-service-fabric-continuous-integration-and-deployment-with-visual-studio-team-services"></a>A Service Fabric folyamatos integrációt és telepítést a Visual Studio Team Services beállítása
Ez a cikk ismerteti a hello lépéseket tooset folyamatos integrációt és az Azure Service Fabric-alkalmazás központi telepítése a Visual Studio Team Services (VSTS) használatával.

Ez a dokumentum tükrözi hello aktuális eljárás és várt toochange adott idő alatt.

## <a name="prerequisites"></a>Előfeltételek
tooget indult el, tegye a következőket:

1. Győződjön meg arról, hogy rendelkezik-e hozzáférési tooa Team Services-fiók vagy [hozzon létre egyet](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services) magát.
2. Győződjön meg arról, hogy rendelkezik-e hozzáférési tooa Team Services csapatprojekt vagy [hozzon létre egyet](https://www.visualstudio.com/docs/setup-admin/create-team-project) magát.
3. Győződjön meg arról, hogy rendelkezik-e a Service Fabric-fürt toowhich Telepítse központilag az alkalmazást, vagy hozzon létre egyet hello segítségével [Azure-portálon](service-fabric-cluster-creation-via-portal.md), egy [Azure Resource Manager sablon](service-fabric-cluster-creation-via-arm.md), vagy [Visual Studio ](service-fabric-cluster-creation-via-visual-studio.md).
4. Győződjön meg arról, hogy már létrehozta a Service Fabric-alkalmazás (.sfproj) projektben. Rendelkeznie kell egy projekt létrehozott vagy frissített a Service Fabric SDK 2.1-es vagy újabb rendszerre (hello .sfproj fájl tartalmaznia kell egy ProjectVersion tulajdonság értéke 1.1-es vagy magasabb).

> [!NOTE]
> Egyéni build ügynökök már nem szükségesek. Team Services üzemeltetett ügynökök most előtelepített Service Fabric-fürt felügyeleti szoftver lehetővé teszi a közvetlenül az adott ügynökök az alkalmazások központi telepítését.
> 
> 

## <a name="configure-and-share-your-source-files"></a>Konfigurálja és a forrás-fájlok megosztása
hello elsőként azt szeretné, hogy a rendszer toodo közzétételi profilt előkészítése hello központi telepítési folyamat, amely végrehajtja a Team Services belül a használatát.  a közzétételi profil hello konfigurált tootarget hello fürt, amely korábban előkészítése után kell lennie:

1. Adja meg a projekt belül közzétételi profilt, hogy toouse a folyamatos integrációt munkafolyamathoz. Hajtsa végre a hello [utasításokat közzététele](service-fabric-publish-app-remote-cluster.md) hogyan toopublish az alkalmazás tooa távoli fürtöt. Nem ténylegesen szükséges toopublish az alkalmazást, ha. Kattinthat a hello **mentése** hello hivatkozás közzététele párbeszédpanelen után dolgot megfelelően van konfigurálva.
2. Ha azt szeretné, hogy az alkalmazás toobe az egyes központi telepítések Team Services belül előforduló frissítve, tooconfigure hello kívánt profil tooenable frissítés közzététele. Az 1. lépésben használt párbeszédpanel azonos közzététele hello, győződjön meg arról, hogy hello **frissítési hello alkalmazás** jelölőnégyzet be van jelölve.  További információ [további frissítési beállítások konfigurálása](service-fabric-visualstudio-configure-upgrade.md). Kattintson a hello **mentése** hivatkozás toosave hello beállítások toohello közzétételi profil.
3. Ellenőrizze, hogy profil mentett toohello közzé a módosításokat, majd szakítsa meg a hello közzétételére párbeszédpanelt.
4. Most azt is túl[megosztani az alkalmazás projekt forrásfájljait](https://www.visualstudio.com/docs/setup-admin/team-services/connect-to-visual-studio-team-services#vs) Team Services. Miután a forrásfájlok Team Services érhető el, most már továbbléphet a következő lépésben toohello buildek generálására. 

## <a name="create-a-build-definition"></a>A build definíció létrehozása
Team Services build definícióját egy munkafolyamatot, amelyik build ismertetett lépések egymás után végrehajtott áll ismerteti. hello hello build-definíciót, amely létrehozásakor célja a Service Fabric alkalmazáscsomagok tooproduce és más összetevők, felhasznált toodeploy hello alkalmazás lehet. További tudnivalók Team Services [definíciók build](https://www.visualstudio.com/docs/build/define/create).

### <a name="create-a-definition-from-hello-build-template"></a>A definíció létrehozása hello build sablonból
1. Nyissa meg a csoport projektet a Visual Studio Team Services.
2. Jelölje be hello **Build** fülre.
3. Jelölje be hello zöld  **+**  toocreate egy új build definition aláírásához.
4. A megnyíló hello párbeszédpanelen válassza ki **Azure Service Fabric-alkalmazás** belül hello **Build** sablon kategóriát.
5. Válassza ki **következő**.
6. Válassza ki a hello tárház és a Service Fabric-alkalmazás társított ág.
7. Válassza ki a hello ügynök várólista toouse kívánja. Bérelt ügynökök támogatottak.
8. Kattintson a **Létrehozás** gombra.
9. Menteni hello build definícióját, és adjon meg egy nevet.
10. hello következő bekezdés hello build lépéseket hello sablon által létrehozott leírása:

| Összeállítása lépés | Leírás |
| --- | --- |
| NuGet visszaállítása |Visszaállítja a NuGet-csomagok hello hello megoldás. |
| Megoldás \*.sln |Hello teljes megoldás alkot. |
| Megoldás \*.sfproj |Hello Service Fabric alkalmazáscsomagot, amely használt toodeploy hello alkalmazást hoz létre. hello alkalmazás csomaghely megadott toobe hello build összetevő címtáron belül. |
| Service Fabric-alkalmazás verziója frissítése |Hello alkalmazás csomag jegyzékfájlt tooallow frissítési támogatásához szereplő hello verzióértékek frissíti. Lásd: hello [feladat dokumentációs oldal](https://go.microsoft.com/fwlink/?LinkId=820529) további információt. |
| Fájlok másolása |Másolatok hello-profil és a központi telepítéshez használt alkalmazás paraméterek fájlok toohello build tartozó összetevők toobe közzététele. |
| Összetevő közzététele |Hello build összetevők közzéteszi. Lehetővé teszi, hogy a kiadás definition tooconsume hello build összetevők. |

### <a name="verify-hello-default-set-of-tasks"></a>Ellenőrizze a tevékenységek alapértelmezett készlete hello
1. Ellenőrizze hello **megoldás** hello beviteli mezőt **NuGet visszaállítási** és **megoldás** build lépéseket.  Alapértelmezés szerint ezek build lépések végrehajtása után a társított hello tárházban található összes megoldásfájlok.  Ha csak hello build definition toooperate egyik megoldás fájlokhoz, tooexplicitly frissítés hello elérési toothat fájl szükséges.
2. Ellenőrizze a hello **megoldás** hello beviteli mezőt **alkalmazás becsomagolása** összeállítása lépés.  Alapértelmezés szerint a build lépés azt feltételezi, hogy hello tárház létezik csak egy Service Fabric-alkalmazás projekt (.sfproj).  Ha több fájlt a tárházban, és szeretné, hogy csak az egyiket tootarget a build definíció, tooexplicitly frissítés hello elérési toothat fájl szükséges.  Ha több alkalmazás projektek toopackage a tárházban, meg kell toocreate további **Visual Studio Build** hello build meghatározása, hogy minden érintett alkalmazás projektben lévő lépéseket.  Ezután is kellene tooupdate hello **MSBuild-argumentumok** mező az összes build lépéseket, hogy hello csomaghely nézve egyedi.
3. Ellenőrizze hello versioning viselkedés hello definiált **Service Fabric App verziók frissítése** összeállítása lépés.  Alapértelmezés szerint ez a lépés build hello build tooall verzió számértékeit hello alkalmazás csomag jegyzékfájlt fűzi hozzá. Lásd: hello [feladat dokumentációs oldal](https://go.microsoft.com/fwlink/?LinkId=820529) további információt. Ez akkor hasznos, támogatásához az alkalmazás frissítését, mivel minden egyes frissítés telepítéséhez szükséges különböző verzióértékek hello előző telepítéséből. Ha a munkafolyamat nem szándékos folyamatban volt toouse az alkalmazásfrissítés, akkor fontolja meg a build lépés letiltása. Le kell tiltani Ha tooproduce build használt toooverwrite egy meglévő Service Fabric-alkalmazás is lehet. hello központi telepítés sikertelen lesz, ha hello build által előállított hello alkalmazás hello verziója nem egyezik meg a hello alkalmazás hello fürt hello verzióját.
4. Ha a megoldás a .NET Core projektet tartalmaz, győződjön meg róla, hogy a build definícióját tartalmazza-e a build lépést, amely visszaállítja az hello függőségek:
   1. Válassza ki **összeállítása lépés hozzáadása...** .
   2. Keresse meg a hello **parancssori** hello segédprogram lapon belül feladatot, és kattintson a Hozzáadás gombra.
   3. Hello feladat beviteli mezőbe a következő értékek hello használata:
   4. Eszköz: dotnet
   5. Argumentumok: visszaállítása
   6. Húzza a hello feladatot úgy, hogy közvetlenül a hello után **NuGet visszaállítási** lépés.
5. Mentse a módosításokat végzett toohello build definíciója.

### <a name="try-it"></a>Kipróbálom
Válassza ki **várólista létrehozása** toomanually build elindításához. Épít is eseményindítók leküldéses vagy be. Miután ellenőrizte, hogy sikeresen végrehajtja az, hogy hello build, most már továbbléphet az toodefining olyan kiadási definíciót, amely az alkalmazás tooa fürt telepíti.

## <a name="create-a-release-definition"></a>Egy kiadási definíció létrehozása
Team Services kiadás definíció olyan munkafolyamatot, amely a feladatok egymás után végrehajtott készlete áll ismerteti. hello cél hello kiadás definíció létrehozása tootake egy alkalmazáscsomagot, és tooa fürtök telepítése. Együttes használatuk esetén a hello definition készítse el és kiadás definition hello teljes munkafolyamat is végre forrás fájlok tooending a fürtön futó alkalmazással kezdve. További tudnivalók Team Services [definíciók kiadási](https://www.visualstudio.com/docs/release/author-release-definition/more-release-definition).

### <a name="create-a-definition-from-hello-release-template"></a>A definíció létrehozása hello kiadási sablon
1. Nyissa meg a projektet a Visual Studio Team Services.
2. Jelölje be hello **kiadás** fülre.
3. Válassza ki a zöld hello  **+**  toocreate új kiadási definíció aláírásához, és válassza ki **létrehozás kiadás definition** hello menüben.
4. A megnyíló hello párbeszédpanelen jelölje ki **Azure Service Fabric telepítési** belül hello **telepítési** sablon kategória.
5. Válassza ki **következő**.
6. Válassza ki a kívánt toouse hello forrásaként, ez kiadás definíció hello build definition.  hello kiadás definition hivatkozások hello összetevőket, amelyeket a kijelölt hello által definíció létrehozása.
7. Ellenőrizze a hello **folyamatos üzembe helyezés** jelölőnégyzetet, ha toohave Team Services automatikusan hozzon létre egy új kiadási és hello Service Fabric-alkalmazás központi telepítése, amikor build befejeződik.
8. Válassza ki a hello ügynök várólista toouse kívánja. Bérelt ügynökök támogatottak.
9. Kattintson a **Létrehozás** gombra.
10. Hello definíció neve szerkesztése hello oldal hello tetején hello ceruza ikonra kattintva.
11. Jelöljön ki kell telepíteni, az alkalmazás hello fürt toowhich hello **Fürtkapcsolat** hello feladat beviteli mezőt. hello fürtkapcsolat hello lehetővé teszi, hogy a hello telepítési feladat tooconnect toohello fürt szükséges információkat biztosít. Ha még nincs egy fürt kapcsolatban a fürt számára, jelölje be a hello **kezelése** hivatkozás tovább toohello mező tooadd egyet. A megnyíló lapon hello hajtsa végre a lépéseket követve hello:
    1. Válassza ki **új szolgáltatási végpont** majd **Azure Service Fabric** hello menüből.
    2. Válassza ki a végpont által megcélzott hello fürt által használt hitelesítési hello típusát.
    3. Adjon meg egy nevet a kapcsolathoz hello **kapcsolatnév** mező.  Általában használja a fürt hello neve.
    4. Hello ügyfél kapcsolat végponti URL-Címének definiálása a hello **a fürt végpontja** mező.  Példa: tcp://contoso.westus.cloudapp.azure.com:19000.
    5. Az Azure Active Directory hitelesítő adatok, adja meg a hello hitelesítő adatok toouse tooconnect toohello fürt a hello **felhasználónév** és **jelszó** mezőket.
    6. A tanúsítványalapú hitelesítéshez, adja meg a hello Base64 kódolás hello ügyfél tanúsítványfájl hello **ügyféltanúsítvány** mező.  Hello súgó előugró ablak megjelenik, hogy a mező kapcsolatos információk, amelyek érték tooget.  Ha a tanúsítvány jelszóval védett, meghatározása hello jelszó hello **jelszó** mező.
    7. A módosítások megerősítése kattintva **OK**. Navigálás vissza tooyour után definition kiadási, kattintson hello frissítési ikonjára hello **Fürtkapcsolat** mező toosee hello végpont az előzőekben adott hozzá.
12. Hello kiadás definíció mentése.

> [!NOTE]
> Microsoft Accounts (például @hotmail.com vagy @outlook.com) Azure Active Directory-hitelesítés használata nem támogatott.
> 
> 

hello definition létrehozott áll egy feladat típusú **Service Fabric-alkalmazás központi telepítésének**. Lásd: hello [feladat dokumentációs oldal](https://go.microsoft.com/fwlink/?LinkId=820528) Ez a feladat további információt.

### <a name="verify-hello-template-defaults"></a>Hello sablon alapértelmezett ellenőrzése
1. Ellenőrizze a hello **közzététele profil** hello beviteli mezőt **Fabric-alkalmazás központi telepítése** feladat. Alapértelmezés szerint ez a mező hello build összetevők szereplő Cloud.xml nevű közzétételi profilt hivatkozik. Ha azt szeretné, hogy tooreference különböző közzétételi profilt, vagy ha hello build tartalmazza az összetevői a több alkalmazáscsomagot, megfelelően kell tooupdate hello elérési útja.
2. Ellenőrizze a hello **alkalmazáscsomag** hello beviteli mezőt **Fabric-alkalmazás központi telepítése** feladat. Alapértelmezés szerint ez a mező hello alapértelmezett alkalmazás csomag használt elérési utat az hello build sablon hivatkozik.  Ha módosította a hello alapértelmezett csomag elérési útja a hello létrehozása a definíciós, szükséges tooupdate hello elérési megfelelően itt is.

### <a name="try-it"></a>Kipróbálom
Válassza ki **létrehozása kiadási** hello a **kiadási** gomb menü toomanually verziót létrehoznia. Megnyíló hello párbeszédpanelben válassza ki a kívánt toobase hello kiadás, és kattintson a hello build **létrehozása**. Ha engedélyezte a folyamatos üzembe helyezés, kiadásokban létrehozza automatikusan hello kapcsolódó összeállítási definition build befejezéséről.

## <a name="next-steps"></a>Következő lépések
További információk a Service Fabric-alkalmazások, olvassa el a következő cikkek hello folyamatos integrációt toolearn:

* [Otthoni csoport szolgáltatások dokumentációja](https://www.visualstudio.com/docs/overview)
* [A Team Services management létrehozása](https://www.visualstudio.com/docs/build/overview)
* [Team Services szolgáltatásban](https://www.visualstudio.com/docs/release/overview)

