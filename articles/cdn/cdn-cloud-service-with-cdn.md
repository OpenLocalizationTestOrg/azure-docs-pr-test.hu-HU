---
title: "egy Azure felhőszolgáltatást Azure CDN aaaIntegrate |} Microsoft Docs"
description: "Ismerje meg, hogyan toodeploy egy felhőalapú szolgáltatás, amely tartalmat szolgáltat az integrált Azure CDN-végpont"
services: cdn, cloud-services
documentationcenter: .net
author: zhangmanling
manager: erikre
editor: tysonn
ms.assetid: b3c0108f-9ec5-43a8-8fd0-40eafbd32637
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: f20d60b0b5edc133adf06d010633a15f62e2b8de
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="intro"></a>Egy felhőalapú szolgáltatás integrálása az Azure CDN szolgáltatás használata
Egy felhőalapú szolgáltatás Azure CDN minden tartalom hello felhőalapú szolgáltatás helyről integrálható. A módszer ad meg a következő előnyök hello:

* Egyszerűen regisztrálhat és lemezképek, parancsprogramok és a felhőszolgáltatás-projekt címtárakban stíluslapok frissítése
* Könnyedén frissíthet a felhőalapú szolgáltatás, például a jQuery vagy rendszerindítási verzió a hello NuGet-csomagok
* A webes alkalmazás kezelése és a CDN-kiszolgált tartalmat összes hello ugyanaz a Visual Studio felület
* A webes alkalmazás és a CDN-kiszolgált tartalom egyesített telepítési munkafolyamat
* ASP.NET kötegelése és minification integrálása az Azure CDN szolgáltatás használata

## <a name="what-you-will-learn"></a>Amiről tanulni fog
Ebből az oktatóanyagból megtudhatja, hogyan:

