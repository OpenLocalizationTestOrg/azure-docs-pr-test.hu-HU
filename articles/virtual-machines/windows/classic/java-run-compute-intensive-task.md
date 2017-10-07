---
title: "a virtuális gép aaaCompute igényű Java-alkalmazás |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy Azure virtuális gépen futó számítási igényű Java-alkalmazás, amely egy másik Java-alkalmazás figyelhető-e."
services: virtual-machines-windows
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: ae6f2737-94c7-4569-9913-d871450c2827
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: Java
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: 02a198802a8d78bd444cd5a9197a78cb94f48e3b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toorun-a-compute-intensive-task-in-java-on-a-virtual-machine"></a>Hogyan toorun számítási igényű feladat a Java virtuális gépen
> [!IMPORTANT] 
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md). Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.

Az Azure-ral a virtuális gép toohandle számítási igénybe vevő feladatokat is használhatja. Például egy virtuális gép feladatokat és eredmények tooclient gépek vagy mobilalkalmazások. A cikk elolvasása után fog megértéséhez hogyan toocreate futtató számítási igényű Java-alkalmazások, amelyek virtuális gép egy másik Java-alkalmazás figyelhető-e.

Ez az oktatóanyag feltételezi, hogy tudja, hogyan toocreate Java konzol alkalmazások, könyvtárak tooyour Java-alkalmazások importálhatja, és hozhat létre egy Java-archívum (JAR). Nem ismeri a Microsoft Azure feltételezi.

Az oktatóanyagban érintett témák köre:

* Hogyan toocreate egy Java fejlesztői készlet (JDK) rendelkező virtuális gép már telepítve.
* Hogyan tooremotely tooyour virtuálisgép jelentkezni.
* Hogyan toocreate egy service bus névtér.
* Hogyan toocreate Java-alkalmazások számítás-igényes feladattá hajtja végre.
* Hogyan toocreate egy Java-alkalmazás, amely figyeli a hello hello számítási igényű feladat előrehaladását.
* Hogyan toorun hello Java-alkalmazások.
* Hogyan toostop hello Java-alkalmazások.

Ez az oktatóanyag hello számítási-igényes tevékenység hello Traveling értékesítők probléma fogja használni. hello az alábbiakban látható egy példa hello Java application futó hello számítási-igényes tevékenység.

![Traveling értékesítők probléma solver][solver_output]

hello az alábbiakban látható egy példa hello Java alkalmazás figyelési hello számítási igényű feladat.

![Traveling értékesítők probléma ügyfél][client_output]

[!INCLUDE [create-account-and-vms-note](../../../../includes/create-account-and-vms-note.md)]

