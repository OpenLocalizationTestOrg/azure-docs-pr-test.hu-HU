---
title: "aaaPredictive karbantartási megoldás előre konfigurált |} Microsoft Docs"
description: "Leírását hello Azure IoT Suite prediktív karbantartási megoldás előre konfigurált."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: b370b3d7-2ce5-4906-9818-3aeedd471ee3
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: dobett
ms.openlocfilehash: 2d09801467d33db6b7d6333fa071aea2bf573f20
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="predictive-maintenance-preconfigured-solution-overview"></a><span data-ttu-id="e9afd-103">A prediktív karbantartási előre konfigurált megoldás áttekintése</span><span class="sxs-lookup"><span data-stu-id="e9afd-103">Predictive maintenance preconfigured solution overview</span></span>

<span data-ttu-id="e9afd-104">Hello *prediktív karbantartási* [előre konfigurált megoldás] [ lnk_preconfigured_solutions] hello egyike [Microsoft Azure IoT Suite] [ lnk_iot_suite] előre konfigurált megoldások.</span><span class="sxs-lookup"><span data-stu-id="e9afd-104">hello *predictive maintenance* [preconfigured solution][lnk_preconfigured_solutions] is one of hello [Microsoft Azure IoT Suite][lnk_iot_suite] preconfigured solutions.</span></span> <span data-ttu-id="e9afd-105">Ez a megoldás a valós idejű eszköztelemetria-gyűjtést az [Azure Machine Learning][lnk-machine-learning] használatával létrehozott prediktív modellel integrálja.</span><span class="sxs-lookup"><span data-stu-id="e9afd-105">This solution integrates real-time device telemetry collection with a predictive model created using [Azure Machine Learning][lnk-machine-learning].</span></span>

<span data-ttu-id="e9afd-106">Azure IoT Suite gyorsan és könnyen tooand figyelő eszközök csatlakozni, és valós idejű irányítópultokat és képi megjelenítéseket telemetriai adatok elemzéséhez.</span><span class="sxs-lookup"><span data-stu-id="e9afd-106">With Azure IoT Suite, you can quickly and easily connect tooand monitor assets, and analyze telemetry in real time in dashboards and visualizations.</span></span> <span data-ttu-id="e9afd-107">A prediktív karbantartási megoldás hello hello irányítópultok és a képi megjelenítések biztosít, amely a meghajtó hatékonyság és javítása érdekében a bevétel adatfolyamok új eszközintelligencia.</span><span class="sxs-lookup"><span data-stu-id="e9afd-107">In hello predictive maintenance solution, hello dashboards and visualizations provide you with new intelligence that can drive efficiencies and enhance revenue streams.</span></span>

## <a name="hello-scenario"></a><span data-ttu-id="e9afd-108">hello forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="e9afd-108">hello Scenario</span></span>

<span data-ttu-id="e9afd-109">A Fabrikam egy regionális légitársaság, amely a nagyszerű ügyfélélményre összpontosít versenyképes árakon.</span><span class="sxs-lookup"><span data-stu-id="e9afd-109">Fabrikam is a regional airline that focuses on great customer experience at competitive prices.</span></span> <span data-ttu-id="e9afd-110">A járatok késésének egyik okai a karbantartási problémák, és a repülőmotorok karbantartása különösen nagy kihívást jelent.</span><span class="sxs-lookup"><span data-stu-id="e9afd-110">One cause of flight delays is maintenance issues and aircraft engine maintenance is particularly challenging.</span></span> <span data-ttu-id="e9afd-111">Fabrikam kerülni kell motor hiba felé továbbított folyamán minden áron, hogy annak rendszeresen megvizsgálja az ütemezi a karbantartási terv tooa szerint.</span><span class="sxs-lookup"><span data-stu-id="e9afd-111">Fabrikam must avoid engine failure during flight at all costs, so it inspects its engines regularly and schedules maintenance according tooa plan.</span></span> <span data-ttu-id="e9afd-112">Azonban motorok nem mindig viselniük repülőgép hello azonos.</span><span class="sxs-lookup"><span data-stu-id="e9afd-112">However, aircraft engines don’t always wear hello same.</span></span> <span data-ttu-id="e9afd-113">Időnként feleslegesen végeznek karbantartást a motorokon.</span><span class="sxs-lookup"><span data-stu-id="e9afd-113">Some unnecessary maintenance is performed on engines.</span></span> <span data-ttu-id="e9afd-114">Még fontosabb, hogy olyan problémák merülnek fel, amelyek miatt a repülő nem szállhat fel a karbantartásig.</span><span class="sxs-lookup"><span data-stu-id="e9afd-114">More importantly, issues arise which can ground an aircraft until maintenance is performed.</span></span> <span data-ttu-id="e9afd-115">Ha repülőgép egy helyen ahol hello jobb technikusok vagy tartalék részei nem érhetők el, ezek a problémák különösen költséges lehet.</span><span class="sxs-lookup"><span data-stu-id="e9afd-115">If an aircraft is at a location where hello right technicians or spare parts are not available, these issues can be especially costly.</span></span>

