---
title: "az Azure mikroszolgáltatások biztonsági házirendekkel kapcsolatos aaaLearn |} Microsoft Docs"
description: "Hogyan toorun a Service Fabric-alkalmazás, a rendszer és a helyi biztonsági fiókok, beleértve a hello SetupEntry pont, amikor a kérelmet kell tooperform néhány privilegizált művelet megkezdése előtt áttekintése"
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: 
ms.assetid: 4242a1eb-a237-459b-afbf-1e06cfa72732
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/30/2017
ms.author: mfussell
ms.openlocfilehash: f5afba69e09aa4f3c9efa4d3efc6995c813a1f71
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-security-policies-for-your-application"></a><span data-ttu-id="8cf01-103">Biztonsági házirendek beállítása az alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="8cf01-103">Configure security policies for your application</span></span>
<span data-ttu-id="8cf01-104">Azure Service Fabric használatával biztonságossá teheti a különböző felhasználói fiókok hello fürtben futó alkalmazás számára.</span><span class="sxs-lookup"><span data-stu-id="8cf01-104">By using Azure Service Fabric, you can secure applications that are running in hello cluster under different user accounts.</span></span> <span data-ttu-id="8cf01-105">A Service Fabric is lehetővé teszi az alkalmazások által használt hello időpontban a központi telepítés hello felhasználói fiókokon – például biztonságos hello erőforrásokat, fájlok, könyvtárak és tanúsítványokat.</span><span class="sxs-lookup"><span data-stu-id="8cf01-105">Service Fabric also helps secure hello resources that are used by applications at hello time of deployment under hello user accounts--for example, files, directories, and certificates.</span></span> <span data-ttu-id="8cf01-106">Így futó alkalmazást, még akkor is, a megosztott üzemeltetési környezetben, nagyobb biztonságot nyújt, egymástól.</span><span class="sxs-lookup"><span data-stu-id="8cf01-106">This makes running applications, even in a shared hosted environment, more secure from one another.</span></span>

<span data-ttu-id="8cf01-107">Alapértelmezés szerint a Service Fabric-alkalmazások hello fiókkal, amely alatt futó hello Fabric.exe folyamatban fut.</span><span class="sxs-lookup"><span data-stu-id="8cf01-107">By default, Service Fabric applications run under hello account that hello Fabric.exe process runs under.</span></span> <span data-ttu-id="8cf01-108">A Service Fabric is lehetővé teszi hello toorun alkalmazások helyi felhasználói fiók vagy helyi rendszerfiók, belül hello alkalmazás jegyzékében meg.</span><span class="sxs-lookup"><span data-stu-id="8cf01-108">Service Fabric also provides hello capability toorun applications under a local user account or local system account, which is specified within hello application manifest.</span></span> <span data-ttu-id="8cf01-109">Támogatott helyi rendszer fiók típusok **LocalUser**, **NetworkService**, **LocalService**, és **LocalSystem**.</span><span class="sxs-lookup"><span data-stu-id="8cf01-109">Supported local system account types are **LocalUser**, **NetworkService**, **LocalService**, and **LocalSystem**.</span></span>

 <span data-ttu-id="8cf01-110">Ha a számítógépén a Service Fabric Windows Server az adatközpontban található hello önálló telepítő használatával, az Active Directory tartományi fiókok, beleértve a csoportosan felügyelt szolgáltatásfiókok is használhatja.</span><span class="sxs-lookup"><span data-stu-id="8cf01-110">When you're running Service Fabric on Windows Server in your datacenter by using hello standalone installer, you can use Active Directory domain accounts, including group managed service accounts.</span></span>

<span data-ttu-id="8cf01-111">Adja meg, és hozzon létre felhasználói csoportokat, hogy egy vagy több felhasználó tooeach csoport toobe felügyelete együtt adható hozzá.</span><span class="sxs-lookup"><span data-stu-id="8cf01-111">You can define and create user groups so that one or more users can be added tooeach group toobe managed together.</span></span> <span data-ttu-id="8cf01-112">Ez akkor hasznos, ha több felhasználó különböző belépési pontot, és bizonyos közös jogosultságok által biztosított hello csoportok szintjén kell toohave.</span><span class="sxs-lookup"><span data-stu-id="8cf01-112">This is useful when there are multiple users for different service entry points and they need toohave certain common privileges that are available at hello group level.</span></span>

## <a name="configure-hello-policy-for-a-service-setup-entry-point"></a><span data-ttu-id="8cf01-113">A telepítő belépési pont hello szabályzat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="8cf01-113">Configure hello policy for a service setup entry point</span></span>
<span data-ttu-id="8cf01-114">Hello leírtak [alkalmazásmodell](service-fabric-application-model.md), hello beállítása belépési pont, **SetupEntryPoint**, egy kiemelt belépési pontja, amelynek ugyanaz, mint a Service Fabric hitelesítő adatok hello (általában hello *NetworkService* fiók) más belépési pont előtt.</span><span class="sxs-lookup"><span data-stu-id="8cf01-114">As described in hello [application model](service-fabric-application-model.md), hello setup entry point, **SetupEntryPoint**, is a privileged entry point that runs with hello same credentials as Service Fabric (typically hello *NetworkService* account) before any other entry point.</span></span> <span data-ttu-id="8cf01-115">hello végrehajtható fájl megadott **EntryPoint** általában hello hosszan futó szolgáltatás gazdagép legyen.</span><span class="sxs-lookup"><span data-stu-id="8cf01-115">hello executable that is specified by **EntryPoint** is typically hello long-running service host.</span></span> <span data-ttu-id="8cf01-116">Ezért ha egy külön telepítő belépési pont elkerülhető toorun hello szolgáltatásgazda végrehajtható magas jogosultsággal rendelkező huzamosabb ideig.</span><span class="sxs-lookup"><span data-stu-id="8cf01-116">So having a separate setup entry point avoids having toorun hello service host executable with high privileges for extended periods of time.</span></span> <span data-ttu-id="8cf01-117">hello végrehajtható, amely **EntryPoint** megadja futtatása **SetupEntryPoint** sikeresen kilép.</span><span class="sxs-lookup"><span data-stu-id="8cf01-117">hello executable that **EntryPoint** specifies is run after **SetupEntryPoint** exits successfully.</span></span> <span data-ttu-id="8cf01-118">hello eredményül kapott folyamat figyeli, és újra kell indítani, és újra kezdődik **SetupEntryPoint** Ha valaha is leáll, vagy összeomlik.</span><span class="sxs-lookup"><span data-stu-id="8cf01-118">hello resulting process is monitored and restarted, and begins again with **SetupEntryPoint** if it ever terminates or crashes.</span></span>

