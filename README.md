# MOES S16Pro — TESLA TA36FFML-1232IAPC

Configuratie ESPHome pentru MOES S16Pro (BK7231N + MCU Tuya), folosit ca telecomanda IR pentru aparatul **TESLA TA36FFML-1232IAPC, 12000 BTU**.

## Descarcare si instalare

1. Deschide [moes-s16pro.yaml](moes-s16pro.yaml), apoi butonul **Raw** pentru copiere/descarcare.
2. In ESPHome Device Builder, salveaza o copie a configuratiei care functioneaza, apoi inlocuieste continutul YAML-ului MOES cu acest fisier.
3. In **Secrets** de pe Home Assistant pastreaza valorile locale pentru `wifi_ssid` si `wifi_password`; adauga `fallback_password` (minimum 8 caractere). Aceste valori NU se incarca in GitHub.
4. Apasa **Validate**, apoi **Install → Wirelessly**. Builder-ul trebuie sa poata ajunge la IP-ul actual al MOES.
5. Dupa repornire, testeaza mai intai butoanele **Tesla Incalzire 24 Auto** si **Tesla Oprire**.
6. Din entitatea **Tesla AC**, selecteaza HEAT, apoi 25°C, 22°C si 28°C; verifica reactia AC-ului. Testeaza si oprirea.
7. Verifica separat modificarile de temperatura si HEAT/OFF de pe touchscreen.

Hostname-ul ramane `moes-s16pro`; numele afisat devine **MOES Tesla**, iar climatizarea **Tesla AC**. Componenta si automatizarile Toshiba au fost eliminate.

## Starea verificarilor — 21 octombrie 2026

| Element | Stare |
|---|---|
| MOES transmite IR catre Tesla | Confirmat de utilizator |
| HEAT 24°C / AUTO si OFF prin butoanele Pronto | Testate anterior cu succes pe AC |
| Compilare si instalare firmware integrat | Reusite cu ESPHome 2026.9.1; firmware.uf2 incarcat, OTA successful |
| Integrare ESPHome si entitatea Tesla AC in HA | Apar corect; utilizatorul confirma ca le foloseste |
| Pornire / oprire de la distanta | Confirmate indirect prin consumul prizei; aproximativ 705,89 W in functionare, apoi scadere dupa OFF |
| Comanda din Tesla AC in HA | Utilizatorul confirma functionarea dupa testul de incalzire solicitat |
| Setpoint-urile 22, 23, 25, 26, 27, 28°C, fiecare separat | Capturate si verificate structural; nu exista confirmare individuala pentru fiecare temperatura |
| Temperatura exacta afisata pe AC si viteza ventilatorului | Neverificate fizic dupa instalarea integrata |
| Controlul de pe touchscreen | In asteptarea testelor langa dispozitiv |

Testele de la distanta confirma indirect functionarea prin consumul prizei. Ele nu demonstreaza temperatura exacta acceptata de AC, viteza ventilatorului sau corectitudinea tuturor setpoint-urilor. Nu a fost demonstrat pe ce masina s-a executat compilarea.

Fiecare captura contine 106 perechi de impulsuri Pronto si un payload de 104 biti. Fisierul pastreaza timpii originali capturati, fara normalizare sau modificarea checksum-ului.

## Control din Home Assistant

Dupa adaugarea MOES prin integrarea **ESPHome**, deschide dispozitivul **MOES Tesla** si entitatea **Tesla AC**. Selecteaza **HEAT / Incalzire**, apoi temperatura intre **22 si 28°C**, in pasi de **1°C**. Pentru acces rapid, adauga un card **Termostat** pe dashboard.

Entitatea Tesla AC este calea pentru controlul temperaturii si pentru automatizari. Butoanele **Tesla Incalzire 24 Auto** si **Tesla Oprire** trimit comenzile IR de test direct si nu actualizeaza starea display-ului.

Prezenta dispozitivului in **ESPHome Device Builder** nu inlocuieste adaugarea lui prin integrarea ESPHome din Home Assistant. In instalarea utilizatorului, aceasta integrare este deja adaugata si folosita.

Pentru automatizari, foloseste identificatorul real al entitatii din instalatia ta; numele de afisare nu garanteaza un anumit entity_id. Logica solara si migrarea comenzilor din Broadlink sunt configuratii separate de acest firmware; starea lor nu este validata de testele descrise aici.

## Comportament disponibil

- Incalzire la **22–28°C**, in pasi de **1°C**, si oprire.
- Ventilator **AUTO** in toate comenzile inregistrate.
- Citirea temperaturii, umiditatii si starii Wi-Fi.
- Schimbarile MCU de mod/temperatura sunt reunite cu o pauza de 300 ms.
- Actualizarile senzorilor fara schimbarea comenzii nu retransmit IR.
- La pornire, dupa 5 secunde, se memoreaza starea MCU fara a trimite intentionat o comanda IR.
- Butoanele de test trimit IR direct, fara sa modifice starea ecranului. Pentru control sincronizat se foloseste entitatea Tesla AC.
- Pentru repetarea unei comenzi neschimbate, modifica temperatura si revino sau foloseste butonul de test.

