---
title: "További tudnivalók az Azure mikroszolgáltatások biztonsági házirendek |} Microsoft Docs"
description: "A rendszer és a helyi biztonsági fiókok, beleértve a SetupEntry pont, amelyen egy alkalmazást kell valamilyen privilegizált művelet végrehajtása előtt a Service Fabric-alkalmazás futtatása áttekintése"
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
ms.openlocfilehash: e673b45a43a06d18040c3437caf8765704d5c36a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="configure-security-policies-for-your-application"></a><span data-ttu-id="b2a66-103">Biztonsági házirendek beállítása az alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="b2a66-103">Configure security policies for your application</span></span>
<span data-ttu-id="b2a66-104">Azure Service Fabric használatával biztonságossá teheti a különböző felhasználói fiókok a fürtben futó alkalmazás számára.</span><span class="sxs-lookup"><span data-stu-id="b2a66-104">By using Azure Service Fabric, you can secure applications that are running in the cluster under different user accounts.</span></span> <span data-ttu-id="b2a66-105">A Service Fabric is lehetővé teszi az alkalmazások által használt felhasználói fiók – központi telepítés alkalmával például erőforrásokat, a fájlokat, a könyvtárak és a tanúsítványok biztonságos.</span><span class="sxs-lookup"><span data-stu-id="b2a66-105">Service Fabric also helps secure the resources that are used by applications at the time of deployment under the user accounts--for example, files, directories, and certificates.</span></span> <span data-ttu-id="b2a66-106">Így futó alkalmazást, még akkor is, a megosztott üzemeltetési környezetben, nagyobb biztonságot nyújt, egymástól.</span><span class="sxs-lookup"><span data-stu-id="b2a66-106">This makes running applications, even in a shared hosted environment, more secure from one another.</span></span>

<span data-ttu-id="b2a66-107">Alapértelmezés szerint a Service Fabric alkalmazások futnak, a fiók, amely alatt futó a Fabric.exe folyamatban.</span><span class="sxs-lookup"><span data-stu-id="b2a66-107">By default, Service Fabric applications run under the account that the Fabric.exe process runs under.</span></span> <span data-ttu-id="b2a66-108">A Service Fabric is lehetővé teszi a helyi felhasználói fiók vagy helyi rendszerfiók, az alkalmazás jegyzékében belül megadott alkalmazások futtatásához.</span><span class="sxs-lookup"><span data-stu-id="b2a66-108">Service Fabric also provides the capability to run applications under a local user account or local system account, which is specified within the application manifest.</span></span> <span data-ttu-id="b2a66-109">Támogatott helyi rendszer fiók típusok **LocalUser**, **NetworkService**, **LocalService**, és **LocalSystem**.</span><span class="sxs-lookup"><span data-stu-id="b2a66-109">Supported local system account types are **LocalUser**, **NetworkService**, **LocalService**, and **LocalSystem**.</span></span>

 <span data-ttu-id="b2a66-110">Ha a számítógépén a Service Fabric Windows Server az adatközpontban található az önálló telepítő használatával, az Active Directory tartományi fiókok, beleértve a csoportosan felügyelt szolgáltatásfiókok is használhatja.</span><span class="sxs-lookup"><span data-stu-id="b2a66-110">When you're running Service Fabric on Windows Server in your datacenter by using the standalone installer, you can use Active Directory domain accounts, including group managed service accounts.</span></span>

<span data-ttu-id="b2a66-111">Hozzon létre felhasználói csoportokat, hogy egy vagy több felhasználó is adható hozzá mindegyik csoport felügyelete együtt, és megadása.</span><span class="sxs-lookup"><span data-stu-id="b2a66-111">You can define and create user groups so that one or more users can be added to each group to be managed together.</span></span> <span data-ttu-id="b2a66-112">Ez akkor hasznos, ha több felhasználó különböző belépési pontot, és bizonyos közös jogosultságok, amelyek elérhetők a csoportok szintjén van szükségük.</span><span class="sxs-lookup"><span data-stu-id="b2a66-112">This is useful when there are multiple users for different service entry points and they need to have certain common privileges that are available at the group level.</span></span>

## <a name="configure-the-policy-for-a-service-setup-entry-point"></a><span data-ttu-id="b2a66-113">A telepítő belépési pont vonatkozó házirend konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b2a66-113">Configure the policy for a service setup entry point</span></span>
<span data-ttu-id="b2a66-114">Lásd: a [alkalmazásmodell](service-fabric-application-model.md), a telepítő belépési pont **SetupEntryPoint**, egy kiemelt belépési pontja, amelynek ugyanazokat a hitelesítő adatokat, mint a Service Fabric (általában a  *NetworkService* fiók) más belépési pont előtt.</span><span class="sxs-lookup"><span data-stu-id="b2a66-114">As described in the [application model](service-fabric-application-model.md), the setup entry point, **SetupEntryPoint**, is a privileged entry point that runs with the same credentials as Service Fabric (typically the *NetworkService* account) before any other entry point.</span></span> <span data-ttu-id="b2a66-115">A megadott végrehajtható fájl **EntryPoint** általában a hosszan futó szolgáltatás gazdagép legyen.</span><span class="sxs-lookup"><span data-stu-id="b2a66-115">The executable that is specified by **EntryPoint** is typically the long-running service host.</span></span> <span data-ttu-id="b2a66-116">Hogy egy külön telepítő belépési pont elkerülhető, hogy jogosultságokkal történő futtatása az adatszolgáltatás gazdájának végrehajtható magas huzamosabb ideig.</span><span class="sxs-lookup"><span data-stu-id="b2a66-116">So having a separate setup entry point avoids having to run the service host executable with high privileges for extended periods of time.</span></span> <span data-ttu-id="b2a66-117">A végrehajtható fájl, amely **EntryPoint** megadja futtatása **SetupEntryPoint** sikeresen kilép.</span><span class="sxs-lookup"><span data-stu-id="b2a66-117">The executable that **EntryPoint** specifies is run after **SetupEntryPoint** exits successfully.</span></span> <span data-ttu-id="b2a66-118">Az eredményül kapott folyamat figyeli, és újra kell indítani, és újra kezdődik **SetupEntryPoint** Ha valaha is leáll, vagy összeomlik.</span><span class="sxs-lookup"><span data-stu-id="b2a66-118">The resulting process is monitored and restarted, and begins again with **SetupEntryPoint** if it ever terminates or crashes.</span></span>