<span data-ttu-id="e9afd-116">Fabrikam repülőgép hello motorok az érzékelők motor feltételek figyelő felé továbbított folyamán vannak tagolva.</span><span class="sxs-lookup"><span data-stu-id="e9afd-116">hello engines of Fabrikam’s aircraft are instrumented with sensors that monitor engine conditions during flight.</span></span> <span data-ttu-id="e9afd-117">Fabrikam hello prediktív karbantartási megoldás toocollect hello érzékelő során gyűjtött adatok hello felhőszolgáltató közötti átviteléhez használja.</span><span class="sxs-lookup"><span data-stu-id="e9afd-117">Fabrikam uses hello predictive maintenance solution toocollect hello sensor data collected during hello flight.</span></span> <span data-ttu-id="e9afd-118">Halmozódó év-kezelő motor működési és adatai, után Fabrikam adatszakértőkön egy módon toopredict hello fennmaradó élettartama (Szabályainak) repülőgép motor van modellezve.</span><span class="sxs-lookup"><span data-stu-id="e9afd-118">After accumulating years of engine operational and failure data, Fabrikam’s data scientists have modeled a way toopredict hello Remaining Useful Life (RUL) of an aircraft engine.</span></span> <span data-ttu-id="e9afd-119">hello modellje négy hello motor érzékelők adatait és motor elhasználódását, amely visszavezet tooeventual hiba közötti kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="e9afd-119">hello model uses a correlation between data from four of hello engine sensors and engine wear that leads tooeventual failure.</span></span> <span data-ttu-id="e9afd-120">Fabrikam tooperform rendszeres vizsgálatokat tooensure biztonsági folytatódik, amíg azt már a hello modellek toocompute hello Szabályainak minden motor után minden felhőszolgáltató közötti átviteléhez.</span><span class="sxs-lookup"><span data-stu-id="e9afd-120">While Fabrikam continues tooperform regular inspections tooensure safety, it can now use hello models toocompute hello RUL for each engine after every flight.</span></span> <span data-ttu-id="e9afd-121">hello modellje hello motorok hello repülési során gyűjtött hello telemetriáját.</span><span class="sxs-lookup"><span data-stu-id="e9afd-121">hello model uses hello telemetry collected from hello engines during hello flight.</span></span> <span data-ttu-id="e9afd-122">A Fabrikam így előre jelezheti a jövőbeli meghibásodási pontokat, és megtervezheti a karbantartást és a javítást.</span><span class="sxs-lookup"><span data-stu-id="e9afd-122">Fabrikam can now predict future points of failure and plan for maintenance and repair in advance.</span></span>

> [!NOTE]
> <span data-ttu-id="e9afd-123">hello megoldás modellje motor tényleges elhasználódását adatokat.</span><span class="sxs-lookup"><span data-stu-id="e9afd-123">hello solution model uses actual engine wear data.</span></span>