<span data-ttu-id="8cf01-119">hello következő látható egy egyszerű service manifest példa, hogy hello SetupEntryPoint mutat be, és hello hello szolgáltatás fő belépési pont.</span><span class="sxs-lookup"><span data-stu-id="8cf01-119">hello following is a simple service manifest example that shows hello SetupEntryPoint and hello main EntryPoint for hello service.</span></span>

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ServiceManifest Name="MyServiceManifest" Version="SvcManifestVersion1" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example service manifest</Description>
  <ServiceTypes>
    <StatelessServiceType ServiceTypeName="MyServiceType" />
  </ServiceTypes>
  <CodePackage Name="Code" Version="1.0.0">
    <SetupEntryPoint>
      <ExeHost>
        <Program>MySetup.bat</Program>
        <WorkingFolder>CodePackage</WorkingFolder>
      </ExeHost>
    </SetupEntryPoint>
    <EntryPoint>
      <ExeHost>
        <Program>MyServiceHost.exe</Program>
      </ExeHost>
    </EntryPoint>
  </CodePackage>
  <ConfigPackage Name="Config" Version="1.0.0" />
</ServiceManifest>
```

### <a name="configure-hello-policy-by-using-a-local-account"></a><span data-ttu-id="8cf01-120">Helyi fiók használatával hello házirend konfigurálása</span><span class="sxs-lookup"><span data-stu-id="8cf01-120">Configure hello policy by using a local account</span></span>
<span data-ttu-id="8cf01-121">Hello szolgáltatás toohave egy telepítő belépési pont konfigurálása után módosíthatja, amely akkor fut a hello alkalmazásjegyzékben hello biztonsági engedélyeket.</span><span class="sxs-lookup"><span data-stu-id="8cf01-121">After you configure hello service toohave a setup entry point, you can change hello security permissions that it runs under in hello application manifest.</span></span> <span data-ttu-id="8cf01-122">hello a következő példa bemutatja, hogyan tooconfigure hello szolgáltatás toorun a felhasználó rendszergazdai jogosultságot.</span><span class="sxs-lookup"><span data-stu-id="8cf01-122">hello following example shows how tooconfigure hello service toorun under user administrator account privileges.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyServiceTypePkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="SetupAdminUser" EntryPointType="Setup" />
      </Policies>
   </ServiceManifestImport>
   <Principals>
      <Users>
         <User Name="SetupAdminUser">
            <MemberOf>
               <SystemGroup Name="Administrators" />
            </MemberOf>
         </User>
      </Users>
   </Principals>
</ApplicationManifest>
```

<span data-ttu-id="8cf01-123">Először hozzon létre egy **rendszerbiztonsági tagok** egy felhasználónevet, pl. SetupAdminUser szakasz ismerteti.</span><span class="sxs-lookup"><span data-stu-id="8cf01-123">First, create a **Principals** section with a user name, such as SetupAdminUser.</span></span> <span data-ttu-id="8cf01-124">Ez azt jelzi, hogy a felhasználó hello hello rendszergazdák system csoport tagja.</span><span class="sxs-lookup"><span data-stu-id="8cf01-124">This indicates that hello user is a member of hello Administrators system group.</span></span>

<span data-ttu-id="8cf01-125">Ezután bontsa hello **ServiceManifestImport** területen konfigurálja a házirend tooapply rendszerbiztonsági túl**SetupEntryPoint**.</span><span class="sxs-lookup"><span data-stu-id="8cf01-125">Next, under hello **ServiceManifestImport** section, configure a policy tooapply this principal too**SetupEntryPoint**.</span></span> <span data-ttu-id="8cf01-126">Ez alapján a Service Fabric, amikor hello **MySetup.bat** futtatása, kell lennie `RunAs` rendszergazdai jogosultságokkal.</span><span class="sxs-lookup"><span data-stu-id="8cf01-126">This tells Service Fabric that when hello **MySetup.bat** file is run, it should be `RunAs` with administrator privileges.</span></span> <span data-ttu-id="8cf01-127">Fényében, hogy már rendelkezik *nem* alkalmazza a házirend toohello fő belépési pont, a kód hello **MyServiceHost.exe** hello rendszer alatt futó **NetworkService** fiók.</span><span class="sxs-lookup"><span data-stu-id="8cf01-127">Given that you have *not* applied a policy toohello main entry point, hello code in **MyServiceHost.exe** runs under hello system **NetworkService** account.</span></span> <span data-ttu-id="8cf01-128">Ez az hello alapértelmezett fiókot, amely az összes szolgáltatás belépési pont alatt futnak.</span><span class="sxs-lookup"><span data-stu-id="8cf01-128">This is hello default account that all service entry points are run as.</span></span>

<span data-ttu-id="8cf01-129">Adjuk hozzá most hello fájl MySetup.bat toohello Visual Studio projekt tootest hello rendszergazdai jogosultságokkal.</span><span class="sxs-lookup"><span data-stu-id="8cf01-129">Let's now add hello file MySetup.bat toohello Visual Studio project tootest hello administrator privileges.</span></span> <span data-ttu-id="8cf01-130">A Visual Studióban kattintson a jobb gombbal a projekt hello, és adja hozzá a MySetup.bat nevű új fájlt.</span><span class="sxs-lookup"><span data-stu-id="8cf01-130">In Visual Studio, right-click hello service project and add a new file called MySetup.bat.</span></span>

<span data-ttu-id="8cf01-131">A következő győződjön meg arról, hogy hello MySetup.bat fájl hello szolgáltatás csomagban található.</span><span class="sxs-lookup"><span data-stu-id="8cf01-131">Next, ensure that hello MySetup.bat file is included in hello service package.</span></span> <span data-ttu-id="8cf01-132">Alapértelmezés szerint nincs.</span><span class="sxs-lookup"><span data-stu-id="8cf01-132">By default, it is not.</span></span> <span data-ttu-id="8cf01-133">Válassza ki hello fájlt, kattintson a jobb gombbal a tooget hello helyi menüt, és válassza a **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="8cf01-133">Select hello file, right-click tooget hello context menu, and choose **Properties**.</span></span> <span data-ttu-id="8cf01-134">A hello tulajdonságai párbeszédpanelen győződjön meg arról, hogy **tooOutput Directory másolása** értéke túl**másolhatja, ha újabb**.</span><span class="sxs-lookup"><span data-stu-id="8cf01-134">In hello Properties dialog box, ensure that **Copy tooOutput Directory** is set too**Copy if newer**.</span></span> <span data-ttu-id="8cf01-135">Tekintse meg a következő képernyőkép hello.</span><span class="sxs-lookup"><span data-stu-id="8cf01-135">See hello following screenshot.</span></span>

![A Visual Studio CopyToOutput SetupEntryPoint kötegelt fájl][image1]

<span data-ttu-id="8cf01-137">Most nyissa meg hello MySetup.bat fájlt, és adja hozzá a következő parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="8cf01-137">Now open hello MySetup.bat file and add hello following commands:</span></span>

```
REM Set a system environment variable. This requires administrator privilege
setx -m TestVariable "MyValue"
echo System TestVariable set too> out.txt
echo %TestVariable% >> out.txt

REM toodelete this system variable us
REM REG delete "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Environment" /v TestVariable /f
```