<span data-ttu-id="b2a66-119">Egy egyszerű service manifest példa bemutatja a SetupEntryPoint és a fő belépési pont a szolgáltatás a következő:</span><span class="sxs-lookup"><span data-stu-id="b2a66-119">The following is a simple service manifest example that shows the SetupEntryPoint and the main EntryPoint for the service.</span></span>

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

### <a name="configure-the-policy-by-using-a-local-account"></a><span data-ttu-id="b2a66-120">Konfigurálja a házirendet egy helyi fiók használatával</span><span class="sxs-lookup"><span data-stu-id="b2a66-120">Configure the policy by using a local account</span></span>
<span data-ttu-id="b2a66-121">Miután konfigurálta a szolgáltatást, hogy a telepítő belépési pont van, módosíthatja a biztonsági engedélyek, amelyek az alkalmazás jegyzékében alatt fusson.</span><span class="sxs-lookup"><span data-stu-id="b2a66-121">After you configure the service to have a setup entry point, you can change the security permissions that it runs under in the application manifest.</span></span> <span data-ttu-id="b2a66-122">A következő példa bemutatja, hogyan konfigurálhatja a szolgáltatás felhasználói rendszergazdai jogokkal futtatásához.</span><span class="sxs-lookup"><span data-stu-id="b2a66-122">The following example shows how to configure the service to run under user administrator account privileges.</span></span>

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

<span data-ttu-id="b2a66-123">Először hozzon létre egy **rendszerbiztonsági tagok** egy felhasználónevet, pl. SetupAdminUser szakasz ismerteti.</span><span class="sxs-lookup"><span data-stu-id="b2a66-123">First, create a **Principals** section with a user name, such as SetupAdminUser.</span></span> <span data-ttu-id="b2a66-124">Ez azt jelzi, hogy a felhasználó tagja a rendszergazdák rendszer csoport.</span><span class="sxs-lookup"><span data-stu-id="b2a66-124">This indicates that the user is a member of the Administrators system group.</span></span>

<span data-ttu-id="b2a66-125">Ezután bontsa a **ServiceManifestImport** területen házirend alkalmazása a rendszerbiztonsági konfigurálása **SetupEntryPoint**.</span><span class="sxs-lookup"><span data-stu-id="b2a66-125">Next, under the **ServiceManifestImport** section, configure a policy to apply this principal to **SetupEntryPoint**.</span></span> <span data-ttu-id="b2a66-126">Ez alapján a Service Fabric, hogy ha a **MySetup.bat** futtatása, kell lennie `RunAs` rendszergazdai jogosultságokkal.</span><span class="sxs-lookup"><span data-stu-id="b2a66-126">This tells Service Fabric that when the **MySetup.bat** file is run, it should be `RunAs` with administrator privileges.</span></span> <span data-ttu-id="b2a66-127">Fényében, hogy már rendelkezik *nem* házirend alkalmazása a fő belépési ponthoz, a kód **MyServiceHost.exe** fut a rendszer a **NetworkService** fiók.</span><span class="sxs-lookup"><span data-stu-id="b2a66-127">Given that you have *not* applied a policy to the main entry point, the code in **MyServiceHost.exe** runs under the system **NetworkService** account.</span></span> <span data-ttu-id="b2a66-128">Ez az az alapértelmezett fiókot, amelyet a szolgáltatás összes belépési pont alatt futnak.</span><span class="sxs-lookup"><span data-stu-id="b2a66-128">This is the default account that all service entry points are run as.</span></span>

<span data-ttu-id="b2a66-129">Most adjunk MySetup.bat fájl tesztelése a rendszergazdai jogosultságokat a Visual Studio-projektet.</span><span class="sxs-lookup"><span data-stu-id="b2a66-129">Let's now add the file MySetup.bat to the Visual Studio project to test the administrator privileges.</span></span> <span data-ttu-id="b2a66-130">A Visual Studióban kattintson a jobb gombbal a projekt, és adjon hozzá MySetup.bat nevű új fájlt.</span><span class="sxs-lookup"><span data-stu-id="b2a66-130">In Visual Studio, right-click the service project and add a new file called MySetup.bat.</span></span>

<span data-ttu-id="b2a66-131">Ezt követően győződjön meg arról, hogy a MySetup.bat fájl megtalálható-e a service-csomag.</span><span class="sxs-lookup"><span data-stu-id="b2a66-131">Next, ensure that the MySetup.bat file is included in the service package.</span></span> <span data-ttu-id="b2a66-132">Alapértelmezés szerint nincs.</span><span class="sxs-lookup"><span data-stu-id="b2a66-132">By default, it is not.</span></span> <span data-ttu-id="b2a66-133">Válassza ki a fájlt, kattintson a jobb gombbal a helyi menü segítségével, és válassza a **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="b2a66-133">Select the file, right-click to get the context menu, and choose **Properties**.</span></span> <span data-ttu-id="b2a66-134">A Tulajdonságok párbeszédpanelen győződjön meg arról, hogy **másolása a kimeneti könyvtárba** értéke **másolhatja, ha újabb**.</span><span class="sxs-lookup"><span data-stu-id="b2a66-134">In the Properties dialog box, ensure that **Copy to Output Directory** is set to **Copy if newer**.</span></span> <span data-ttu-id="b2a66-135">Tekintse meg a következő képernyőképet.</span><span class="sxs-lookup"><span data-stu-id="b2a66-135">See the following screenshot.</span></span>

![A Visual Studio CopyToOutput SetupEntryPoint kötegelt fájl][image1]

<span data-ttu-id="b2a66-137">Most nyissa meg a MySetup.bat fájlt, és adja hozzá a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="b2a66-137">Now open the MySetup.bat file and add the following commands:</span></span>

```
REM Set a system environment variable. This requires administrator privilege
setx -m TestVariable "MyValue"
echo System TestVariable set to > out.txt
echo %TestVariable% >> out.txt

REM To delete this system variable us
REM REG delete "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Environment" /v TestVariable /f
```