<span data-ttu-id="e9afd-124">Előrejelzésére hello pontot, ha karbantartási szükség, amelyet a Fabrikam optimalizálhatja műveletek tooreduce költségeit.</span><span class="sxs-lookup"><span data-stu-id="e9afd-124">By predicting hello point when maintenance is required, Fabrikam can optimize its operations tooreduce costs.</span></span>

<span data-ttu-id="e9afd-125">A karbantartási koordinátorok és a menetrendek készítői együttműködve elvégzik a következőket:</span><span class="sxs-lookup"><span data-stu-id="e9afd-125">Maintenance coordinators work with schedulers to:</span></span>

- <span data-ttu-id="e9afd-126">Egy adott helyen leállítása repülőgép toocoincide karbantartási terv.</span><span class="sxs-lookup"><span data-stu-id="e9afd-126">Plan maintenance toocoincide with an aircraft stopping at a particular location.</span></span>
- <span data-ttu-id="e9afd-127">Gondoskodjon arról, hogy elegendő idő áll rendelkezésre hello repülőgép toobe nem működik az ütemezés megszakítása nélkül.</span><span class="sxs-lookup"><span data-stu-id="e9afd-127">Ensure sufficient time is available for hello aircraft toobe out of service without causing schedule disruption.</span></span>
- <span data-ttu-id="e9afd-128">tooschedule technikusok tooensure, hogy repülőgép a terjesztéskezelő hatékonyan várakozási idő nélkül.</span><span class="sxs-lookup"><span data-stu-id="e9afd-128">tooschedule technicians tooensure that aircraft are serviced efficiently without wait time.</span></span>

<span data-ttu-id="e9afd-129">A készletgazdálkodási vezetők karbantartási terveket kapnak, hogy optimalizálhassák a rendelési folyamatokat és a pótalkatrészek készletét.</span><span class="sxs-lookup"><span data-stu-id="e9afd-129">Inventory control managers receive maintenance plans, so they can optimize their ordering process and spare parts inventory.</span></span>

<span data-ttu-id="e9afd-130">Ezek a tevékenységek Fabrikam toominimize repülőgép ground idő engedélyezése, és a működési költségek csökkentése során a hello utasok és a személyzet biztonságát.</span><span class="sxs-lookup"><span data-stu-id="e9afd-130">These activities enable Fabrikam toominimize aircraft ground time and reduce operating costs while ensuring hello safety of passengers and crew.</span></span>

<span data-ttu-id="e9afd-131">toounderstand hogyan [Azure IoT Suite] [ lnk_iot_suite] biztosít hello képességek kell toorealize hello lehetőségeket kínál a prediktív karbantartási, tekintse át a [infographic] [lnk_infographic].</span><span class="sxs-lookup"><span data-stu-id="e9afd-131">toounderstand how [Azure IoT Suite][lnk_iot_suite] provides hello capabilities customers need toorealize hello potential of predictive maintenance, review this [infographic][lnk_infographic].</span></span>

## <a name="how-hello-predictive-maintenance-solution-is-built"></a><span data-ttu-id="e9afd-132">Hogyan hello prediktív karbantartási megoldás épül.</span><span class="sxs-lookup"><span data-stu-id="e9afd-132">How hello predictive maintenance solution is built</span></span>

<span data-ttu-id="e9afd-133">hello megoldást használja egy meglévő Azure Machine Learning modell egy sablon tooshow ezeket a képességeket IoT Suite szolgáltatás segítségével gyűjtött telemetriát dolgozik.</span><span class="sxs-lookup"><span data-stu-id="e9afd-133">hello solution uses an existing Azure Machine Learning model available as a template tooshow these capabilities working from device telemetry collected through IoT Suite services.</span></span> <span data-ttu-id="e9afd-134">A Microsoft létrehozta a [regressziós modell] [ lnk_regression_model] nyilvánosan elérhető adatok alapján repülőgép motor<sup>\[1\]</sup>, és lépésről lépésre Hogyan toouse hello modell útmutatást.</span><span class="sxs-lookup"><span data-stu-id="e9afd-134">Microsoft has built a [regression model][lnk_regression_model] of an aircraft engine based on publically available data<sup>\[1\]</sup>, and step-by-step guidance on how toouse hello model.</span></span>

