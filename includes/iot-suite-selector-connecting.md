> [!div class="op_single_selector"]
> * [C Windowson](../articles/iot-suite/iot-suite-connecting-devices.md)
> * [C Linuxon](../articles/iot-suite/iot-suite-connecting-devices-linux.md)
> * [Node.js](../articles/iot-suite/iot-suite-connecting-devices-node.md)
> 
> 

## <a name="scenario-overview"></a>Forgatókönyv áttekintése
Ebben a forgatókönyvben a következő telemetriai toohello távoli megfigyelési hello küldő eszköz létrehozása [előre konfigurált megoldás][lnk-what-are-preconfig-solutions]:

* Külső hőmérséklet
* Belső hőmérséklet
* Páratartalom

Az egyszerűség kedvéért hello kód hello eszközön mintaértékek állít elő, de javasoljuk, tooextend hello minta valós érzékelők tooyour eszköz csatlakoztatása és valós telemetriai adatok küldését.

hello eszköz is, képes toorespond toomethods hello megoldás irányítópultja meghívni, és tulajdonságértékeket hello megoldás irányítópultjának beállítása szükséges.

toocomplete ebben az oktatóanyagban egy aktív Azure-fiókra van szüksége. Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot. További információ: [Ingyenes Azure-fiók létrehozása][lnk-free-trial].

## <a name="before-you-start"></a>Előkészületek
Mielőtt bármilyen kódot írna az eszközhöz, ki kell építenie az előre konfigurált távoli figyelési megoldást, és meg kell adnia egy új egyéni eszközt ebben a megoldásban.

### <a name="provision-your-remote-monitoring-preconfigured-solution"></a>Az előre konfigurált távoli figyelési megoldás kiépítése
Ebben az oktatóanyagban létrehozhat hello eszköz küldi hello adatok tooan példányának [távoli megfigyelési] [ lnk-remote-monitoring] előre konfigurált megoldás. Ha még nem már kiépített hello távoli felügyeleti előkonfigurált megoldás az Azure-fiókjába, használja a hello a következő lépéseket:

1. A hello <https://www.azureiotsuite.com/> kattintson  **+**  toocreate megoldást.
2. Kattintson a **válasszon** a hello **távoli megfigyelési** panel toocreate a megoldás.
3. A hello **létrehozása távoli felügyeleti megoldás** lapján adjon meg egy **megoldásnév** az Ön által választott, válassza ki a hello **régió** szeretné, hogy toodeploy, és válassza ki a hello Azure előfizetés toowant toouse. Ezután kattintson a **Megoldás létrehozása** parancsra.
4. Várjon, amíg a kiépítési folyamat hello befejeződik.

> [!WARNING]
> előre konfigurált hello megoldások számlázható Azure-szolgáltatásokat használja. Ne feledje tooremove hello előkonfigurált megoldás az előfizetésből, amikor elkészült, azt tooavoid a szükségtelen díjakat. Teljesen eltávolíthatja a előkonfigurált megoldás az előfizetésből hello felkeresésével <https://www.azureiotsuite.com/> lap.
> 
> 

Ha létesítésének folyamatát kell használnia a távoli felügyeleti megoldás hello hello befejeződik, kattintson a **indítása** tooopen hello megoldás irányítópultja a böngészőben.

![A megoldás irányítópultja][img-dashboard]

### <a name="provision-your-device-in-hello-remote-monitoring-solution"></a>Az eszköz a távoli felügyeleti megoldás hello kiépítése
> [!NOTE]
> Ha már kiépített egy eszközt a megoldásában, kihagyhatja ezt a lépést. Hello ügyfélalkalmazás létrehozásakor tooknow hello eszköz hitelesítő adatok szükségesek.
> 
> 

Eszköz tooconnect előre konfigurált toohello megoldás, azt kell azonosítani magát tooIoT Hub érvényes hitelesítő adatok használatával. Hello megoldás irányítópultja hello eszközhitelesítő adatok kérhetnek le. Hello eszközhitelesítő adatok szerepeljenek az ügyfélalkalmazás az oktatóanyag későbbi részében.

tooadd eszköz tooyour távoli figyelési megoldás, a következő teljes hello hello megoldás irányítópultja lépései:

1. Hello bal alsó sarkában hello irányítópultot, kattintson **hozzáad egy eszközt**.
   
   ![Eszköz hozzáadása][1]
2. A hello **egyéni eszköz** panelen, kattintson a **új hozzáadása**.
   
   ![Egyéni eszköz hozzáadása][2]
3. Válassza a **Meghatározom a saját eszközazonosítómat** elemet. Adja meg például az Eszközazonosítót **mydevice**, kattintson a **ellenőrizze azonosító** tooverify ugyanez a neve már nincs használatban, és kattintson a **létrehozása** tooprovision hello eszköz.
   
   ![Eszközazonosító hozzáadása][3]
4. Megjegyzés: hello eszköz tétele (Eszközazonosító IoT Hub állomásnév és eszközkulcs) hitelesítő adatait. Az ügyfélalkalmazás kell ezen értékek tooconnect toohello távoli felügyeleti megoldás. Ezután kattintson a **Done** (Kész) gombra.
   
    ![Eszköz hitelesítő adatainak megtekintése][4]
5. Jelölje ki az eszköz hello megoldás irányítópultjának hello eszközök listáját. Ezt követően a hello **eszközadatok** panelen, kattintson a **eszköz engedélyezése**. hello az eszköz állapota ezután **futtató**. Mostantól telemetriai adatok fogadhatók az eszközről és metódusok hello eszközön hello távoli figyelési megoldást igényelnek.

[img-dashboard]: ./media/iot-suite-selector-connecting/dashboard.png
[1]: ./media/iot-suite-selector-connecting/suite0.png
[2]: ./media/iot-suite-selector-connecting/suite1.png
[3]: ./media/iot-suite-selector-connecting/suite2.png
[4]: ./media/iot-suite-selector-connecting/suite3.png

[lnk-what-are-preconfig-solutions]: ../articles/iot-suite/iot-suite-what-are-preconfigured-solutions.md
[lnk-remote-monitoring]: ../articles/iot-suite/iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/