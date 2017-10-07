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
# <a name="azure-log-integration-faq"></a><span data-ttu-id="5fdce-103">Azure Naplóelemzés integrációs – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="5fdce-103">Azure Log Integration FAQ</span></span>
<span data-ttu-id="5fdce-104">Ebben a cikkben megválaszolunk kapcsolatos gyakori kérdések (GYIK) Azure napló integráció.</span><span class="sxs-lookup"><span data-stu-id="5fdce-104">This article answers frequently asked questions (FAQ) about Azure Log Integration.</span></span> 

<span data-ttu-id="5fdce-105">Azure napló integrációs szolgáltatás használható toointegrate nyers naplók az Azure-erőforrások az azokat a helyi biztonsági információkat és az esemény (SIEM) felügyeleti rendszerek Windows operációs rendszer szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="5fdce-105">Azure Log Integration is a Windows operating system service that you can use toointegrate raw logs from your Azure resources into your on-premises security information and event management (SIEM) systems.</span></span> <span data-ttu-id="5fdce-106">Ez az integráció biztosítja az új, egységesített irányítópult minden Ez az eszköz, a helyszíni vagy felhőben hello.</span><span class="sxs-lookup"><span data-stu-id="5fdce-106">This integration provides a unified dashboard for all your assets, on-premises or in hello cloud.</span></span> <span data-ttu-id="5fdce-107">Ezután összesíteni, összefüggéseket, elemzése, és az alkalmazások társított biztonsági események riasztást.</span><span class="sxs-lookup"><span data-stu-id="5fdce-107">You can then aggregate, correlate, analyze, and alert for security events associated with your applications.</span></span>

## <a name="is-hello-azure-log-integration-software-free"></a><span data-ttu-id="5fdce-108">Egy szabad hello Azure napló integrációs szoftver?</span><span class="sxs-lookup"><span data-stu-id="5fdce-108">Is hello Azure Log Integration software free?</span></span>
<span data-ttu-id="5fdce-109">Igen.</span><span class="sxs-lookup"><span data-stu-id="5fdce-109">Yes.</span></span> <span data-ttu-id="5fdce-110">Nincs hello Azure napló integrációs szoftver díjmentes.</span><span class="sxs-lookup"><span data-stu-id="5fdce-110">There is no charge for hello Azure Log Integration software.</span></span>

## <a name="where-is-azure-log-integration-available"></a><span data-ttu-id="5fdce-111">Amennyiben Azure napló integrációs rendelkezésre áll?</span><span class="sxs-lookup"><span data-stu-id="5fdce-111">Where is Azure Log Integration available?</span></span>

<span data-ttu-id="5fdce-112">Jelenleg az Azure kereskedelmi és Azure Government érhető el, és nem áll rendelkezésre a kínai vagy Németország.</span><span class="sxs-lookup"><span data-stu-id="5fdce-112">It is currently available in Azure Commercial and Azure Government and is not available in China or Germany.</span></span>

## <a name="how-can-i-see-hello-storage-accounts-from-which-azure-log-integration-is-pulling-azure-vm-logs"></a><span data-ttu-id="5fdce-113">Honnan látom, amelyből Azure napló integrációs Azure virtuális gép naplók van húzza hello tárfiókok?</span><span class="sxs-lookup"><span data-stu-id="5fdce-113">How can I see hello storage accounts from which Azure Log Integration is pulling Azure VM logs?</span></span>
<span data-ttu-id="5fdce-114">Hello paranccsal **azlog forráslista**.</span><span class="sxs-lookup"><span data-stu-id="5fdce-114">Run hello command **azlog source list**.</span></span>

## <a name="how-can-i-tell-which-subscription-hello-azure-log-integration-logs-are-from"></a><span data-ttu-id="5fdce-115">Hogyan állapítható meg mely előfizetés hello Azure feldolgozásra a napló-integráció?</span><span class="sxs-lookup"><span data-stu-id="5fdce-115">How can I tell which subscription hello Azure Log Integration logs are from?</span></span>

