---
title: "egy felhőalapú szolgáltatás helyi a Compute Emulator hello aaaProfiling |} Microsoft Docs"
services: cloud-services
description: "Hello Visual Studio Profilkészítő vizsgálatot segítő teljesítményproblémákat a cloud services csomag"
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
tags: 
ms.assetid: 25e40bf3-eea0-4b0b-9f4a-91ffe797f6c3
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 11/18/2016
ms.author: kraigb
ms.openlocfilehash: fc37c85dad4db4cc0310f73afad56fc0fe5f3963
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="testing-hello-performance-of-a-cloud-service-locally-in-hello-azure-compute-emulator-using-hello-visual-studio-profiler"></a><span data-ttu-id="38491-103">Hello Azure Compute Emulator használatával hello Visual Studio Profilkészítő hello teljesítményét egy felhőalapú szolgáltatás helyi tesztelése</span><span class="sxs-lookup"><span data-stu-id="38491-103">Testing hello Performance of a Cloud Service Locally in hello Azure Compute Emulator Using hello Visual Studio Profiler</span></span>
<span data-ttu-id="38491-104">Számos különféle eszközöket és technikákat felhőszolgáltatások hello teljesítményének teszteléséhez érhetők el.</span><span class="sxs-lookup"><span data-stu-id="38491-104">A variety of tools and techniques are available for testing hello performance of cloud services.</span></span>
<span data-ttu-id="38491-105">Egy felhőalapú szolgáltatás tooAzure közzétételekor a profilkészítési adatokat gyűjteni, és majd elemezze a Visual Studio lehet a helyi [Azure alkalmazás profilkészítési][1].</span><span class="sxs-lookup"><span data-stu-id="38491-105">When you publish a cloud service tooAzure, you can have Visual Studio collect profiling data and then analyze it locally, as described in [Profiling an Azure Application][1].</span></span>
<span data-ttu-id="38491-106">Használhatja diagnosztika tootrack teljesítményszámlálók teljesítmény számos, a [teljesítményszámlálók segítségével az Azure-ban][2].</span><span class="sxs-lookup"><span data-stu-id="38491-106">You can also use diagnostics tootrack a variety of performance counters, as described in [Using performance counters in Azure][2].</span></span>
<span data-ttu-id="38491-107">Érdemes lehet tooprofile az alkalmazás helyi a hello compute emulator toohello felhő üzembe helyezése előtt.</span><span class="sxs-lookup"><span data-stu-id="38491-107">You might also want tooprofile your application locally in hello compute emulator before deploying it toohello cloud.</span></span>

<span data-ttu-id="38491-108">Ez a cikk ismerteti a hello CPU mintavételi metódusában profilkészítési, amely helyileg végezhető hello emulátorban.</span><span class="sxs-lookup"><span data-stu-id="38491-108">This article covers hello CPU Sampling method of profiling, which can be done locally in hello emulator.</span></span> <span data-ttu-id="38491-109">CPU mintavételi egy metódust, amely profilkészítési nem nagyon zavaró.</span><span class="sxs-lookup"><span data-stu-id="38491-109">CPU sampling is a method of profiling that is not very intrusive.</span></span> <span data-ttu-id="38491-110">A kijelölt mintavételi történik hello Profilkészítő pillanatképet készít a hello hívási verem.</span><span class="sxs-lookup"><span data-stu-id="38491-110">At a designated sampling interval, hello profiler takes a snapshot of hello call stack.</span></span> <span data-ttu-id="38491-111">hello adatok gyűjtése egy meghatározott időtartamra vonatkozóan, és a jelentésben látható.</span><span class="sxs-lookup"><span data-stu-id="38491-111">hello data is collected over a period of time, and shown in a report.</span></span> <span data-ttu-id="38491-112">Ez a módszer a profilkészítési általában tooindicate, ahol számításilag intenzív alkalmazásban hello CPU munka nagyobb része történik-e.</span><span class="sxs-lookup"><span data-stu-id="38491-112">This method of profiling tends tooindicate where in a computationally intensive application most of hello CPU work is being done.</span></span>  <span data-ttu-id="38491-113">Ezáltal hello lehetőség toofocus útvonalon hello"Forró" Ha az alkalmazás van kell hello legtöbb időt.</span><span class="sxs-lookup"><span data-stu-id="38491-113">This gives you hello opportunity toofocus on hello "hot path" where your application is spending hello most time.</span></span>

