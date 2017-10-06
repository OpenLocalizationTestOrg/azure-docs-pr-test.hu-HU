---
title: "aaaTroubleshoot szerepkörök toostart teljesítő |} Microsoft Docs"
description: "Az alábbiakban néhány gyakori okáról, miért egy felhőalapú szolgáltatás szerepkör toostart sikertelenek lehetnek. Megoldások toothese problémák is biztosítja."
services: cloud-services
documentationcenter: 
author: simonxjx
manager: felixwu
editor: 
tags: top-support-issue
ms.assetid: 674b2faf-26d7-4f54-99ea-a9e02ef0eb2f
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 7/26/2017
ms.author: v-six
ms.openlocfilehash: e2fbecb08a10984add79dfc74e73de6869bb314f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-cloud-service-roles-that-fail-toostart"></a><span data-ttu-id="6635c-104">Sikertelen toostart Felhőszolgáltatási szerepkörök hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="6635c-104">Troubleshoot Cloud Service roles that fail toostart</span></span>
<span data-ttu-id="6635c-105">Az alábbiakban néhány gyakori problémák és megoldások kapcsolódó tooAzure Felhőszolgáltatások szerepkörök toostart sikertelen.</span><span class="sxs-lookup"><span data-stu-id="6635c-105">Here are some common problems and solutions related tooAzure Cloud Services roles that fail toostart.</span></span>

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="missing-dlls-or-dependencies"></a><span data-ttu-id="6635c-106">Hiányzó DLL-ek vagy függőségek</span><span class="sxs-lookup"><span data-stu-id="6635c-106">Missing DLLs or dependencies</span></span>
<span data-ttu-id="6635c-107">Nem válaszoló szerepkörök és a szerepköröket, amelyek között vannak Váltás **inicializálás**, **foglalt**, és **leállítása** állapotok hiányzik a dll-fájl vagy szerelvény okozhatja.</span><span class="sxs-lookup"><span data-stu-id="6635c-107">Unresponsive roles and roles that are cycling between **Initializing**, **Busy**, and **Stopping** states can be caused by missing DLLs or assemblies.</span></span>

<span data-ttu-id="6635c-108">Hiányzik a dll-fájl vagy szerelvény tünetei lehet:</span><span class="sxs-lookup"><span data-stu-id="6635c-108">Symptoms of missing DLLs or assemblies can be:</span></span>

* <span data-ttu-id="6635c-109">A szerepkör példánya van Váltás **inicializálás**, **foglalt**, és **leállítása** állapotok.</span><span class="sxs-lookup"><span data-stu-id="6635c-109">Your role instance is cycling through **Initializing**, **Busy**, and **Stopping** states.</span></span>
* <span data-ttu-id="6635c-110">A szerepkör példánya túl áthelyezte**készen** tooyour webalkalmazás keresse meg, ha hello lap nem jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="6635c-110">Your role instance has moved too**Ready** but if you navigate tooyour web application, hello page does not appear.</span></span>

<span data-ttu-id="6635c-111">Többféleképpen ajánlott vizsgálata ezeket a problémákat.</span><span class="sxs-lookup"><span data-stu-id="6635c-111">There are several recommended methods for investigating these issues.</span></span>

## <a name="diagnose-missing-dll-issues-in-a-web-role"></a><span data-ttu-id="6635c-112">A webes szerepkör hiányzó DLL problémák diagnosztizálása</span><span class="sxs-lookup"><span data-stu-id="6635c-112">Diagnose missing DLL issues in a web role</span></span>
<span data-ttu-id="6635c-113">Navigálás tooa webhely, amely a webes szerepkör telepítve van, és hello böngészőben megjelenik egy kiszolgáló hiba hasonló toohello következő, azt jelezheti, hogy a dll-fájl hiányzik.</span><span class="sxs-lookup"><span data-stu-id="6635c-113">When you navigate tooa website that is deployed in a web role, and hello browser displays a server error similar toohello following, it may indicate that a DLL is missing.</span></span>