<span data-ttu-id="5fdce-116">Hello esetében, amelyek hello kerülnek naplókat **AzureResourcemanagerJson** könyvtárak, hello előfizetés-azonosító van a hello naplófájl neve.</span><span class="sxs-lookup"><span data-stu-id="5fdce-116">In hello case of audit logs that are placed in hello **AzureResourcemanagerJson** directories, hello subscription ID is in hello log file name.</span></span> <span data-ttu-id="5fdce-117">Ugyanez a naplók a hello **AzureSecurityCenterJson** mappa.</span><span class="sxs-lookup"><span data-stu-id="5fdce-117">This is also true for logs in hello **AzureSecurityCenterJson** folder.</span></span> <span data-ttu-id="5fdce-118">Példa:</span><span class="sxs-lookup"><span data-stu-id="5fdce-118">For example:</span></span>

<span data-ttu-id="5fdce-119">20170407T070805_2768037.0000000023. **1111e5ee-1111-111b-a11e-1e111e1111dc**.JSON kiterjesztésű</span><span class="sxs-lookup"><span data-stu-id="5fdce-119">20170407T070805_2768037.0000000023.**1111e5ee-1111-111b-a11e-1e111e1111dc**.json</span></span>

<span data-ttu-id="5fdce-120">Az Azure Active Directory-naplók hello Bérlőazonosító hello nevének részeként tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="5fdce-120">Azure Active Directory audit logs include hello tenant ID as part of hello name.</span></span>

<span data-ttu-id="5fdce-121">Diagnosztikai naplók az eseményközpontban lévő olvasható hello nevének részeként hello előfizetés-azonosító nem tartalmazzák.</span><span class="sxs-lookup"><span data-stu-id="5fdce-121">Diagnostic logs that are read from an event hub do not include hello subscription ID as part of hello name.</span></span> <span data-ttu-id="5fdce-122">Ehelyett tartoznak hello hub eseményforrás hello létrehozásának részeként megadott hello rövid nevét.</span><span class="sxs-lookup"><span data-stu-id="5fdce-122">Instead, they include hello friendly name specified as part of hello creation of hello event hub source.</span></span> 

## <a name="how-can-i-update-hello-proxy-configuration"></a><span data-ttu-id="5fdce-123">Hogyan frissíthetem hello proxykonfigurációt?</span><span class="sxs-lookup"><span data-stu-id="5fdce-123">How can I update hello proxy configuration?</span></span>
<span data-ttu-id="5fdce-124">Ha a megadott proxybeállítást nem teszi lehetővé az Azure storage access közvetlenül, nyissa meg a hello **AZLOG. EXE-FÁJL. CONFIG** fájlt **c:\Program Files\Microsoft Azure napló integrációs**.</span><span class="sxs-lookup"><span data-stu-id="5fdce-124">If your proxy setting does not allow Azure storage access directly, open hello **AZLOG.EXE.CONFIG** file in **c:\Program Files\Microsoft Azure Log Integration**.</span></span> <span data-ttu-id="5fdce-125">Frissítés hello fájl tooinclude hello **defaultProxy** a szervezet hello proxycímmel szakasza.</span><span class="sxs-lookup"><span data-stu-id="5fdce-125">Update hello file tooinclude hello **defaultProxy** section with hello proxy address of your organization.</span></span> <span data-ttu-id="5fdce-126">Hello frissítés után szolgáltatás leállítása és indítása hello hello parancsokkal **net stop azlog** és **net start azlog**.</span><span class="sxs-lookup"><span data-stu-id="5fdce-126">After hello update is done, stop and start hello service by using hello commands **net stop azlog** and **net start azlog**.</span></span>

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

## <a name="how-can-i-see-hello-subscription-information-in-windows-events"></a><span data-ttu-id="5fdce-127">Honnan látom hello előfizetési adatokat a Windows-eseményeket?</span><span class="sxs-lookup"><span data-stu-id="5fdce-127">How can I see hello subscription information in Windows events?</span></span>
<span data-ttu-id="5fdce-128">Hello előfizetési azonosító toohello rövid név hozzáfűzése hello forrás hozzáadása közben:</span><span class="sxs-lookup"><span data-stu-id="5fdce-128">Append hello subscription ID toohello friendly name while adding hello source:</span></span>

    Azlog source add <sourcefriendlyname>.<subscription id> <StorageName> <StorageKey>  
<span data-ttu-id="5fdce-129">hello esemény XML metaadatok, többek között a hello előfizetés-azonosító a következő hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="5fdce-129">hello event XML has hello following metadata, including hello subscription ID:</span></span>

![Esemény XML][1]

