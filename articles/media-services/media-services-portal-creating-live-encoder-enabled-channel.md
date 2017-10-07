---
title: "aaaHow tooperform élő adatfolyam-Azure Media Services toocreate többféle sávszélességű adatfolyamok hello Azure-portál használatával |} Microsoft Docs"
description: "Az oktatóanyag bemutatja, hogyan hello lépéseit egy olyan csatorna létrehozásának egy egyféle sávszélességű élő streamet és hello Azure-portál használatával toomulti sávszélességűvé kódolja."
services: media-services
documentationcenter: 
author: anilmur
manager: cfowler
editor: 
ms.assetid: 504f74c2-3103-42a0-897b-9ff52f279e23
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/09/2017
ms.author: juliako
ms.openlocfilehash: 963a25b8ba4683a2ce34d9fb0e19499874b4707c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooperform-live-streaming-using-azure-media-services-toocreate-multi-bitrate-streams-with-hello-azure-portal"></a>Hogyan tooperform élő adatfolyam-használó Azure Media Services toocreate többszörös sávszélességű adatfolyamokat a hello Azure-portálon
> [!div class="op_single_selector"]
> * [Portal](media-services-portal-creating-live-encoder-enabled-channel.md)
> * [.NET](media-services-dotnet-creating-live-encoder-enabled-channel.md)
> * [REST API](https://docs.microsoft.com/rest/api/media/operations/channel)
> 
> 

Ez az oktatóanyag végigvezeti hello létrehozásának egy **csatorna** , amely egy egyszeres sávszélességű élő streamet és toomulti sávszélességűvé kódolja.

> [!NOTE]
> További információ az élő kódolás engedélyezett kapcsolódó tooChannels lásd [használó Azure Media Services toocreate többféle sávszélességű adatfolyamot élő Stream továbbítása](media-services-manage-live-encoder-enabled-channels.md).
> 
> 

## <a name="common-live-streaming-scenario"></a>Gyakori élő adatfolyam-továbbítási forgatókönyv
Az alábbiakban hello általános lépéseket streamelő alkalmazásokat létrehozni.

> [!NOTE]
> Maximális hello élő esemény időtartama ajánlott 8 órára is jelenleg. Ha hosszabb ideig kell toorun egy csatornát, forduljon amslived@Microsoft.com e-mail címen.
> 
> 

1. Csatlakozás egy videokamerát tooa számítógép. Indítson el és konfiguráljon egy helyszíni élő kódoló, amely a kimenetre küldheti egyféle sávszélességű adatfolyamot hello a következő protokollok valamelyikével: RTMP, Smooth Streaming vagy RTP (MPEG-TS). További tudnivalók: [Azure Media Services RMTP-támogatása és valós idejű kódolók](http://go.microsoft.com/fwlink/?LinkId=532824)
   
    Ezt a lépést a csatorna létrehozása után is elvégezheti.
2. Hozzon létre és indítson el egy csatornát. 
3. Kérje le hello csatorna feldolgozó URL-CÍMÉT. 
   
    hello betöltési URL-cím hello élő kódoló toosend hello adatfolyam toohello csatorna által használt.
4. Hello csatorna előnézeti URL-cím beolvasása. 
   
    Az URL-cím tooverify, hogy a csatornája megfelelően fogadja-hello élő adatfolyam használata.
5. Hozzon létre egy eseményt/programot (ezzel egy objektumot is létrehoz). 
6. Tegye közzé hello esemény (által létrehozandó hello társított objektumhoz tartozó OnDemand-lokátort).    
7. Amikor készen áll a toostart streamelésre és az archiválásra hello esemény indítása.
8. Másik lehetőségként hello élő kódoló jelzett toostart hirdetést is lehet. hello hirdetés bekerül hello kimeneti adatfolyam.
9. Állítsa le a hello eseményt, ha azt szeretné, hogy toostop streamelésre és az archiválásra hello esemény.
10. Hello esemény törlése (és választhatóan a hello eszköz törlése).   

## <a name="in-this-tutorial"></a>Az oktatóanyag tartalma
Ebben az oktatóanyagban hello Azure-portálon használt tooaccomplish hello a következő feladatokat: 

1. Hozzon létre egy csatornát, amely engedélyezett tooperform élő kódolás.
2. Get hello betöltési URL-címet a rendelés toosupply azt toolive kódoló. hello élő kódoló hello csatorna be fogja használni az URL-cím tooingest hello adatfolyam.
3. Egy esemény/program (és egy objektum) létrehozása.
4. Hello objektum közzététele, és a streamelési URL-címek lekérése.  
5. Tartalom lejátszása
6. Tisztítás.

## <a name="prerequisites"></a>Előfeltételek
Az alábbiakban hello szükséges toocomplete hello oktatóanyag.

* toocomplete ebben az oktatóanyagban egy Azure-fiókra van szüksége. Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot. 
  További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).
* Egy Media Services-fiók. egy Media Services-fiók toocreate lásd [fiók létrehozása](media-services-portal-create-account.md).
* Egy webkamera és egy egyféle sávszélességű élő adatfolyamot küldő kódoló.

## <a name="create-a-channel"></a>Csatorna létrehozása
1. A hello [Azure-portálon](https://portal.azure.com/)válassza ki a Media Services, majd kattintson a a Media Services-fiók neve.
2. Válassza a **Live Streaming** (Élő adatfolyam) lehetőséget.
3. Válassza a **Custom create** (Egyéni létrehozás) lehetőséget. Ez a beállítás lehetővé teszi egy olyan csatorna létrehozását, amellyel használható a Live Encoding.
   
    ![Csatorna létrehozása](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-create-channel.png)
4. Kattintson a **Settings** (Beállítások) lehetőségre.
   
   1. Válassza ki a hello **élő kódolás** csatornatípus. Ez azt határozza meg, amelyet az toocreate egy csatornát, amely engedélyezve van a valós idejű kódolásra. Hogy azt jelenti, hogy hello bejövő egyszeres sávszélességű adatfolyamot küldött toohello csatorna és a megadott élő kódoló beállításai többszörös sávszélességű streammé kódolja. További információkért lásd: [használó Azure Media Services toocreate többféle sávszélességű adatfolyamot élő Stream továbbítása](media-services-manage-live-encoder-enabled-channels.md). Kattintson az OK gombra.
   2. Adja meg a csatorna nevét.
   3. Kattintson az OK gombra a üdvözlő képernyőt hello alján.
5. Jelölje be hello **bemeneti** fülre.
   
   1. Ezen a lapon választhat egy streamprotokollt. A hello **élő kódolás** csatornatípust, érvényes opciók a következők:
      
      * Single bitrate Fragmented MP4 (Egyszeres sávszélességű, fragmentált MP4) (Smooth Streaming)
      * Single bitrate RTMP (Egyszeres sávszélességű RTMP)
      * RTP (MPEG-TS): MPEG-2 Transport Stream over RTP (MPEG-2 adatátviteli stream RTP fölött)
        
        Minden protokoll kapcsolatos részletes ismertetése [használó Azure Media Services toocreate többféle sávszélességű adatfolyamot élő Stream továbbítása](media-services-manage-live-encoder-enabled-channels.md).
        
        Hello protokollbeállítás hello csatorna közben nem módosítható, vagy a kapcsolódó események/programok már elindultak. Ha eltérő protokollok használatára van szükség, hozzon létre külön-külön csatornákat az egyes streamprotokollokhoz.  
   2. IP-korlátozási alkalmazhatja hello betöltési. 
      
       Meghatározhatja a hello IP címeket engedélyezett tooingest videó toothis csatornát. Az engedélyezett IP-címek köre tartalmazhat egyetlen IP-címet (például „10.0.0.1”), vagy egy IP-tartományt, amelyet egy IP-cím és egy CIDR alhálózati maszk segítségével (például „10.0.0.1/22”), vagy egy IP-cím és egy pontozott decimális alhálózati maszk (például „10.0.0.1(255.255.252.0)”) segítségével lehet megadni.
      
       Ha nem ad meg IP-címeket, és nem határoz meg szabálydefiníciót, a rendszer egyetlen IP-címet sem engedélyez. tooallow IP-címeket, hozzon létre egy szabályt, és állítsa be a 0.0.0.0/0.
6. A hello **előzetes** lapra, IP-korlátozási alkalmazni a hello Preview-ban.
7. A hello **kódolás** lapra, adja meg a hello kódolási beállításkészlet. 
   
    Jelenleg a rendszer csak hello készletet választhatja van **alapértelmezett 720p**. egyéni toospecify készletet, nyissa meg a Microsoft támogatási jegy. Ezután írja be a hello hello neve létrehozza a készletet. 

> [!NOTE]
> Hello csatorna indítása jelenleg too30 percig is eltarthat. A csatorna újraindítása too5 percig is eltarthat.
> 
> 

Hello csatorna létrehozása után kattintson a hello csatornán, és válassza ki **beállítások** ahol megtekintheti a csatornakonfigurációkat. 

További információkért lásd: [használó Azure Media Services toocreate többféle sávszélességű adatfolyamot élő Stream továbbítása](media-services-manage-live-encoder-enabled-channels.md).

## <a name="get-ingest-urls"></a>A betöltési URL-címek beolvasása
Hello csatorna létrehozása után kaphat a betöltési URL-címeket adja meg az élő kódoló toohello. a kódoló hello ezen URL-címek tooinput élő adatfolyam.

![betöltési URL-címek](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-ingest-urls.png)

## <a name="create-and-manage-events"></a>Események létrehozása és kezelése
### <a name="overview"></a>Áttekintés
A csatorna egy események/programok, amelyek lehetővé teszik a toocontrol hello közzétételét és tárolását élő stream szegmenseinek társítva. Az eseményeket/programokat a csatornák kezelik. hello csatornák és programok viszonya nagyon hasonló tootraditional media, ahol egy csatornát állandó adatfolyam tartalom és a program túllépte az időkorlátot hatókörön belüli toosome esemény adott csatornán.

Megadhatja a hello óraszámon belül hello esemény hello beállítása kívánt tooretain rögzített hello tartalom **archiválási időtartammal** hossza. Ez az érték 5 perc tooa legfeljebb 25 óra közötti állítható be. Az archiválási időtartam is határozzák meg, hogy a hello maximális időtartam ügyfeleket is kérhet időben hello aktuális élő pozíciótól. Események hosszabbak lehetnek hello megadott időtartamig, de a rendszer folyamatosan elveti azokat a tartalmakat, amelyek korábbiak a hello időtartamnál. Ez a tulajdonság értékének is meghatározza, hogy mennyi ideig hello ügyfél jegyzékfájljai milyen mértékben növelhető.

Minden esemény egy objektumhoz van társítva. létre kell hoznia egy OnDemand-lokátort hello toopublish hello esemény társított eszköz. Ez a lokátor lehetővé teszi a toobuild tooyour ügyfeleknek biztosítani tudják adatfolyam-továbbítási URL-címet.

Egy csatorna legfeljebb események egyidejűleg fut, így létrehozhat több archívumot hello toothree támogat egy bejövő streamből. Ez lehetővé teszi toopublish kapcsolatos és archiválási különböző részei egy eseményt, igény szerint. Például az üzleti igény szerint tooarchive egy esemény, de toobroadcast 6 óra csak az elmúlt 10 perc. tooaccomplish, kell toocreate két egyidejűleg futó esemény. Egy esemény értéke tooarchive hello esemény 6 óra, de hello program nincs közzétéve. hello más esemény set tooarchive 10 percig, és a program közzé van téve.

A meglévő programokat nem szabad új eseményekhez ismét felhasználni. Ehelyett inkább hozzon létre új programokat az új eseményekhez.

Amikor készen áll a toostart streamelésre és az archiválásra, indítsa el az eseményt/programot. Állítsa le a hello eseményt, ha azt szeretné, hogy toostop streamelésre és az archiválásra hello esemény. 

toodelete archivált tartalmat, állítsa le és hello esemény törlése, és törölje a hozzá társított objektumot hello. Egy eszköz nem törölhető, ha használják a hello esemény; hello esemény először törölni kell. 

Állítsa le és a hello esemény törlése után is hello felhasználók állapotban tud toostream az archivált tartalmakat, igény szerint lekért videóként mindaddig, amíg nem törli hello eszköz.

Ha szeretné tooretain hello archivált tartalom, de nem rendelkezik az adatfolyamként történő elérhetővé, törölje a streamelési locator hello.

### <a name="createstartstop-events"></a>Események létrehozása/indítása/leállítása
Miután hello adatfolyam hello csatorna összekapcsolását megkezdheti hello adatfolyamként történő esemény létrehozása egy eszköz, a Program és a Streamelési Lokátort. Ezzel archiválja hello adatfolyam és hello Streaming Endpoint keresztül elérhető tooviewers teszik. 

>[!NOTE]
>Az AMS-fiók létrehozásakor egy **alapértelmezett** adatfolyam-továbbítási végpontra tooyour fiók kerül hello **leállítva** állapotát. a dinamikus csomagolás és a dinamikus titkosítás tartalmat, és hajtsa végre a megfelelő előnyeit streaming toostart hello streamvégpontra, amelyből el kívánja toostream tartalom toobe rendelkezik hello **futtató** állapotát. 

Két módon toostart esemény történik: 

1. A hello **csatorna** lapján nyomja le az ENTER **élő esemény** tooadd új esemény.
   
    Adja meg az esemény nevét, az objektum nevét, az archiválás időtartamát és a titkosítási beállítást.
   
    ![program létrehozása](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-create-program.png)
   
    Ha nem törli **közzéteszi az élő esemény** be van jelölve, hello esemény hello KÖZZÉTÉTELI URL-címeket a rendszer létrehozza.
   
    Lenyomhatja **Start**, készen áll a toostream hello esemény áll.
   
    Hello esemény elindítását követően lenyomhatja **figyelési** hello tartalom lejátszása toostart.
2. Alternatív megoldásként használhatja a helyi és nyomja le az **élő** hello gombjára **csatorna** lap. Ekkor létrejön az alapértelmezett objektum, program és streamelési lokátor.
   
    hello esemény nevű **alapértelmezett** hello archiválási időtartamra van beállítva, és too8 óra.

Közzétett eseményadatok hello hello a figyelheti **élő esemény** lap. 

Az **Off Air** (Közvetítés leállítása) gombbal az összes élő eseményt leállíthatja. 

## <a name="watch-hello-event"></a>A figyelési hello esemény
toowatch hello esemény, kattintson a **figyelési** a streamelési URL-cím az Azure-portál vagy a példány hello hello és a Windows Media Player az Ön által választott. 

![Létrehozva](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-play-event.png)

Az élő esemény automatikusan átalakítja események tooon igény szerinti tartalomterjesztésről leállításakor.

## <a name="clean-up"></a>A fölöslegessé vált elemek eltávolítása
Ha befejezte az esemény streamelését és tooclean mentése hello korábban kiosztott erőforrásokat, hajtsa végre az eljárást követő hello.

* Állítsa le hello adatfolyam a hello kódoló.
* Hello csatornájának leállítására. Hello csatorna leállítását követően nem számítunk a díjakat. Ha toostart kell azt újra, hogy fog rendelkezni hello azonos feldolgozó URL-CÍMÉT, akkor nem kell tooreconfigure a kódoló.
* A Streamvégpontot, leállíthatja, kivéve, ha azt szeretné, hogy toocontinue tooprovide hello archív az élő esemény egy igény szerinti adatfolyamként. Hello csatorna leállított állapotban van, nem számítunk fel díjakat.

## <a name="view-archived-content"></a>Archivált tartalom megtekintése
Állítsa le és a hello esemény törlése után is hello felhasználók állapotban tud toostream az archivált tartalmakat, igény szerint lekért videóként mindaddig, amíg nem törli hello eszköz. Egy eszköz nem törölhető, ha egy esemény; használják hello esemény először törölni kell. 

toomanage az objektumok kiválasztása **beállítás** kattintson **eszközök**.

![Objektumok](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-assets.png)

## <a name="considerations"></a>Megfontolandó szempontok
* Maximális hello élő esemény időtartama ajánlott 8 órára is jelenleg. Ha hosszabb ideig kell toorun egy csatornát, forduljon amslived@Microsoft.com e-mail címen.
* Ügyeljen arra, hogy streamvégpontra, amelyből el kívánja toostream a tartalom hello hello **futtató** állapotát.

## <a name="next-step"></a>Következő lépés
Tekintse át a Media Services képzési terveket.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

