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
# <a name="testing-hello-performance-of-a-cloud-service-locally-in-hello-azure-compute-emulator-using-hello-visual-studio-profiler"></a>Hello Azure Compute Emulator használatával hello Visual Studio Profilkészítő hello teljesítményét egy felhőalapú szolgáltatás helyi tesztelése
Számos különféle eszközöket és technikákat felhőszolgáltatások hello teljesítményének teszteléséhez érhetők el.
Egy felhőalapú szolgáltatás tooAzure közzétételekor a profilkészítési adatokat gyűjteni, és majd elemezze a Visual Studio lehet a helyi [Azure alkalmazás profilkészítési][1].
Használhatja diagnosztika tootrack teljesítményszámlálók teljesítmény számos, a [teljesítményszámlálók segítségével az Azure-ban][2].
Érdemes lehet tooprofile az alkalmazás helyi a hello compute emulator toohello felhő üzembe helyezése előtt.

Ez a cikk ismerteti a hello CPU mintavételi metódusában profilkészítési, amely helyileg végezhető hello emulátorban. CPU mintavételi egy metódust, amely profilkészítési nem nagyon zavaró. A kijelölt mintavételi történik hello Profilkészítő pillanatképet készít a hello hívási verem. hello adatok gyűjtése egy meghatározott időtartamra vonatkozóan, és a jelentésben látható. Ez a módszer a profilkészítési általában tooindicate, ahol számításilag intenzív alkalmazásban hello CPU munka nagyobb része történik-e.  Ezáltal hello lehetőség toofocus útvonalon hello"Forró" Ha az alkalmazás van kell hello legtöbb időt.

## <a name="1-configure-visual-studio-for-profiling"></a>1: profilkészítési a Visual Studio konfigurálása
Először néhány Visual Studio konfigurálására lehetőség áll rendelkezésre, előfordulhat, hogy lehet hasznos, amikor a profilkészítési. toomake értelmében hello profilkészítési jelentések, az alkalmazás és a is rendszer könyvtárak szimbólumait lesz szüksége a szimbólumok (.pdb fájlok). Érdemes toomake meg arról, hogy a hello elérhető szimbólum kiszolgálók hivatkoznak. toodo Ez a hello **eszközök** Visual Studio menüjében válassza a **beállítások**, majd válassza a **hibakeresés**, majd **szimbólumok**. Győződjön meg arról, hogy a Microsoft szimbólum kiszolgálók megtalálható-e **szimbólum (.pdb) fájlhelyek**.  Http://referencesource.microsoft.com/symbols, amely lehet További szimbólumfájlok is hivatkozhat.

![Szimbólum beállításai][4]

Ha szükséges, egyszerűbbé teheti az hello jelenti, hogy hello Profilkészítő hoz létre úgy, hogy csak saját kód. Csak saját kód engedélyezve van a függvény hívási verem egyszerűsítettek, így teljesen belső toolibraries meghív és .NET-keretrendszer hello rejtve maradnak az hello jelentések. A hello **eszközök** menüben válasszon **beállítások**. Bontsa ki a hello **teljesítmény eszközök** csomópont, és válassza a **általános**. Válassza ki a hello jelölőnégyzetét **engedélyezése csak saját kód Profilkészítő jelentések**.

![Csak saját kód beállítása][17]

Ezek az utasítások is használhatja, egy meglévő projektjébe vagy egy új projektet.  Ha az alább ismertetett technikák hoz létre egy új projekt tootry hello, válasszon egy C# **Azure Cloud Service** projektre, és válassza ki a **webes szerepkör** és egy **feldolgozói szerepkör**.

![Azure Cloud Service projekt szerepkörök][5]

Például céljából, adjon hozzá néhány kódot tooyour projekt, amely sok időt vesz igénybe, és bemutatja a nyilvánvaló teljesítmény kapcsolatos problémára. Például adja hozzá a következő kód tooa feldolgozói szerepkör projekt hello:

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

