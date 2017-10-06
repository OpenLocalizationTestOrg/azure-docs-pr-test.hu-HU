---
title: "aaaAzure service fabric biztonsági áttekintése |} Microsoft Docs"
description: "Ez a cikk áttekintést hello Azure service fabric biztonsági."
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
ms.openlocfilehash: ec5355983c5d59f4e0c3b855965f03ac47f1a4c1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-service-fabric-security-overview"></a>Az Azure Service Fabric biztonsági áttekintése
[Az Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-overview) egy elosztott rendszerek platform, amely megkönnyíti a könnyen toopackage, telepítéséhez és felügyeletéhez a méretezhető és megbízható micro-szolgáltatások. A Service Fabric címek hello jelentős kihívásaira fejlesztésének és kezelésének a felhőalapú alkalmazásokhoz. A fejlesztők és a rendszergazdák elkerülhetik az infrastruktúrával kapcsolatos összetett problémákat, és a kritikus fontosságú, nagy erőforrás-igényű, skálázható, megbízható és felügyelhető számítási feladatok megvalósítására koncentrálhatnak.

Azure Service Fabric biztonsági áttekintő cikkben a következő területeken hello koncentrál:

-   A fürt védelme
-   Megfigyelési és diagnosztikai
-   Biztonságos tanúsítványok használatával
-   Szerepköralapú hozzáférés-vezérlést (RBAC)
-   Biztonságos Windows biztonsági használó fürt
-   A Service Fabric-alkalmazások biztonságának konfigurálása
-   Az Azure Service Fabric biztonsági szolgáltatások biztonságos kommunikáció

## <a name="securing-your-cluster"></a>A fürt védelme
Az Azure Service Fabric egy orchestrator szolgáltatások gépet fürtön belül, fürtök szerepelnie kell biztonságos tooprevent nem engedélyezett felhasználók tooyour fürthöz csatlakozzon, különösen akkor, ha a termelési számítási feladatokhoz fut rajta van. Bár lehetséges toocreate egy nem biztonságos fürthöz, így lehetővé teszi a névtelen felhasználók tooconnect tooit, ha felügyeleti végpontok toohello teszi elérhetővé nyilvános internethez.

Ez a szakasz biztonsági forgatókönyvek hello áttekintést nyújt az Azure vagy az önálló rendszert futtató fürtöket, és különböző technológiák tooimplement hello azokra. hello fürt biztonsági forgatókönyvek a következők:

-   Csomópontok biztonsági
-   Ügyfél-csomópont biztonsági

### <a name="node-to-node-security"></a>Csomópontok biztonsági
Hello virtuális gép vagy gépek hello fürt közötti kommunikáció biztonságossá tételére. Ez biztosítja, hogy csak engedélyezett toojoin hello fürt számítógépek részt vehetnek-alkalmazások és szolgáltatások hello fürt üzemeltetéséhez.

