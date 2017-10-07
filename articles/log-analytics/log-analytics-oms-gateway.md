---
title: "aaaConnect számítógépek tooOMS használatával hello OMS átjáró |} Microsoft Docs"
description: "Csatlakoztassa a OMS által felügyelt eszközök és az Operations Manager által figyelt számítógépek hello OMS átjáró toosend adatok toohello OMS szolgáltatással, ha nem rendelkeznek Internet-hozzáféréssel."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: ae9a1623-d2ba-41d3-bd97-36e65d3ca119
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/20/2017
ms.author: magoedte
ms.openlocfilehash: 0cfa8f2fb66016e494f22c780e328be472b5fdee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-computers-without-internet-access-toooms-using-hello-oms-gateway"></a>Csatlakoztassa a számítógépet Internet access tooOMS hello OMS-átjáró használata nélkül

Ez a dokumentum azt ismerteti, hogyan az OMS-vezérelt, és a System Center Operations Manager figyelt számítógépek is küldése toohello OMS adatszolgáltatás nem rendelkeznek Internet-hozzáféréssel. hello OMS-átjárón, amely támogatja a HTTP-bújtatás előre HTTP-proxy van hello HTTP-csatlakozás parancs használatával is adatokat gyűjteni, és küldje el a nevében toohello OMS szolgáltatáshoz.  

hello OMS-átjáró támogatja:

* Azure Automation szolgáltatásbeli hibrid forgatókönyv-feldolgozók  
* Windows rendszerű számítógépek a Microsoft Monitoring Agent hello közvetlenül csatlakoztatott tooan OMS-munkaterület
* Linux rendszerű számítógépek hello Linux OMS-ügynököt a közvetlenül csatlakoztatott tooan OMS-munkaterület  
* A System Center Operations Manager 2012 SP1 az UR7, az Operations Manager 2012 R2 UR3 vagy az Operations Manager 2016 felügyeleti csoportban OMS integrálva.  

Ha az IT-biztonsági házirendeknek nem teszi lehetővé a számítógépek a a hálózati tooconnect toohello Internet, például a pénztári (POS) eszközök és a informatikai szolgáltatásokat támogató kiszolgálókra, de tooconnect kell őket tooOMS toomanage és figyelheti azokat, azok konfigurálható toocommunicate közvetlenül az hello OMS tooreceive átjárókonfiguráció és adatokat továbbítson a részére azok nevében.  Ha ezek a számítógépek hello OMS-ügynök toodirectly konfigurált csatlakozás tooan OMS-munkaterület minden számítógép helyett a hello OMS átjáró kommunikál.  hello átjáró átviszi az adatokat a hello ügynökök tooOMS közvetlenül, nem elemzi a hello adatokat átvitel közben.

Ha az Operations Manager felügyeleti csoport integrálva van az OMS, hello felügyeleti kiszolgálók konfigurált tooconnect toohello OMS átjáró tooreceive konfigurációs adatokat kell, és engedélyezte a hello megoldástól függően az összegyűjtött adatok küldése.  Például az Operations Manager riasztásokat konfigurációelemzés, példánytér vagy kapacitás az toohello felügyeleti kiszolgálót az Operations Manager-ügynökök adatküldés néhány. Egyéb nagy mennyiségű adatok, például az IIS-napló, a teljesítmény és a biztonsági események közvetlenül toohello OMS átjáró küldése.  Ha egy vagy több Operations Manager átjáró kiszolgálók üzembe DMZ vagy más elkülönített hálózati toomonitor rendszerek nem megbízható, nem tud kommunikálni az OMS-átjáró.  Operations Manager átjáró kiszolgálók csak a jelentés tooa felügyeleti kiszolgáló is.  Az OMS-átjáró hello konfigurált toocommunicate az Operations Manager felügyeleti csoport esetén hello proxy konfigurációs adatokat-e automatikusan elosztott tooevery ügynök által felügyelt számítógép, amely konfigurált toocollect adatai a Naplóelemzési, akkor is, ha hello érték üres.    