<span data-ttu-id="8cf01-138">Ezt követően hozza létre, és hello megoldás tooa helyi fejlesztési fürtök telepítése.</span><span class="sxs-lookup"><span data-stu-id="8cf01-138">Next, build and deploy hello solution tooa local development cluster.</span></span> <span data-ttu-id="8cf01-139">Hello szolgáltatás elindult, a Service Fabric Explorerben látható, miután láthatja, hogy hello MySetup.bat fájl két módon sikeres volt.</span><span class="sxs-lookup"><span data-stu-id="8cf01-139">After hello service has started, as shown in Service Fabric Explorer, you can see that hello MySetup.bat file was successful in a two ways.</span></span> <span data-ttu-id="8cf01-140">Nyisson meg egy PowerShell-parancssort, és írja be:</span><span class="sxs-lookup"><span data-stu-id="8cf01-140">Open a PowerShell command prompt and type:</span></span>

```
PS C:\ [Environment]::GetEnvironmentVariable("TestVariable","Machine")
MyValue
```

<span data-ttu-id="8cf01-141">Jegyezze fel hello csomópont ahol hello szolgáltatást telepítették, és elindítani a Service Fabric Explorerben – például csomópont 2 hello neve.</span><span class="sxs-lookup"><span data-stu-id="8cf01-141">Then, note hello name of hello node where hello service was deployed and started in Service Fabric Explorer--for example, Node 2.</span></span> <span data-ttu-id="8cf01-142">Ezután keresse meg a toohello példány munkahelyi mappa toofind hello out.txt alkalmazásfájl hello értékének megjelenítő **TestVariable**.</span><span class="sxs-lookup"><span data-stu-id="8cf01-142">Next, navigate toohello application instance work folder toofind hello out.txt file that shows hello value of **TestVariable**.</span></span> <span data-ttu-id="8cf01-143">Például, ha a szolgáltatás telepített tooNode 2 volt, majd elvégezheti a toothis elérési útját hello **MyApplicationType**:</span><span class="sxs-lookup"><span data-stu-id="8cf01-143">For example, if this service was deployed tooNode 2, then you can go toothis path for hello **MyApplicationType**:</span></span>

```
C:\SfDevCluster\Data\_App\Node.2\MyApplicationType_App\work\out.txt
```

### <a name="configure-hello-policy-by-using-local-system-accounts"></a><span data-ttu-id="8cf01-144">Hello házirend konfigurálása a helyi rendszer fiók használatával</span><span class="sxs-lookup"><span data-stu-id="8cf01-144">Configure hello policy by using local system accounts</span></span>
<span data-ttu-id="8cf01-145">Gyakran érdemes előnyösebb toorun hello indítási parancsfájl rendszergazdai fiók helyett a helyi rendszer fiók használatával.</span><span class="sxs-lookup"><span data-stu-id="8cf01-145">Often, it's preferable toorun hello startup script by using a local system account rather than an administrator account.</span></span> <span data-ttu-id="8cf01-146">A Rendszergazdák csoport általában nem működik jól, mert a felhasználói hozzáférés felügyelete (UAC) alapértelmezés szerint engedélyezve vannak hello tagjaként hello RunAs házirend fut.</span><span class="sxs-lookup"><span data-stu-id="8cf01-146">Running hello RunAs policy as a member of hello Administrators group typically doesn’t work well because machines have User Access Control (UAC) enabled by default.</span></span> <span data-ttu-id="8cf01-147">Ilyen esetben **helyi felhasználói hozzáadott tooAdministrators csoportként hello ajánljuk, az helyett toorun hello LocalSystem, SetupEntryPoint**.</span><span class="sxs-lookup"><span data-stu-id="8cf01-147">In such cases, **hello recommendation is toorun hello SetupEntryPoint as LocalSystem, instead of as a local user added tooAdministrators group**.</span></span> <span data-ttu-id="8cf01-148">hello alábbi példa bemutatja a beállítás hello SetupEntryPoint toorun LocalSystem néven:</span><span class="sxs-lookup"><span data-stu-id="8cf01-148">hello following example shows setting hello SetupEntryPoint toorun as LocalSystem:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyServiceTypePkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="SetupLocalSystem" EntryPointType="Setup" />
      </Policies>
   </ServiceManifestImport>
   <Principals>
      <Users>
         <User Name="SetupLocalSystem" AccountType="LocalSystem" />
      </Users>
   </Principals>
</ApplicationManifest>
```

## <a name="start-powershell-commands-from-a-setup-entry-point"></a><span data-ttu-id="8cf01-149">A telepítő belépési pontról indítsa el a PowerShell-parancsok</span><span class="sxs-lookup"><span data-stu-id="8cf01-149">Start PowerShell commands from a setup entry point</span></span>
<span data-ttu-id="8cf01-150">a hello PowerShell toorun **SetupEntryPoint** pont, futtathatja **PowerShell.exe** mutat, tooa PowerShell parancsfájlban fájlt.</span><span class="sxs-lookup"><span data-stu-id="8cf01-150">toorun PowerShell from hello **SetupEntryPoint** point, you can run **PowerShell.exe** in a batch file that points tooa PowerShell file.</span></span> <span data-ttu-id="8cf01-151">Első lépésként adja meg egy PowerShell fájl toohello projekt – például **MySetup.ps1**.</span><span class="sxs-lookup"><span data-stu-id="8cf01-151">First, add a PowerShell file toohello service project--for example, **MySetup.ps1**.</span></span> <span data-ttu-id="8cf01-152">Ne feledje tooset hello *másolhatja, ha újabb* tulajdonság úgy, hogy hello fájl is hello szolgáltatás csomagban található.</span><span class="sxs-lookup"><span data-stu-id="8cf01-152">Remember tooset hello *Copy if newer* property so that hello file is also included in hello service package.</span></span> <span data-ttu-id="8cf01-153">hello alábbi példa azt mutatja, amely elindítja a MySetup.ps1, amelyek a nevezett rendszer környezeti változó beállítása nevű PowerShell fájl kötegelt mintafájl **TestVariable**.</span><span class="sxs-lookup"><span data-stu-id="8cf01-153">hello following example shows a sample batch file that starts a PowerShell file called MySetup.ps1, which sets a system environment variable called **TestVariable**.</span></span>

<span data-ttu-id="8cf01-154">MySetup.bat toostart egy PowerShell-fájlt:</span><span class="sxs-lookup"><span data-stu-id="8cf01-154">MySetup.bat toostart a PowerShell file:</span></span>

```
powershell.exe -ExecutionPolicy Bypass -Command ".\MySetup.ps1"
```

<span data-ttu-id="8cf01-155">Hello PowerShell fájlban adja hozzá a következő környezeti változót tooset hello:</span><span class="sxs-lookup"><span data-stu-id="8cf01-155">In hello PowerShell file, add hello following tooset a system environment variable:</span></span>

```
[Environment]::SetEnvironmentVariable("TestVariable", "MyValue", "Machine")
[Environment]::GetEnvironmentVariable("TestVariable","Machine") > out.txt
```

> [!NOTE]
> <span data-ttu-id="8cf01-156">Alapértelmezés szerint hello kötegfájl futtatásakor jelek nevű hello alkalmazásmappa **működik** fájlok.</span><span class="sxs-lookup"><span data-stu-id="8cf01-156">By default, when hello batch file runs, it looks at hello application folder called **work** for files.</span></span> <span data-ttu-id="8cf01-157">Ebben az esetben, amikor MySetup.bat fut, azt szeretnénk, ha a toofind hello MySetup.ps1 fájlt hello azonos mappába, amely hello alkalmazás **kódcsomag** mappa.</span><span class="sxs-lookup"><span data-stu-id="8cf01-157">In this case, when MySetup.bat runs, we want this toofind hello MySetup.ps1 file in hello same folder, which is hello application **code package** folder.</span></span> <span data-ttu-id="8cf01-158">toochange ezt a mappát, a set hello munkamappa:</span><span class="sxs-lookup"><span data-stu-id="8cf01-158">toochange this folder, set hello working folder:</span></span>
> 
> 

```xml
<SetupEntryPoint>
    <ExeHost>
    <Program>MySetup.bat</Program>
    <WorkingFolder>CodePackage</WorkingFolder>
    </ExeHost>