## <a name="1-configure-visual-studio-for-profiling"></a><span data-ttu-id="38491-114">1: profilkészítési a Visual Studio konfigurálása</span><span class="sxs-lookup"><span data-stu-id="38491-114">1: Configure Visual Studio for profiling</span></span>
<span data-ttu-id="38491-115">Először néhány Visual Studio konfigurálására lehetőség áll rendelkezésre, előfordulhat, hogy lehet hasznos, amikor a profilkészítési.</span><span class="sxs-lookup"><span data-stu-id="38491-115">First, there are a few Visual Studio configuration options that might be helpful when profiling.</span></span> <span data-ttu-id="38491-116">toomake értelmében hello profilkészítési jelentések, az alkalmazás és a is rendszer könyvtárak szimbólumait lesz szüksége a szimbólumok (.pdb fájlok).</span><span class="sxs-lookup"><span data-stu-id="38491-116">toomake sense of hello profiling reports, you'll need symbols (.pdb files) for your application and also symbols for system libraries.</span></span> <span data-ttu-id="38491-117">Érdemes toomake meg arról, hogy a hello elérhető szimbólum kiszolgálók hivatkoznak.</span><span class="sxs-lookup"><span data-stu-id="38491-117">You'll want toomake sure that you reference hello available symbol servers.</span></span> <span data-ttu-id="38491-118">toodo Ez a hello **eszközök** Visual Studio menüjében válassza a **beállítások**, majd válassza a **hibakeresés**, majd **szimbólumok**.</span><span class="sxs-lookup"><span data-stu-id="38491-118">toodo this, on hello **Tools** menu in Visual Studio, choose **Options**, then choose **Debugging**, then **Symbols**.</span></span> <span data-ttu-id="38491-119">Győződjön meg arról, hogy a Microsoft szimbólum kiszolgálók megtalálható-e **szimbólum (.pdb) fájlhelyek**.</span><span class="sxs-lookup"><span data-stu-id="38491-119">Make sure that Microsoft Symbol Servers is listed under **Symbol file (.pdb) locations**.</span></span>  <span data-ttu-id="38491-120">Http://referencesource.microsoft.com/symbols, amely lehet További szimbólumfájlok is hivatkozhat.</span><span class="sxs-lookup"><span data-stu-id="38491-120">You can also reference http://referencesource.microsoft.com/symbols, which might have additional symbol files.</span></span>

![Szimbólum beállításai][4]

<span data-ttu-id="38491-122">Ha szükséges, egyszerűbbé teheti az hello jelenti, hogy hello Profilkészítő hoz létre úgy, hogy csak saját kód.</span><span class="sxs-lookup"><span data-stu-id="38491-122">If desired, you can simplify hello reports that hello profiler generates by setting Just My Code.</span></span> <span data-ttu-id="38491-123">Csak saját kód engedélyezve van a függvény hívási verem egyszerűsítettek, így teljesen belső toolibraries meghív és .NET-keretrendszer hello rejtve maradnak az hello jelentések.</span><span class="sxs-lookup"><span data-stu-id="38491-123">With Just My Code enabled, function call stacks are simplified so that calls entirely internal toolibraries and hello .NET Framework are hidden from hello reports.</span></span> <span data-ttu-id="38491-124">A hello **eszközök** menüben válasszon **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="38491-124">On hello **Tools** menu, choose **Options**.</span></span> <span data-ttu-id="38491-125">Bontsa ki a hello **teljesítmény eszközök** csomópont, és válassza a **általános**.</span><span class="sxs-lookup"><span data-stu-id="38491-125">Then expand hello **Performance Tools** node, and choose **General**.</span></span> <span data-ttu-id="38491-126">Válassza ki a hello jelölőnégyzetét **engedélyezése csak saját kód Profilkészítő jelentések**.</span><span class="sxs-lookup"><span data-stu-id="38491-126">Select hello checkbox for **Enable Just My Code for profiler reports**.</span></span>