<span data-ttu-id="b2a66-138">Ezt követően hozza létre, és a megoldás telepítése egy helyi fejlesztési fürtöt.</span><span class="sxs-lookup"><span data-stu-id="b2a66-138">Next, build and deploy the solution to a local development cluster.</span></span> <span data-ttu-id="b2a66-139">A szolgáltatás elindult, a Service Fabric Explorerben látható, miután láthatja, hogy a két módon sikeres volt-e a MySetup.bat fájl.</span><span class="sxs-lookup"><span data-stu-id="b2a66-139">After the service has started, as shown in Service Fabric Explorer, you can see that the MySetup.bat file was successful in a two ways.</span></span> <span data-ttu-id="b2a66-140">Nyisson meg egy PowerShell-parancssort, és írja be:</span><span class="sxs-lookup"><span data-stu-id="b2a66-140">Open a PowerShell command prompt and type:</span></span>

```
PS C:\ [Environment]::GetEnvironmentVariable("TestVariable","Machine")
MyValue
```

<span data-ttu-id="b2a66-141">Jegyezze fel a csomópont, ahol a szolgáltatás telepítve lett, és elindítani a Service Fabric Explorerben – például csomópont 2 neve.</span><span class="sxs-lookup"><span data-stu-id="b2a66-141">Then, note the name of the node where the service was deployed and started in Service Fabric Explorer--for example, Node 2.</span></span> <span data-ttu-id="b2a66-142">Ezután keresse meg az alkalmazás példány munkahelyi mappát értékét bemutató out.txt fájlt **TestVariable**.</span><span class="sxs-lookup"><span data-stu-id="b2a66-142">Next, navigate to the application instance work folder to find the out.txt file that shows the value of **TestVariable**.</span></span> <span data-ttu-id="b2a66-143">Például, ha a szolgáltatás telepítve van a csomópont 2, majd lépjen az elérési útját a **MyApplicationType**:</span><span class="sxs-lookup"><span data-stu-id="b2a66-143">For example, if this service was deployed to Node 2, then you can go to this path for the **MyApplicationType**:</span></span>

```
C:\SfDevCluster\Data\_App\Node.2\MyApplicationType_App\work\out.txt
```

### <a name="configure-the-policy-by-using-local-system-accounts"></a><span data-ttu-id="b2a66-144">A házirend konfigurálása a helyi rendszer fiók használatával</span><span class="sxs-lookup"><span data-stu-id="b2a66-144">Configure the policy by using local system accounts</span></span>
<span data-ttu-id="b2a66-145">Gyakran célszerű a indítási parancsfájl futtatásához rendszergazdai fiók helyett a helyi rendszer fiók használatával.</span><span class="sxs-lookup"><span data-stu-id="b2a66-145">Often, it's preferable to run the startup script by using a local system account rather than an administrator account.</span></span> <span data-ttu-id="b2a66-146">A RunAs házirend fut, a Rendszergazdák csoport tagjaként általában nem működik jól, mert a felhasználói hozzáférés felügyelete (UAC) alapértelmezés szerint engedélyezve vannak.</span><span class="sxs-lookup"><span data-stu-id="b2a66-146">Running the RunAs policy as a member of the Administrators group typically doesn’t work well because machines have User Access Control (UAC) enabled by default.</span></span> <span data-ttu-id="b2a66-147">Ilyen esetben **felhasználóként történő futása a SetupEntryPoint LocalSystem, ahelyett, hogy a helyi Rendszergazdák csoportba felvett a javaslat**.</span><span class="sxs-lookup"><span data-stu-id="b2a66-147">In such cases, **the recommendation is to run the SetupEntryPoint as LocalSystem, instead of as a local user added to Administrators group**.</span></span> <span data-ttu-id="b2a66-148">A következő példa bemutatja a LocalSystem fiókként való futtatásra SetupEntryPoint beállítása:</span><span class="sxs-lookup"><span data-stu-id="b2a66-148">The following example shows setting the SetupEntryPoint to run as LocalSystem:</span></span>

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

## <a name="start-powershell-commands-from-a-setup-entry-point"></a><span data-ttu-id="b2a66-149">A telepítő belépési pontról indítsa el a PowerShell-parancsok</span><span class="sxs-lookup"><span data-stu-id="b2a66-149">Start PowerShell commands from a setup entry point</span></span>
<span data-ttu-id="b2a66-150">A PowerShell futtatásához a **SetupEntryPoint** pont, futtathatja **PowerShell.exe** parancsfájlban, amely egy PowerShell-fájlra mutat.</span><span class="sxs-lookup"><span data-stu-id="b2a66-150">To run PowerShell from the **SetupEntryPoint** point, you can run **PowerShell.exe** in a batch file that points to a PowerShell file.</span></span> <span data-ttu-id="b2a66-151">Először adjon hozzá egy PowerShell fájlt például a projekt-- **MySetup.ps1**.</span><span class="sxs-lookup"><span data-stu-id="b2a66-151">First, add a PowerShell file to the service project--for example, **MySetup.ps1**.</span></span> <span data-ttu-id="b2a66-152">Ne felejtse el, állítsa be a *másolhatja, ha újabb* tulajdonságot, hogy a fájl a szolgáltatáscsomagot is megtalálhatók.</span><span class="sxs-lookup"><span data-stu-id="b2a66-152">Remember to set the *Copy if newer* property so that the file is also included in the service package.</span></span> <span data-ttu-id="b2a66-153">A következő példa bemutatja egy minta-kötegfájl MySetup.ps1, amelyek a nevezett rendszer környezeti változó beállítása nevű PowerShell fájl elinduló **TestVariable**.</span><span class="sxs-lookup"><span data-stu-id="b2a66-153">The following example shows a sample batch file that starts a PowerShell file called MySetup.ps1, which sets a system environment variable called **TestVariable**.</span></span>

<span data-ttu-id="b2a66-154">A PowerShell fájl indítása MySetup.bat:</span><span class="sxs-lookup"><span data-stu-id="b2a66-154">MySetup.bat to start a PowerShell file:</span></span>

```
powershell.exe -ExecutionPolicy Bypass -Command ".\MySetup.ps1"
```

