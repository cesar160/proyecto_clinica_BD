# Especificación del proyecto para agente de IA
## Sistema de Gestión de un Laboratorio de Análisis Clínicos — Base de Datos

> **Objetivo de este archivo:** servir como contexto e instrucciones de trabajo para que un agente de IA diseñe, implemente, pruebe, documente y prepare para despliegue una base de datos PostgreSQL correspondiente a un laboratorio de análisis clínicos.
>
> El proyecto es **exclusivamente de base de datos**. **No se debe desarrollar frontend ni backend.**

---

# 1. Contexto académico

El proyecto corresponde a una actividad de la materia de bases de datos enfocada en:

- diseño de una base de datos relacional;
- dimensionamiento;
- integridad de datos;
- índices;
- operaciones SQL;
- pruebas funcionales;
- pruebas de restricciones;
- pruebas de volumen;
- pruebas de concurrencia;
- análisis de rendimiento;
- identificación de limitaciones;
- propuestas de mejora;
- despliegue en un entorno remoto;
- documentación técnica;
- recopilación de evidencias.

La evaluación **no debe limitarse a demostrar que las consultas SQL funcionan**. Se debe demostrar que el diseño es correcto, que mantiene la integridad de los datos, que puede soportar los flujos principales del laboratorio y que se conocen sus limitaciones bajo escenarios de crecimiento o uso simultáneo.

También debe conservarse evidencia de interacción con IA generativa en los puntos donde se utilice, acompañada de un comentario reflexivo del estudiante.

---

# 2. Caso de uso

Se requiere diseñar una base de datos para un **laboratorio de análisis clínicos**.

El laboratorio recibe diariamente pacientes que pueden solicitar uno o varios estudios.

El sistema debe administrar información relacionada con:

- pacientes;
- médicos;
- estudios de laboratorio;
- solicitudes;
- muestras;
- resultados;
- personal;
- equipos;
- procesamiento de muestras y estudios.

Debe ser posible relacionar toda esta información y conservar el historial.

---

# 3. Alcance

## 3.1 Incluido

El agente deberá trabajar en:

1. Análisis de requisitos.
2. Modelo entidad-relación.
3. Modelo relacional.
4. Normalización.
5. Creación del esquema PostgreSQL.
6. Claves primarias y foráneas.
7. Restricciones.
8. Índices.
9. Vistas, funciones o triggers cuando aporten valor real.
10. Datos sintéticos de prueba.
11. Operaciones CRUD mediante SQL.
12. Consultas de múltiples tablas.
13. Pruebas de integridad.
14. Pruebas de casos límite.
15. Pruebas de rendimiento.
16. Pruebas de crecimiento de volumen.
17. Pruebas de concurrencia.
18. Análisis de planes de ejecución.
19. Despliegue de PostgreSQL en Amazon RDS.
20. Validación del esquema remoto.
21. Documentación técnica.
22. Evidencias.
23. Análisis de limitaciones.
24. Propuestas de mejora.
25. Preparación del material necesario para el reporte final.

## 3.2 Fuera de alcance

No desarrollar:

- frontend;
- aplicación web;
- aplicación móvil;
- API REST;
- backend;
- sistema de autenticación de usuarios;
- interfaces gráficas propias;
- microservicios.

La interacción con la base se realizará mediante:

- `psql`;
- pgAdmin;
- DBeaver, si se desea;
- scripts SQL;
- scripts auxiliares de generación de datos;
- herramientas de AWS.

---

# 4. Motor de base de datos

Usar:

**PostgreSQL**

Preferentemente una versión actualmente soportada por Amazon RDS.

La misma versión mayor debe utilizarse, en lo posible, tanto en desarrollo local como en RDS para reducir diferencias de comportamiento.

---

# 5. Herramientas recomendadas

## Obligatorias o principales

- PostgreSQL.
- pgAdmin 4 o DBeaver.
- `psql`.
- Visual Studio Code.
- Git.
- GitHub.
- Amazon RDS for PostgreSQL.

## Auxiliares

- Python 3, exclusivamente para:
  - generación de datos sintéticos;
  - pruebas automatizadas;
  - pruebas de carga o concurrencia;
  - medición de tiempos.

Python **no debe convertirse en un backend**.

## No necesario

Docker no es obligatorio para este proyecto.

Puede omitirse para reducir complejidad, salvo que posteriormente el profesor solicite explícitamente utilizarlo.

---

# 6. Flujo general de trabajo

El proyecto deberá seguir este flujo:

```text
Análisis de requisitos
        ↓
Diseño conceptual
        ↓
Modelo entidad-relación
        ↓
Modelo relacional
        ↓
Normalización
        ↓
PostgreSQL local
        ↓
Creación de tablas
        ↓
Restricciones
        ↓
Índices
        ↓
Datos sintéticos
        ↓
Consultas y operaciones
        ↓
Pruebas funcionales
        ↓
Pruebas de integridad
        ↓
Pruebas de casos límite
        ↓
Pruebas de volumen
        ↓
Pruebas de concurrencia
        ↓
EXPLAIN / EXPLAIN ANALYZE
        ↓
Optimización
        ↓
Versionado en Git
        ↓
Amazon RDS for PostgreSQL
        ↓
Despliegue del esquema
        ↓
Validación remota
        ↓
Documentación y evidencias
        ↓
Conclusiones
```