<span data-ttu-id="e9afd-135">hello Azure IoT prediktív karbantartási megoldás a sablon alapján létrehozott hello regressziós modellt használja.</span><span class="sxs-lookup"><span data-stu-id="e9afd-135">hello Azure IoT predictive maintenance solution uses hello regression model created from this template.</span></span> <span data-ttu-id="e9afd-136">hello modell központilag telepítik az Azure-előfizetéshez, és elérhetővé tett egy automatikusan létrehozott API-n keresztül.</span><span class="sxs-lookup"><span data-stu-id="e9afd-136">hello model is deployed into your Azure subscription and exposed through an automatically generated API.</span></span> <span data-ttu-id="e9afd-137">hello megoldás tesztelési 4 (a teljes 100) képviselő adatokat hello egy részét tartalmazza motorok és hello 4 (az összesen 21) érzékelő adatfolyamot.</span><span class="sxs-lookup"><span data-stu-id="e9afd-137">hello solution includes a subset of hello testing data representing 4 (of 100 total) engines and hello 4 (of 21 total) sensor data streams.</span></span> <span data-ttu-id="e9afd-138">Ezek az adatok megfelelő tooprovide hello betanított modell egy pontos eredménye.</span><span class="sxs-lookup"><span data-stu-id="e9afd-138">This data is sufficient tooprovide an accurate result from hello trained model.</span></span>

<span data-ttu-id="e9afd-139">*\[1\] A. Saxena és K. Goebel (2008). „Turbofan Engine Degradation Simulation Data Set”, NASA Ames Prognostics Data Repository (http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/), NASA Ames Research Center, Moffett Field, CA*</span><span class="sxs-lookup"><span data-stu-id="e9afd-139">*\[1\] A. Saxena and K. Goebel (2008). "Turbofan Engine Degradation Simulation Data Set", NASA Ames Prognostics Data Repository (http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/), NASA Ames Research Center, Moffett Field, CA*</span></span>

## <a name="get-started-with-predictive-maintenance"></a><span data-ttu-id="e9afd-140">Ismerkedés a prediktív karbantartással</span><span class="sxs-lookup"><span data-stu-id="e9afd-140">Get started with predictive maintenance</span></span>

<span data-ttu-id="e9afd-141">Az oktatóanyag bemutatja, hogyan tooprovision hello prediktív karbantartási megoldás.</span><span class="sxs-lookup"><span data-stu-id="e9afd-141">This tutorial shows you how tooprovision hello predictive maintenance solution.</span></span> <span data-ttu-id="e9afd-142">Azt is bemutatja, hogyan hello prediktív karbantartási megoldás hello alapvető funkcióit.</span><span class="sxs-lookup"><span data-stu-id="e9afd-142">It also walks you through hello basic features of hello predictive maintenance solution.</span></span> <span data-ttu-id="e9afd-143">Ezek a szolgáltatások számos hello megoldás irányítópultja együtt előre konfigurált hello megoldás központi telepítését végző keresztül érheti el.</span><span class="sxs-lookup"><span data-stu-id="e9afd-143">You can access many of these features through hello solution dashboard that deploys along with hello preconfigured solution.</span></span>

<span data-ttu-id="e9afd-144">toocomplete ebben az oktatóanyagban aktív Azure-előfizetés szükséges.</span><span class="sxs-lookup"><span data-stu-id="e9afd-144">toocomplete this tutorial, you need an active Azure subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="e9afd-145">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="e9afd-145">If you don’t have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="e9afd-146">További információ: [Ingyenes Azure-fiók létrehozása][lnk_free_trial].</span><span class="sxs-lookup"><span data-stu-id="e9afd-146">For details, see [Azure Free Trial][lnk_free_trial].</span></span>