## Limite cunoscute

- Racire, DRY, FAN, alte viteze de ventilator, swing si pasi de 0,5°C nu sunt implementate.
- Limitele vizuale 22–28°C se aplica interfetei HA. MCU-ul poate permite alte temperaturi pe ecran; scriptul respinge valori neintregi sau in afara intervalului.
- Butoanele de viteza ale ecranului nu sunt integrate; toate comenzile trimise raman AUTO.
- Starea AC-ului este presupusa pe baza comenzii transmise. Nu avem confirmare de la AC si nici sincronizare de la telecomanda originala catre display.
- Senzorul MOES raporteaza temperatura camerei; nu este implementata o bucla de termostat care sa comande automat pornire/oprire dupa aceasta temperatura.
- Firmware-ul existent a raportat anterior `Invalid command (0x38) received`. Tratarea comenzii specifice MCU nu este rezolvata aici. Nu sunt trimise ACK-uri presupuse si nu sunt ascunse erorile; pentru reparare trebuie capturat continutul UART.
- Captura originala de 24°C contine si biti diferiti in afara campului temperaturii fata de noile capturi. Au fost pastrati exact, deoarece acea comanda a fost testata pe AC; semnificatia lor nu a fost stabilita.

## Hardware si datapoints

| Functie | Configuratie |
|---|---|
| Placa | generic-bk7231n-qfn32-tuya |
| Emitator IR | P7, carrier duty 50% |
| Receptor IR | P8, inverted |
| UART MCU | RX1 / TX1, 9600 baud |
| DP1 | Pornire/oprire |
| DP2 | Temperatura camerei, factor 0,1 |
| DP3 | Temperatura tinta, factor 1 |
| DP4 | Mod; incalzire = 1 |
| DP5 | Viteza ventilatorului in configuratia anterioara; neexpusa in noul YAML |
| DP12 | Umiditate |

Mapping-ul provine din configuratia utilizatorului si trebuie verificat pe alte revizii hardware.

## Capturi IR

Vezi [captures/tesla-ta36ffml-1232iapc.json](captures/tesla-ta36ffml-1232iapc.json). Fisierul include codurile Pronto complete, orele din loguri, temperaturile confirmate de utilizator si starea testelor.

Identificatorii JVC/LG din logurile `dump: all` sunt potriviri partiale ale decodoarelor. Nu reprezinta o confirmare ca AC-ul foloseste protocol JVC sau LG. Nu este presupus un protocol Toshiba, Gree sau AUX.

Pentru capturi suplimentare, modifica temporar `logger.level` la `DEBUG` si adauga `dump: all` in `remote_receiver`. Pastreaza `logger.baud_rate: 0` pentru a evita folosirea UART-ului MCU de catre logger.

## Istoricul instalarii

1. Firmware original: Main Module 1.3.1, MCU 1.0.0.
2. Initializare Smart Life si profil AC, apoi Tuya CloudCutter in Ubuntu VM pe VMware, cu adaptor USB Wi-Fi cu suport AP.
3. Profil CloudCutter: `1.3.1 - BK7231N / bk7231n_common_ir_ctr_ty`.
4. Flash ESPHome Kickstart BK7231N reusit.
5. Web OTA Kickstart a respins fisiere CloudCutter .ug.bin si .rbl cu **Bad Magic Number**; UF2 este formatul pentru acea etapa Kickstart. Nu redenumi extensiile binare.
6. Configuratia veche folosita ca baza continea Toshiba; aceasta versiune foloseste exclusiv comenzile Tesla capturate.

Acum dispozitivul are deja ESPHome; nu este necesara reluarea CloudCutter. Pentru actualizarile curente foloseste compilarea si OTA din ESPHome Device Builder.

## Referinte

- [MOES S16Pro — ESPHome Devices](https://devices.esphome.io/devices/moes-ir-thermostat-s16pro/)
- [ESPHome Remote Transmitter](https://esphome.io/components/remote_transmitter/)
- [ESPHome Tuya Climate](https://esphome.io/components/climate/tuya/)
- [Tuya CloudCutter](https://github.com/tuya-cloudcutter/tuya-cloudcutter)
- [ESPHome Kickstart](https://github.com/libretiny-eu/esphome-kickstart)

Repository-ul contine configuratie si date IR. Nu contine SSID-uri personale, parole, chei API, adrese IP private, numere de serie sau fotografiile utilizatorului. Fisierul local `secrets.yaml` este exclus prin .gitignore.
