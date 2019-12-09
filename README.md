## Alegerile Prezidențiale din 2019 în România - Date geografice la nivel de UAT

### Surse de date
- Geometriile au fost create de către ANCPI și publicate pe [Geoportalul INSPIRE al României](http://geoportal.gov.ro)
 și pe [Portalul de date deschise al Guvernului României](http://geoportal.gov.ro).
- Tabelul de atribute a fost completat cu date publicate liber de Biroul Electoral Central pe [Prezenta BEC](http://prezenta.bec.ro).
- Datele se pot reutiliza sub termenii [Licenței pentru o Guvernare Deschisă](http://data.gov.ro/base/images/logoinst/OGL-ROU-1.0.pdf).

Autor: Cristina Alexa  
Data: 2019-12-07  

### Fișiere
- Unitățile administrative tip poligon, din layer-ul Unitate_administrativa_UAT de pe http://geoportal.ancpi.ro/portal/apps/webappviewer/index.html?id=faeba2d173374445b1f13512bd477bb2
- Rezultate finale alegeri din turul 1, 10 noiembrie 2019, din fișierul pv_RO_PRSD_FINAL.csv, de pe https://prezenta.bec.ro/prezidentiale10112019/romania-pv-final
- Rezultate finale alegeri din turul 2, 24 noiembrie 2019, din fișierul pv_RO_PRSD_FINAL.csv, de pe https://prezenta.bec.ro/prezidentiale24112019/romania-pv-final

### Sistem de Coordonate
- EPSG:3844 - Pulkovo 1942(58) / Stereo70 - Projected
- EPSG:4326 - WGS 84 - Geographic

### Metode
  Procesarea datelor a fost făcută în QGIS 3.8.3-Zanzibar și în Spatialite (tot prin QGIS).  
  Au fost descărcate datele geografice și cele electorale, apoi s-a trecut la agregarea rezultatelor electorale la nivel de UAT (deoarece erau la nivel de secție de votare), pentru a putea face legătura între cele două seturi de date.  
  Gruparea datelor din tabelul pv_RO_PRSD_FINAL s-a făcut pe câmpul Siruta, în Spatialite, și a fost creat un view temporar pv_RO_UAT_VIEW.  
  A urmat Join-ul între fișierul Unitate_administrativa_UAT și pv_RO_UAT_VIEW, pe Siruta = natCode.  
  Au rezultat înregistrări fără date electorale în urma Join-ului, a urmat inspectarea cauzelor.  
  S-a constatat că:  
    În pv_RO_PRSD_FINAL, sectoarele Bucureștiului au Siruta 179132, orașul Băneasa din Constanța are Siruta 63171  
    În Unitate_administrativa_UAT:  
        - BUCUREŞTI SECTORUL 1: 179141  
        - BUCUREŞTI SECTORUL 2: 179150  
        - BUCUREŞTI SECTORUL 3: 179169  
        - BUCUREŞTI SECTORUL 4: 179178  
        - BUCUREŞTI SECTORUL 5: 179187  
        - BUCUREŞTI SECTORUL 6: 179196  
        - BĂNEASA: 61069  
  Problema a fost remediată prin folosirea unui câmp intermediar, Siruta_Mod. A fost creat un tabel intermediar din pv_RO_UAT_VIEW, pv_RO_UAT_MOD, a fost adăugat un câmp Siruta_Mod și updatat din Siruta, dar pentru București și Băneasa au fost inserate codurile de mai sus.  
  S-a refacut Join-ul cu Unitate_administrativa_UAT, de data asta cu pv_RO_UAT_MOD, și au rezultat 3186 rânduri populate complet.  

### Rezultate
  Fișierul rezultat în urma Join-ului a fost exportat ca ESRI shapefile, cu numele UAT_elections_round_1(respectiv UAT_elections_round_2), EPSG 3844, și encoding UTF8.  
  În final, a fost adăugat un nou câmp pv_siruta în care sunt trecute pentru București și Băneasa codurile Siruta originale din pv_RO_PRSD_FINAL.  
  Rezultatele au fost verificate prin query-uri în Spatialite și compararea cu rezultatele finale și cele la nivel de UAT de pe https://prezenta.bec.ro.  

### Alte considerente
  S-a ales agregarea datelor electorale la nivel de Siruta (natCode) deoarece s-a urmat structura din fișierul geografic de pe ANCPI, și s-a dorit folosirea unui nivel de detalii mai mare, de unde se pot generaliza datele, dacă este nevoie.  
  Se putea folosi și agregarea la nivel de 'Siruta' ca în fișierul cu date electorale, folosind sectoarele Bucureștiului nediferențiate, prin dizolvarea geometriilor corespunzătoare din fișierul geografic.  
  De asemenea, se putea folosi și agregarea la nivel de 'Cod birou electoral', iar atunci am fi dizolvat geometriile la nivel de județ, păstrând sectoarele Bucureștiului nedizolvate.  
  Putem ajunge la cele două nivele de generalizare de mai sus și pornind de la fișierul UAT_elections_round_1(respectiv UAT_elections_round_2).  

### Detalii despre proces 
https://blog.maptheclouds.com/ro/tutoriale/perspectiva-spatiala-alegeri  

&nbsp;
&nbsp;

## Romania 2019 Presidential Elections - Geographic data at UAT level

### Data sources
- The geometries were created by ANCPI and published on [Geoportalul INSPIRE al României](http://geoportal.gov.ro) and on [Portalul de date deschise al Guvernului României](http://geoportal.gov.ro). 
- The attribute table was complemented with open data from Biroul Electoral Central on [Prezenta BEC](http://prezenta.bec.ro).
- The data can be reused under the [Open Government License](http://data.gov.ro/base/images/logoinst/OGL-ROU-1.0.pdf).

Author: Cristina Alexa  
Date: 2019-12-07  

### Files
- Administrative units of polygon type, from the Unitate_administrativa_UAT layer, from http://geoportal.ancpi.ro/portal/apps/webappviewer/index.html?id=faeba2d173374445b1f13512bd477bb2  
- Final elections results from the second round, 24th of November 2019, from the file pv_RO_PRSD_FINAL.csv, from https://prezenta.bec.ro/prezidentiale24112019/romania-pv-final  

### Coordinate Systems
- EPSG:3844 - Pulkovo 1942(58) / Stereo70 - Projected  
- EPSG:4326 - WGS 84 - Geographic  

### Methods
  Data processing was made in QGIS 3.8.3-Zanzibar and Spatialite (in QGIS).  
  Geographic data and elections data were downloaded, then we proceeded to the aggregation of the elections results at UAT level (because they were at precinct level), in order to join the two data sets.  
  The data grouping from pv_RO_PRSD_FINAL was made on the Siruta field, in Spatialite, and a temporary view was created, pv_RO_UAT_VIEW.  
  Next, it was made the Join between Unitate_administrativa_UAT and pv_RO_UAT_VIEW, on Siruta = natCode.  
  There have resulted records without electoral data after the Join, the inspecion of the causes followed up.  
  It was found that:  
    In pv_RO_PRSD_FINAL, Bucharest sectors have Siruta 179132, Băneasa city from Constanța has Siruta 63171  
    In Unitate_administrativa_UAT:  
      - BUCUREŞTI SECTORUL 1: 179141  
      - BUCUREŞTI SECTORUL 2: 179150  
      - BUCUREŞTI SECTORUL 3: 179169  
      - BUCUREŞTI SECTORUL 4: 179178  
      - BUCUREŞTI SECTORUL 5: 179187  
      - BUCUREŞTI SECTORUL 6: 179196  
      - BĂNEASA: 61069  
  The issue was solved by using an intermediate field, Siruta_Mod. An intermediary table was created from pv_RO_UAT_VIEW, pv_RO_UAT_MOD, a field Siruta_Mod was added and updated from Siruta, but for Bucharest and Băneasa the codes above were inserted.  
  The Join with Unitate_administrativa_UAT was made again, this time with pv_RO_UAT_MOD, and there have resulted 3186 completely populated rows.  

### Results
  The resulting file from the Join was exported as ESRI shapefile, named UAT_elections_round_1 (respectively UAT_elections_round_2), with EPSG 3844 and UTF8 encoding.  
  Finally, a new field, pv_siruta was added, in which there are recorded the original Siruta codes for București and Băneasa from pv_RO_PRSD_FINAL.  
  The results were checked by queries in Spatialite and cross-checking with the final results and the UAT level results from [Prezenta BEC](http://prezenta.bec.ro).  

### Other considerations
  The aggregation of the electoral data at Siruta (natCode) level was chosen because it followed the structure from the geographic file from ANCPI, and the use of a greater detail level was intended, from where the data could be generalized, if needed.  
  The aggregation at 'Siruta' level could be used in a similar way like in the electoral data file, using the Bucharest sectors together, dissolving the corresponding geometries from the geographic file.  
  The aggregation at 'Cod birou electoral' level could also be used, and them we would have dissolved the geometries at county level, and keep the Bucharest sectors undissolved.  
  We could get to the two generalization levels from above starting from the UAT_elections_round_1 (respectively UAT_elections_round_2) file, too.  

### Details about the process
https://blog.maptheclouds.com/tutorials/spatial-perspective-elections  
