import streamlit as st
import base64
import random
import os

# --- 1. CONFIGURACIÓN ---
NUM_PREGUNTAS_TECNICAS = 12
NUM_PREGUNTAS_BLANDAS = 13
PUNTAJE_MINIMO_BLANDAS = 0.75  # 75%

# ---------------- RSA SIMULADO ---------------------
def generar_clave_rsa_simulada():
    return base64.b64encode(os.urandom(16)).decode("utf-8")

def cifrar_rsa_simulado(datos, clave_publica):
    datos_codificados = base64.b64encode(datos.encode("utf-8")).decode("utf-8")
    return f"RSA-CIPHER:{clave_publica[:10]}...{datos_codificados}"

# --- 2. PREGUNTAS ---
PREGUNTAS_TECNICAS = [
    "¿Sabe realizar inventarios físicos y conteos rápidos de material de construcción?",
    "¿Ha utilizado algún software de Punto de Venta (POS) o sistema de facturación electrónica?",
    "¿Sabe leer e interpretar una hoja de ruta o usar GPS para optimizar entregas?",
    "¿Tiene experiencia o conoce el manejo básico de un montacargas o patín hidráulico?",
    "¿Puede identificar y diferenciar los tipos de cemento, varilla y arena comunes en la construcción?",
    "¿Sabe cómo se calcula el volumen o área de un material (ej. m³ de arena, m² de loseta)?",
    "¿Se siente cómodo manejando una terminal bancaria y realizando cobros con tarjeta?",
    "¿Puede verificar y cotejar una orden de compra contra el material físico que se va a cargar?",
    "¿Conoce las normas básicas de seguridad para cargar y descargar materiales pesados?",
    "¿Tiene conocimientos básicos de uso de hojas de cálculo (Excel, Google Sheets) para reportes?",
    "¿Sabe realizar el mantenimiento básico o limpieza de herramientas y equipos de trabajo?",
    "¿Puede calcular rápidamente el cambio para un pago en efectivo?"
]

PREGUNTAS_BLANDAS = [
    "¿Mantiene la calma y es cortés al tratar con un cliente enojado o impaciente?",
    "¿Se adapta fácilmente a los cambios de tareas o prioridades de última hora?",
    "¿Ofrece ayuda a sus compañeros sin que se lo pidan?",
    "¿Es puntual y respeta estrictamente los horarios?",
    "¿Acepta retroalimentación sin ponerse a la defensiva?",
    "¿Se enfoca en soluciones en lugar de culpar a otros?",
    "¿Es proactivo buscando tareas pendientes?",
    "¿Se comunica de manera clara y concisa?",
    "¿Trabaja bien bajo presión?",
    "¿Su honestidad y ética son inquebrantables?",
    "¿Prioriza la seguridad ante todo?",
    "¿Propone mejoras a procesos ineficientes?",
    "¿Se siente cómodo pidiendo al cliente verificar la carga/ticket?"
]

# --- ASIGNAR ÁREA ---
def asignar_area(respuestas):
    AREAS = {
        "CARGADOR":       [0, 3, 4, 7, 8],
        "VENTAS/COBRANZA":[1, 6, 11],
        "REPARTIDOR":     [2, 3, 7, 8],
        "LIMPIEZA":       [10],
        "ADMINISTRATIVO": [9]
    }

    conteo = {area: 0 for area in AREAS}

    for area, indices in AREAS.items():
        for idx in indices:
            if respuestas[idx] == "S":
                conteo[area] += 1

    return max(conteo, key=conteo.get)

# --- 3. APP WEB ---
st.title("PROCESO DE SELECCIÓN - BRICOS")
st.write("Completa el formulario para evaluar tus habilidades.")

# --- DATOS PERSONALES ---
st.subheader("Datos personales")

nombre = st.text_input("Nombre completo")
edad = st.text_input("Edad")
sexo = st.selectbox("Sexo", ["M", "F", "Otro"])
correo = st.text_input("Correo electrónico")
telefono = st.text_input("Teléfono")

# Cuestionarios
st.subheader("Habilidades Técnicas")
respuestas_tecnicas = []
for i, p in enumerate(PREGUNTAS_TECNICAS):
    r = st.radio(f"{i+1}. {p}", ["S", "N"], index=1, horizontal=True)
    respuestas_tecnicas.append(r)

pt_tecnico = respuestas_tecnicas.count("S")

st.subheader("Habilidades Blandas")
respuestas_blandas = []
for i, p in enumerate(PREGUNTAS_BLANDAS):
    r = st.radio(f"{i+1}. {p}", ["S", "N"], index=1, horizontal=True)
    respuestas_blandas.append(r)

pt_blando = respuestas_blandas.count("S")

# --- BOTÓN EVALUACIÓN ---
if st.button("Evaluar candidato"):
    if not nombre or not edad or not correo or not telefono:
        st.error("Por favor llena todos los datos personales.")
    else:
        # Cifrado simulado
        clave_publica = generar_clave_rsa_simulada()
        datos_cifrados = cifrar_rsa_simulado(
            f"Email:{correo}|Tel:{telefono}",
            clave_publica
        )

        # Calcular porcentajes
        porcentaje_tecnico = (pt_tecnico / NUM_PREGUNTAS_TECNICAS) * 100
        porcentaje_blando = (pt_blando / NUM_PREGUNTAS_BLANDAS) * 100

        # Veredicto
        es_aceptado = porcentaje_blando >= (PUNTAJE_MINIMO_BLANDAS * 100)

        # Área asignada
        area = asignar_area(respuestas_tecnicas)

        # Resultados
        st.subheader("RESULTADO FINAL")
        st.write(f"**Habilidades Técnicas:** {pt_tecnico}/{NUM_PREGUNTAS_TECNICAS} ({porcentaje_tecnico:.2f}%)")
        st.write(f"**Habilidades Blandas:** {pt_blando}/{NUM_PREGUNTAS_BLANDAS} ({porcentaje_blando:.2f}%)")
        st.write("---")

        if es_aceptado:
            st.success("ESTADO FINAL: ¡ACEPTADO! Cumple el 75% en habilidades blandas.")
        else:
            st.error("ESTADO FINAL: NO ACEPTADO. No cumple el 75% en habilidades blandas.")

        st.info(f"Área recomendada según habilidades técnicas: **{area}**")

        # Datos cifrados
        st.write("### Datos cifrados")
        st.code(datos_cifrados)
        
        st.write("---")
        st.write(f"Registro generado para: **{nombre}**, Edad: {edad}")
