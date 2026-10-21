# Jurnalul proiectului

## 2026-10-21

- Compilarea si instalarea OTA a firmware-ului Tesla integrat au reusit cu ESPHome 2026.9.1. Logul utilizatorului confirma incarcare UF2 si OTA successful.
- Integrarea ESPHome apare corect in Home Assistant; utilizatorul confirma utilizarea entitatii Tesla AC.
- Functionarea si oprirea au fost verificate de la distanta prin consumul prizei: aproximativ 705,89 W in functionare, apoi scadere dupa OFF.
- Utilizatorul confirma ca entitatea de control face ce trebuie dupa testul de incalzire solicitat.
- Au fost adaugate instructiuni pentru controlul temperaturii din HA si folosirea unui card Termostat.
- Ramin in asteptare verificarea fizica a ecranului MOES, temperatura exacta afisata de AC si testarea individuala a tuturor setpoint-urilor.
- Firmware-ul si codurile IR nu au fost modificate prin aceasta actualizare de documentatie.
- SSID-urile si parolele ramin exclusiv in fisierul local secrets.yaml.

## 2026-10-20

- Configuratie pentru MOES S16Pro, BK7231N + MCU Tuya, si TESLA TA36FFML-1232IAPC, 12000 BTU.
- Arhivarea capturilor Pronto originale pentru HEAT 22–28°C / AUTO si OFF.
- Eliminarea componentei Toshiba si folosirea comenzilor Tesla capturate.
- Testele initiale cu HEAT 24°C / AUTO si OFF au fost confirmate pe AC.