![Csak saját kód beállítása][17]

<span data-ttu-id="38491-128">Ezek az utasítások is használhatja, egy meglévő projektjébe vagy egy új projektet.</span><span class="sxs-lookup"><span data-stu-id="38491-128">You can use these instructions with an existing project or with a new project.</span></span>  <span data-ttu-id="38491-129">Ha az alább ismertetett technikák hoz létre egy új projekt tootry hello, válasszon egy C# **Azure Cloud Service** projektre, és válassza ki a **webes szerepkör** és egy **feldolgozói szerepkör**.</span><span class="sxs-lookup"><span data-stu-id="38491-129">If you create a new project tootry hello techniques described below, choose a C# **Azure Cloud Service** project, and select a **Web Role** and a **Worker Role**.</span></span>

![Azure Cloud Service projekt szerepkörök][5]

<span data-ttu-id="38491-131">Például céljából, adjon hozzá néhány kódot tooyour projekt, amely sok időt vesz igénybe, és bemutatja a nyilvánvaló teljesítmény kapcsolatos problémára.</span><span class="sxs-lookup"><span data-stu-id="38491-131">For example purposes, add some code tooyour project that takes a lot of time and demonstrates some obvious performance problem.</span></span> <span data-ttu-id="38491-132">Például adja hozzá a következő kód tooa feldolgozói szerepkör projekt hello:</span><span class="sxs-lookup"><span data-stu-id="38491-132">For example, add hello following code tooa worker role project:</span></span>

    public class Concatenator
    {
        public static string Concatenate(int number)
        {
            int count;
            string s = "";
            for (count = 0; count < number; count++)
            {
                s += "\n" + count.ToString();
            }
            return s;
        }
    }

<span data-ttu-id="38491-133">Ez a kód hívja hello RunAsync metódusában hello feldolgozói szerepkör RoleEntryPoint származtatott osztályban.</span><span class="sxs-lookup"><span data-stu-id="38491-133">Call this code from hello RunAsync method in hello worker role's RoleEntryPoint-derived class.</span></span> <span data-ttu-id="38491-134">(A párhuzamosan futó hello módszerrel kapcsolatos hello figyelmeztetést figyelmen kívül.)</span><span class="sxs-lookup"><span data-stu-id="38491-134">(Ignore hello warning about hello method running synchronously.)</span></span>

        private async Task RunAsync(CancellationToken cancellationToken)
        {
            // TODO: Replace hello following with your own logic.
            while (!cancellationToken.IsCancellationRequested)
            {
                Trace.TraceInformation("Working");
                Concatenator.Concatenate(10000);
            }
        }

<span data-ttu-id="38491-135">Létrehozása és futtatása a felhőalapú szolgáltatás helyileg nélkül hibakeresés (Ctrl + F5), a hello megoldás konfigurációs készlet túl**kiadás**.</span><span class="sxs-lookup"><span data-stu-id="38491-135">Build and run your cloud service locally without debugging (Ctrl+F5), with hello solution configuration set too**Release**.</span></span> <span data-ttu-id="38491-136">Ez biztosítja, hogy minden fájl és mappa hello alkalmazást futtató helyi hoz létre, és biztosítja, hogy minden hello emulátorok vannak-e indítva.</span><span class="sxs-lookup"><span data-stu-id="38491-136">This ensures that all files and folders are created for running hello application locally, and ensures that all hello emulators are started.</span></span> <span data-ttu-id="38491-137">Indítsa el, hogy fut-e a feldolgozói szerepkör hello tálca tooverify hello Compute Emulator felhasználói felületén.</span><span class="sxs-lookup"><span data-stu-id="38491-137">Start hello Compute Emulator UI from hello taskbar tooverify that your worker role is running.</span></span>

## <a name="2-attach-tooa-process"></a><span data-ttu-id="38491-138">2: tooa folyamat csatolása</span><span class="sxs-lookup"><span data-stu-id="38491-138">2: Attach tooa process</span></span>
<span data-ttu-id="38491-139">Profilkészítési hello alkalmazást a Visual Studio 2010 IDE hello elindításával, helyett hello Profilkészítő tooa folyamatának futtatása kell csatolnia.</span><span class="sxs-lookup"><span data-stu-id="38491-139">Instead of profiling hello application by starting it from hello Visual Studio 2010 IDE, you must attach hello profiler tooa running process.</span></span> 

