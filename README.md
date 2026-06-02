<img width="670" height="274" alt="Captura de pantalla 2026-06-02 004649" src="https://github.com/user-attachments/assets/c6e3c0e3-7e79-4ab9-8453-ef771a2d18e7" />


# Write-up-Maquina-Admin-Portal

Maquina de www.whoami-labs.com

Iniciamos la maquina vulnerable

<img width="1920" height="1140" alt="Captura de pantalla 2026-06-02 001408" src="https://github.com/user-attachments/assets/27da66cf-659e-4d7c-801d-abb654650697" />

# ENUMERACION

Una vez iniciada la maquina vulnerable realizamos un escaneo de puertos utilizando la herramienta nmap

<img width="1920" height="1140" alt="Captura de pantalla 2026-06-02 001434" src="https://github.com/user-attachments/assets/6d394950-4c41-433a-bbcb-5080113d99fb" />

Como podemos observar la maquina cuenta con los siguientes puertos abiertos

✳️Puerto #22 corriendo un servicio ssh en una version Apache httpd 2.4.67 ((Debian))

✳️Puerto #80 corriendo un servicio hhtp en una version Apache/2.4.67 (Debian)

Luego fuimos a un navegador a ver que contenia el servicio http

<img width="1920" height="1140" alt="Captura de pantalla 2026-06-02 001500" src="https://github.com/user-attachments/assets/549d3448-b802-4ea0-b00f-7cbdf6a97804" />

Tambien encontramos un login el cual vamos a tratar de vulnerar

<img width="1920" height="1140" alt="Captura de pantalla 2026-06-02 001516" src="https://github.com/user-attachments/assets/74803adb-67f5-4904-9168-01c488fa238d" />

Verificamos el codigo de la pagina y encontramos un comentario que nos decia que la validacion de frontend nos obligaria a usar burp suite

<img width="1920" height="1140" alt="Captura de pantalla 2026-06-02 001601" src="https://github.com/user-attachments/assets/e5d34f62-aed7-4174-87f8-967b99b4538b" />

# EXPLOTACION

No esperamos mas asi que fuimos a burp suite a ver que encontrabamos, interceptamos cuando tratamos de hacer el ingreso en el login

<img width="1920" height="1140" alt="Captura de pantalla 2026-06-02 001822" src="https://github.com/user-attachments/assets/c07692ce-13f4-4eb1-b363-9901d612d718" />

Hicimos el envio a repeater

<img width="1920" height="1140" alt="Captura de pantalla 2026-06-02 001834" src="https://github.com/user-attachments/assets/e18fa8be-e457-4782-8b41-cdd5e538e9e3" />

Hicimos el cambio del usuario porun codigo sqli para tratar de bypasear el login, obteniendo exito y nos daba como resultado el directorio donde teniamos que ir que era admin.php

<img width="1920" height="1140" alt="Captura de pantalla 2026-06-02 001925" src="https://github.com/user-attachments/assets/45b9c155-a115-423e-b9aa-53c783d6a257" />

Ingresamos y encontramos una herramienta de mapeo de red 

<img width="1920" height="1140" alt="Captura de pantalla 2026-06-02 002203" src="https://github.com/user-attachments/assets/3f17c923-de51-429c-bfd6-f782dd012aa7" />

Intentamos ingresar codigo para ver si podiamos obtener información pero fue en vano ya que tenia una restriccion por un firewall que no permitia la inyeccion de comandos

<img width="1920" height="1140" alt="Captura de pantalla 2026-06-02 002356" src="https://github.com/user-attachments/assets/d8a71626-2014-425f-9b47-1438cf52b077" />

Luego lo que hicimos fue interceptarla con el burp suite a ver si lograbamos la respuesta

<img width="1920" height="1140" alt="Captura de pantalla 2026-06-02 003004" src="https://github.com/user-attachments/assets/b8ae1ae2-d7e0-421d-add7-664c73f57bfe" />

Hicimos el envio a repeater

<img width="1920" height="1140" alt="Captura de pantalla 2026-06-02 003015" src="https://github.com/user-attachments/assets/cb1c871f-0d26-4d2d-8e25-943851fb193e" />

Cambiamos y pusimos el comando whoami a ver si lograbamos una respuesta exitosa y si

<img width="1920" height="1140" alt="Captura de pantalla 2026-06-02 003217" src="https://github.com/user-attachments/assets/6707e851-ced3-4155-b71b-9940a4ed71a8" />

Pusimos nuestro kali en escucha para tratar de obtener una reverse shell

<img width="1920" height="1140" alt="Captura de pantalla 2026-06-02 003356" src="https://github.com/user-attachments/assets/21571a93-e4b3-494f-853e-5a126fe6a8d4" />

Ingresamos el comando con el payload

<img width="1920" height="1140" alt="Captura de pantalla 2026-06-02 003437" src="https://github.com/user-attachments/assets/27d2147b-179b-445a-b1fa-0caa2809a170" />

Obtuvimos acceso a la maquina

<img width="1920" height="1140" alt="Captura de pantalla 2026-06-02 003443" src="https://github.com/user-attachments/assets/bca6399c-4722-4c1b-8716-6565e2095803" />

Ya dentro de ella procedimos a buscar la flag de usuario

<img width="1920" height="1140" alt="Captura de pantalla 2026-06-02 003548" src="https://github.com/user-attachments/assets/ded88ccc-d8ba-47fa-817b-82e4f1675e72" />

<img width="1920" height="1140" alt="Captura de pantalla 2026-06-02 003600" src="https://github.com/user-attachments/assets/6d126e08-bcc9-4d04-b799-6a99d4d071fb" />

# ESCALADA DE PRIVILEGIOS
Primero buscamos los binarios a ver si encontrabamos alguno con el suid activo pero no fue asi, luego buscamos si tenian cronjobs pero tampoco, hasta que procedimos a buscar las capabilities y ahi si encontramos una que nos ayudaria a obtener acceso como root

<img width="1920" height="1140" alt="Captura de pantalla 2026-06-02 004101" src="https://github.com/user-attachments/assets/587ecc53-f487-40ec-8929-2508f3fab19b" />

Acceso a root

<img width="1920" height="1140" alt="Captura de pantalla 2026-06-02 004232" src="https://github.com/user-attachments/assets/49808f07-9808-4daa-824f-bb8afe859104" />

Buscamos la root

<img width="1920" height="1140" alt="Captura de pantalla 2026-06-02 004253" src="https://github.com/user-attachments/assets/1a7f9d49-3b72-4254-98f6-2063236aacb2" />

# RESULTADOS


<img width="1920" height="1140" alt="Captura de pantalla 2026-06-02 004443" src="https://github.com/user-attachments/assets/415b911c-ec4c-482a-a3f6-dfc01975175b" />


<img width="2688" height="1596" alt="Gemini_Generated_Image_6m953u6m953u6m95" src="https://github.com/user-attachments/assets/5840690a-cab2-41e7-83af-9ac012a7f787" />