Ez a kód hívja hello RunAsync metódusában hello feldolgozói szerepkör RoleEntryPoint származtatott osztályban. (A párhuzamosan futó hello módszerrel kapcsolatos hello figyelmeztetést figyelmen kívül.)

        private async Task RunAsync(CancellationToken cancellationToken)
        {
            // TODO: Replace hello following with your own logic.
            while (!cancellationToken.IsCancellationRequested)
            {
                Trace.TraceInformation("Working");
                Concatenator.Concatenate(10000);
            }
        }

Létrehozása és futtatása a felhőalapú szolgáltatás helyileg nélkül hibakeresés (Ctrl + F5), a hello megoldás konfigurációs készlet túl**kiadás**. Ez biztosítja, hogy minden fájl és mappa hello alkalmazást futtató helyi hoz létre, és biztosítja, hogy minden hello emulátorok vannak-e indítva. Indítsa el, hogy fut-e a feldolgozói szerepkör hello tálca tooverify hello Compute Emulator felhasználói felületén.

## <a name="2-attach-tooa-process"></a>2: tooa folyamat csatolása
Profilkészítési hello alkalmazást a Visual Studio 2010 IDE hello elindításával, helyett hello Profilkészítő tooa folyamatának futtatása kell csatolnia. 

tooattach hello Profilkészítő tooa folyamata, a hello **elemzés** menüben válasszon **Profilkészítő** és **Attach/Detach**.

![Profil beállítás csatolása][6]

A feldolgozói szerepkör esetében hello WaWorkerHost.exe folyamat található.

![WaWorkerHost folyamat][7]

A projektmappa egy hálózati meghajtón van, hello Profilkészítő rákérdez tooprovide egy másik hely toosave hello jelentések adatainak összegyűjtése.

 Tooa webes szerepkör is csatolhat a tooWaIISHost.exe csatolásával.
Ha több szerepkör folyamata az alkalmazásban, toouse hello folyamatazonosító toodistinguish kell őket. Hello folyamatazonosító programozott módon lekérdezheti hello folyamatobjektumot elérésével. Például a kód toohello Run metódus hello RoleEntryPoint származtatott osztály szerepkör hozzáadásakor vessen egy pillantást a napló a hello Compute Emulator felhasználói felületén tooknow milyen folyamat tooconnect számára.

    var process = System.Diagnostics.Process.GetCurrentProcess();
    var message = String.Format("Process ID: {0}", process.Id);
    Trace.WriteLine(message, "Information");

tooview hello napló, kezdő hello Compute Emulator felhasználói felületén.

![Indítsa el a hello Compute Emulator felhasználói felületén][8]

Nyissa meg a hello feldolgozói szerepkör napló konzolablak hello Compute Emulator felhasználói felületén a hello konzol ablakának címsorában kattintva. Hello Folyamatazonosító hello naplóban tekintheti meg.

![Nézet folyamat azonosítója][9]

Egy csatolt, az alkalmazás (ha szükséges) felhasználói felület tooreproduce hello forgatókönyv hello lépéseket hajtsa végre.

Ha azt szeretné, hogy toostop profilkészítési, válassza a hello **Profilkészítés leállítása** hivatkozásra.

![Beállítás Profilkészítés leállítása][10]

## <a name="3-view-performance-reports"></a>3: teljesítmény jelentések megtekintése
hello rendszerteljesítmény-jelentés az alkalmazás jelenik meg.

Ezen a ponton hello Profilkészítő végrehajtása megszakad, adatok .vsp fájlba menti, és ezek az adatok elemzése a jelentés megjeleníti.

![Profilkészítő jelentés][11]

Ha String.wstrcpy hello jelenik meg a gyakran használt adatok elérési útja, kattintson a csak saját kód toochange hello tooshow felhasználói kód megtekintése.  Ha String.Concat, próbálja hello összes kód megjelenítése gomb lenyomásával.

Hello ÖSSZEFŰZ metódus és String.Concat hello végrehajtási idő nagy részét fel kell megjelennie.

![Jelentése][12]

Ebben a cikkben hozzáadott hello karakterláncot kapott kódot, ha a megjelennie hello feladatlista figyelmeztetés. Is megjelenik egy figyelmeztetés, hogy nincs-e aránytalanul szemétgyűjtés, amely létrehozott és használaton karakterláncok toohello száma miatt.

