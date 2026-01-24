## Laitteisto
Macbook Pro M2 2023, jossa on MacOS Tahoe 26.2. Pyöritän Kali Linuxia VirtualBoxissa, jonka versio on 7.2.4. Kali Linux on taas ARM 64-bit. Kali Linuxissa on 8GB ram:ia, 2CPU corea pyörittämässä sitä ja 20GB tallennustilaa.

a) Break into 010-staff-only
Aloitin sivuston murtamisen tunnilla olevan ohjeen mukaan eli `value="OR+1=1--`. Se ei vielä antanut mitään. Huomasin että inputissa oli `type="number"`kohta ja kun tyhjensin sen ja kokeilin uudestaan niin se antoi minulle salasanan, mutta se ei ollut oikea, jota tehtävänannossa kuvailtiin:

<img width="1904" height="1552" alt="image" src="https://github.com/user-attachments/assets/15f68b0e-05f2-4e0a-98fe-bf7306a0650e" />
Tietämys minulta loppui siihen, mutta sain yhdeltä kurssilaiselta vinkin, että PortSwiggerin sivuilla saa neuvoja kyseiseen tehtävään. Sieltä löysin SQL injection UNION attacks kohdan, josta sain lisätietoa. Kokeilin tehtävää hieman eri tavalla. Ensin muutin `type="number"`kohdan tyhjäksi ja sitten kokeilin tällaista SQL lauseketta `value="'UNION SELECT password FROM pins--"`. Tämä antoi oikean tuloksen:

<img width="1916" height="1202" alt="image" src="https://github.com/user-attachments/assets/b5db9be2-c256-4355-a0da-462a884a8046" />

b) Fix the 010-staff-only vulnerability from source code
Avasin source-koodin nano-teksti editorissa.
<img width="1828" height="1196" alt="image" src="https://github.com/user-attachments/assets/17647bc9-c5ea-4947-89a2-bd1405a73661" />
Korjasin haavoittovuuden koodista muuttamalla: sql = "SELECT password FROM pins WHERE pin= :pin;" ja res=db.session.execute(text(sql),{"pin": pin})

<img width="1052" height="802" alt="image" src="https://github.com/user-attachments/assets/5f5090a5-d1ef-44b3-8569-793b72f9549b" />

Kun testasin samaa SQL injectonia niin en saannut salasanaa näkyville.

<img width="1730" height="1198" alt="image" src="https://github.com/user-attachments/assets/bfa5e4d9-359b-4208-8aa4-686d5a101dd0" />





