## Laitteisto
Macbook Pro M2 2023, jossa on MacOS Tahoe 26.2. Pyöritän Kali Linuxia VirtualBoxissa, jonka versio on 7.2.4. Kali Linux on taas ARM 64-bit. Kali Linuxissa on 8GB ram:ia, 2CPU corea pyörittämässä sitä ja 20GB tallennustilaa.

a) Break into 010-staff-only
Aloitin sivuston murtamisen tunnilla olevan ohjeen mukaan eli `value="OR+1=1--`. Se ei vielä antanut mitään. Huomasin että inputissa oli `type="number"`kohta ja kun tyhjensin sen ja kokeilin uudestaan niin se antoi minulle salasanan, mutta se ei ollut oikea, jota tehtävänannossa kuvailtiin.

<img width="1904" height="1552" alt="image" src="https://github.com/user-attachments/assets/15f68b0e-05f2-4e0a-98fe-bf7306a0650e" />
Tietämys minulta loppui siihen, mutta sain yhdeltä kurssilaiselta vinkin, että PortSwiggerin sivuilla saa neuvoja kyseiseen tehtävään. Sieltä löysin SQL injection UNION attacks kohdan, josta sain lisätietoa. Kokeilin tehtävää hieman eri tavalla. Ensin muutin `type="number"`kohdan tyhjäksi ja sitten kokeilin tällaista SQL lauseketta `value="'UNION SELECT password FROM pins--"`. Tämä antoi oikean tuloksen

<img width="1916" height="1202" alt="image" src="https://github.com/user-attachments/assets/b5db9be2-c256-4355-a0da-462a884a8046" />