---

# 7. Regla fundamental de trabajo para el agente

El agente debe trabajar de manera incremental.

Después de cada fase deberá:

1. ejecutar o proporcionar los comandos necesarios;
2. verificar el resultado;
3. indicar qué evidencia debe guardarse;
4. detectar errores antes de continuar;
5. no considerar una fase terminada hasta que exista una validación observable.

Ejemplos de validaciones:

```sql
SELECT version();
```

```sql
\dt
```

```sql
\d nombre_tabla
```

```sql
SELECT COUNT(*) FROM pacientes;
```

```sql
EXPLAIN ANALYZE
SELECT ...;
```

El agente **no debe inventar resultados de pruebas**.

Los tiempos, conteos, errores y planes de ejecución incluidos en el reporte deben provenir de ejecuciones reales.

---

# 8. Volumen inicial del sistema

El laboratorio inicia aproximadamente con:

- 5,000 pacientes;
- 50 médicos;
- 100 tipos de estudios;
- 10 técnicos;
- 15 equipos.

Carga diaria estimada:

- aproximadamente 150 pacientes por día;
- cada paciente puede solicitar varios estudios.

La información crecerá continuamente.

El agente deberá considerar escenarios como:

- 50,000 solicitudes;
- 100,000 solicitudes;
- cientos de miles de estudios solicitados;
- cientos de miles de resultados;
- hasta aproximadamente 1,000,000 de filas en tablas transaccionales para pruebas de rendimiento, cuando el equipo local lo permita.

Los datos de prueba deben ser **sintéticos**.

No usar información médica real.

---

# 9. Requisitos funcionales mínimos

La base deberá soportar:

## Pacientes

Guardar:

- identificador;
- datos generales;
- fecha de nacimiento;
- sexo, cuando sea necesario para rangos de referencia;
- datos de contacto;
- antecedentes relevantes;
- estado activo/inactivo;
- fechas de creación y actualización.

El historial clínico del laboratorio no debe eliminarse por el hecho de que el paciente deje de utilizar el servicio.

## Médicos

Guardar:

- identificador;
- nombre;
- cédula profesional;
- especialidad;
- información de contacto;
- estado.

## Estudios

Catálogo de estudios que ofrece el laboratorio.

Guardar:

- código;
- nombre;
- descripción;
- unidad de medida;
- tipo de resultado;
- valores o texto de referencia;
- indicaciones;
- estado activo/inactivo.

Los valores de referencia pueden requerir una estructura separada si dependen de sexo, edad u otras condiciones.

## Solicitudes

Registrar:

- paciente;
- médico solicitante;
- fecha;
- estado;
- observaciones.

Una solicitud puede incluir varios estudios.

## Estudios de una solicitud

Crear una tabla intermedia que relacione:

- solicitud;
- estudio;
- estado individual;
- prioridad, cuando corresponda;
- fecha de incorporación.

No almacenar varios estudios dentro de una sola columna.

## Muestras

Registrar:

- código único;
- paciente/solicitud asociada mediante relaciones correctas;
- tipo de muestra;
- fecha y hora de toma;
- personal que realizó la toma;
- estado;
- observaciones.

Un estudio puede requerir una o más muestras.

El diseño debe soportar esa condición sin duplicar información innecesariamente.

## Resultados

Registrar resultados asociados al estudio solicitado.

Deben conservarse:

- resultado;
- unidad;
- valores de referencia relevantes;
- fecha;
- responsable;
- estado;
- observaciones.

No se debe perder el historial al corregir o repetir un resultado.

## Personal

Guardar:

- número de empleado;
- nombre;
- rol o función;
- datos de contacto;
- estado.

Como mínimo, debe ser posible identificar quién:

- tomó una muestra;
- procesó una muestra;
- registró o validó un resultado.

## Equipos

Guardar:

- código;
- nombre;
- fabricante;
- modelo;
- número de serie;
- estado operativo;
- fechas relevantes;
- observaciones.

Estados posibles, como mínimo:

- DISPONIBLE;
- MANTENIMIENTO;
- FUERA_SERVICIO.

## Procesamiento

Registrar:

- estudio solicitado;
- muestra utilizada;
- equipo utilizado;
- responsable;
- fecha/hora de inicio;
- fecha/hora de fin;
- estado;
- número de intento o repetición;
- observaciones.

---

# 10. Modelo relacional preliminar

El agente deberá revisar y mejorar este modelo antes de implementarlo.

Tablas principales propuestas:

```text
pacientes
medicos
personal
equipos
estudios

solicitudes
solicitud_estudios

muestras
muestra_estudios

procesamientos

resultados
resultado_historial

estudio_equipos
equipo_estado_historial
```

Podrán agregarse tablas adicionales si existe justificación técnica.

---

# 11. Relaciones principales esperadas

## Paciente → Solicitud

```text
paciente 1 ───── N solicitudes
```

Un paciente puede generar muchas solicitudes a lo largo del tiempo.

## Médico → Solicitud

```text
medico 1 ───── N solicitudes
```

Un médico puede solicitar estudios para muchos pacientes.

## Solicitud ↔ Estudio

Relación muchos a muchos resuelta mediante:

```text
solicitudes
     |
     | 1:N
     v
solicitud_estudios
     ^
     | N:1
     |
estudios
```

## Estudio ↔ Equipo

Algunos estudios requieren equipos específicos.

Relación muchos a muchos:

```text
estudios
    |
estudio_equipos
    |
equipos
```

## Solicitud de estudio ↔ Muestra

El diseño debe permitir que un estudio requiera una o varias muestras.

Se propone una tabla intermedia:

```text
muestra_estudios
```

que relacione:

```text
muestras
solicitud_estudios
```

## Procesamiento

Cada procesamiento deberá asociar como mínimo:

```text
solicitud_estudio
muestra
equipo
personal
```

según corresponda.

Un mismo estudio solicitado puede tener múltiples procesamientos en caso de repetición.

## Resultado

Un resultado debe corresponder a un estudio solicitado y, cuando aplique, al procesamiento que lo originó.

---

# 12. Requerimientos adicionales que deben soportarse

Después de varios meses aparecen nuevos requerimientos:

## Repetición de estudios

Un análisis puede repetirse debido a inconsistencias.

No sobrescribir simplemente el procesamiento anterior.

Registrar múltiples intentos.

Ejemplo:

```text
intento 1 → inconsistente
intento 2 → repetición
intento 3 → validado
```

## Equipos fuera de servicio

Debe ser posible:

- cambiar el estado actual del equipo;
- impedir o detectar el uso de un equipo fuera de servicio;
- conservar historial de cambios de estado.

## Estudios pendientes con muestra tomada

Debe poder consultarse:

> estudios pendientes de procesamiento cuya muestra ya fue tomada.

El modelo debe permitir responder esta consulta de forma clara y eficiente.

## Historial de modificaciones de resultados

No borrar el valor anterior.

Implementar uno de los siguientes enfoques, justificando la decisión:

1. tabla `resultado_historial`;
2. versionado de resultados;
3. trigger de auditoría;
4. combinación de los anteriores.

Para fines académicos se recomienda:

```text
resultados
resultado_historial
```

con un trigger de auditoría si resulta apropiado.

## Mayor volumen

Evaluar qué pasa si el laboratorio incrementa considerablemente:

- pacientes;
- solicitudes;
- muestras;
- procesamientos;
- resultados.

---

# 13. Normalización

El diseño deberá analizarse hasta al menos **Tercera Forma Normal (3FN)**.

Documentar:

## Primera Forma Normal

- un valor por celda;
- no almacenar listas separadas por comas;
- no guardar múltiples estudios dentro de una sola columna.

## Segunda Forma Normal

En tablas con claves compuestas, los atributos deben depender de la clave completa.

## Tercera Forma Normal

Evitar dependencias transitivas y duplicación innecesaria.

Ejemplo:

No copiar permanentemente:

```text
nombre_medico
```

dentro de `solicitudes` si ya existe `medico_id`.

La información deberá obtenerse mediante relaciones y JOIN.

---

# 14. Tipos de datos

Elegir tipos PostgreSQL apropiados.

Preferencias generales:

```text
BIGINT / BIGSERIAL
INTEGER
VARCHAR(n)
TEXT
DATE
TIMESTAMPTZ
NUMERIC
BOOLEAN
JSONB solamente cuando exista una justificación
```

No usar `VARCHAR` o `TEXT` de forma indiscriminada si existe un tipo mejor.

Las fechas y horas importantes deberán preferir:

```sql
TIMESTAMPTZ
```

cuando tenga sentido conservar correctamente la zona temporal.

---

# 15. Integridad de datos

Implementar y documentar:

- PRIMARY KEY;
- FOREIGN KEY;
- NOT NULL;
- UNIQUE;
- CHECK;
- DEFAULT;
- ON DELETE;
- ON UPDATE, cuando aplique.

Cada restricción debe tener una justificación.

Ejemplos de reglas a evaluar:

- cédula profesional única;
- código de estudio único;
- código de muestra único;
- número de serie de equipo único;
- fechas coherentes;
- procesamiento no puede terminar antes de comenzar;
- número de intento mayor que cero;
- estados limitados a valores permitidos;
- resultado numérico válido cuando el estudio requiera un valor numérico;
- no permitir relaciones con registros inexistentes.