## <a name="error-messages"></a><span data-ttu-id="5fdce-131">Hibaüzenetek</span><span class="sxs-lookup"><span data-stu-id="5fdce-131">Error messages</span></span>
### <a name="when-i-run-hello-command-azlog-createazureid-why-do-i-get-hello-following-error"></a><span data-ttu-id="5fdce-132">Hello parancs futtatásakor **azlog createazureid**, miért jelenik meg a következő hiba hello?</span><span class="sxs-lookup"><span data-stu-id="5fdce-132">When I run hello command **azlog createazureid**, why do I get hello following error?</span></span>
<span data-ttu-id="5fdce-133">Hiba:</span><span class="sxs-lookup"><span data-stu-id="5fdce-133">Error:</span></span>

  <span data-ttu-id="5fdce-134">*Nem sikerült az AAD-alkalmazást - bérlő 72f988bf-86f1-41af-91ab-2d7cd011db37 - OK toocreate = "Tiltott" - Message = "a megfelelő jogosultságok hiánya toocomplete hello művelet."*</span><span class="sxs-lookup"><span data-stu-id="5fdce-134">*Failed toocreate AAD Application - Tenant 72f988bf-86f1-41af-91ab-2d7cd011db37 - Reason = 'Forbidden' - Message = 'Insufficient privileges toocomplete hello operation.'*</span></span>

<span data-ttu-id="5fdce-135">Hello **azlog createazureid** parancs megpróbálja toocreate hozzáféréssel rendelkezik az összes hello Azure AD bérlők hello Azure bejelentkezési hello előfizetésekhez egy egyszerű szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="5fdce-135">hello **azlog createazureid** command attempts toocreate a service principal in all hello Azure AD tenants for hello subscriptions that hello Azure login has access to.</span></span> <span data-ttu-id="5fdce-136">Ha az Azure bejelentkezési csak a Vendég felhasználó az Azure AD bérlőre, hello parancs az "a megfelelő jogosultságok hiánya toocomplete hello műveletet."</span><span class="sxs-lookup"><span data-stu-id="5fdce-136">If your Azure login is only a guest user in that Azure AD tenant, hello command fails with "Insufficient privileges toocomplete hello operation."</span></span> <span data-ttu-id="5fdce-137">Kérje meg a hello Bérlői rendszergazda tooadd hello bérlői felhasználói fiókját.</span><span class="sxs-lookup"><span data-stu-id="5fdce-137">Ask hello tenant admin tooadd your account as a user in hello tenant.</span></span>

### <a name="when-i-run-hello-command-azlog-authorize-why-do-i-get-hello-following-error"></a><span data-ttu-id="5fdce-138">Hello parancs futtatásakor **azlog engedélyezik**, miért jelenik meg a következő hiba hello?</span><span class="sxs-lookup"><span data-stu-id="5fdce-138">When I run hello command **azlog authorize**, why do I get hello following error?</span></span>
<span data-ttu-id="5fdce-139">Hiba:</span><span class="sxs-lookup"><span data-stu-id="5fdce-139">Error:</span></span>

  <span data-ttu-id="5fdce-140">*Figyelmeztetés a szerepkör-hozzárendelés - AuthorizationFailed létrehozása: hello ügyfél janedo@microsoft.com"objektum"fe9e03e4-4dad-4328-910f-fd24a9660bd2"azonosítója nem tartozik engedélyezési tooperform művelet"Microsoft.Authorization/roleAssignments/write"hatókörben" / 97 – b971-0d8ff0000000 előfizetések / 70d 95299-d689-4c ".*</span><span class="sxs-lookup"><span data-stu-id="5fdce-140">*Warning creating Role Assignment - AuthorizationFailed: hello client janedo@microsoft.com' with object id 'fe9e03e4-4dad-4328-910f-fd24a9660bd2' does not have authorization tooperform action 'Microsoft.Authorization/roleAssignments/write' over scope '/subscriptions/70d95299-d689-4c97-b971-0d8ff0000000'.*</span></span>

