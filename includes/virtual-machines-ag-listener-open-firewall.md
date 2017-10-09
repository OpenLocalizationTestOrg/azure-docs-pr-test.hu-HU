Ebben a lépésben hoz létre szabályt tooopen hello mintavételi tűzfalportot hello elosztott terhelésű végpont (korábban megadott 59999) és egy másik szabály tooopen hello rendelkezésre állási csoport figyelőjének portszámára. Mivel hello elosztott terhelésű végpont a virtuális gépek rendelkezésre állási csoport replikái tartalmazó hello hozott létre, meg kell tooopen hello mintavételi portot és hello figyelő portjának hello megfelelő virtuális gépek.

1. Indítsa el a virtuális gépeken replika, **fokozott biztonságú Windows tűzfal**.

2. Kattintson a jobb gombbal **bejövő szabályok**, és kattintson a **új szabály**.

3. A hello **szabálytípus** lapon jelölje be **Port**, és kattintson a **következő**.

4. A hello **protokoll és portok** lapon jelölje be **TCP**, típus **59999** a hello **adott helyi portok** mezőbe, és kattintson a **Következő**.

5. A hello **művelet** lapon, tartsa **hello csatlakozás engedélyezése** kiválasztva, és kattintson **következő**.

6. A hello **profil** lapon fogadja el hello alapértelmezett beállításokat, és kattintson a **következő**.

7. A hello **neve** lap hello **neve** szöveg adja meg a szabály nevét, például a **mindig a figyelő mintavételi portot**, és kattintson a **Befejezés**.

8. Ismételje meg az előző lépésekben hello rendelkezésre állási csoport figyelőjének portszámára (ahogy korábban a hello parancsfájl hello $EndpointPort paraméter) a hello, majd adjon meg egy megfelelő szabályt, például a **mindig a figyelő Port**.