1. <span data-ttu-id="e9afd-147">Jelentkezzen be túl[azureiotsuite.com] [ lnk-azureiotsuite] az Azure használatával fiók hitelesítő adatait, és kattintson a  **+**  toocreate megoldást.</span><span class="sxs-lookup"><span data-stu-id="e9afd-147">Log on too[azureiotsuite.com][lnk-azureiotsuite] using your Azure account credentials, and click **+** toocreate a solution.</span></span>
1. <span data-ttu-id="e9afd-148">Kattintson a **válasszon** hello **prediktív karbantartási** csempére.</span><span class="sxs-lookup"><span data-stu-id="e9afd-148">Click **Select** hello **Predictive maintenance** tile.</span></span>
1. <span data-ttu-id="e9afd-149">Adja meg a **Megoldásnevet** az előre konfigurált prediktív karbantartási megoldáshoz.</span><span class="sxs-lookup"><span data-stu-id="e9afd-149">Enter a **Solution name** for your predictive maintenance preconfigured solution.</span></span>
1. <span data-ttu-id="e9afd-150">Jelölje be hello **régió** és **előfizetés** kívánt toouse tooprovision hello megoldás.</span><span class="sxs-lookup"><span data-stu-id="e9afd-150">Select hello **Region** and **Subscription** you want toouse tooprovision hello solution.</span></span>
1. <span data-ttu-id="e9afd-151">Kattintson a **megoldás létrehozása** toobegin hello létesítésének folyamatát kell használnia.</span><span class="sxs-lookup"><span data-stu-id="e9afd-151">Click **Create Solution** toobegin hello provisioning process.</span></span> <span data-ttu-id="e9afd-152">Ez a folyamat általában több percet toorun időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="e9afd-152">This process typically takes several minutes toorun.</span></span>

### <a name="wait-for-hello-provisioning-process-toocomplete"></a><span data-ttu-id="e9afd-153">Várjon, amíg a kiépítési folyamat toocomplete hello</span><span class="sxs-lookup"><span data-stu-id="e9afd-153">Wait for hello provisioning process toocomplete</span></span>

1. <span data-ttu-id="e9afd-154">Kattintson a megoldás a hello csempe **kiépítési** állapotát.</span><span class="sxs-lookup"><span data-stu-id="e9afd-154">Click hello tile for your solution with **Provisioning** status.</span></span>
1. <span data-ttu-id="e9afd-155">Értesítés hello **állapotok kiépítés** , Azure-szolgáltatások vannak telepítve az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="e9afd-155">Notice hello **Provisioning states** as Azure services are deployed in your Azure subscription.</span></span>
1. <span data-ttu-id="e9afd-156">Miután kiépítése befejeződött, hello állapotmódosítások túl**készen**.</span><span class="sxs-lookup"><span data-stu-id="e9afd-156">Once provisioning completes, hello status changes too**Ready**.</span></span>
1. <span data-ttu-id="e9afd-157">A megoldás hello jobb oldali ablaktáblában hello csempe toosee hello Részletek gombra.</span><span class="sxs-lookup"><span data-stu-id="e9afd-157">Click hello tile toosee hello details of your solution in hello right-hand pane.</span></span> <span data-ttu-id="e9afd-158">Ebben az ablaktáblában indítja el a hello megoldás-irányítópult és az access hello Machine Learning munkaterülettel.</span><span class="sxs-lookup"><span data-stu-id="e9afd-158">From this pane, you can launch hello solution dashboard and access hello Machine Learning workspace.</span></span>

