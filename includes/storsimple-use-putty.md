<!--author=SharS last changed: 9/17/15-->

#### <a name="tooconnect-through-hello-serial-console"></a>tooconnect hello soros konzolon keresztül
1. Csatlakoztassa a soros kábelt toohello eszköz (közvetlenül vagy egy USB – soros adapteren keresztül).
2. Nyissa meg hello **Vezérlőpult**, majd nyissa meg a hello **Eszközkezelő**.
3. Azonosítsa hello COM-port a hello a következő ábrán látható módon.
   
     ![Csatlakozás soros konzolon keresztül](./media/storsimple-use-putty/HCS_ConnectingDeviceS-include.png)
4. Indítsa el a PuTTY alkalmazást. 
5. Hello jobb oldali ablaktáblában módosíthatja a hello **kapcsolattípus** túl**soros**.
6. Hello jobb oldali ablaktáblában írja be a hello megfelelő COM-portot. Győződjön meg arról, hogy hello soros konfiguráció paraméterei az alábbiak szerint vannak-e beállítva:
   
   * Sebesség: 115 200
   * Adatbitek: 8
   * Stopbitek: 1
   * Paritás: Nincs
   * Folyamatvezérlés: Nincs.
     
     Ezek a beállítások hello a következő ábrán láthatók.
     
     ![PuTTY-beállítások](./media/storsimple-use-putty/HCS_PuttyConfig-include.png) 
     
     > [!NOTE]
     > Ha hello alapértelmezett folyamatvezérlési beállítás nem működik, próbálja meg hello adatfolyam vezérlés tooXON/XOFF értékre.
     > 
     > 
7. Kattintson a **nyitott** toostart soros munkamenet.