<span data-ttu-id="b2a66-155">A PowerShell fájlban adja hozzá a következő, a rendszer környezeti változó beállítása:</span><span class="sxs-lookup"><span data-stu-id="b2a66-155">In the PowerShell file, add the following to set a system environment variable:</span></span>

```
[Environment]::SetEnvironmentVariable("TestVariable", "MyValue", "Machine")
[Environment]::GetEnvironmentVariable("TestVariable","Machine") > out.txt
```

> [!NOTE]
> <span data-ttu-id="b2a66-156">Alapértelmezés szerint a parancsfájl futtatása azt ellenőrzi, hogy az alkalmazás mappájában nevű **működik** fájlok.</span><span class="sxs-lookup"><span data-stu-id="b2a66-156">By default, when the batch file runs, it looks at the application folder called **work** for files.</span></span> <span data-ttu-id="b2a66-157">Ebben az esetben, amikor MySetup.bat fut, történjen a MySetup.ps1 fájl található abban a mappában, amely az alkalmazás **kódcsomag** mappa.</span><span class="sxs-lookup"><span data-stu-id="b2a66-157">In this case, when MySetup.bat runs, we want this to find the MySetup.ps1 file in the same folder, which is the application **code package** folder.</span></span> <span data-ttu-id="b2a66-158">Ez a mappa módosításához állítsa a munkamappa:</span><span class="sxs-lookup"><span data-stu-id="b2a66-158">To change this folder, set the working folder:</span></span>
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

## <a name="use-console-redirection-for-local-debugging"></a><span data-ttu-id="b2a66-159">A hibakereséshez a helyi konzol átirányítást használ</span><span class="sxs-lookup"><span data-stu-id="b2a66-159">Use console redirection for local debugging</span></span>
<span data-ttu-id="b2a66-160">Bizonyos esetekben célszerű tekintse meg a konzol kimeneti hibakeresési célra parancsfájl futtatását.</span><span class="sxs-lookup"><span data-stu-id="b2a66-160">Occasionally, it's useful to see the console output from running a script for debugging purposes.</span></span> <span data-ttu-id="b2a66-161">Ehhez az szükséges, beállíthat egy konzol átirányítási házirendet, amely a kimeneti fájlba írja.</span><span class="sxs-lookup"><span data-stu-id="b2a66-161">To do this, you can set a console redirection policy, which writes the output to a file.</span></span> <span data-ttu-id="b2a66-162">A kimeneti fájl beíródik nevű alkalmazás mappát **napló** a csomóponton, ha az alkalmazás üzemel, és futtassa.</span><span class="sxs-lookup"><span data-stu-id="b2a66-162">The file output is written to the application folder called **log** on the node where the application is deployed and run.</span></span> <span data-ttu-id="b2a66-163">(Lásd az előző példában megkeresése a.)</span><span class="sxs-lookup"><span data-stu-id="b2a66-163">(See where to find this in the preceding example.)</span></span>

> [!WARNING]
> <span data-ttu-id="b2a66-164">Soha ne használja a konzol átirányítási házirend a kérelmet, mert ez hatással lehet az alkalmazás feladatátvételi éles környezetben telepített.</span><span class="sxs-lookup"><span data-stu-id="b2a66-164">Never use the console redirection policy in an application that is deployed in production because this can affect the application failover.</span></span> <span data-ttu-id="b2a66-165">*Csak* használja ezt a helyi fejlesztési és hibakeresési célra.</span><span class="sxs-lookup"><span data-stu-id="b2a66-165">*Only* use this for local development and debugging purposes.</span></span>  
> 
> 

<span data-ttu-id="b2a66-166">A következő példa bemutatja, hogy a konzol átirányítása FileRetentionCount érték beállítása:</span><span class="sxs-lookup"><span data-stu-id="b2a66-166">The following example shows setting the console redirection with a FileRetentionCount value:</span></span>

```xml
<SetupEntryPoint>
    <ExeHost>
    <Program>MySetup.bat</Program>
    <WorkingFolder>CodePackage</WorkingFolder>
    <ConsoleRedirection FileRetentionCount="10"/>
    </ExeHost>
</SetupEntryPoint>
```

<span data-ttu-id="b2a66-167">Ha most módosítja a MySetup.ps1 fájl, amelybe írni egy **Echo** parancsban, ez fogja írni a kimeneti fájl hibakeresési célra:</span><span class="sxs-lookup"><span data-stu-id="b2a66-167">If you now change the MySetup.ps1 file to write an **Echo** command, this will write to the output file for debugging purposes:</span></span>

```
Echo "Test console redirection which writes to the application log folder on the node that the application is deployed to"
```

<span data-ttu-id="b2a66-168">**A parancsfájl hibakeresése azonnal távolítsa el a konzol átirányítási házirend**.</span><span class="sxs-lookup"><span data-stu-id="b2a66-168">**After you debug your script, immediately remove this console redirection policy**.</span></span>

## <a name="configure-a-policy-for-service-code-packages"></a><span data-ttu-id="b2a66-169">A szolgáltatás kód csomagok házirend konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b2a66-169">Configure a policy for service code packages</span></span>
<span data-ttu-id="b2a66-170">Az előző lépésben megtudhatta, hogyan RunAs házirend alkalmazása setupentrypoint típust.</span><span class="sxs-lookup"><span data-stu-id="b2a66-170">In the preceding steps, you saw how to apply a RunAs policy to SetupEntryPoint.</span></span> <span data-ttu-id="b2a66-171">Nézzük kissé mélyebb ismerteti, hogyan lehet létrehozni a különböző rendszerbiztonsági tagok közül melyek szolgáltatás házirendjei is alkalmazható.</span><span class="sxs-lookup"><span data-stu-id="b2a66-171">Let's look a little deeper into how to create different principals that can be applied as service policies.</span></span>

