> [!div class="op_single_selector"]
> * [Linux](../articles/iot-hub/iot-hub-linux-iot-edge-get-started.md)
> * [Windows](../articles/iot-hub/iot-hub-windows-iot-edge-get-started.md)
> 
> 

Ez a cikk ismerteti a hello részletes bemutatóért [Hello World mintakód] [ lnk-helloworld-sample] tooillustrate hello alapvető összetevői hello [Azure IoT peremhálózati] [ lnk-iot-edge] architektúra. hello minta hello Azure IoT peremhálózati toobuild egy egyszerű átjárót használja, amelyre bejelentkezik a "hello world" üzenet tooa fájl, ötpercenként.

A bemutató tartalma:

* **Hello World – mintaarchitektúra**: ismerteti, hogyan [Azure IoT peremhálózati architekturális fogalmak] [ lnk-edge-concepts] toohello Hello World PéldaAlkalmazás, és hogyan hello összetevők működnek együtt a vonatkoznak.
* **Hogyan toobuild hello minta**: hello lépéseket szükséges toobuild hello minta.
* **Hogyan toorun hello minta**: hello lépéseket szükséges toorun hello minta. 
* **Tipikus kimeneti**: hello például kimeneti tooexpect hello minta futtatásakor.
* **Kód kódtöredékek**: hogyan hello Hello World PéldaAlkalmazás megvalósító kódot kódtöredékek tooshow gyűjteménye kulcs IoT peremhálózati átjáró összetevőket.


## <a name="hello-world-sample-architecture"></a>Hello World mintaarchitektúra
hello Hello World PéldaAlkalmazás hello előző szakaszban leírt hello fogalmak mutatja be. hello Hello World PéldaAlkalmazás valósít meg olyan IoT peremhálózati átjáróval, amely rendelkezik egy folyamat két IoT peremhálózati modulok áll:

* Hello *hello world* modul létrehoz egy üzenetet, ötpercenként, és átadja toohello naplózó modul.
* Hello *naplózó* modul írási műveletek köszönőüzenetei tooa fájl kap.

![Példa az Azure IoT Edge segítségével összeállított Hello World- architektúrára][4]

Hello előző szakaszban leírtak a modul nem felel meg a Hello World hello üzenetek közvetlenül toohello naplózó modul öt másodpercenként. Ehelyett azt tesz közzé egy üzenet toohello broker öt másodpercenként.

hello naplózó modul hello broker hello üzenetet kap, és kezeli rá hello üzenet tooa fájl hello tartalmának írása.

hello naplózó modul csak akkor hello broker származó üzenetek, új üzenetek toohello broker soha nem teszi közzé.

![Hogyan hello broker üzenetirányítást végez az Azure IoT Edge modulok között][5]

hello a fenti ábrán architektúráját mutatja be hello hello Hello World PéldaAlkalmazás és hello relatív elérési toohello forrásfájlok hello különböző részeit hello mintát megvalósító [tárház][lnk-iot-edge]. Hello kód saját elképzelései, vagy használjon hello kódrészletek, alá útmutatóként.

<!-- Images -->
[4]: media/iot-hub-iot-edge-getstarted-selector/high_level_architecture.png
[5]: media/iot-hub-iot-edge-getstarted-selector/detailed_architecture.png

<!-- Links -->
[lnk-helloworld-sample]: https://github.com/Azure/iot-edge/tree/master/samples/hello_world
[lnk-iot-edge]: https://github.com/Azure/iot-edge
[lnk-edge-concepts]: ../articles/iot-hub/iot-hub-iot-edge-overview.md