![Kiszolgálóhiba a "/" alkalmazás.](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503388.png)

## <a name="diagnose-issues-by-turning-off-custom-errors"></a><span data-ttu-id="6635c-115">Az egyéni hibák kikapcsolásával eseményadatokat</span><span class="sxs-lookup"><span data-stu-id="6635c-115">Diagnose issues by turning off custom errors</span></span>
<span data-ttu-id="6635c-116">Bővebb hibainformáció hello web.config a hello webes szerepkör tooset hello egyéni hiba mód tooOff konfigurálásával és a fájl újbóli terjesztése hello szolgáltatást is megtekinthetők.</span><span class="sxs-lookup"><span data-stu-id="6635c-116">More complete error information can be viewed by configuring hello web.config for hello web role tooset hello custom error mode tooOff and redeploying hello service.</span></span>

<span data-ttu-id="6635c-117">tooview hibák több végezze el a távoli asztal használata nélkül:</span><span class="sxs-lookup"><span data-stu-id="6635c-117">tooview more complete errors without using Remote Desktop:</span></span>

1. <span data-ttu-id="6635c-118">Nyissa meg a hello megoldást a Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6635c-118">Open hello solution in Microsoft Visual Studio.</span></span>
2. <span data-ttu-id="6635c-119">A hello **Megoldáskezelőben**, keresse meg hello web.config fájlt, és nyissa meg.</span><span class="sxs-lookup"><span data-stu-id="6635c-119">In hello **Solution Explorer**, locate hello web.config file and open it.</span></span>
3. <span data-ttu-id="6635c-120">Hello web.config fájlban keresse meg a hello system.web szakaszt, és adja hozzá a következő sor hello:</span><span class="sxs-lookup"><span data-stu-id="6635c-120">In hello web.config file, locate hello system.web section and add hello following line:</span></span>

    ```xml
    <customErrors mode="Off" />
    ```
4. <span data-ttu-id="6635c-121">Hello fájl mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="6635c-121">Save hello file.</span></span>
5. <span data-ttu-id="6635c-122">Csomagolja újra, és telepítse újra hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="6635c-122">Repackage and redeploy hello service.</span></span>

<span data-ttu-id="6635c-123">Miután hello szolgáltatást újra kell telepíteni, megjelenik a hibaüzenet hello hello hiányzik a szerelvény vagy dll-fájl neve.</span><span class="sxs-lookup"><span data-stu-id="6635c-123">Once hello service is redeployed, you will see an error message with hello name of hello missing assembly or DLL.</span></span>

## <a name="diagnose-issues-by-viewing-hello-error-remotely"></a><span data-ttu-id="6635c-124">Hello hiba távolról megtekintésével eseményadatokat</span><span class="sxs-lookup"><span data-stu-id="6635c-124">Diagnose issues by viewing hello error remotely</span></span>
<span data-ttu-id="6635c-125">A távoli asztal tooaccess hello szerepkört használjon, és távolról tekintheti meg bővebb hibainformáció.</span><span class="sxs-lookup"><span data-stu-id="6635c-125">You can use Remote Desktop tooaccess hello role and view more complete error information remotely.</span></span> <span data-ttu-id="6635c-126">Írja be a következő lépéseket tooview hello hibák távoli asztal használatával hello használata:</span><span class="sxs-lookup"><span data-stu-id="6635c-126">Use hello following steps tooview hello errors by using Remote Desktop:</span></span>