![Teljesítmény-figyelmeztetések][14]

## <a name="4-make-changes-and-compare-performance"></a>4: módosításokat, és hasonlítsa össze a teljesítmény
Hello teljesítmény előtt és után kódváltoztatást is összehasonlíthatja.  Állítsa le a hello folyamat fut, és hello kód tooreplace hello karakterláncot kapott műveletet hello használata a StringBuilder szerkesztése:

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

Ne újabb teljesítmény futtatás, majd hasonlítsa össze a hello teljesítményét. A teljesítmény Explorer hello, ha hello futtatása vannak hello ugyanabban a munkamenetben, most is válassza ki mindkét jelentést, hello helyi menü megnyitásához, és válassza **összehasonlítása teljesítmény jelentések**. A Futtatás teljesítmény egy másik munkamenetben toocompare, nyissa meg hello **elemzés** menüben, és válassza **összehasonlítása teljesítmény jelentések**. Adja meg mindkét fájlok hello párbeszédpanelt, amely akkor jelenik meg.

![Jelentések teljesítménybeállítását összehasonlítása][15]

hello jelentések hello két futtatása közötti különbségek jelöljön ki.

![Összehasonlító jelentés][16]

Gratulálunk! Ön a hello Profilkészítő megtette az első lépéseket.

## <a name="troubleshooting"></a>Hibaelhárítás
* Győződjön meg arról, amelyek profilkészítési kiadott buildjét és hibakeresés nélkül indítsa el.
* Ha hello Attach/Detach beállítás nincs engedélyezve a hello Profilkészítő menüben, futtassa a hello teljesítmény varázsló.
* Hello Compute Emulator felhasználói felületén tooview hello állapot az alkalmazás használja. 
* Ha problémák alkalmazások hello emulátorban, vagy csatolása hello Profilkészítő, hello compute emulator leállítása, és indítsa újra. Ha ez nem oldja meg a hello problémát, próbálja meg újraindítani. Ez a probléma akkor fordulhat elő, ha hello Compute Emulator toosuspend használja, és távolítsa el a futó központi telepítéseket.
* Ha a parancsok parancssorból profilkészítési hello már használta, különösen a globális beállítások hello, győződjön meg arról, hogy VSPerfClrEnv /globaloff hívása történt, és hogy VsPerfMon.exe le lett állítva.
* Ha hello üzenet jelenik meg, amikor a mintavételi, "PRF0025: nem történt adatgyűjtés,", ellenőrizze, hogy a folyamat toohas CPU tevékenység csatolt hello. Előfordulhat, hogy alkalmazásokat, amelyek nem tűnik, hogy minden számítási munka tudott mintavételi adatokat.  Lehetőség arra is, hogy hello a folyamat kilépett a előtt minden olyan végezhető el. Ellenőrizze a toosee, amely egy szerepkörhöz vannak profilkészítési hello Run metódus nem szünteti meg.

## <a name="next-steps"></a>Következő lépések
Hello Visual Studio Profilkészítő nem támogatja az Azure bináris hello emulátorban tagolása, de ha tootest memóriafoglalás, is válassza ezt a beállítást, ha a profilkészítési. Dönthet úgy is párhuzamossági profilkészítési, segítségével határozza meg, hogy szálak jelentős időt használják a zárolások vannak, vagy profilkészítési interakció, réteg segítségével nyomon követésük Ez teljesítményproblémákat okozhat, ha a fizetősökbe, az alkalmazások közötti kommunikáció során leggyakrabban gyakran közötti hello adatszint és egy feldolgozói szerepkört.  Amely az alkalmazás létrehozza hello adatbázis-lekérdezések megtekintése és a profilkészítési adatokat tooimprove hello adatbázis használatára hello használata. Réteg interakció profilkészítési kapcsolatos információkért lásd a hello blogbejegyzésben [forgatókönyv: a Visual Studio Team System 2010 réteg interakció Profiler használatával hello][3].

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
