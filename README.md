## Alegerile Prezidențiale din 2019 în România - Date geografice la nivel de UAT

- Geometriile au fost create de către ANCPI și publicate pe Geoportalul INSPIRE al României
 și pe Portalul de date deschise al Guvernului României. 
- Tabelul de atribute a fost completat cu date publicate liber de Biroul Electoral Central pe Prezenta BEC
- Datele se pot reutiliza sub termenii licenței pentru o Guvernare Deschisă. 

Fișiere utilizate:
- Unitățile administrative tip poligon, din layer-ul Unitate_administrativa_UAT
 de pe http://geoportal.ancpi.ro/geoportal/catalog/main/home.page 
- Rezultate finale alegeri din turul 2, 24 noiembrie 2019, din fișierul pv_RO_PRSD_FINAL.csv,
 de pe https://prezenta.bec.ro/

Autor:
Cristina Alexa

Sistem de Coordonate:
EPSG:3844 - Pulkovo 1942(58) / Stereo70 - Projected
(https://europe.foss4g.org/2014/slides/Daniel%20Urda%20-%20PROJ4%20Issues.The%20case%20of%20Romania2.pdf)

Procesare in QGIS 3.8.3-Zanzibar:
- Agregarea datelor din pv_RO_PRSD_FINAL pe Siruta, în Spatialite
- Join între Unitate_administrativa_UAT și pv_RO_PRSD_FINAL pe Siruta = natCode
- Constatare elemente fără date electorale în urma join-ului
    În pv_RO_PRSD_FINAL, sectoarele Bucureștiului au Siruta 179132, orașul Băneasa din Constanța are Siruta 63171
    În Unitate_administrativa_UAT:
        BUCUREŞTI SECTORUL 1: 179141
        BUCUREŞTI SECTORUL 2: 179150
        BUCUREŞTI SECTORUL 3: 179169
        BUCUREŞTI SECTORUL 4: 179178
        BUCUREŞTI SECTORUL 5: 179187
        BUCUREŞTI SECTORUL 6: 179196
        BĂNEASA: 61069
- Reparare lipsă date prin tabel intermediar pv_RO_UAT_MOD, cu câmp Siruta_Mod în care updatez Siruta 
pentru elementele cu probleme, apoi din nou agregare și join cu Unitate_administrativa_UAT
- Adăugare camp pv_siruta in care sunt trecute codurile pe care le au poligoanele cu probleme în pv_RO_PRSD_FINAL
- Rezultă 3186 rânduri populate complet
- QA pe https://prezenta.bec.ro/prezidentiale24112019/romania-precincts