<span data-ttu-id="5fdce-141">Hello **azlog engedélyezik** rendel hello olvasó toohello az Azure AD szolgáltatás egyszerű szerepe parancs (létre **azlog createazureid**) megadott toohello előfizetések.</span><span class="sxs-lookup"><span data-stu-id="5fdce-141">hello **azlog authorize** command assigns hello role of reader toohello Azure AD service principal (created with **azlog createazureid**) toohello provided subscriptions.</span></span> <span data-ttu-id="5fdce-142">Ha hello Azure bejelentkezési nem társadminisztrátorának vagy hello előfizetés tulajdonosa, azt "Engedélyezési sikertelen" hibaüzenettel meghiúsul.</span><span class="sxs-lookup"><span data-stu-id="5fdce-142">If hello Azure login is not a co-administrator or an owner of hello subscription, it fails with an "Authorization Failed" error message.</span></span> <span data-ttu-id="5fdce-143">Azure szerepköralapú hozzáférés-vezérlés (RBAC) társadminisztrátoraként vagy a tulajdonos szükséges toocomplete van ezzel a művelettel.</span><span class="sxs-lookup"><span data-stu-id="5fdce-143">Azure Role-Based Access Control (RBAC) of co-administrator or owner is needed toocomplete this action.</span></span>

## <a name="where-can-i-find-hello-definition-of-hello-properties-in-hello-audit-log"></a><span data-ttu-id="5fdce-144">Hol találok hello tulajdonságok hello definíciója hello naplóban?</span><span class="sxs-lookup"><span data-stu-id="5fdce-144">Where can I find hello definition of hello properties in hello audit log?</span></span>
<span data-ttu-id="5fdce-145">Lásd:</span><span class="sxs-lookup"><span data-stu-id="5fdce-145">See:</span></span>