</SetupEntryPoint>
```

## <a name="use-console-redirection-for-local-debugging"></a><span data-ttu-id="8cf01-159">A hibakereséshez a helyi konzol átirányítást használ</span><span class="sxs-lookup"><span data-stu-id="8cf01-159">Use console redirection for local debugging</span></span>
<span data-ttu-id="8cf01-160">Alkalmanként is hasznos toosee hello konzol kimeneti hibakeresési célra parancsfájl futtatását.</span><span class="sxs-lookup"><span data-stu-id="8cf01-160">Occasionally, it's useful toosee hello console output from running a script for debugging purposes.</span></span> <span data-ttu-id="8cf01-161">toodo, beállíthatja a konzol átirányítási házirend, amely hello kimeneti tooa fájlba írja.</span><span class="sxs-lookup"><span data-stu-id="8cf01-161">toodo this, you can set a console redirection policy, which writes hello output tooa file.</span></span> <span data-ttu-id="8cf01-162">hello fájl kimeneti alkalmazásmappa írásbeli toohello nevezik **napló** hello csomóponton, amelyen hello alkalmazás üzemel, és futtassa.</span><span class="sxs-lookup"><span data-stu-id="8cf01-162">hello file output is written toohello application folder called **log** on hello node where hello application is deployed and run.</span></span> <span data-ttu-id="8cf01-163">(Lásd: a where toofind ezt az előző példa hello.)</span><span class="sxs-lookup"><span data-stu-id="8cf01-163">(See where toofind this in hello preceding example.)</span></span>

> [!WARNING]
> <span data-ttu-id="8cf01-164">Az alkalmazásban, mert ez hatással lehet a hello alkalmazás feladatátvételi éles környezetben telepített soha ne használja a hello konzol átirányítási házirend.</span><span class="sxs-lookup"><span data-stu-id="8cf01-164">Never use hello console redirection policy in an application that is deployed in production because this can affect hello application failover.</span></span> <span data-ttu-id="8cf01-165">*Csak* használja ezt a helyi fejlesztési és hibakeresési célra.</span><span class="sxs-lookup"><span data-stu-id="8cf01-165">*Only* use this for local development and debugging purposes.</span></span>  
> 
> 

<span data-ttu-id="8cf01-166">hello alábbi példa bemutatja a beállítás hello segítségével FileRetentionCount értékkel:</span><span class="sxs-lookup"><span data-stu-id="8cf01-166">hello following example shows setting hello console redirection with a FileRetentionCount value:</span></span>

```xml
<SetupEntryPoint>
    <ExeHost>
    <Program>MySetup.bat</Program>
    <WorkingFolder>CodePackage</WorkingFolder>
    <ConsoleRedirection FileRetentionCount="10"/>
    </ExeHost>
</SetupEntryPoint>
```

<span data-ttu-id="8cf01-167">Ha most módosítja hello MySetup.ps1 fájl toowrite egy **Echo** parancsot fog kiírni, a hibakeresési célra toohello kimeneti fájl:</span><span class="sxs-lookup"><span data-stu-id="8cf01-167">If you now change hello MySetup.ps1 file toowrite an **Echo** command, this will write toohello output file for debugging purposes:</span></span>

```
Echo "Test console redirection which writes toohello application log folder on hello node that hello application is deployed to"
```

<span data-ttu-id="8cf01-168">**A parancsfájl hibakeresése azonnal távolítsa el a konzol átirányítási házirend**.</span><span class="sxs-lookup"><span data-stu-id="8cf01-168">**After you debug your script, immediately remove this console redirection policy**.</span></span>

## <a name="configure-a-policy-for-service-code-packages"></a><span data-ttu-id="8cf01-169">A szolgáltatás kód csomagok házirend konfigurálása</span><span class="sxs-lookup"><span data-stu-id="8cf01-169">Configure a policy for service code packages</span></span>
<span data-ttu-id="8cf01-170">Az előző lépésekben hello, megtudhatta, hogyan tooapply egy csoportházirend RunAs tooSetupEntryPoint.</span><span class="sxs-lookup"><span data-stu-id="8cf01-170">In hello preceding steps, you saw how tooapply a RunAs policy tooSetupEntryPoint.</span></span> <span data-ttu-id="8cf01-171">Nézzük kissé mélyebb be, hogyan toocreate különböző rendszerbiztonsági tagok közül melyek alkalmazhatók, mint szolgáltatás házirendek.</span><span class="sxs-lookup"><span data-stu-id="8cf01-171">Let's look a little deeper into how toocreate different principals that can be applied as service policies.</span></span>

### <a name="create-local-user-groups"></a><span data-ttu-id="8cf01-172">Helyi felhasználói csoportok létrehozása</span><span class="sxs-lookup"><span data-stu-id="8cf01-172">Create local user groups</span></span>
<span data-ttu-id="8cf01-173">Határozza meg, és hozzon létre felhasználói csoportokat, amelyek lehetővé teszik egy vagy több felhasználók toobe hozzáadta tooa csoportot.</span><span class="sxs-lookup"><span data-stu-id="8cf01-173">You can define and create user groups that allow one or more users toobe added tooa group.</span></span> <span data-ttu-id="8cf01-174">Ez akkor hasznos, ha több felhasználó különböző belépési pontot, és bizonyos közös jogosultságok által biztosított hello csoportok szintjén kell toohave.</span><span class="sxs-lookup"><span data-stu-id="8cf01-174">This is useful if there are multiple users for different service entry points and they need toohave certain common privileges that are available at hello group level.</span></span> <span data-ttu-id="8cf01-175">hello következő példa bemutatja egy nevű helyi csoporthoz **LocalAdminGroup** , amely rendszergazdai jogosultságokkal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="8cf01-175">hello following example shows a local group called **LocalAdminGroup** that has administrator privileges.</span></span> <span data-ttu-id="8cf01-176">Két felhasználók Customer1 és Customer2, a helyi csoport tagjai válnak.</span><span class="sxs-lookup"><span data-stu-id="8cf01-176">Two users, Customer1 and Customer2, are made members of this local group.</span></span>

```xml
<Principals>
 <Groups>
   <Group Name="LocalAdminGroup">
     <Membership>
       <SystemGroup Name="Administrators"/>
     </Membership>
   </Group>
 </Groups>
  <Users>
     <User Name="Customer1">
        <MemberOf>
           <Group NameRef="LocalAdminGroup" />
        </MemberOf>
     </User>
    <User Name="Customer2">
      <MemberOf>
        <Group NameRef="LocalAdminGroup" />
      </MemberOf>
    </User>
  </Users>
