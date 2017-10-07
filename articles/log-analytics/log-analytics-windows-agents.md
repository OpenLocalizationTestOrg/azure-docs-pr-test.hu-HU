---
title: "aaaConnect Windows számítógépek tooAzure Naplóelemzési |} Microsoft Docs"
description: "Ez a cikk jeleníti meg hello lépéseket tooconnect hello Windows rendszerű számítógépek a helyszíni infrastruktúra toohello Naplóelemzés szolgáltatás a Microsoft Monitoring Agent (MMA) hello testre szabott verzióját használja."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 932f7b8c-485c-40c1-98e3-7d4c560876d2
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/03/2017
ms.author: magoedte
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7e15f9eeb0440bd2f6557d7215df701526e4f9aa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-windows-computers-toohello-log-analytics-service-in-azure"></a>Csatlakozás a Windows számítógépek toohello Naplóelemzés szolgáltatás az Azure-ban

Ez a cikk lépéseit mutatja be, hello tooconnect Windows rendszerű számítógépek a helyszíni infrastruktúra tooOMS munkaterületeken a testreszabott hello Microsoft Monitoring Agent (MMA) használatával. Tooinstall kell, és csatlakoztassa az összes, amelyet ahhoz tooonboard Naplóelemzési toohello toosend adatszolgáltatás és tooview és az adatok act hello számítógépek ügynökök. Minden ügynök toomultiple munkaterületek is tud jelentéseket.

Telepítés, parancssor, ügynökök telepítése vagy a Szükségeskonfiguráció-konfiguráló (DSC) az Azure Automationben.  

>[!NOTE]
Az Azure-ban futó virtuális gépek, egyszerűbbé teheti az telepítési hello segítségével [virtuálisgép-bővítmény](log-analytics-azure-vm-extension.md).

Internetkapcsolattal rendelkező számítógépeken a hello ügynök hello kapcsolat toohello Internet toosend adatok tooOMS használja. Azok a számítógépek, amelyek nem rendelkeznek internetkapcsolattal, használhatja a proxy vagy az hello [OMS átjáró](log-analytics-oms-gateway.md).

Csatlakozás a Windows-számítógépek tooOMS egyszerű három egyszerű lépéseket követve:

1. Hello ügynök telepítő fájl letöltését hello OMS-portálon
2. Dönthet hello metódussal hello ügynök telepítése
3. Hello ügynök beállítása, vagy adja hozzá a további munkaterületek, ha szükséges

hello alábbi ábrán látható a Windows rendszerű számítógépek és az OMS hello kapcsolatát telepíteni és konfigurálni az ügynököket után.

![OMS-közvetlen-ügynök – diagram](./media/log-analytics-windows-agents/oms-direct-agent-diagram.png)

Ha az IT-biztonsági házirendeknek nem engedélyezi a hálózati tooconnect toohello Internet a számítógépek, konfigurálhatja a számítógépek tooconnect toohello OMS-átjáró. További információk és bemutatjuk, hogyan tooconfigure egy OMS átjáró toohello OMS szolgáltatással, a kiszolgálók toocommunicate: [számítógépek tooOMS hello OMS átjáró használatával csatlakozzon](log-analytics-oms-gateway.md).

## <a name="system-requirements-and-required-configuration"></a>Rendszerkövetelmények és a szükséges konfigurációval
Telepítéséhez, és ügynökök telepítésére, tekintse át a következő részleteket tooensure hello követelményeknek hello.

- Csak telepíthető hello OMS MMA futtató Windows Server 2008 SP 1 vagy újabb vagy Windows 7 SP1 vagy újabb.
- Azure-előfizetés szükséges.  További információkért lásd: [Ismerkedés a Naplóelemzési](log-analytics-get-started.md).
- Minden Windows-számítógép kell tudni tooconnect toohello Internet HTTPS vagy toohello OMS átjáró használatával. Lehet, hogy a kapcsolat közvetlen, egy proxyn keresztül, vagy hello OMS-átjárón keresztül.
- Hello OMS MMA telepíthető önálló számítógépek, kiszolgálók és virtuális gépek. Ha azt szeretné, hogy tooconnect Azure által üzemeltetett virtuális gépek tooOMS, [csatlakozás Azure virtuális gépek tooLog Analytics](log-analytics-azure-vm-extension.md).
- a különböző erőforrások toouse 443-as TCP-portot kell hello ügynököt.