* [<span data-ttu-id="5fdce-146">Naplózási műveletek az Azure Resource Manager eszközzel</span><span class="sxs-lookup"><span data-stu-id="5fdce-146">Audit operations with Azure Resource Manager</span></span>](../azure-resource-manager/resource-group-audit.md)
* [<span data-ttu-id="5fdce-147">Lista hello felügyeleti események hello Azure figyelő REST API-t az előfizetés</span><span class="sxs-lookup"><span data-stu-id="5fdce-147">List hello management events in a subscription in hello Azure Monitor REST API</span></span>](https://msdn.microsoft.com/library/azure/dn931934.aspx)

## <a name="where-can-i-find-details-on-azure-security-center-alerts"></a><span data-ttu-id="5fdce-148">Hol található az Azure Security Center riasztásait részleteit?</span><span class="sxs-lookup"><span data-stu-id="5fdce-148">Where can I find details on Azure Security Center alerts?</span></span>
<span data-ttu-id="5fdce-149">Lásd: [az Azure Security Centerben riasztások kezelése és válaszol toosecurity](../security-center/security-center-managing-and-responding-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="5fdce-149">See [Managing and responding toosecurity alerts in Azure Security Center](../security-center/security-center-managing-and-responding-alerts.md).</span></span>

## <a name="how-can-i-modify-what-is-collected-with-vm-diagnostics"></a><span data-ttu-id="5fdce-150">Hogyan módosítható meg, hogy a Virtuálisgép-diagnosztika gyűjtött adatok?</span><span class="sxs-lookup"><span data-stu-id="5fdce-150">How can I modify what is collected with VM diagnostics?</span></span>
<span data-ttu-id="5fdce-151">Hogyan tooget, módosítására és hello Azure Diagnostics-konfigurációját állítsa témakör [használja a Powershellt tooenable Azure Diagnostics Windows rendszerű virtuális gépként](../virtual-machines/windows/ps-extensions-diagnostics.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="5fdce-151">For details on how tooget, modify, and set hello Azure Diagnostics configuration, see [Use PowerShell tooenable Azure Diagnostics in a virtual machine running Windows](../virtual-machines/windows/ps-extensions-diagnostics.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> 

<span data-ttu-id="5fdce-152">a következő példa hello hello Azure Diagnostics konfigurációs beolvasása:</span><span class="sxs-lookup"><span data-stu-id="5fdce-152">hello following example gets hello Azure Diagnostics configuration:</span></span>

    -AzureRmVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient
    $publicsettings = (Get-AzureRmVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient).PublicSettings
    $encodedconfig = (ConvertFrom-Json -InputObject $publicsettings).xmlCfg
    $xmlconfig = [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($encodedconfig))
    Write-Host $xmlconfig

    $xmlconfig | Out-File -Encoding utf8 -FilePath "d:\WADConfig.xml"

<span data-ttu-id="5fdce-153">a következő példa hello módosítja a hello Azure Diagnostics konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="5fdce-153">hello following example modifies hello Azure Diagnostics configuration.</span></span> <span data-ttu-id="5fdce-154">Ebben a konfigurációban csak azonosító 4624 és esemény azonosítója 4625 hello biztonsági eseménynaplójából gyűjti.</span><span class="sxs-lookup"><span data-stu-id="5fdce-154">In this configuration, only event ID 4624 and event ID 4625 are collected from hello security event log.</span></span> <span data-ttu-id="5fdce-155">A Microsoft Antimalware Azure események begyűjti hello rendszer-eseménynaplójában.</span><span class="sxs-lookup"><span data-stu-id="5fdce-155">Microsoft Antimalware for Azure events are collected from hello system event log.</span></span> <span data-ttu-id="5fdce-156">További részletek az XPath kifejezések hello használata: [fel események](https://msdn.microsoft.com/library/windows/desktop/dd996910(v=vs.85)).</span><span class="sxs-lookup"><span data-stu-id="5fdce-156">For details on hello use of XPath expressions, see [Consuming Events](https://msdn.microsoft.com/library/windows/desktop/dd996910(v=vs.85)).</span></span>

    <WindowsEventLog scheduledTransferPeriod="PT1M">
        <DataSource name="Security!*[System[(EventID=4624 or EventID=4625)]]" />
        <DataSource name="System!*[System[Provider[@Name='Microsoft Antimalware']]]"/>
    </WindowsEventLog>

<span data-ttu-id="5fdce-157">hello alábbi mintakód hello Azure diagnosztikai konfiguráció:</span><span class="sxs-lookup"><span data-stu-id="5fdce-157">hello following example sets hello Azure Diagnostics configuration:</span></span>

    $diagnosticsconfig_path = "d:\WADConfig.xml"
    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient -DiagnosticsConfigurationPath $diagnosticsconfig_path -StorageAccountName log3121 -StorageAccountKey <storage key>

<span data-ttu-id="5fdce-158">Módosítása után ellenőrizze, hogy helyes-e eseményeket a rendszer gyűjti hello hello tárolási fiók tooensure.</span><span class="sxs-lookup"><span data-stu-id="5fdce-158">After you make changes, check hello storage account tooensure that hello correct events are collected.</span></span>

<span data-ttu-id="5fdce-159">Ha probléma merül fel hello telepítés és konfigurálás során, nyisson meg egy [támogatási kérelem](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request).</span><span class="sxs-lookup"><span data-stu-id="5fdce-159">If you have any issues during hello installation and configuration, please open a [support request](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request).</span></span> <span data-ttu-id="5fdce-160">Válassza ki **napló integrációs** hello szolgáltatást, amelynek támogatási kérelmet.</span><span class="sxs-lookup"><span data-stu-id="5fdce-160">Select **Log Integration** as hello service for which you are requesting support.</span></span>

## <a name="can-i-use-azure-log-integration-toointegrate-network-watcher-logs-into-my-siem"></a><span data-ttu-id="5fdce-161">A saját SIEM is használható Azure napló integrációs toointegrate hálózati figyelőt naplókat?</span><span class="sxs-lookup"><span data-stu-id="5fdce-161">Can I use Azure Log Integration toointegrate Network Watcher logs into my SIEM?</span></span>

<span data-ttu-id="5fdce-162">Az Azure hálózati figyelőt naplózási információk nagy mennyiségű állít elő.</span><span class="sxs-lookup"><span data-stu-id="5fdce-162">Azure Network Watcher generates large quantities of logging information.</span></span> <span data-ttu-id="5fdce-163">Ezek a naplók nem küldött jelentette toobe tooa SIEM.</span><span class="sxs-lookup"><span data-stu-id="5fdce-163">These logs are not meant toobe sent tooa SIEM.</span></span> <span data-ttu-id="5fdce-164">csak a támogatott hello hálózati figyelőt naplók célja egy tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="5fdce-164">hello only supported destination for Network Watcher logs is a storage account.</span></span> <span data-ttu-id="5fdce-165">Azure napló-integráció nem támogatja ezek a naplók beolvasásakor, és így válnak elérhetővé tooa SIEM.</span><span class="sxs-lookup"><span data-stu-id="5fdce-165">Azure Log Integration does not support reading these logs and making them available tooa SIEM.</span></span>

<!--Image references-->
[1]: ./media/security-azure-log-integration-faq/event-xml.png
