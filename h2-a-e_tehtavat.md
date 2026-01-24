## Laitteisto
Macbook Pro M2 2023, jossa on MacOS Tahoe 26.2. Pyöritän Kali Linuxia VirtualBoxissa, jonka versio on 7.2.4. Kali Linux on taas ARM 64-bit. Kali Linuxissa on 8GB ram:ia, 2CPU corea pyörittämässä sitä ja 20GB tallennustilaa.

a) Break into 010-staff-only
Aloitin sivuston murtamisen tunnilla olevan ohjeen mukaan eli `value="OR+1=1--`. Se ei vielä antanut mitään. Huomasin että inputissa oli `type="number"`kohta ja kun tyhjensin sen ja kokeilin uudestaan niin se antoi minulle salasanan, mutta se ei ollut oikea, jota tehtävänannossa kuvailtiin:

<img width="1904" height="1552" alt="image" src="https://github.com/user-attachments/assets/15f68b0e-05f2-4e0a-98fe-bf7306a0650e" />
Tietämys minulta loppui siihen, mutta sain yhdeltä kurssilaiselta vinkin, että PortSwiggerin sivuilla saa neuvoja kyseiseen tehtävään. Sieltä löysin SQL injection UNION attacks kohdan, josta sain lisätietoa. Kokeilin tehtävää hieman eri tavalla. Ensin muutin `type="number"`kohdan tyhjäksi ja sitten kokeilin tällaista SQL lauseketta `value="'UNION SELECT password FROM pins--"`. Tämä antoi oikean tuloksen:

<img width="1916" height="1202" alt="image" src="https://github.com/user-attachments/assets/b5db9be2-c256-4355-a0da-462a884a8046" />

b) Fix the 010-staff-only vulnerability from source code.
Avasin source-koodin nano-teksti editorissa.
<img width="1828" height="1196" alt="image" src="https://github.com/user-attachments/assets/17647bc9-c5ea-4947-89a2-bd1405a73661" />
Korjasin haavoittovuuden koodista muuttamalla: sql = "SELECT password FROM pins WHERE pin= :pin;" ja res=db.session.execute(text(sql),{"pin": pin})

<img width="1052" height="802" alt="image" src="https://github.com/user-attachments/assets/5f5090a5-d1ef-44b3-8569-793b72f9549b" />

Kun testasin samaa SQL injectonia niin en saannut salasanaa näkyville.

<img width="1730" height="1198" alt="image" src="https://github.com/user-attachments/assets/bfa5e4d9-359b-4208-8aa4-686d5a101dd0" />

c)Solve dirfuzt-1.
Onnistuin tiedoston lataamisessa, mutta en saanut tiedostoa auki. Tuktin netistä ja kävin tekoälyn kanssa keskustelua ja sain tietää, että kun minulla on M sarjan macbook ja siihen tehty eri versio ei anna avata tiedostoa. Kokeilin ladata qemu emulaattorin, joka pystyisi muuttamaan tiedoston, jotta se avautuisi, mutta se ei toiminut. Jouduin turvautumaan toiseen koneeseeni, joka on vanha windows koneeseeni.

Suoritin: AMD Ryzen 7 1700 (8-Core Processor) 
RAM: 32 Gt 
Näytönohjain: 8 Gt 
Tallennustila: 1000 Gt

Guest: Oracle VM VirtualBox 7.2.4

Virtuaalikone: Debian 13.3
RAM: 4GB
Levytila: 20 GB

Onnistuin lataamaan tiedoston ja ajoin sen. 

<img width="1280" height="800" alt="VirtualBox_Debian Clone_24_01_2026_18_02_31" src="https://github.com/user-attachments/assets/4a656917-aca1-443d-b4ab-3110f6c52809" />
Tällainen nettisivu tuli esille:

<img width="1280" height="800" alt="VirtualBox_Debian Clone_24_01_2026_18_05_11" src="https://github.com/user-attachments/assets/0b800a93-a655-4bb7-a556-fb20bdd642d8" />

suoritin terminaalissa komennon `./ffuf -w common.txt -u http://127.0.0.2:8000/FUZZ.` Tuloksena tuli iso määrä pyyntöjä:

<img width="1280" height="800" alt="VirtualBox_Debian Clone_24_01_2026_18_12_45" src="https://github.com/user-attachments/assets/d1d59f6c-4e74-41e1-ba87-52eaf2044752" />

Lähestulkoon kaikissa pyynnöissä filter size oli 154. joten käytin päätin filtteröidä komennolla: `./ffuf -w common.txt -u http://127.0.0.2:8000/FUZZ -fs 154`. Näkyville tuli vähemmän rivejä:


<img width="1280" height="800" alt="VirtualBox_Debian Clone_24_01_2026_18_17_45" src="https://github.com/user-attachments/assets/20cca6fd-4e32-483e-abb4-bc220cc6b8f7" />

Kokeilin jokaista kohtaa url:lin loppuun, käyttämällä sanoja /wp-admin ja .git/config. Nämä paljastivat molemmat liput:

wp-admin:
<img width="1280" height="800" alt="VirtualBox_Debian Clone_24_01_2026_18_21_32" src="https://github.com/user-attachments/assets/b7c32d1b-2bc7-41d4-987c-7247f54bf04c" />

/.git/config:


<img width="1280" height="800" alt="VirtualBox_Debian Clone_24_01_2026_18_25_49" src="https://github.com/user-attachments/assets/a9c6b5c6-c861-4dc8-a5b0-941a024cf20f" />

d) Break into 020-your-eyes-only. Tämän pystyin tekemään taas macbookillani. Käytin käynnistysohjeissa Teron Hack'n Fix artikkelin ohjeita ja sain sen käyntiin:
<img width="1768" height="1296" alt="image" src="https://github.com/user-attachments/assets/35c2f477-3906-4ee4-829c-bd381e452bcd" />

Suoritin toisessa terminaalissa komennon `ffuf -w common.txt u- http://127.0.0.1:8000/FUZZ`. 

<img width="1336" height="1154" alt="image" src="https://github.com/user-attachments/assets/51aa8137-89ad-4519-b537-a9c9ed6dfc27" />
Sieltä tuli àdmin-console` rivi esille ja päätin kokeilla sitä url-osoitteen jatkona. Sieltä tuli tälläinen error page:
<img width="1772" height="1292" alt="image" src="https://github.com/user-attachments/assets/aef9a443-7856-4135-8a80-90e2a8afe448" />
Menin takasin etusivulle ja kokeilin muutamia keinoja ja sit löysin oikean. Sivustolle piti kirjautua ensin ja sit laittaa url-osoitteen perään /admin-console,josta tuli sivu:
<img width="1824" height="1284" alt="image" src="https://github.com/user-attachments/assets/ddd20be8-772d-41eb-825e-81707cbfd8bb" />
eli pääsin admin sivustolle vaikka olin kirjautunut normaalina käyttäjänä.

e) Fix the 020-your-eyes-only vulnerability.
Avasin tiedoston views.py nanolla:
<img width="1014" height="980" alt="image" src="https://github.com/user-attachments/assets/c7e67ce5-6b96-45c1-842b-893308d285bc" />
Lisäsin rivin `"and self.request.user.is_staff"`jo kun kokeilin uudestaan päästä samalla keinolla admin sivulle tuli forbidden error:
<img width="1784" height="1288" alt="image" src="https://github.com/user-attachments/assets/385e3164-0578-447c-8fb3-ebababb5e9b5" />

















