---
title: "aaaTroubleshoot az adatkezelési átjáró problémák |} Microsoft Docs"
description: "Tippek tootroubleshoot problémák kapcsolódó tooData adatkezelési átjáró biztosít."
services: data-factory
author: nabhishek
manager: jhubbard
editor: monicar
ms.assetid: c6756c37-4e5a-4d1e-ab52-365f149b4128
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: abnarain
published: True
ms.openlocfilehash: 85dacc8a1e8d574d6e7d5b556c995cebdc148fde
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-issues-with-using-data-management-gateway"></a>Az adatkezelési átjáró használata közben felmerülő hibák elhárítása
Ez a cikk tájékoztatást nyújt az adatkezelési átjáró használatával kapcsolatos hibák elhárításához.

> [!NOTE]
> Lásd: hello [az adatkezelési átjáró](data-factory-data-management-gateway.md) cikk hello átjáró kapcsolatos részletes információkat. Lásd: hello [helyezze át az adatokat a helyszíni és a felhő között](data-factory-move-data-between-onprem-and-cloud.md) adatok áthelyezése egy helyi SQL Server adatbázis tooMicrosoft Azure Blob Storage tárolóban hello átjáró használatával kapcsolatos általános bemutatóért cikkében.
>
>

## <a name="failed-tooinstall-or-register-gateway"></a>Nem sikerült tooinstall vagy regisztrálni átjáró
### <a name="1-problem"></a>1. Probléma
Megjelenik a hibaüzenet, amikor telepítése és regisztrálása az átjáró pontosabban hello átjáró telepítési fájl letöltése során.

`Unable tooconnect toohello remote server". Please check your local settings (Error Code: 10003).`

#### <a name="cause"></a>Ok
hello gép tooinstall hello átjáró próbált toodownload hello legújabb átjáró telepítési fájlját a letöltőközpontból hello tooa hálózati probléma miatt nem sikerült.