### <a name="create-local-user-groups"></a><span data-ttu-id="b2a66-172">Helyi felhasználói csoportok létrehozása</span><span class="sxs-lookup"><span data-stu-id="b2a66-172">Create local user groups</span></span>
<span data-ttu-id="b2a66-173">Adja meg, és hozzon létre felhasználói csoportokat, amelyek lehetővé teszik egy vagy több felhasználót a csoporthoz lehet hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="b2a66-173">You can define and create user groups that allow one or more users to be added to a group.</span></span> <span data-ttu-id="b2a66-174">Ez akkor hasznos, ha több felhasználó különböző belépési pontot, és bizonyos közös jogosultságok, amelyek elérhetők a csoportok szintjén van szükségük.</span><span class="sxs-lookup"><span data-stu-id="b2a66-174">This is useful if there are multiple users for different service entry points and they need to have certain common privileges that are available at the group level.</span></span> <span data-ttu-id="b2a66-175">A következő példa bemutatja a csoport **LocalAdminGroup** , amely rendszergazdai jogosultságokkal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="b2a66-175">The following example shows a local group called **LocalAdminGroup** that has administrator privileges.</span></span> <span data-ttu-id="b2a66-176">Két felhasználók Customer1 és Customer2, a helyi csoport tagjai válnak.</span><span class="sxs-lookup"><span data-stu-id="b2a66-176">Two users, Customer1 and Customer2, are made members of this local group.</span></span>

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

### <a name="create-local-users"></a><span data-ttu-id="b2a66-177">Helyi felhasználók létrehozása</span><span class="sxs-lookup"><span data-stu-id="b2a66-177">Create local users</span></span>
<span data-ttu-id="b2a66-178">Létrehozhat egy helyi felhasználót, amelyek segítségével biztonságossá tétele az alkalmazásban egy szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="b2a66-178">You can create a local user that can be used to help secure a service within the application.</span></span> <span data-ttu-id="b2a66-179">Ha egy **LocalUser** fióktípus szakaszban van megadva a rendszerbiztonsági tagoknak az alkalmazás jegyzékének Service Fabric helyi felhasználói fiókokat hoz gépekre, ahol az alkalmazás központi telepítése.</span><span class="sxs-lookup"><span data-stu-id="b2a66-179">When a **LocalUser** account type is specified in the principals section of the application manifest, Service Fabric creates local user accounts on machines where the application is deployed.</span></span> <span data-ttu-id="b2a66-180">Alapértelmezés szerint ezek a fiókok nem rendelkeznek (például a következő mintában Customer3) az alkalmazás jegyzékében megadottakkal azonos néven.</span><span class="sxs-lookup"><span data-stu-id="b2a66-180">By default, these accounts do not have the same names as those specified in the application manifest (for example, Customer3 in the following sample).</span></span> <span data-ttu-id="b2a66-181">Ehelyett dinamikusan jön létre, és véletlenszerű jelszót.</span><span class="sxs-lookup"><span data-stu-id="b2a66-181">Instead, they are dynamically generated and have random passwords.</span></span>

```xml
<Principals>
  <Users>
     <User Name="Customer3" AccountType="LocalUser" />
  </Users>
</Principals>
```

<span data-ttu-id="b2a66-182">Ha egy alkalmazás megköveteli, hogy a felhasználói fiókkal és jelszóval (például, hogy engedélyezze az NTLM-hitelesítés) összes gépen azonos, a fürtjegyzékben NTLMAuthenticationEnabled kell értéke igaz.</span><span class="sxs-lookup"><span data-stu-id="b2a66-182">If an application requires that the user account and password be same on all machines (for example, to enable NTLM authentication), the cluster manifest must set NTLMAuthenticationEnabled to true.</span></span> <span data-ttu-id="b2a66-183">A fürtjegyzékben is meg kell adnia egy NTLMAuthenticationPasswordSecret, amely ugyanazt a jelszót összes számítógépével előállítására szolgál.</span><span class="sxs-lookup"><span data-stu-id="b2a66-183">The cluster manifest must also specify an NTLMAuthenticationPasswordSecret that is used to generate the same password across all machines.</span></span>

```xml
<Section Name="Hosting">
      <Parameter Name="EndpointProviderEnabled" Value="true"/>
      <Parameter Name="NTLMAuthenticationEnabled" Value="true"/>
      <Parameter Name="NTLMAuthenticationPassworkSecret" Value="******" IsEncrypted="true"/>
 </Section>
```

### <a name="assign-policies-to-the-service-code-packages"></a><span data-ttu-id="b2a66-184">A szolgáltatás kód csomagok házirendek hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="b2a66-184">Assign policies to the service code packages</span></span>
<span data-ttu-id="b2a66-185">A **RunAsPolicy** szakasz egy **ServiceManifestImport** a rendszerbiztonsági tagok szakaszában a kódcsomag futtatásához használt fiókot adja meg.</span><span class="sxs-lookup"><span data-stu-id="b2a66-185">The **RunAsPolicy** section for a **ServiceManifestImport** specifies the account from the principals section that should be used to run a code package.</span></span> <span data-ttu-id="b2a66-186">A felhasználói fiókok a rendszerbiztonsági tagok szakaszban kód csomagok a szolgáltatás-jegyzékfájlból is társítja.</span><span class="sxs-lookup"><span data-stu-id="b2a66-186">It also associates code packages from the service manifest with user accounts in the principals section.</span></span> <span data-ttu-id="b2a66-187">Ez a beállítás vagy a fő belépési pontok adhat meg, vagy megadhat `All` egyaránt vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="b2a66-187">You can specify this for the setup or main entry points, or you can specify `All` to apply it to both.</span></span> <span data-ttu-id="b2a66-188">A következő példa bemutatja a különböző szabályzatok:</span><span class="sxs-lookup"><span data-stu-id="b2a66-188">The following example shows different policies being applied:</span></span>

```xml
<Policies>
<RunAsPolicy CodePackageRef="Code" UserRef="LocalAdmin" EntryPointType="Setup"/>
<RunAsPolicy CodePackageRef="Code" UserRef="Customer3" EntryPointType="Main"/>
</Policies>
```

<span data-ttu-id="b2a66-189">Ha **EntryPointType** nincs megadva, az alapértelmezett érték az EntryPointType = "Main".</span><span class="sxs-lookup"><span data-stu-id="b2a66-189">If **EntryPointType** is not specified, the default is set to EntryPointType=”Main”.</span></span> <span data-ttu-id="b2a66-190">Adja meg **SetupEntryPoint** akkor különösen akkor hasznos, ha bizonyos magas szintű jogosultságokat igénylő telepítési műveletekhez a rendszerfiók futtatja.</span><span class="sxs-lookup"><span data-stu-id="b2a66-190">Specifying **SetupEntryPoint** is especially useful when you want to run certain high-privilege setup operations under a system account.</span></span> <span data-ttu-id="b2a66-191">A tényleges kódját egy alacsonyabb szintű szolgáltatásfiók alatt futhatnak.</span><span class="sxs-lookup"><span data-stu-id="b2a66-191">The actual service code can run under a lower-privilege account.</span></span>

