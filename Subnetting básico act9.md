¡Hola, futuros arquitectos de redes! Bienvenidos a nuestro taller intensivo de **Diseño de Redes LAN**. Soy su docente y hoy vamos a dedicar estas **5 horas** a dominar una de las habilidades más críticas y elegantes de las redes: el **Subnetting (Subdivisión de redes)**.

Para que estas 5 horas sean productivas, las he dividido en **3 Casos o Fases de Aprendizaje**. Pasaremos de la teoría pura a la práctica de diseño. Saquen sus calculadoras (o usen la del celular, ¡no los juzgaré!) y sus libretas. ¡Comenzamos!

---

### 🕒 FASE 1 (Horas 1 y 2): Fundamentos Binarios y Máscaras
**Objetivo:** Perder el miedo al sistema binario y entender cómo la máscara de subred "divide" la red.
*Tip del Profe: El binario no es más que una serie de interruptores de luz (encendido=1, apagado=0). La máscara le dice al router qué parte de la IP es la "Calle" (Red) y qué parte es el "Número de casa" (Host).*

#### Ejemplo 1.1: Traductor Binario-Decimal (La base de todo)
**El Reto:** Convertir la IP `192.168.10.150` y la Máscara `255.255.255.192` a binario.
**Resolución Didáctica:**
*   Recordemos los pesos: `128 | 64 | 32 | 16 | 8 | 4 | 2 | 1`
*   **IP (192):** 128 + 64 = `11000000`
*   **IP (150):** 128 + 16 + 4 + 2 = `10010110`
*   **Máscara (192):** 128 + 64 = `11000000`
*   *Conclusión visual:* Al ver la máscara en binario (`11111111.11111111.11111111.11000000`), los estudiantes ven físicamente los "1" (bits de red) y los "0" (bits de host). Aquí introducimos la notación **CIDR (/26)**.

#### Ejemplo 1.2: El "Préstamo" de Bits (Cálculo de Máscaras)
**El Reto:** Tenemos la red `10.0.0.0/8`. El jefe nos pide crear subredes. Si "prestamos" 5 bits de la parte de host, ¿cuál es la nueva máscara en decimal y en CIDR?
**Resolución Didáctica:**
*   Máscara original: `11111111.00000000.00000000.00000000` (/8)
*   Prestamos 5 bits: `11111111.11100000.00000000.00000000`
*   Nuevo CIDR: 8 + 5 = **/13**
*   Conversión a decimal del segundo octeto: 128 + 64 + 32 = **224**
*   *Nueva Máscara:* `255.224.0.0`

#### Ejemplo 1.3: Adivina la Máscara (Juego rápido de clase)
**El Reto:** Les proyecto en el pizarrón 3 máscaras en binario y ellos deben gritar el CIDR y el último octeto en decimal.
1.  `11111111.11111111.11111111.11110000` -> **Respuesta:** /28, Máscara `255.255.255.240`
2.  `11111111.11111111.11111111.10000000` -> **Respuesta:** /25, Máscara `255.255.255.128`
3.  `11111111.11111111.11111111.11111100` -> **Respuesta:** /30, Máscara `255.255.255.252`

---

### 🕒 FASE 2 (Hora 3): La Matemática de Subredes, Hosts y Rangos
**Objetivo:** Aplicar las fórmulas mágicas ($2^n$ y $2^h - 2$) y entender el concepto de "Bloque" o "Salto".
*Tip del Profe: Siempre restamos 2 a los hosts. ¿Por qué? Porque el primer IP es el "Nombre de la Red" (Network ID) y el último es el "Grito de Broadcast". ¡No se los podemos dar a las PCs!*

#### Ejemplo 2.1: Aplicando las Fórmulas (Subredes y Hosts)
**El Reto:** Nos dan la red `172.16.5.0/24`. Necesitamos 6 subredes. ¿Cuántos bits prestamos? ¿Cuántas subredes reales obtenemos? ¿Cuántos hosts usa cada una?
**Resolución Didáctica:**
*   Fórmula de subredes: $2^n \ge 6$. Si prestamos 2 bits ($2^2=4$, no alcanza). Si prestamos 3 bits ($2^3=8$, ¡sí alcanza!).
*   **Bits prestados (n):** 3
*   **Subredes totales:** 8
*   **Bits para host (h):** 8 - 3 = 5 bits.
*   **Hosts por subred:** $2^5 - 2 = 32 - 2 =$ **30 hosts útiles**.
*   **Nueva Máscara:** /27 (`255.255.255.224`).

#### Ejemplo 2.2: Calculando Rangos (El corazón del Subnetting)
**El Reto:** Para la red `192.168.1.0/26`, calcular el ID de Red, Primer Host, Último Host y Broadcast de la **segunda** subred.
**Resolución Didáctica:**
*   *Paso 1 (El Salto Mágico):* 256 - 224 (máscara) = **32**. Las subredes saltan de 32 en 32.
*   *Paso 2 (Listar subredes):* Subred 1: `.0`, Subred 2: `.32`, Subred 3: `.64`...
*   *Paso 3 (Rangos de la Subred 2 - `192.168.1.32/26`):*
    *   **ID de Red:** `192.168.1.32` (No asignable)
    *   **Primer Host:** `192.168.1.33`
    *   **Último Host:** `192.168.1.62` (porque la siguiente red es la .64)
    *   **Broadcast:** `192.168.1.63` (No asignable)

