# Fallo de revocación de accesos en cerraduras inteligentes Nuki

## Análisis técnico de un incidente real en sistemas de control de accesos IoT

---

## Resumen ejecutivo

Este artículo documenta y analiza un incidente real ocurrido en el ecosistema **Nuki Smart Lock**, en el que no fue posible revocar una autorización de acceso a una cerradura inteligente de forma fiable y en un plazo razonable, pese a que el sistema se encontraba correctamente conectado y operativo.

El caso pone de manifiesto un **problema estructural de diseño** en arquitecturas IoT *cloud‑first* aplicadas a sistemas de **seguridad física**, donde la aceptación de una orden no garantiza su ejecución efectiva en el dispositivo final.

El objetivo de este artículo no es desacreditar a un fabricante concreto, sino **analizar técnicamente** por qué este tipo de diseño supone un riesgo real y por qué resulta inadecuado para sistemas de control de accesos.

---

## Contexto del sistema

El sistema afectado estaba compuesto por:

* Cerradura Nuki Smart Lock
* Nuki Bridge operativo y conectado
* Gestión mediante:

  * Nuki Web
  * Aplicación Android
  * API REST oficial de Nuki

En el momento del incidente:

* La cerradura era visible y accesible desde Nuki Web
* El Bridge funcionaba correctamente
* Otras acciones (estado, apertura, etc.) se ejecutaban sin incidencias

---

## Descripción del incidente

El problema detectado fue la imposibilidad de eliminar una autorización concreta asociada a la cerradura.

Características del fallo:

* Reproducible de forma consistente
* Afectaba a todas las interfaces disponibles:

  * Web
  * App Android
  * API REST
* La autorización:

  * desaparecía ocasionalmente del backend
  * pero seguía existiendo y siendo válida en la cerradura física

Esto implica que un usuario al que se pretendía revocar el acceso podía seguir accediendo físicamente.

---

## Línea temporal del incidente

* **Anterior al Día 24: Se notifica el incidente al soporte de Nuki y se recibe respuesta del Servicio de Atención al Cliente, refiriendo que buscara soluciones en foros técnicos.
* **Día 24: Se notifica el incidente al soporte de Nuki y se concede un plazo de 24 horas para la revocación efectiva.
* **Días 24–28: No se recibe respuesta ni se ejecuta la revocación.
* **Día 28: El soporte responde indicando que la API funciona de forma asíncrona y que la aceptación de la orden no garantiza su ejecución.

No se produjo la eliminación efectiva de la autorización.

---

## Respuesta oficial del fabricante

La respuesta recibida por parte del soporte indicaba que:

* La API de Nuki es asíncrona
* Un código de estado `204` solo indica que la solicitud fue aceptada
* No garantiza que la acción se haya ejecutado en la cerradura
* Se recomienda el uso de webhooks para confirmar la ejecución

Además, se indica que si la cerradura no es accesible en ese momento, ciertas acciones pueden fallar.

---

## Por qué los webhooks no solucionan el problema

Desde un punto de vista técnico, esta recomendación es insuficiente:

* Un webhook solo notifica el resultado de una operación
* No garantiza su ejecución
* No proporciona un mecanismo alternativo de revocación

En este caso concreto:

* El problema no era la falta de confirmación
* El problema era que la revocación no se ejecutaba

Usar webhooks únicamente serviría para confirmar que el sistema no había eliminado el acceso, algo que ya era evidente.

---

## Análisis técnico del fallo

El incidente no puede atribuirse a:

* Problemas de conectividad
* Bluetooth
* WiFi
* Uso incorrecto por parte del usuario

El origen del problema es arquitectural:

* Dependencia de colas asíncronas en backend
* Falta de garantías de consistencia entre backend y dispositivo
* Ausencia de mecanismos de revocación forzada o de emergencia

En sistemas de control de accesos físicos, la revocación debe ser determinista, no eventual.

---

## Impacto en seguridad

Un sistema de control de accesos debe cumplir, como mínimo:

* Revocación inmediata o casi inmediata
* Garantía de ejecución
* Capacidad de intervención ante incidentes

Cuando un sistema permite que:

* Una autorización persista indefinidamente
* Sin posibilidad de eliminación fiable

Se genera un riesgo de seguridad física inaceptable.

---

## Cloud‑first vs Local‑first en seguridad física

Este incidente ilustra un problema común en IoT:

* Arquitecturas *cloud‑first* priorizan escalabilidad y comodidad
* Pero sacrifican control y determinismo

En control de accesos:

* La lógica crítica no debería depender de la nube
* Las decisiones de seguridad deben resolverse localmente

La nube puede ser un complemento, pero no el árbitro final.

---

## Qué debería haber ocurrido

Desde un punto de vista técnico y operativo:

* El incidente debería haberse escalado a un equipo de seguridad
* Debería haberse forzado la invalidación de la autorización en backend
* Y verificado posteriormente el estado real de la cerradura

La ausencia de este procedimiento evidencia la falta de un plan de respuesta ante incidentes de seguridad.

---

## Conclusión

Este caso no demuestra un bug aislado, sino una deficiencia de diseño.

Un sistema donde:

* La revocación no está garantizada
* La seguridad es asíncrona
* Y la responsabilidad se desplaza al usuario

No es adecuado para entornos donde el acceso físico debe estar bajo control estricto.

La experiencia documentada aquí justifica la búsqueda de soluciones local‑first, el uso de estándares como Matter y una reevaluación profunda de cómo se diseñan los sistemas IoT aplicados a seguridad física.

---

## Nota final

Este artículo se publica con fines técnicos y divulgativos. Todos los hechos descritos se basan en un incidente real, documentado y comunicados oficiales del fabricante.

El objetivo es contribuir a un debate necesario sobre seguridad, arquitectura y responsabilidad en IoT, no señalar culpables individuales.

---

## Apéndice A – Publicación en GitHub

Este artículo ha sido preparado para su publicación como repositorio técnico en GitHub, utilizando el formato estándar de *security write‑up* y *post‑mortem*.

Este enfoque facilita:

* Indexación por buscadores técnicos
* Revisión por ingenieros y equipos de seguridad
* Referenciabilidad desde informes institucionales (INCIBE, AEPD)
* Transparencia y reproducibilidad

---