1. <span data-ttu-id="6635c-127">Győződjön meg arról, hogy telepítve van-e az Azure SDK 1.3-as vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="6635c-127">Ensure that Azure SDK 1.3 or later is installed.</span></span>
2. <span data-ttu-id="6635c-128">Hello megoldás üzembe helyezése közben hello Visual Studio használatával, válassza a túl "... konfigurálása távoli asztali kapcsolatok".</span><span class="sxs-lookup"><span data-stu-id="6635c-128">During hello deployment of hello solution by using Visual Studio, choose too“Configure Remote Desktop connections…”.</span></span> <span data-ttu-id="6635c-129">Hello távoli asztali kapcsolat konfigurálásával kapcsolatos további információkért lásd: [a távoli asztal Azure szerepkörök](../vs-azure-tools-remote-desktop-roles.md).</span><span class="sxs-lookup"><span data-stu-id="6635c-129">For more information on configuring hello Remote Desktop connection, see [Using Remote Desktop with Azure Roles](../vs-azure-tools-remote-desktop-roles.md).</span></span>
3. <span data-ttu-id="6635c-130">Hello Microsoft klasszikus Azure portál, miután hello példány egy állapotát jeleníti meg a **készen**, kattintson az egyik hello szerepkörpéldányokat.</span><span class="sxs-lookup"><span data-stu-id="6635c-130">In hello Microsoft Azure classic portal, once hello instance shows a status of **Ready**, click one of hello role instances.</span></span>
4. <span data-ttu-id="6635c-131">Hello kattintson **Connect** hello ikonra **távelérési** hello menüszalag területén.</span><span class="sxs-lookup"><span data-stu-id="6635c-131">Click hello **Connect** icon in hello **Remote Access** area of hello ribbon.</span></span>
5. <span data-ttu-id="6635c-132">Jelentkezzen be toohello virtuális gép hello hello távoli asztal konfigurálása során megadott hitelesítő adatok használatával.</span><span class="sxs-lookup"><span data-stu-id="6635c-132">Sign in toohello virtual machine by using hello credentials that were specified during hello Remote Desktop configuration.</span></span>
6. <span data-ttu-id="6635c-133">Nyisson meg egy parancsablakot.</span><span class="sxs-lookup"><span data-stu-id="6635c-133">Open a command window.</span></span>
7. <span data-ttu-id="6635c-134">Gépelje be: `IPconfig`.</span><span class="sxs-lookup"><span data-stu-id="6635c-134">Type `IPconfig`.</span></span>
8. <span data-ttu-id="6635c-135">Jegyezze fel a hello IPV4-cím értékét.</span><span class="sxs-lookup"><span data-stu-id="6635c-135">Note hello IPV4 Address value.</span></span>
9. <span data-ttu-id="6635c-136">Nyissa meg az Internet Explorert.</span><span class="sxs-lookup"><span data-stu-id="6635c-136">Open Internet Explorer.</span></span>
10. <span data-ttu-id="6635c-137">Írja be a hello cím és hello webalkalmazás hello nevét.</span><span class="sxs-lookup"><span data-stu-id="6635c-137">Type hello address and hello name of hello web application.</span></span> <span data-ttu-id="6635c-138">Például: `http://<IPV4 Address>/default.aspx`.</span><span class="sxs-lookup"><span data-stu-id="6635c-138">For example, `http://<IPV4 Address>/default.aspx`.</span></span>

<span data-ttu-id="6635c-139">Navigálás toohello webhely most visszaadható egyértelműbben hibaüzenetek:</span><span class="sxs-lookup"><span data-stu-id="6635c-139">Navigating toohello website will now return more explicit error messages:</span></span>

* <span data-ttu-id="6635c-140">Kiszolgálóhiba a "/" alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="6635c-140">Server Error in '/' Application.</span></span>
* <span data-ttu-id="6635c-141">Leírás: Nem kezelt kivétel történt hello hello aktuális webes kérelem végrehajtása során.</span><span class="sxs-lookup"><span data-stu-id="6635c-141">Description: An unhandled exception occurred during hello execution of hello current web request.</span></span> <span data-ttu-id="6635c-142">Tekintse át a hello Veremkivonat hello hibával kapcsolatos további tudnivalókért és az azt okozó kódrészletre hello kódban.</span><span class="sxs-lookup"><span data-stu-id="6635c-142">Please review hello stack trace for more information about hello error and where it originated in hello code.</span></span>
* <span data-ttu-id="6635c-143">A kivétel részletei: System.IO.FIleNotFoundException: nem sikerült betölteni a fájl vagy szerelvény "Microsoft.WindowsAzure.StorageClient, verzió: 1.1.0.0-s, Culture = neutral, PublicKeyToken = = 31bf856ad364e35" vagy annak valamelyik függősége.</span><span class="sxs-lookup"><span data-stu-id="6635c-143">Exception Details: System.IO.FIleNotFoundException: Could not load file or assembly ‘Microsoft.WindowsAzure.StorageClient, Version=1.1.0.0, Culture=neutral, PublicKeyToken=31bf856ad364e35’ or one of its dependencies.</span></span> <span data-ttu-id="6635c-144">hello rendszer nem találja a megadott hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="6635c-144">hello system cannot find hello file specified.</span></span>

