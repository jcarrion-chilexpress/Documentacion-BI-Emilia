# Documentación del Proyecto - Indicadores Emilia

## 📋 Diccionario de Indicadores

* Tabla origen `adl_gold.enol_v2.t_captura_api_onemarketer_encuesta_data_cruda`

| Indicador | Descripción | Estado |
|------------|------------|------------|
| `%Satisfaccion cliente / CSAT`| Mide que Tan satisfecho esta el usuario con el Canal | 🟡 En Validación |
| `%Q EPA ISN / Indice de satisfaccion Neta`|Mide la Satisfaccion reltativa del clientes en una escala de 1 a 7<br>• Cliente Satisfecha entre 6 y 7 <br>• Cliente Neutro evalua 5 <br>• Cliente insatisfecho evalua entra 1 y 4 | 🟡 En Validación |
| `%FCR / Tasa de Resolución`| % de Respuestas de clientes que declara que su problema fue resuelto | 🟡 En Validación |
| `%Facilidad Acceso`| Índice Neto de Satisfacción | 🟡 En Validación |
| `Q interacciones IA`| Índice Neto de Satisfacción | 🟡 En Validación |
| `Q interacciones Ejecutivo`| Índice Neto de Satisfacción | 🟡 En Validación |
| `% Abandono` | Porcentaje de conversaciones abandonadas | 🔴 Pendiente |
| `%Tasa de derivación` | % de conversaciones derivadas hacia un ejecutivo | 🔴 Pendiente |
| `Tasa de abandono` | % de clientes que abandona el canal, sin resolver | 🔴 Pendiente |


---
* Tabla origen `adl_sandbox.nriosm.conversation_session_history`

| Indicador | Descripción | Área |
|------------|------------|------------|
| `% Reclamos`| Índice Neto de Satisfacción | CX |
| `%Emociones`| Índice Neto de Satisfacción | CX |


* Tabla origen `adl_sandbox.nriosm.adl_sandbox.nriosm.balancer_sessions`

| Indicador | Descripción | Área |
|------------|------------|------------|
| ``|  | CX |
| ``|  | CX |

---

## 📋 Indicadores


``` dax
%Q EPA ISN

# %Q EPA ISN = 
DIVIDE(
    CALCULATE(COUNT(onemarketer_encuesta_data_cruda[Id]) , FILTER(
        onemarketer_encuesta_data_cruda, lower(onemarketer_encuesta_data_cruda[Resutl_Eval_IA])="satisfecho"
    ))
    -
    CALCULATE(COUNT(onemarketer_encuesta_data_cruda[Id]) , FILTER(
        onemarketer_encuesta_data_cruda, lower(onemarketer_encuesta_data_cruda[Resutl_Eval_IA])="insatisfecho"
    ))
    ,CALCULATE(
        COUNT(onemarketer_encuesta_data_cruda[Id]) , FILTER(
            onemarketer_encuesta_data_cruda, onemarketer_encuesta_data_cruda[Resutl_Eval_IA]<>"Nulo")
        )
    ,0)
```


```dax
FCR / Tasa de Resolución

#% FCR = 
DIVIDE(
    CALCULATE(
        COUNT(onemarketer_encuesta_data_cruda[Id]) , FILTER(
            onemarketer_encuesta_data_cruda, LOWER(onemarketer_encuesta_data_cruda[Resutl_Tasa_de_resolucion])="resuelve"
        )
    ),
    CALCULATE(
        COUNT(onemarketer_encuesta_data_cruda[Id]) , FILTER(
            onemarketer_encuesta_data_cruda, onemarketer_encuesta_data_cruda[Resutl_Tasa_de_resolucion]<>"Nulo"
        )
    ),
0)
```

```dax
%Facilidad Acceso

#%Facilidad_Acceso = 
DIVIDE(
    CALCULATE(
        COUNT(onemarketer_encuesta_data_cruda[Id]) , FILTER(
            onemarketer_encuesta_data_cruda, LOWER(onemarketer_encuesta_data_cruda[Resutl_Facilidad]) ="facilidad"))
    -
    CALCULATE(
        COUNT(onemarketer_encuesta_data_cruda[Id]) , FILTER(
            onemarketer_encuesta_data_cruda, LOWER(onemarketer_encuesta_data_cruda[Resutl_Facilidad]) ="No facilidad"))
    ---- todo lo que no sea Nulo
    ,CALCULATE(
        COUNT(onemarketer_encuesta_data_cruda[Id]) , FILTER(
            onemarketer_encuesta_data_cruda, onemarketer_encuesta_data_cruda[Resutl_Facilidad]<>"Nulo"
        ))
    ,0)
```

```dax
Q interacciones IA

#Q interacciones IA =
CALCULATE(
    COUNT(onemarketer_encuesta_data_cruda[Id]),
    FILTER(
        onemarketer_encuesta_data_cruda,
        LOWER(onemarketer_encuesta_data_cruda[OperadorAbre]) = "robot"
    ),
    FILTER(
        onemarketer_encuesta_data_cruda,
        LOWER(onemarketer_encuesta_data_cruda[OperadorCierra]) = "robot"
    ),
    FILTER(
        onemarketer_encuesta_data_cruda,
        LOWER(onemarketer_encuesta_data_cruda[AtencionIA]) = "si"
    )
)
```

```dax
Q interacciones Ejecutivo

#Q interacciones Ejecutivo = 
CALCULATE(
    COUNT(onemarketer_encuesta_data_cruda[Id]),
    FILTER(
        onemarketer_encuesta_data_cruda,
        NOT (
            LOWER(onemarketer_encuesta_data_cruda[OperadorAbre])
                IN {"robot", "admin"}
        )
        &&
        NOT (
            LOWER(onemarketer_encuesta_data_cruda[OperadorCierra])
                IN {"robot", "admin"}
        )
        &&
        LOWER(onemarketer_encuesta_data_cruda[AtencionIA]) <> "si"
    )
)
```

Tasa de derivación - NOK: data cruda
Tasa de abandono   - NOK: conversaciones

FCR                - NOK: hay que conversarlo con el equipo de SAC
---
Cobertura Canal    - NOK: faltan accesos a tráfico web


## Tabla: adl_sandbox.nriosm.conversation_session_history
Reclamos           - NOK: En Proceso
Emociones          - NOK: falta revisar y adaptar el codigo


## Tabla: adl_sandbox.nriosm.adl_sandbox.nriosm.balancer_sessions

