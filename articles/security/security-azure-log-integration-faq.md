---
title: "aaaAzure napló integrációs – gyakori kérdések |} Microsoft Docs"
description: "Ez a cikk az Azure napló integrációs kapcsolatos kérdésekre ad választ."
services: security
documentationcenter: na
author: TomShinder
manager: MBaldwin
editor: TerryLanfear
ms.assetid: d06d1ac5-5c3b-49de-800e-4d54b3064c64
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload8: na
ms.date: 08/07/2017
ms.author: TomSh
ms.custom: azlog
ms.openlocfilehash: e886035c9a180d0cd5fcbe9cc02483782df6dbe4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-log-integration-faq"></a>Azure Naplóelemzés integrációs – gyakori kérdések
Ebben a cikkben megválaszolunk kapcsolatos gyakori kérdések (GYIK) Azure napló integráció. 

Azure napló integrációs szolgáltatás használható toointegrate nyers naplók az Azure-erőforrások az azokat a helyi biztonsági információkat és az esemény (SIEM) felügyeleti rendszerek Windows operációs rendszer szolgáltatás. Ez az integráció biztosítja az új, egységesített irányítópult minden Ez az eszköz, a helyszíni vagy felhőben hello. Ezután összesíteni, összefüggéseket, elemzése, és az alkalmazások társított biztonsági események riasztást.

## <a name="is-hello-azure-log-integration-software-free"></a>Egy szabad hello Azure napló integrációs szoftver?
Igen. Nincs hello Azure napló integrációs szoftver díjmentes.

## <a name="where-is-azure-log-integration-available"></a>Amennyiben Azure napló integrációs rendelkezésre áll?

Jelenleg az Azure kereskedelmi és Azure Government érhető el, és nem áll rendelkezésre a kínai vagy Németország.

## <a name="how-can-i-see-hello-storage-accounts-from-which-azure-log-integration-is-pulling-azure-vm-logs"></a>Honnan látom, amelyből Azure napló integrációs Azure virtuális gép naplók van húzza hello tárfiókok?
Hello paranccsal **azlog forráslista**.

## <a name="how-can-i-tell-which-subscription-hello-azure-log-integration-logs-are-from"></a>Hogyan állapítható meg mely előfizetés hello Azure feldolgozásra a napló-integráció?

Hello esetében, amelyek hello kerülnek naplókat **AzureResourcemanagerJson** könyvtárak, hello előfizetés-azonosító van a hello naplófájl neve. Ugyanez a naplók a hello **AzureSecurityCenterJson** mappa. Példa:

20170407T070805_2768037.0000000023. **1111e5ee-1111-111b-a11e-1e111e1111dc**.JSON kiterjesztésű

Az Azure Active Directory-naplók hello Bérlőazonosító hello nevének részeként tartalmazza.

Diagnosztikai naplók az eseményközpontban lévő olvasható hello nevének részeként hello előfizetés-azonosító nem tartalmazzák. Ehelyett tartoznak hello hub eseményforrás hello létrehozásának részeként megadott hello rövid nevét. 

## <a name="how-can-i-update-hello-proxy-configuration"></a>Hogyan frissíthetem hello proxykonfigurációt?
Ha a megadott proxybeállítást nem teszi lehetővé az Azure storage access közvetlenül, nyissa meg a hello **AZLOG. EXE-FÁJL. CONFIG** fájlt **c:\Program Files\Microsoft Azure napló integrációs**. Frissítés hello fájl tooinclude hello **defaultProxy** a szervezet hello proxycímmel szakasza. Hello frissítés után szolgáltatás leállítása és indítása hello hello parancsokkal **net stop azlog** és **net start azlog**.

    <?xml version="1.0" encoding="utf-8"?>
    <configuration>
      <system.net>
        <connectionManagement>
          <add address="*" maxconnection="400" />
        </connectionManagement>
        <defaultProxy>
          <proxy usesystemdefault="true"
          proxyaddress=http://127.0.0.1:8888
          bypassonlocal="true" />
        </defaultProxy>
      </system.net>
      <system.diagnostics>
        <performanceCounters filemappingsize="20971520" />
      </system.diagnostics>   

## <a name="how-can-i-see-hello-subscription-information-in-windows-events"></a>Honnan látom hello előfizetési adatokat a Windows-eseményeket?
Hello előfizetési azonosító toohello rövid név hozzáfűzése hello forrás hozzáadása közben:

    Azlog source add <sourcefriendlyname>.<subscription id> <StorageName> <StorageKey>  
hello esemény XML metaadatok, többek között a hello előfizetés-azonosító a következő hello rendelkezik:

![Esemény XML][1]

## <a name="error-messages"></a>Hibaüzenetek
### <a name="when-i-run-hello-command-azlog-createazureid-why-do-i-get-hello-following-error"></a>Hello parancs futtatásakor **azlog createazureid**, miért jelenik meg a következő hiba hello?
Hiba:

  *Nem sikerült az AAD-alkalmazást - bérlő 72f988bf-86f1-41af-91ab-2d7cd011db37 - OK toocreate = "Tiltott" - Message = "a megfelelő jogosultságok hiánya toocomplete hello művelet."*