> [!NOTE]
> <span data-ttu-id="e9afd-159">Ha hibát tapasztal előre konfigurált hello megoldás telepítésének, tekintse át a [hello azureiotsuite.com hely engedélyeinek] [ lnk-permissions] és hello [gyakran ismételt kérdések] [ lnk-faq].</span><span class="sxs-lookup"><span data-stu-id="e9afd-159">If you encounter issues deploying hello preconfigured solution, review [Permissions on hello azureiotsuite.com site][lnk-permissions] and hello [FAQ][lnk-faq].</span></span> <span data-ttu-id="e9afd-160">Ha hello problémák továbbra is fennáll, hozzon létre szolgáltatásjegyet a hello [portal][lnk-portal].</span><span class="sxs-lookup"><span data-stu-id="e9afd-160">If hello issues persist, create a service ticket on hello [portal][lnk-portal].</span></span>

<span data-ttu-id="e9afd-161">Vannak-e részletek toosee teheti meg, amelyek nem jelennek meg a megoldáshoz?</span><span class="sxs-lookup"><span data-stu-id="e9afd-161">Are there details you'd expect toosee that aren't listed for your solution?</span></span> <span data-ttu-id="e9afd-162">A [felhasználói visszajelzési webhelyen](https://feedback.azure.com/forums/321918-azure-iot) elküldheti a szolgáltatásokkal kapcsolatos javaslatait.</span><span class="sxs-lookup"><span data-stu-id="e9afd-162">Give us feature suggestions on [User Voice](https://feedback.azure.com/forums/321918-azure-iot).</span></span>

## <a name="view-hello-solution"></a><span data-ttu-id="e9afd-163">Hello megoldás megtekintése</span><span class="sxs-lookup"><span data-stu-id="e9afd-163">View hello solution</span></span>

<span data-ttu-id="e9afd-164">Ez a szakasz végigvezeti hello megoldás felhasználói felületén.</span><span class="sxs-lookup"><span data-stu-id="e9afd-164">This section guides you through hello solution UI.</span></span>

### <a name="predictive-maintenance-dashboard"></a><span data-ttu-id="e9afd-165">Prediktív karbantartási irányítópult</span><span class="sxs-lookup"><span data-stu-id="e9afd-165">Predictive Maintenance Dashboard</span></span>

<span data-ttu-id="e9afd-166">Ez a lap hello webalkalmazásban használ a Power bi JavaScript vezérlők (lásd: hello [Power bi-látványelemek tárház][lnk-powerbi]) toovisualize:</span><span class="sxs-lookup"><span data-stu-id="e9afd-166">This page in hello web application uses PowerBI JavaScript controls (see hello [PowerBI-visuals repository][lnk-powerbi]) toovisualize:</span></span>

* <span data-ttu-id="e9afd-167">a blob Storage tárolóban hello Stream Analytics-feladatok hello kimeneti adatait.</span><span class="sxs-lookup"><span data-stu-id="e9afd-167">hello output data from hello Stream Analytics jobs in blob storage.</span></span>
* <span data-ttu-id="e9afd-168">hello Szabályainak és ciklus (Event) számának repülőgép motor.</span><span class="sxs-lookup"><span data-stu-id="e9afd-168">hello RUL and cycle count per aircraft engine.</span></span>

### <a name="observing-hello-behavior-of-hello-cloud-solution"></a><span data-ttu-id="e9afd-169">Hello viselkedését betartásával hello felhőalapú megoldás</span><span class="sxs-lookup"><span data-stu-id="e9afd-169">Observing hello behavior of hello cloud solution</span></span>

<span data-ttu-id="e9afd-170">A hello Azure-portálon, keresse meg a toohello erőforráscsoport hello megoldás nevű úgy döntött, hogy tooview a kiosztott erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="e9afd-170">In hello Azure portal, navigate toohello resource group with hello solution name you chose tooview your provisioned resources.</span></span>

![][img-resource-group]

<span data-ttu-id="e9afd-171">Ha előre konfigurált hello megoldás, kap e-mailben található hivatkozásra toohello Machine Learning munkaterülettel.</span><span class="sxs-lookup"><span data-stu-id="e9afd-171">When you provision hello preconfigured solution, you receive an email with a link toohello Machine Learning workspace.</span></span> <span data-ttu-id="e9afd-172">Is megtalálhatja a hello toohello Machine Learning-munkaterület [azureiotsuite.com] [ lnk-azureiotsuite] lap kiosztott megoldást.</span><span class="sxs-lookup"><span data-stu-id="e9afd-172">You can also navigate toohello Machine Learning workspace from hello [azureiotsuite.com][lnk-azureiotsuite] page for your provisioned solution.</span></span> <span data-ttu-id="e9afd-173">Egy csempe esetén érhető el ezen a lapon hello megoldás a hello **készen** állapotát.</span><span class="sxs-lookup"><span data-stu-id="e9afd-173">A tile is available on this page when hello solution is in hello **Ready** state.</span></span>

![][img-machine-learning]

<span data-ttu-id="e9afd-174">Hello megoldás portálon láthatja, hogy hello minta ki van építve négy szimulált eszköz toorepresent két repülőgép / repülőgép, egyenként négy érzékelők két motorral.</span><span class="sxs-lookup"><span data-stu-id="e9afd-174">In hello solution portal, you can see that hello sample is provisioned with four simulated devices toorepresent two aircraft with two engines per aircraft, each with four sensors.</span></span> <span data-ttu-id="e9afd-175">Amikor először toohello megoldás portal, hello szimuláció le van állítva.</span><span class="sxs-lookup"><span data-stu-id="e9afd-175">When you first navigate toohello solution portal, hello simulation is stopped.</span></span>

![][img-simulation-stopped]

<span data-ttu-id="e9afd-176">Kattintson a **indítsa el a szimuláció** toobegin hello szimulálása.</span><span class="sxs-lookup"><span data-stu-id="e9afd-176">Click **Start simulation** toobegin hello simulation.</span></span> <span data-ttu-id="e9afd-177">hello érzékelő előzmények, Szabályainak, ciklusokat és Szabályainak előzmények hello irányítópult feltöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="e9afd-177">hello sensor history, RUL, Cycles, and RUL history populate hello dashboard.</span></span>

![][img-simulation-running]

<span data-ttu-id="e9afd-178">Ha Szabályainak kisebb, mint 160 (egy tetszőleges bemutatásra szolgál a kiválasztott küszöbérték), a hello megoldás portál egy figyelmeztetés szimbólum következő toohello Szabályainak megjelenítése jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="e9afd-178">When RUL is less than 160 (an arbitrary threshold chosen for demonstration purposes), hello solution portal displays a warning symbol next toohello RUL display.</span></span> <span data-ttu-id="e9afd-179">hello megoldás portálon is hello repülőgép motor sárga mutatja be.</span><span class="sxs-lookup"><span data-stu-id="e9afd-179">hello solution portal also highlights hello aircraft engine in yellow.</span></span> <span data-ttu-id="e9afd-180">Figyelje meg, hogyan hello Szabályainak értékek rendelkezik teljes általános csökkenő tendenciát, de az egyes toobounce felfelé és lefelé.</span><span class="sxs-lookup"><span data-stu-id="e9afd-180">Notice how hello RUL values have a general downward trend overall, but tend toobounce up and down.</span></span> <span data-ttu-id="e9afd-181">Ez a viselkedés hello különböző ciklus hosszak és hello modell pontosságát az eredménye.</span><span class="sxs-lookup"><span data-stu-id="e9afd-181">This behavior results from hello varying cycle lengths and hello model accuracy.</span></span>

![][img-simulation-warning]

<span data-ttu-id="e9afd-182">hello teljes szimuláció körülbelül 35 perc toocomplete 148 ciklusok vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="e9afd-182">hello full simulation takes around 35 minutes toocomplete 148 cycles.</span></span> <span data-ttu-id="e9afd-183">a hello hello 160 Szabályainak küszöbérték körülbelül 5 percig, először teljesül, és mindkét motorok hello küszöbérték találati körülbelül 8 perccel.</span><span class="sxs-lookup"><span data-stu-id="e9afd-183">hello 160 RUL threshold is met for hello first time at around 5 minutes and both engines hit hello threshold at around 8 minutes.</span></span>

<span data-ttu-id="e9afd-184">hello szimuláció keresztül hello teljes adatkészlet 148 ciklus fut, és a végső Szabályainak és ciklus értékek rendezi.</span><span class="sxs-lookup"><span data-stu-id="e9afd-184">hello simulation runs through hello complete dataset for 148 cycles and settles on final RUL and cycle values.</span></span>

<span data-ttu-id="e9afd-185">Hello szimuláció bármikor leállíthatja, pont, de a gombra kattintva **indítsa el a szimuláció** felhasználásait hello hello dataset hello indítás szimulálása.</span><span class="sxs-lookup"><span data-stu-id="e9afd-185">You can stop hello simulation at any point, but clicking **Start Simulation** replays hello simulation from hello start of hello dataset.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e9afd-186">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e9afd-186">Next steps</span></span>

<span data-ttu-id="e9afd-187">További információk miként prediktív karbantartási forgatókönyvben olvassa el az Azure IoT toolearn [hello az eszközök internetes hálózatát értéket rögzítése][lnk_capture_value].</span><span class="sxs-lookup"><span data-stu-id="e9afd-187">toolearn more about how Azure IoT enables predictive maintenance scenarios, read [Capture value from hello Internet of Things][lnk_capture_value].</span></span>

<span data-ttu-id="e9afd-188">Igénybe vehet egy [forgatókönyv] [ lnk-predictive-walkthrough] hello prediktív karbantartási megoldás.</span><span class="sxs-lookup"><span data-stu-id="e9afd-188">Take a [walkthrough][lnk-predictive-walkthrough] of hello predictive maintenance solution.</span></span>

<span data-ttu-id="e9afd-189">Akkor is is felfedezheti hello más szolgáltatásokat és képességeket hello előre konfigurált IoT Suite megoldások:</span><span class="sxs-lookup"><span data-stu-id="e9afd-189">You can also explore some of hello other features and capabilities of hello IoT Suite preconfigured solutions:</span></span>

* <span data-ttu-id="e9afd-190">[Gyakran ismételt kérdések az IoT Suite-ról][lnk-faq]</span><span class="sxs-lookup"><span data-stu-id="e9afd-190">[Frequently asked questions for IoT Suite][lnk-faq]</span></span>
* <span data-ttu-id="e9afd-191">[A hello IoT biztonsági szabad][lnk-security-groundup]</span><span class="sxs-lookup"><span data-stu-id="e9afd-191">[IoT security from hello ground up][lnk-security-groundup]</span></span>

[img-resource-group]: media/iot-suite-predictive-overview/resource-group.png
[img-simulation-stopped]: media/iot-suite-predictive-overview/simulation-stopped.png
[img-simulation-running]: media/iot-suite-predictive-overview/simulation-running.png
[img-simulation-warning]: media/iot-suite-predictive-overview/simulation-warning.png
[img-machine-learning]: media/iot-suite-predictive-overview/machine-learning.png

[lnk-powerbi]: https://www.github.com/Microsoft/PowerBI-visuals
[lnk-predictive-walkthrough]: iot-suite-predictive-walkthrough.md
[lnk_preconfigured_solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk_iot_suite]: iot-suite-overview.md
[lnk_infographic]: https://www.microsoft.com/server-cloud/predictivemaintenance/Index.html
[lnk_regression_model]: http://gallery.cortanaanalytics.com/Collection/Predictive-Maintenance-Template-3

[lnk_capture_value]: http://download.microsoft.com/download/0/7/D/07D394CE-185D-4B96-AC3C-9B61179F7080/Capture_value_from_the_Internet%20of%20Things_with_Predictive_Maintenance.PDF
[lnk-faq]: iot-suite-faq.md
[lnk-security-groundup]: securing-iot-ground-up.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-azureiotsuite]: https://www.azureiotsuite.com
[lnk-permissions]: iot-suite-permissions.md
[lnk-portal]: http://portal.azure.com/
[lnk-machine-learning]: https://azure.microsoft.com/services/machine-learning/