#### <a name="resolution"></a>Megoldás:
A tűzfal-proxy kiszolgáló beállítások toosee ellenőrizheti, hogy hello-beállítások le hello hálózati kapcsolat a hello számítógép toohello [letöltőközpontból](https://download.microsoft.com/), hello-beállítások frissítése, és ennek megfelelően.

Alternatív megoldásként letöltheti hello telepítőfájlja legújabb átjáró hello hello [letöltőközpontból](https://www.microsoft.com/download/details.aspx?id=39717) hello letöltőközpontból hozzáférő más számítógépeken. Ezután a másolási hello installer fájl toohello átjáró állomás, és futtassa manuálisan tooinstall és frissítés hello átjáró.

### <a name="2-problem"></a>2. Probléma
Ezt a hibaüzenetet látja, amikor egy átjáró tooinstall kattintva végrehajtani kívánt **telepíthető közvetlenül a számítógépen** a hello Azure-portálon.

`Error:  Abort installing a new gateway on this computer because this computer has an existing installed gateway and a computer without any installed gateway is required for installing a new gateway.`  

#### <a name="cause"></a>Ok
Átjáró hello számítógépen már telepítve van.

#### <a name="resolution"></a>Megoldás:
Távolítsa el a meglévő átjáró hello hello gépen, és kattintson a hello **telepíthető közvetlenül a számítógépen** újra hivatkozásra.

### <a name="3-problem"></a>3. Probléma
Ez a hiba jelenhet meg, amikor egy új átjáró regisztrálása.

`Error: hello gateway has encountered an error during registration.`

#### <a name="cause"></a>Ok
Láthatja, hogy ez az üzenet hello a következő okok valamelyike miatt:

* hello átjárókulcs hello formátuma érvénytelen.
* hello átjárókulcs érvénytelenné vált.
* hello átjárókulcs újra lett létrehozza a rendszer hello portálról.  

#### <a name="resolution"></a>Megoldás:
Győződjön meg arról, hogy használ-e hello jobb átjárókulcs hello portálról. Ha szükséges, a kulcs újragenerálása és hello kulcs tooregister hello átjáró használatára.

### <a name="4-problem"></a>4. Probléma
Hello átjáró regisztrálásakor még a következő hibaüzenet jelenhet meg.

`Error: hello content or format of hello gateway key "{gatewayKey}" is invalid, please go tooazure portal toocreate one new gateway or regenerate hello gateway key.`



![Tartalom és a kulcs formátuma érvénytelen](media/data-factory-troubleshoot-gateway-issues/invalid-format-gateway-key.png)

#### <a name="cause"></a>Ok
hello tartalom vagy hello bemeneti átjárókulcs formátuma nem megfelelő. Hello okok valamelyike miatt lehet, hogy a másolt hello kulcs egy részének hello portálról, vagy érvénytelen kulcsot használ.

#### <a name="resolution"></a>Megoldás:
Hozzon létre egy átjáró kulcsot hello portálon, és hello Másolás gombra toocopy hello teljes kulcsot használ. Illessze be az ablakot tooregister hello átjáró majd.

### <a name="5-problem"></a>5. Probléma
Hello átjáró regisztrálásakor még a következő hibaüzenet jelenhet meg.

`Error: hello gateway key is invalid or empty. Specify a valid gateway key from hello portal.`

![Átjáró kulcsa érvénytelen vagy üres:](media/data-factory-troubleshoot-gateway-issues/gateway-key-is-invalid-or-empty.png)

#### <a name="cause"></a>Ok
hello átjárókulcs újra lett létrehozva. vagy hello átjáró törölve lett az hello Azure-portálon. Akkor is előfordulhat, ha nincs legújabb hello az adatkezelési átjáró beállítása.

#### <a name="resolution"></a>Megoldás:
Ellenőrizze, hogy ha hello az adatkezelési átjáró telepítő hello legújabb verzióját, a hello Microsoft hello legújabb verziója található [letöltőközpontból](https://go.microsoft.com/fwlink/p/?LinkId=271260).

Beállítás aktuális / legújabb és átjáró még létezik a portálon, ha az Azure-portálon hello hello átjáró kulcs újragenerálása és hello Másolás gombra toocopy hello teljes kulcsot használ, és majd illessze be az ablakot tooregister hello átjáró. Ellenkező esetben hozza létre újra a hello átjáró, és kezdje újra a folyamatot.

### <a name="6-problem"></a>6. Probléma
Hello átjáró regisztrálásakor még a következő hibaüzenet jelenhet meg.

`Error: Gateway has been online for a while, then shows “Gateway is not registered” with hello status “Gateway key is invalid”`

![Átjáró kulcsa érvénytelen vagy üres:](media/data-factory-troubleshoot-gateway-issues/gateway-not-registered-key-invalid.png)

#### <a name="cause"></a>Ok
Ez a hiba akkor fordulhat elő, mert hello átjáró törölve lett vagy hello társított átjárókulcs újra lett létrehozva..

#### <a name="resolution"></a>Megoldás:
Ha hello átjáró törölve lett, hozza létre újból hello átjáró hello portálról, kattintson a **regisztrálása**, hello kulcs másolása hello portálról, illessze be és próbálja meg tooregister hello átjáró.

Ha hello átjáró még létezik, de a kulcsot újra lett létrehozva., hello új kulcs tooregister hello átjáró használatára. Ha nem rendelkezik hello kulccsal, generálja újra hello kulcsot újra hello portálról.

### <a name="7-problem"></a>7. Probléma
Amikor egy átjáró regisztrálás szükség lehet tooenter elérési útját és a jelszó tanúsítványt.

![Adja meg a tanúsítvány](media/data-factory-troubleshoot-gateway-issues/specify-certificate.png)

#### <a name="cause"></a>Ok
hello átjáró előtt más gépeken regisztrálva van. Hello kezdeti regisztrációs átjáró, során hello átjáró társítva lett egy titkosítási tanúsítványt. hello tanúsítványt kell önálló hello átjáró által létrehozott vagy hello felhasználó által megadott.  Ez a tanúsítvány használt tooencrypt hello adattár (társított szolgáltatás) hitelesítő adatait.  

![Tanúsítvány exportálása](media/data-factory-troubleshoot-gateway-issues/export-certificate.png)

Ha hello átjárót egy másik gazdaszámítógépet, hello regisztrációs varázsló megkérdezi, hogy a tanúsítvány toodecrypt korábban hitelesítő adatok a titkosított visszaállítása a tanúsítvánnyal.  Ez a tanúsítvány nélkül hello hitelesítő adatok nem tudja visszafejteni hello új átjáró, és az új átjáró társított későbbi másolási tevékenység végrehajtások sikertelen lesz.  

#### <a name="resolution"></a>Megoldás:
Ha exportálása hello hitelesítő adatainak tanúsítványa hello eredeti átjáró gépen hello segítségével **exportálása** hello gombjára **beállítások** az adatkezelési átjáró konfigurációkezelőjének lapján hello használata Itt szerepel tanúsítvány.

Ebben a szakaszban nem hagyható ki, amikor egy átjáró helyreállítása. Ha hello tanúsítványa hiányzik, toodelete hello átjáró hello portálról kell, és hozza létre az új átjárón.  Emellett frissítse az összes társított szolgáltatások, amelyek kapcsolódó toohello átjáró által ismét be kell írni a hitelesítő adataikat.

### <a name="8-problem"></a>8. Probléma
Hello a következő hibaüzenet jelenhet meg.

`Error: hello remote server returned an error: (407) Proxy Authentication Required.`

#### <a name="cause"></a>Ok
Ez a hiba történik, ha az átjáró egy olyan környezetben, szükséges egy HTTP-proxy tooaccess internetes erőforrásokhoz, vagy a proxy hitelesítési jelszó megváltozik, de nem frissül megfelelően a átjáróban.

#### <a name="resolution"></a>Megoldás:
Hello hello utasításait követve [Proxy server szempontjai](#proxy-server-considerations) Ez a szakasz a következő cikket, és konfigurálja a proxybeállításokat az adatkezelési átjáró konfigurációkezelőjének.

## <a name="gateway-is-online-with-limited-functionality"></a>Átjáró korlátozott funkciójú online állapotban
### <a name="1-problem"></a>1. Probléma
Hello átjáró hello állapotának online, az korlátozott funkciókkal láthatja.

#### <a name="cause"></a>Ok
Látni hello átjáró hello állapotának az online korlátozott funkciójú hello a következő okok valamelyike miatt:

* Átjáró nem tud csatlakozni a toocloud szolgáltatás Azure Service Buson keresztül.
* A felhőalapú szolgáltatás nem tud kapcsolódni a toogateway Service Buson keresztül.

Ha hello átjáró korlátozott funkciójú online állapotban, nem feltétlenül tudja toouse hello Data Factory másolása varázsló toocreate adatok adatcsatornákat az adatok tooor másolását a helyszíni adattárolókhoz. A probléma megoldásához használhatja Data Factory Editor hello portálon, a Visual Studio vagy Azure PowerShell.

#### <a name="resolution"></a>Megoldás:
A probléma feloldása (online korlátozott funkciójú) alapuló e hello átjáró nem connect toohello felhőalapú szolgáltatás, vagy más módon hello. a következő szakaszok hello adja meg, hogy ezek a megoldások.

### <a name="2-problem"></a>2. Probléma
Lásd a következő hiba hello.

`Error: Gateway cannot connect toocloud service through service bus`

![Átjáró toocloud szolgáltatás nem tud csatlakozni.](media/data-factory-troubleshoot-gateway-issues/gateway-cannot-connect-to-cloud-service.png)

#### <a name="cause"></a>Ok
Átjáró nem tud csatlakozni a felhőalapú szolgáltatás toohello Service Buson keresztül.

#### <a name="resolution"></a>Megoldás:
Kövesse a lépéseket tooget hello átjáró újra online:

1. IP-cím engedélyezése a hello átjárót futtató gép és a vállalati tűzfal hello kimenő szabályok. IP-címeket a Windows Eseménynapló hello található (azonosító == 401): kísérlet történt egy szoftvercsatorna tooaccess végrehajtott oly módon, a hozzáférési engedélyeket XX tiltott. A(Z) XX. A(Z) XX. XX:9350.
* Proxybeállítások konfigurálása hello átjárón. Lásd: hello [Proxy server szempontjai](#proxy-server-considerations) című szakaszban talál információt.
* Kimenő portok 5671 és 9350 – 9354-es a hello átjáró gépen és a vállalati tűzfal hello mindkét hello Windows tűzfal engedélyezése. Lásd: hello [portok és a tűzfalon](#ports-and-firewall) című szakaszban talál információt. Ez a lépés nem kötelező, de a teljesítmény okokból ajánlott.

### <a name="3-problem"></a>3. Probléma
Lásd a következő hiba hello.

`Error: Cloud service cannot connect toogateway through service bus.`

#### <a name="cause"></a>Ok
Egy átmeneti hiba történt a hálózati kapcsolatot.

#### <a name="resolution"></a>Megoldás:
Kövesse a lépéseket tooget hello átjáró újra online:

1. Várjon néhány percig, hello kapcsolatot a rendszer automatikusan helyreállítja a eltűnt hello hiba esetén.
* Ha hello hiba továbbra is fennáll, indítsa újra a hello átjáró szolgáltatást.

## <a name="failed-tooauthor-linked-service"></a>Sikertelen tooauthor társított szolgáltatás
### <a name="problem"></a>Probléma
Ez a hiba jelenhet meg próbálja toouse hitelesítőadat-kezelő hello portál tooinput hitelesítő adatait, egy új kapcsolódó szolgáltatás, vagy egy meglévő társított szolgáltatást a hitelesítő adatok frissítésekor.

`Error: hello data store '<Server>/<Database>' cannot be reached. Check connection settings for hello data source.`

Ha ezt a hibaüzenetet látja, hello beállítások lapon az adatkezelési átjáró konfigurációkezelőjének nézhet ki például a következő képernyőkép hello.

![Adatbázis nem érhető el](media/data-factory-troubleshoot-gateway-issues/database-cannot-be-reached.png)

#### <a name="cause"></a>Ok
hello SSL-tanúsítvány előfordulhat, hogy rendelkezik elveszett hello-átjáró számítógépén. hello átjáró-számítógép hello jelenleg használt tanúsítványt az SSL-titkosítás nem tölthető be. Hibaüzenet, amely a következő üzenet hasonló toohello hello eseménynaplójában is megjelenhet.

 `Unable tooget hello gateway settings from cloud service. Check hello gateway key and hello network connection. (Certificate with thumbprint cannot be loaded.)`

#### <a name="resolution"></a>Megoldás:
Kövesse a lépéseket toosolve hello probléma:

1. Indítsa el az adatkezelési átjáró Konfigurációkezelőjében.
2. Váltás toohello **beállítások** fülre.  
3. Kattintson a hello **módosítás** gomb toochange hello SSL-tanúsítvány.

   ![Módosítás tanúsítvány gombra](media/data-factory-troubleshoot-gateway-issues/change-button-ssl-certificate.png)
4. Jelöljön ki új tanúsítványt, hello SSL-tanúsítvány. Bármely Ön által létrehozott SSL-tanúsítvány vagy bármely szervezeti is használhatja.

   ![Adja meg a tanúsítvány](media/data-factory-troubleshoot-gateway-issues/specify-http-end-point.png)

## <a name="copy-activity-fails"></a>Másolási tevékenység sikertelen lesz.
### <a name="problem"></a>Probléma
Bizonyára észrevette, hogy egy folyamat hello portálon beállítása után a következő "UserErrorFailedToConnectToSqlserver" hiba hello.

`Error: Copy activity encountered a user error: ErrorCode=UserErrorFailedToConnectToSqlServer,'Type=Microsoft.DataTransfer.Common.Shared.HybridDeliveryException,Message=Cannot connect tooSQL Server`

#### <a name="cause"></a>Ok
Ez akkor fordulhat elő különböző okokból, és a megoldás ennek megfelelően változik.

#### <a name="resolution"></a>Megoldás:
Kimenő TCP-kapcsolatok engedélyezése az adatkezelési átjáró ügyféloldali hello TCP/1433-as porton keresztül tooan SQL-adatbázis csatlakoztatása előtt.

Ha hello céladatbázis Azure SQL-adatbázis, ellenőrizze az SQL-kiszolgáló a tűzfal beállításai Azure is.

Tekintse meg a következő szakasz tootest hello kapcsolat toohello helyszíni adattár hello.

## <a name="data-store-connection-or-driver-related-errors"></a>Az adattároló csatlakozási vagy kapcsolatos hibák
Ha adatok tárolásához a kapcsolódáshoz vagy az illesztőprogram-kapcsolatos hibákat, végezze el a lépéseket követve hello:

1. Indítsa el az adatkezelési átjáró konfigurációkezelőjének hello-átjáró számítógépén.
2. Váltás toohello **diagnosztika** fülre.
3. A **kapcsolat tesztelése**, hello átjáró csoport értékek hozzáadásához.
4. Kattintson a **teszt** toosee toohello tudja csatlakoztatni a helyszíni adatforrás hello átjáró gépről hello kapcsolati adatokat és hitelesítő adatok használatával. Hello kapcsolat tesztelése az illesztőprogram telepítése után továbbra is sikertelen, ha újraindítás hello átjáró azt toopick hello legújabb módosítás mentése.

![Kapcsolat tesztelése a Diagnosztika lap](media/data-factory-troubleshoot-gateway-issues/test-connection-in-diagnostics-tab.png)

## <a name="gateway-logs"></a>Átjáró naplói
### <a name="send-gateway-logs-toomicrosoft"></a>Átjáró naplók tooMicrosoft küldése
Amikor kapcsolatba lép a Microsoft Support tooget segítséget átjáró problémák elhárításához, megadását tooshare az átjáró naplói. Hello kiadás hello átjáró, a szükséges átjáró naplók és a két gombra történő kattintás az adatkezelési átjáró konfigurációkezelőjének is megoszthatja.    

1. Váltás toohello **diagnosztika** az adatkezelési átjáró konfigurációkezelőjének fülre.

    ![Felügyeleti átjáró Diagnostics lapon](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-diagnostics-tab.png)
2. Kattintson a **naplók küldése** toosee hello a következő párbeszédpanel megnyitásához.

    ![Felügyeleti átjáró küldése adatnaplók](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-send-logs-dialog.png)
3. (Választható) Kattintson a **naplók megtekintése** tooreview hello eseménynaplóban naplózza.
4. (Választható) Kattintson a **adatvédelmi** tooreview Microsoft web services adatvédelmi nyilatkozatát.
5. Ha elégedett a tooupload kapcsolatos esetén, kattintson a **naplók küldése** tooactually hello elmúlt hét napban tooMicrosoft hibaelhárítási hello naplókat küldeni. Ahogy az alábbi képernyőfelvétel a hello hello küldési-naplók művelet hello állapotának kell megjelennie.

    ![Adatok felügyeleti átjáró küldése naplózza állapota](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-send-logs-status.png)
6. Hello művelet befejezése után, párbeszédpanel jelenik meg a látható módon a következő képernyőkép hello.

    ![Adatok felügyeleti átjáró küldése naplózza állapota](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-send-logs-result.png)
7. Mentse a hello **azonosítója** és ossza meg a Microsoft Support. hello azonosítója használt toolocate hello átjáró naplók hibaelhárítási feltöltött.  hello azonosítója hello eseménynaplójában is menti.  Hello eseményazonosító "25" megtekintésével megtalálja, és ellenőrizze a hello dátuma és időpontja.

    ![Adatok felügyeleti átjáró küldése naplózza az azonosítója](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-send-logs-report-id.png)    

### <a name="archive-gateway-logs-on-gateway-host-machine"></a>Az átjáró állomás számítógépén archív-átjáró naplói
Léteznek olyan forgatókönyvek, ahol átjáró problémák vannak, és közvetlenül nem osztható meg átjáró naplói:

* Manuálisan telepítse hello átjárót, és hello átjáró regisztrálása.
* Az adatkezelési átjáró konfigurációkezelőjének újragenerált kulccsal tooregister hello átjáró meg.
* Toosend naplók meg, és nem lehet csatlakozni a hello átjáró gazdaszolgáltatás.

Ezek a forgatókönyvek az átjáró naplói elmentse egy zip-fájlt, és ossza meg, amikor a Microsoft támogatási szolgálatához. Például ha arra vonatkozó hibaüzenetet kap, amíg hello átjáró mint regisztrálnia jelenik meg a következő képernyőkép hello.   

![Felügyeleti átjáró regisztrációs hiba](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-registration-error.png)

Kattintson a hello **átjáró naplók archiválása** tooarchive hivatkozásra, és mentse a naplókat, és majd hello zip-fájl megosztása a Microsoft támogatási szolgálatához.

![Felügyeleti átjáró archív adatnaplók](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-archive-logs.png)

### <a name="locate-gateway-logs"></a>Keresse meg az átjáró naplói
Részletes átjáró napló információt hello Windows eseménynaplóiban talál.

1. A Windows indítása **Eseménynapló**.
2. Keresse meg a naplókat a hello **alkalmazás- és szolgáltatásnaplók** > **az adatkezelési átjáró** mappát.

 Átjáró kapcsolatos problémák elhárításakor még szintű hibaesemények hello eseménynaplójában keresse meg.

![Az adatkezelési átjáró naplózza az eseménynaplóban](media/data-factory-troubleshoot-gateway-issues/gateway-logs-event-viewer.png)