<span data-ttu-id="38491-140">tooattach hello Profilkészítő tooa folyamata, a hello **elemzés** menüben válasszon **Profilkészítő** és **Attach/Detach**.</span><span class="sxs-lookup"><span data-stu-id="38491-140">tooattach hello profiler tooa process, on hello **Analyze** menu, choose **Profiler** and **Attach/Detach**.</span></span>

![Profil beállítás csatolása][6]

<span data-ttu-id="38491-142">A feldolgozói szerepkör esetében hello WaWorkerHost.exe folyamat található.</span><span class="sxs-lookup"><span data-stu-id="38491-142">For a worker role, find hello WaWorkerHost.exe process.</span></span>

![WaWorkerHost folyamat][7]

<span data-ttu-id="38491-144">A projektmappa egy hálózati meghajtón van, hello Profilkészítő rákérdez tooprovide egy másik hely toosave hello jelentések adatainak összegyűjtése.</span><span class="sxs-lookup"><span data-stu-id="38491-144">If your project folder is on a network drive, hello profiler will ask you tooprovide another location toosave hello profiling reports.</span></span>

 <span data-ttu-id="38491-145">Tooa webes szerepkör is csatolhat a tooWaIISHost.exe csatolásával.</span><span class="sxs-lookup"><span data-stu-id="38491-145">You can also attach tooa web role by attaching tooWaIISHost.exe.</span></span>
<span data-ttu-id="38491-146">Ha több szerepkör folyamata az alkalmazásban, toouse hello folyamatazonosító toodistinguish kell őket.</span><span class="sxs-lookup"><span data-stu-id="38491-146">If there are multiple worker role processes in your application, you need toouse hello processID toodistinguish them.</span></span> <span data-ttu-id="38491-147">Hello folyamatazonosító programozott módon lekérdezheti hello folyamatobjektumot elérésével.</span><span class="sxs-lookup"><span data-stu-id="38491-147">You can query hello processID programmatically by accessing hello Process object.</span></span> <span data-ttu-id="38491-148">Például a kód toohello Run metódus hello RoleEntryPoint származtatott osztály szerepkör hozzáadásakor vessen egy pillantást a napló a hello Compute Emulator felhasználói felületén tooknow milyen folyamat tooconnect számára.</span><span class="sxs-lookup"><span data-stu-id="38491-148">For example, if you add this code toohello Run method of hello RoleEntryPoint-derived class in a role, you can look at the log in hello Compute Emulator UI tooknow what process tooconnect to.</span></span>

    var process = System.Diagnostics.Process.GetCurrentProcess();
    var message = String.Format("Process ID: {0}", process.Id);
    Trace.WriteLine(message, "Information");

<span data-ttu-id="38491-149">tooview hello napló, kezdő hello Compute Emulator felhasználói felületén.</span><span class="sxs-lookup"><span data-stu-id="38491-149">tooview hello log, start hello Compute Emulator UI.</span></span>

![Indítsa el a hello Compute Emulator felhasználói felületén][8]

<span data-ttu-id="38491-151">Nyissa meg a hello feldolgozói szerepkör napló konzolablak hello Compute Emulator felhasználói felületén a hello konzol ablakának címsorában kattintva.</span><span class="sxs-lookup"><span data-stu-id="38491-151">Open hello worker role log console window in hello Compute Emulator UI by clicking on hello console window's title bar.</span></span> <span data-ttu-id="38491-152">Hello Folyamatazonosító hello naplóban tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="38491-152">You can see hello process ID in hello log.</span></span>

![Nézet folyamat azonosítója][9]

<span data-ttu-id="38491-154">Egy csatolt, az alkalmazás (ha szükséges) felhasználói felület tooreproduce hello forgatókönyv hello lépéseket hajtsa végre.</span><span class="sxs-lookup"><span data-stu-id="38491-154">One you've attached, perform hello steps in your application's UI (if needed) tooreproduce hello scenario.</span></span>