Hello **azlog createazureid** parancs megpróbálja toocreate hozzáféréssel rendelkezik az összes hello Azure AD bérlők hello Azure bejelentkezési hello előfizetésekhez egy egyszerű szolgáltatást. Ha az Azure bejelentkezési csak a Vendég felhasználó az Azure AD bérlőre, hello parancs az "a megfelelő jogosultságok hiánya toocomplete hello műveletet." Kérje meg a hello Bérlői rendszergazda tooadd hello bérlői felhasználói fiókját.

### <a name="when-i-run-hello-command-azlog-authorize-why-do-i-get-hello-following-error"></a>Hello parancs futtatásakor **azlog engedélyezik**, miért jelenik meg a következő hiba hello?
Hiba:

  *Figyelmeztetés a szerepkör-hozzárendelés - AuthorizationFailed létrehozása: hello ügyfél janedo@microsoft.com"objektum"fe9e03e4-4dad-4328-910f-fd24a9660bd2"azonosítója nem tartozik engedélyezési tooperform művelet"Microsoft.Authorization/roleAssignments/write"hatókörben" / 97 – b971-0d8ff0000000 előfizetések / 70d 95299-d689-4c ".*

Hello **azlog engedélyezik** rendel hello olvasó toohello az Azure AD szolgáltatás egyszerű szerepe parancs (létre **azlog createazureid**) megadott toohello előfizetések. Ha hello Azure bejelentkezési nem társadminisztrátorának vagy hello előfizetés tulajdonosa, azt "Engedélyezési sikertelen" hibaüzenettel meghiúsul. Azure szerepköralapú hozzáférés-vezérlés (RBAC) társadminisztrátoraként vagy a tulajdonos szükséges toocomplete van ezzel a művelettel.

## <a name="where-can-i-find-hello-definition-of-hello-properties-in-hello-audit-log"></a>Hol találok hello tulajdonságok hello definíciója hello naplóban?
Lásd:

* [Naplózási műveletek az Azure Resource Manager eszközzel](../azure-resource-manager/resource-group-audit.md)
* [Lista hello felügyeleti események hello Azure figyelő REST API-t az előfizetés](https://msdn.microsoft.com/library/azure/dn931934.aspx)

## <a name="where-can-i-find-details-on-azure-security-center-alerts"></a>Hol található az Azure Security Center riasztásait részleteit?
Lásd: [az Azure Security Centerben riasztások kezelése és válaszol toosecurity](../security-center/security-center-managing-and-responding-alerts.md).

## <a name="how-can-i-modify-what-is-collected-with-vm-diagnostics"></a>Hogyan módosítható meg, hogy a Virtuálisgép-diagnosztika gyűjtött adatok?
Hogyan tooget, módosítására és hello Azure Diagnostics-konfigurációját állítsa témakör [használja a Powershellt tooenable Azure Diagnostics Windows rendszerű virtuális gépként](../virtual-machines/windows/ps-extensions-diagnostics.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

a következő példa hello hello Azure Diagnostics konfigurációs beolvasása:

    -AzureRmVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient
    $publicsettings = (Get-AzureRmVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient).PublicSettings
    $encodedconfig = (ConvertFrom-Json -InputObject $publicsettings).xmlCfg
    $xmlconfig = [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($encodedconfig))
    Write-Host $xmlconfig

    $xmlconfig | Out-File -Encoding utf8 -FilePath "d:\WADConfig.xml"

a következő példa hello módosítja a hello Azure Diagnostics konfigurációt. Ebben a konfigurációban csak azonosító 4624 és esemény azonosítója 4625 hello biztonsági eseménynaplójából gyűjti. A Microsoft Antimalware Azure események begyűjti hello rendszer-eseménynaplójában. További részletek az XPath kifejezések hello használata: [fel események](https://msdn.microsoft.com/library/windows/desktop/dd996910(v=vs.85)).

    <WindowsEventLog scheduledTransferPeriod="PT1M">
        <DataSource name="Security!*[System[(EventID=4624 or EventID=4625)]]" />
        <DataSource name="System!*[System[Provider[@Name='Microsoft Antimalware']]]"/>
    </WindowsEventLog>

hello alábbi mintakód hello Azure diagnosztikai konfiguráció:

    $diagnosticsconfig_path = "d:\WADConfig.xml"
    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient -DiagnosticsConfigurationPath $diagnosticsconfig_path -StorageAccountName log3121 -StorageAccountKey <storage key>

Módosítása után ellenőrizze, hogy helyes-e eseményeket a rendszer gyűjti hello hello tárolási fiók tooensure.

Ha probléma merül fel hello telepítés és konfigurálás során, nyisson meg egy [támogatási kérelem](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). Válassza ki **napló integrációs** hello szolgáltatást, amelynek támogatási kérelmet.

## <a name="can-i-use-azure-log-integration-toointegrate-network-watcher-logs-into-my-siem"></a>A saját SIEM is használható Azure napló integrációs toointegrate hálózati figyelőt naplókat?

Az Azure hálózati figyelőt naplózási információk nagy mennyiségű állít elő. Ezek a naplók nem küldött jelentette toobe tooa SIEM. csak a támogatott hello hálózati figyelőt naplók célja egy tárfiókot. Azure napló-integráció nem támogatja ezek a naplók beolvasásakor, és így válnak elérhetővé tooa SIEM.

<!--Image references-->
[1]: ./media/security-azure-log-integration-faq/event-xml.png