---

# 16. Eliminación de información

Por tratarse de información histórica, evitar eliminaciones físicas innecesarias.

Preferir estados como:

```text
activo = false
```

o equivalentes para:

- pacientes;
- médicos;
- estudios;
- personal;
- equipos.

Las tablas históricas o transaccionales, como:

- solicitudes;
- resultados;
- procesamientos;

no deben eliminarse automáticamente solo porque un registro maestro se desactive.

Analizar cuidadosamente:

```text
ON DELETE RESTRICT
ON DELETE CASCADE
SET NULL
```

No utilizar `CASCADE` sin una justificación clara.

---

# 17. Índices

Los índices deberán responder a consultas reales.

No crear índices arbitrariamente.

Como mínimo analizar índices para:

```text
solicitudes(paciente_id, fecha_solicitud)
solicitudes(medico_id, fecha_solicitud)
solicitud_estudios(estado)
muestras(estado)
procesamientos(equipo_id, fecha_inicio)
resultados(solicitud_estudio_id)
resultados(fecha_resultado)
```

También evaluar índices compuestos.

Antes de crear un índice:

1. ejecutar la consulta;
2. obtener `EXPLAIN ANALYZE`;
3. guardar resultado;
4. crear índice;
5. repetir `EXPLAIN ANALYZE`;
6. comparar;
7. justificar si el índice aporta valor.

---

# 18. Consultas obligatorias

La base debe permitir consultar como mínimo:

1. historial de estudios realizados por un paciente;
2. estudios solicitados por un médico;
3. resultados asociados a una solicitud;
4. estudios pendientes de procesamiento;
5. muestras pendientes de análisis;
6. estudios realizados por cada equipo;
7. historial de resultados de un paciente;
8. estudios realizados durante un periodo determinado;
9. estudios pendientes cuya muestra ya fue tomada;
10. resultados que han tenido modificaciones;
11. procesamientos repetidos;
12. equipos actualmente fuera de servicio.

Las consultas deberán incluir JOIN cuando corresponda.

---

# 19. Operaciones principales

Documentar y probar:

## INSERT

Ejemplos:

- paciente;
- médico;
- solicitud;
- estudio en solicitud;
- muestra;
- procesamiento;
- resultado.

## SELECT

Consultas simples y complejas.

## UPDATE

Ejemplos:

- actualizar teléfono del paciente;
- cambiar estado de una muestra;
- marcar equipo fuera de servicio;
- actualizar un resultado generando historial.

## DELETE

Se deben probar eliminaciones permitidas y eliminaciones rechazadas por integridad referencial.

No eliminar datos históricos únicamente para demostrar `DELETE`.

---

# 20. Transacciones

Los flujos que involucren varias operaciones dependientes deben probarse mediante transacciones.

Ejemplo conceptual:

```sql
BEGIN;

INSERT INTO solicitudes (...);

INSERT INTO solicitud_estudios (...);
INSERT INTO solicitud_estudios (...);

COMMIT;
```

También probar un error:

```sql
BEGIN;

-- operación 1 correcta
-- operación 2 incorrecta

ROLLBACK;
```

Demostrar que no queda información parcialmente registrada.

---

# 21. Casos de prueba

Cada prueba debe documentar:

```text
ID:
Nombre:
Situación:
Objetivo:
Datos de entrada:
Operaciones realizadas:
Resultado esperado:
Resultado obtenido:
Estado: APROBADO / FALLIDO
Evidencia:
Comentario:
```

---

# 22. Pruebas funcionales mínimas

Crear como mínimo pruebas para:

1. registrar paciente válido;
2. registrar médico válido;
3. crear solicitud válida;
4. agregar varios estudios;
5. registrar muestra;
6. asociar muestra a estudio;
7. registrar procesamiento;
8. registrar resultado;
9. consultar historial del paciente;
10. consultar solicitudes del médico;
11. consultar estudios pendientes;
12. consultar muestras pendientes;
13. consultar actividad por equipo;
14. actualizar estado de equipo;
15. repetir procesamiento;
16. actualizar resultado conservando historial.

---

# 23. Pruebas de restricciones

Probar explícitamente operaciones que deben fallar.

Ejemplos:

## Duplicado

Intentar registrar dos equipos con el mismo número de serie.

Resultado esperado:

```text
PostgreSQL rechaza la operación por UNIQUE.
```

## Clave foránea inválida

Crear solicitud para:

```text
paciente_id inexistente
```

Resultado esperado:

```text
PostgreSQL rechaza la operación por FOREIGN KEY.
```

## Campo obligatorio

Paciente sin nombre.

Esperado:

```text
rechazo por NOT NULL
```

## CHECK

Registrar:

```text
numero_intento = 0
```

si la condición es:

```sql
CHECK (numero_intento > 0)
```

Esperado:

```text
operación rechazada
```

---

# 24. Escenarios límite

Se deben probar valores cercanos a los límites esperados.

