---
title: "az Azure Traffic Manager \"csökkentett teljesítményű\" aaaTroubleshooting állapota"
description: "Hogyan \"tootroubleshoot Traffic Manager-profilok azt mutatja, mint amikor csökkentett teljesítményű\" állapota."
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
ms.assetid: 8af0433d-e61b-4761-adcc-7bc9b8142fc6
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/03/2017
ms.author: kumud
ms.openlocfilehash: fd95697781472b52e98d856e66beb7b89dfeaf23
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-degraded-state-on-azure-traffic-manager"></a><span data-ttu-id="80d4a-103">Hibaelhárítás "csökkentett teljesítményű" állapota az Azure Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="80d4a-103">Troubleshooting degraded state on Azure Traffic Manager</span></span>

<span data-ttu-id="80d4a-104">Ez a cikk ismerteti, hogyan tootroubleshoot az Azure Traffic Manager-profilt, amely azt leromlott állapot.</span><span class="sxs-lookup"><span data-stu-id="80d4a-104">This article describes how tootroubleshoot an Azure Traffic Manager profile that is showing a degraded status.</span></span> <span data-ttu-id="80d4a-105">Ebben az esetben fontolja meg, hogy konfigurálta a cloudapp.net üzemeltetett szolgáltatások toosome mutató Traffic Manager-profil.</span><span class="sxs-lookup"><span data-stu-id="80d4a-105">For this scenario, consider that you have configured a Traffic Manager profile pointing toosome of your cloudapp.net hosted services.</span></span> <span data-ttu-id="80d4a-106">Ha a Traffic Manager hello állapotát jeleníti meg a **csökkentett teljesítményű** állapotát, majd egy vagy több végpontot hello állapotának lehet **csökkentett teljesítményű**:</span><span class="sxs-lookup"><span data-stu-id="80d4a-106">If hello health of your Traffic Manager displays a **Degraded** status, then hello status of one or more endpoints may be **Degraded**:</span></span>

![csökkent végpont állapota](./media/traffic-manager-troubleshooting-degraded/traffic-manager-degradedifonedegraded.png)

<span data-ttu-id="80d4a-108">Ha a Traffic Manager hello állapotát jeleníti meg egy **inaktív** állapot, akkor mindkét végpontok lehetnek **letiltott**:</span><span class="sxs-lookup"><span data-stu-id="80d4a-108">If hello health of your Traffic Manager displays an **Inactive** status, then both end points may be **Disabled**:</span></span>

![Inaktív Traffic Manager-állapot](./media/traffic-manager-troubleshooting-degraded/traffic-manager-inactive.png)

## <a name="understanding-traffic-manager-probes"></a><span data-ttu-id="80d4a-110">Understanding Traffic Manager-mintavétel</span><span class="sxs-lookup"><span data-stu-id="80d4a-110">Understanding Traffic Manager probes</span></span>

