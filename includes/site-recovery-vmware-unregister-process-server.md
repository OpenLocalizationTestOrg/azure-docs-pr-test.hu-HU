hello lépéseket toounregister egy folyamatkiszolgálóra eltér attól függően, hogy a konfigurációs kiszolgáló hello kapcsolat állapotát.

### <a name="unregister-a-process-server-that-is-in-a-connected-state"></a>Csatlakoztatott állapotban lévő folyamatkiszolgáló regisztrációjának visszavonása

1. Távoli az hello folyamat kiszolgálóra rendszergazdaként.
2. Indítsa el a hello **Vezérlőpult** , és nyissa meg **programok > program eltávolítása**
3. Program eltávolítása hello nevű **Microsoft Azure Recovery konfigurációs/folyamat helykiszolgáló**
4. A 3. lépés befejezése után eltávolíthatja a **Microsoft Azure Site Recovery Configuration/Process Server Dependencies** elemet.

### <a name="unregister-a-process-server-that-is-in-a-disconnected-state"></a>Leválasztott állapotban lévő folyamatkiszolgáló regisztrációjának visszavonása

> [!WARNING]
> Hello használja a következő lépések kell használni, ha nincs módja toorevive hello virtuális gép mely hello folyamat kiszolgáló lett telepítve.

1. Rendszergazdaként jelentkezzen tooyour konfigurációs kiszolgálón.
2. Nyisson meg egy rendszergazdai parancssort, és keresse meg a toohello directory `%ProgramData%\ASR\home\svsystems\bin`.
3. Most futtassa hello parancsot.

    ```
    perl Unregister-ASRComponent.pl -IPAddress <IP_of_Process_Server> -Component PS
    ```
4. Hello rendszer hello hello folyamat-kiszolgáló adatait az törli.