### <a name="apply-a-default-policy-to-all-service-code-packages"></a><span data-ttu-id="b2a66-192">Minden szolgáltatás kód csomagok egy alapértelmezett házirend alkalmazása</span><span class="sxs-lookup"><span data-stu-id="b2a66-192">Apply a default policy to all service code packages</span></span>
<span data-ttu-id="b2a66-193">Használja a **DefaultRunAsPolicy** szakaszban adja meg az összes kód alapértelmezett felhasználói fiók csomagok, amelyeket nem kell egy adott **RunAsPolicy** definiálva.</span><span class="sxs-lookup"><span data-stu-id="b2a66-193">You use the **DefaultRunAsPolicy** section to specify a default user account for all code packages that don’t have a specific **RunAsPolicy** defined.</span></span> <span data-ttu-id="b2a66-194">Ha a szolgáltatás egy alkalmazás által használt jegyzékfájlban megadott kód csomagokat a legtöbb kell futni ugyanahhoz a felhasználóhoz, az alkalmazás csak definiálhat egy alapértelmezett RunAs házirendet, hogy felhasználói fiókkal.</span><span class="sxs-lookup"><span data-stu-id="b2a66-194">If most of the code packages that are specified in the service manifest used by an application need to run under the same user, the application can just define a default RunAs policy with that user account.</span></span> <span data-ttu-id="b2a66-195">Az alábbi példa azt jelenti, hogy ha a kódcsomag nem rendelkezik egy **RunAsPolicy** megadva, a kódcsomag kell futnia a **MyDefaultAccount** rendszerbiztonsági tagok szakaszában megadott.</span><span class="sxs-lookup"><span data-stu-id="b2a66-195">The following example specifies that if a code package does not have a **RunAsPolicy** specified, the code package should run under the **MyDefaultAccount** specified in the principals section.</span></span>

```xml
<Policies>
  <DefaultRunAsPolicy UserRef="MyDefaultAccount"/>
</Policies>
```
### <a name="use-an-active-directory-domain-group-or-user"></a><span data-ttu-id="b2a66-196">Az Active Directory tartományi csoport vagy felhasználó használata</span><span class="sxs-lookup"><span data-stu-id="b2a66-196">Use an Active Directory domain group or user</span></span>
<span data-ttu-id="b2a66-197">A Service Fabric az önálló telepítő segítségével a Windows Server telepített példánya futtathatja a hitelesítő adatok a szolgáltatás az Active Directory-felhasználó vagy csoport fiókját.</span><span class="sxs-lookup"><span data-stu-id="b2a66-197">For an instance of Service Fabric that was installed on Windows Server by using the standalone installer, you can run the service under the credentials for an Active Directory user or group account.</span></span> <span data-ttu-id="b2a66-198">Ez az Active Directory helyszíni saját tartományában, és nincs az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="b2a66-198">This is Active Directory on-premises within your domain and is not with Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="b2a66-199">Tartományi felhasználó vagy csoport segítségével érheti el más erőforrások (például fájlmegosztások) a tartományban, amely engedéllyel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="b2a66-199">By using a domain user or group, you can then access other resources in the domain (for example, file shares) that have been granted permissions.</span></span>

<span data-ttu-id="b2a66-200">A következő példa bemutatja az Active Directory-felhasználó nevű *tesztfelhasználó néven* a tartomány-tanúsítvány használatával titkosítja jelszó az úgynevezett *MyCert*.</span><span class="sxs-lookup"><span data-stu-id="b2a66-200">The following example shows an Active Directory user called *TestUser* with their domain password encrypted by using a certificate called *MyCert*.</span></span> <span data-ttu-id="b2a66-201">Használhatja a `Invoke-ServiceFabricEncryptText` PowerShell-parancsot a titkos titkosított szöveg létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="b2a66-201">You can use the `Invoke-ServiceFabricEncryptText` PowerShell command to create the secret cipher text.</span></span> <span data-ttu-id="b2a66-202">Lásd: [Service Fabric-alkalmazások a titkos kulcsok kezelése](service-fabric-application-secret-management.md) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="b2a66-202">See [Managing secrets in Service Fabric applications](service-fabric-application-secret-management.md) for details.</span></span>

<span data-ttu-id="b2a66-203">Ha már telepítette a titkosított jelszót a helyi számítógép egy sávon kívüli módszer használatával a tanúsítvány titkos kulcsának (Azure-ban ez az Azure Resource Manager használatával).</span><span class="sxs-lookup"><span data-stu-id="b2a66-203">You must deploy the private key of the certificate to decrypt the password to the local machine by using an out-of-band method (in Azure, this is via Azure Resource Manager).</span></span> <span data-ttu-id="b2a66-204">Ezt követően a Service Fabric telepíti a szolgáltatáscsomagnak a gép, esetén képes visszafejteni a titkos kulcsot, és (valamint a felhasználónév) hitelesítést és az Active Directory futtatásához használandó ezeket a hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="b2a66-204">Then, when Service Fabric deploys the service package to the machine, it is able to decrypt the secret and (along with the user name) authenticate with Active Directory to run under those credentials.</span></span>

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
### <a name="use-a-group-managed-service-account"></a><span data-ttu-id="b2a66-205">Adjon meg egy csoportot felügyelt szolgáltatásfiók.</span><span class="sxs-lookup"><span data-stu-id="b2a66-205">Use a Group Managed Service Account.</span></span>
<span data-ttu-id="b2a66-206">A Service Fabric az önálló telepítő segítségével a Windows Server telepített példánya a service is futhat, a csoportosan felügyelt szolgáltatásfiókok (gMSA).</span><span class="sxs-lookup"><span data-stu-id="b2a66-206">For an instance of Service Fabric that was installed on Windows Server by using the standalone installer, you can run the service as a group Managed Service Account (gMSA).</span></span> <span data-ttu-id="b2a66-207">Vegye figyelembe, hogy ez az Active Directory a helyszíni tartományán belüli és nem az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="b2a66-207">Note that this is Active Directory on-premises within your domain and is not with Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="b2a66-208">Csoportosan felügyelt szolgáltatásfiók használatával nem kell jelszót vagy a titkosított jelszót a `Application Manifest`.</span><span class="sxs-lookup"><span data-stu-id="b2a66-208">By using a gMSA there is no password or encrypted password stored in the `Application Manifest`.</span></span>