* <span data-ttu-id="80d4a-111">A TRAFFIC Manager úgy véli, hogy egy végpont toobe csak akkor, ha hello mintavételi egy 200-as HTTP-válasz fogadása ONLINE biztonsági hello mintavételi elérési útról.</span><span class="sxs-lookup"><span data-stu-id="80d4a-111">Traffic Manager considers an endpoint toobe ONLINE only when hello probe receives an HTTP 200 response back from hello probe path.</span></span> <span data-ttu-id="80d4a-112">A rendszer hibát bármely más nem 200 választ.</span><span class="sxs-lookup"><span data-stu-id="80d4a-112">Any other non-200 response is a failure.</span></span>
* <span data-ttu-id="80d4a-113">30 x átirányítást nem sikerül, akkor is, ha hello átirányított URL-címet adja vissza a 200-as.</span><span class="sxs-lookup"><span data-stu-id="80d4a-113">A 30x redirect fails, even if hello redirected URL returns a 200.</span></span>
* <span data-ttu-id="80d4a-114">A HTTPs mintavételek menüpontban hitelesítési hibák figyelmen kívül lesznek hagyva.</span><span class="sxs-lookup"><span data-stu-id="80d4a-114">For HTTPs probes, certificate errors are ignored.</span></span>
* <span data-ttu-id="80d4a-115">hello tényleges tartalmat hello mintavételi elérési út nem számít, mindaddig, amíg egy 200 adja vissza.</span><span class="sxs-lookup"><span data-stu-id="80d4a-115">hello actual content of hello probe path doesn't matter, as long as a 200 is returned.</span></span> <span data-ttu-id="80d4a-116">Egy URL-cím toosome statikus tartalom hasonló probing "/ favicon.ico" egy általános módszer.</span><span class="sxs-lookup"><span data-stu-id="80d4a-116">Probing a URL toosome static content like "/favicon.ico" is a common technique.</span></span> <span data-ttu-id="80d4a-117">Dinamikus tartalom, például a hello ASP-lapok, előfordulhat, hogy mindig vissza 200, akkor is, ha hello alkalmazás állapota kifogástalan.</span><span class="sxs-lookup"><span data-stu-id="80d4a-117">Dynamic content, like hello ASP pages, may not always return 200, even when hello application is healthy.</span></span>
* <span data-ttu-id="80d4a-118">Az ajánlott eljárás, amely rendelkezik elegendő logikai toodetermine, amely a hely hello elérési toosomething felfelé vagy lefelé mintavételi tooset hello.</span><span class="sxs-lookup"><span data-stu-id="80d4a-118">A best practice is tooset hello Probe path toosomething that has enough logic toodetermine that hello site is up or down.</span></span> <span data-ttu-id="80d4a-119">"Hello az előző példában úgy, hogy elérési too"/favicon.ico hello szövegrészt, hogy csak adott w3wp.exe tesztelés válaszol.</span><span class="sxs-lookup"><span data-stu-id="80d4a-119">In hello previous example, by setting hello path too"/favicon.ico", you are only testing that w3wp.exe is responding.</span></span> <span data-ttu-id="80d4a-120">Ez a Hálózatfigyelő is jelzi, hogy a webalkalmazás működik megfelelően.</span><span class="sxs-lookup"><span data-stu-id="80d4a-120">This probe may not indicate that your web application is healthy.</span></span> <span data-ttu-id="80d4a-121">Jobb megoldás lehet egy elérési utat tooa tooset valamit, mint "/ Probe.aspx", amely rendelkezik logika toodetermine hello hello hely állapotát.</span><span class="sxs-lookup"><span data-stu-id="80d4a-121">A better option would be tooset a path tooa something such as "/Probe.aspx" that has logic toodetermine hello health of hello site.</span></span> <span data-ttu-id="80d4a-122">Használhatja például a teljesítmény számlálók tooCPU kihasználtsági vagy mérték hello sikertelen kérések száma.</span><span class="sxs-lookup"><span data-stu-id="80d4a-122">For example, you could use performance counters tooCPU utilization or measure hello number of failed requests.</span></span> <span data-ttu-id="80d4a-123">Vagy adatbázis-erőforrások tooaccess vagy munkamenet-állapot toomake, hogy működik-e hello webalkalmazás sikertelen kísérlet.</span><span class="sxs-lookup"><span data-stu-id="80d4a-123">Or you could attempt tooaccess database resources or session state toomake sure that hello web application is working.</span></span>
* <span data-ttu-id="80d4a-124">Ha egy profil végpontjai állapotromlást, Traffic Manager kezeli az összes végpontok állapota kiváló, és a útvonalak forgalom tooall végpontok.</span><span class="sxs-lookup"><span data-stu-id="80d4a-124">If all endpoints in a profile are degraded, then Traffic Manager treats all endpoints as healthy and routes traffic tooall endpoints.</span></span> <span data-ttu-id="80d4a-125">Ez a viselkedés garantálja, hogy mechanizmus probing hello problémákat nem eredményez a szolgáltatás egy teljes leállás.</span><span class="sxs-lookup"><span data-stu-id="80d4a-125">This behavior ensures that problems with hello probing mechanism do not result in a complete outage of your service.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="80d4a-126">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="80d4a-126">Troubleshooting</span></span>

<span data-ttu-id="80d4a-127">mintavételi hiba tootroubleshoot, kell eszközzel hello HTTP-állapotkód visszatérési hello mintavételi URL-címről.</span><span class="sxs-lookup"><span data-stu-id="80d4a-127">tootroubleshoot a probe failure, you need a tool that shows hello HTTP status code return from hello probe URL.</span></span> <span data-ttu-id="80d4a-128">Nincsenek elérhető számos eszközt, hogy Ön hello nyers HTTP-válasz megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="80d4a-128">There are many tools available that show you hello raw HTTP response.</span></span>

