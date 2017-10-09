<!--author=alkohli last changed: 9/16/15-->


#### <a name="toocable-your-device-for-power"></a>toocable power az eszköz
> [!NOTE]
> A StorSimple eszköz mindkét ház redundáns PCMs tartalmazza. Az egyes ház hello PCMs telepíteni kell, és toodifferent power források tooensure magas rendelkezésre állású csatlakoztatva.
> 
> 

1. Győződjön meg arról, hogy minden hello PCMs hello power kapcsolókkal hello OFF állásban van-e.
2. Hello elsődleges ház, a csatlakozás hello power zsinór tooboth PCMs. az alábbi ábrán, kábelek hello power piros hello tápkábelek azonosítja.
3. Győződjön meg arról, hogy a hello elsődleges ház használata külön áramforrásokból két PCMs hello.
4. Hello power zsinór toohello bekapcsolás hello állvány Áramelosztó egységekből, ahogy az ábra kábelek hello power csatolni.
5. Hello EBOD ház ismételje meg a 2 – 4.
6. Kapcsolja be a hello EBOD ház minden PCM toohello ON el a kikapcsolás hello átállításával.
7. Győződjön meg arról, hogy hello EBOD ház úgy, hogy a zöld hello LED hello hello EBOD vezérlő oldalán a vannak-e kapcsolva a ON be van kapcsolva.
8. Kapcsolja be a hello elsődleges ház által tükrözés minden PCM kapcsoló toohello ON el.
9. Győződjön meg arról, hogy hello rendszer a hello eszköz vezérlő LED be van kapcsolva biztosításával.
10. Ellenőrizze, hogy hello kapcsolat hello EBOD vezérlő és hello eszköz vezérlő aktív szerint annak ellenőrzése, hogy hello négy LED következő toohello SAS-portjához hello EBOD vezérlő zöld.
    
    > [!IMPORTANT]
    > tooensure magas rendelkezésre állás a rendszerhez, ajánlott toohello energiagazdálkodási séma hello a következő ábrán látható kábelek szigorúan igazodik.
    > 
    > 
    
    ![Az energiagazdálkodási 4U eszköz bekábelezése](./media/storsimple-cable-8600-for-power/HCSCableYour4UDeviceforPower.png)
    
    **Energiagazdálkodási kábelek**
    
    | Címke | Leírás |
    |:--- |:--- |
    | 1 |Elsődleges ház |
    | 2 |PCM 0 |
    | 3 |PCM 1 |
    | 4 |A vezérlő 0 |
    | 5 |1. vezérlő |
    | 6 |EBOD vezérlő 0 |
    | 7 |1. EBOD vezérlő |
    | 8 |EBOD ház |
    | 9 |PDU-k |

