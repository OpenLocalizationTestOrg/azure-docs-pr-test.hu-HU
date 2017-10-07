---
title: "a Data Factory adatkezelési átjáró aaaData |} Microsoft Docs"
description: "Egy adatok átjáró toomove adatok között a helyszíni és hello beállítása felhő. Használja az adatkezelési átjáró Azure Data Factory toomove adatait."
services: data-factory
documentationcenter: 
author: nabhishek
manager: jhubbard
editor: monicar
ms.assetid: b9084537-2e1c-4e96-b5bc-0e2044388ffd
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: abnarain
ms.openlocfilehash: 6f523891a6230cbc7b407f46f4b02d91dfd2cf46
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="data-management-gateway"></a>Adatkezelési átjáró
hello az adatkezelési átjáró egy olyan ügyfélügynök, telepítenie kell a helyszíni környezet toocopy adatok között felhő- és a helyszíni adattárolókhoz. hello a helyszíni adatok adat-előállító által támogatott tárolók hello szereplő [támogatott adatforrások](data-factory-data-movement-activities.md#supported-data-stores-and-formats) szakasz.

Ez a cikk kiegészíti a hello hello forgatókönyv [adatok áthelyezése között a helyszíni és felhőalapú adattároló](data-factory-move-data-between-onprem-and-cloud.md) cikk. Hello forgatókönyv hozzon létre egy folyamatot, amely egy helyi SQL Server adatbázis tooan Azure blob hello átjáró toomove adatokat használ. Ez a cikk hello az adatkezelési átjáró részletes részletes információkat nyújt. 

Az adatkezelési átjáró kiterjesztése hello átjáró több helyszíni gépet társít. Méretezheti növelésével csomóponton egyidejűleg futtatható az adatátviteli feladatok száma legfeljebb. Ez a szolgáltatás egy logikai átjárójának egyetlen csomópont is érhető el. Lásd: [méretezés az adatkezelési átjáró az Azure Data Factory](data-factory-data-management-gateway-high-availability-scalability.md) cikkben alább.

> [!NOTE]
> Átjáró jelenleg csak a hello másolási tevékenység és a tárolt eljárási tevékenység adat-előállítóban. Már nem lehetséges toouse hello átjáró egy egyéni tevékenység tooaccess a helyszíni adatforrásokból.      

## <a name="overview"></a>Áttekintés
### <a name="capabilities-of-data-management-gateway"></a>Az adatkezelési átjáró képességei
Az adatkezelési átjáró hello a következő lehetőségeket biztosítja:

* Modell a helyszíni adatforrások és felhő adatforrások belül azonos adat-előállító hello és áthelyezni az adatokat.
* Figyelési és-kezelés az átjáró állapotának hello adat-előállító oldalról lássák az üveg egytáblás rendelkezik.
* Hozzáférés tooon helyszíni adatforrások biztonságosan kezelése.
  * Nincs változás toocorporate tűzfal szükséges. Átjáró csak lehetővé teszi a kimenő HTTP-alapú kapcsolatok tooopen internet.
  * A helyszíni adattárolókhoz a tanúsítványhoz tartozó hitelesítő adatok titkosításához.
* Hatékony áthelyezni az adatokat – adatátvitel automatikus újrapróbálkozási logika párhuzamos, rugalmas toointermittent hálózati problémákat.

### <a name="command-flow-and-data-flow"></a>Parancs folyamata és adatfolyama
A másolási tevékenység toocopy adatok a helyszíni és a felhő között használatakor hello tevékenység használja egy átjáró tootransfer adatokat a helyszíni adatok forrás toocloud, és ez fordítva is igaz.

Ez hello magas szintű adatok áramlását és adatátjáró Sablonhivatkozás lépései összefoglalása: ![átjáró adatfolyama](./media/data-factory-data-management-gateway/data-flow-using-gateway.png)

1. Adatok fejlesztői átjárót hoz létre egy Azure Data Factory használatával vagy hello [Azure-portálon](https://portal.azure.com) vagy [PowerShell-parancsmag](https://msdn.microsoft.com/library/dn820234.aspx).
2. Adatok fejlesztői hello átjáró megadásával hoz létre egy helyszíni adattároló összekapcsolt szolgáltatás. Hello beállításakor társított szolgáltatás, mert adatok fejlesztő hello hitelesítő adatok beállítása a toospecify hitelesítési típusok és a hitelesítő adatokat.  hello hitelesítő adatok beállítása a alkalmazás párbeszédpanel kommunikál hello adatok tootest kapcsolat és hello átjáró toosave hitelesítő adatok tárolására.
3. Átjáró hello hitelesítő adatok (adatok fejlesztő megadva), hello átjáróhoz társított hello tanúsítvánnyal hello felhőben hello hitelesítő adatok mentése előtt titkosítja.
4. Data Factory szolgáltatásnak hello átjáró ütemezés & a felügyeleti feladatok egy közös Azure service bus-üzenetsort használó vezérlő csatornán keresztül kommunikál. Ha a másolási tevékenység feladat toobe kezdődött el, a Data Factory várólisták hello kérelem együtt hitelesítő adatokat. Átjáró másolattól hello feladat lekérdezési hello várólista után.
5. hello átjáró visszafejti hello azonos tanúsítványt, és csatlakoztatja a megfelelő hitelesítési típus és a hitelesítő adatok toohello helyszíni adattár hello hitelesítő adatokat.
6. hello átjáró másolja az adatokat a helyszíni tárolási tooa felhő tárolóból, vagy fordítva attól függően, hogy a másolási tevékenység hello hello adatok feldolgozási konfigurálását. Ebben a lépésben hello átjáró közvetlenül kommunikál a felhőalapú tárolási szolgáltatások például az Azure Blob Storage egy biztonságos csatornán (HTTPS).

### <a name="considerations-for-using-gateway"></a>Átjáró használatának szempontjai
* Az adatkezelési átjáró egyetlen példányán több helyszíni adatforrások használható. Azonban **átjáró egyetlen példányán a feltételekhez tooonly egy az Azure data factory** és nem lehet megosztani az egy másik data factoryvel.
* Akkor is **csak egy példányát az adatkezelési átjáró** egy gépen telepítve. Tegyük fel, amelyeket a helyszíni adatforrások tooaccess két adat-előállítók rendelkezik, a két helyszíni számítógépeken tooinstall átjárók szüksége van. Ez azt jelenti az átjáró az adott adat-előállító kapcsolt tooa
* Hello **átjáró nem kell azonos számítógépre hello adatforrásként hello a toobe**. Azonban szorosabb toohello adatforrás átjáró csökkenti a hello idő hello átjáró tooconnect toohello az adatforrásra vonatkozóan. Azt javasoljuk, hogy a gép, amely egy adott adatforrást tartalmaz a helyszíni hello különböző hello átjáró telepítése. Ha hello átjáró és az adatforrás a különböző gépeken, hello átjáró nem "versenyeznek" az adatforrás erőforrásokhoz.
* Akkor is **több átjáró különböző gépeken toohello csatlakozás egy helyszíni adatforrás**. Például előfordulhat, hogy két átjáró két adat-előállítók, de egy helyszíni adatforrás regisztrálva van az mindkét hello adat-előállítók hello szolgál.
* Ha már van egy átjáró telepíthető a számítógép szolgál egy **Power BI** esetben telepítése egy **az Azure Data Factory külön átjáró** egy másik számítógépen.
* Átjáró kell használni, még akkor is, ha használ **ExpressRoute**.
* Az adatforrás tekinti egy helyszíni adatforrás (tűzfal mögött van) is használatos **ExpressRoute**. Használja a tooestablish átjárókapcsolat hello hello szolgáltatást és hello adatforrás között.
* Meg kell **hello átjáró** akkor is, ha a hello felhőben van hello adattár egy **Azure IaaS virtuális**.

## <a name="installation"></a>Telepítés
### <a name="prerequisites"></a>Előfeltételek
* hello támogatott **operációs rendszer** azok Windows 7, Windows 8 vagy 8.1, Windows 10, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 verziók. Hello az adatkezelési átjáró egy olyan tartományvezérlő telepítése jelenleg nem támogatott.
* .NET-keretrendszer 4.5.1-es vagy újabb verzió szükséges. Ha átjáró a Windows 7 számítógépen telepíti, telepítse a .NET-keretrendszer 4.5-ös vagy újabb. Lásd: [.NET-keretrendszer rendszerkövetelmények](https://msdn.microsoft.com/library/8z6watww.aspx) részleteiről.
* ajánlott hello **konfigurációs** hello átjáró számítógépén legalább 2 GHz, 4 mag, 8 GB RAM és 80 GB-os lemez van.
* Ha hello gazdaszámítógépen hibernálás hello átjáró toodata kérelmek nem válaszol. Ezért, konfigurálja a megfelelő **energiaséma** hello számítógépen hello-átjáró telepítése előtt. Ha hello gép konfigurált toohibernate, hello átjáró telepítésének megadását kéri az üzenetet.
* Rendszergazdaként bejelentkezve hello gép tooinstall és hello az adatkezelési átjáró sikeresen konfigurálnia kell. Hozzáadhat további felhasználók toohello **az adatkezelési átjáró felhasználók** helyi Windows-csoport. hello e csoport tagjai képes toouse hello **az adatkezelési átjáró konfigurációkezelőjének** eszköz tooconfigure hello átjáró.

Az adott gyakoriságát a hiba akkor fordulhat elő másolja a tevékenység fut, mint hello Erőforrás kihasználtsága (Processzor, memória) hello gépen is a következő hello azonos mintát csúcs és üresjárati idő. Erőforrás-használat is függ, erősen hello áthelyezett adatok mennyiségét. Ha több másolási feladat van folyamatban, megjelenik az Erőforrás kihasználtsága csúcsidőben feljebb.

### <a name="installation-options"></a>Telepítési beállítások
Az adatkezelési átjáró hello a következő módokon telepíthető:

* Az MSI-telepítő csomag letöltésére hello által [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=39717).  hello MSI is használt tooupgrade meglévő felügyeleti adatátjáró toohello legújabb verziója, az összes beállítás megőrzi.
* Kattintva **töltse le és telepítse az adatátjáró** manuális telepítés alatt vagy **telepíthető közvetlenül a számítógépen** EXPRESSZ telepítés alatt. Lásd: [helyezze át az adatokat a helyszíni és a felhő között](data-factory-move-data-between-onprem-and-cloud.md) cikk lépéseit a gyors telepítés használatával. hello manuális lépés viszi toohello letöltőközpontból.  hello utasításokat letöltése és telepítése hello átjáró letöltőközpontból hello a következő szakaszban találhatók.

### <a name="installation-best-practices"></a>Gyakorlati tanácsok a telepítéshez:
1. Energiaséma konfigurálása, hello gazdagépen hello átjáró számára, hogy hello gép hibernálásra nem. Ha hello gazdaszámítógépen hibernálás hello átjáró toodata kérelmek nem válaszol.
2. Készítsen biztonsági másolatot hello átjáró társított hello tanúsítvány.

### <a name="install-hello-gateway-from-download-center"></a>A letöltőközpontból hello átjáró telepítése
1. Keresse meg a túl[Microsoft adatkezelési átjáró letöltési oldala](https://www.microsoft.com/download/details.aspx?id=39717).
2. Kattintson a **letöltése**, válassza ki a megfelelő verzió hello (**32 bites** vs. **64 bites**), és kattintson a **következő**.
3. Futtassa a hello **MSI** közvetlenül vagy tooyour merevlemezre mentheti, és futtassa.
4. A hello **üdvözlő** lapon jelölje be a **nyelvi** kattintson **következő**.
5. **Fogadja el** hello a végfelhasználói licencszerződést, és kattintson a **következő**.
6. Válassza ki **mappa** tooinstall hello átjáró, és kattintson a **következő**.
7. A hello **készen tooinstall** kattintson **telepítése**.
8. Kattintson a **Befejezés** toocomplete telepítését.
9. Hello kulcs beszerzése hello Azure-portálon. Című rész hello következő lépéseit.
10. A hello **Register átjáró** oldalán **az adatkezelési átjáró konfigurációkezelőjének** fut a gépen hello a következő lépéseket:
    1. Beillesztés hello kulcs hello szöveg.
    2. Ha úgy gondolja, a **megjelenítése átjárókulcs** toosee hello kulcs szövegét.
    3. Kattintson a **regisztrálása**.

### <a name="register-gateway-using-key"></a>Regisztrálja az átjárót kulcsával.
#### <a name="if-you-havent-already-created-a-logical-gateway-in-hello-portal"></a>Ha még nem hozott létre egy logikai átjáró hello portálon
hello hello portál és a get-hello kulcs egy átjárót toocreate **konfigurálása** lapon, a hello forgatókönyv lépéseit kövesse [helyezze át az adatokat a helyszíni és a felhő között](data-factory-move-data-between-onprem-and-cloud.md) cikk.    

#### <a name="if-you-have-already-created-hello-logical-gateway-in-hello-portal"></a>Ha már létrehozott logikai átjáró hello hello portálon
1. Azure-portálon lépjen a toohello **adat-előállító** lapon, majd kattintson **összekapcsolt szolgáltatások** csempére.

    ![Data Factory lap](media/data-factory-data-management-gateway/data-factory-blade.png)
2. A hello **összekapcsolt szolgáltatások** lapon, jelölje be hello logikai **átjáró** hello portálon létrehozott.

    ![logikai átjáró](media/data-factory-data-management-gateway/data-factory-select-gateway.png)  
3. A hello **Data Gateway** kattintson **töltse le és telepítse az adatátjáró**.

    ![Töltse le a hivatkozás hello portálon](media/data-factory-data-management-gateway/download-and-install-link-on-portal.png)   
4. A hello **konfigurálása** kattintson **hozza létre újra kulcs**. Kattintson az Igen gombra a hello figyelmeztető üzenet, gondosan elolvasása után.

    ![Kulcs újbóli létrehozása](media/data-factory-data-management-gateway/recreate-key-button.png)
5. Kattintson a Másolás gombra következő toohello kulcs. hello kulcsa a vágólapra másolt toohello.

    ![Kulcs másolása](media/data-factory-data-management-gateway/copy-gateway-key.png)

### <a name="system-tray-icons-notifications"></a>Rendszer tálcaikonok / értesítések
hello következő kép bemutatja, néhány hello tálcaikonok látható.

![rendszer Tálcaikonok igazítása](./media/data-factory-data-management-gateway/gateway-tray-icons.png)

Ha kurzor hello rendszer tálcai ikon/értesítési üzenetet, lásd: hello átjáró/frissítés műveletet egy felugró ablakban hello állapotának adatait.

### <a name="ports-and-firewall"></a>Portok és a tűzfalon
Két tűzfal tooconsider szüksége van: **vállalati tűzfal** hello szervezet hello központi útválasztón futó és **Windows tűzfal** konfigurálva démonként hello helyi számítógépen, ahol hello átjáró telepítve van.  

![tűzfalak](./media/data-factory-data-management-gateway/firewalls2.png)

Vállalati tűzfal szinten, konfigurálnia kell a hello következő tartományok és a kimenő portok:

| Tartománynevek | Portok | Leírás |
| --- | --- | --- |
| *. servicebus.windows.net |443, 80 |Az adatátviteli szolgáltatás háttér-kommunikációhoz használt |
| *. core.windows.net |443 |Használt előkészített másolása Azure Blob használatával (Ha be van állítva)|
| *. frontend.clouddatahub.net |443 |Az adatátviteli szolgáltatás háttér-kommunikációhoz használt |


A Windows tűzfal szinten a kimenő portok általában engedélyezve vannak. Ha nem, konfigurálható hello tartományok és portok ennek megfelelően az átjárót működtető gépen.

> [!NOTE]
> 1. A forrás alapján / felül, előfordulhat, hogy toowhitelist további tartományokkal és a kimenő portok a vállalati és a Windows tűzfalon.
> 2. Az egyes felhőalapú adatbázisok (például: [Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-configure-firewall-settings), [Azure Data Lake](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-secure-data#set-ip-address-range-for-data-access)stb), előfordulhat, hogy a tűzfal konfigurációját az átjárót működtető gépen toowhitelist IP-címét.
>
>


#### <a name="copy-data-from-a-source-data-store-tooa-sink-data-store"></a>Adatok másolása egy forrás adatokat tároló tooa fogadó adatokat tároló
Győződjön meg arról, hogy hello tűzfalszabályai engedélyezve vannak a megfelelő hello vállalati tűzfal, a Windows tűzfal hello-átjáró számítógépén, hello adattároló magát. Ezek a szabályok lehetővé teszi, hogy átjáró tooconnect tooboth forrás hello és gyűjtése sikeres volt. Szabályok minden adattároló, amely részt vesz hello másolási műveletek engedélyezése.

Például a toocopy **egy a helyszíni adatokat tároló tooan Azure SQL Database fogadó vagy egy Azure SQL Data Warehouse fogadó**, hello a következő lépéseket:

* Kimenő forgalom engedélyezése **TCP** kommunikációs port **1433** a Windows tűzfal és a vállalati tűzfalon.
* Azure SQL server tooadd hello IP-cím hello átjáró gép toohello listájának engedélyezett IP-címek hello tűzfal beállításainak konfigurálása.

> [!NOTE]
> Ha a tűzfal nem engedélyezi a 1433-as kimenő port, átjáró közvetlenül Azure SQL nem tud hozzáférni. Ebben az esetben használhatja [előkészített másolási](https://docs.microsoft.com/azure/data-factory/data-factory-copy-activity-performance#staged-copy) tooSQL Azure-adatbázis / SQL Azure DW. Ebben a forgatókönyvben csak hello adatátvitelt jelölik a lenne szükséges HTTPS (443-as port).
>
>


### <a name="proxy-server-considerations"></a>Proxy server szempontjai
Ha a vállalati hálózati környezet proxy server tooaccess hello internet, felügyeleti átjáró toouse megfelelő proxy beállításainak megadása. Hello proxy hello kezdeti regisztrációs fázis során állíthatja be.

![Set proxy regisztráció során](media/data-factory-data-management-gateway/SetProxyDuringRegistration.png)

Átjáró hello proxy server tooconnect toohello felhőalapú szolgáltatást használ. Kattintson a **módosítás** hivatkozás kezdeti beállítás során. Megjelenik a hello **proxybeállítást** párbeszédpanel.

![A Konfigurációkezelő használatával állítsa proxy](media/data-factory-data-management-gateway/SetProxySettings.png)

Három konfigurációs lehetőség áll rendelkezésre:

* **Ne használjon proxyt**: átjáró nincs explicit módon bármely proxy tooconnect toocloud szolgáltatást használni.
* **Használja a rendszer proxy**: átjáró hello proxybeállítást, amely a konfigurált diahost.exe.config és diawp.exe.config használja.  Ha nincs olyan proxy diahost.exe.config és diawp.exe.config van konfigurálva, az átjáró csatlakozik toocloud szolgáltatás közvetlenül a proxy áthaladás nélkül.
* **Egyéni proxyt használ**: konfigurálása hello HTTP proxy beállítás toouse átjáró diahost.exe.config és diawp.exe.config konfigurációk használata helyett.  Cím és Port is szükséges.  Felhasználónév és jelszó megadása nem kötelező, attól függően, hogy a proxy hitelesítési beállítást.  Minden beállítás hello átjáró hitelesítő tanúsítványa hello titkosítva, és helyben tárolja, hello átjáró gazdagépen.

hello az adatkezelési átjáró gazdaszolgáltatás a frissített hello proxybeállítások mentése után automatikusan újraindul.

Miután az átjáró sikeresen regisztrálva van, ha szeretné, hogy tooview vagy frissíteni a proxykiszolgáló beállításait, használja az adatkezelési átjáró konfigurációkezelőjének.

1. Indítsa el **az adatkezelési átjáró konfigurációkezelőjének**.
2. Váltás toohello **beállítások** fülre.
3. Kattintson a **módosítás** hivatkozásra **HTTP-Proxy** szakasz toolaunch hello **HTTP-Proxy beállítása** párbeszédpanel.  
4. Miután rákattintott hello **következő** gomb, megjelenik egy figyelmeztető párbeszédpanel esetében az engedély toosave hello proxybeállítást, és indítsa újra a hello átjáró gazdaszolgáltatás kéri.

Megtekintheti és HTTP-proxy frissítse a Configuration Manager eszközzel.

![A Konfigurációkezelő használatával állítsa proxy](media/data-factory-data-management-gateway/SetProxyConfigManager.png)

> [!NOTE]
> Ha korábban beállított proxykiszolgálót NTLM-hitelesítéssel, átjáró gazdaszolgáltatás hello tartományi fiók alatt fut. Hello jelszó később hello tartományi fiók módosításakor megjegyzése tooupdate hello szolgáltatás konfigurációs beállításait, és indítsa újra a ennek megfelelően. Toothis követelmény miatt javasoljuk, hogy egy dedikált tartományi fiókot tooaccess hello proxykiszolgálóra, amely nem követeli meg tooupdate hello gyakran használ.
>
>

### <a name="configure-proxy-server-settings"></a>Proxykiszolgáló-beállításainak konfigurálása
Ha **system proxy használata** hello HTTP proxy beállításnál átjáró hello proxybeállítást diahost.exe.config és diawp.exe.config használja.  Ha nincs olyan proxy diahost.exe.config és diawp.exe.config van megadva, az átjáró csatlakozik toocloud szolgáltatás közvetlenül a proxy áthaladás nélkül. hello alábbi eljárás ismerteti hello diahost.exe.config fájl frissítése.  

1. A Fájlkezelőben hello eredeti fájlt a C:\Program Files\Microsoft Data felügyeleti Gateway\2.0\Shared\diahost.exe.config tooback példányának alkotják.
2. Indítsa el a Notepad.exe rendszergazdaként fut, és nyissa meg a szöveges fájl "C:\Program Files\Microsoft Data felügyeleti Gateway\2.0\Shared\diahost.exe.config. Hello az alapértelmezett címke a System.NET névtérbeli találja, ahogy az a következő kód hello:

         <system.net>
             <defaultProxy useDefaultCredentials="true" />
         </system.net>    

   Proxy kiszolgálóadatok amelyet ezután felvehet, ahogy az alábbi példa hello:

         <system.net>
               <defaultProxy enabled="true">
                     <proxy bypassonlocal="true" proxyaddress="http://proxy.domain.org:8888/" />
               </defaultProxy>
         </system.net>

   További tulajdonságok megengedettek hello proxy címke toospecify hello szükséges beállítások, például scriptLocation belül. Tekintse meg a túl[proxy (hálózati beállítások) elem](https://msdn.microsoft.com/library/sa91de1e.aspx) a szintaxist.

         <proxy autoDetect="true|false|unspecified" bypassonlocal="true|false|unspecified" proxyaddress="uriString" scriptLocation="uriString" usesystemdefault="true|false|unspecified "/>
3. Hello eredeti helyre mentse a hello konfigurációs fájlt, majd indítsa újra az adatkezelési átjáró Gazdaszolgáltatáshoz szolgáltatást, amely átveszi a hello módosítások hello. toorestart hello szolgáltatás: hello vagy hello Vezérlőpult szolgáltatások kisalkalmazását használja **az adatkezelési átjáró konfigurációkezelőjének** > hello kattintson **szolgáltatás leállítása** gombra, majd kattintson az hello **Szolgáltatás elindítása**. Hello szolgáltatás nem indul el, akkor valószínű, hogy egy érvénytelen XML-címke szintaxissal szerkesztették hello alkalmazás konfigurációs fájljában hozzáadta-e.

> [!IMPORTANT]
> Ne feledje tooupdate **mindkét** diahost.exe.config és diawp.exe.config.  


Ezenkívül toothese pontok szükség toomake meg arról, hogy a Microsoft Azure a vállalat engedélyezett. érvényes Microsoft Azure IP-címek listája hello tölthető le: hello [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=41653).

#### <a name="possible-symptoms-for-firewall-and-proxy-server-related-issues"></a>A tűzfal és proxy-kiszolgálóval kapcsolatos problémák lehetséges jelenségek
Hogy a felmerülő hibák hasonló toohello azokat, a következő valószínű hello tűzfalhoz vagy proxyhoz kiszolgáló, amely megakadályozza a kapcsolódó tooData gyári tooauthenticate maga az átjáró tooimproper konfigurációja miatt. Tekintse meg a tooprevious szakasz tooensure a tűzfal és proxy kiszolgáló konfigurációja megfelelő-e.

1. Tooregister hello átjáró használatakor kapni hello hiba a következő: "nem sikerült tooregister hello átjáró kulcsa. Mielőtt újra próbálkozna tooregister hello átjárókulcsot, győződjön meg arról, hogy csatlakoztatott állapotban van az adatkezelési átjáró hello és hello adatkezelési átjáró gazdaszolgáltatás elindult."
2. A Configuration Manager megnyitásakor állapota megjelenik "Kapcsolat" vagy "Csatlakozás". Amikor megtekintik a Windows eseménynaplóiban keresse meg, a "Eseménynapló" > "Alkalmazások és szolgáltatások Logs" > "Az adatkezelési átjáró" hibaüzenetek jelennek meg például a következő hiba hello:`Unable tooconnect toohello remote server`
   `A component of Data Management Gateway has become unresponsive and restarts automatically. Component name: Gateway.`

### <a name="open-port-8050-for-credential-encryption"></a>Nyissa meg a portot 8050 a hitelesítő adatok titkosításához
Hello **hitelesítő adatok beállítása a** alkalmazás használja a bejövő portot hello **8050** toorelay hitelesítő adatok toohello átjáró egy helyszíni beállításakor társított szolgáltatásnak az hello Azure-portálon. Átjáró telepítésekor alapértelmezés szerint hello átjáró telepítése megnyitja hello-átjáró számítógépén.

Ha egy külső tűzfalat használ, manuálisan hello portot 8050 is megnyithatja. Ha futtatja a tűzfallal kapcsolatos probléma átjáró telepítése során, próbálkozhat hello parancs tooinstall hello átjáró következő hello tűzfal konfigurálása nélkül.

    msiexec /q /i DataManagementGateway.msi NOFIREWALL=1

Ha nem tooopen hello port 8050 hello-átjáró számítógépén, használja a hello használatától eltérő mechanizmusok **hitelesítő adatok beállítása a** tooconfigure alkalmazásadatok tárolja a hitelesítő adatait. Használhat például [New-AzureRmDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) PowerShell-parancsmagot. Lásd: [hitelesítő adatok beállítása és biztonsági](#set-credentials-and-securityy) állíthat be szakaszt, hogyan tárolja az adatokat a hitelesítő adatait.

## <a name="update"></a>Frissítés
Alapértelmezés szerint az adatkezelési átjáró automatikusan frissül, amikor hello átjáró egy újabb verziója érhető el. hello átjáró frissítése nem történik meg, amíg minden hello ütemezett feladat befejezése. Nincsenek további feladatok dolgozza fel hello átjáró hello frissítési művelet befejeződéséig. Ha hello frissítés sikertelen, átjáró vissza lesz állítva toohello régi verzióját.

Megjelenik a következő helyek hello hello ütemezett frissítés időpontja:

* hello átjáró tulajdonságlapján hello Azure-portálon.
* Az adatkezelési átjáró konfigurációkezelőjének hello kezdőlapja
* Rendszer tálca értesítési üzenetet.

hello kezdőlap lapján hello az adatkezelési átjáró konfigurációkezelőjének hello frissítési ütemezés jeleníti meg, és hello utolsó idő hello átjáró volt telepítése/frissítése.

![Frissítések ütemezése](media/data-factory-data-management-gateway/UpdateSection.png)

A frissítés hello azonnal, vagy várjon, amíg hello átjáró toobe hello ütemezett időpontban automatikusan frissül. Például hello a következő kép azt mutatja be, akkor hello látható hello átjáró konfigurációkezelőjének együtt hello frissítés gombra, hogy tooinstall kattinthat, azonnal értesítési üzenetet.

![Frissítés a DMG Configuration Managerben](./media/data-factory-data-management-gateway/gateway-auto-update-config-manager.png)

hello értesítési üzenet hello tálcán látható módon hello kép a következő lenne:

![Rendszer tálca üzenet](./media/data-factory-data-management-gateway/gateway-auto-update-tray-message.png)

Lásd a frissítési művelet (Manuális vagy automatikus) hello tálcán hello állapotát. Átjáró konfigurációkezelőjének elindításakor legközelebb látja hello értesítési sáv adott hello átjáró üzenet frissítve lett hivatkozással együtt túl[Mi az az új témakör](data-factory-gateway-release-notes.md).

### <a name="toodisableenable-auto-update-feature"></a>toodisable/enable automatikus frissítési szolgáltatás
Akkor is tiltása/engedélyezése hello automatikus frissítési szolgáltatás hello lépések végrehajtásával:

[Az átjáró egyetlen csomópont]
1. Indítsa el a Windows PowerShell hello-átjáró számítógépén.
2. Váltás toohello C:\Program Files\Microsoft Data felügyeleti Gateway\2.0\PowerShellScript mappát.
3. Futtassa a következő parancs tooturn hello az automatikus frissítés hello szolgáltatás kikapcsolása (Letiltás).   

    ```PowerShell
    .\GatewayAutoUpdateToggle.ps1  -off
    ```
4. tooturn újra be:

    ```PowerShell
    .\GatewayAutoUpdateToggle.ps1  -on  
    ```
[[Többcsomópontos magas rendelkezésre állású és méretezhető átjáró (előzetes verzió)](data-factory-data-management-gateway-high-availability-scalability.md)]
1. Indítsa el a Windows PowerShell hello-átjáró számítógépén.
2. Váltás toohello C:\Program Files\Microsoft Data felügyeleti Gateway\2.0\PowerShellScript mappát.
3. Futtassa a következő parancs tooturn hello az automatikus frissítés hello szolgáltatás kikapcsolása (Letiltás).   

    Átjáró (előzetes verzió) magas rendelkezésre állás szolgáltatással egy extra AuthKey paraméter megadása kötelező.
    ```PowerShell
    .\GatewayAutoUpdateToggle.ps1  -off -AuthKey <your auth key>
    ```
4. tooturn újra be:

    ```PowerShell
    .\GatewayAutoUpdateToggle.ps1  -on -AuthKey <your auth key> 


## Configuration Manager
Once you install hello gateway, you can launch Data Management Gateway Configuration Manager in one of hello following ways:

1. In hello **Search** window, type **Data Management Gateway** tooaccess this utility.
2. Run hello executable **ConfigManager.exe** in hello folder: **C:\Program Files\Microsoft Data Management Gateway\2.0\Shared**

### Home page
hello Home page allows you toodo hello following actions:

* View status of hello gateway (connected toohello cloud service etc.).
* **Register** using a key from hello portal.
* **Stop** and start hello **Data Management Gateway Host service** on hello gateway machine.
* **Schedule updates** at a specific time of hello days.
* View hello date when hello gateway was **last updated**.

### Settings page
hello Settings page allows you toodo hello following actions:

* View, change, and export **certificate** used by hello gateway. This certificate is used tooencrypt data source credentials.
* Change **HTTPS port** for hello endpoint. hello gateway opens a port for setting hello data source credentials.
* **Status** of hello endpoint
* View **SSL certificate** is used for SSL communication between portal and hello gateway tooset credentials for data sources.  

### Diagnostics page
hello Diagnostics page allows you toodo hello following actions:

* Enable verbose **logging**, view logs in event viewer, and send logs tooMicrosoft if there was a failure.
* **Test connection** tooa data source.  

### Help page
hello Help page displays hello following information:  

* Brief description of hello gateway
* Version number
* Links tooonline help, privacy statement, and license agreement.  

## Monitor gateway in hello portal
In hello Azure portal, you can view near-real time snapshot of resource utilization (CPU, memory, network(in/out), etc.) on a gateway machine.  

1. In Azure portal, navigate toohello home page for your data factory, and click **Linked services** tile. 

    ![Data factory home page](./media/data-factory-data-management-gateway/monitor-data-factory-home-page.png) 
2. Select hello **gateway** in hello **Linked services** page.

    ![Linked services page](./media/data-factory-data-management-gateway/monitor-linked-services-blade.png)
3. In hello **Gateway** page, you can see hello memory and CPU usage of hello gateway.

    ![CPU and memory usage of gateway](./media/data-factory-data-management-gateway/gateway-simple-monitoring.png) 
4. Enable **Advanced settings** toosee more details such as network usage.
    
    ![Advanced monitoring of gateway](./media/data-factory-data-management-gateway/gateway-advanced-monitoring.png)

hello following table provides descriptions of columns in hello **Gateway Nodes** list:  

Monitoring Property | Description
:------------------ | :---------- 
Name | Name of hello logical gateway and nodes associated with hello gateway. Node is an on-premises Windows machine that has hello gateway installed on it. For information on having more than one node (up toofour nodes) in a single logical gateway, see [Data Management Gateway - high availability and scalability](data-factory-data-management-gateway-high-availability-scalability.md).    
Status | Status of hello logical gateway and hello gateway nodes. Example: Online/Offline/Limited/etc. For information about these statuses, See [Gateway status](#gateway-status) section. 
Version | Shows hello version of hello logical gateway and each gateway node. hello version of hello logical gateway is determined based on version of majority of nodes in hello group. If there are nodes with different versions in hello logical gateway setup, only hello nodes with hello same version number as hello logical gateway function properly. Others are in hello limited mode and need toobe manually updated (only in case auto-update fails). 
Available memory | Available memory on a gateway node. This value is a near real-time snapshot. 
CPU utilization | CPU utilization of a gateway node. This value is a near real-time snapshot. 
Networking (In/Out) | Network utilization of a gateway node. This value is a near real-time snapshot. 
Concurrent Jobs (Running/ Limit) | Number of jobs or tasks running on each node. This value is a near real-time snapshot. Limit signifies hello maximum concurrent jobs for each node. This value is defined based on hello machine size. You can increase hello limit tooscale up concurrent job execution in advanced scenarios, where CPU/memory/network is under-utilized, but activities are timing out. This capability is also available with a single-node gateway (even when hello scalability and availability feature is not enabled).  
Role | There are two types of roles in a multi-node gateway – Dispatcher and worker. All nodes are workers, which means they can all be used tooexecute jobs. There is only one dispatcher node, which is used toopull tasks/jobs from cloud services and dispatch them toodifferent worker nodes (including itself).

In this page, you see some settings that make more sense when there are two or more nodes (scale out scenario) in hello gateway. See [Data Management Gateway - high availability and scalability](data-factory-data-management-gateway-high-availability-scalability.md) for details about setting up a multi-node gateway.

### Gateway status
hello following table provides possible statuses of a **gateway node**: 

Status  | Comments/Scenarios
:------- | :------------------
Online | Node connected tooData Factory service.
Offline | Node is offline.
Upgrading | hello node is being auto-updated.
Limited | Due tooConnectivity issue. May be due tooHTTP port 8050 issue, service bus connectivity issue, or credential sync issue. 
Inactive | Node is in a configuration different from hello configuration of other majority nodes.<br/><br/> A node can be inactive when it cannot connect tooother nodes. 


hello following table provides possible statuses of a **logical gateway**. hello gateway status depends on statuses of hello gateway nodes. 

Status | Comments
:----- | :-------
Needs Registration | No node is yet registered toothis logical gateway
Online | Gateway Nodes are online
Offline | No node in online status.
Limited | Not all nodes in this gateway are in healthy state. This status is a warning that some node might be down! <br/><br/>Could be due toocredential sync issue on dispatcher/worker node. 

## Scale up gateway
You can configure hello number of **concurrent data movement jobs** that can run on a node tooscale up hello capability of moving data between on-premises and cloud data stores. 

When hello available memory and CPU are not utilized well, but hello idle capacity is 0, you should scale up by increasing hello number of concurrent jobs that can run on a node. You may also want tooscale up when activities are timing out because hello gateway is overloaded. In hello advanced settings of a gateway node, you can increase hello maximum capacity for a node. 
  

## Troubleshooting gateway issues
See [Troubleshooting gateway issues](data-factory-troubleshoot-gateway-issues.md) article for information/tips for troubleshooting issues with using hello data management gateway.  

## Move gateway from one machine tooanother
This section provides steps for moving gateway client from one machine tooanother machine.

1. In hello portal, navigate toohello **Data Factory home page**, and click hello **Linked Services** tile.

    ![Data Gateways Link](./media/data-factory-data-management-gateway/DataGatewaysLink.png)
2. Select your gateway in hello **DATA GATEWAYS** section of hello **Linked Services** page.

    ![Linked Services page with gateway selected](./media/data-factory-data-management-gateway/LinkedServiceBladeWithGateway.png)
3. In hello **Data gateway** page, click **Download and install data gateway**.

    ![Download gateway link](./media/data-factory-data-management-gateway/DownloadGatewayLink.png)
4. In hello **Configure** page, click **Download and install data gateway**, and follow instructions tooinstall hello data gateway on hello machine.

    ![Configure page](./media/data-factory-data-management-gateway/ConfigureBlade.png)
5. Keep hello **Microsoft Data Management Gateway Configuration Manager** open.

    ![Configuration Manager](./media/data-factory-data-management-gateway/ConfigurationManager.png)    
6. In hello **Configure** page in hello portal, click **Recreate key** on hello command bar, and click **Yes** for hello warning message. Click **copy button** next tookey text that copies hello key toohello clipboard. hello gateway on hello old machine stops functioning as soon you recreate hello key.  

    ![Recreate key](./media/data-factory-data-management-gateway/RecreateKey.png)
7. Paste hello **key** into text box in hello **Register Gateway** page of hello **Data Management Gateway Configuration Manager** on your machine. (optional) Click **Show gateway key** check box toosee hello key text.

    ![Copy key and Register](./media/data-factory-data-management-gateway/CopyKeyAndRegister.png)
8. Click **Register** tooregister hello gateway with hello cloud service.
9. On hello **Settings** tab, click **Change** tooselect hello same certificate that was used with hello old gateway, enter hello **password**, and click **Finish**.

   ![Specify Certificate](./media/data-factory-data-management-gateway/SpecifyCertificate.png)

   You can export a certificate from hello old gateway by doing hello following steps: launch Data Management Gateway Configuration Manager on hello old machine, switch toohello **Certificate** tab, click **Export** button and follow hello instructions.
10. After successful registration of hello gateway, you should see hello **Registration** set too**Registered** and **Status** set too**Started** on hello Home page of hello Gateway Configuration Manager.

## Encrypting credentials
tooencrypt credentials in hello Data Factory Editor, do hello following steps:

1. Launch web browser on hello **gateway machine**, navigate too[Azure portal](http://portal.azure.com). Search for your data factory if needed, open data factory in hello **DATA FACTORY** page and then click **Author & Deploy** toolaunch Data Factory Editor.   
2. Click an existing **linked service** in hello tree view toosee its JSON definition or create a linked service that requires a data management gateway (for example: SQL Server or Oracle).
3. In hello JSON editor, for hello **gatewayName** property, enter hello name of hello gateway.
4. Enter server name for hello **Data Source** property in hello **connectionString**.
5. Enter database name for hello **Initial Catalog** property in hello **connectionString**.    
6. Click **Encrypt** button on hello command bar that launches hello click-once **Credential Manager** application. You should see hello **Setting Credentials** dialog box.

    ![Setting credentials dialog](./media/data-factory-data-management-gateway/setting-credentials-dialog.png)
7. In hello **Setting Credentials** dialog box, do hello following steps:
   1. Select **authentication** that you want hello Data Factory service toouse tooconnect toohello database.
   2. Enter name of hello user who has access toohello database for hello **USERNAME** setting.
   3. Enter password for hello user for hello **PASSWORD** setting.  
   4. Click **OK** tooencrypt credentials and close hello dialog box.
8. You should see a **encryptedCredential** property in hello **connectionString** now.

    ```JSON
    {
        "name": "SqlServerLinkedService",
        "properties": {
            "type": "OnPremisesSqlServer",
            "description": "",
            "typeProperties": {
                "connectionString": "data source=myserver;initial catalog=mydatabase;Integrated Security=False;EncryptedCredential=eyJDb25uZWN0aW9uU3R",
                "gatewayName": "adftutorialgateway"
            }
        }
    }
    ```
Ha olyan számítógépen, amelyen eltér a hello átjárót működtető gépen hello portálon elérhető, meg kell győződnie arról, hogy hello hitelesítőadat-kezelője alkalmazás csatlakozni tud-e toohello átjárót működtető gépen. Ha hello alkalmazások nem tudják elérni a hello átjárót működtető gépen, akkor nem teszi lehetővé tooset hitelesítő adatok hello adatforrás és tootest kapcsolat toohello adatforrás.  

Hello használata esetén **hitelesítő adatok beállítása a** alkalmazás, hello portal titkosítja hello hitelesítő adatok megadott hello hello tanúsítvány **tanúsítvány** hello lapján **átjáró A Configuration Manager** hello-átjáró számítógépén.

Ha hello hitelesítő adatok titkosítására egy API-alapú módszert keres, használhatja a hello [New-AzureRmDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) PowerShell parancsmag tooencrypt hitelesítő adatokat. hello parancsmagot, hogy az átjáró konfigurált toouse tooencrypt hello hitelesítő adatokat az hello tanúsítványt használ. Akkor adja hozzá a titkosított hitelesítő toohello **EncryptedCredential** hello eleme **connectionString** a hello JSON. Hello JSON használata hello [New-AzureRmDataFactoryLinkedService](https://msdn.microsoft.com/library/mt603647.aspx) parancsmag vagy a Data Factory Editor hello.

```JSON
"connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=True;EncryptedCredential=<encrypted credential>",
```

Nincs egy további módszer használatával a Data Factory Editor hitelesítő adatok beállítására. Ha létrehoz egy csatolt SQL Server szolgáltatás használatával hello szerkesztőt, és adja meg a hitelesítő adatokat egyszerű szöveges, hello azonosító adatok titkosítása hello Data Factory szolgáltatásnak birtokló tanúsítványt használ. Nem használ, hogy az átjáró az beállított toouse hello tanúsítványt. Lehet, hogy bizonyos esetekben kissé gyorsabb ezt a módszert használja, de napjainkban kevésbé biztonságos. Ezért azt javasoljuk, hogy hajtsa végre ezt a módszert csak fejlesztési/tesztelési célokra.

## <a name="powershell-cmdlets"></a>PowerShell-parancsmagok
Ez a szakasz ismerteti, hogyan toocreate és regisztrál egy átjárót, Azure PowerShell-parancsmagok használatával.

1. Indítsa el **Azure PowerShell** rendszergazdai módban.
2. Jelentkezzen be Azure-fiók tooyour futtatásával hello a következő parancsot, majd gépelje be a Azure hitelesítő adatait.

    ```PowerShell
    Login-AzureRmAccount
    ```
3. Használjon hello **New-AzureRmDataFactoryGateway** parancsmag toocreate logikai átjáró az alábbiak szerint:

    ```PowerShell
    $MyDMG = New-AzureRmDataFactoryGateway -Name <gatewayName> -DataFactoryName <dataFactoryName> -ResourceGroupName ADF –Description <desc>
    ```
    **Példa parancs és a kimeneti**:

    ```
    PS C:\> $MyDMG = New-AzureRmDataFactoryGateway -Name MyGateway -DataFactoryName $df -ResourceGroupName ADF –Description “gateway for walkthrough”

    Name              : MyGateway
    Description       : gateway for walkthrough
    Version           :
    Status            : NeedRegistration
    VersionStatus     : None
    CreateTime        : 9/28/2014 10:58:22
    RegisterTime      :
    LastConnectTime   :
    ExpiryTime        :
    ProvisioningState : Succeeded
    Key               : ADF#00000000-0000-4fb8-a867-947877aef6cb@fda06d87-f446-43b1-9485-78af26b8bab0@4707262b-dc25-4fe5-881c-c8a7c3c569fe@wu#nfU4aBlq/heRyYFZ2Xt/CD+7i73PEO521Sj2AFOCmiI
    ```

1. Azure PowerShell, a kapcsoló toohello mappa: **C:\Program Files\Microsoft Data felügyeleti Gateway\2.0\PowerShellScript\**. Futtatás **RegisterGateway.ps1** hello lokális változó társított **$Key** hello parancs a következő ábrán. Ez a parancsfájl regisztrálja hello ügyfélügynök hello logikai átjáróval korábban létrehozta a gépen telepítve van.

    ```PowerShell
    PS C:\> .\RegisterGateway.ps1 $MyDMG.Key
    ```
    ```
    Agent registration is successful!
    ```
    Hello IsRegisterOnRemoteMachine paraméter használatával regisztrálhatja hello átjáró egy távoli számítógépen. Példa:

    ```PowerShell
    .\RegisterGateway.ps1 $MyDMG.Key -IsRegisterOnRemoteMachine true
    ```
2. Használhatja a hello **Get-AzureRmDataFactoryGateway** parancsmag tooget hello az adat-előállítóban átjárók listája. Ha hello **állapot** látható **online**, az azt jelenti, hogy az átjáró készen áll a toouse.

    ```PowerShell        
    Get-AzureRmDataFactoryGateway -DataFactoryName <dataFactoryName> -ResourceGroupName ADF
    ```
Egy átjárónak hello segítségével eltávolíthatja **Remove-AzureRmDataFactoryGateway** parancsmag és a frissítés leírása hello segítségével átjáró **Set-AzureRmDataFactoryGateway** parancsmagok. A szintaxis és egyéb részletek ezekről a parancsmagokról tekintse meg a Data Factory parancsmag-referencia.  

### <a name="list-gateways-using-powershell"></a>PowerShell-lel átjáróit

```PowerShell
Get-AzureRmDataFactoryGateway -DataFactoryName jasoncopyusingstoredprocedure -ResourceGroupName ADF_ResourceGroup
```

### <a name="remove-gateway-using-powershell"></a>Távolítsa el a PowerShell használatával átjáró

```PowerShell
Remove-AzureRmDataFactoryGateway -Name JasonHDMG_byPSRemote -ResourceGroupName ADF_ResourceGroup -DataFactoryName jasoncopyusingstoredprocedure -Force
```


## <a name="next-steps"></a>Következő lépések
* Lásd: [adatok áthelyezése között a helyszíni és felhőalapú adattároló](data-factory-move-data-between-onprem-and-cloud.md) cikk. Hello forgatókönyv hozzon létre egy folyamatot, amely egy helyi SQL Server adatbázis tooan Azure blob hello átjáró toomove adatokat használ.  