Ejemplos:

- textos cercanos a la longitud máxima;
- fechas antiguas;
- pacientes con gran cantidad de solicitudes;
- solicitudes con muchos estudios;
- estudios con múltiples muestras;
- numerosos procesamientos de un equipo;
- varios intentos de un estudio;
- gran cantidad de resultados asociados a un paciente.

No utilizar valores absurdos únicamente para generar errores.

Cada escenario debe relacionarse con una situación realista del sistema.

---

# 25. Pruebas de volumen

Evaluar consultas con distintos tamaños de información.

Ejemplo:

```text
5,000 filas
50,000 filas
100,000 filas
500,000 filas
1,000,000 filas
```

El volumen exacto dependerá de los recursos disponibles.

Medir:

- tiempo de consulta;
- plan de ejecución;
- tipo de scan;
- uso de índices;
- costo estimado;
- filas procesadas.

Utilizar:

```sql
EXPLAIN
```

y preferentemente:

```sql
EXPLAIN (ANALYZE, BUFFERS)
SELECT ...;
```

No inventar tiempos.

---

# 26. Comparación antes y después de índices

Crear una tabla de resultados reales similar a:

| Filas | Consulta | Sin índice | Con índice | Observación |
|---:|---|---:|---:|---|
| 5,000 | historial paciente | real | real | análisis |
| 50,000 | historial paciente | real | real | análisis |
| 100,000 | pendientes | real | real | análisis |

Los valores deberán provenir de ejecuciones reales.

---

# 27. Pruebas de concurrencia

Evaluar múltiples operaciones simultáneas cuando sea aplicable.

Escenarios sugeridos:

## Caso A

Dos sesiones intentan modificar el mismo resultado.

Analizar:

- bloqueo;
- aislamiento;
- valor final;
- pérdida o no de actualizaciones.

## Caso B

Varias sesiones registran solicitudes simultáneamente.

## Caso C

Dos operadores intentan utilizar el mismo recurso lógico cuando exista una regla que lo impida.

Utilizar dos sesiones `psql` o un script Python.

Analizar el nivel de aislamiento de PostgreSQL.

No afirmar que existe un problema de concurrencia sin reproducirlo.

---

# 28. Generación de datos sintéticos

Crear scripts separados.

Ejemplo:

```text
scripts/generar_datos.py
```

El script debe poder generar:

- pacientes;
- médicos;
- estudios;
- solicitudes;
- muestras;
- procesamientos;
- resultados.

Debe permitir configurar la cantidad de registros.

Ejemplo conceptual:

```bash
python scripts/generar_datos.py --pacientes 5000 --solicitudes 50000
```

No es obligatorio que la interfaz de línea de comandos sea exactamente esa.

---

# 29. Semilla mínima

Debe existir un archivo SQL que permita crear un pequeño conjunto reproducible para pruebas manuales.

Ejemplo:

```text
sql/03_seed_minimo.sql
```

Debe incluir suficientes datos para:

- múltiples pacientes;
- médicos;
- varios estudios;
- equipos;
- personal;
- solicitud con múltiples estudios;
- muestras;
- procesamiento;
- resultados.

---

# 30. Migraciones y versionado

Aunque no existe backend, la estructura de la base debe versionarse.

Se recomienda usar scripts SQL numerados.

Ejemplo:

```text
migrations/
001_initial_schema.sql
002_constraints.sql
003_indexes.sql
004_audit_results.sql
005_equipment_status_history.sql
```

Nunca modificar silenciosamente una migración ya utilizada para desplegar un entorno.

Para cambios nuevos:

```text
crear una nueva migración
```

---

# 31. Git

Todo el proyecto debe quedar versionado.

Nunca subir:

- contraseñas;
- endpoints privados si contienen secretos;
- credenciales de AWS;
- archivos `.env`;
- claves privadas.

Crear:

```text
.env.example
```

si se utilizan variables auxiliares.

---

# 32. Despliegue en Amazon RDS

El despliegue deberá utilizar:

**Amazon RDS for PostgreSQL**

Flujo:

1. terminar y validar el esquema local;
2. crear la instancia RDS;
3. configurar conectividad;
4. configurar Security Group;
5. restringir acceso;
6. obtener endpoint;
7. probar conexión con `psql` o pgAdmin;
8. aplicar migraciones;
9. cargar semilla de prueba;
10. ejecutar consultas de validación;
11. guardar evidencias.

No diseñar las tablas por primera vez directamente en producción.

El diseño debe originarse localmente y reproducirse en RDS.

---

# 33. Seguridad básica de RDS

No dejar PostgreSQL abierto a todo Internet.

El puerto por defecto es:

```text
5432
```

Cuando se utilice acceso público para el laboratorio:

- permitir solo la IP necesaria;
- retirar reglas innecesarias;
- utilizar contraseña robusta;
- no usar el usuario maestro para todas las operaciones;
- utilizar TLS cuando corresponda;
- eliminar o detener recursos cuando ya no se necesiten para evitar costos.

