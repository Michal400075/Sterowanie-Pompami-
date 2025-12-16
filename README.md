# MODEL STEROWANIA POMPAMI W DOMU JEDNORODZINNYM 
System sterowania pompami C.O., C.W.U. i cyrkulacji – model AADL SCR

Dane studenta
Michał Górnik 
400075 
mgornik@student.agh.edu.pl


# Ogólny opis modelu 
Model przedstawia System Sterowania Pompami w Domu Jednorodzinnym (Home Heating Pump Control System) zbudowany w języku AADL. System obejmuje sterowanie pompą centralnego ogrzewania, pompą ciepłej wody użytkowej (C.W.U.) oraz pompą cyrkulacji. Model odwzorowuje architekturę systemu sterowania wraz z czujnikami temperatury, czujnikiem przepływu, panelem trybów pracy, jednostkami wykonawczymi, procesem sterującym, pamięcią, magistralą komunikacyjną oraz zasobami sprzętowymi.
Celem modelu jest odzwierciedlenie architektury logicznej systemu grzewczego, umożliwienie analizy przepływu danych, konfiguracji procesów, komunikacji oraz integracji urządzeń w ramach architektury AADL.

# Opis modelu dla użytkownika
System automatycznie zarządza pracą pomp ogrzewania oraz monitoruje parametry instalacji grzewczej. Wykorzystuje dane z czujników temperatury i przepływu oraz ustawienia użytkownika z panelu trybu pracy. Na tej podstawie podejmuje decyzje dotyczące:

•	pracy pompy C.O.,

•	grzania zasobnika C.W.U.,

•	działania pompy cyrkulacji,

•	sygnalizacji stanów alarmowych (np. brak przepływu, przegrzanie, tryb awaryjny).

System działa w oparciu o magistralę Modbus, gdzie wszystkie urządzenia komunikują się z centralnym sterownikiem.

# Spis komponentów AADL z komentarzem
1. Dane (data)
   
•	PumpCommand – polecenia sterujące pompami.;

•	TemperatureData – dane temperatur z czujników.;

•	FlowData – dane przepływu z czujnika przepływu.;

•	SystemStatus – tryb pracy i status alarmowy.;

•	ConfigData – konfiguracja dostępna dla wątków sterowania.;

2. Czujniki (device – wejściowe)
 
•	BoilerTempSensor – czujnik temperatury kotła.

•	DHWTankTempSensor – czujnik temperatury zasobnika C.W.U.

•	RadiatorTempSensor – czujnik temperatury grzejników.

•	RoomThermostat – czujnik temperatury zadanej z pomieszczenia.

•	FlowSensor – czujnik przepływu.

•	ModeSwitchPanel – panel trybów pracy systemu.

3. Urządzenia wykonawcze (device – wyjściowe)
   
•	CHPumpUnit – pompa obiegu C.O.

•	DHWPumpUnit – pompa obiegu C.W.U.

•	CirculationPumpUnit – pompa cyrkulacji.

•	AlarmIndicator – sygnalizator stanu alarmowego.

4. Proces (process)
   
HeatingPumpControlProcess – główny proces odpowiedzialny za całą logikę systemu.

Zawiera podprocesy:

•	CHPumpController – sterowanie pompą C.O.

•	DHWPumpController – sterowanie pompą C.W.U.

•	CirculationPumpController – sterowanie pompą cyrkulacji.

•	MonitoringLogic – monitorowanie temperatur, przepływu i wykrywanie usterek.

•	ConfigDataStore – pamięć konfiguracji dla wątków sterujących.

5. Procesor (processor)

•	HeatingControlECU – jednostka obliczeniowa systemu.

6. Pamięć (memory)
    
•	ECUMemory – pamięć sterownika przechowująca dane i konfigurację.

7. Magistrala (bus)
    
•	ModbusBus – magistrala komunikacyjna używana przez czujniki, urządzania wykonawcze i sterownik.

8. System główny (system)
    
•	HomeHeatingPumpControl – integruje całą architekturę oraz połączenia pomiędzy czujnikami, aktuatorami, procesem i zasobami sprzętowymi.

# Model – rysunek

![schemat](https://github.com/user-attachments/assets/b9953831-c533-4ef8-b910-ac387a091e91)



# Proponowane metody analizy modelu w OSATE oraz wyniki
Model zawiera pełną specyfikację przepływów umożliwiającą analizę pełnego przepływu.
Analiza przepływu pełnego (End-to-End Flows):
Poprzez implementację informacji dotyczących przepływu w urządzeniach wejściowych, wątkach oraz urządzeniach wyjściowych przeprowadzono analizę pełnego przepływu dla czterech wybranych ścieżek przepływu danych:

Ścieżka sterowania pompą CO - Termostaty -> Modbus -> ECU/CHPumpController -> Modbus -> Pompa CO - Oczekiwany czas: 50-250ms
Ścieżka sterowania pompą CWU - Czujnik bojlera -> Modbus -> ECU/DHWPumpController -> Modbus -> Pompa CWU - Oczekiwany czas: 50-250ms
Ścieżka sterowania pompą cyrkulacyjną - Panel trybu -> Modbus -> ECU/CirculationPumpController -> Modbus -> Pompa PO - Oczekiwany czas: 100-600ms
Ścieżka alarmowa - Czujniki -> Modbus -> ECU/MonitoringLogic -> Modbus -> Alarm - Oczekiwany czas: 200-1200ms

Ustawienia analizy przepływu danych:

Typ systemu: asynchroniczny
Najgorszy czas przetwarzania: maksymalny czas wykonania obliczeń
Czas kolejkowania: wyłączony

# Literatura
SAE International, "AS5506C: Architecture Analysis & Design Language (AADL)," SAE International Standard, 2017. Available: https://www.sae.org/standards/content/as5506c/

P. H. Feiler and D. P. Gluch, Model-Based Engineering with AADL: An Introduction to the SAE Architecture Analysis & Design Language, Addison-Wesley Professional, 2012. ISBN: 978-0321888945

OSATE 2 - Open Source AADL Tool Environment. Available: https://osate.org/

Modbus Organization, "Modbus Application Protocol Specification V1.1b3," 2012. Available: https://modbus.org/

ASHRAE, "HVAC Systems and Equipment Handbook," American Society of Heating, Refrigerating and Air-Conditioning Engineers, 2020.

EN 12828:2012, "Heating systems in buildings - Design for water-based heating systems," European Standard.
