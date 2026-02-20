# DATOS - Relevamiento de Limpieza Escolar (Version 3 Final)


## 2) Universo analizado
- Registros totales en relevamiento: **1122**
- Registros con CUE valido (universo analizado): **1060**
- Registros sin CUE (excluidos): **62**
- Registros en padron geográfico: **1278**
- Escuelas con match geográfico: **1037 / 1060**
- CUE sin georreferencia: **23**

## 3) Criterio final definitivo (sin imputar faltantes criticos)
- `R_obj = 10.0` secciones equivalentes por personal de limpieza.
- `W_dep = 0.5` (cada dependencia distinta a salones suma media seccion equivalente).
- `extra_compartidos = 2.0` solo con evidencia explicita:
  - `comparte_edif != "No comparte"` y
  - `portero_limpia_esp_compartidos == "Si"`.
- Regla de criticidad de dato faltante: respuestas faltantes en espacios compartidos **no se imputan**.

## 4) Formulas utilizadas
```text
secciones_equivalentes = secciones + (0.5 * dependencias) + extra_compartidos_explicito

porteros_efectivos_estimados =
  cant_porteros, si sit_actual contiene "Activa/o" y no tiene bloqueantes
  cant_porteros - 1, si contiene "Activa/o" y tiene bloqueantes
  cant_porteros, si sit_actual == "A meses de jubilarse"
  0, en el resto

req_total = ceil(secciones_equivalentes / 10)
req_priv = max(req_total - porteros_efectivos_estimados, 0)
gap_priv = req_priv - privados_actuales
```

## 5) Resultado global del modelo ajustado
- Escuelas con prioridad (`gap_priv > 0`): **552**
- Escuelas con exceso (`gap_priv < 0` y privados > 0): **151**
- Escuelas en equilibrio (`gap_priv = 0`): **357**
- Necesidad privada total: **649.0**
- Exceso privado total: **209.0**

## 6) Escenarios de redistribucion (comparativo)
| Escenario | Movible maximo | Cobertura sobre deficit | Deficit residual |
|---|---:|---:|---:|
| Misma localidad + misma empresa | 44.0 | 6.78% | 605.0 |
| Misma localidad (empresa agnostica) | 135.0 | 20.80% | 514.0 |
| Mismo departamento + misma empresa | 65.0 | 10.02% | 584.0 |
| Provincia completa (sin limite territorial ni empresa) | 209.0 | 32.20% | 440.0 |

### Capacidad de equilibrio por estrategia de asignacion (escenario provincial libre)
- Escuelas equilibrables si se priorizan brechas pequenas: **209**
- Escuelas equilibrables si se priorizan brechas grandes: **112**

## 7) Profundizacion: edificios compartidos y faltantes criticos
- Escuelas que comparten edificio: **541**
- Con respuesta explicita `Si` (se aplica extra de carga): **200**
- Con respuesta explicita `No`: **73**
- Sin respuesta en campo critico (no imputado): **262**
- Inconsistencias (`comparte` y respuesta `No comparte edificio`): **6**
- Decision metodologica aplicada: los faltantes no modifican carga; se reportan para auditoria y mejora de calidad del relevamiento.

## 8) Profundizacion: dependencias como carga operativa
- Dependencias promedio: **4.93**
- Dependencias mediana: **4.0**
- Dependencias p90: **10.0**
- Escuelas con dependencias > secciones: **448** (42.26%)
- Correlacion Pearson secciones-dependencias: **0.4408**
- Escuelas con gap modificado por incluir dependencias+compartidos: **153**

## 9) Seccion: porteros no presentes fisicamente (estimacion)
- Porteros nominales totales: **807.0**
- Porteros efectivos estimados (escenario base): **728.0**
- Porteros no fisicos estimados (escenario base): **79.0** (9.79%)
- Sensibilidad optimista: no fisicos **39.0**
- Sensibilidad pesimista: no fisicos **130.0**