tooprovide magas rendelkezésre állás a közvetlenül csatlakoztatott vagy a műveletek felügyeleti csoportokról, amelyek kommunikálni OMS hello átjárón keresztül is használhatja a hálózati terheléselosztás tooredirect, és hello forgalom szét több átjárókiszolgáló.  Ha egy átjáró kiszolgáló leáll, hello akkor kimenő forgalomról átirányított tooanother csomópont érhető el.  

Ajánlott hello futtató hello OMS átjáró szoftver toomonitor hello OMS átjáró hello OMS-ügynököt telepíteni, és a teljesítmény- vagy esemény adatok elemzésére. Hello ügynök segítségével hello OMS átjáró ezenkívül azonosítsa be hello szolgáltatásvégpontjaira toocommunicate a szükséges.

Minden ügynök hálózati kapcsolat tooits átjáró rendelkeznie kell, hogy az ügynökök is automatikusan át adatokat tooand hello átjáró. Egy olyan tartományvezérlő telepítése hello átjáró nem ajánlott.

a következő diagram hello adatfolyam a közvetlen ügynökök tooOMS hello átjárókiszolgáló használatával jeleníti meg.  Ügynökök rendelkeznie kell a proxy konfigurációs egyezés hello azonos port hello OMS átjáró tooOMS a konfigurált toocommunicate.  

![közvetlen ügynökkommunikációhoz OMS diagramja](./media/log-analytics-oms-gateway/oms-omsgateway-agentdirectconnect.png)

hello következő folyamatábra szemlélteti adatokat egy Operations Manager felügyeleti csoport tooOMS a.   

![Az Operations Manager kommunikáció OMS diagramja](./media/log-analytics-oms-gateway/oms-omsgateway-opsmgrconnect.png)

## <a name="prerequisites"></a>Előfeltételek

Ha egy számítógép toorun hello OMS átjáró kijelölő, ezen a számítógépen hello következő kell rendelkeznie:

* Windows 10, Windows 8.1, Windows 7
* Windows Server 2016-ot, a Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008
* .NET-keretrendszer 4.5
* Legalább egy 4 magos processzor és 8 GB memóriát

### <a name="language-availability"></a>Nyelvi rendelkezésre állása

hello OMS átjáró hello a következő nyelveken érhető el:

- Kínai (egyszerűsített)
- Kínai (hagyományos)
- cseh
- holland
- Angol
- francia
- német
- magyar
- olasz
- japán
- koreai
- lengyel
- Portugál (brazíliai)
- Portugál (Portugália)
- orosz
- Spanyol (nemzetközi)

## <a name="download-hello-oms-gateway"></a>Töltse le a hello OMS-átjáró

Nincsenek három módon tooget hello legújabb verziójának hello OMS Gateway Setup fájlt.

1. Töltse le a hello [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=54443).

2. Töltse le a hello OMS-portálon.  Tooyour OMS-munkaterület a bejelentkezést, keresse meg túl**beállítások** > **csatlakoztatott források** > **Windows kiszolgálók** kattintson**Töltse le az OMS-átjáró**.

