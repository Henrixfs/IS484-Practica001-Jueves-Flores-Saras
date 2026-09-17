# IS484-Practica001-Dia-Flores

**Práctica Calificada 001**

| | |
|---|---|
| **Curso** | Inteligencia Artificial I (IS-484) |
| **Escuela** | Escuela Profesional de Ingeniería de Sistemas |
| **Universidad** | Universidad Nacional de San Cristóbal de Huamanga (UNSCH) |
| **Alumno** | Henry Josue Flores Saras |

---

## Descripción

Este repositorio contiene la **Parte 2** de la práctica: tres agentes inteligentes diseñados y programados en Python. Cada uno sigue el mismo proceso:

```
Ficha PEAS  →  Justificación de reglas  →  Código  →  Simulación y pruebas
```

Cada agente es una función que recibe sus percepciones y retorna una tupla `(accion, motivo)`, donde `motivo` explica en texto la razón de la decisión.

> La **Parte 1** (clasificación de sistemas de IA y ficha PEAS del sistema anti-fraude bancario) se entrega a mano en PDF por Google Classroom.

---

## Estructura del repositorio

```
IS484-Practica001-Dia-Flores/
├── Ejercicio1_Credito_Flores.ipynb      # Agente de evaluación de crédito bancario
├── Ejercicio2_Soporte_Flores.ipynb      # Agente de escalamiento de tickets de soporte
├── Ejercicio3_Inspeccion_Flores.ipynb   # Agente de inspección visual de productos
└── README.md
```

Todos los notebooks tienen la misma secuencia de secciones:

1. Ficha PEAS
2. Justificación de las reglas
3. Código
4. Simulación y pruebas
5. Visualización (solo en el Ejercicio 1)

---

## Ejercicios

### Ejercicio 1 · Evaluación de solicitud de crédito bancario

`agente_credito(ingreso_mensual, monto_solicitado, tiene_historial_moroso)`

El agente calcula `relacion = monto_solicitado / ingreso_mensual` y decide así:

| Historial moroso | Relación monto/ingreso | Acción |
|---|---|---|
| No | ≤ 3 | `aprobar` |
| No | > 3 y ≤ 6 | `aprobar con condiciones` |
| No | > 6 | `rechazar` |
| Sí | ≤ 1 | `aprobar con condiciones` |
| Sí | > 1 | `rechazar` |
| — | ingreso o monto ≤ 0 | `rechazar` (dato inválido) |

- **Pruebas:** 8 combinaciones, con casos en el borde (relación exactamente 3.0, 3.01 y 1.0 con historial moroso).
- **Visualización:** se generan 200 solicitudes aleatorias con `numpy.random` y se grafica un scatter de relación vs. ingreso mensual, con cada punto coloreado según la acción y las líneas de umbral marcadas.

### Ejercicio 2 · Escalamiento de tickets de soporte técnico

`agente_soporte(tiempo_espera_minutos, nivel_urgencia, cliente_premium)`

| Urgencia | Condición de espera | Acción |
|---|---|---|
| Alta | inmediata | `escalar a nivel 3` |
| Media | premium ≥ 30 min · estándar ≥ 60 min | `escalar a nivel 2` |
| Baja | premium ≥ 120 min · estándar ≥ 240 min | `escalar a nivel 2` |
| Media o baja | por debajo del umbral | `mantener en nivel 1` |
| Inválida | urgencia desconocida o tiempo negativo | `revisar clasificacion` |

- **Pruebas:** 10 combinaciones. Incluyen casos donde las condiciones no coinciden (urgencia baja con cliente premium y espera alta), un borde en 30 minutos exactos y entradas con mayúsculas o espacios.

### Ejercicio 3 · Inspección visual de un producto

`agente_inspeccion(imagen)`

La percepción es una matriz NumPy de 5×5 (0 = píxel normal, 1 = píxel defectuoso).

| Píxeles defectuosos | Acción |
|---|---|
| 0 | `aprobar` |
| 1 a 3 | `revision manual` |
| Más de 3 | `rechazar` |
| Imagen no 5×5 o no binaria | `revision manual` (lectura corrupta) |

- **Pruebas:** matrices con 0, 1, 3 y 4 defectos (los bordes entre acciones), 8 defectos y una imagen corrupta.
- **Visualización:** cada imagen se muestra con `plt.imshow(imagen, cmap="gray")`.

---

## Requisitos y ejecución

- Python 3.9 o superior
- `numpy`
- `matplotlib`
- `jupyter`

```bash
# Clonar el repositorio
git clone https://github.com/TU_USUARIO/IS484-Practica001-Dia-Flores.git
cd IS484-Practica001-Dia-Flores

# Instalar dependencias
pip install numpy matplotlib jupyter

# Abrir los notebooks
jupyter notebook
```

También se pueden abrir directamente en **Google Colab** (Archivo → Subir notebook) o en **VS Code** con la extensión de Jupyter.

---

## Conceptos aplicados

- **Modelo PEAS** (Performance, Environment, Actuators, Sensors) para especificar cada agente.
- **Agentes reactivos simples:** las decisiones dependen solo de la percepción actual, mediante reglas condición–acción.
- **Validación de percepciones:** los datos corruptos o inválidos se manejan de forma explícita, para que el agente no actúe de manera irracional ante lecturas erróneas.
- **Casos límite:** se prueba en los bordes exactos entre dos decisiones.