| `sit_actual` | Escuelas | Nominal | Efectivo est. | No fisico est. | % no fisico |
|---|---:|---:|---:|---:|---:|
| De licencia por enfermedad (largo tratamiento) | 12 | 12.0 | 0.0 | 12.0 | 100.00% |
| Activa/o, Con cambio de destino | 11 | 25.0 | 14.0 | 11.0 | 44.00% |
| Activa/o, Con tareas pasivas / Cambio de funciones | 9 | 19.0 | 10.0 | 9.0 | 47.37% |
| Con cambio de destino | 8 | 9.0 | 0.0 | 9.0 | 100.00% |
| Activa/o, De licencia por enfermedad (largo tratamiento) | 8 | 17.0 | 9.0 | 8.0 | 47.06% |
| De licencia por enfermedad (largo tratamiento), Adscripto / En comisión en otra repartición | 2 | 7.0 | 0.0 | 7.0 | 100.00% |
| Activa/o, De licencia por enfermedad (largo tratamiento), A meses de jubilarse | 5 | 15.0 | 10.0 | 5.0 | 33.33% |
| Activa/o, Adscripto / En comisión en otra repartición | 4 | 9.0 | 5.0 | 4.0 | 44.44% |
| Con tareas pasivas / Cambio de funciones | 3 | 3.0 | 0.0 | 3.0 | 100.00% |
| (vacio) | 2 | 2.0 | 0.0 | 2.0 | 100.00% |
| De licencia por enfermedad (largo tratamiento), Con cambio de destino | 1 | 2.0 | 0.0 | 2.0 | 100.00% |
| Con tareas pasivas / Cambio de funciones, A meses de jubilarse | 1 | 2.0 | 0.0 | 2.0 | 100.00% |
| Activa/o, Con tareas pasivas / Cambio de funciones, Adscripto / En comisión en otra repartición | 1 | 3.0 | 2.0 | 1.0 | 33.33% |
| Activa/o, Con tareas pasivas / Cambio de funciones, Con cambio de destino | 1 | 2.0 | 1.0 | 1.0 | 50.00% |
| Adscripto / En comisión en otra repartición | 1 | 1.0 | 0.0 | 1.0 | 100.00% |
| De licencia por enfermedad (largo tratamiento), A meses de jubilarse | 1 | 1.0 | 0.0 | 1.0 | 100.00% |
| Activa/o, De licencia por enfermedad (largo tratamiento), A meses de jubilarse, Adscripto / En comisión en otra repartición | 1 | 1.0 | 0.0 | 1.0 | 100.00% |
| Activa/o | 430 | 600.0 | 600.0 | 0.0 | 0.00% |
| Activa/o, A meses de jubilarse | 27 | 59.0 | 59.0 | 0.0 | 0.00% |
| A meses de jubilarse | 15 | 18.0 | 18.0 | 0.0 | 0.00% |

## 10) Calidad del dato de porteros efectivos
| Bandera | Definicion | Escuelas |
|---|---|---:|
| `fuerte` | Activa/o sin condicion bloqueante | 457 |
| `estimado_mixto` | Activa/o con condicion bloqueante | 40 |
| `estimado_jubilacion` | Solo a meses de jubilarse | 15 |
| `probable_no_disponible` | Sin Activa/o y con dotacion nominal | 31 |
| `sin_portero` | Sin dotacion nominal de portero | 517 |

## 11) Conteos por escala de priorizacion
- Prioridad critica: **38**
- Prioridad alta: **10**
- Prioridad media: **504**
- Exceso alto: **10**
- Exceso medio: **35**
- Exceso leve: **106**

## 12) Top 10 necesidad por departamento
| Departamento | Necesidad estimada |
|---|---:|
| GOYA | 85.0 |
| CAPITAL | 67.0 |
| ESQUINA | 52.0 |
| CURUZÚ CUATIÁ | 47.0 |
| BELLA VISTA | 43.0 |
| LAVALLE | 36.0 |
| MONTE CASEROS | 31.0 |
| SANTO TOMÉ | 28.0 |
| MERCEDES | 25.0 |
| PASO DE LOS LIBRES | 24.0 |

## 13) CUE sin georreferencia
- `118011970`
- `160180701`
- `180010350`
- `180019400`
- `180039909`
- `180045100`
- `180047700`
- `180060009`
- `180106300`
- `180131101`
- `180137200`
- `180164099`
- `180170001`
- `180173902`
- `180195709`
- `180264200`
- `180273301`
- `180321500`
- `180970500`
- `190104600`
- `190164301`
- `280096600`
- `280110700`

---

|