<span data-ttu-id="6635c-145">Példa:</span><span class="sxs-lookup"><span data-stu-id="6635c-145">For example:</span></span>

![A "/" alkalmazás explicit kiszolgálóhiba](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503389.png)

## <a name="diagnose-issues-by-using-hello-compute-emulator"></a><span data-ttu-id="6635c-147">Eseményadatokat hello compute emulator használatával</span><span class="sxs-lookup"><span data-stu-id="6635c-147">Diagnose issues by using hello compute emulator</span></span>
<span data-ttu-id="6635c-148">Hello Microsoft Azure compute emulator toodiagnose használja, és a Hiányzó függőségek és web.config hibák elhárítása.</span><span class="sxs-lookup"><span data-stu-id="6635c-148">You can use hello Microsoft Azure compute emulator toodiagnose and troubleshoot issues of missing dependencies and web.config errors.</span></span>

<span data-ttu-id="6635c-149">A legjobb eredmény érdekében ezzel a módszerrel a diagnosztikai egy számítógép vagy virtuális géphez, amelyen a Windows tiszta telepítését kell használnia.</span><span class="sxs-lookup"><span data-stu-id="6635c-149">For best results in using this method of diagnosis, you should use a computer or virtual machine that has a clean installation of Windows.</span></span> <span data-ttu-id="6635c-150">toobest hello Azure környezetbe szimulálja, Windows Server 2008 R2 x64 használja.</span><span class="sxs-lookup"><span data-stu-id="6635c-150">toobest simulate hello Azure environment, use Windows Server 2008 R2 x64.</span></span>