#### Ejemplo 2.3: Validación de IPs (¿Es un host válido?)
**El Reto:** Dada la subred `10.10.10.64/27`, determinar si las siguientes IPs son Hosts válidos, ID de red o Broadcast:
1.  `10.10.10.65` -> **Host válido** (Primer host).
2.  `10.10.10.94` -> **Host válido** (Último host).
3.  `10.10.10.95` -> **Broadcast** (Inválido para host).
4.  `10.10.10.96` -> **ID de la SIGUIENTE red** (Inválido para host).

---

### 🕒 FASE 3 (Horas 4 y 5): Diseño de Subredes para Escenarios LAN
**Objetivo:** Pasar de la matemática pura al diseño lógico. Resolver problemas del mundo real.
*Tip del Profe: En la vida real, no solo importan las matemáticas, importa la organización. Una buena LAN se diseña pensando en el futuro y en la seguridad por departamentos.*

#### Ejemplo 3.1: Escenario de Oficina Pequeña (Diseño FLSM)
**El Escenario:** La empresa "TechSolutions" tiene la red `192.168.50.0/24`. Tienen 3 departamentos: Ventas, RRHH y Contabilidad. Por política de la empresa, cada departamento debe tener su propia subred del **mismo tamaño**, aunque no usen todos los hosts.
**El Reto de Diseño:** Dividir la red para los 3 departamentos.
**Resolución Didáctica:**
*   Necesitamos 3 subredes. La potencia de 2 más cercana que cubra 3 es $2^2 = 4$ subredes.
*   Prestamos 2 bits. Nueva máscara: **/26** (`255.255.255.192`).
*   *Asignación lógica (El estudiante debe llenar esta tabla):*
    *   **Subred 1 (Ventas):** `192.168.50.0/26` (Rango hosts: .1 - .62)
    *   **Subred 2 (RRHH):** `192.168.50.64/26` (Rango hosts: .65 - .126)
    *   **Subred 3 (Contab.):** `192.168.50.128/26` (Rango hosts: .129 - .190)
    *   *Subred 4 (Reserva/Crecimiento):* `192.168.50.192/26`

#### Ejemplo 3.2: Escenario de Laboratorio Universitario (Optimización)
**El Escenario:** La universidad asigna a nuestra facultad la red `172.16.20.0/24`. Debemos diseñar la LAN para 4 laboratorios de cómputo. Cada laboratorio tiene exactamente **50 PCs**.
**El Reto de Diseño:** ¿Qué máscara usamos para no desperdiciar direcciones IP?
**Resolución Didáctica:**
*   Analizamos los requerimientos: 4 subredes, 50 hosts por subred.
*   Si usamos el ejemplo anterior (/26), tendríamos 62 hosts por subred. ¡Perfecto, nos alcanza y es la máscara más eficiente para este caso!*
*   *Diseño:*
    *   Lab 1: `172.16.20.0/26`
    *   Lab 2: `172.16.20.64/26`
    *   Lab 3: `172.16.20.128/26`
    *   Lab 4: `172.16.20.192/26`
*   *Reflexión con los alumnos:* ¿Qué pasaría si el laboratorio 5 tuviera 70 PCs? (Respuesta: /26 no alcanzaría, tendríamos que haber usado /25, sacrificando cantidad de subredes por cantidad de hosts).

#### Ejemplo 3.3: Escenario de Enlace Punto a Punto (El caso extremo)
**El Escenario:** Tenemos dos edificios en el campus conectados por un enlace de fibra óptica directo (solo 2 routers conectados entre sí). Usaremos la red `10.200.1.0/24` para este enlace.
**El Reto de Diseño:** Diseñar la subred más pequeña y eficiente posible para este enlace para no desperdiciar IPs.
**Resolución Didáctica:**
*   Un enlace punto a punto solo necesita **2 IPs útiles** (una para cada router).
*   Buscamos la fórmula: $2^h - 2 \ge 2 \rightarrow 2^h \ge 4 \rightarrow h = 2$ bits para hosts.
*   Si tenemos 2 bits para hosts, tenemos 6 bits para red en el último octeto.
*   Máscara: **/30** (`255.255.255.252`).
*   *Asignación:*
    *   Router A: `10.200.1.1`
    *   Router B: `10.200.1.2`
    *   (ID de red: .0, Broadcast: .3).
*   *Lección final:* En redes LAN y WAN, la eficiencia es dinero y seguridad. Usar /30 para enlaces punto a punto es un estándar de la industria.

---

### 📝 Cierre de la Clase (Minutos finales)
"Chicos, en estas 5 horas pasamos de ver unos y ceros en la pizarra a diseñar la estructura lógica de una red corporativa real. El subnetting al principio parece matemática alienígena, pero con la práctica se vuelve tan natural como sumar. 

**Tarea para la próxima clase:** Les dejo un diagrama de una empresa con 5 departamentos y diferentes requerimientos de hosts. Quiero que me traigan el diseño completo con sus tablas de rangos. ¡Nos vemos en el laboratorio de Packet Tracer la próxima semana para configurar lo que hoy diseñamos en papel!"

*¿Tienen alguna duda antes de que suene el timbre?*