La configuración final debe documentarse sin mostrar secretos.

---

# 34. Usuario de aplicación / laboratorio

Aunque no existe backend, crear un usuario distinto al superusuario para las pruebas ordinarias.

Ejemplo conceptual:

```sql
CREATE ROLE laboratorio_app LOGIN PASSWORD '...';
```

Otorgar únicamente los permisos necesarios.

No colocar la contraseña real dentro del repositorio.

---

# 35. Evidencias

Crear una carpeta:

```text
evidencias/
```

Guardar capturas de:

- versión PostgreSQL;
- creación de base;
- tablas;
- relaciones;
- restricciones;
- índices;
- inserciones correctas;
- errores esperados;
- consultas;
- `EXPLAIN ANALYZE`;
- pruebas de volumen;
- concurrencia;
- conexión a RDS;
- tablas en RDS;
- consultas ejecutadas contra RDS.

Nombrar archivos de forma ordenada.

Ejemplo:

```text
E01_postgresql_version.png
E02_tablas_creadas.png
E03_foreign_keys.png
E04_unique_rechazo.png
E05_consulta_historial.png
E06_explain_sin_indice.png
E07_explain_con_indice.png
E08_conexion_rds.png
```

---

# 36. Evidencias de uso de IA

La actividad exige evidencia de interacción con IA generativa.

Crear:

```text
evidencias/ia/
```

Para cada parte donde se utilice IA guardar:

- captura de la pregunta;
- captura o extracto de la respuesta;
- punto del proyecto donde se utilizó;
- comentario reflexivo.

Formato sugerido:

```text
Uso de IA:
Se utilizó IA para proponer una estructura inicial de las entidades.

Decisión tomada:
Se revisó la propuesta y se separó la relación entre solicitudes y estudios
mediante solicitud_estudios para representar correctamente una relación N:M.

Comentario reflexivo:
La propuesta inicial fue útil como punto de partida, pero fue necesario validar
la normalización y adaptar las relaciones a los requisitos reales del caso.
```

No escribir:

```text
"La IA hizo todo y se copió."
```

Debe demostrarse análisis del estudiante.

---

# 37. Documentación técnica requerida

La documentación deberá incluir:

## Modelo

- diagrama entidad-relación;
- descripción de entidades;
- cardinalidades;
- decisiones de diseño.

## Diccionario de datos

Para cada tabla:

| Campo | Tipo | Nulo | Llave | Restricción | Descripción |
|---|---|---|---|---|---|

## Llaves

Documentar:

- PK;
- FK;
- UNIQUE;
- relaciones.

## Restricciones

Explicar:

- qué protege cada restricción;
- qué error evita.

## Índices

Para cada índice:

```text
Nombre:
Tabla:
Columnas:
Consulta beneficiada:
Justificación:
Evidencia:
```

## Operaciones

Describir las principales operaciones del laboratorio.

## Pruebas

Documentar cada caso.

## Rendimiento

Comparar consultas antes y después de optimizaciones.

## Limitaciones

Identificar problemas encontrados.

## Escalabilidad

Explicar qué ocurriría al aumentar significativamente el volumen.

---

# 38. Preguntas que el proyecto debe poder responder

Al terminar las pruebas debe poder contestarse con evidencia:

1. ¿La base permite realizar correctamente los flujos principales?
2. ¿Las restricciones garantizan la integridad?
3. ¿Existen operaciones que no pueden realizarse adecuadamente?
4. ¿Qué ocurre cuando aumenta el volumen de información?
5. ¿Qué consultas pueden convertirse en un problema?
6. ¿En qué escenarios la solución actual deja de ser suficiente?
7. ¿Qué modificaciones serían necesarias?
8. ¿Los índices seleccionados realmente mejoran consultas importantes?
9. ¿Cómo se comporta PostgreSQL con operaciones simultáneas?
10. ¿El esquema local puede reproducirse correctamente en RDS?

---

# 39. Posibles limitaciones a investigar

No asumir que estas limitaciones existen. Deben comprobarse o analizarse.

Investigar:

- crecimiento excesivo de `resultados`;
- crecimiento de `procesamientos`;
- consultas históricas sin filtros;
- índices demasiado numerosos;
- tablas de auditoría muy grandes;
- actualización frecuente de estados;
- bloqueos concurrentes;
- almacenamiento de valores de referencia históricos;
- crecimiento de muestras;
- necesidad futura de particionamiento.

---

# 40. Posibles mejoras futuras

Dependiendo de los resultados reales, analizar:

- índices adicionales;
- índices parciales;
- índices compuestos;
- particionamiento por fecha;
- archivado de información;
- vistas materializadas;
- ajustes de configuración;
- réplicas de lectura;
- mayor capacidad de RDS;
- mejoras al modelo;
- tablas especializadas para rangos de referencia;
- auditoría más completa.