1. <span data-ttu-id="6635c-151">Hello hello önálló verziójának telepítése [Azure SDK](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="6635c-151">Install hello standalone version of hello [Azure SDK](https://azure.microsoft.com/downloads/).</span></span>
2. <span data-ttu-id="6635c-152">Hello fejlesztői gépen hello felhőszolgáltatás-projekt létrehozása.</span><span class="sxs-lookup"><span data-stu-id="6635c-152">On hello development machine, build hello cloud service project.</span></span>
3. <span data-ttu-id="6635c-153">A Windows Intézőben keresse meg a toohello bin\debug hello felhőszolgáltatás-projekt mappájából.</span><span class="sxs-lookup"><span data-stu-id="6635c-153">In Windows Explorer, navigate toohello bin\debug folder of hello cloud service project.</span></span>
4. <span data-ttu-id="6635c-154">Másolja a hello .csx mappát és a .cscfg fájl toohello számítógép toodebug hello problémák használt.</span><span class="sxs-lookup"><span data-stu-id="6635c-154">Copy hello .csx folder and .cscfg file toohello computer that you are using toodebug hello issues.</span></span>
5. <span data-ttu-id="6635c-155">Hello tiszta gépi, nyissa meg az Azure SDK parancssori ablakot, és írja be `csrun.exe /devstore:start`.</span><span class="sxs-lookup"><span data-stu-id="6635c-155">On hello clean machine, open an Azure SDK Command Prompt window and type `csrun.exe /devstore:start`.</span></span>
6. <span data-ttu-id="6635c-156">Írja be a parancssorba hello `run csrun <path too.csx folder> <path too.cscfg file> /launchBrowser`.</span><span class="sxs-lookup"><span data-stu-id="6635c-156">At hello command prompt, type `run csrun <path too.csx folder> <path too.cscfg file> /launchBrowser`.</span></span>
7. <span data-ttu-id="6635c-157">Hello szerepkör indulásakor látni fogja a hibával kapcsolatos részletes információk az Internet Explorerben.</span><span class="sxs-lookup"><span data-stu-id="6635c-157">When hello role starts, you will see detailed error information in Internet Explorer.</span></span> <span data-ttu-id="6635c-158">Is használhatja a szabványos Windows hibaelhárító eszközök toofurther hello probléma diagnosztizálásához.</span><span class="sxs-lookup"><span data-stu-id="6635c-158">You can also use standard Windows troubleshooting tools toofurther diagnose hello problem.</span></span>

## <a name="diagnose-issues-by-using-intellitrace"></a><span data-ttu-id="6635c-159">Eseményadatokat IntelliTrace használatával</span><span class="sxs-lookup"><span data-stu-id="6635c-159">Diagnose issues by using IntelliTrace</span></span>
<span data-ttu-id="6635c-160">A munkavégző és a .NET-keretrendszer 4 webes szerepkörök is használhatja [IntelliTrace](https://msdn.microsoft.com/library/dd264915.aspx), elérhető a Microsoft Visual Studio Enterprise.</span><span class="sxs-lookup"><span data-stu-id="6635c-160">For worker and web roles that use .NET Framework 4, you can use [IntelliTrace](https://msdn.microsoft.com/library/dd264915.aspx), which is available in Microsoft Visual Studio Enterprise.</span></span>

<span data-ttu-id="6635c-161">Kövesse ezeket lépéseket toodeploy hello szolgáltatás intellitrace engedélyezve:</span><span class="sxs-lookup"><span data-stu-id="6635c-161">Follow these steps toodeploy hello service with IntelliTrace enabled:</span></span>

1. <span data-ttu-id="6635c-162">Győződjön meg arról, hogy telepítve van-e az Azure SDK 1.3-as vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="6635c-162">Confirm that Azure SDK 1.3 or later is installed.</span></span>
2. <span data-ttu-id="6635c-163">Hello megoldás telepítése a Visual Studio használatával.</span><span class="sxs-lookup"><span data-stu-id="6635c-163">Deploy hello solution by using Visual Studio.</span></span> <span data-ttu-id="6635c-164">Ellenőrizze a telepítés során a hello **.NET 4-szerepkörök engedélyezése IntelliTrace** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="6635c-164">During deployment, check hello **Enable IntelliTrace for .NET 4 roles** check box.</span></span>
3. <span data-ttu-id="6635c-165">Miután hello példány elindítja, nyissa meg a hello **Server Explorer**.</span><span class="sxs-lookup"><span data-stu-id="6635c-165">Once hello instance starts, open hello **Server Explorer**.</span></span>
4. <span data-ttu-id="6635c-166">Bontsa ki a hello **Azure\\Felhőszolgáltatások** csomópont, és keresse meg hello központi telepítését.</span><span class="sxs-lookup"><span data-stu-id="6635c-166">Expand hello **Azure\\Cloud Services** node and locate hello deployment.</span></span>
5. <span data-ttu-id="6635c-167">Bontsa ki hello telepítési, amíg megjelenik a szerepkörpéldányok hello.</span><span class="sxs-lookup"><span data-stu-id="6635c-167">Expand hello deployment until you see hello role instances.</span></span> <span data-ttu-id="6635c-168">Kattintson a jobb gombbal az egyik hello példányok.</span><span class="sxs-lookup"><span data-stu-id="6635c-168">Right-click on one of hello instances.</span></span>
6. <span data-ttu-id="6635c-169">Válasszon **nézet IntelliTrace-naplók**.</span><span class="sxs-lookup"><span data-stu-id="6635c-169">Choose **View IntelliTrace logs**.</span></span> <span data-ttu-id="6635c-170">Hello **IntelliTrace összegzés** csak akkor nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="6635c-170">hello **IntelliTrace Summary** will open.</span></span>
7. <span data-ttu-id="6635c-171">Keresse meg az összefoglaló hello hello kivételek szakasza.</span><span class="sxs-lookup"><span data-stu-id="6635c-171">Locate hello exceptions section of hello summary.</span></span> <span data-ttu-id="6635c-172">Ha nincsenek kivételek, hello szakasz neve **Kivételadat**.</span><span class="sxs-lookup"><span data-stu-id="6635c-172">If there are exceptions, hello section will be labeled **Exception Data**.</span></span>
8. <span data-ttu-id="6635c-173">Bontsa ki a hello **Kivételadat** , és keressen **System.IO.FileNotFoundException** hibák hasonló toohello következő:</span><span class="sxs-lookup"><span data-stu-id="6635c-173">Expand hello **Exception Data** and look for **System.IO.FileNotFoundException** errors similar toohello following:</span></span>

![Kivételadat, hiányzó fájl vagy szerelvény](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503390.png)

## <a name="address-missing-dlls-and-assemblies"></a><span data-ttu-id="6635c-175">Hiányzó dll-EK és szerelvények</span><span class="sxs-lookup"><span data-stu-id="6635c-175">Address missing DLLs and assemblies</span></span>
<span data-ttu-id="6635c-176">Hiányzik a dll-fájl- és szerelvényfájlokat hibák tooaddress kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="6635c-176">tooaddress missing DLL and assembly errors, follow these steps:</span></span>

1. <span data-ttu-id="6635c-177">Nyissa meg a hello megoldást a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6635c-177">Open hello solution in Visual Studio.</span></span>
2. <span data-ttu-id="6635c-178">A **Megoldáskezelőben**, nyissa meg hello **hivatkozások** mappát.</span><span class="sxs-lookup"><span data-stu-id="6635c-178">In **Solution Explorer**, open hello **References** folder.</span></span>
3. <span data-ttu-id="6635c-179">Kattintson a hello hiba azonosított hello szerelvény.</span><span class="sxs-lookup"><span data-stu-id="6635c-179">Click hello assembly identified in hello error.</span></span>
4. <span data-ttu-id="6635c-180">A hello **tulajdonságok** ablaktáblán keresse meg **másolása helyi tulajdonság** és hello érték túl**igaz**.</span><span class="sxs-lookup"><span data-stu-id="6635c-180">In hello **Properties** pane, locate **Copy Local property** and set hello value too**True**.</span></span>
5. <span data-ttu-id="6635c-181">Telepítse újra a hello felhőalapú szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="6635c-181">Redeploy hello cloud service.</span></span>

<span data-ttu-id="6635c-182">Miután ellenőrizte, hogy az összes hiba javítva lett, hello szolgáltatás hello ellenőrzése nélkül telepítheti **.NET 4-szerepkörök engedélyezése IntelliTrace** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="6635c-182">Once you have verified that all errors have been corrected, you can deploy hello service without checking hello **Enable IntelliTrace for .NET 4 roles** check box.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6635c-183">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6635c-183">Next steps</span></span>
<span data-ttu-id="6635c-184">További [cikkek hibaelhárítási](https://azure.microsoft.com/documentation/articles/?tag=top-support-issue&product=cloud-services) felhőszolgáltatásai számára.</span><span class="sxs-lookup"><span data-stu-id="6635c-184">View more [troubleshooting articles](https://azure.microsoft.com/documentation/articles/?tag=top-support-issue&product=cloud-services) for cloud services.</span></span>

<span data-ttu-id="6635c-185">Hogyan tootroubleshoot felhő szerepkör-szolgáltatás Azure PaaS számítógép diagnosztikai adatok használatával állít toolearn lásd [Kevin Williamson blog adatsorozat](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).</span><span class="sxs-lookup"><span data-stu-id="6635c-185">toolearn how tootroubleshoot cloud service role issues by using Azure PaaS computer diagnostics data, see [Kevin Williamson's blog series](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).</span></span>
