---
title: "aaaAzure Service Fabric ajánlott biztonsági eljárások |} Microsoft Docs"
description: "Ez a cikk ajánlott eljárások az Azure Service Fabric biztonsági biztosít."
services: security
documentationcenter: na
author: unifycloud
manager: swadhwa
editor: tomsh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/04/2017
ms.author: tomsh
ms.openlocfilehash: 483a21240da17d56bb4641653093ddcbad379d6a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-service-fabric-security-best-practices"></a>Az Azure Service Fabric ajánlott biztonsági eljárások
Gyors, egyszerű és költséghatékony Azure-alkalmazás központi telepítése. Éles hasznos toohave felhő alkalmazás telepítése előtt a bevált gyakorlat tooassist kiértékelésekor listáját az alapvető és ajánlott gyakorlati tanácsok az alkalmazást.

Az Azure Service Fabric egy elosztottrendszer platform, így könnyen toopackage, telepítéséhez és felügyeletéhez a méretezhető és megbízható mikroszolgáltatások létrehozására. A Service Fabric is megoldást hello jelentős kihívásaira fejlesztésének és kezelésének a felhőalapú alkalmazásokhoz. A fejlesztők és a rendszergazdák elkerülhetik az infrastruktúrával kapcsolatos összetett problémákat, és a kritikus fontosságú, nagy erőforrás-igényű, skálázható, megbízható és felügyelhető számítási feladatok megvalósítására koncentrálhatnak. 

A minden ajánlott eljárás az hogy ismertetik:

-   Milyen hello ajánlott eljárás
-   Miért érdemes tooenable, hogy a legjobb
-   Mi lehet hello eredmény, ha Ön nem tooenable hello ajánlott
-   Hogyan szerezhet tooenable hello ajánlott

Jelenleg tudunk hello Azure Service Fabric ajánlott biztonsági eljárások a következő:

-   Sablon használata Azure Resource Manager (ARM) és a Service Fabric Azure PowerShell-modul toocreate biztonságos fürt
-   X.509-tanúsítványokat használ
-   Biztonsági szabályzatok konfigurálása
-   Megbízható szereplője biztonsági konfiguráció
-   Az SSL konfigurálása az Azure Service Fabric
-   Hálózati elkülönítési/biztonságot, az Azure Service Fabric
-   A kulcstároló, a biztonság beállítása
-   Felhasználók tooroles hozzárendelése


## <a name="best-practices-for-securing-your-cluster"></a>Ajánlott eljárások a fürt védelme

**Nagy vonalakban tekinti**

Mindig használjon biztonságos fürtöt
-   A fürt biztonsági, – használja a tanúsítványok
-   Ügyfél-hozzáférési (Admin és csak olvasható) – használja az aad-ben

Az automatikus központi telepítési módszerekkel
-   Parancsfájlok toogenerate használja, központi telepítése és titkos kulcsok váltása
-   Hello titkos kulcsok ne KV, használhatja az Active Directory minden más ügyfélhozzáféréshez
-   Nincs emberi hozzáférés toothem hitelesítés nélkül kell rendelkeznie.

Emellett vegye figyelembe a következő hello:
-   Hálózati biztonsági csoportokkal (NSG-k) DMZ-k létrehozása
-   A fürt használatára Jump kiszolgálók tooRDP fürt virtuális gépek vagy toomanage

Fürtök biztonságos tooprevent nem engedélyezett felhasználók tooyour fürthöz csatlakozzon, különösen akkor, ha a termelési számítási feladatokhoz fut rajta van kell lennie. Bár lehetséges toocreate egy nem biztonságos fürthöz, így lehetővé teszi a névtelen felhasználók tooconnect tooit, ha felügyeleti végpontok toohello teszi elérhetővé nyilvános internethez.

Használt technológiák tooimplement azokra. Hello [fürtök biztonsági forgatókönyveinek](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security) vannak:

-   Csomópontok security-ez hello virtuális gépek és a számítógépek hello fürt közötti kommunikáció biztonságossá tételére. Ez biztosítja, hogy csak engedélyezett toojoin hello fürt számítógépek részt vehetnek-alkalmazások és szolgáltatások hello fürt üzemeltetéséhez.
Az Azure vagy az önálló fürtökön fut a Windows rendszert futtató fürtöket választhat [tanúsítvány biztonsági](https://docs.microsoft.com/azure/service-fabric/service-fabric-windows-cluster-x509-security) vagy [Windows biztonsági](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-windows-cluster-windows-security) a Windows Server-gépek.
-   Ügyfél-csomópont biztonsági – Ez a Service Fabric-ügyfél és az egyes csomópontok hello fürt közötti kommunikáció biztonságossá tételére.
-   Szerepköralapú hozzáférés-vezérlést (RBAC) - megadhatja hello rendszergazdai és felhasználói ügyfél szerepkörök a fürt létrehozása hello időpontjában, adja meg a külön identitások (tanúsítványok, AAD stb.) az egyes.
-   Biztonsági javaslatok – az Azure-fürtök, javasoljuk, hogy használjon AAD biztonsági tooauthenticate ügyfelek és tanúsítványokat-csomópontok biztonsági.

tooconfigure hello önálló Windows-fürt, lásd: [önálló windows fürt megadásával](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-manifest).

Azure Resource Manager-sablonok és a Service Fabric Azure PowerShell-modul toocreate biztonságos fürt használja.
Egy részletes útmutató bemutatja, hogyan le egy biztonságos Azure Service Fabric beállítása a fürt az Azure-ban Azure Resource Manager használatával érhető el [Itt](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm).

A fürt hello Azure Resource Manager sablon toocustomize használatára
-   A telepítő által felügyelt tárolási virtuális gép VHD-k

Hello Azure Resource Manager sablon toodrive módosítások tooyour erőforráscsoport használata
-   Könnyű kezelés
-   Naplózás

A fürt konfigurációját, akkor kezelje kódot
-   Teljes körű úgy dönt, hogy toodeploy hello konfiguráció ellenőrzése
-   Kerülje a implicit parancsok tootweak az erőforrások közvetlenül

Hello sok aspektusainak [Service Fabric-alkalmazás életciklusa](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-lifecycle) automatizálható. [Service Fabric Azure PowerShell-modul](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications#upload-the-application-package) automatizálja a szokásos feladatokat telepítése, frissítése, eltávolítása és tesztelés az Azure Service Fabric-alkalmazások. Felügyelt, és az Alkalmazáskezelés HTTP API-k is elérhetők.

## <a name="use-x509-certificates"></a>X.509-tanúsítványokat használ
Fürtök mindig titkosítani kell X.509-tanúsítványokat vagy a Windows biztonsági. Biztonsági csak konfigurálni kell a fürt létrehozásának idejét, és már nem lehetséges tooenable biztonsági hello fürt létrehozása után.

Megadása esetén egy [fürt tanúsítvány](https://docs.microsoft.com/azure/service-fabric/service-fabric-windows-cluster-x509-security), állítsa be hello ClusterCredentialType tooX509. Ha külső kapcsolatok kiszolgálói tanúsítványának, állítsa be a hello ServerCredentialType tooX509.

-   A termelési számítási feladatokhoz rendszert futtató fürtöket használt tanúsítványok legyen a megfelelően konfigurált Windows kiszolgálói tanúsítvány szolgáltatások segítségével létrehozott vagy egy jóváhagyott tanúsítvány hitelesítésszolgáltatói (CA) kapott.
-   Soha ne használja az összes ideiglenes vagy tanúsítványok tesztelése éles üzemben eszközöket, például a MakeCert.exe használatával létrehozott.
-   Használhat önaláírt tanúsítványt, de csak akkor tegye ezt tesztfürtökön és nem éles környezetben.

Ha hello fürt nem biztonságos. Bárki csatlakozhat hozzá névtelenül és végrehajthat kezelési műveleteket, ezért az üzemben lévő fürtöket mindig X.509 tanúsítványok vagy a Windows rendszerbiztonság használatával kell védeni.

Hogyan több service fabric-fürt tooenable tanúsítványok megtekintéséhez toolearn [hozzáadhat és eltávolíthat tanúsítványokat a service fabric-fürt](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security-update-certs-azure).

## <a name="configure-security-policies"></a>Biztonsági szabályzatok konfigurálása
A Service Fabric is lehetővé teszi az alkalmazások által használt hello időpontban a központi telepítés hello felhasználói fiókokon – például biztonságos hello erőforrásokat, fájlok, könyvtárak és tanúsítványokat. Így futó alkalmazást, még akkor is, a megosztott üzemeltetési környezetben, nagyobb biztonságot nyújt, egymástól.

-   Active Directory tartományi csoport vagy felhasználó használata: az Active Directory-felhasználó vagy csoport fiókkal hello credentials hello szolgáltatást is futtathatja. Ez az Active Directory helyszíni saját tartományában, és nincs az Azure Active Directoryval (Azure AD). Tartományi felhasználó vagy csoport segítségével érheti el más erőforrásokat hello tartomány (például fájlmegosztások), amely engedéllyel rendelkezik.

-   Rendelje hozzá a biztonsági házirend HTTP és HTTPS-végpont: Ha a csoportházirend RunAs tooa szolgáltatás telepítését, és hello szolgáltatás jegyzékfájl deklarál végpont erőforrások hello HTTP protokollal, meg kell adnia egy SecurityAccessPolicy tooensure, hogy a portok toothese lefoglalt végpontok megfelelően access-vezérelt hello futtató felhasználói fiók, amely hello szolgáltatás fut a tájékoztatási céllal felsorolja. Ellenkező esetben a http.sys nincs toohello szolgáltatás, és sikertelen hívások lehívása hello ügyfél.
toolearn több engedélyezéséhez tekintse meg a service fabric, a biztonsági házirendeket [konfigurálhat biztonsági házirendeket az alkalmazás](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-runas-security).

## <a name="reliable-actors-security-configuration"></a>Megbízható szereplője biztonsági konfiguráció
Service Fabric Reliable Actors hello szereplő kialakítási mintája megvalósítása. A szoftver a kialakítási mintában a hello döntés, hogy toouse egy adott minta történik-e a szoftverfrissítések tervezése probléma alapján megfelelő hello mintát.

Általános útmutatást fontolja meg a hello szereplő mintát toomodel a probléma vagy forgatókönyv ha:
-   A probléma tárhely magában foglalja a sok (több ezer vagy több) kis, független és elkülönített állapot és a logikai egységek.
-   Azt szeretné, hogy a külső összetevők, beleértve az állapot lekérdezése szereplője csoportja között jelentős beavatkozást nem igénylő egyszálas objektummal toowork.
-   A szereplő példányok nem blokkolja a hívók előre nem látható késlelteti az i/o-műveletek kiállításával.

A Service Fabric szereplője valósíthatók meg hello Reliable Actors keretrendszer: a beépített szereplő minta-alapú alkalmazás-keretrendszer [Service Fabric Reliable Services](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-introduction). Minden megbízható szereplő szolgáltatás ír egy ténylegesen egy particionált, állapot-nyilvántartó megbízható szolgáltatás.
Minden aktor egy szereplő típusú típusúként van definiálva, azonos toohello egy .NET-objektum módja a .NET-típus példánya. Például előfordulhat, hogy egy szereplő típus, amely megvalósítja a Számológép hello funkcióit, és előfordulhat, hogy sok szereplője az adott típusú fürt különböző csomópontokon elosztott. Minden ilyen szereplő egyedileg azonosít egy szereplő.

[A replikáló biztonsági beállításokkal](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-kvsactorstateprovider-configuration) használt toosecure hello kommunikációs csatornát replikáció során használt vannak. Ez azt jelenti, hogy a szolgáltatások nem látható egymás replikációs forgalmat, amely biztosítja, hogy magas rendelkezésre állási hello adatokat is biztonságos. Alapértelmezés szerint egy üres biztonsági konfigurációs szakasz megakadályozza, hogy a replikációs biztonságot.
Replikátor konfigurációk hello replikátor, amely feladata, hogy nagymértékben megbízható hello szereplő Állapotszolgáltató állapotot konfigurálja.

## <a name="configure-ssl-for-azure-service-fabric"></a>Az SSL konfigurálása az Azure Service Fabric

Kiszolgálóhitelesítés: [hitelesíti](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) hello fürt felügyeleti végpontok tooa felügyeleti ügyfél, így hello felügyeleti ügyfél tudja azt toohello valós fürt van van szó. Ez a tanúsítvány is biztosít egy [SSL](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-creation-via-arm) hello HTTPS felügyeleti API és a Service Fabric Explorer HTTPS-KAPCSOLATON keresztül.
Be kell szereznie egy egyéni tartománynevet a fürt számára. A hitelesítésszolgáltató tanúsítvány kérése, amikor hello tanúsítvány tulajdonosának nevét meg kell egyeznie a fürt használó hello egyéni tartománynevet.

SSL tooconfigure az alkalmazáshoz, először tooget által egy tanúsítvány (CA), a megbízható tanúsítványokat állít ki erre a célra harmadik fél aláírt SSL-tanúsítvány. Ha még nem rendelkezik egy, akkor tooobtain egy vállalat, amely SSL-tanúsítványok értékesít.

hello tanúsítvány hello az SSL-tanúsítványok az Azure-követelményeknek kell megfelelniük:
-   hello tanúsítványnak tartalmaznia kell egy titkos kulccsal.
-   a kulcscsere, exportálható tooa személyes információcsere (.pfx) fájlt hello tanúsítványt kell létrehozni.
-   hello tanúsítvány tulajdonosának nevét meg kell egyeznie hello használt tartomány tooaccess hello felhőalapú szolgáltatás. Egy SSL-tanúsítványt a hitelesítésszolgáltatótól (CA) hello cloudapp.net tartomány nem lehet megszerezni. Be kell szerezni a egyéni tartomány nevét toouse amikor éri el a szolgáltatást. A hitelesítésszolgáltató tanúsítvány kérése, amikor hello tanúsítvány tulajdonosának nevét meg kell egyeznie hello egyéni tartomány használt név tooaccess az alkalmazás. Például ha az egyéni tartománynevet contoso.com, akkor tanúsítvány kérése a hitelesítésszolgáltató a **. contoso.com** vagy **www.contoso.com.**
-   hello tanúsítvány legalább 2048 bites titkosítást kell használnia.

A HTTP nem biztonságos, és azért tulajdonos tooeavesdropping támadások, mert a hello hello webes böngésző toohello webkiszolgáló vagy más végpontok közötti átvitel során továbbított adatok egyszerű szövegként. Ez azt jelenti, hogy a támadók intercept, és a bizalmas adatokat, például a hitelkártya adatait és a fiók bejelentkezési adatok megtekintéséhez. Amikor adatokat küldött, vagy a HTTPS-t böngészőn keresztül közzétett, SSL biztosítja, hogy ezeket az információkat titkosított és biztonságos hozzáférés a.

több, lásd: toolearn, [SSL konfigurálása az azure alkalmazás](https://docs.microsoft.com/azure/cloud-services/cloud-services-configure-ssl-certificate).

## <a name="network-isolationsecurity-with-azure-service-fabric"></a>Hálózati elkülönítési/biztonságot, az Azure Service Fabric
Használjon [Azure Resource Managerrel (ARM) sablon](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates) három nodetype beállításának mintaként biztonságos fürt és toocontrol hello a hálózati biztonsági csoportok bejövő és kimenő hálózati forgalmat.

hello sablonjának hálózati biztonsági csoport az egyes hello virtuális gép méretezési set(VMSS) toocontrol hello forgalom mindkét hello VMSS. Alapértelmezés szerint hello szabályok tooallow összes forgalom hello beállítása hello rendszer szolgáltatások és hello alkalmazás portok hello sablonban megadott. Ellenőrizze ezeket a szabályokat, és végezze el módosítások toofit hozzáadása az igényeinek, beleértve az alkalmazások bármely újakat.

További információkért lásd: [Azure Service Fabric – általános hálózati forgatókönyvek](https://docs.microsoft.com/azure/service-fabric/service-fabric-patterns-networking).

## <a name="set-up-a-key-vault-for-security"></a>A kulcstároló, a biztonság beállítása
A rendszer tanúsítványokat használ a Service Fabric tooprovide hitelesítési és titkosítási toosecure különböző szempontjairól a fürt és az alkalmazásokhoz.

A Service Fabric X.509 tanúsítvány toosecure egy fürt használja, és adja meg az alkalmazás biztonsági funkciók. Key Vault túl használja[tanúsítványok kezelése](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security-update-certs-azure) Service Fabric-fürtök az Azure-ban. A fürt telepítésekor az Azure-ban, hello Azure erőforrás-szolgáltató, amely felelős az Service Fabric-fürtök létrehozására kéri le a tanúsítványokat a Key Vault, és telepíti őket hello fürt virtuális gépeket.

hello közötti kapcsolat [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault), a service fabric-fürt és a tanúsítványokat, amikor létrehozza a fürtöt a key vaultban tárolt használó hello Azure erőforrás-szolgáltató.

**Hozzon létre egy erőforráscsoportot** hello első lépése az toocreate kimondottan a key vaultban egy erőforráscsoportot. Azt javasoljuk, hogy hello kulcstároló kerüljenek-e a saját erőforráscsoport. Ez a művelet lehetővé teszi hello számítási és tárolási erőforrás csoportokhoz, beleértve a Service Fabric-fürt, a kulcsok és titkos kulcsok elvesztése tartalmazó erőforráscsoport hello eltávolítását. a kulcstartót tartalmazó erőforráscsoportot hello hello kell lennie és hello fürt által használt ugyanabban a régióban.

**Hozzon létre egy kulcstartót hello új erőforráscsoportban** hello kulcstároló engedélyezni kell a központi telepítés tooallow hello számítási erőforrás szolgáltató tooget tanúsítványokat, és telepíti azt a virtuálisgép-példánya.
Hogyan több másolatot az Azure key vault tooset megtekintéséhez toolearn [Ismerkedés az Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-get-started).

## <a name="assign-users-roles"></a>Felhasználói szerepkörök hozzárendelése
A létrehozást követően hello alkalmazások toorepresent a fürthöz, a felhasználók toohello szerepkörök hozzárendelése a Service Fabric által támogatott: olvasási és a rendszergazda segítségét. Hello szerepköröket rendelhet hello klasszikus Azure portál használatával.

>[!Note]
> További információ a szerepkörök a Service Fabric: [szerepköralapú hozzáférés-vezérlés a Service Fabric ügyfelek](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security-roles).

Azure Service Fabric két különböző hozzáférést vezérlő típusokat támogatja az ügyfelek, amelyek csatlakoztatott tooa [Service Fabric-fürt](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm): rendszergazdai és felhasználói. Hozzáférés-vezérlés lehetővé teszi, hogy hello rendszergazda toolimit hozzáférés toocertain fürt fürtműveletekben hello fürt biztonságosabbá tétele a felhasználók különböző csoportok számára.

## <a name="next-steps"></a>Következő lépések
- A Service Fabric beállítása [fejlesztőkörnyezet](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started).
- További tudnivalók [Service Fabric támogatási lehetőségek](https://docs.microsoft.com/azure/service-fabric/service-fabric-support).