No implementar una solución compleja si las pruebas no justifican su necesidad.

---

# 41. Estructura recomendada del repositorio

```text
laboratorio-clinico-db/
│
├── README.md
├── .gitignore
│
├── docs/
│   ├── modelo-er/
│   │   └── laboratorio-er.png
│   │
│   ├── diccionario-datos.md
│   ├── decisiones-diseno.md
│   ├── indices.md
│   ├── pruebas.md
│   ├── rendimiento.md
│   ├── despliegue-rds.md
│   └── conclusiones.md
│
├── migrations/
│   ├── 001_initial_schema.sql
│   ├── 002_constraints.sql
│   ├── 003_indexes.sql
│   ├── 004_result_audit.sql
│   └── 005_equipment_history.sql
│
├── sql/
│   ├── 01_create_database.sql
│   ├── 02_seed_minimo.sql
│   ├── 03_queries.sql
│   ├── 04_crud_tests.sql
│   ├── 05_integrity_tests.sql
│   ├── 06_limit_tests.sql
│   ├── 07_performance_tests.sql
│   └── 08_concurrency_tests.sql
│
├── scripts/
│   ├── generar_datos.py
│   └── prueba_concurrencia.py
│
└── evidencias/
    ├── local/
    ├── rendimiento/
    ├── concurrencia/
    ├── rds/
    └── ia/
```

La estructura puede ajustarse, pero deberá mantenerse ordenada.

---

# 42. Orden exacto recomendado para ejecutar el proyecto

## Fase 1 — Requisitos

- convertir el caso de uso en reglas de negocio;
- detectar relaciones;
- detectar restricciones;
- identificar datos históricos.

### Validación

Crear `docs/requisitos.md`.

---

## Fase 2 — Modelo ER

Diseñar entidades y relaciones.

### Validación

Revisar que pueda representarse:

- N solicitudes por paciente;
- N estudios por solicitud;
- múltiples muestras por estudio;
- varios equipos por tipo de estudio;
- múltiples intentos de procesamiento;
- historial de resultados.

---

## Fase 3 — Modelo relacional

Definir:

- tablas;
- PK;
- FK;
- tipos de datos.

### Validación

Crear diccionario preliminar.

---

## Fase 4 — Normalización

Comprobar 1FN, 2FN y 3FN.

### Validación

Documentar decisiones.

---

## Fase 5 — PostgreSQL local

Instalar/configurar PostgreSQL.

Crear:

```text
laboratorio_clinico
```

### Validación

```sql
SELECT current_database();
SELECT version();
```

---

## Fase 6 — Esquema

Aplicar migración inicial.

### Validación

```text
listar tablas
inspeccionar PK
inspeccionar FK
```

---

## Fase 7 — Restricciones

Agregar:

```text
NOT NULL
UNIQUE
CHECK
FK
```

### Validación

Ejecutar pruebas que intencionalmente deben fallar.

---

## Fase 8 — Datos mínimos

Cargar semilla.

### Validación

Ejecutar conteos por tabla.

---

## Fase 9 — Consultas

Implementar todas las consultas obligatorias.

### Validación

Cada consulta debe devolver datos coherentes con la semilla.

---

## Fase 10 — CRUD

Probar:

```text
INSERT
SELECT
UPDATE
DELETE
```

### Validación

Registrar resultado esperado y obtenido.

---

## Fase 11 — Auditoría

Implementar historial de resultados.

### Validación

Modificar un resultado y demostrar que el valor anterior permanece disponible.

---

## Fase 12 — Repeticiones

Implementar varios intentos de procesamiento.

### Validación

Un estudio solicitado debe poder tener más de un procesamiento sin perder el anterior.

---

## Fase 13 — Equipos

Implementar historial de estado.

### Validación

Cambiar:

```text
DISPONIBLE
→ MANTENIMIENTO
→ DISPONIBLE
```

y conservar historial.

---

## Fase 14 — Datos masivos

Generar datos sintéticos.

### Validación

Registrar:

```sql
SELECT COUNT(*)
```

por tabla.

---

## Fase 15 — Rendimiento

Ejecutar consultas críticas.

Guardar:

```text
EXPLAIN ANALYZE
```

antes y después de índices.

---

## Fase 16 — Concurrencia

Ejecutar pruebas con múltiples sesiones.

### Validación

Guardar evidencia del comportamiento y explicar qué ocurrió.

---

## Fase 17 — RDS

Crear Amazon RDS PostgreSQL.

### Validación

Conectar desde el equipo local.

---

## Fase 18 — Despliegue

Aplicar migraciones en RDS.

### Validación

Comparar estructura local y remota.

---

## Fase 19 — Pruebas en RDS

Ejecutar un subconjunto representativo de:

- INSERT;
- SELECT;
- JOIN;
- integridad;
- rendimiento básico.

---

## Fase 20 — Documentación final

Compilar:

- diseño;
- pruebas;
- evidencias;
- resultados;
- limitaciones;
- mejoras;
- conclusiones.

---

# 43. Reglas para el agente de IA durante la implementación

El agente debe:

1. trabajar una fase a la vez;
2. explicar brevemente qué va a hacer;
3. proporcionar comandos exactos;
4. esperar o solicitar el resultado cuando sea necesario para validar;
5. corregir errores antes de avanzar;
6. no inventar capturas;
7. no inventar tiempos de ejecución;
8. no inventar resultados de `EXPLAIN`;
9. no asumir que una restricción funciona sin probarla;
10. no asumir que un índice mejora una consulta sin medirlo;
11. evitar complejidad innecesaria;
12. mantener PostgreSQL como tecnología principal;
13. no convertir el proyecto en una aplicación web;
14. mantener scripts reproducibles;
15. indicar en qué momento tomar evidencia;
16. explicar el propósito académico de cada prueba;
17. mantener un registro claro de las decisiones de diseño.

---

# 44. Criterios de aceptación

El proyecto se considerará terminado cuando:

- exista un modelo ER coherente;
- exista un esquema PostgreSQL reproducible;
- las relaciones estén correctamente implementadas;
- las restricciones hayan sido probadas;
- las operaciones principales funcionen;
- las consultas obligatorias funcionen;
- exista historial de resultados;
- puedan registrarse repeticiones;
- puedan registrarse equipos fuera de servicio;
- existan índices justificados;
- exista evidencia de `EXPLAIN ANALYZE`;
- existan pruebas con volumen elevado;
- exista al menos una prueba de concurrencia;
- la base esté desplegada en Amazon RDS;
- el esquema remoto haya sido validado;
- exista documentación técnica;
- existan evidencias;
- se hayan identificado limitaciones reales;
- existan propuestas de mejora;
- exista evidencia y reflexión del uso de IA.

---

# 45. Entregables

El agente deberá ayudar a producir:

1. repositorio Git;
2. scripts SQL;
3. migraciones;
4. modelo ER;
5. diccionario de datos;
6. documentación de restricciones;
7. documentación de índices;
8. consultas principales;
9. casos de prueba;
10. resultados de pruebas;
11. scripts de generación de datos;
12. pruebas de rendimiento;
13. pruebas de concurrencia;
14. documentación de RDS;
15. evidencias;
16. análisis de limitaciones;
17. propuestas de mejora;
18. conclusiones;
19. material necesario para construir el reporte final.

---

# 46. Consideraciones sobre el reporte

El reporte final se elaborará después de realizar las pruebas.

No escribir resultados ficticios de antemano.

El reporte deberá basarse en:

```text
diseño realmente implementado
+
pruebas realmente ejecutadas
+
evidencias reales
+
resultados reales
+
análisis del estudiante
```

La conclusión deberá indicar:

- funcionalidades soportadas correctamente;
- problemas encontrados;
- limitaciones;
- escenarios donde la solución sería insuficiente;
- cambios necesarios;
- pruebas adicionales necesarias antes de producción.

---

# 47. Nota sobre la guía de la materia

Existe una guía proporcionada por la materia orientada a un flujo:

```text
desarrollo local
→ esquema versionado
→ despliegue en Amazon RDS
→ validación
```

La guía utiliza MySQL como ejemplo.

Para este proyecto se utilizará **PostgreSQL**, adaptando los mismos principios:

- diseñar y probar primero en local;
- mantener el esquema versionado;
- evitar diseñar directamente en RDS;
- desplegar de forma reproducible;
- proteger credenciales;
- restringir el acceso de red;
- validar el entorno remoto.

---

# 48. Primera tarea que debe realizar el agente

Comenzar por:

> Analizar el caso de uso y proponer el modelo entidad-relación completo de la base de datos del laboratorio clínico.

La respuesta deberá incluir:

1. lista de entidades;
2. propósito de cada entidad;
3. atributos principales;
4. claves primarias;
5. relaciones;
6. cardinalidades;
7. tablas intermedias;
8. reglas de integridad;
9. tratamiento del historial;
10. tratamiento de repeticiones;
11. tratamiento de equipos fuera de servicio;
12. posibles dudas o decisiones de diseño.

Después de validar el modelo ER, continuar con el modelo relacional y la implementación PostgreSQL.

---

# 49. Instrucción final al agente

No intentar terminar todo el proyecto en una sola respuesta.

Trabajar por etapas.

Para cada etapa:

```text
PLAN
→ IMPLEMENTACIÓN
→ VALIDACIÓN
→ EVIDENCIA
→ DOCUMENTACIÓN
```

No avanzar cuando una validación importante falle.

El objetivo no es únicamente crear una base que "funcione", sino demostrar mediante pruebas que:

- mantiene la integridad;
- soporta los flujos del laboratorio;
- responde razonablemente al crecimiento;
- posee límites identificables;
- puede desplegarse de manera reproducible en Amazon RDS.