</Principals>
```

### <a name="create-local-users"></a><span data-ttu-id="8cf01-177">Helyi felhasználók létrehozása</span><span class="sxs-lookup"><span data-stu-id="8cf01-177">Create local users</span></span>
<span data-ttu-id="8cf01-178">Létrehozhat egy helyi felhasználót, amely biztonságos használt toohelp hello alkalmazásban egy szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="8cf01-178">You can create a local user that can be used toohelp secure a service within hello application.</span></span> <span data-ttu-id="8cf01-179">Ha egy **LocalUser** fióktípus szakaszban van megadva hello rendszerbiztonsági tagok hello az alkalmazás jegyzékének, a Service Fabric helyi felhasználói fiókok létrehozása a gépek hello alkalmazás telepítési helyét.</span><span class="sxs-lookup"><span data-stu-id="8cf01-179">When a **LocalUser** account type is specified in hello principals section of hello application manifest, Service Fabric creates local user accounts on machines where hello application is deployed.</span></span> <span data-ttu-id="8cf01-180">Alapértelmezés szerint ezek a fiókok nem rendelkeznek hello (például a következő minta hello Customer3) manifest hello alkalmazás megadottakkal azonos néven.</span><span class="sxs-lookup"><span data-stu-id="8cf01-180">By default, these accounts do not have hello same names as those specified in hello application manifest (for example, Customer3 in hello following sample).</span></span> <span data-ttu-id="8cf01-181">Ehelyett dinamikusan jön létre, és véletlenszerű jelszót.</span><span class="sxs-lookup"><span data-stu-id="8cf01-181">Instead, they are dynamically generated and have random passwords.</span></span>

```xml
<Principals>
  <Users>
     <User Name="Customer3" AccountType="LocalUser" />
  </Users>
</Principals>
```

<span data-ttu-id="8cf01-182">Ha egy alkalmazás megköveteli, hogy hello felhasználói fiókkal és jelszóval azonos minden számítógépen (például tooenable NTLM-hitelesítés), hello fürtjegyzékben NTLMAuthenticationEnabled tootrue be kell állítani.</span><span class="sxs-lookup"><span data-stu-id="8cf01-182">If an application requires that hello user account and password be same on all machines (for example, tooenable NTLM authentication), hello cluster manifest must set NTLMAuthenticationEnabled tootrue.</span></span> <span data-ttu-id="8cf01-183">hello fürtjegyzékben is meg kell adnia egy NTLMAuthenticationPasswordSecret használt toogenerate ugyanazt a jelszót hello összes számítógépével.</span><span class="sxs-lookup"><span data-stu-id="8cf01-183">hello cluster manifest must also specify an NTLMAuthenticationPasswordSecret that is used toogenerate hello same password across all machines.</span></span>

```xml
<Section Name="Hosting">
      <Parameter Name="EndpointProviderEnabled" Value="true"/>
      <Parameter Name="NTLMAuthenticationEnabled" Value="true"/>
      <Parameter Name="NTLMAuthenticationPassworkSecret" Value="******" IsEncrypted="true"/>
 </Section>
```

### <a name="assign-policies-toohello-service-code-packages"></a><span data-ttu-id="8cf01-184">Házirendek toohello szolgáltatáscsomagok kód hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="8cf01-184">Assign policies toohello service code packages</span></span>
<span data-ttu-id="8cf01-185">Hello **RunAsPolicy** szakasz egy **ServiceManifestImport** hello rendszerbiztonsági tagok szakaszában, amelyeket a kódcsomag használt toorun hello fiókot adja meg.</span><span class="sxs-lookup"><span data-stu-id="8cf01-185">hello **RunAsPolicy** section for a **ServiceManifestImport** specifies hello account from hello principals section that should be used toorun a code package.</span></span> <span data-ttu-id="8cf01-186">A felhasználói fiókok hello rendszerbiztonsági tagok szakaszban hello service-csomagok jegyzékfájljában kódot is társítja.</span><span class="sxs-lookup"><span data-stu-id="8cf01-186">It also associates code packages from hello service manifest with user accounts in hello principals section.</span></span> <span data-ttu-id="8cf01-187">Megadhatja a hello beállítása vagy a fő belépési pontok, vagy megadhat `All` tooapply azt tooboth.</span><span class="sxs-lookup"><span data-stu-id="8cf01-187">You can specify this for hello setup or main entry points, or you can specify `All` tooapply it tooboth.</span></span> <span data-ttu-id="8cf01-188">a következő példa azt mutatja be másik-szabályzatok hello:</span><span class="sxs-lookup"><span data-stu-id="8cf01-188">hello following example shows different policies being applied:</span></span>

```xml
<Policies>
<RunAsPolicy CodePackageRef="Code" UserRef="LocalAdmin" EntryPointType="Setup"/>
<RunAsPolicy CodePackageRef="Code" UserRef="Customer3" EntryPointType="Main"/>
</Policies>
```

<span data-ttu-id="8cf01-189">Ha **EntryPointType** nincs megadva, tooEntryPointType hello az alapértelmezett érték = "Main".</span><span class="sxs-lookup"><span data-stu-id="8cf01-189">If **EntryPointType** is not specified, hello default is set tooEntryPointType=”Main”.</span></span> <span data-ttu-id="8cf01-190">Adja meg **SetupEntryPoint** is különösen hasznos, ha azt szeretné, toorun bizonyos magas szintű jogosultságokat igénylő telepítési műveletekhez a rendszerfiók alatt.</span><span class="sxs-lookup"><span data-stu-id="8cf01-190">Specifying **SetupEntryPoint** is especially useful when you want toorun certain high-privilege setup operations under a system account.</span></span> <span data-ttu-id="8cf01-191">hello tényleges szolgáltatás kód egy alacsonyabb szintű szolgáltatásfiók alatt futhatnak.</span><span class="sxs-lookup"><span data-stu-id="8cf01-191">hello actual service code can run under a lower-privilege account.</span></span>

### <a name="apply-a-default-policy-tooall-service-code-packages"></a><span data-ttu-id="8cf01-192">Egy alapértelmezett kód házirend tooall szolgáltatáscsomagok alkalmazása</span><span class="sxs-lookup"><span data-stu-id="8cf01-192">Apply a default policy tooall service code packages</span></span>
<span data-ttu-id="8cf01-193">Hello használata **DefaultRunAsPolicy** szakasz toospecify egy alapértelmezett felhasználói fiókot az összes kódot csomagokat, amelyek nem rendelkeznek egy adott **RunAsPolicy** definiálva.</span><span class="sxs-lookup"><span data-stu-id="8cf01-193">You use hello **DefaultRunAsPolicy** section toospecify a default user account for all code packages that don’t have a specific **RunAsPolicy** defined.</span></span> <span data-ttu-id="8cf01-194">Ha egy alkalmazás által használt hello szolgáltatás jegyzékben megadott hello kód csomagok többsége toorun alatt kell hello ugyanahhoz a felhasználóhoz, hello alkalmazás csak határozhatja meg, hogy felhasználói fiókkal alapértelmezett RunAs házirend.</span><span class="sxs-lookup"><span data-stu-id="8cf01-194">If most of hello code packages that are specified in hello service manifest used by an application need toorun under hello same user, hello application can just define a default RunAs policy with that user account.</span></span> <span data-ttu-id="8cf01-195">hello alábbi példa azt jelenti, hogy ha a kódcsomag nem rendelkezik egy **RunAsPolicy** megadott, hello kódcsomag kell futni hello **MyDefaultAccount** hello rendszerbiztonsági tagok szakaszában megadott.</span><span class="sxs-lookup"><span data-stu-id="8cf01-195">hello following example specifies that if a code package does not have a **RunAsPolicy** specified, hello code package should run under hello **MyDefaultAccount** specified in hello principals section.</span></span>

```xml
<Policies>
  <DefaultRunAsPolicy UserRef="MyDefaultAccount"/>