<span data-ttu-id="38491-155">Ha azt szeretné, hogy toostop profilkészítési, válassza a hello **Profilkészítés leállítása** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="38491-155">When you want toostop profiling, choose hello **Stop Profiling** link.</span></span>

![Beállítás Profilkészítés leállítása][10]

## <a name="3-view-performance-reports"></a><span data-ttu-id="38491-157">3: teljesítmény jelentések megtekintése</span><span class="sxs-lookup"><span data-stu-id="38491-157">3: View performance reports</span></span>
<span data-ttu-id="38491-158">hello rendszerteljesítmény-jelentés az alkalmazás jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="38491-158">hello performance report for your application is displayed.</span></span>

<span data-ttu-id="38491-159">Ezen a ponton hello Profilkészítő végrehajtása megszakad, adatok .vsp fájlba menti, és ezek az adatok elemzése a jelentés megjeleníti.</span><span class="sxs-lookup"><span data-stu-id="38491-159">At this point, hello profiler stops executing, saves data in a .vsp file, and displays a report that shows an analysis of this data.</span></span>

![Profilkészítő jelentés][11]

<span data-ttu-id="38491-161">Ha String.wstrcpy hello jelenik meg a gyakran használt adatok elérési útja, kattintson a csak saját kód toochange hello tooshow felhasználói kód megtekintése.</span><span class="sxs-lookup"><span data-stu-id="38491-161">If you see String.wstrcpy in hello Hot Path, click on Just My Code toochange hello view tooshow user code only.</span></span>  <span data-ttu-id="38491-162">Ha String.Concat, próbálja hello összes kód megjelenítése gomb lenyomásával.</span><span class="sxs-lookup"><span data-stu-id="38491-162">If you see String.Concat, try pressing hello Show All Code button.</span></span>

<span data-ttu-id="38491-163">Hello ÖSSZEFŰZ metódus és String.Concat hello végrehajtási idő nagy részét fel kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="38491-163">You should see hello Concatenate method and String.Concat taking up a large portion of hello execution time.</span></span>

![Jelentése][12]

<span data-ttu-id="38491-165">Ebben a cikkben hozzáadott hello karakterláncot kapott kódot, ha a megjelennie hello feladatlista figyelmeztetés.</span><span class="sxs-lookup"><span data-stu-id="38491-165">If you added hello string concatenation code in this article, you should see a warning in hello Task List for this.</span></span> <span data-ttu-id="38491-166">Is megjelenik egy figyelmeztetés, hogy nincs-e aránytalanul szemétgyűjtés, amely létrehozott és használaton karakterláncok toohello száma miatt.</span><span class="sxs-lookup"><span data-stu-id="38491-166">You may also see a warning that there is an excessive amount of garbage collection, which is due toohello number of strings that are created and disposed.</span></span>

![Teljesítmény-figyelmeztetések][14]

## <a name="4-make-changes-and-compare-performance"></a><span data-ttu-id="38491-168">4: módosításokat, és hasonlítsa össze a teljesítmény</span><span class="sxs-lookup"><span data-stu-id="38491-168">4: Make changes and compare performance</span></span>
<span data-ttu-id="38491-169">Hello teljesítmény előtt és után kódváltoztatást is összehasonlíthatja.</span><span class="sxs-lookup"><span data-stu-id="38491-169">You can also compare hello performance before and after a code change.</span></span>  <span data-ttu-id="38491-170">Állítsa le a hello folyamat fut, és hello kód tooreplace hello karakterláncot kapott műveletet hello használata a StringBuilder szerkesztése:</span><span class="sxs-lookup"><span data-stu-id="38491-170">Stop hello running process, and edit hello code tooreplace hello string concatenation operation with hello use of StringBuilder:</span></span>

    public static string Concatenate(int number)
    {
        int count;
        System.Text.StringBuilder builder = new System.Text.StringBuilder("");
        for (count = 0; count < number; count++)
        {
             builder.Append("\n" + count.ToString());
        }
        return builder.ToString();
    }