### <a name="network"></a>Network (Hálózat)

A Windows ügynökök tooconnect tooand register hello OMS szolgáltatással toonetwork erőforrások eléréséhez, beleértve hello portszámok és a tartomány URL-címek kell rendelkezniük.

- Proxykiszolgálók van szüksége, amely megfelelő proxykiszolgáló erőforrások vannak konfigurálva ügynökbeállítások hello tooensure.
- A, amelyek korlátozzák a hozzáférést toohello Internet tűzfalakat akkor vagy a hálózati mérnökök kell tooconfigure a tűzfal toopermit hozzáférés tooOMS. Az ügynök beállításait nem kell módosítania.

a következő táblázat hello látható kommunikációhoz szükséges erőforrásokat.

>[!NOTE]
>A következő erőforrások hello némelyike említse meg az Operational Insights eddig Naplóelemzési előző nevét.

| Ügynök erőforrása | Portok | HTTPS-ellenőrzés kihagyása |
|---|---|---|
| *.ods.opinsights.azure.com | 443 | Igen |
| *.oms.opinsights.azure.com | 443 | Igen |
| *.blob.core.windows.net | 443 | Igen |
| *.azure-automation.net | 443 | Igen |



## <a name="download-hello-agent-setup-file-from-oms"></a>Az OMS Szolgáltatáshoz hello ügynök telepítési fájl letöltése
1. Az hello OMS-portálon a hello **áttekintése** hello kattintson **beállítások** csempére.  Kattintson a hello **csatlakoztatott források** hello felső fülre.  
    ![Csatlakoztatott adatforrások lap](./media/log-analytics-windows-agents/oms-direct-agent-connected-sources.png)
2. Kattintson a **Windows kiszolgálók** majd **Windows-ügynök letöltése** alkalmazható tooyour számítógép processzor típusa toodownload hello telepítőfájl.
3. A jobbra hello **munkaterület azonosítója**, hello másolás ikonra, és illessze be a Jegyzettömbbe hello azonosítója.
4. A jobbra hello **elsődleges kulcs**, hello másolás ikonra, és illessze be a hello kulcs a Jegyzettömbbe.     

## <a name="install-hello-agent-using-setup"></a>A telepítőprogram használatával hello ügynök telepítése
1. Futtassa a telepítő tooinstall hello ügynököt a számítógépen, amelyet az toomanage.
2. A hello kezdőlapján kattintson **következő**.
3. Hello licencfeltételeket oldalon olvassa el a hello licenc, és kattintson **elfogadom**.
4. Hello célmappa oldalon módosíthatja vagy tartsa hello alapértelmezett telepítési mappáját, majd kattintson **következő**.
5. Hello ügynök telepítésének beállításai lapon tooconnect hello ügynök tooAzure Naplóelemzés (OMS), Operations Manager, dönthet úgy, vagy üresen hello döntések Ha később szeretné tooconfigure hello ügynök. Kattintson a **Tovább** gombra.   
    - Ha úgy dönt, hogy tooconnect tooAzure Naplóelemzés (OMS), illessze be a hello **munkaterület azonosítója** és **Munkaterületkulcsot (elsődleges kulcs)** másolja a Jegyzettömbbe hello előző eljárásban, és kattintson  **Következő**.  
        ![illessze be a munkaterület azonosítója és az elsődleges kulcs](./media/log-analytics-windows-agents/connect-workspace.png)
    - Ha úgy dönt, hogy a kezelő tooconnect tooOperations, írja be a hello **felügyeleti csoport neve**, **felügyeleti kiszolgáló** nevét, és **felügyeleti kiszolgáló portszáma**, majd **Következő**. Hello Ügynökműveleti fiók lapon válassza ki vagy a helyi rendszerfiók hello, vagy a helyi tartományi fiókot, és kattintson a **következő**.  
        ![felügyeleti csoport konfigurációja](./media/log-analytics-windows-agents/oms-mma-om-setup01.png)![a műveleti fiók](./media/log-analytics-windows-agents/oms-mma-om-setup02.png)