* [<span data-ttu-id="80d4a-129">Fiddler</span><span class="sxs-lookup"><span data-stu-id="80d4a-129">Fiddler</span></span>](http://www.telerik.com/fiddler)
* [<span data-ttu-id="80d4a-130">curl</span><span class="sxs-lookup"><span data-stu-id="80d4a-130">curl</span></span>](https://curl.haxx.se/)
* [<span data-ttu-id="80d4a-131">wget</span><span class="sxs-lookup"><span data-stu-id="80d4a-131">wget</span></span>](http://gnuwin32.sourceforge.net/packages/wget.htm)

<span data-ttu-id="80d4a-132">Is használhatja az Internet Explorer tooview hello HTTP-válaszok F12 hibakeresési eszközök hello hello hálózati lapján.</span><span class="sxs-lookup"><span data-stu-id="80d4a-132">Also, you can use hello Network tab of hello F12 Debugging Tools in Internet Explorer tooview hello HTTP responses.</span></span>

<span data-ttu-id="80d4a-133">Ehhez a példához azt szeretnénk, ha a mintavételi URL-címhez toosee hello válasza: http://watestsdp2008r2.cloudapp.net:80/mintavétel.</span><span class="sxs-lookup"><span data-stu-id="80d4a-133">For this example we want toosee hello response from our probe URL: http://watestsdp2008r2.cloudapp.net:80/Probe.</span></span> <span data-ttu-id="80d4a-134">a következő PowerShell-példa hello hello probléma mutatja be.</span><span class="sxs-lookup"><span data-stu-id="80d4a-134">hello following PowerShell example illustrates hello problem.</span></span>

```powershell
Invoke-WebRequest 'http://watestsdp2008r2.cloudapp.net/Probe' -MaximumRedirection 0 -ErrorAction SilentlyContinue | Select-Object StatusCode,StatusDescription
```

<span data-ttu-id="80d4a-135">Példa a kimenetre:</span><span class="sxs-lookup"><span data-stu-id="80d4a-135">Example output:</span></span>

    StatusCode StatusDescription
    ---------- -----------------
           301 Moved Permanently

<span data-ttu-id="80d4a-136">Figyelje meg, hogy az átirányítási válasz érkezett.</span><span class="sxs-lookup"><span data-stu-id="80d4a-136">Notice that we received a redirect response.</span></span> <span data-ttu-id="80d4a-137">Ahogy korábban is hangsúlyoztuk, StatusCode bármely más, mint 200 tekinthető hiba.</span><span class="sxs-lookup"><span data-stu-id="80d4a-137">As stated previously, any StatusCode other than 200 is considered a failure.</span></span> <span data-ttu-id="80d4a-138">A TRAFFIC Manager módosítások hello végpont állapota tooOffline.</span><span class="sxs-lookup"><span data-stu-id="80d4a-138">Traffic Manager changes hello endpoint status tooOffline.</span></span> <span data-ttu-id="80d4a-139">tooresolve hello probléma elhárításához ellenőrizze hello webhely konfigurációs tooensure, amely megfelelő StatusCode hello hello mintavételi elérési útról adhatók vissza.</span><span class="sxs-lookup"><span data-stu-id="80d4a-139">tooresolve hello problem, check hello website configuration tooensure that hello proper StatusCode can be returned from hello probe path.</span></span> <span data-ttu-id="80d4a-140">Konfigurálja újra a hello Traffic Manager mintavételi toopoint tooa elérési utat, amely egy 200 adja vissza.</span><span class="sxs-lookup"><span data-stu-id="80d4a-140">Reconfigure hello Traffic Manager probe toopoint tooa path that returns a 200.</span></span>

<span data-ttu-id="80d4a-141">Ha a mintavételi hello HTTPS protokollt használ, szükség lehet a toodisable tanúsítványellenőrzést tooavoid SSL/TLS-hibákat a vizsgálat során.</span><span class="sxs-lookup"><span data-stu-id="80d4a-141">If your probe is using hello HTTPS protocol, you may need toodisable certificate checking tooavoid SSL/TLS errors during your test.</span></span> <span data-ttu-id="80d4a-142">hello következő PowerShell-utasításokat tiltsa le a tanúsítvány érvényesítési hello aktuális PowerShell-munkamenethez:</span><span class="sxs-lookup"><span data-stu-id="80d4a-142">hello following PowerShell statements disable certificate validation for hello current PowerShell session:</span></span>

```powershell
add-type @"
using System.Net;
using System.Security.Cryptography.X509Certificates;
public class TrustAllCertsPolicy : ICertificatePolicy {
    public bool CheckValidationResult(
    ServicePoint srvPoint, X509Certificate certificate,
    WebRequest request, int certificateProblem) {
    return true;
    }
}
"@
[System.Net.ServicePointManager]::CertificatePolicy = New-Object TrustAllCertsPolicy
```

## <a name="next-steps"></a><span data-ttu-id="80d4a-143">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="80d4a-143">Next Steps</span></span>

[<span data-ttu-id="80d4a-144">A Traffic Manager forgalom-útválasztási módszerei</span><span class="sxs-lookup"><span data-stu-id="80d4a-144">About Traffic Manager traffic routing methods</span></span>](traffic-manager-routing-methods.md)

[<span data-ttu-id="80d4a-145">Mi az a Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="80d4a-145">What is Traffic Manager</span></span>](traffic-manager-overview.md)

[<span data-ttu-id="80d4a-146">Felhőszolgáltatások</span><span class="sxs-lookup"><span data-stu-id="80d4a-146">Cloud Services</span></span>](http://go.microsoft.com/fwlink/?LinkId=314074)

[<span data-ttu-id="80d4a-147">Azure Web Apps</span><span class="sxs-lookup"><span data-stu-id="80d4a-147">Azure Web Apps</span></span>](https://azure.microsoft.com/documentation/services/app-service/web/)

[<span data-ttu-id="80d4a-148">A Traffic Manager műveletei (REST API-referencia)</span><span class="sxs-lookup"><span data-stu-id="80d4a-148">Operations on Traffic Manager (REST API Reference)</span></span>](http://go.microsoft.com/fwlink/?LinkId=313584)

<span data-ttu-id="80d4a-149">[Azure Traffic Manager-parancsmagok][1]</span><span class="sxs-lookup"><span data-stu-id="80d4a-149">[Azure Traffic Manager Cmdlets][1]</span></span>

[1]: https://msdn.microsoft.com/library/mt125941(v=azure.200).aspx