<span data-ttu-id="38491-171">Ne újabb teljesítmény futtatás, majd hasonlítsa össze a hello teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="38491-171">Do another performance run, and then compare hello performance.</span></span> <span data-ttu-id="38491-172">A teljesítmény Explorer hello, ha hello futtatása vannak hello ugyanabban a munkamenetben, most is válassza ki mindkét jelentést, hello helyi menü megnyitásához, és válassza **összehasonlítása teljesítmény jelentések**.</span><span class="sxs-lookup"><span data-stu-id="38491-172">In hello Performance Explorer, if hello runs are in hello same session, you can just select both reports, open hello shortcut menu, and choose **Compare Performance Reports**.</span></span> <span data-ttu-id="38491-173">A Futtatás teljesítmény egy másik munkamenetben toocompare, nyissa meg hello **elemzés** menüben, és válassza **összehasonlítása teljesítmény jelentések**.</span><span class="sxs-lookup"><span data-stu-id="38491-173">If you want toocompare with a run in another performance session, open hello **Analyze** menu, and choose **Compare Performance Reports**.</span></span> <span data-ttu-id="38491-174">Adja meg mindkét fájlok hello párbeszédpanelt, amely akkor jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="38491-174">Specify both files in hello dialog box that appears.</span></span>

![Jelentések teljesítménybeállítását összehasonlítása][15]

<span data-ttu-id="38491-176">hello jelentések hello két futtatása közötti különbségek jelöljön ki.</span><span class="sxs-lookup"><span data-stu-id="38491-176">hello reports highlight differences between hello two runs.</span></span>

![Összehasonlító jelentés][16]

<span data-ttu-id="38491-178">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="38491-178">Congratulations!</span></span> <span data-ttu-id="38491-179">Ön a hello Profilkészítő megtette az első lépéseket.</span><span class="sxs-lookup"><span data-stu-id="38491-179">You've gotten started with hello profiler.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="38491-180">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="38491-180">Troubleshooting</span></span>
* <span data-ttu-id="38491-181">Győződjön meg arról, amelyek profilkészítési kiadott buildjét és hibakeresés nélkül indítsa el.</span><span class="sxs-lookup"><span data-stu-id="38491-181">Make sure you are profiling a Release build and start without debugging.</span></span>
* <span data-ttu-id="38491-182">Ha hello Attach/Detach beállítás nincs engedélyezve a hello Profilkészítő menüben, futtassa a hello teljesítmény varázsló.</span><span class="sxs-lookup"><span data-stu-id="38491-182">If hello Attach/Detach option is not enabled on hello Profiler menu, run hello Performance Wizard.</span></span>
* <span data-ttu-id="38491-183">Hello Compute Emulator felhasználói felületén tooview hello állapot az alkalmazás használja.</span><span class="sxs-lookup"><span data-stu-id="38491-183">Use hello Compute Emulator UI tooview hello status of your application.</span></span> 
* <span data-ttu-id="38491-184">Ha problémák alkalmazások hello emulátorban, vagy csatolása hello Profilkészítő, hello compute emulator leállítása, és indítsa újra.</span><span class="sxs-lookup"><span data-stu-id="38491-184">If you have problems starting applications in hello emulator, or attaching hello profiler, shut down hello compute emulator and restart it.</span></span> <span data-ttu-id="38491-185">Ha ez nem oldja meg a hello problémát, próbálja meg újraindítani.</span><span class="sxs-lookup"><span data-stu-id="38491-185">If that doesn't solve hello problem, try rebooting.</span></span> <span data-ttu-id="38491-186">Ez a probléma akkor fordulhat elő, ha hello Compute Emulator toosuspend használja, és távolítsa el a futó központi telepítéseket.</span><span class="sxs-lookup"><span data-stu-id="38491-186">This problem can occur if you use hello Compute Emulator toosuspend and remove running deployments.</span></span>
* <span data-ttu-id="38491-187">Ha a parancsok parancssorból profilkészítési hello már használta, különösen a globális beállítások hello, győződjön meg arról, hogy VSPerfClrEnv /globaloff hívása történt, és hogy VsPerfMon.exe le lett állítva.</span><span class="sxs-lookup"><span data-stu-id="38491-187">If you have used any of hello profiling commands from the command line, especially hello global settings, make sure that VSPerfClrEnv /globaloff has been called and that VsPerfMon.exe has been shut down.</span></span>
* <span data-ttu-id="38491-188">Ha hello üzenet jelenik meg, amikor a mintavételi, "PRF0025: nem történt adatgyűjtés,", ellenőrizze, hogy a folyamat toohas CPU tevékenység csatolt hello.</span><span class="sxs-lookup"><span data-stu-id="38491-188">If when sampling, you see hello message "PRF0025: No data was collected," check that hello process you attached toohas CPU activity.</span></span> <span data-ttu-id="38491-189">Előfordulhat, hogy alkalmazásokat, amelyek nem tűnik, hogy minden számítási munka tudott mintavételi adatokat.</span><span class="sxs-lookup"><span data-stu-id="38491-189">Applications that are not doing any computational work might not produce any sampling data.</span></span>  <span data-ttu-id="38491-190">Lehetőség arra is, hogy hello a folyamat kilépett a előtt minden olyan végezhető el.</span><span class="sxs-lookup"><span data-stu-id="38491-190">It's also possible that hello process exited before any sampling was done.</span></span> <span data-ttu-id="38491-191">Ellenőrizze a toosee, amely egy szerepkörhöz vannak profilkészítési hello Run metódus nem szünteti meg.</span><span class="sxs-lookup"><span data-stu-id="38491-191">Check toosee that hello Run method for a role that you are profiling does not terminate.</span></span>