<span data-ttu-id="b2a66-209">A következő példa bemutatja, hogyan nevű csoportosan felügyelt szolgáltatásfiókok létrehozása *svc-teszt$*; központi telepítése a fürt csomópontjai; a felügyelt szolgáltatásfiók és a konfigurálása egyszerű felhasználónév.</span><span class="sxs-lookup"><span data-stu-id="b2a66-209">The following example shows how to create a gMSA account called *svc-Test$*; how to deploy that managed service account to the cluster nodes; and how to configure the user principal.</span></span>

##### <a name="prerequisites"></a><span data-ttu-id="b2a66-210">Előfeltételek.</span><span class="sxs-lookup"><span data-stu-id="b2a66-210">Prerequisites.</span></span>
- <span data-ttu-id="b2a66-211">A tartomány kell KDS-gyökérkulcsot.</span><span class="sxs-lookup"><span data-stu-id="b2a66-211">The domain needs a KDS root key.</span></span>
- <span data-ttu-id="b2a66-212">A tartományban kell lennie egy Windows Server 2012 vagy újabb működési szintjét.</span><span class="sxs-lookup"><span data-stu-id="b2a66-212">The domain needs to be at a Windows Server 2012 or later functional level.</span></span>

##### <a name="example"></a><span data-ttu-id="b2a66-213">Példa</span><span class="sxs-lookup"><span data-stu-id="b2a66-213">Example</span></span>
1. <span data-ttu-id="b2a66-214">Active directory tartományi rendszergazdának létre csoportosan felügyelt szolgáltatás fiókja rendelkezik a `New-ADServiceAccount` parancsmagot, és győződjön meg arról, hogy a `PrincipalsAllowedToRetrieveManagedPassword` tartalmazza az összes, a service fabric-csomópont.</span><span class="sxs-lookup"><span data-stu-id="b2a66-214">Have an active directory domain administrator create a group managed service account using the `New-ADServiceAccount` commandlet and ensure that the `PrincipalsAllowedToRetrieveManagedPassword` includes all of the service fabric cluster nodes.</span></span> <span data-ttu-id="b2a66-215">Vegye figyelembe, hogy `AccountName`, `DnsHostName`, és `ServicePrincipalName` egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="b2a66-215">Note that `AccountName`, `DnsHostName`, and `ServicePrincipalName` must be unique.</span></span>
```
New-ADServiceAccount -name svc-Test$ -DnsHostName svc-test.contoso.com  -ServicePrincipalNames http/svc-test.contoso.com -PrincipalsAllowedToRetrieveManagedPassword SfNode0$,SfNode1$,SfNode2$,SfNode3$,SfNode4$
```
2. <span data-ttu-id="b2a66-216">A service fabric-csomópont minden egyes (például `SfNode0$,SfNode1$,SfNode2$,SfNode3$,SfNode4$`), telepítése és tesztelése a csoportosan felügyelt szolgáltatásfiókot.</span><span class="sxs-lookup"><span data-stu-id="b2a66-216">On each of the service fabric cluster nodes (for example, `SfNode0$,SfNode1$,SfNode2$,SfNode3$,SfNode4$`), install and test the gMSA.</span></span>
```
Add-WindowsFeature RSAT-AD-PowerShell
Install-AdServiceAccount svc-Test$
Test-AdServiceAccount svc-Test$
```
3. <span data-ttu-id="b2a66-217">Az egyszerű, valamint a felhasználói hivatkozni RunAsPolicy konfigurálni.</span><span class="sxs-lookup"><span data-stu-id="b2a66-217">Configure the User principal, and configure the RunAsPolicy to reference the user.</span></span>
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

## <a name="assign-a-security-access-policy-for-http-and-https-endpoints"></a><span data-ttu-id="b2a66-218">Rendelje hozzá a biztonsági házirend HTTP és HTTPS-végpontok</span><span class="sxs-lookup"><span data-stu-id="b2a66-218">Assign a security access policy for HTTP and HTTPS endpoints</span></span>
<span data-ttu-id="b2a66-219">Ha RunAs házirend vonatkozik egy szolgáltatás, és a szolgáltatás jegyzékfájl deklarálja a HTTP protokoll végpont erőforrások, meg kell adnia egy **SecurityAccessPolicy** arról, hogy ezeket a végpontokat rendelt portok megfelelően a futtató felhasználói fiók, amely a szolgáltatás fut a felsorolt hozzáférés-vezérlés.</span><span class="sxs-lookup"><span data-stu-id="b2a66-219">If you apply a RunAs policy to a service and the service manifest declares endpoint resources with the HTTP protocol, you must specify a **SecurityAccessPolicy** to ensure that ports allocated to these endpoints are correctly access-control listed for the RunAs user account that the service runs under.</span></span> <span data-ttu-id="b2a66-220">Ellenkező esetben **http.sys** nem rendelkezik hozzáféréssel a szolgáltatáshoz, és sikertelen hívások kapott az ügyféltől.</span><span class="sxs-lookup"><span data-stu-id="b2a66-220">Otherwise, **http.sys** does not have access to the service, and you get failures with calls from the client.</span></span> <span data-ttu-id="b2a66-221">A következő példa a Customer1 fiók vonatkozik nevű végpont **EndpointName**, amely azt teszi teljes körű hozzáférési jogosultságokat.</span><span class="sxs-lookup"><span data-stu-id="b2a66-221">The following example applies the Customer1 account to an endpoint called **EndpointName**, which gives it full access rights.</span></span>

```xml
<Policies>
   <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
   <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
   <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
</Policies>
```