</Policies>
```
### <a name="use-an-active-directory-domain-group-or-user"></a><span data-ttu-id="8cf01-196">Az Active Directory tartományi csoport vagy felhasználó használata</span><span class="sxs-lookup"><span data-stu-id="8cf01-196">Use an Active Directory domain group or user</span></span>
<span data-ttu-id="8cf01-197">A Service Fabric hello önálló telepítő segítségével a Windows Server telepített példánya hello szolgáltatás hello credentials az Active Directory-felhasználó vagy csoport fiókkal is futtathatja.</span><span class="sxs-lookup"><span data-stu-id="8cf01-197">For an instance of Service Fabric that was installed on Windows Server by using hello standalone installer, you can run hello service under hello credentials for an Active Directory user or group account.</span></span> <span data-ttu-id="8cf01-198">Ez az Active Directory helyszíni saját tartományában, és nincs az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="8cf01-198">This is Active Directory on-premises within your domain and is not with Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="8cf01-199">Tartományi felhasználó vagy csoport segítségével érheti el más erőforrásokat hello tartomány (például fájlmegosztások), amely engedéllyel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="8cf01-199">By using a domain user or group, you can then access other resources in hello domain (for example, file shares) that have been granted permissions.</span></span>

<span data-ttu-id="8cf01-200">hello alábbi példa bemutatja az Active Directory-felhasználó nevű *tesztfelhasználó néven* a tartomány-tanúsítvány használatával titkosítja jelszó az úgynevezett *MyCert*.</span><span class="sxs-lookup"><span data-stu-id="8cf01-200">hello following example shows an Active Directory user called *TestUser* with their domain password encrypted by using a certificate called *MyCert*.</span></span> <span data-ttu-id="8cf01-201">Használhatja a hello `Invoke-ServiceFabricEncryptText` PowerShell toocreate hello titkos titkosítási parancsszöveget.</span><span class="sxs-lookup"><span data-stu-id="8cf01-201">You can use hello `Invoke-ServiceFabricEncryptText` PowerShell command toocreate hello secret cipher text.</span></span> <span data-ttu-id="8cf01-202">Lásd: [Service Fabric-alkalmazások a titkos kulcsok kezelése](service-fabric-application-secret-management.md) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="8cf01-202">See [Managing secrets in Service Fabric applications](service-fabric-application-secret-management.md) for details.</span></span>

<span data-ttu-id="8cf01-203">Titkos kulcs hello hello tanúsítvány toodecrypt hello jelszó toohello helyi számítógép telepítenie kell egy sávon kívüli módszer használatával (Azure-ban ez az Azure Resource Manager használatával).</span><span class="sxs-lookup"><span data-stu-id="8cf01-203">You must deploy hello private key of hello certificate toodecrypt hello password toohello local machine by using an out-of-band method (in Azure, this is via Azure Resource Manager).</span></span> <span data-ttu-id="8cf01-204">Ezután Service Fabric hello szolgáltatás csomag toohello számítógépe telepíti, ha képes toodecrypt hello titkos kulcs, és ezeket a hitelesítő adatokat az Active Directory toorun végzett hitelesítéshez (együtt hello felhasználónév).</span><span class="sxs-lookup"><span data-stu-id="8cf01-204">Then, when Service Fabric deploys hello service package toohello machine, it is able toodecrypt hello secret and (along with hello user name) authenticate with Active Directory toorun under those credentials.</span></span>

```xml
<Principals>
  <Users>
    <User Name="TestUser" AccountType="DomainUser" AccountName="Domain\User" Password="[Put encrypted password here using MyCert certificate]" PasswordEncrypted="true" />
  </Users>
</Principals>
<Policies>
  <DefaultRunAsPolicy UserRef="TestUser" />
  <SecurityAccessPolicies>
    <SecurityAccessPolicy ResourceRef="MyCert" PrincipalRef="TestUser" GrantRights="Full" ResourceType="Certificate" />
  </SecurityAccessPolicies>