## <a name="next-steps"></a><span data-ttu-id="38491-192">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="38491-192">Next Steps</span></span>
<span data-ttu-id="38491-193">Hello Visual Studio Profilkészítő nem támogatja az Azure bináris hello emulátorban tagolása, de ha tootest memóriafoglalás, is válassza ezt a beállítást, ha a profilkészítési.</span><span class="sxs-lookup"><span data-stu-id="38491-193">Instrumenting Azure binaries in hello emulator is not supported in hello Visual Studio profiler, but if you want tootest memory allocation, you can choose that option when profiling.</span></span> <span data-ttu-id="38491-194">Dönthet úgy is párhuzamossági profilkészítési, segítségével határozza meg, hogy szálak jelentős időt használják a zárolások vannak, vagy profilkészítési interakció, réteg segítségével nyomon követésük Ez teljesítményproblémákat okozhat, ha a fizetősökbe, az alkalmazások közötti kommunikáció során leggyakrabban gyakran közötti hello adatszint és egy feldolgozói szerepkört.</span><span class="sxs-lookup"><span data-stu-id="38491-194">You can also choose concurrency profiling, which helps you determine whether threads are wasting time competing for locks, or tier interaction profiling, which helps you track down performance problems when interacting between tiers of an application, most frequently between hello data tier and a worker role.</span></span>  <span data-ttu-id="38491-195">Amely az alkalmazás létrehozza hello adatbázis-lekérdezések megtekintése és a profilkészítési adatokat tooimprove hello adatbázis használatára hello használata.</span><span class="sxs-lookup"><span data-stu-id="38491-195">You can view hello database queries that your app generates and use hello profiling data tooimprove your use of hello database.</span></span> <span data-ttu-id="38491-196">Réteg interakció profilkészítési kapcsolatos információkért lásd a hello blogbejegyzésben [forgatókönyv: a Visual Studio Team System 2010 réteg interakció Profiler használatával hello][3].</span><span class="sxs-lookup"><span data-stu-id="38491-196">For information about tier interaction profiling, see hello blog post [Walkthrough: Using hello Tier Interaction Profiler in Visual Studio Team System 2010][3].</span></span>

[1]: http://msdn.microsoft.com/library/azure/hh369930.aspx
[2]: http://msdn.microsoft.com/library/azure/hh411542.aspx
[3]: http://blogs.msdn.com/b/habibh/archive/2009/06/30/walkthrough-using-the-tier-interaction-profiler-in-visual-studio-team-system-2010.aspx
[4]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally09.png
[5]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally10.png
[6]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally02.png
[7]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally05.png
[8]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally010.png
[9]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally07.png
[10]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally06.png
[11]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally03.png
[12]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally011.png
[14]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally04.png 
[15]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally013.png
[16]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally012.png
[17]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally08.png