* [Az Azure CDN-végpont integrálható a felhőalapú szolgáltatás, és a statikus tartalmat szolgáltat a weblapok Azure CDN](#deploy)
* [Adja meg a statikus tartalom gyorsítótár beállításait a felhőalapú szolgáltatás](#caching)
* [Az Azure CDN keresztül vezérlő műveletekből tartalmat](#controller)
* [Szolgálnak kötegelve, és Azure CDN tartalom minified hello parancsprogram-hibakeresés a Visual Studio élmény megőrzésével](#bundling)
* [Konfigurálhatja tartalék CSS és parancsfájlok, amikor az Azure CDN offline állapotban.](#fallback)

## <a name="what-you-will-build"></a>Mit fog létrehozni
Fogja telepíteni egy felhőalapú szolgáltatás webes szerepkör hello alapértelmezett ASP.NET MVC-sablont használ, kép, a tartományvezérlő műveleti eredmény és hello alapértelmezett JavaScript és CSS fájlok, például egy integrált Azure CDN kód tooserve tartalom hozzáadása és is írhatnak a kód tooconfigure hello tartalék mechanizmus hello eseményben kiszolgált adott CDN hello csomagok offline állapotban.

## <a name="what-you-will-need"></a>Mit kell
Ez az oktatóanyag a következő előfeltételek hello rendelkezik:

* Az aktív [Microsoft Azure-fiók](/account/)
* A Visual Studio 2015 [Azure SDK](http://go.microsoft.com/fwlink/?linkid=518003&clcid=0x409)

> [!NOTE]
> Ez az oktatóanyag kell egy Azure-fiók toocomplete:
> 
> * Is [szabad nyissa meg az Azure-fiók](https://azure.microsoft.com/pricing/free-trial/) -jóváírásokat kap használhatja ki tootry fizetős Azure-szolgáltatásokat, és még azok lejárta után is megtarthatja hello fiókot és használhatja az ingyenes Azure-szolgáltatások, például webhelyekhez.
> * Is [aktiválhatja MSDN-előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) -az MSDN-előfizetés biztosít Önnek krediteket minden hónapban, fizetős Azure-szolgáltatásokat is használhat.
> 
> 

<a name="deploy"></a>

## <a name="deploy-a-cloud-service"></a>A felhőalapú szolgáltatás telepítése
Ebben a szakaszban fogja telepíteni hello alapértelmezett ASP.NET MVC alkalmazássablon a Visual Studio 2015 tooa felhőalapú szolgáltatás webes szerepkör, és integrálhatja egy új CDN-végponthoz. Kövesse az alábbi hello utasításokat:

1. A Visual Studio 2015, hozzon létre egy új Azure-felhőszolgáltatásban hello menüsorában túl címen**fájl > Új > Projekt > felhő > Azure Cloud Service**. Adjon neki egy nevet, és kattintson a **OK**.
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-1-new-project.PNG)
2. Válassza ki **ASP.NET webes szerepkör** hello kattintson  **>**  gombra. Kattintson az OK gombra.
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-2-select-role.PNG)
3. Válassza ki **MVC** kattintson **OK**.
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-3-mvc-template.PNG)
4. Most tegye közzé a webes szerepkör tooan Azure felhőszolgáltatást. Kattintson a jobb gombbal a hello felhőszolgáltatás-projekt, és válassza ki **közzététel**.
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-4-publish-a.png)
5. Ha még nem írta be a Microsoft Azure, kattintson a hello **fiók hozzáadása...**  legördülő menüből, majd kattintson a hello **vegyen fel egy fiókot** menüpont.
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-5-publish-signin.png)
6. Hello bejelentkezési oldal, a bejelentkezés hello tooactivate az Azure-fiókjával használt Microsoft-fiók.
7. Ha be van jelentkezve, kattintson **következő**.
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-6-publish-signedin.png)
8. Feltételezve, hogy egy felhőalapú szolgáltatás, vagy tárolási fiókja még nem hozott létre, a Visual Studio segítségével hozhat létre is. A hello **felhőalapú szolgáltatás létrehozása és a fiók** párbeszédpanel, hello kívánt szolgáltatás nevét, és jelölje be hello kívánt régiót. Ezt követően kattintson a **Create** (Létrehozás) gombra.
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-7-publish-createserviceandstorage.png)
9. Hello a beállítások lap közzététele, ellenőrizze hello konfigurációját, és kattintson a **közzététel**.
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-8-publish-finalize.png)
   
   > [!NOTE]
   > a felhőszolgáltatások hello a közzétételi folyamat hosszú ideig tart. minden szerepkörök beállítás engedélyezéséhez a Web Deploy hello teheti a felhőalapú szolgáltatás sokkal gyorsabb, adja meg a webes szerepkörök gyors (de ideiglenes) frissítések tooyour hibakeresést. Ez a beállítás további információkért lásd: [közzététele a felhőalapú szolgáltatás hello Azure eszközökkel](http://msdn.microsoft.com/library/ff683672.aspx).
   > 
   > 
   
    Ha hello **Microsoft Azure tevékenységnapló** mutatja, hogy közzétételi állapotát **befejezve**, létrehozhat egy CDN-végpontot, amely integrálva van a felhőalapú szolgáltatás.
   
   > [!WARNING]
   > Ha a közzététel után, a telepített hello felhőalapú szolgáltatás egy hiba képernyő jelenik meg, valószínű, mert hello felhőalapú szolgáltatás telepítése után használja a [vendég operációs rendszer, amely nem tartalmazza a .NET 4.5.2](../cloud-services/cloud-services-guestos-update-matrix.md#news-updates).  Által a probléma megkerüléséhez [központi telepítése a .NET 4.5.2 az indítási feladatok](../cloud-services/cloud-services-dotnet-install-dotnet.md).
   > 
   > 

## <a name="create-a-new-cdn-profile"></a>Új CDN-profil létrehozása
A CDN-profil CDN-végpontok gyűjteménye.  Minden profil egy vagy több CDN-végpontot tartalmaz.  Kezdésként érdemes lehet toouse több profilok tooorganize a CDN-végpontokat internetes tartomány, webalkalmazás vagy más feltétel alapján.

> [!TIP]
> Ha már rendelkezik, amelyet az toouse ebben az oktatóanyagban a CDN-profil, a folytatáshoz túl[hozzon létre egy új CDN-végpont](#create-a-new-cdn-endpoint).
> 
> 

[!INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]

## <a name="create-a-new-cdn-endpoint"></a>Új CDN-végpont létrehozása
**új CDN-végpont a tárfiók toocreate**

1. A hello [Azure felügyeleti portálon](https://portal.azure.com), keresse meg a tooyour CDN-profilt.  Előfordulhat, hogy rendelkezik rögzítette azt toohello irányítópult hello előző lépésben.  Ha nem, akkor megtalálja kattintva **Tallózás**, majd **CDN-profilra**, és kattintson a hello-profil tervezi tooadd a végponthoz.
   
    CDN-profil panelje hello jelenik meg.
   
    ![CDN-profil][cdn-profile-settings]
2. Kattintson a hello **végpont hozzáadása** gombra.
   
    ![Végpont hozzáadása gomb][cdn-new-endpoint-button]
   
    Hello **végpont hozzáadása** panel jelenik meg.
   
    ![Végpont hozzáadása panel][cdn-add-endpoint]
3. Adja meg a CDN-végpont kívánt nevét a **Név** mezőben.  Ez a név a gyorsítótárazott erőforrások hello tartomány lesz használt tooaccess `<EndpointName>.azureedge.net`.
4. A hello **forrása típusa** legördülő menüből válassza *felhőalapú szolgáltatás*.  
5. A hello **forrásállomásnév** legördülő menüben válassza ki a felhőalapú szolgáltatás.
6. Hagyja meg az alapértelmezett hello beállításait **forrás elérési útvonalának**, **forrás állomásfejlécét**, és **protokoll/forrásport**.  Meg kell adnia legalább egy protokollt (HTTP vagy HTTPS).
7. Kattintson a hello **Hozzáadás** gomb toocreate hello új végpont.
8. Hello végpont létrehozását követően hello-profil végpontjainak listájában jelenik meg. hello listanézetben látható, hello URL-cím toouse tooaccess gyorsítótárazott tartalmat, valamint hello forrástartományt.
   
    ![CDN-végpont][cdn-endpoint-success]
   
   > [!NOTE]
   > hello végpont nem azonnal lesz használható.  Hello regisztrációs toopropagate hello CDN a hálózaton keresztül too90 percig is eltarthat. Felhasználók, akik toouse hello CDN-tartománynevet azonnal próbálkozzon előfordulhat, hogy fogadni a 404 állapotkódot, amíg hello tartalom hello CDN keresztül érhető el.
   > 
   > 

## <a name="test-hello-cdn-endpoint"></a>Teszt hello CDN-végpont
Közzétételi állapot hello esetén **befejezve**, nyisson meg egy böngészőablakot, és keresse meg a túl**http://<cdnName>*.azureedge.net/Content/bootstrap.css**. A beállítás az URL-cím van:

    http://camservice.azureedge.net/Content/bootstrap.css

Amely megfelel a következő forrás URL-cím hello CDN-végpont toohello:

    http://camcdnservice.cloudapp.net/Content/bootstrap.css

Kiválasztásakor túl**http://*&lt;cdnName >*.azureedge.net/Content/bootstrap.css**, attól függően, hogy a böngészőben fog az rákérdezéses toodownload vagy nyitott hello bootstrap.css, a közzétett webalkalmazás származik.

![](media/cdn-cloud-service-with-cdn/cdn-1-browser-access.PNG)

Hasonló módon érheti el bármely nyilvánosan elérhető URL-  **http://*&lt;serviceName >*.cloudapp.net/** rögtön a CDN-végpont. Példa:

* Egy .js fájl hello/Script elérési útjáról
* Bármely tartalomfájl hello /Content az elérési út
* Bármely tartományvezérlő/művelet
* Ha a CDN-végpontot, a lekérdezési karakterláncokat tartalmazó valamennyi URL-cím jelenleg engedélyezett hello lekérdezési karakterlánc

Valójában a fenti konfigurációs hello, tárolhatja hello teljes körű felhőalapú szolgáltatás  **http://*&lt;cdnName >*.azureedge.net/**. Ha I túl**http://camservice.azureedge.net/ ** kapok hello művelet eredménye a kezdőlap/Index.

![](media/cdn-cloud-service-with-cdn/cdn-2-home-page.PNG)

Ez nem jelenti azt, azonban, hogy a rendszer mindig egy jó ötlet tooserve egy teljes körű felhőalapú szolgáltatás Azure CDN keresztül. 

A statikus kézbesítési optimalizálásával CDN nem feltétlenül felgyorsítása dinamikus eszközöket, amelyek nem jelentenek gyorsítótárazott toobe, vagy nagyon gyakran frissülnek, mivel a hello CDN kell lekéréses hello eszközök új verziójának hello forráskiszolgálóról nagyon gyakran kézbesítését. A jelen esetben engedélyezhető [dinamikus hely Acceleration](cdn-dynamic-site-acceleration.md) CDN-végpont nem gyorsítótárazható dinamikus eszközök kézbesítését be különböző módszereket toospeed használó optimalizálása (DSA). 

Ha statikus és dinamikus tartalom vegyesen hellyel rendelkezik, választhat tooserve a CDN (például általános webes kézbesítés) statikus optimalizálási típusú statikus tartalmat, és a tooserve dinamikus tartalmat közvetlenül a hello eredeti kiszolgálóra vagy egy CDN keresztül DSA optimalizálásával végpont-eseti alapon van kapcsolva. toothat célból már láthatta hogyan tooaccess egyes tartalom fájljainak hello CDN-végpontot. I bemutatja, hogyan tooserve egy adott vezérlő művelet a megadott CDN-végpontot keresztül kiszolgálására tartalom vezérlő műveletekből Azure CDN keresztül.

hello alternatív toodetermine, amely a felhőszolgáltatás-eseti alapon Azure CDN tooserve tartalom. toothat célból már láthatta hogyan tooaccess egyes tartalom fájljainak hello CDN-végpontot. I bemutatja, hogyan tooserve egy adott vezérlő művelet keresztül hello a CDN-végpont [vezérlő műveletekből Azure CDN keresztül tartalmat](#controller).

<a name="caching"></a>

## <a name="configure-caching-options-for-static-files-in-your-cloud-service"></a>A felhőszolgáltatás a statikus fájlok gyorsítótárazási beállítások konfigurálása
Az Azure CDN integráció a felhőalapú szolgáltatás megadhatja a statikus tartalom toobe tárolja a CDN-végpont hello módját. toodo a, nyissa meg *Web.config* a webes szerepkör projekt (pl. WebRole1), és adja hozzá a `<staticContent>` elem túl`<system.webServer>`. az alábbi XML hello hello gyorsítótár tooexpire azt a 3 nap.  

    <system.webServer>
      <staticContent>
        <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="3.00:00:00"/>
      </staticContent>
      ...
    </system.webServer>

Ezután a, a felhőszolgáltatásban található összes statikus fájlok megfigyelheti hello ugyanaz a szabály a CDN-gyorsítótárban. Részletesebben vezérelhető, a gyorsítótár beállításait, vegye fel a *Web.config* fájlt egy mappába, és ott a beállítások. Például hozzáadhat egy *Web.config* toohello fájl *\Content* mappa, és cserélje le a következő XML hello hello tartalom:

    <?xml version="1.0"?>
    <configuration>
      <system.webServer>
        <staticContent>
          <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="15.00:00:00"/>
        </staticContent>
      </system.webServer>
    </configuration>

Ez a beállítás feldolgozza az összes statikus fájlt hello *\Content* mappa toobe 15 napig.

További információt a tooconfigure hello `<clientCache>` elem, lásd: [Ügyfélgyorsítótár &lt;clientCache >](http://www.iis.net/configreference/system.webserver/staticcontent/clientcache).

A [vezérlő műveletekből Azure CDN keresztül tartalmat](#controller), I is bemutatja, hogyan konfigurálhatja a tartományvezérlő műveleti eredmény-gyorsítótárának beállításait a hello CDN gyorsítótár.

<a name="controller"></a>

## <a name="serve-content-from-controller-actions-through-azure-cdn"></a>Az Azure CDN keresztül vezérlő műveletekből tartalmat
Egy felhőalapú szolgáltatás webes szerepkör integrálása Azure CDN, esetén viszonylag egyszerű tooserve tartalom vezérlő műveletekből hello Azure CDN keresztül. A felhőbeli szolgáltató eltérő szolgáltatás közvetlenül az Azure CDN (egy fent), [Martin Balliauw](https://twitter.com/maartenballiauw) bemutatja, hogyan toodo azt egy visszatöltött MemeGenerator tartományvezérlőre [csökkenti a késéseket hello Azure CDN hello weben ](http://channel9.msdn.com/events/TechDays/Techdays-2014-the-Netherlands/Reducing-latency-on-the-web-with-the-Windows-Azure-CDN). I fog egyszerűen Reprodukálja azt itt.

Tegyük fel, hogy a felhőszolgáltatásban található egy fiatal Horváth Norris lemezképen alapuló toogenerate memes kívánt (által fénykép [Alan világos](http://www.flickr.com/photos/alan-light/218493788/)) Ez, például:

![](media/cdn-cloud-service-with-cdn/cdn-5-memegenerator.PNG)

Rendelkezik egy egyszerű `Index` műveletet, amely lehetővé teszi az hello ügyfelek toospecify hello superlatives hello kép, majd állít elő hello meme azok utáni toohello művelet után. Mivel ez Horváth Norris, teheti meg a lap toobecome menet népszerű globálisan. Ez az Azure CDN félig dinamikus tartalmat szolgáltató jól szemlélteti.

Lépésekkel hello toosetup fent a vezérlő művelet:

1. A hello *\Controllers* mappa, hozzon létre egy új .cs fájlt nevű *MemeGeneratorController.cs* és a következő kód csere hello hello tartalom. Lehet, hogy tooreplace hello kiemelt része a CDN-névvel.  
   
        using System;
        using System.Collections.Generic;
        using System.Diagnostics;
        using System.Drawing;
        using System.IO;
        using System.Net;
        using System.Web.Hosting;
        using System.Web.Mvc;
        using System.Web.UI;
   
        namespace WebRole1.Controllers
        {
            public class MemeGeneratorController : Controller
            {
                static readonly Dictionary<string, Tuple<string ,string>> Memes = new Dictionary<string, Tuple<string, string>>();
   
                public ActionResult Index()
                {
                    return View();
                }
   
                [HttpPost, ActionName("Index")]
                public ActionResult Index_Post(string top, string bottom)
                {
                    var identifier = Guid.NewGuid().ToString();
                    if (!Memes.ContainsKey(identifier))
                    {
                        Memes.Add(identifier, new Tuple<string, string>(top, bottom));
                    }
   
                    return Content("<a href=\"" + Url.Action("Show", new {id = identifier}) + "\">here's your meme</a>");
                }
   
                [OutputCache(VaryByParam = "*", Duration = 1, Location = OutputCacheLocation.Downstream)]
                public ActionResult Show(string id)
                {
                    Tuple<string, string> data = null;
                    if (!Memes.TryGetValue(id, out data))
                    {
                        return new HttpStatusCodeResult(HttpStatusCode.NotFound);
                    }
   
                    if (Debugger.IsAttached) // Preserve hello debug experience
                    {
                        return Redirect(string.Format("/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
                    }
                    else // Get content from Azure CDN
                    {
                        return Redirect(string.Format("http://<yourCdnName>.azureedge.net/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
                    }
                }
   
                [OutputCache(VaryByParam = "*", Duration = 3600, Location = OutputCacheLocation.Downstream)]
                public ActionResult Generate(string top, string bottom)
                {
                    string imageFilePath = HostingEnvironment.MapPath("~/Content/chuck.bmp");
                    Bitmap bitmap = (Bitmap)Image.FromFile(imageFilePath);
   
                    using (Graphics graphics = Graphics.FromImage(bitmap))
                    {
                        SizeF size = new SizeF();
                        using (Font arialFont = FindBestFitFont(bitmap, graphics, top.ToUpperInvariant(), new Font("Arial Narrow", 100), out size))
                        {
                            graphics.DrawString(top.ToUpperInvariant(), arialFont, Brushes.White, new PointF(((bitmap.Width - size.Width) / 2), 10f));
                        }
                        using (Font arialFont = FindBestFitFont(bitmap, graphics, bottom.ToUpperInvariant(), new Font("Arial Narrow", 100), out size))
                        {
                            graphics.DrawString(bottom.ToUpperInvariant(), arialFont, Brushes.White, new PointF(((bitmap.Width - size.Width) / 2), bitmap.Height - 10f - arialFont.Height));
                        }
                    }
   
                    MemoryStream ms = new MemoryStream();
                    bitmap.Save(ms, System.Drawing.Imaging.ImageFormat.Png);
                    return File(ms.ToArray(), "image/png");
                }
   
                private Font FindBestFitFont(Image i, Graphics g, String text, Font font, out SizeF size)
                {
                    // Compute actual size, shrink if needed
                    while (true)
                    {
                        size = g.MeasureString(text, font);
   
                        // It fits, back out
                        if (size.Height < i.Height &&
                             size.Width < i.Width) { return font; }
   
                        // Try a smaller font (90% of old size)
                        Font oldFont = font;
                        font = new Font(font.Name, (float)(font.Size * .9), font.Style);
                        oldFont.Dispose();
                    }
                }
            }
        }
2. Kattintson a jobb gombbal a hello alapértelmezett `Index()` műveletet, és kijelölheti **nézet hozzáadása**.
   
    ![](media/cdn-cloud-service-with-cdn/cdn-6-addview.PNG)
3. Fogadja el az alábbi hello beállításokat, és kattintson a **Hozzáadás**.
   
   ![](media/cdn-cloud-service-with-cdn/cdn-7-configureview.PNG)
4. Megnyitás új hello *Views\MemeGenerator\Index.cshtml* , és cserélje hello tartalom a következő egyszerű HTML hello superlatives küldésére hello:
   
        <h2>Meme Generator</h2>
   
        <form action="" method="post">
            <input type="text" name="top" placeholder="Enter top text here" />
            <br />
            <input type="text" name="bottom" placeholder="Enter bottom text here" />
            <br />
            <input class="btn" type="submit" value="Generate meme" />
        </form>
5. Tegye közzé újra hello felhőalapú szolgáltatás, és keresse meg a túl**http://*&lt;serviceName >*.cloudapp.net/MemeGenerator/Index** a böngészőben.

Ha hello űrlap értékek elküldését, túl`/MemeGenerator/Index`, hello `Index_Post` tartozó műveleti módszer adja vissza a hivatkozás toohello `Show` hello megfelelő bemeneti azonosítóval tartozó műveleti módszer. Amikor hello hivatkozásra kattint, a következő kód hello érhető el:  

    [OutputCache(VaryByParam = "*", Duration = 1, Location = OutputCacheLocation.Downstream)]
    public ActionResult Show(string id)
    {
        Tuple<string, string> data = null;
        if (!Memes.TryGetValue(id, out data))
        {
            return new HttpStatusCodeResult(HttpStatusCode.NotFound);
        }

        if (Debugger.IsAttached) // Preserve hello debug experience
        {
            return Redirect(string.Format("/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
        }
        else // Get content from Azure CDN
        {
            return Redirect(string.Format("http://<yourCDNName>.azureedge.net/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
        }
    }

Ha a helyi hibakereső van csatlakoztatva, majd elérhetővé válik a helyi átirányítást hello rendszeres hibakeresési élményt. Ha hello felhőszolgáltatásban fut, majd azt fogja átirányítani Önt:

    http://<yourCDNName>.azureedge.net/MemeGenerator/Generate?top=<formInput>&bottom=<formInput>

Amely megfelel a következő forrás URL-címet a CDN-végpont toohello:

    http://<youCloudServiceName>.cloudapp.net/MemeGenerator/Generate?top=<formInput>&bottom=<formInput>


Hello segítségével `OutputCacheAttribute` hello attribútum `Generate` metódus toospecify hogyan hello művelet eredménye gyorsítótárazza, amely Azure CDN megtörténni. hello kódot adja meg a gyorsítótár lejárati 1 óra (3600 másodperc).

    [OutputCache(VaryByParam = "*", Duration = 3600, Location = OutputCacheLocation.Downstream)]

Ehhez hasonlóan, szolgálhat bármely tartományvezérlő művelet a tartalmakat a felhőszolgáltatásban keresztül az Azure CDN szükséges hello gyorsítótárazási kapcsolóval.

A következő szakaszban hello I bemutatja, hogyan tooserve hello kötegelve, és a parancsfájlokat és Azure CDN keresztül CSS minified.

<a name="bundling"></a>

## <a name="integrate-aspnet-bundling-and-minification-with-azure-cdn"></a>ASP.NET kötegelése és minification integrálása az Azure CDN szolgáltatás használata
Parancsfájlok és CSS stíluslapok ritkán módosítsa, majd hello Azure CDN gyorsítótár elsődleges vár. Szolgál hello teljes webes szerepkört az Azure CDN keresztül hello legegyszerűbb módja toointegrate kötegelése és az Azure CDN minification. Azonban előfordulhat, hogy nem kívánt toodo ez, I bemutatja, hogyan toodo azt megőrzése mellett hello szükséges develper élmény ASP.NET kötegelése és minification, többek között:

* Nagyszerű hibakeresési mód élmény
* Egyszerű telepítés
* A parancsfájl/CSS verziófrissítések azonnali frissítést tooclients
* Ha a CDN-végpontot nem sikerült tartalék mechanizmus
* Minimalizálása érdekében a kód módosítása

A hello **WebRole1** létrehozott projekt [Azure CDN-végpont integrálása az Azure webhelyén, és a statikus tartalmat szolgáltat a weblapok Azure CDN](#deploy), nyissa meg *App_Start\ BundleConfig.cs* és vessen egy pillantást hello `bundles.Add()` metódushívások.

    public static void RegisterBundles(BundleCollection bundles)
    {
        bundles.Add(new ScriptBundle("~/bundles/jquery").Include(
                    "~/Scripts/jquery-{version}.js"));
        ...
    }

először hello `bundles.Add()` utasítás ad hozzá egy parancsfájl köteg hello virtuális könyvtár `~/bundles/jquery`. Ezután nyissa meg *Views\Shared\_Layout.cshtml* toosee hogyan hello parancsfájlkód köteg lett megjelenítve. Meg kell tudni toofind hello Razor kódsort a következő:

    @Scripts.Render("~/bundles/jquery")

Futtatásakor a Razor kódot hello Azure webes szerepkör, akkor jelenik meg egy `<script>` hello címkéjének köteg hasonló toohello következő parancsfájlt:

    <script src="/bundles/jquery?v=FVs3ACwOLIVInrAl5sdzR2jrCDmVOWFbZMY6g6Q0ulE1"></script>

Azonban azt futása közben a Visual Studio beírásával `F5`, külön-külön, ezzel veszélyezteti a hello kötegben minden parancsfájl (hello fenti esetben csak egy parancsfájl van hello csomagot):

    <script src="/Scripts/jquery-1.10.2.js"></script>

Ez lehetővé teszi a fejlesztési környezet toodebug hello JavaScript-kód közben csökkentése (kötegelése) egyidejű kapcsolatok és jobb lesz a fájl letöltése teljesítmény (minification) éles környezetben. Az Azure CDN integráció egy nagyszerű szolgáltatás toopreserve. Ezenkívül megjelenítve hello csomagot már tartalmaz egy automatikusan létrehozott verzió-karakterláncnak, mivel azt szeretné, tooreplicate, amely funkciót úgy hello frissítése jQuery keresztül NuGet, amikor frissíthető hello ügyfél oldalán, amint lehetséges.

Kövesse az alábbi toointegration ASP.NET kötegelése és a CDN-végponthoz minification hello lépéseket.

1. Vissza a *App_Start\BundleConfig.cs*, hello módosítása `bundles.Add()` módszerek toouse egy másik [köteg konstruktor](http://msdn.microsoft.com/library/jj646464.aspx), egy, a CDN-címet adja meg. toodo, ez a név felülírandó hello `RegisterBundles` metódus-definíció hello a következő kódot:  
   
        public static void RegisterBundles(BundleCollection bundles)
        {
            bundles.UseCdn = true;
            var version = System.Reflection.Assembly.GetAssembly(typeof(Controllers.HomeController))
                .GetName().Version.ToString();
            var cdnUrl = "http://<yourCDNName>.azureedge.net/{0}?v=" + version;
   
            bundles.Add(new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "bundles/jquery")).Include(
                        "~/Scripts/jquery-{version}.js"));
   
            bundles.Add(new ScriptBundle("~/bundles/jqueryval", string.Format(cdnUrl, "bundles/jqueryval")).Include(
                        "~/Scripts/jquery.validate*"));
   
            // Use hello development version of Modernizr toodevelop with and learn from. Then, when you're
            // ready for production, use hello build tool at http://modernizr.com toopick only hello tests you need.
            bundles.Add(new ScriptBundle("~/bundles/modernizr", string.Format(cdnUrl, "bundles/modernizer")).Include(
                        "~/Scripts/modernizr-*"));
   
            bundles.Add(new ScriptBundle("~/bundles/bootstrap", string.Format(cdnUrl, "bundles/bootstrap")).Include(
                        "~/Scripts/bootstrap.js",
                        "~/Scripts/respond.js"));
   
            bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css")).Include(
                        "~/Content/bootstrap.css",
                        "~/Content/site.css"));
        }
   
    Lehet, hogy tooreplace `<yourCDNName>` az Azure CDN hello nevére.
   
    Egyszerű szavakkal állítja `bundles.UseCdn = true` és gondosan kialakított CDN URL-cím tooeach köteg hozzáadni. Például hello első konstruktor hello kódban:
   
        new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "bundles/jquery"))
   
    ugyanaz, mint a rendszer hello:
   
        new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "http://<yourCDNName>.azureedge.net/bundles/jquery?v=<W.X.Y.Z>"))
   
    Ez a konstruktor közli az ASP.NET kötegelése és minification egyéni parancsfájlok toorender, amikor indítja helyi, de használja hello CDN cím tooaccess hello parancsfájl az adott meg. Azonban ügyeljen arra, hogy alaposan kialakított CDN URL-címet a két fontos jellemzők:
   
   * a CDN URL-címhez hello forrása: `http://<yourCloudService>.cloudapp.net/bundles/jquery?v=<W.X.Y.Z>`, vagyis tényleges hello virtuális könyvtár hello parancsfájl köteg a felhőszolgáltatásban.
   * CDN konstruktor használ, mivel a hello CDN script kód a hello köteg már nem tartalmaz URL-cím megjelenítése hello automatikusan generált hello verzió-karakterlánca. Manuálisan létre kell hoznia egy egyedi verzió-karakterláncot, minden alkalommal, amikor hello parancsfájl köteg, az Azure CDN teljesíti a gyorsítótár módosított tooforce. At hello azonos időben, a egyedi verzió-karakterlánca állandónak kell keresztül hello telepítési toomaximize találatot eredményező gyorsítótárbeli kereséseinek, az Azure CDN hello élettartama hello köteg telepítése után.
   * lekérdezési karakterlánc v hello = < W.X.Y.Z > ponttá a *Properties\AssemblyInfo.cs* a webes szerepkör projekt. Akkor is, amely tartalmazza a növekvő hello szerelvény verziója, minden alkalommal, amikor tooAzure közzéteszi a telepítési munkafolyamat. Vagy csak módosíthatja *Properties\AssemblyInfo.cs* a projekt tooautomatically növekmény hello verzió-karakterlánc minden alkalommal, amikor a build, használatával hello helyettesítő karakter "*". Példa:
     
        [szerelvény: AssemblyVersion("1.0.0.*")]
     
     Más stratégia toostreamline egy egyedi karakterlánc hello során a központi telepítés létrehozásakor itt fog működni.
2. Hello felhőalapú szolgáltatás, és hozzáférést hello kezdőlap közzé.
3. Nézet hello hello oldal HTML-kódja. Meg kell tudni toosee hello CDN URL-cím, egy egyedi verzió-karakterláncot, minden alkalommal, amikor újra közzé módosítások tooyour felhőalapú szolgáltatás együtt jelenik meg. Példa:  
   
        ...
   
        <link href="http://camservice.azureedge.net/Content/css?v=1.0.0.25449" rel="stylesheet"/>
   
        <script src="http://camservice.azureedge.net/bundles/modernizer?v=1.0.0.25449"></script>
   
        ...
   
        <script src="http://camservice.azureedge.net/bundles/jquery?v=1.0.0.25449"></script>
   
        <script src="http://camservice.azureedge.net/bundles/bootstrap?v=1.0.0.25449"></script>
   
        ...
4. A Visual Studio hibakeresési írja be a Visual Studio felhőszolgáltatás hello `F5`.,
5. Nézet hello hello oldal HTML-kódja. Minden egyes külön-külön megjelenítve, hogy akkor is egy egységes élmény a Visual Studio hibakeresési parancsfájl továbbra is megjelenik.  
   
        ...
   
            <link href="/Content/bootstrap.css" rel="stylesheet"/>
        <link href="/Content/site.css" rel="stylesheet"/>
   
            <script src="/Scripts/modernizr-2.6.2.js"></script>
   
        ...
   
            <script src="/Scripts/jquery-1.10.2.js"></script>
   
            <script src="/Scripts/bootstrap.js"></script>
        <script src="/Scripts/respond.js"></script>
   
        ...   

<a name="fallback"></a>

## <a name="fallback-mechanism-for-cdn-urls"></a>Tartalék mechanizmus CDN URL-címek esetén
Azure CDN-végpont bármilyen okból nem sikerül, ha azt szeretné, a weblap toobe intelligens elég tooaccess a származási webkiszolgáló JavaScript vagy rendszerindítási feltöltését hello megoldásként. Elég súlyos toolose képek a webhelyen tooCDN elérhetetlensége, de a parancsfájlok és a stíluslapok által biztosított sokkal több súlyos toolose kritikus fontosságú lap funkciókat miatt.

Hello [köteg](http://msdn.microsoft.com/library/system.web.optimization.bundle.aspx) osztály tartalmazza a tulajdonságot, [CdnFallbackExpression](http://msdn.microsoft.com/library/system.web.optimization.bundle.cdnfallbackexpression.aspx) , amely lehetővé teszi tooconfigure hello tartalék mechanizmus a CDN-hiba. toouse ezt a tulajdonságot, hajtsa végre hello alábbi lépéseket:

1. A webes szerepkör projektben nyissa meg *App_Start\BundleConfig.cs*, amelyikhez hozzáadta a CDN URL-cím az egyes [köteg konstruktor](http://msdn.microsoft.com/library/jj646464.aspx), és ellenőrizze a kijelölt hello következő tooadd tartalék mechanizmus toohello változik alapértelmezett csomagokat:  
   
        public static void RegisterBundles(BundleCollection bundles)
        {
            var version = System.Reflection.Assembly.GetAssembly(typeof(BundleConfig))
                .GetName().Version.ToString();
            var cdnUrl = "http://cdnurl.azureedge.net/.../{0}?" + version;
            bundles.UseCdn = true;
   
            bundles.Add(new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "bundles/jquery"))
                        { CdnFallbackExpression = "window.jquery" }
                        .Include("~/Scripts/jquery-{version}.js"));
   
            bundles.Add(new ScriptBundle("~/bundles/jqueryval", string.Format(cdnUrl, "bundles/jqueryval"))
                        { CdnFallbackExpression = "$.validator" }
                        .Include("~/Scripts/jquery.validate*"));
   
            // Use hello development version of Modernizr toodevelop with and learn from. Then, when you&#39;re
            // ready for production, use hello build tool at http://modernizr.com toopick only hello tests you need.
            bundles.Add(new ScriptBundle("~/bundles/modernizr", string.Format(cdnUrl, "bundles/modernizer"))
                        { CdnFallbackExpression = "window.Modernizr" }
                        .Include("~/Scripts/modernizr-*"));
   
            bundles.Add(new ScriptBundle("~/bundles/bootstrap", string.Format(cdnUrl, "bundles/bootstrap"))     
                        { CdnFallbackExpression = "$.fn.modal" }
                        .Include(
                                  "~/Scripts/bootstrap.js",
                                  "~/Scripts/respond.js"));
   
            bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css")).Include(
                        "~/Content/bootstrap.css",
                        "~/Content/site.css"));
        }
   
    Ha `CdnFallbackExpression` értéke nem null értékű, parancsfájl van be a nézetmodellbe hello HTML tootest e hello csomag sikeresen betöltődött és, ha nem, érheti el hello köteg közvetlenül hello származási webkiszolgáló. Ez a tulajdonság kell toobe beállítási tooa JavaScript kifejezés, amely teszteli, hogy hello megfelelő CDN csomagot megfelelően be van töltve. hello kifejezés szükséges tootest minden alkalmazáscsomag eltér függően toohello tartalmat. A fenti hello alapértelmezett csomagokat:
   
   * `window.jquery`jquery-{version} .js definiálva van
   * `$.validator`jquery.validate.js definiálva van
   * `window.Modernizr`modernizer-{version} .js definiálva van
   * `$.fn.modal`bootstrap.js definiálva van
     
     Észrevette, hogy, hogy I nincs beállítva CdnFallbackExpression a hello `~/Cointent/css` csomagot. Ez azért, mert jelenleg nincs egy [System.Web.Optimization hiba](https://aspnetoptimization.codeplex.com/workitem/104) , amelyek esetében a `<script>` helyett hello tartalék CSS várt hello címkéjének `<link>` címke.
     
     Nincs, de jó [stílus köteg tartalék](https://github.com/EmberConsultingGroup/StyleBundleFallback) által kínált [decemberéig tanácsadás csoport](https://github.com/EmberConsultingGroup).
2. toouse hello megoldás a CSS, hozzon létre egy új .cs-fájlt a webes szerepkör projekt *App_Start* nevű mappát *StyleBundleExtensions.cs*, és cserélje le benne lévő tartalom hello [a kód GitHub](https://github.com/EmberConsultingGroup/StyleBundleFallback/blob/master/Website/App_Start/StyleBundleExtensions.cs).
3. A *App_Start\StyleFundleExtensions.cs*, nevezze át a hello névtér tooyour webes szerepkör nevét (pl. **WebRole1**).
4. Lépjen vissza túl`App_Start\BundleConfig.cs` és hello utolsó módosítása `bundles.Add` hello kiemelt kódot a következő utasítást:  
   
        bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css"))
            <mark>.IncludeFallback("~/Content/css", "sr-only", "width", "1px")</mark>
            .Include(
                  "~/Content/bootstrap.css",
                  "~/Content/site.css"));
   
    A bővítmény metódus használ hello azonos ötlet tooinject parancsfájl a hello HTML toocheck hello DOM hello egyező osztály neve, a szabály nevét és hello CSS csomagot, és visszatér hátsó toohello származási webkiszolgáló Ha toofind hello egyeztetés sikertelen a megadott szabály értékét.
5. Tegye közzé újra hello felhőalapú szolgáltatás és az access hello kezdőlap.
6. Nézet hello hello oldal HTML-kódja. Keresse meg injected parancsfájlok hasonló toohello következő:    
   
        ...
   
        <link href="http://az632148.azureedge.net/Content/css?v=1.0.0.25474" rel="stylesheet"/>
        <script>(function() {
                        var loadFallback,
                            len = document.styleSheets.length;
                        for (var i = 0; i < len; i++) {
                            var sheet = document.styleSheets[i];
                            if (sheet.href.indexOf('http://camservice.azureedge.net/Content/css?v=1.0.0.25474') !== -1) {
                                var meta = document.createElement('meta');
                                meta.className = 'sr-only';
                                document.head.appendChild(meta);
                                var value = window.getComputedStyle(meta).getPropertyValue('width');
                                document.head.removeChild(meta);
                                if (value !== '1px') {
                                    document.write('<link href="/Content/css" rel="stylesheet" type="text/css" />');
                                }
                            }
                        }
                        return true;
                    }())||document.write('<script src="/Content/css"><\/script>');</script>
   
            <script src="http://camservice.azureedge.net/bundles/modernizer?v=1.0.0.25474"></script>
        <script>(window.Modernizr)||document.write('<script src="/bundles/modernizr"><\/script>');</script>
   
        ...
   
            <script src="http://camservice.azureedge.net/bundles/jquery?v=1.0.0.25474"></script>
        <script>(window.jquery)||document.write('<script src="/bundles/jquery"><\/script>');</script>
   
            <script src="http://camservice.azureedge.net/bundles/bootstrap?v=1.0.0.25474"></script>
        <script>($.fn.modal)||document.write('<script src="/bundles/bootstrap"><\/script>');</script>
   
        ...

    A CSS-köteg hello injected parancsfájl továbbra is tartalmaz hello errant tartós hello a `CdnFallbackExpression` tulajdonság hello sorban:

        }())||document.write('<script src="/Content/css"><\/script>');</script>

    De óta hello hello első része. kifejezés mindig ad vissza IGAZ (a hello sorban fölött, amely), hello document.write() függvény soha nem fog futni.

## <a name="more-information"></a>További információ
* [Hello Azure Content Delivery Network (CDN) áttekintése](http://msdn.microsoft.com/library/azure/ff919703.aspx)
* [Az Azure CDN szolgáltatás használata](cdn-create-new-endpoint.md)
* [ASP.NET kötegelése és Minification](http://www.asp.net/mvc/tutorials/mvc-4/bundling-and-minification)

[new-cdn-profile]: ./media/cdn-cloud-service-with-cdn/cdn-new-profile.png
[cdn-profile-settings]: ./media/cdn-cloud-service-with-cdn/cdn-profile-settings.png
[cdn-new-endpoint-button]: ./media/cdn-cloud-service-with-cdn/cdn-new-endpoint-button.png
[cdn-add-endpoint]: ./media/cdn-cloud-service-with-cdn/cdn-add-endpoint.png
[cdn-endpoint-success]: ./media/cdn-cloud-service-with-cdn/cdn-endpoint-success.png