</Policies>
<Certificates>
```
### <a name="use-a-group-managed-service-account"></a><span data-ttu-id="8cf01-205">Adjon meg egy csoportot felügyelt szolgáltatásfiók.</span><span class="sxs-lookup"><span data-stu-id="8cf01-205">Use a Group Managed Service Account.</span></span>
<span data-ttu-id="8cf01-206">A Service Fabric hello önálló telepítő segítségével a Windows Server telepített példánya hello szolgáltatást is futhat, a csoportosan felügyelt szolgáltatásfiókok (gMSA).</span><span class="sxs-lookup"><span data-stu-id="8cf01-206">For an instance of Service Fabric that was installed on Windows Server by using hello standalone installer, you can run hello service as a group Managed Service Account (gMSA).</span></span> <span data-ttu-id="8cf01-207">Vegye figyelembe, hogy ez az Active Directory a helyszíni tartományán belüli és nem az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="8cf01-207">Note that this is Active Directory on-premises within your domain and is not with Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="8cf01-208">A csoportosan felügyelt szolgáltatásfiók nincs jelszó vagy hello tárolt titkosított jelszót `Application Manifest`.</span><span class="sxs-lookup"><span data-stu-id="8cf01-208">By using a gMSA there is no password or encrypted password stored in hello `Application Manifest`.</span></span>

<span data-ttu-id="8cf01-209">hello következő példa bemutatja, hogyan toocreate csoportosan felügyelt szolgáltatásfiókok nevű *svc-teszt$*; hogyan toodeploy, amely felügyeli szolgáltatás fiók toohello fürtcsomópontok; és hogyan tooconfigure hello egyszerű.</span><span class="sxs-lookup"><span data-stu-id="8cf01-209">hello following example shows how toocreate a gMSA account called *svc-Test$*; how toodeploy that managed service account toohello cluster nodes; and how tooconfigure hello user principal.</span></span>

##### <a name="prerequisites"></a><span data-ttu-id="8cf01-210">Előfeltételek.</span><span class="sxs-lookup"><span data-stu-id="8cf01-210">Prerequisites.</span></span>
- <span data-ttu-id="8cf01-211">hello tartomány kell KDS-gyökérkulcsot.</span><span class="sxs-lookup"><span data-stu-id="8cf01-211">hello domain needs a KDS root key.</span></span>
- <span data-ttu-id="8cf01-212">hello tartomány toobe egy Windows Server 2012 vagy újabb működési szint szükséges.</span><span class="sxs-lookup"><span data-stu-id="8cf01-212">hello domain needs toobe at a Windows Server 2012 or later functional level.</span></span>

##### <a name="example"></a><span data-ttu-id="8cf01-213">Példa</span><span class="sxs-lookup"><span data-stu-id="8cf01-213">Example</span></span>
1. <span data-ttu-id="8cf01-214">Kérjen meg egy active directory tartományi rendszergazdát hello segítségével csoportosan felügyelt szolgáltatásfiók létrehozása `New-ADServiceAccount` parancsmagot, és győződjön meg arról, hogy hello `PrincipalsAllowedToRetrieveManagedPassword` tartalmazza az összes hello service fabric-csomópont.</span><span class="sxs-lookup"><span data-stu-id="8cf01-214">Have an active directory domain administrator create a group managed service account using hello `New-ADServiceAccount` commandlet and ensure that hello `PrincipalsAllowedToRetrieveManagedPassword` includes all of hello service fabric cluster nodes.</span></span> <span data-ttu-id="8cf01-215">Vegye figyelembe, hogy `AccountName`, `DnsHostName`, és `ServicePrincipalName` egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="8cf01-215">Note that `AccountName`, `DnsHostName`, and `ServicePrincipalName` must be unique.</span></span>
```
New-ADServiceAccount -name svc-Test$ -DnsHostName svc-test.contoso.com  -ServicePrincipalNames http/svc-test.contoso.com -PrincipalsAllowedToRetrieveManagedPassword SfNode0$,SfNode1$,SfNode2$,SfNode3$,SfNode4$
```
2. <span data-ttu-id="8cf01-216">Minden egyes hello service fabric-csomópont (például `SfNode0$,SfNode1$,SfNode2$,SfNode3$,SfNode4$`), telepítése és tesztelése hello csoportosan felügyelt szolgáltatásfiókot.</span><span class="sxs-lookup"><span data-stu-id="8cf01-216">On each of hello service fabric cluster nodes (for example, `SfNode0$,SfNode1$,SfNode2$,SfNode3$,SfNode4$`), install and test hello gMSA.</span></span>
```
Add-WindowsFeature RSAT-AD-PowerShell
Install-AdServiceAccount svc-Test$
Test-AdServiceAccount svc-Test$
```
3. <span data-ttu-id="8cf01-217">Hello egyszerű, valamint hello RunAsPolicy tooreference hello felhasználói konfigurálni.</span><span class="sxs-lookup"><span data-stu-id="8cf01-217">Configure hello User principal, and configure hello RunAsPolicy tooreference hello user.</span></span>
```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyServiceTypePkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="DomaingMSA"/>
      </Policies>
   </ServiceManifestImport>
  <Principals>
    <Users>
      <User Name="DomaingMSA" AccountType="ManagedServiceAccount" AccountName="domain\svc-Test$"/>
    </Users>
  </Principals>
</ApplicationManifest>
```

## <a name="assign-a-security-access-policy-for-http-and-https-endpoints"></a><span data-ttu-id="8cf01-218">Rendelje hozzá a biztonsági házirend HTTP és HTTPS-végpontok</span><span class="sxs-lookup"><span data-stu-id="8cf01-218">Assign a security access policy for HTTP and HTTPS endpoints</span></span>
<span data-ttu-id="8cf01-219">Ha csoportházirend RunAs tooa szolgáltatás telepítését, és hello szolgáltatás jegyzékfájl deklarál végpont erőforrások hello HTTP protokollal, meg kell adnia egy **SecurityAccessPolicy** , hogy a portok toothese végpontok lefoglalt tooensure megfelelően vannak hozzáférés-vezérlési felsorolt hello futtató felhasználói fiók, amely hello szolgáltatás fut a.</span><span class="sxs-lookup"><span data-stu-id="8cf01-219">If you apply a RunAs policy tooa service and hello service manifest declares endpoint resources with hello HTTP protocol, you must specify a **SecurityAccessPolicy** tooensure that ports allocated toothese endpoints are correctly access-control listed for hello RunAs user account that hello service runs under.</span></span> <span data-ttu-id="8cf01-220">Ellenkező esetben **http.sys** toohello szolgáltatás nem rendelkezik, és sikertelen hívások lehívása hello ügyfél.</span><span class="sxs-lookup"><span data-stu-id="8cf01-220">Otherwise, **http.sys** does not have access toohello service, and you get failures with calls from hello client.</span></span> <span data-ttu-id="8cf01-221">hello alábbi példa érvényes hello Customer1 fiók tooan végpont nevű **EndpointName**, amely azt teszi teljes körű hozzáférési jogosultságokat.</span><span class="sxs-lookup"><span data-stu-id="8cf01-221">hello following example applies hello Customer1 account tooan endpoint called **EndpointName**, which gives it full access rights.</span></span>

```xml
<Policies>
   <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
   <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and hello Endpoint is http -->
   <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
</Policies>
```

<span data-ttu-id="8cf01-222">A HTTPS-végpont hello is hello tanúsítvány tooreturn toohello ügyfél tooindicate hello neve.</span><span class="sxs-lookup"><span data-stu-id="8cf01-222">For hello HTTPS endpoint, you also have tooindicate hello name of hello certificate tooreturn toohello client.</span></span> <span data-ttu-id="8cf01-223">Ehhez a **EndpointBindingPolicy**, egy tanúsítvány szakaszban hello alkalmazásjegyzékben hello tanúsítvánnyal.</span><span class="sxs-lookup"><span data-stu-id="8cf01-223">You can do this by using **EndpointBindingPolicy**, with hello certificate defined in a certificates section in hello application manifest.</span></span>

```xml
<Policies>
   <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
  <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and hello Endpoint is http -->
   <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
  <!--EndpointBindingPolicy is needed if hello EndpointName is secured with https -->
  <EndpointBindingPolicy EndpointRef="EndpointName" CertificateRef="Cert1" />