6. Hello készen tooInstall oldalon tekintse át a kiválasztott beállításokat, és kattintson **telepítése**.
7. Hello konfigurálása sikeresen befejeződött a lapon, és kattintson **Befejezés**.
8. Amikor végzett, hello **Microsoft-Figyelőügynök** megjelenik **Vezérlőpult**. Tekintse át a hiba a konfiguráció, és győződjön meg arról, hogy hello ügynök csatlakoztatott tooOperational Insights (OMS). Ha csatlakoztatott tooOMS hello ügynök jeleníti meg a következő üzenet: **hello Microsoft Monitoring Agent sikeresen csatlakozott toohello a Microsoft Operations Management Suite szolgáltatással.**

## <a name="configure-proxy-settings"></a>Proxybeállítások konfigurálása

A következő eljárás tooconfigure proxybeállítások hello Microsoft Monitoring Agent vezérlőpultján keresztül hello is használhatja. Szükség van toouse ezt az eljárást minden kiszolgálón. Ha telepíteni kell, amely tooconfigure kiszolgálók számát, bizonyára hasznosnak találja azt egy parancsfájl tooautomate könnyebb toouse ezt a folyamatot. Ha igen, tekintse meg a következő eljárással hello [tooconfigure proxybeállítások hello Microsoft Monitoring Agent egy olyan parancsfájllal](#to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-a-script).

### <a name="tooconfigure-proxy-settings-for-hello-microsoft-monitoring-agent-using-control-panel"></a>a Microsoft Monitoring Agent vezérlőpultján keresztül hello tooconfigure proxybeállítások
1. Nyissa meg **Vezérlőpultot**.
2. Válassza a **Microsoft Monitoring Agent** lehetőséget.
3. Kattintson a hello **proxybeállítások** fülre.  
    ![proxybeállítások lap](./media/log-analytics-windows-agents/proxy-direct-agent-proxy.png)
4. Válassza ki **proxykiszolgálót használni** és hello URL-címét és portszámát, ha szükséges, hasonló toohello példában is látható. Ha a proxykiszolgálóhoz hitelesítés szükséges, írja be a hello felhasználónév és jelszó tooaccess hello proxykiszolgálót.


### <a name="verify-agent-connectivity-toooms"></a>Ellenőrizze az ügynök csatlakozási tooOMS

Könnyen ellenőrizheti, hogy az ügynökök kommunikáló OMS eljárást követő hello használata:

1.  A Windows hello-ügynökkel hello számítógépen nyissa meg a Vezérlőpultot.
2.  Nyissa meg a Microsoft Monitoring Agent szolgáltatásnál.
3.  Kattintson a hello Azure Naplóelemzés (OMS) fülre.
4.  Hello Állapot oszlopban megtekintheti az adott hello ügynök sikeresen csatlakoztatva toohello Operations Management Suite szolgáltatással.

![ügynök csatlakoztatva](./media/log-analytics-windows-agents/mma-connected.png)


### <a name="tooconfigure-proxy-settings-for-hello-microsoft-monitoring-agent-using-a-script"></a>a Microsoft Monitoring Agent egy olyan parancsfájllal hello tooconfigure proxybeállítások
Másolja a következő mintát, információkat adott tooyour környezet frissíti, mentse a PS1 fájlnév-kiterjesztésűeket, és futtassa a hello parancsfájl minden számítógépen, amely közvetlenül a toohello OMS szolgáltatás hello.

    param($ProxyDomainName="http://proxy.contoso.com:80", $cred=(Get-Credential))

    # First we get hello Health Service configuration object.  We need toodetermine if we
    #have hello right update rollup with hello API we need.  If not, no need toorun hello rest of hello script.
    $healthServiceSettings = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'

    $proxyMethod = $healthServiceSettings | Get-Member -Name 'SetProxyInfo'

    if (!$proxyMethod)
    {
         Write-Output 'Health Service proxy API not present, will not update settings.'
         return
    }

    Write-Output "Clearing proxy settings."
    $healthServiceSettings.SetProxyInfo('', '', '')

    $ProxyUserName = $cred.username

    Write-Output "Setting proxy too$ProxyDomainName with proxy username $ProxyUserName."
    $healthServiceSettings.SetProxyInfo($ProxyDomainName, $ProxyUserName, $cred.GetNetworkCredential().password)



## <a name="install-hello-agent-using-hello-command-line"></a>Hello parancssorból hello ügynök telepítése
- Módosítsa, majd a következő példa tooinstall hello ügynök hello parancssorból hello. hello példa teljesen csendes telepítést hajt végre.

    >[!NOTE]
    Ha azt szeretné, hogy az ügynök tooupgrade, toouse hello Naplóelemzési kell parancsfájlkezelési API. Lásd: hello következő szakasz tooupgrade ügynök.

    ```dos
    MMASetup-AMD64.exe /Q:A /R:N /C:"setup.exe /qn ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_ID=<your workspace id> OPINSIGHTS_WORKSPACE_KEY=<your workspace key> AcceptEndUserLicenseAgreement=1"
    ```

hello ügynök használ annak önkibontó IExpress hello segítségével `/c` parancsot. Megtekintheti a hello parancssori kapcsolók: [parancssori kapcsolók a IExpress](https://support.microsoft.com/help/197147/command-line-switches-for-iexpress-software-update-packages) és majd frissítés hello példa toosuit igényeinek.

|MMA vonatkozó beállítások                   |Megjegyzések         |
|---------------------------------------|--------------|
|ADD_OPINSIGHTS_WORKSPACE               | 1 = konfigurálása hello ügynök tooreport tooa munkaterület                |
|OPINSIGHTS_WORKSPACE_ID                | Hello munkaterület tooadd munkaterület azonosítója (guid)                    |
|OPINSIGHTS_WORKSPACE_KEY               | Munkaterület kulcs használt tooinitially hitelesítéshez hello munkaterület |
|OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE  | Adja meg, ahol hello munkaterület hello felhőalapú környezetben <br> 0 = azure kereskedelmi cloud (alapértelmezett) <br> 1 = azure Government |
|OPINSIGHTS_PROXY_URL               | URI-JÁNAK hello proxy toouse |
|OPINSIGHTS_PROXY_USERNAME               | Felhasználónév tooaccess egy hitelesített proxykiszolgálót |
|OPINSIGHTS_PROXY_PASSWORD               | Jelszó tooaccess egy hitelesített proxykiszolgálót |

>[!NOTE]
tooavoid elérte-e hello parancssori hosszkorlátot IExpress, nincs konfigurálva munkaterület hello ügynök telepítése, majd egy parancsfájl tooset konfigurációs hello munkaterülethez.

>[!NOTE]
Ha egy `Command line option syntax error.` hello használatakor `OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE` paraméter, a megoldás a következő hello használhatja:
```dos
MMASetup-AMD64.exe /C /T:.\MMAExtract
cd .\MMAExtract
setup.exe /qn ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE=1 OPINSIGHTS_WORKSPACE_ID=<your workspace id> OPINSIGHTS_WORKSPACE_KEY=<your workspace key> AcceptEndUserLicenseAgreement=1
```

## <a name="add-a-workspace-using-a-script"></a>Egy parancsfájl segítségével munkaterület hozzáadása
A munkaterület hello Naplóelemzési ügynök parancsfájlkezelési API használata a következő példa hello hozzáadása:

```PowerShell
$mma = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'
$mma.AddCloudWorkspace($workspaceId, $workspaceKey)
$mma.ReloadConfiguration()
```

az Amerikai Egyesült államokbeli kormányzati, a következő parancsfájl használata hello Azure munkaterületeinek tooadd:
```PowerShell
$mma = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'
$mma.AddCloudWorkspace($workspaceId, $workspaceKey, 1)
$mma.ReloadConfiguration()
```

>[!NOTE]
Ha a használt hello parancssori vagy parancsfájl korábban tooinstall vagy hello ügynök konfigurálása `EnableAzureOperationalInsights` le lett cserélve `AddCloudWorkspace`.

## <a name="install-hello-agent-using-dsc-in-azure-automation"></a>Azure Automation DSC használata hello ügynök telepítése

A következő parancsfájl például tooinstall hello ügynök az Azure Automation DSC használata hello is használhatja. telepíti a 64 bites ügynök hello által azonosított hello hello példa `URI` érték. Hello 32 bites verziója tartományvezérlőkkel történő lecserélésével hello URI érték is használható. hello URI-azonosítók mindkét-verziók a következők:

- A Windows 64 bites ügynök – https://go.microsoft.com/fwlink/?LinkId=828603
- A Windows 32 bites ügynök – https://go.microsoft.com/fwlink/?LinkId=828604


>[!NOTE]
A művelet és a parancsfájl például nem frissíti a meglévő ügynököt.

1. Hello xPSDesiredStateConfiguration DSC modul importálása a [http://www.powershellgallery.com/packages/xPSDesiredStateConfiguration](http://www.powershellgallery.com/packages/xPSDesiredStateConfiguration) az Azure Automation.  
2.  Azure Automation változó eszközök létrehozása *OPSINSIGHTS_WS_ID* és *OPSINSIGHTS_WS_KEY*. Állítsa be *OPSINSIGHTS_WS_ID* tooyour OMS Naplóelemzési munkaterület azonosítója és *OPSINSIGHTS_WS_KEY* toohello elsődleges kulcs a munkaterületet.
3.  Használja a következő parancsfájlt, és mentse MMAgent.ps1 hello
4.  Módosítsa, majd a következő példa tooinstall hello ügynök az Azure Automation DSC használata hello. Azure Automation hello Azure Automation-illesztőt vagy parancsmag használatával MMAgent.ps1 importálja.
5.  Egy csomópont toohello-konfiguráció hozzárendelése. 15 percen belül hello csomópont ellenőrzi a konfigurációját, és hello MMA fejlesztőre toohello csomópont.

```PowerShell
Configuration MMAgent
{
    $OIPackageLocalPath = "C:\MMASetup-AMD64.exe"
    $OPSINSIGHTS_WS_ID = Get-AutomationVariable -Name "OPSINSIGHTS_WS_ID"
    $OPSINSIGHTS_WS_KEY = Get-AutomationVariable -Name "OPSINSIGHTS_WS_KEY"


    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    Node OMSnode {
        Service OIService
        {
            Name = "HealthService"
            State = "Running"
            DependsOn = "[Package]OI"
        }

        xRemoteFile OIPackage {
            Uri = "https://go.microsoft.com/fwlink/?LinkId=828603"
            DestinationPath = $OIPackageLocalPath
        }

        Package OI {
            Ensure = "Present"
            Path  = $OIPackageLocalPath
            Name = "Microsoft Monitoring Agent"
            ProductId = "8A7F2C51-4C7D-4BFD-9014-91D11F24AAE2"
            Arguments = '/C:"setup.exe /qn ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_ID=' + $OPSINSIGHTS_WS_ID + ' OPINSIGHTS_WORKSPACE_KEY=' + $OPSINSIGHTS_WS_KEY + ' AcceptEndUserLicenseAgreement=1"'
            DependsOn = "[xRemoteFile]OIPackage"
        }
    }
}


```

### <a name="get-hello-latest-productid-value"></a>Hello legújabb ProductId érték

Hello `ProductId value` parancsfájl hello MMAgent.ps1 egyedi tooeach ügynök verziója. Minden ügynök frissített verziójának közzétételekor hello ProductId érték megváltozik. Igen hello ProductId a jövőbeli hello megváltozásakor található hello ügynök verziója egyszerű parancsfájl használatával. Miután hello ügynök legfrissebb teszt kiszolgálóra telepíteni, a következő parancsfájl tooget telepítve hello ProductId érték hello is használhatja. Hello legújabb ProductId értéket használva frissítheti hello MMAgent.ps1 parancsfájl hello értéket.

```PowerShell
$InstalledApplications  = Get-ChildItem hklm:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall


foreach ($Application in $InstalledApplications)

{

     $Key = Get-ItemProperty $Application.PSPath

     if ($Key.DisplayName -eq "Microsoft Monitoring Agent")

     {

        $Key.DisplayName

        Write-Output ("Product ID is: " + $Key.PSChildName.Substring(1,$Key.PSChildName.Length -2))

     }

}  
```

## <a name="configure-an-agent-manually-or-add-additional-workspaces"></a>Az ügynök manuális konfigurálásához, vagy adja hozzá a további munkaterületek
Ha telepített ügynökök, de nem állította be, vagy ha azt szeretné, hogy hello ügynök tooreport toomultiple munkaterületek, használja a következő információk tooenable ügynök hello, vagy konfigurálja azt újra. Hello ügynök konfigurálása után regisztrálja az hello ügynök szolgáltatás, és megkapja a szükséges konfigurációs adatokat és a felügyeleti csomagok, amelyekben a megoldással kapcsolatos információkat.

1. Ha már telepítette a Microsoft Monitoring Agent hello, nyissa meg a **Vezérlőpult**.
2. Nyissa meg **Microsoft-Figyelőügynök** majd hello **Azure Naplóelemzés (OMS)** fülre.   
3. Kattintson a **Hozzáadás** tooopen hello **hozzáadni a Naplóelemzési munkaterület** mezőbe.
4. Beillesztés hello **munkaterület azonosítója** és **Munkaterületkulcsot (elsődleges kulcs)** másolja a Jegyzettömbbe hello munkaterület tooadd szeretne, és kattintson az előző eljárásban **OK**.  
    ![Operational insights szolgáltatás konfigurálása](./media/log-analytics-windows-agents/add-workspace.png)

Adatgyűjtés hello ügynök által felügyelt számítógépek, hello száma OMS által felügyelt számítógépek megjelennek a hello hello OMS-portálon **csatlakoztatott források** lapján **beállítások** ,  **Csatlakoztatott**.


## <a name="toodisable-an-agent"></a>az ügynök toodisable
1. Hello ügynök telepítése után nyissa meg a **Vezérlőpult**.
2. Nyissa meg a Microsoft Monitoring Agent, és kattintson a hello **Azure Naplóelemzés (OMS)** fülre.
3. Válasszon egy munkaterületet, és kattintson a **eltávolítása**. Ismételje meg ezt a lépést minden egyéb munkaterületekkel.


## <a name="optionally-configure-agents-tooreport-tooan-operations-manager-management-group"></a>Nem kötelező lépésként beállíthatja az ügynökök tooreport tooan Operations Manager felügyeleti csoport

Ha az Operations Manager az informatikai infrastruktúrát használ, az Operations Manager ügynök is használhatja a hello MMA ügynök.

### <a name="tooconfigure-mma-agents-tooreport-tooan-operations-manager-management-group"></a>tooconfigure MMA ügynökök tooreport tooan Operations Manager felügyeleti csoport
1.  Hello számítógépre, amelyen telepítve van-e az hello ügynök, nyissa meg **Vezérlőpult**.  
2.  Nyissa meg **Microsoft-Figyelőügynök** majd hello **Operations Manager** fülre.  
    ![A Microsoft figyelési ügynök Operations Manager lap](./media/log-analytics-windows-agents/om-mg01.png)
3.  Ha az Operations Manager-kiszolgáló Active Directory integrációja a, kattintson a **az AD DS-ből felügyeleti csoport-hozzárendelések automatikus frissítése**.
4.  Kattintson a **Hozzáadás** tooopen hello **felügyeleti csoport hozzáadása** párbeszédpanel megnyitásához.  
    ![A Microsoft Monitoring Agent egy felügyeleti csoport hozzáadása](./media/log-analytics-windows-agents/oms-mma-om02.png)
5.  A **felügyeleti csoport neve** mezőbe, a felügyeleti csoport hello típusnév.
6.  A hello **elsődleges felügyeleti kiszolgáló** mezőbe, a típus hello hello elsődleges felügyeleti kiszolgáló számítógépneve.
7.  A hello **felügyeleti kiszolgáló portszáma** mezőbe típus hello TCP-portszámot.
8.  A **Ügynökműveleti fiók**, vagy a helyi rendszerfiók hello, vagy a helyi tartományi fiókot válasszon.
9.  Kattintson a **OK** tooclose hello **felügyeleti csoport hozzáadása** párbeszédpanel megnyitásához, majd kattintson **OK** tooclose hello **Microsoft Monitoring Agent tulajdonságai**párbeszédpanel megnyitásához.


## <a name="next-steps"></a>Következő lépések

- [Log Analytics-megoldások hozzáadása hello megoldások gyűjtemény](log-analytics-add-solutions.md) tooadd funkciókat és összefog adatokat.