<span data-ttu-id="b2a66-222">A HTTPS-végpont esetében is a tanúsítvány vissza az ügyfélnek jelzésére.</span><span class="sxs-lookup"><span data-stu-id="b2a66-222">For the HTTPS endpoint, you also have to indicate the name of the certificate to return to the client.</span></span> <span data-ttu-id="b2a66-223">Ehhez a **EndpointBindingPolicy**, a tanúsítványok szakaszban található az alkalmazás jegyzékében meghatározott tanúsítvánnyal.</span><span class="sxs-lookup"><span data-stu-id="b2a66-223">You can do this by using **EndpointBindingPolicy**, with the certificate defined in a certificates section in the application manifest.</span></span>

```xml
<Policies>
   <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
  <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
   <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
  <!--EndpointBindingPolicy is needed if the EndpointName is secured with https -->
  <EndpointBindingPolicy EndpointRef="EndpointName" CertificateRef="Cert1" />
</Policies
```
## <a name="upgrading-multiple-applications-with-https-endpoints"></a><span data-ttu-id="b2a66-224">A https-végpontnak több alkalmazás frissítése</span><span class="sxs-lookup"><span data-stu-id="b2a66-224">Upgrading multiple applications with https endpoints</span></span>
<span data-ttu-id="b2a66-225">Ügyeljen arra, hogy használni szeretné a **ugyanazt a portot** http használata esetén egy alkalmazás különböző példányai**s**.</span><span class="sxs-lookup"><span data-stu-id="b2a66-225">You need to be careful not to use the **same port** for different instances of the same application when using http**s**.</span></span> <span data-ttu-id="b2a66-226">A hiba oka, hogy a Service Fabric nem lehet frissíteni a tanúsítvány az alkalmazás példányai közül.</span><span class="sxs-lookup"><span data-stu-id="b2a66-226">The reason is that Service Fabric won't be able to upgrade the cert for one of the application instances.</span></span> <span data-ttu-id="b2a66-227">Ha például 1 vagy alkalmazás 2 mindkét alkalmazás szeretné, hogy az 1. tanúsítvány frissítése a cert 2.</span><span class="sxs-lookup"><span data-stu-id="b2a66-227">For example, if application 1 or application 2 both want to upgrade their cert 1 to cert 2.</span></span> <span data-ttu-id="b2a66-228">Ha a frissítés történik, a Service Fabric előfordulhat, hogy rendelkezik kiüríti az 1. tanúsítvány regisztrálása a következőben: http.sys annak ellenére, hogy a többi alkalmazás továbbra is használja.</span><span class="sxs-lookup"><span data-stu-id="b2a66-228">When the upgrade happens, Service Fabric might have cleaned up the cert 1 registration with http.sys even though the other application is still using it.</span></span> <span data-ttu-id="b2a66-229">Ennek megelőzése érdekében a Service Fabric észleli, hogy már egy másik alkalmazáspéldány regisztrálva a porton (mert a http.sys) a tanúsítványhoz, és a művelet sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="b2a66-229">To prevent this, Service Fabric detects that there is already another application instance registered on the port with the certificate (due to http.sys) and fails the operation.</span></span>

<span data-ttu-id="b2a66-230">Ezért a Service Fabric nem támogatja a frissítést két különböző szolgáltatásokat **ugyanazt a portot** másik alkalmazási esetekben.</span><span class="sxs-lookup"><span data-stu-id="b2a66-230">Hence Service Fabric does not support upgrading two different services using **the same port** in different application instances.</span></span> <span data-ttu-id="b2a66-231">Ez azt jelenti ugyanazt a tanúsítványt nem használhat a különböző szolgáltatások ugyanazt a portot.</span><span class="sxs-lookup"><span data-stu-id="b2a66-231">In other words, you cannot use the same certificate on different services on the same port.</span></span> <span data-ttu-id="b2a66-232">Ha egy megosztott tanúsítvány rendelkezik ugyanazon a porton van szüksége, győződjön meg arról, hogy a szolgáltatások egy elhelyezési korlátozás rendelkező különböző gépeken kerülnek szeretné.</span><span class="sxs-lookup"><span data-stu-id="b2a66-232">If you need to have a shared certificate on the same port, you need to ensure that the services are placed on different machines with placement constraints.</span></span> <span data-ttu-id="b2a66-233">Vagy fontolja meg a Service Fabric dinamikus portok lehetőség szerint minden egyes szolgáltatás minden egyes alkalmazás-példányban.</span><span class="sxs-lookup"><span data-stu-id="b2a66-233">Or consider using Service Fabric dynamic ports if possible for each service in each application instance.</span></span> 

<span data-ttu-id="b2a66-234">Ha megjelenik egy frissítés sikertelen lesz, a https, hiba, figyelmeztetés, amely szerint "A Windows HTTP-kiszolgáló API nem támogatja több tanúsítvány egy portot használó alkalmazásokhoz."</span><span class="sxs-lookup"><span data-stu-id="b2a66-234">If you see an upgrade fail with https, an error warning saying "The Windows HTTP Server API does not support multiple certificates for applications that share a port.”</span></span>

## <a name="a-complete-application-manifest-example"></a><span data-ttu-id="b2a66-235">A teljes alkalmazás jegyzékének példa</span><span class="sxs-lookup"><span data-stu-id="b2a66-235">A complete application manifest example</span></span>
<span data-ttu-id="b2a66-236">A következő alkalmazás jegyzékfájlja jeleníti meg a különböző beállításainak jelentős része:</span><span class="sxs-lookup"><span data-stu-id="b2a66-236">The following application manifest shows many of the different settings:</span></span>

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
        <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
         <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
        <!--EndpointBindingPolicy is needed the EndpointName is secured with https -->
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


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a><span data-ttu-id="b2a66-237">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b2a66-237">Next steps</span></span>
* [<span data-ttu-id="b2a66-238">Az alkalmazásmodell ismertetése</span><span class="sxs-lookup"><span data-stu-id="b2a66-238">Understand the application model</span></span>](service-fabric-application-model.md)
* [<span data-ttu-id="b2a66-239">Adja meg a szolgáltatás jegyzék erőforrások</span><span class="sxs-lookup"><span data-stu-id="b2a66-239">Specify resources in a service manifest</span></span>](service-fabric-service-manifest-resources.md)
* [<span data-ttu-id="b2a66-240">Alkalmazás üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="b2a66-240">Deploy an application</span></span>](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png