</Policies
```
## <a name="upgrading-multiple-applications-with-https-endpoints"></a><span data-ttu-id="8cf01-224">A https-végpontnak több alkalmazás frissítése</span><span class="sxs-lookup"><span data-stu-id="8cf01-224">Upgrading multiple applications with https endpoints</span></span>
<span data-ttu-id="8cf01-225">Gondosan kell toobe nem toouse hello **ugyanazt a portot** hello különböző példányai ugyanahhoz az alkalmazáshoz http használata esetén**s**.</span><span class="sxs-lookup"><span data-stu-id="8cf01-225">You need toobe careful not toouse hello **same port** for different instances of hello same application when using http**s**.</span></span> <span data-ttu-id="8cf01-226">hello oka, hogy a Service Fabric nem lesz képes tooupgrade hello cert egy hello alkalmazáspéldányok.</span><span class="sxs-lookup"><span data-stu-id="8cf01-226">hello reason is that Service Fabric won't be able tooupgrade hello cert for one of hello application instances.</span></span> <span data-ttu-id="8cf01-227">Ha például 1 vagy alkalmazás 2 mindkét alkalmazás szeretné tooupgrade az 1. tanúsítvány toocert 2.</span><span class="sxs-lookup"><span data-stu-id="8cf01-227">For example, if application 1 or application 2 both want tooupgrade their cert 1 toocert 2.</span></span> <span data-ttu-id="8cf01-228">Hello frissítés akkor fordul elő, amikor a Service Fabric előfordulhat, hogy rendelkezik tisztítani hello 1. tanúsítvány regisztrálása a következőben: http.sys annak ellenére, hogy hello többi alkalmazás továbbra is használja.</span><span class="sxs-lookup"><span data-stu-id="8cf01-228">When hello upgrade happens, Service Fabric might have cleaned up hello cert 1 registration with http.sys even though hello other application is still using it.</span></span> <span data-ttu-id="8cf01-229">tooprevent, a Service Fabric észleli, hogy nincs-e már egy másik alkalmazáspéldány hello port hello tanúsítvánnyal regisztrált (esedékes toohttp.sys) és a sikertelen hello műveletet.</span><span class="sxs-lookup"><span data-stu-id="8cf01-229">tooprevent this, Service Fabric detects that there is already another application instance registered on hello port with hello certificate (due toohttp.sys) and fails hello operation.</span></span>

<span data-ttu-id="8cf01-230">Ezért a Service Fabric nem támogatja a frissítést két különböző szolgáltatásokat **ugyanazt a portot hello** másik alkalmazási esetekben.</span><span class="sxs-lookup"><span data-stu-id="8cf01-230">Hence Service Fabric does not support upgrading two different services using **hello same port** in different application instances.</span></span> <span data-ttu-id="8cf01-231">Ez azt jelenti, nem használható ugyanaz a tanúsítvány hello hello különböző szolgáltatások ugyanazt a portot.</span><span class="sxs-lookup"><span data-stu-id="8cf01-231">In other words, you cannot use hello same certificate on different services on hello same port.</span></span> <span data-ttu-id="8cf01-232">Ha toohave van szüksége egy megosztott tanúsítványt futó hello azonos porton, tooensure adott szolgáltatások kerülnek, a különböző gépeken elhelyezési-korlátozásokkal rendelkező hello van szüksége.</span><span class="sxs-lookup"><span data-stu-id="8cf01-232">If you need toohave a shared certificate on hello same port, you need tooensure that hello services are placed on different machines with placement constraints.</span></span> <span data-ttu-id="8cf01-233">Vagy fontolja meg a Service Fabric dinamikus portok lehetőség szerint minden egyes szolgáltatás minden egyes alkalmazás-példányban.</span><span class="sxs-lookup"><span data-stu-id="8cf01-233">Or consider using Service Fabric dynamic ports if possible for each service in each application instance.</span></span> 

<span data-ttu-id="8cf01-234">Ha egy frissítés sikertelen, a https, megjelenik egy hibaüzenet figyelmeztetést, amely szerint a "hello Windows HTTP-kiszolgáló API nem támogatja több tanúsítvány egy portot használó alkalmazásokhoz."</span><span class="sxs-lookup"><span data-stu-id="8cf01-234">If you see an upgrade fail with https, an error warning saying "hello Windows HTTP Server API does not support multiple certificates for applications that share a port.”</span></span>

## <a name="a-complete-application-manifest-example"></a><span data-ttu-id="8cf01-235">A teljes alkalmazás jegyzékének példa</span><span class="sxs-lookup"><span data-stu-id="8cf01-235">A complete application manifest example</span></span>
<span data-ttu-id="8cf01-236">a következő alkalmazás jegyzékfájlja hello látható hello számos különböző beállításokat:</span><span class="sxs-lookup"><span data-stu-id="8cf01-236">hello following application manifest shows many of hello different settings:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="Application3Type" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Parameters>
      <Parameter Name="Stateless1_InstanceCount" DefaultValue="-1" />
   </Parameters>
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="Stateless1Pkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
         <RunAsPolicy CodePackageRef="Code" UserRef="LocalAdmin" EntryPointType="Setup" />
        <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and hello Endpoint is http -->
         <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
        <!--EndpointBindingPolicy is needed hello EndpointName is secured with https -->
        <EndpointBindingPolicy EndpointRef="EndpointName" CertificateRef="Cert1" />
     </Policies>
   </ServiceManifestImport>
   <DefaultServices>
      <Service Name="Stateless1">
         <StatelessService ServiceTypeName="Stateless1Type" InstanceCount="[Stateless1_InstanceCount]">
            <SingletonPartition />
         </StatelessService>
      </Service>
   </DefaultServices>
   <Principals>
      <Groups>
         <Group Name="LocalAdminGroup">
            <Membership>
               <SystemGroup Name="Administrators" />
            </Membership>
         </Group>
      </Groups>
      <Users>
         <User Name="LocalAdmin">
            <MemberOf>
               <Group NameRef="LocalAdminGroup" />
            </MemberOf>
         </User>
         <!--Customer1 below create a local account that this service runs under -->
         <User Name="Customer1" />
         <User Name="MyDefaultAccount" AccountType="NetworkService" />
      </Users>
   </Principals>
   <Policies>
      <DefaultRunAsPolicy UserRef="LocalAdmin" />
   </Policies>
   <Certificates>
     <EndpointCertificate Name="Cert1" X509FindValue="FF EE E0 TT JJ DD JJ EE EE XX 23 4T 66 "/>
  </Certificates>
</ApplicationManifest>
```


<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a><span data-ttu-id="8cf01-237">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8cf01-237">Next steps</span></span>
* [<span data-ttu-id="8cf01-238">Hello alkalmazásmodell ismertetése</span><span class="sxs-lookup"><span data-stu-id="8cf01-238">Understand hello application model</span></span>](service-fabric-application-model.md)
* [<span data-ttu-id="8cf01-239">Adja meg a szolgáltatás jegyzék erőforrások</span><span class="sxs-lookup"><span data-stu-id="8cf01-239">Specify resources in a service manifest</span></span>](service-fabric-service-manifest-resources.md)
* [<span data-ttu-id="8cf01-240">Alkalmazás üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="8cf01-240">Deploy an application</span></span>](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png
