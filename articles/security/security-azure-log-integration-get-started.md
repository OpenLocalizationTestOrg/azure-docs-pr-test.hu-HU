---
title: "az Azure naplóelemzés integráció lépései aaaGet |} Microsoft Docs"
description: "Ismerje meg, hogyan tooinstall hello Azure integrációs szolgáltatás naplófájl, és integrálhatja a napló, az Azure storage, Azure-beli Auditnaplók és az Azure Security Center riasztásokat."
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomShinder
ms.assetid: 53f67a7c-7e17-4c19-ac5c-a43fabff70e1
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ums.workload: na
ms.date: 07/26/2017
ms.author: TomSh
ms.custom: azlog
ms.openlocfilehash: 26c19070d76ff73b1bdbd32ba77fb04978af387e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-log-integration-with-azure-diagnostics-logging-and-windows-event-forwarding"></a>Azure Diagnostics-naplózás és a Windows-Eseménytovábbítást Azure naplóelemzés integrációját
Azure napló-integráció (AzLog) lehetővé teszi toointegrate nyers napló, az Azure-erőforrások a helyszíni biztonsági adatai és az esemény felügyeleti SIEM-rendszerekbe. Ez az integráció lehetővé teszi az eszközök egyesített biztonsági irányítópult lehetséges toohave, a helyszíni vagy hello felhőben, így meg tudja-e összesíteni, összefüggéseket, elemzése és az alkalmazások társított biztonsági események riasztás.
>[!NOTE]
További információ Azure napló integrált, tekintse át hello [Azure napló integrációjának áttekintése](https://docs.microsoft.com/azure/security/security-azure-log-integration-overview).

Ez a cikk segít hello Azlog szolgáltatás telepítésének hello összpontosít, és hello szolgáltatás integrálása az Azure Diagnostics Ismerkedés az Azure napló integráció. hello Azure napló integrációs szolgáltatás lesz képes toocollect hello Windows biztonsági esemény csatorna üzembe helyezett Azure IaaS virtuális gépekről a Windows Eseménynapló adatait. Ez a nagyon hasonló túl "Eseménytovábbítást", amely előfordulhat, hogy használja a helyszínen.

>[!NOTE]
>hello képességét toobring hello kimeneti az Azure naplóelemzés integráció toohello SIEM hello maga SIEM által biztosított. Hello cikke [integrálása Azure napló integrálva a helyszíni SIEM](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) további információt.

világossá - toobe hello Azure napló integrációs szolgáltatás fut a fizikai vagy virtuális számítógép által használt hello Windows Server 2008 R2 vagy újabb operációs rendszert (Windows Server 2012 R2 vagy Windows Server 2016 használata javasolt).

hello fizikai számítógép futtathatná a helyszíni (vagy egy tárhelyszolgáltató helyen). Ha úgy dönt, hogy toorun hello Azure napló integrációs szolgáltatás virtuális gépre, hogy a virtuális gép is lehet a helyszínen vagy a nyilvános felhő, például a Microsoft Azure.

hello fizikai vagy virtuális gép hello Azure napló integrációs szolgáltatást futtató igényel a hálózati kapcsolat toohello Azure nyilvános felhőjében. A cikkben ismertetett hello konfigurációs részletekkel szolgálnak.

## <a name="prerequisites"></a>Előfeltételek
Legalább a hello AzLog telepítéséhez a következő elemek hello:
* Egy **Azure-előfizetés**. Ha nem rendelkezik előfizetéssel, regisztrálhat egy [ingyenes fiókkal](https://azure.microsoft.com/free/).
* A **tárfiók** , amely használható a Windows Azure diagnosztikai naplózás (előre beállított tárfiókot használja, vagy hozzon létre egy újat – fog bemutatjuk, hogyan tooconfigure hello tárfiók a cikk későbbi részében)
  >[!NOTE]
  A forgatókönyvtől függően egy tárfiókot nem lehet szükség. Hello Azure diagnostics forgatókönyv szükséges ez a cikk tárgyalja.
* **Két rendszer**: olyan számítógépen, amelyen hello Azure napló integrációs szolgáltatás elindul, és olyan gépen, amelyet figyeli a rendszer és a naplózási információi toohello Azlog szolgáltatás gépek elküldve.
   * A gépre, amelyen szeretné toomonitor – Ez az a virtuális gép futtatásához használt egy [Azure virtuális gépen](../virtual-machines/virtual-machines-windows-overview.md)
   * Olyan számítógépen, amelyen hello Azure naplóelemzés integrációs szolgáltatás; fog futni. Ez a számítógép összes később importálja a saját siem-Rendszerébe hello napló információkat gyűjt.
    * A rendszer lehet helyszíni vagy a Microsoft Azure-ban.  
    * Egy x64 futtató toobe kell a Windows server 2008 R2 SP1 verziója vagy magasabb és .NET 4.5.1-es verziója telepítve van. Azt is meghatározhatja, hello .NET verziója című, következő hello cikkben [hogyan: határozza meg a .NET keretrendszer verziója települt](https://msdn.microsoft.com/library/hh925568)  
    Kapcsolat toohello Azure storage-fiókhoz használt Azure diagnosztikai naplózás kell rendelkeznie. Hogyan ellenőrizheti a kapcsolatot a rendszer nyújtunk a később a jelen cikkben lévő utasítások

Hello folyamat létrehozása a virtuális gépek Azure-portálon hello gyors bemutatója az alábbi videó hello tekintsen meg.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Log-Integration-Videos-Create-a-Virtual-Machine/player]



## <a name="deployment-considerations"></a>Telepítési szempontok
Azure napló integrációs tesztelésekor, használhatja a rendszer hello az operációs rendszer minimális követelményeknek. Azonban egy éles környezetben hello terhelés megkövetelhetik a felfelé skálázással vagy horizontális tooplan.

Hello Azure napló integrációs szolgáltatás (fizikai vagy virtuális gép egy példány) több példányát is futtathatja, ha esemény kötet magas. Ezenkívül is végezze el a Windows (ÜVEGVATTA) és az előfizetések tooprovide toohello példányok száma hello Azure Diagnostics storage-fiókok alapján a kapacitás.
>[!NOTE]
Jelenleg nem találtunk kimenő Azure példányai tooscale napló a integrációs gépek (azaz futó gépek hello Azure naplóelemzés integrációs szolgáltatás), vagy storage-fiókok vagy előfizetések konkrét javaslatokért. Skálázás döntések alapjául a teljesítmény megfigyelések minden ezeket a területeket.

Akkor is hello beállítás tooscale mentése hello Azure napló integrációs szolgáltatás toohelp teljesítmény javításához. a következő metrikák hello hello gépek ezt a módot toorun hello Azure naplóelemzés integrációs szolgáltatás méretezése nyújt segítséget:
* Egy 8 processzor (core) gépen Azlog integráló egyetlen példányát (~1M/hour) naponta körülbelül 24 millió esemény tud feldolgozni.

* A 4-processzor (core) gépen Azlog integráló egyetlen példányát (~62.5K/hour) naponta körülbelül 1,5 millió esemény tud feldolgozni.

## <a name="install-azure-log-integration"></a>Azure naplóelemzés integráció telepítése
Azure napló integrációs tooinstall, kell toodownload hello [Azure naplóelemzés integrációs](https://www.microsoft.com/download/details.aspx?id=53324) telepítőfájlt. Hello beállítása rutin futtatását, és döntse el, ha azt szeretné, tooprovide telemetriai adatokat tooMicrosoft.  

![Telemetria mezőben be van jelölve a telepítési képernyőn](./media/security-azure-log-integration-get-started/telemetry.png)

*
> [!NOTE]
> Azt javasoljuk, hogy engedélyezi-e a Microsoft toocollect telemetriai adatokat. Ez a beállítás jelének kapcsolhatja ki telemetrikus adatok gyűjtését.
>


hello Azure napló integrációs szolgáltatás gyűjt telemetrikus adatokat hello számítógép, amelyen telepítve van.  

Az összegyűjtött telemetrikus adatok van:

* Az Azure naplóelemzés integráció során bekövetkező kivételek
* Hello számáról, lekérdezések és a feldolgozott események metrikák
* Mely Azlog.exe kapcsolatos parancssori kapcsolók használt statisztikák

hello telepítési folyamat alatt videó hello is tartalmazza.
> [!VIDEO https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Log-Integration-Videos-Install-Azure-Log-Integration/player]



## <a name="post-installation-and-validation-steps"></a>Telepítés és ellenőrzési lépések utáni
Hello alapbeállítások rutin befejezése után már csak a Kész lépés tooperform post telepítési és -ellenőrzés lépés:
1. Nyisson meg egy emelt szintű PowerShell ablakot, és keresse meg a túl**c:\Program Files\Microsoft Azure napló integráció**
2. hello első lépéseként tootake kell tooget hello AzLog parancsmagok importálva. Hello parancsfájl futtatásával megtehetné **LoadAzlogModule.ps1** (értesítés hello ". \" hello, a következő parancsot a). Típus **.\LoadAzlogModule.ps1** nyomja le az ENTER **ENTER**.  
Meg kell jelennie hasonlót mi hello az alábbi ábra jelenik meg. </br></br>
![Telemetria mezőben be van jelölve a telepítési képernyőn](./media/security-azure-log-integration-get-started/loaded-modules.png) </br></br>
3. A továbbiakban létre kell tooconfigure AzLog toouse egy adott Azure környezetben. "Környezetet az Azure" hello "type" toowork a kívánt Azure-felhőbe adatközpont. Miközben jelenleg több Azure-környezetek, hello jelenleg vonatkozó beállítások vagy **AzureCloud** vagy **AzureUSGovernment**.   A rendszergazda jogú PowerShell-környezetében, győződjön meg arról, hogy **c:\program files\Microsoft Azure napló integráció\** </br></br>
    Egyszer, parancsot hello: </br>
    ``Set-AzlogAzureEnvironment -Name AzureCloud``(az Azure kereskedelmi)

      >[!NOTE]
      Hello parancs végrehajtása sikeres, ha Ön nem kap olyan visszajelzést.  Ha azt szeretné, hogy toouse hello US Government Azure felhőbe, használja **AzureUSGovernment** (a hello - változó neve) az USA szövetségi felhő hello. Egyéb Azure felhők jelenleg nem támogatottak.  
4. Figyeléséhez a rendszer szüksége lesz Azure Diagnostics hello neve hello tárfiókot használja.  Hello Azure-portálon lépjen a túl**virtuális gépek** , és tekintse meg a hello virtuális gép, amelyet felügyel. A hello **tulajdonságok** területen válasszon **diagnosztikai beállítások**.  Kattintson a **ügynök** és jegyezze fel a tárfiók neve hello megadott. Szüksége lesz a fiók nevét egy későbbi lépésben.
![Az Azure diagnosztikai beállításai](./media/security-azure-log-integration-get-started/storage-account-large.png) </br></br>

      ![Az Azure diagnosztikai beállításai](./media/security-azure-log-integration-get-started/azure-monitoring-not-enabled-large.png)
      >[!NOTE]
      Ha a figyelés nem volt engedélyezve a virtuális gépek létrehozásához az aktiválási hello beállítás tooenable, ahogy fent látható.
5. Most azt a figyelmet hátsó toohello Azure naplóelemzés integrációs gép váltani. Igazolnia kell, hogy kapcsolatot toohello Tárfiók hello rendszerből Azure napló integrációs telepítési tooverify. hello fizikai számítógépre vagy virtuális géphez, hello Azure napló integrációs szolgáltatás kell elérni toohello tooretrieve tárfiókadatok Azure Diagnostics által naplózott, az egyes hello konfigurált figyeli a rendszer.  
  1. Letöltheti a Azure Tártallózó [Itt](http://storageexplorer.com/).
  2. Hello beállítása rutin végigfuttatása
  3. Miután hello telepítésének befejezése után kattintson **következő** és hello jelölőnégyzetet hagyja **indítsa el a Microsoft Azure Tártallózó** be van jelölve.  
  4. Jelentkezzen be tooAzure.
  5. Győződjön meg arról, hogy látja-e az Azure Diagnostics konfigurált hello tárfiók.  
![Tárfiókok](./media/security-azure-log-integration-get-started/storage-account.jpg) </br></br>
   6. Figyelje meg, hogy a storage-fiókok mindössze néhány lehetőség áll rendelkezésre. Az egyik **táblák**. A **táblák** látnia kell egy **WADWindowsEventLogsTable**. </br></br>
   ![Storage-fiókok](./media/security-azure-log-integration-get-started/storage-explorer.png) </br>

## <a name="integrate-azure-diagnostic-logging"></a>Integrálása az Azure diagnosztikai naplózás
Ebben a lépésben a futó hello Azure napló integrációs szolgáltatás tooconnect toohello tárfiók hello naplófájlokat tartalmazó hello gépre konfigurál.
toocomplete ebben a lépésben fel kell néhány dolgot előre.  
* **FriendlyNameForSource:** azt, hogy konfigurálta az hello virtuális gép toostore adatait az Azure diagnosztikai tárfiók toohello is alkalmazható egy rövid nevet
* **StorageAccountName:** hello hello Azure diagnotics konfigurálásakor megadott tárfiók neve.  
* **StorageKey tulajdonságát:** Ez az hello tárolási kulcs hello tárfiók a virtuális gép hello Azure diagnosztikai információk tárolására.  

Hajtsa végre a következő lépéseket tooobtain hello biztonságitár-kulcs hello:
 1. Keresse meg a toohello [Azure-portálon](http://portal.azure.com).
 2. Hello navigációs ablaktábláján található hello Azure konzolon görgessen toohello alsó, kattintson a **további szolgáltatások.**

 ![További szolgáltatások](./media/security-azure-log-integration-get-started/more-services.png)
 3. Adja meg **tárolási** a hello **szűrő** szövegmezőben. Kattintson a **tárfiókok** (Ez akkor jelenik meg, miután megadta **tárolási**)

   ![Szűrő mezőbe](./media/security-azure-log-integration-get-started/filter.png)
 4. Storage-fiókok listája jelenik meg, kattintson duplán a tooLog tárolási rendelt hello fiók.

   ![storage-fiókok listája](./media/security-azure-log-integration-get-started/storage-accounts.png)
 5. Kattintson a **hívóbetűk** a hello **beállítások** szakasz.

  ![Tárelérési kulcsok](./media/security-azure-log-integration-get-started/storage-account-access-keys.png)
 6. Másolás **key1** , és egy biztonságos helyre, amely a következő lépés hello végezheti el.

   ![két tárelérési kulcsok](./media/security-azure-log-integration-get-started/storage-account-access-keys.png)
 7. Nyissa meg egy rendszergazda jogú parancssort (vegye figyelembe, hogy használjuk egy rendszergazda jogú parancssort itt, nem egy rendszergazda jogú PowerShell-konzolt), hogy telepítette-e Azure napló integrációs hello kiszolgálón.
 8. Keresse meg a túl**c:\Program Files\Microsoft Azure napló integráció**
 9. Futtassa a ``Azlog source add <FriendlyNameForTheSource> WAD <StorageAccountName> <StorageKey> `` parancsot. </br> Például ``Azlog source add Azlogtest WAD Azlog9414 fxxxFxxxxxxxxywoEJK2xxxxxxxxxixxxJ+xVJx6m/X5SQDYc4Wpjpli9S9Mm+vXS2RVYtp1mes0t9H5cuqXEw==`` Ha azt szeretné hello előfizetési azonosító tooshow mentése hello eseményben XML, hozzáfűző hello előfizetési azonosító toohello rövid név: ``Azlog source add <FriendlyNameForTheSource>.<SubscriptionID> WAD <StorageAccountName> <StorageKey>`` vagy például``Azlog source add Azlogtest.YourSubscriptionID WAD Azlog9414 fxxxFxxxxxxxxywoEJK2xxxxxxxxxixxxJ+xVJx6m/X5SQDYc4Wpjpli9S9Mm+vXS2RVYtp1mes0t9H5cuqXEw==``

>[!NOTE]  
Várjon legalább too60 percet, majd hello tárfiók kikerülnek hello események megtekintése. tooview, nyissa meg **Eseménynapló > Windows-naplók > továbbított események** a hello Azlog integráló.

Itt láthatja, fent említett címen keresztül hello lépéseket videó.
> [!VIDEO https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Log-Integration-Videos-Enable-Diagnostics-and-Storage/player]


## <a name="what-if-data-is-not-showing-up-in-hello-forwarded-events-folder"></a>Mi történik, ha adat nem látható hello továbbított események mappában?
Ha egy óra múlva adat nem látható a hello **továbbított események** mappát, majd:

1. Ellenőrizze a hello gépen futó hello Azure napló integrációs szolgáltatást, és győződjön meg arról, hogy Azure hozzáférhet. tootest kapcsolatot, próbálja meg tooopen hello [Azure-portálon](http://portal.azure.com) hello böngészőből.
2. Győződjön meg arról, hogy hello felhasználói fiók **Azlog** írási engedéllyel rendelkezik a hello mappa **users\Azlog**.
  <ol type="a">
   <li>Nyissa meg **Windows Explorer** </li>
  <li> Keresse meg a túl**c:\users** </li>
  <li> Kattintson a jobb gombbal **c:\users\Azlog** </li>
  <li> Kattintson a **biztonsági**  </li>
  <li> Kattintson a **NT Service\Azlog** és hello fiókok hello engedélyeinek ellenőrzése. Ha hello fiók hiányzik a ezen a lapon, vagy ha hello megfelelő engedélyek jelenleg nem láthatók is jogokat hello fiók ezen a lapon.</li>
  </ol>
3.Győződjön meg arról, hogy a hozzáadott hello parancs hello tárfiók **Azlog forrás hozzáadása** hello parancs futtatásakor szerepel **Azlog forráslista**.
4. Nyissa meg túl**Eseménynapló > Windows-naplók > alkalmazás** hello Azure naplóelemzés integrációs jelentett toosee, ha hiba történik.


Ha probléma merül fel hello telepítés és konfigurálás során, nyisson meg egy [támogatási kérelem](../azure-supportability/how-to-create-azure-support-request.md), jelölje be **napló integrációs** hello szolgáltatást, amelynek támogatási kérelmet.

Egy másik támogatási lehetőség hello [Azure napló integrációs MSDN fórumon](https://social.msdn.microsoft.com/Forums/home?forum=AzureLogIntegration). Itt hello közösségi támogathat egymással kérdéseket, válaszokat, tippeket és trükkök hogyan tooget hello legtöbb Azure napló integrációs kívül a. Ezenkívül hello Azure napló integrációs csoport ezen a fórumon figyeli, és amikor azt is segít.

## <a name="next-steps"></a>Következő lépések
toolearn Azure napló-nal kapcsolatos további információkért tekintse meg a következő dokumentumok hello:

* [A Microsoft Azure napló integráció az Azure-naplók](https://www.microsoft.com/download/details.aspx?id=53324) – letöltőközpontból részletes rendszerkövetelményeket, és telepítése Azure naplóelemzés integrációs utasításokat.
* [Bevezetés tooAzure napló integrációs](security-azure-log-integration-overview.md) – Ez a dokumentum bemutatja a tooAzure napló integrációs, annak főbb funkcióit és annak működéséről.
* [Partner konfigurációs lépések](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) – ebben a blogbejegyzésben bemutatja, hogyan tooconfigure Azure naplófájl-integráció toowork Splunk, HP ArcSight és az IBM QRadar partneri megoldások. Ez az az aktuális útmutatást hogyan tooconfigure hello SIEM-összetevőket. Ellenőrizze az SIEM gyártójánál, először a további részleteket.
* [Gyakori kérdések (GYIK) integrációs Azure naplóelemzés](security-azure-log-integration-faq.md) -Ez gyakran ismételt kérdések Azure naplóelemzés integrációs kapcsolatos kérdésekre ad választ.
* [A Security Center integrálása az Azure-ral riasztások jelentkezzen integrációs](../security-center/security-center-integrating-alerts-with-log-integration.md) – Ez a dokumentum bemutatja, hogyan toosync Security Center riasztást küld, és a virtuális gép biztonsági eseményeket a naplóelemzési az Azure Diagnostics és az Azure tevékenységi naplóit, által gyűjtött vagy SIEM-megoldás.
* [Az Azure diagnostics és az Azure-beli Auditnaplók újdonságai](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/) – ebben a blogbejegyzésben bemutatja tooAzure naplókat, és egyéb szolgáltatásokat, amelyek segítenek betekintést nyerhet az Azure-erőforrások hello működésére.