## <a name="toocreate-a-virtual-machine"></a>a virtuális gépek toocreate
1. Jelentkezzen be toohello [a klasszikus Azure portálon](https://manage.windowsazure.com).
2. Kattintson a **új**, kattintson a **számítási**, kattintson a **virtuális gép**, és kattintson a **a gyűjtemény**.
3. A hello **virtuális gép lemezképét válasszon** párbeszédpanelen jelölje ki **JDK 7 a Windows Server 2012**.
   Vegye figyelembe, hogy **JDK 6 Windows Server 2012** áll rendelkezésre, amennyiben örökölt alkalmazásokat, amelyek még nem kész toorun JDK 7.
4. Kattintson a **Tovább** gombra.
5. A hello **virtuálisgép-konfiguráció** párbeszédpanel:
   1. Adja meg a hello virtuális gép nevét.
   2. Adja meg a hello mérete toouse hello virtuális géphez.
   3. Adja meg egy nevet a hello rendszergazda hello **felhasználónév** mező. Ezt a nevet és hello jelszót következő módba lép, használhatja őket toohello virtuális gép távoli bejelentkezéskor.
   4. Adjon meg egy jelszót hello **új jelszó** mezőben, majd adja meg újra a hello **megerősítése** mező. Ez a hello rendszergazdai fiók jelszavát.
   5. Kattintson a **Tovább** gombra.
6. Hello a következő **virtuálisgép-konfiguráció** párbeszédpanel:
   1. A **felhőalapú szolgáltatás**, hello alapértelmezett **hozzon létre egy új felhőalapú szolgáltatás**.
   2. a következő hello **felhőalapú szolgáltatás DNS-név** cloudapp.net belül egyedinek kell lennie. Szükség esetén módosítsa ezt az értéket úgy, hogy Azure azt jelzi, hogy egyedi.
   3. Adjon meg egy régiót, az affinitáscsoport vagy a virtuális hálózati. Ez az oktatóanyag céljából, adjon meg egy régiót például **USA nyugati régiója**.
   4. A **Tárfiók**, jelölje be **automatikusan létrehozott tárfiók használata**.
   5. A **rendelkezésre állási csoport**, jelölje be **(nincs)**.
   6. Kattintson a **Tovább** gombra.
7. A végső hello **virtuálisgép-konfiguráció** párbeszédpanel:
   1. Fogadja el a hello alapértelmezett végpont bejegyzéseket.
   2. Kattintson a **Befejezés** gombra.

## <a name="tooremotely-log-in-tooyour-virtual-machine"></a>a virtuális gép tooyour tooremotely napló
1. Jelentkezzen be toohello [a klasszikus Azure portálon](https://manage.windowsazure.com).
2. Kattintson a **virtuális gépek**.
3. Kattintson a kívánt toolog a hello virtuális gép hello nevére.
4. Kattintson a **Connect** (Csatlakozás) gombra.
5. Válaszoljon toohello kérni fogja a szükséges tooconnect toohello virtuális gépként. Amikor a rendszer kéri, hello rendszergazda felhasználónevét és jelszavát, hello virtuális gép létrehozásakor adott hello értékeket használja.

Vegye figyelembe, hogy hello Azure Service Bus-funkciókat igényli hello Baltimore CyberTrust legfelső szintű tanúsítvány toobe a JRE részeként telepített **cacerts** tárolja. Ez a tanúsítvány automatikusan hello Java-futtatókörnyezet (JRE) ebben az oktatóanyagban használt tartalmazza. Ha nincs ezt a tanúsítványt a a JRE **cacerts** tárolja, lásd: [hozzáadása a Java-hitelesítésszolgáltató tanúsítványtároló tanúsítvány toohello] [ add_ca_cert] hozzáadásával kapcsolatos (valamint információ a cacerts tárolójában hello tanúsítványok megtekintése).

## <a name="how-toocreate-a-service-bus-namespace"></a>Hogyan toocreate egy service bus névtér
a Service Bus használatával toobegin várólisták az Azure-ban, először létre kell hoznia egy szolgáltatásnévteret. A névtér egy hatókörkezelési tárolót biztosít az alkalmazáson belül a Service Bus erőforrásainak kezeléséhez.

a névtér toocreate:

1. Jelentkezzen be toohello [a klasszikus Azure portálon](https://manage.windowsazure.com).
2. A klasszikus Azure portálon hello hello bal alsó navigációs ablaktábláján kattintson **Service Bus, a hozzáférés-vezérlés és a gyorsítótár**.
3. A klasszikus Azure portálon hello hello bal felső ablaktábláján kattintson hello **Service Bus** csomópont, és kattintson a hello **új** gombra.  
   ![Service Bus csomópont képernyőképe][svc_bus_node]
4. A hello **hozzon létre egy új szolgáltatás Namespace** párbeszédpanelen adja meg egy **Namespace**, és toomake meg arról, hogy a rendszer egyedi, kattintson a **ellenőrizze a rendelkezésre állási** gombra.  
   ![Hozzon létre egy új Namespace képernyőképe][create_namespace]
5. Miután meggyőződött arról, hogy hello névtér neve elérhető, válassza ki az országot vagy régiót, amelyben a névtér üzemeltetve lesz, és kattintson a hello **létrehozása Namespace** gombra.  
   
   hello létrehozott névtér hello a klasszikus Azure portálon megjelenik, és egy rövid ideig tooactivate vesz igénybe. Várjon, amíg a hello állapota **aktív** a hello következő lépés elvégzése előtt.

## <a name="obtain-hello-default-management-credentials-for-hello-namespace"></a>Hello alapértelmezett felügyeleti hitelesítő adatok beszerzése hello névtér
A sorrend tooperform felügyeleti műveletek, például létrehozhat egy várólista hello új névtéren tooobtain hello felügyeleti hitelesítő adatokat a névtér kell.

1. Hello bal oldali navigációs panelen, kattintson a hello **Service Bus** csomópontra hello az elérhető névterek listájának megjelenítéséhez.
   ![Elérhető névterek képernyőképe][avail_namespaces]
2. Válassza ki a most létrehozott hello listán látható hello névteret.
   ![Namespace lista képernyőképe][namespace_list]
3. jobb oldali hello **tulajdonságok** ablaktábla listázza az új névtéren hello tulajdonságait.
   ![Képernyőfelvétel a Tulajdonságok panelen][properties_pane]
4. Hello **alapértelmezett kulcs** van-e rejtve. Kattintson a hello **nézet** gomb toodisplay hello hitelesítő adatokat.
   ![Alapértelmezett kulcs képernyőképe][default_key]
5. Jegyezze fel a hello **alapértelmezett kibocsátó** és hello **alapértelmezett kulcs** módon használja, akkor ez az információ alább tooperform műveletek a névteret.

## <a name="how-toocreate-a-java-application-that-performs-a-compute-intensive-task"></a>Hogyan toocreate számítási-igényes feladattá végző Java-alkalmazások
1. A fejlesztői számítógépén (amelyhez nem tartozik toobe hello virtuális gép, amelyet hozott létre), letöltési hello [Javához készült Azure SDK](https://azure.microsoft.com/develop/java/).
2. Hozzon létre egy Java konzolalkalmazást, ez a szakasz végén hello hello példa kód használatával. Ebben az oktatóanyagban fogjuk használni **TSPSolver.java** hello Java fájlnév. Hello módosítása **a\_szolgáltatás\_bus\_névtér**, **a\_szolgáltatás\_bus\_tulajdonos**, és **a\_szolgáltatás\_bus\_kulcs** toouse helyőrzőket a service bus **névtér**, **alapértelmezett kibocsátó** és  **Alapértelmezett kulcs** értékei, illetve.
3. Után kódolási, exportálás hello tooa futtatható Java archívumából (JAR), és a csomag hello szükséges hello a szalagtárak JAR jön létre. Ebben az oktatóanyagban fogjuk használni **TSPSolver.jar** generált hello JAR neveként.

<p/>

    // TSPSolver.java

    import com.microsoft.windowsazure.services.core.Configuration;
    import com.microsoft.windowsazure.services.core.ServiceException;
    import com.microsoft.windowsazure.services.serviceBus.*;
    import com.microsoft.windowsazure.services.serviceBus.models.*;
    import java.io.*;
    import java.text.DateFormat;
    import java.text.SimpleDateFormat;
    import java.util.ArrayList;
    import java.util.Date;
    import java.util.List;

    public class TSPSolver {

        //  Value specifying how often tooprovide an update toohello console.
        private static long loopCheck = 100000000;  

        private static long nTimes = 0, nLoops=0;

        private static double[][] distances;
        private static String[] cityNames;
        private static int[] bestOrder;
        private static double minDistance;
        private static ServiceBusContract service;

        private static void buildDistances(String fileLocation, int numCities) throws Exception{
            try{
                BufferedReader file = new BufferedReader(new InputStreamReader(new DataInputStream(new FileInputStream(new File(fileLocation)))));
                double[][] cityLocs = new double[numCities][2];
                for (int i = 0; i<numCities; i++){
                    String[] line = file.readLine().split(", ");
                    cityNames[i] = line[0];
                    cityLocs[i][0] = Double.parseDouble(line[1]);
                    cityLocs[i][1] = Double.parseDouble(line[2]);
                }
                for (int i = 0; i<numCities; i++){
                    for (int j = i; j<numCities; j++){
                        distances[i][j] = Math.hypot(Math.abs(cityLocs[i][0] - cityLocs[j][0]), Math.abs(cityLocs[i][1] - cityLocs[j][1]));
                        distances[j][i] = distances[i][j];
                    }
                }
            } catch (Exception e){
                throw e;
            }
        }

        private static void permutation(List<Integer> startCities, double distSoFar, List<Integer> restCities) throws Exception {

            try
            {
                nTimes++;
                if (nTimes == loopCheck)
                {
                    nLoops++;
                    nTimes = 0;
                    DateFormat dateFormat = new SimpleDateFormat("MM/dd/yyyy HH:mm:ss");
                    Date date = new Date();
                    System.out.print("Current time is " + dateFormat.format(date) + ". ");
                    System.out.println(  "Completed " + nLoops + " iterations of size of " + loopCheck + ".");
                }

                if ((restCities.size() == 1) && ((minDistance == -1) || (distSoFar + distances[restCities.get(0)][startCities.get(0)] + distances[restCities.get(0)][startCities.get(startCities.size()-1)] < minDistance))){
                    startCities.add(restCities.get(0));
                    newBestDistance(startCities, distSoFar + distances[restCities.get(0)][startCities.get(0)] + distances[restCities.get(0)][startCities.get(startCities.size()-2)]);
                    startCities.remove(startCities.size()-1);
                }
                else{
                    for (int i=0; i<restCities.size(); i++){
                        startCities.add(restCities.get(0));
                        restCities.remove(0);
                        permutation(startCities, distSoFar + distances[startCities.get(startCities.size()-1)][startCities.get(startCities.size()-2)],restCities);
                        restCities.add(startCities.get(startCities.size()-1));
                        startCities.remove(startCities.size()-1);
                    }
                }
            }
            catch (Exception e)
            {
                throw e;
            }
        }

        private static void newBestDistance(List<Integer> cities, double distance) throws ServiceException, Exception {
            try
            {
                minDistance = distance;
                String cityList = "Shortest distance is "+minDistance+", with route: ";
                for (int i = 0; i<bestOrder.length; i++){
                    bestOrder[i] = cities.get(i);
                    cityList += cityNames[bestOrder[i]];
                    if (i != bestOrder.length -1)
                        cityList += ", ";
                }
                System.out.println(cityList);
                service.sendQueueMessage("TSPQueue", new BrokeredMessage(cityList));
            }
            catch (ServiceException se)
            {
                throw se;
            }
            catch (Exception e)
            {
                throw e;
            }
        }

        public static void main(String args[]){

            try {

                Configuration config = ServiceBusConfiguration.configureWithWrapAuthentication(
                        "your_service_bus_namespace", "your_service_bus_owner",
                        "your_service_bus_key",
                        ".servicebus.windows.net",
                        "-sb.accesscontrol.windows.net/WRAPv0.9");

                service = ServiceBusService.create(config);

                int numCities = 10;  // Use as hello default, if no value is specified at command line.
                if (args.length != 0)
                {
                    if (args[0].toLowerCase().compareTo("createqueue")==0)
                    {
                        // No processing toooccur other than creating hello queue.
                        QueueInfo queueInfo = new QueueInfo("TSPQueue");

                        service.createQueue(queueInfo);

                        System.out.println("Queue named TSPQueue was created.");

                        System.exit(0);
                    }

                    if (args[0].toLowerCase().compareTo("deletequeue")==0)
                    {
                        // No processing toooccur other than deleting hello queue.
                        service.deleteQueue("TSPQueue");

                        System.out.println("Queue named TSPQueue was deleted.");

                        System.exit(0);
                    }

                    // Neither creating or deleting a queue.
                    // Assume hello value passed in is hello number of cities toosolve.
                    numCities = Integer.valueOf(args[0]);  
                }

                System.out.println("Running for " + numCities + " cities.");

                List<Integer> startCities = new ArrayList<Integer>();
                List<Integer> restCities = new ArrayList<Integer>();
                startCities.add(0);
                for(int i = 1; i<numCities; i++)
                    restCities.add(i);
                distances = new double[numCities][numCities];
                cityNames = new String[numCities];
                buildDistances("c:\\TSP\\cities.txt", numCities);
                minDistance = -1;
                bestOrder = new int[numCities];
                permutation(startCities, 0, restCities);
                System.out.println("Final solution found!");
                service.sendQueueMessage("TSPQueue", new BrokeredMessage("Complete"));
            }
            catch (ServiceException se)
            {
                System.out.println(se.getMessage());
                se.printStackTrace();
                System.exit(-1);
            }
            catch (Exception e)
            {
                System.out.println(e.getMessage());
                e.printStackTrace();
                System.exit(-1);
            }
        }

    }



## <a name="how-toocreate-a-java-application-that-monitors-hello-progress-of-hello-compute-intensive-task"></a>Hogyan toocreate egy Java-alkalmazás, amely figyeli a hello hello számítási-igényes tevékenység végrehajtási állapota
1. A fejlesztői számítógépén ez a szakasz végén hello hello példakódot használatával Java Konzolalkalmazás létrehozása. Ebben az oktatóanyagban fogjuk használni **TSPClient.java** hello Java fájlnév. Ahogy korábban is látható, módosítsa a hello **a\_szolgáltatás\_bus\_névtér**, **a\_szolgáltatás\_bus\_tulajdonos**, és **a\_szolgáltatás\_bus\_kulcs** toouse helyőrzőket a service bus **névtér**, **alapértelmezett kibocsátó**és **alapértelmezett kulcs** értékei, illetve.
2. Hello alkalmazás tooa exportálás futtatható JAR, és a csomag hello szükséges hello a szalagtárak JAR jön létre. Ebben az oktatóanyagban fogjuk használni **TSPClient.jar** generált hello JAR neveként.

<p/>

    // TSPClient.java

    import java.util.Date;
    import java.text.DateFormat;
    import java.text.SimpleDateFormat;
    import com.microsoft.windowsazure.services.serviceBus.*;
    import com.microsoft.windowsazure.services.serviceBus.models.*;
    import com.microsoft.windowsazure.services.core.*;

    public class TSPClient
    {

        public static void main(String[] args)
        {
                try
                {

                    DateFormat dateFormat = new SimpleDateFormat("MM/dd/yyyy HH:mm:ss");
                    Date date = new Date();
                    System.out.println("Starting at " + dateFormat.format(date) + ".");

                    String namespace = "your_service_bus_namespace";
                    String issuer = "your_service_bus_owner";
                    String key = "your_service_bus_key";

                    Configuration config;
                    config = ServiceBusConfiguration.configureWithWrapAuthentication(
                            namespace, issuer, key,
                            ".servicebus.windows.net",
                            "-sb.accesscontrol.windows.net/WRAPv0.9");

                    ServiceBusContract service = ServiceBusService.create(config);

                    BrokeredMessage message;

                    int waitMinutes = 3;  // Use as hello default, if no value is specified at command line.
                    if (args.length != 0)
                    {
                        waitMinutes = Integer.valueOf(args[0]);  
                    }

                    String waitString;

                    waitString = (waitMinutes == 1) ? "minute." : waitMinutes + " minutes.";

                    // This queue must have previously been created.
                    service.getQueue("TSPQueue");

                    int numRead;

                    String s = null;

                    while (true)
                    {

                        ReceiveQueueMessageResult resultQM = service.receiveQueueMessage("TSPQueue");
                        message = resultQM.getValue();

                        if (null != message && null != message.getMessageId())
                        {

                            // Display hello queue message.
                            byte[] b = new byte[200];

                            System.out.print("From queue: ");

                            s = null;
                            numRead = message.getBody().read(b);
                            while (-1 != numRead)
                            {
                                s = new String(b);
                                s = s.trim();
                                System.out.print(s);
                                numRead = message.getBody().read(b);
                            }
                            System.out.println();
                            if (s.compareTo("Complete") == 0)
                            {
                                // No more processing toooccur.
                                date = new Date();
                                System.out.println("Finished at " + dateFormat.format(date) + ".");
                                break;
                            }
                        }
                        else
                        {
                            // hello queue is empty.
                            System.out.println("Queue is empty. Sleeping for another " + waitString);
                            Thread.sleep(60000 * waitMinutes);
                        }
                    }

            }
            catch (ServiceException se)
            {
                System.out.println(se.getMessage());
                se.printStackTrace();
                System.exit(-1);
            }
            catch (Exception e)
            {
                System.out.println(e.getMessage());
                e.printStackTrace();
                System.exit(-1);
            }

        }

    }

## <a name="how-toorun-hello-java-applications"></a>Hogyan toorun hello Java-alkalmazások
Hello számítási igényű alkalmazás futtatásához, első toocreate hello várólistát, majd toosolve hello utazás Saleseman probléma, amely felveszi hello aktuális legjobb útvonalat toohello service bus-üzenetsorba. Hello közben számítási igényű alkalmazásra az fut (vagy ezek után), futtatási hello ügyfél toodisplay eredmények hello service bus-üzenetsorba.

### <a name="toorun-hello-compute-intensive-application"></a>toorun hello számítási igényű alkalmazás
1. Jelentkezzen be a virtuális gép tooyour.
2. Hozzon létre egy mappát, ahol az alkalmazás elindul. Például **c:\TSP**.
3. Másolás **TSPSolver.jar** túl**c:\TSP**,
4. Hozzon létre egy fájlt **c:\TSP\cities.txt** a tartalom a következő hello.
   
        City_1, 1002.81, -1841.35
        City_2, -953.55, -229.6
        City_3, -1363.11, -1027.72
        City_4, -1884.47, -1616.16
        City_5, 1603.08, -1030.03
        City_6, -1555.58, 218.58
        City_7, 578.8, -12.87
        City_8, 1350.76, 77.79
        City_9, 293.36, -1820.01
        City_10, 1883.14, 1637.28
        City_11, -1271.41, -1670.5
        City_12, 1475.99, 225.35
        City_13, 1250.78, 379.98
        City_14, 1305.77, 569.75
        City_15, 230.77, 231.58
        City_16, -822.63, -544.68
        City_17, -817.54, -81.92
        City_18, 303.99, -1823.43
        City_19, 239.95, 1007.91
        City_20, -1302.92, 150.39
        City_21, -116.11, 1933.01
        City_22, 382.64, 835.09
        City_23, -580.28, 1040.04
        City_24, 205.55, -264.23
        City_25, -238.81, -576.48
        City_26, -1722.9, -909.65
        City_27, 445.22, 1427.28
        City_28, 513.17, 1828.72
        City_29, 1750.68, -1668.1
        City_30, 1705.09, -309.35
        City_31, -167.34, 1003.76
        City_32, -1162.85, -1674.33
        City_33, 1490.32, 821.04
        City_34, 1208.32, 1523.3
        City_35, 18.04, 1857.11
        City_36, 1852.46, 1647.75
        City_37, -167.44, -336.39
        City_38, 115.4, 0.2
        City_39, -66.96, 917.73
        City_40, 915.96, 474.1
        City_41, 140.03, 725.22
        City_42, -1582.68, 1608.88
        City_43, -567.51, 1253.83
        City_44, 1956.36, 830.92
        City_45, -233.38, 909.93
        City_46, -1750.45, 1940.76
        City_47, 405.81, 421.84
        City_48, 363.68, 768.21
        City_49, -120.3, -463.13
        City_50, 588.51, 679.33
5. Egy parancssorban módosítsa a könyvtárak tooc:\TSP.
6. Győződjön meg arról, hello JRE bin mappájában megtalálható-e hello PATH környezeti változóhoz.
7. Hello Szolgáltató solver kombinációinak futtatása előtt kell toocreate hello service bus-üzenetsorba. Futtassa a következő parancs toocreate hello service bus-üzenetsorba hello.
   
        java -jar TSPSolver.jar createqueue
8. Most, hogy hello várólista létrehozása hello Szolgáltató solver kombinációinak is futtathatja. Például futtassa a következő parancs toorun hello solver 8 városokat hello.
   
        java -jar TSPSolver.jar 8
   
   Ha nem adja meg egy számot, akkor a 10 városokat fog futni. Hello solver aktuális legrövidebb útvonalak talál, mivel azt hozzáadja azokat toohello várólista.

> [!NOTE]
> nagyobb hello hello száma, hogy megadja, hello hosszabb hello solver fog futni. Például futtató 14 városokat eltarthat néhány percig, és fut, amely 15 várost több óráig is eltarthat. Növelése too16 vagy több város tartozik (idővel hét, hónap és év) futásidejű napon eredményezheti. Ez a gyors növekedése toohello hello városokat növekszik hello száma hello Solver kiértékelése kombinációinak száma miatt.
> 
> 

### <a name="how-toorun-hello-monitoring-client-application"></a>Hogyan toorun hello figyelési ügyfélalkalmazás
1. Jelentkezzen be tooyour gép hello ügyfélalkalmazást futtató lesz. Ez nem szükséges toobe hello egyazon számítógépen futó hello **TSPSolver** alkalmazás, bár lehet.
2. Hozzon létre egy mappát, ahol az alkalmazás elindul. Például **c:\TSP**.
3. Másolás **TSPClient.jar** túl**c:\TSP**,
4. Győződjön meg arról, hello JRE bin mappájában megtalálható-e hello PATH környezeti változóhoz.
5. Egy parancssorban módosítsa a könyvtárak tooc:\TSP.
6. Futtassa a következő parancs hello.
   
        java -jar TSPClient.jar
   
    Szükség esetén adja meg percben toosleep Between hello várólista, a parancssori argumentum történő ellenőrzése hello számát. hello alapértelmezett alvó időszak hello várólista ellenőrzése a következő: 3 percenként, amely akkor használatos, ha nincs parancssori argumentum túl átadása**TSPClient**. Ha toouse egy másik értéket hello alvó időtartam alatt, például egy percet, futtassa a következő parancs hello.
   
        java -jar TSPClient.jar 1
   
    hello ügyfél fog futni, amíg azt egy üzenetet várólista a "Teljes". Ne feledje, hogy ha több példányban hello solver hello ügyfél futtatása nélkül futtatja, előfordulhat, hogy toorun hello ügyfél többször toocompletely üres hello várólista. Másik lehetőségként hello várólista törlése, és akkor hozza létre újra. toodelete hello várólista, futtassa a következő hello **TSPSolver** (nem **TSPClient**) parancsot.
   
        java -jar TSPSolver.jar deletequeue
   
    hello solver fog futni, amíg be nem fejezi megvizsgálta az összes útvonal.

## <a name="how-toostop-hello-java-applications"></a>Hogyan toostop hello Java-alkalmazások
Hello solver és ügyfélalkalmazások lenyomhatja **Ctrl + C** tooexit, ha azt szeretné, hogy tooend előzetes toonormal befejezését.

[solver_output]:media/java-run-compute-intensive-task/WA_JavaTSPSolver.png
[client_output]:media/java-run-compute-intensive-task/WA_JavaTSPClient.png
[svc_bus_node]:media/java-run-compute-intensive-task/SvcBusQueues_02_SvcBusNode.jpg
[create_namespace]:media/java-run-compute-intensive-task/SvcBusQueues_03_CreateNewSvcNamespace.jpg
[avail_namespaces]:media/java-run-compute-intensive-task/SvcBusQueues_04_SvcBusNode_AvailNamespaces.jpg
[namespace_list]:media/java-run-compute-intensive-task/SvcBusQueues_05_NamespaceList.jpg
[properties_pane]:media/java-run-compute-intensive-task/SvcBusQueues_06_PropertiesPane.jpg
[default_key]:media/java-run-compute-intensive-task/SvcBusQueues_07_DefaultKey.jpg
[add_ca_cert]: ../../../java-add-certificate-ca-store.md