3. Töltse le a hello [Azure-portálon](https://portal.azure.com).  Miután bejelentkezik:  

   1. Keresse meg a szolgáltatás hello listája, majd válassza ki **Naplóelemzési**.  
   2. Jelöljön ki egy munkaterületet.
   3. A munkaterület panelen a **általános**, kattintson a **gyors üzembe helyezés**.
   4. A **válassza ki az adatok forrás tooconnect toohello munkaterület**, kattintson a **számítógépek**.
   5. A hello **közvetlen ügynök** panelen kattintson a **OMS-átjáró letöltése**.<br><br> ![Töltse le az OMS-átjáró](./media/log-analytics-oms-gateway/download-gateway.png)


## <a name="install-hello-oms-gateway"></a>Hello OMS-átjáró telepítése

tooinstall átjárót, hajtsa végre a lépéseket követve hello.  Ha egy korábbi verzióját telepítette, korábbi nevén *napló Analytics továbbító*, lesz toothis kiadás frissítése.  

1. Hello célmappát, kattintson duplán **OMS Gateway.msi**.
2. A hello **üdvözlő** kattintson **következő**.<br><br> ![Átjáró telepítővarázslója](./media/log-analytics-oms-gateway/gateway-wizard01.png)<br>
3. A hello **licencszerződés** lapon jelölje be **elfogadom a licencszerződés hello hello feltételeit** tooagree toohello végfelhasználói LICENCSZERZŐDÉST, majd **következő**.
4. A hello **Port és a proxy címét** lap:
   1. Típus hello TCP port száma toobe hello átjáróként használt. A telepítő konfigurálja egy bejövő forgalomra vonatkozó szabály ezen a portszámon, a Windows tűzfalat.  hello alapértelmezett érték: 8080-as.
      hello érvényes hello portszám tartománya 1-65535. Ha hello bemeneti nem esik a tartományba, hibaüzenet jelenik meg.
   2. Ha hello kiszolgálóra, amelyen hello átjáró telepítése egy proxyn keresztül kell toocommunicate megadhat ahol hello regisztrálnia kell az tooconnect hello proxykiszolgáló címe. Például: `http://myorgname.corp.contoso.com:80`.  Ha üres, hello átjáró megpróbál tooconnect toohello Internet közvetlenül.  Ha a proxykiszolgálóhoz hitelesítés szükséges, adja meg a felhasználónevet és jelszót.<br><br> ![Átjáró varázsló proxy konfigurálása](./media/log-analytics-oms-gateway/gateway-wizard02.png)<br>   
   3. Kattintson a **Tovább** gombra.
5. Ha nem rendelkezik a Microsoft Update szolgáltatást, hello Microsoft Update lap jelenik meg, adja meg a tooenable azt. Válasszon ki egy, és kattintson a **következő**. Ellenkező esetben folytassuk a toohello következő lépésre.
6. A hello **célmappa** lap, vagy hagyja hello alapértelmezett C:\Program Files\OMS átjáró vagy típus hello mappahelyet Ha szeretné, hogy tooinstall átjáró, és kattintson a **következő**.
7. A hello **készen tooinstall** kattintson **telepítése**. Felhasználói fiókok felügyelete kérelmező engedély tooinstall jelenhet meg. Ha igen, kattintson a **Igen**.
8. Kattintson a telepítés után **Befejezés**. Ellenőrizheti, hogy hello szolgáltatás hello services.msc beépülő modul megnyitásával fut, és ellenőrizze, hogy **OMS átjáró** állapota a szolgáltatások és hello listájában jelenik meg az **futtató**.<br><br> ![Szolgáltatások – OMS-átjáró](./media/log-analytics-oms-gateway/gateway-service.png)  

## <a name="configure-network-load-balancing"></a>A hálózati terheléselosztás konfigurálása
A hálózati terheléselosztás (NLB) a Microsoft hálózati terheléselosztó terheléselosztási (NLB) vagy a hardveres terheléselosztó használatával magas rendelkezésre állású hello átjárót konfigurálhatja.  hello terheléselosztó kezeli a forgalom átirányításával hello kért a csomópontokon keresztüli kapcsolódásának hello OMS-ügynököt vagy Operations Manager felügyeleti kiszolgálón. Ha egy átjáró kiszolgáló leáll, a hello forgalom lekérdezi átirányított tooother csomópontok.

toolearn hogyan toodesign és a Windows Server 2016-os hálózati terheléselosztási fürt központi telepítése, lásd: [a hálózati terheléselosztás](https://technet.microsoft.com/windows-server-docs/networking/technologies/network-load-balancing).  hello következő lépések bemutatják, hogyan tölthető be a tooconfigure a Microsoft hálózati terheléselosztási fürt.  

1.  Bejelentkezés hello Windows-kiszolgálóra, amely rendszergazdai jogosultságokkal rendelkező fiókkal hello hálózati Terheléselosztási fürt tagja.  
2.  Hálózati terheléselosztás kezelőjének megnyitása a Kiszolgálókezelőben, kattintson a **eszközök**, és kattintson a **hálózati terheléselosztás kezelője**.
3. tooconnect egy OMS átjárókiszolgálót hello Microsoft Monitoring Agent telepítése, kattintson a jobb gombbal a hello fürt IP-címét, és kattintson **gazdagép hozzáadása tooCluster**.<br><br> ![A hálózati terheléselosztás kezelőjével – állomás tooCluster hozzáadása](./media/log-analytics-oms-gateway/nlb02.png)<br>
4. Adja meg, amelyet az tooconnect hello átjárókiszolgáló hello IP-címét.<br><br> ![A hálózati terheléselosztás kezelőjével – hozzáadása a gazdagép tooCluster: csatlakozás](./media/log-analytics-oms-gateway/nlb03.png)

## <a name="configure-oms-agent-and-operations-manager-management-group"></a>OMS-ügynököt, és az Operations Manager felügyeleti csoport konfigurálása
hello következő szakasz tartalmazza bemutatjuk, hogyan tooconfigure közvetlenül kapcsolódó OMS ügynököket, az Operations Manager felügyeleti csoport vagy Azure Automation hibrid forgatókönyv-feldolgozók hello OMS átjáró toocommunicate az OMS Szolgáltatáshoz.  

toounderstand követelmények és bemutatjuk, hogyan tooinstall hello OMS-ügynököt a Windows rendszerű számítógépeken közvetlenül csatlakozik a tooOMS, lásd: [csatlakozás Windows számítógépek tooOMS](log-analytics-windows-agents.md) vagy a Linux rendszerű számítógépek lásd [csatlakozás Linux rendszerű számítógépek tooOMS](log-analytics-linux-agents.md).

### <a name="configuring-hello-oms-agent-and-operations-manager-toouse-hello-oms-gateway-as-a-proxy-server"></a>A proxykiszolgáló hello OMS-ügynököt, és az Operations Manager toouse hello OMS-átjáró konfigurálása

### <a name="configure-standalone-oms-agent"></a>Önálló OMS-ügynököt konfigurálása
Lásd: [proxy és tűzfal beállításainak konfigurálása a Microsoft Monitoring Agent hello](log-analytics-proxy-firewall.md) egy ügynök toouse proxykiszolgálót, amely ebben az esetben hello átjáró konfigurálásával kapcsolatos információkat.  Ha egy hálózati terheléselosztó mögött több átjárókiszolgáló telepített, akkor a hello OMS állapotügynök proxykonfigurációjának hello NLB hello virtuális IP-címe:<br><br> ![A Microsoft Monitoring Agent tulajdonságai – Proxybeállítások](./media/log-analytics-oms-gateway/nlb04.png)

### <a name="configure-operations-manager---all-agents-use-hello-same-proxy-server"></a>Konfigurálja az Operations Manager – összes ügynököt használja a hello azonos proxykiszolgáló
Az Operations Manager tooadd hello átjáró kiszolgáló konfigurálása.  hello proxykonfigurációt a rendszer automatikusan az Operations Manager ügynök tooall tooOperations-kezelő alkalmazza, akkor is, ha hello beállítás üres.

toouse hello átjáró toosupport Operations Manager, kell rendelkeznie:

* A Microsoft Monitoring Agent (ügynök verziója – **8.0.10900.0** és újabb verziók) hello átjárókiszolgálón telepítve és konfigurálva hello OMS-munkaterület kívánja toocommunicate.
* hello átjáró kell kapcsolódik az internethez, vagy csatlakoztatott tooa proxykiszolgáló, amely lehet.

> [!NOTE]
> Ha nem adja meg a hello átjáró értékét, üres értékeket tooall ügynökök elküldte azokat.


1. Megnyitás hello Operations Manager-konzolra és a **Operations Management Suite**, kattintson a **kapcsolat** , majd **Proxy kiszolgáló konfigurálása**.<br><br> ![Az Operations Manager-Proxy kiszolgáló konfigurálása](./media/log-analytics-oms-gateway/scom01.png)<br>
2. Válassza ki **használja a proxy server tooaccess hello Operations Management Suite** és írja be a hello IP-cím hello OMS átjárókiszolgáló vagy hello hálózati Terheléselosztási virtuális IP-címét. Győződjön meg arról, hogy a kiindulási pont hello `http://` előtag.<br><br> ![Az Operations Manager – a proxykiszolgáló címét](./media/log-analytics-oms-gateway/scom02.png)<br>
3. Kattintson a **Befejezés** gombra. Az Operations Manager-kiszolgálóhoz csatlakoztatott tooyour OMS-munkaterület.

### <a name="configure-operations-manager---specific-agents-use-proxy-server"></a>Konfigurálja az Operations Manager - adott ügynökök proxykiszolgáló használatára
Nagy vagy összetett környezetek csak érdemes az adott kiszolgálók (vagy csoportok) toouse hello OMS-átjárókiszolgáló.  Az ezeken a kiszolgálókon hello Operations Manager ügynök közvetlenül a értékként felülírják hello globális érték hello felügyeleti csoport nem frissíthető.  Ehelyett szükség toooverride hello használt szabály toopush ezeket az értékeket.

> [!NOTE]
> Az azonos konfigurációs módszer lehet a környezetben használt tooallow hello használata több OMS-átjárókiszolgáló.  Például lehet szükség az adott OMS átjáró kiszolgálók toobe megadott terület alapon.

1. Operations Manager-konzol megnyitása hello és select hello **szerzői műveletek** munkaterületen.  
2. Hello szerzői műveletek területen válassza ki **szabályok** hello kattintson **hatókör** hello Operations Manager gombjára. Ha ez a gomb nem érhető el, ellenőrizze, hogy rendelkezik-e egy objektumot, nem pedig mappát jelölt hello figyelés ablaktáblán toomake. Hello **felügyeleti csomag objektumai** párbeszédpanel közös célzott osztályok, csoportok vagy objektumok listáját jeleníti meg.
3. Típus **Állapotfigyelő szolgáltatás** a hello **keressen** mezőben, majd válassza ki a hello listából.  Kattintson az **OK** gombra.  
4. Hello szabály keresése **Advisor Proxy beállítás szabály** hello operatív konzol eszköztárán kattintson **felülbírálja** és ezt követően irányítsa túl**felülbírálás hello Rule\For osztály egy adott objektumához: állapota Szolgáltatás** hello listában válassza ki az egy adott objektumot.  Másik lehetőségként létrehozhat egyéni hello Állapotfigyelő szolgáltatás objektumot tartalmazó csoport hello kiszolgálók tooapply a felülbírálás tooand kívánja, majd hello felülbírálás toothat csoport alkalmazni.
5. A hello **felülbírálás tulajdonságai** párbeszédpanelen kattintson tooplace hello be **felülbírálása** oszlop következő toohello **WebProxyAddress** paraméter.  A hello **felülbírálás értékét** mezőbe írja be a hello URL-címe hello OMS átjáró kiszolgáló biztosítása hello kezdődő `http://` előtag.
   >[!NOTE]
   > Mivel azt már kezeli automatikusan egy felülbírálással hello Microsoft System Center Advisor biztonságos hivatkozás Override felügyeleti csomag tartalmazza a Microsoft System Center Advisor figyelési kiszolgáló csoport hello célzó nem kell tooenable hello szabály.
   >
6. Válassza a felügyeleti csomag a hello **célhelyként szolgáló felügyeleti csomag** listában, vagy hozzon létre egy új lezáratlan felügyeleti csomag kattintva **új**.
7. A módosítások befejeztével kattintson **OK**.

### <a name="configure-for-automation-hybrid-workers"></a>Automatizálási hibrid feldolgozó konfigurálása
Ha Automation hibrid forgatókönyv-feldolgozók a környezetben, hello a lépéseket követve adja meg a manuális, ideiglenes lehetséges megoldások tooconfigure hello átjáró toosupport őket.

Az alábbi lépésekkel hello kell tooknow hello Azure-régió, ahol hello Automation-fiók található. toolocate hello helye:

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).
2. Válassza ki a hello Azure Automation szolgáltatást.
3. Válassza ki a megfelelő hello Azure Automation-fiók.
4. A régió szerinti megtekintéséhez **hely**.<br><br> ![Azure portál – Automation fiókjának helye](./media/log-analytics-oms-gateway/location.png)  

A következő táblák tooidentify hello URL-cím mindegyik helyen hello használata:

**Feladat futásidejű adatok URL-címei**

| **hely** | **URL-CÍME** |
| --- | --- |
| USA északi középső régiója |ncus-jobruntimedata-termék-su1.azure-automation.net |
| Nyugat-Európa |we-jobruntimedata-prod-su1.azure-automation.net |
| USA déli középső régiója |scus-jobruntimedata-prod-su1.azure-automation.net |
| USA 2. keleti régiója |eus2-jobruntimedata-prod-su1.azure-automation.net |
| Közép-Kanada |cc-jobruntimedata-prod-su1.azure-automation.net |
| Észak-Európa |ne-jobruntimedata-prod-su1.azure-automation.net |
| Délkelet-Ázsia |sea-jobruntimedata-prod-su1.azure-automation.net |
| Közép-India |cid-jobruntimedata-prod-su1.azure-automation.net |
| Japán |jpe-jobruntimedata-prod-su1.azure-automation.net |
| Ausztrália |ase-jobruntimedata-prod-su1.azure-automation.net |

**Ügynök szolgáltatás URL-címek**

| **hely** | **URL-CÍME** |
| --- | --- |
| USA északi középső régiója |ncus-agentservice-termék-1.azure-automation.net |
| Nyugat-Európa |a Microsoft-agentservice-termék-1.azure-automation.net |
| USA déli középső régiója |scus-agentservice-termék-1.azure-automation.net |
| USA 2. keleti régiója |eus2-agentservice-termék-1.azure-automation.net |
| Közép-Kanada |a CC-agentservice-termék-1.azure-automation.net |
| Észak-Európa |Ne-agentservice-termék-1.azure-automation.net |
| Délkelet-Ázsia |tenger-agentservice-termék-1.azure-automation.net |
| Közép-India |CID-agentservice-termék-1.azure-automation.net |
| Japán |jpe-agentservice-termék-1.azure-automation.net |
| Ausztrália |ASE-agentservice-termék-1.azure-automation.net |

Ha a számítógép regisztrálva van, a hibrid forgatókönyv-feldolgozók automatikus javítás hello frissítés felügyeleti megoldást használni, kövesse az alábbi lépéseket:

1. Hello OMS átjáró hello feladat futási adataihoz szolgáltatás URL-címek toohello engedélyezett állomások listájához hozzáadni. Például:`Add-OMSGatewayAllowedHost we-jobruntimedata-prod-su1.azure-automation.net`
2. Indítsa újra a hello OMS-átjáró szolgáltatás hello a következő PowerShell-parancsmag segítségével:`Restart-Service OMSGatewayService`

Ha a számítógép előre telepített tooAzure Automation hello hibrid forgatókönyv-feldolgozó regisztrálása parancsmag használatával, kövesse az alábbi lépéseket:

1. Hello ügynök szolgáltatás regisztrációs URL-cím toohello engedélyezett állomások listájához hozzáadni az OMS-átjáró hello. Például:`Add-OMSGatewayAllowedHost ncus-agentservice-prod-1.azure-automation.net`
2. Hello OMS átjáró hello feladat futási adataihoz szolgáltatás URL-címek toohello engedélyezett állomások listájához hozzáadni. Például:`Add-OMSGatewayAllowedHost we-jobruntimedata-prod-su1.azure-automation.net`
3. Hello OMS-átjáró szolgáltatás újraindítása.
    `Restart-Service OMSGatewayService`

## <a name="useful-powershell-cmdlets"></a>Hasznos PowerShell-parancsmagok
Parancsmagok segítségével végezze el a szükséges tooupdate hello OMS átjáró konfigurációs beállítások feladatokat. Mielőtt azokat használni, ügyeljen arra, hogy:

1. Telepítse a hello OMS-átjáró (MSI).
2. Nyisson meg egy PowerShell-konzol ablakot.
3. tooimport hello modul, futtassa az alábbi parancsot:`Import-Module OMSGateway`
4. Ha nincs hiba történt az előző lépésben hello, hello modul importálása sikeres volt, és hello parancsmag is használható. Típusa`Get-Module OMSGateway`
5. Módosítása után hello parancsmagokkal, győződjön meg arról, hogy hello átjáró szolgáltatás újraindítása.

Ha hibaüzenetet kap a 3. lépésben, hello modul nem importálható. hello hiba akkor fordulhat elő, amikor PowerShell nem toofind hello modul. Hello átjáró telepítési elérési úton található: *C:\Program Files\Microsoft OMS Gateway\PowerShell*.

| **A parancsmag** | **Paraméterek** | **Leírás** | **Példa** |
| --- | --- | --- | --- |  
| `Get-OMSGatewayConfig` |Kulcs |Hello szolgáltatás hello konfigurációjának beolvasása |`Get-OMSGatewayConfig` |  
| `Set-OMSGatewayConfig` |Kulcs (kötelező) <br> Érték |Módosítások hello hello szolgáltatás konfigurációját |`Set-OMSGatewayConfig -Name ListenPort -Value 8080` |  
| `Get-OMSGatewayRelayProxy` | |Továbbító (fölérendelt) proxy hello címét |`Get-OMSGatewayRelayProxy` |  
| `Set-OMSGatewayRelayProxy` |Cím<br> Felhasználónév<br> Jelszó |Beállítja a hello cím (és a hitelesítő adatok) továbbítási (fölérendelt) proxy |1. Állítsa be a továbbító proxy és a hitelesítő adat:<br> `Set-OMSGatewayRelayProxy`<br>`-Address http://www.myproxy.com:8080`<br>`-Username user1 -Password 123` <br><br> 2. A továbbító proxy hitelesítést nem igénylő beállításait:`Set-OMSGatewayRelayProxy`<br> `-Address http://www.myproxy.com:8080` <br><br> 3. Törölje a jelet hello továbbítási proxybeállítást:<br> `Set-OMSGatewayRelayProxy` <br> `-Address ""` |  
| `Get-OMSGatewayAllowedHost` | |Lekérdezi hello jelenleg engedélyezett a gazdagép, (csak hello helyileg konfigurált engedélyezett állomás, nem tartalmaz engedélyezett gazdagépek automatikusan letöltött) |`Get-OMSGatewayAllowedHost` |
| `Add-OMSGatewayAllowedHost` |Állomás (kötelező) |Hozzáadja a hello állomás toohello engedélyezettek listájához |`Add-OMSGatewayAllowedHost -Host www.test.com` |  
| `Remove-OMSGatewayAllowedHost` |Állomás (kötelező) |Hello gazdagép eltávolítása hello engedélyezettek listájához |`Remove-OMSGatewayAllowedHost`<br> `-Host www.test.com` |  
| `Add-OMSGatewayAllowedClientCertificate` |Tulajdonos (kötelező) |Hozzáadja a hello ügyfél tanúsítvány tárgy toohello engedélyezettek listájához |`Add-OMSGatewayAllowed`<br>`ClientCertificate` <br> `-Subject mycert` |  
| `Remove-OMSGatewayAllowedClientCertificate` |Tulajdonos (kötelező) |Ügyfél-tanúsítvány tulajdonosának hello eltávolítása hello engedélyezettek listájához |`Remove-OMSGatewayAllowed` <br> `ClientCertificate` <br> `-Subject mycert` |  
| `Get-OMSGatewayAllowedClientCertificate` | |Lekérdezi hello jelenleg engedélyezett ügyfél tanúsítványtulajdonosok (csak hello helyileg engedélyezett témák beállítva, nem tartalmaz engedélyezett témák automatikusan letöltött) |`Get-`<br>`OMSGatewayAllowed`<br>`ClientCertificate` |  

## <a name="troubleshooting"></a>Hibaelhárítás
toocollect a hello átjáró által naplózott eseményeket, kell tooalso hello OMS-ügynök telepítve van.<br><br> ![Eseménynapló – OMS átjáró napló](./media/log-analytics-oms-gateway/event-viewer.png)

**OMS átjáró esemény azonosítók és leírások**

hello következő táblázatban látható hello eseményazonosítók és OMS átjáró naplóeseményeket leírását.

| **Azonosító** | **Leírás** |
| --- | --- |
| 400 |Bármely alkalmazás hiba, amely nem rendelkezik egyedi azonosítója |
| 401 |Helytelen konfiguráció. Például: listenPort = "text" helyett egész szám |
| 402 |Kivétel fordult elő a TLS-kézfogás üzenet elemzése |
| 403 |Hálózati hiba történt. Például: tootarget kiszolgáló nem tud csatlakozni. |
| 100 |Általános információk |
| 101 |Szolgáltatás elindult |
| 102 |Szolgáltatás leállt |
| 103 |HTTP-Csatlakozás parancsot kapott az ügyféltől |
| 104 |Nem egy HTTP-csatlakozás parancs |
| 105 |Célkiszolgáló nem engedélyezett listában vagy hello célport nem biztonságos portja (443) <br> <br> Győződjön meg arról, hogy hello MMA ügynök az átjáró kiszolgálón, és hello átjáró folytatott kommunikáció hello ügynökök csatlakoztatott toohello ugyanazt a Naplóelemzési munkaterület. |
| 105 |Hiba TcpConnection – érvénytelen ügyféltanúsítvány: CN = átjáró <br><br> Győződjön meg arról, hogy: <br>    <br> &#149; Átjáró verziószámú 1.0.395.0 használ, vagy nagyobb. <br> &#149; hello MMA ügynök az átjárókiszolgálón és hello átjáró folytatott kommunikáció hello ügynökök is csatlakoztatott toohello ugyanazt a Naplóelemzési munkaterület. |
| 106 |Bármilyen okból hello TLS-munkamenet számára gyanús és visszautasított |
| 107 |TLS-munkamenet hello ellenőrzése megtörtént |

**Teljesítmény számlálók toocollect**

hello következő táblázatban hello teljesítményszámlálók hello OMS átjáró érhető el. Hello számlálókat a Teljesítményfigyelő használata is hozzáadhat.

| **Name (Név)** | **Leírás** |
| --- | --- |
| OMS-aktív átjáró Ügyfélkapcsolat |Aktív ügyfél (TCP) hálózati kapcsolatok száma |
| OMS átjáró/hibák száma |Hibák száma |
| OMS átjáró/csatlakoztatva ügyfél |Kapcsolódó ügyfelek száma |
| OMS átjáró/elutasítása száma |Tooany TLS érvényesítési hiba miatt elutasítások számát |

![OMS-átjáró teljesítményszámlálói](./media/log-analytics-oms-gateway/counters.png)

## <a name="get-assistance"></a>Segítségkérés
Ha már bejelentkezett toohello Azure-portálon, a segítségért hello OMS-átjáró vagy bármely más Azure-szolgáltatások vagy a szolgáltatás funkciója is létrehozhat.
toorequest segítséget hello kérdőjel szimbólum hello jobb felső sarkában hello portálon kattintson, és kattintson a **új támogatja a kérelem**. Kövesse az új támogatási kérelem hello űrlap.

![Új támogatási kérelem](./media/log-analytics-oms-gateway/support.png)

A hello OMS vagy Naplóelemzési visszajelzést is hagyhatja [Microsoft Azure-visszajelzési fórumon](https://feedback.azure.com/forums/267889).

## <a name="next-steps"></a>Következő lépések
* [Adatforrások hozzáadása](log-analytics-data-sources.md) toocollect adatait hello csatlakoztatott források szakaszát az OMS-munkaterület, és tárolja a hello OMS-tárházban.