Az Azure vagy az önálló fürtökön fut a Windows rendszert futtató fürtöket választhat [tanúsítvány biztonsági](https://msdn.microsoft.com/library/ff649801.aspx) vagy [Windows biztonsági](https://msdn.microsoft.com/library/ff649396.aspx) a Windows Server-gépek.

**Csomópontok tanúsítvány biztonság**

Service Fabric X.509 kiszolgálói tanúsítványok, melyet a hello csomóponttípus konfigurációk részeként egy fürt használja. Mik azok a tanúsítványok gyors áttekintést és [hogyan szerezni, vagy hozza létre a címzetteket megtalálható ez a cikk](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/working-with-certificates).

Tanúsítvány biztonsági hello Azure-portálon keresztül, Azure Resource Manager-sablonok vagy egy önálló JSON-sablon létrehozásakor hello fürt úgy van beállítva. Megadhat egy elsődleges tanúsítványnak és egy nem kötelező másodlagos tanúsítvány előgörgetések használt tanúsítványt. hello megadhat elsődleges és másodlagos tanúsítványok nem lehet ugyanaz, mint hello rendszergazdai ügyfél és a csak olvasható ügyféltanúsítványokat állít be [ügyfél-csomópont biztonsági](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-security).

### <a name="client-to-node-security"></a>Ügyfél-csomópont biztonsági
Ügyfél toonode biztonsági ügyfél identitások segítségével konfigurálható. egy ügyfél és a hello fürt közötti megbízhatóság tooestablish hello fürt tooknow melyik ügyfél identitások megbízhatónak is kell konfigurálnia. Ehhez két különböző módon:

-   Adja meg a hello tartományi csoport felhasználók csatlakoztatható vagy
-   Adja meg a hello csomópont Tartományfelhasználók csatlakoztatható.

Service Fabric két különböző hozzáférést vezérlő típusokat támogatja az ügyfelek, amelyek csatlakoztatott tooa Service Fabric-fürt:

-   Rendszergazda
-   Felhasználó

Hozzáférés-vezérlés segítségével hello hello a fürt rendszergazdai toolimit hozzáférés toocertain típusú különböző csoportok számára, így biztonságosabb hello fürt a fürt működését. A rendszergazdák teljes hozzáféréssel toomanagement képességek (beleértve az olvasási/írási képességek) rendelkeznek. Felhasználók, alapértelmezés szerint rendelkezik csak olvasási hozzáféréssel toomanagement képességek (például lekérdezési lehetőségek), és hello képességét tooresolve alkalmazások és szolgáltatások.

**Ügyfél-csomópont tanúsítvány biztonság**

Ügyfél-csomópont tanúsítvány a biztonsági beállítások konfigurálása a Resource Manager-sablonok vagy egy önálló JSON-sablon egy rendszergazdai ügyféltanúsítvány és/vagy felhasználói ügyféltanúsítványt megadásával hello Azure-portálon keresztül hello fürt létrehozása során. hello felügyeleti ügyfél és a felhasználói ügyféltanúsítványok adja meg, hogy nem lehet ugyanaz, mint hello elsődleges és másodlagos tanúsítványokat állít be csomópontok biztonsági.

Toohello fürt hello felügyeleti tanúsítvány használatával csatlakozó ügyfelek teljes körű hozzáférési toomanagement képességek állnak a rendelkezésére. Toohello fürt hello írásvédett felhasználó ügyféltanúsítvány használatával csatlakozó ügyfelek csak olvasási hozzáféréssel toomanagement képességekkel rendelkezik. Ezek a tanúsítványok használhatók hello más Word szerepkör adatbázisok hozzáférés-vezérlést (RBAC).

Az Azure olvasási [fürt beállítása az Azure Resource Manager-sablon használatával](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) toolearn hogyan tooconfigure tanúsítvány biztonsági fürtben.

**Ügyfél-csomópont Azure Active Directory (AAD) biztonsági az Azure-on**

Azure-on futó fürtök is biztosíthassa hozzáférés toohello felügyeleti végpontok Azure Active Directory (AAD) használatával. Lásd: [fürt beállítása az Azure Resource Manager-sablon használatával](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) hogyan toocreate hello szükséges AAD összetevők olvashat, hogyan toopopulate őket során fürt létrehozása, és hogyan tooconnect toothose ezután fürtök.

Aad-ben lehetővé teszi, hogy a szervezetek (más néven bérlői) toomanage felhasználói hozzáférés tooapplications, amelyeknek az alkalmazásoknak a webalapú bejelentkezési felhasználói felület és az alkalmazások natív ügyfél nyújthassunk.

A Service Fabric-fürt biztosít több belépési pontok tooits felügyeleti funkciót, beleértve a hello webalapú Service Fabric Explorer és a Visual Studio. Ennek eredményeképpen hozzon létre két AAD alkalmazások toocontrol hozzáférés toohello fürt, egy webes alkalmazás és egy natív alkalmazás.
Az Azure-fürtök esetén ajánlott használni, amikor az AAD biztonsági tooauthenticate ügyfelek és tanúsítványokat-csomópontok biztonsági.

Az önálló Windows Server-fürtök esetén ajánlott, hogy a Windows biztonsági a csoportosan felügyelt fiókokkal (GMA) használhat el, ha a Windows Server 2012 R2 és az Active Directory. Ellenkező esetben továbbra is használni Windows biztonsági Windows-fiókkal.

## <a name="monitoring-and-diagnostics-for-azure-service-fabric"></a>Megfigyelési és diagnosztikai az Azure Service Fabric
[Megfigyelési és diagnosztikai](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-overview) kritikus toodeveloping, tesztelése és telepítése az alkalmazások és szolgáltatások minden környezetben vannak. Service Fabric-megoldás a legmegfelelőbb tervezése és megvalósítása a figyelési és diagnosztika, amelyek segítenek győződjön meg róla, alkalmazásokat, és szolgáltatások vannak a várt módon működik a helyi környezet vagy a termelési.

Biztonsági szempontból hello fő célja, figyelés és diagnosztika:

-   Észleli és eseményadatokat hardver- és infrastruktúra, amely tooa biztonsági esemény miatt lehet.
-   Észleli a szoftver- és alkalmazás problémáit, amely jelzi, hogy a biztonsági sérülés (IoC) biztosítani.
-   Erőforrás megértéséhez fogyasztás toohelp véletlen szolgáltatásmegtagadást megelőzése.

a figyelés általános munkafolyamat hello és diagnosztika három lépésből áll:

-   **Az eseménygenerálás:** Ez magában foglalja (naplókat, nyomkövetéseket, egyéni események) események, mind a hello infrastruktúra (fürt), és az alkalmazás / szolgáltatás szintjén. Tudjon meg többet az [infrastruktúra szintű események](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-generation-infra) és [szintű alkalmazásesemények](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-generation-app) toounderstand milyen áll rendelkezésre, és hogyan tooadd további instrumentation.
-   **Esemény összesítési:** létrehozott események kell toobe gyűjti, majd összesíti azokat megjelenítése előtt. Általában javasoljuk [Azure Diagnostics](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-aggregation-wad) (több hasonló tooagent alapú naplógyűjtést) vagy [EventFlow](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-aggregation-eventflow) (a folyamat naplógyűjtést).
-   **Elemzési:** események szükséges toobe feladatkonfigurációkat és néhány formátumban, tooallow érhető el elemzés és megjelenítési igény szerint. Nincsenek meglévő hello piaci toohello elemzés és a figyelés és diagnosztikai adatok képi megjelenítések ismét számos nagy platformra. hello javasolt kettő [OMS](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-analysis-oms) és [Application Insights](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-analysis-appinsights) tootheir jobb integráció a Service Fabric miatt.

Is [Azure figyelő](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview) toomonitor számos hello, amelyen a Service Fabric-fürt épül Azure-erőforrások.

Egy figyelő olyan külön szolgáltatás állapotának megtekintése és hello állapotfigyelő modell hierarchia semmit a szolgáltatások és a jelentés állapotának betöltése. Ez segítséget nyújt egyetlen szolgáltatás hello ábrázolása alapján, amely nem észlelhető hiba. Watchdogs egy remek toohost kódot, amely végrehajtja a javító műveleteket (például bizonyos időközönként törölje a naplófájlokat tároló) felhasználói beavatkozás nélkül is. A minta-figyelő szolgáltatás megvalósításának található [Itt](https://azure.microsoft.com/resources/samples/service-fabric-watchdog-service/).

## <a name="secure-using-certificates"></a>Biztonságos tanúsítványok használatával
Tanúsítványokat használ, információt szolgáltat arról, hogyan toosecure hello közötti kommunikáció hello a különálló Windows-fürt különböző csomópontok módjáról, valamint tooauthenticate csatlakozó-ügyfeleket toothis fürt X.509-tanúsítványokat használ. Ez biztosítja, amelyek csak a jogosult felhasználók férhetnek hello fürt hello központilag telepített alkalmazások, és felügyeleti feladatok elvégzésére. Tanúsítvány biztonsági engedélyezni kell a fürt hello hello fürt létrehozásakor.

### <a name="x509-certificates-and-service-fabric"></a>X.509-tanúsítványokat és a Service Fabric
X.509 digitális tanúsítványok a gyakran használt tooauthenticate ügyfelek és kiszolgálók és tooencrypt és üzenetek digitális aláírását.

hello következő tábla hello-tanúsítványokat sorolja fel a fürt beállítása lesz szükség:

|Tanúsítvány-információkat beállítás |Leírás|
|-------------------------------|-----------|
|ClusterCertificate|    Ez a tanúsítvány szükséges toosecure hello kommunikációs fürt hello csomópontok között. Frissítés két különböző tanúsítványok, egy elsődleges és másodlagos használható.|
|ServerCertificate| Ez a tanúsítvány toohello ügyfél áll rendelkezésre, amikor tooconnect toothis fürt. Frissítés két különböző kiszolgálói tanúsítványok, egy elsődleges és másodlagos használható.|
|ClientCertificateThumbprints|  Ez olyan tanúsítványok, amelyet az tooinstall hitelesített hello ügyfeleken.|
|ClientCertificateCommonNames|  Hello köznapi neve hello első ügyféltanúsítvány hello CertificateCommonName beállítása. hello CertificateIssuerThumbprint hello ujjlenyomata a tanúsítvány kiállítója hello.|
|ReverseProxyCertificate|   Ez az egy nem kötelező tanúsítvány, amely meg, ha azt szeretné, toosecure a [fordított Proxy](https://docs.microsoft.com/azure/service-fabric/service-fabric-reverseproxy).|

További információ a tanúsítványok, biztonságossá tétele [ide](https://docs.microsoft.com/azure/service-fabric/service-fabric-windows-cluster-x509-security).

## <a name="role-based-access-control-rbac"></a>Szerepköralapú hozzáférés-vezérlést (RBAC)
Hozzáférés-vezérlés lehetővé teszi, hogy hello rendszergazda toolimit hozzáférés toocertain fürt fürtműveletekben hello fürt biztonságosabbá tétele a felhasználók különböző csoportok számára. Két különböző hozzáférési ellenőrzési típusok támogatottak tooa fürt csatlakozó ügyfeleket: rendszergazdai szerepkör és a felhasználói szerepkör.

A rendszergazdák teljes hozzáféréssel toomanagement képességek (beleértve az olvasási/írási képességek) rendelkeznek. Felhasználók, alapértelmezés szerint rendelkezik csak olvasási hozzáféréssel toomanagement képességek (például lekérdezési lehetőségek), és hello képességét tooresolve alkalmazások és szolgáltatások.

Megadhatja hello rendszergazdai és felhasználói ügyfél szerepkörök a fürt létrehozása hello időpontjában, adja meg a külön identitások (tanúsítványok, AAD stb.) az egyes. Hello alapértelmezett hozzáférés-vezérlési beállításokkal, és hogyan toochange hello alapértelmezett beállítások további információkért lásd: [szerepköralapú hozzáférés-vezérlés a Service Fabric-ügyfelek](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security-roles).

## <a name="secure-standalone-cluster-using-windows-security"></a>Windows biztonsági használó biztonságos önálló fürt
tooprevent jogosulatlan hozzáférés tooa Service Fabric-fürt, biztosítania kell hello fürt. Biztonsági különösen fontos, termelési számítási feladatokhoz hello fürt futtatásakor. Leírja, hogyan tooconfigure csomópontok és ügyfél-csomópont biztonsági a Windows rendszerbiztonság használatával hello művelet fájlt.

**Csoportosan felügyelt szolgáltatásfiókot használó Windows biztonságának konfigurálása**

Úgy, hogy a csomópont toonode biztonsági beállítások [ClustergMSAIdentity](https://docs.microsoft.com/azure/service-fabric/service-fabric-windows-cluster-windows-security) Ha szüksége van a service fabric toorun csoportosan felügyelt szolgáltatásfiók alatt. A sorrend toobuild megbízhatósági kapcsolatokat csomópontok között is el kell tisztában legyen egymással.

Ügyfél toonode biztonsági ClientIdentities használatával van konfigurálva. A sorrend tooestablish közötti megbízhatósági kapcsolat egy ügyfél és a hello fürt konfigurálnia kell hello fürt tooknow melyik ügyfél identitások megbízhatónak is.

**A számítógép csoport használata Windows biztonságának konfigurálása**

Csomópont toonode biztonsági ClusterIdentity használata, ha azt szeretné, hogy az Active Directory-tartományba tartozó számítógép csoport toouse beállítás szerint van konfigurálva. További információkért lásd: [létrehoz egy gép csoportot az Active Directory](https://msdn.microsoft.com/library/aa545347).

Ügyfél-csomópont biztonsági ClientIdentities segítségével konfigurálhatók. egy ügyfél és a hello fürt közötti megbízhatóság tooestablish hello fürt tooknow hello ügyfél identitásokat tartalmaz, amelyek a fürt hello is megbízhat kell konfigurálnia. Megbízhatósági kapcsolat két különböző módon is létrehozása:

-   Adja meg a hello tartományi csoport felhasználók csatlakozhatnak.
-   Adja meg a hello csomópont Tartományfelhasználók csatlakoztatható.

## <a name="configure-application-security-in-service-fabric"></a>A Service Fabric-alkalmazások biztonságának konfigurálása
### <a name="managing-secrets-in-service-fabric-applications"></a>A Service Fabric-alkalmazások titkos kulcsok kezelése
Ezzel a módszerrel segíti a Service Fabric-alkalmazás a titkos kulcsok kezelése. Titkos kulcsok lehet bármely bizalmas adatokat, például a tárolási kapcsolati karakterláncok, jelszavak és egyéb értékek, amelyek nem egyszerű szöveges kezelje.

Ezt a módszert használja [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-whatis) toomanage kulcsok és titkos kulcsok. Azonban titkos kulcsok egy alkalmazás használata felhő platform-független tooallow alkalmazások telepített toobe tooa fürt tárolt bármely részén. Ez a folyamat négy fő lépésből áll:

-   Adatok rejtjelezése tanúsítványának beszerzése.
-   Hello tanúsítvány telepítése a fürtön.
-   Titkosítani a titkos értékek hello tanúsítvánnyal alkalmazások telepítésekor, és azokat behelyezése egy szolgáltatás Settings.xml konfigurációs fájlt.
-   Olvassa el a titkosított értékeket abból Settings.xml visszafejti a hello rejtjelezése ugyanazt a tanúsítványt.

>[!Note]
>További információ [Service Fabric-alkalmazások a titkos kulcsok kezelése](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-secret-management).

### <a name="configure-security-policies-for-your-application"></a>Biztonsági házirendek beállítása az alkalmazáshoz
Azure Service Fabric biztonsági használatával segíthet a biztonságos hello fürt különböző felhasználói fiók alatt futó alkalmazásokat. Service Fabric biztonsági is lehetővé teszi az alkalmazások által használt hello időpontban a központi telepítés hello felhasználói fiókokon – például biztonságos hello erőforrásokat, fájlok, könyvtárak és tanúsítványokat. Így futó alkalmazást, még akkor is, a megosztott üzemeltetési környezetben, nagyobb biztonságot nyújt, egymástól.
hello lépések az alábbiak:

-   A telepítő belépési pont hello szabályzat konfigurálása.
-   A telepítő belépési pontról indítsa el a PowerShell-parancsokat.
-   A hibakereséshez a helyi konzol átirányítást használ.
-   A szolgáltatás kód csomagok szabályzat beállítása.
-   Rendelje hozzá a HTTP és HTTPS-végpont egy biztonsági házirend.

## <a name="secure-communication-for-services-in-azure-service-fabric-security"></a>Azure Service Fabric biztonsági szolgáltatások biztonságos kommunikáció
A biztonság az egyik legfontosabb szempontja hello kommunikációra. hello Reliable Services alkalmazás-keretrendszer néhány előre elkészített kommunikációs verem és eszközök, amelyek lehetnek biztonsági használt tooimprove biztosít.

-   [Számítógépek biztonságossá tétele a szolgáltatás használatakor a távelérési szolgáltatás](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-secure-communication).
-   [Számítógépek biztonságossá tétele a szolgáltatás egy WCF-alapú kommunikációs verem használatakor](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-secure-communication#help-secure-a-service-when-youre-using-a-wcf-based-communication-stack).

## <a name="next-steps"></a>Következő lépések
- Fürt biztonsággal kapcsolatos általános információkért lásd: [fürt létrehozása az Azure Resource Manager-sablon használatával](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) és [Azure-portálon](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-portal).
- További információ című [Service Fabric-fürt biztonsági](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security).
