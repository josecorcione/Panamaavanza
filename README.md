# Panamaavanza
import streamlit as st
import pandas as pd
from datetime import datetime
from enum import Enum
from typing import List, Dict, Optional
import plotly.graph_objects as go
import plotly.express as px

class NivelNegocio(Enum):
    EXPLORADOR_INICIAL = 1
    BUSCADOR_CRECIMIENTO = 2
    CONSTRUCTOR_ESTABILIDAD = 3
    LOGRADOR_FORMALIZACION = 4
    CAMPEON_NEGOCIOS = 5

def main():
    st.set_page_config(page_title="FinPath - Gestión Financiera", layout="wide")
    
    # Sidebar para navegación
    st.sidebar.title("FinPath")
    pagina = st.sidebar.selectbox(
        "Navegación",
        ["Inicio", "Transacciones", "Estados Financieros", "Alertas", "Progreso"]
    )
    
    # Inicializar estado de la sesión si no existe
    if 'transacciones' not in st.session_state:
        st.session_state.transacciones = []
    
    if pagina == "Inicio":
        mostrar_inicio()
    elif pagina == "Transacciones":
        mostrar_transacciones()
    elif pagina == "Estados Financieros":
        mostrar_estados_financieros()
    elif pagina == "Alertas":
        mostrar_alertas()
    elif pagina == "Progreso":
        mostrar_progreso()

def mostrar_inicio():
    st.title("Bienvenido a FinPath")
    
    col1, col2 = st.columns(2)
    
    with col1:
        st.subheader("Resumen del Negocio")
        # Métricas clave
        col_metricas1, col_metricas2, col_metricas3 = st.columns(3)
        with col_metricas1:
            st.metric(label="Ingresos del Mes", value="B/. 2,500", delta="300")
        with col_metricas2:
            st.metric(label="Gastos del Mes", value="B/. 1,800", delta="-200")
        with col_metricas3:
            st.metric(label="Flujo de Caja", value="B/. 700", delta="500")
    
    with col2:
        st.subheader("Nivel Actual")
        nivel_actual = NivelNegocio.EXPLORADOR_INICIAL
        st.progress(0.2)
        st.write(f"Nivel: {nivel_actual.name}")
        st.write("Próximo hito: Completar registro de transacciones por 1 mes")

def mostrar_transacciones():
    st.title("Gestión de Transacciones")
    
    # Formulario para nueva transacción
    with st.form("nueva_transaccion"):
        st.subheader("Registrar Nueva Transacción")
        fecha = st.date_input("Fecha")
        descripcion = st.text_input("Descripción")
        monto = st.number_input("Monto (B/.)", min_value=0.0)
        tipo = st.selectbox("Tipo", ["Ingreso", "Gasto"])
        categoria = st.selectbox(
            "Categoría",
            ["Ventas", "Servicios", "Inventario", "Alquiler", "Servicios Públicos", "Otros"]
        )
        foto_recibo = st.file_uploader("Foto del Recibo (opcional)", type=['png', 'jpg'])
        
        if st.form_submit_button("Guardar Transacción"):
            nueva_transaccion = {
                "fecha": fecha,
                "descripcion": descripcion,
                "monto": monto,
                "tipo": tipo,
                "categoria": categoria
            }
            st.session_state.transacciones.append(nueva_transaccion)
            st.success("¡Transacción registrada exitosamente!")
    
    # Mostrar historial de transacciones
    if st.session_state.transacciones:
        st.subheader("Historial de Transacciones")
        df = pd.DataFrame(st.session_state.transacciones)
        st.dataframe(df)

def mostrar_estados_financieros():
    st.title("Estados Financieros")
    
    # Tabs para diferentes estados financieros
    tab1, tab2, tab3 = st.tabs(["Estado de Resultados", "Balance General", "Flujo de Caja"])
    
    with tab1:
        st.subheader("Estado de Resultados")
        # Aquí iría la lógica para generar el estado de resultados
        st.write("Estado de resultados en desarrollo...")
    
    with tab2:
        st.subheader("Balance General")
        st.write("Balance general en desarrollo...")
    
    with tab3:
        st.subheader("Flujo de Caja")
        st.write("Flujo de caja en desarrollo...")

def mostrar_alertas():
    st.title("Sistema de Alertas")
    
    # Ejemplo de alertas
    st.error("⚠️ Alerta: El flujo de caja está por debajo del mínimo recomendado")
    st.warning("⚠️ Advertencia: Inventario bajo en productos principales")
    st.info("ℹ️ Recordatorio: Fecha límite de declaración de impuestos próxima")

def mostrar_progreso():
    st.title("Progreso de Formalización")
    
    # Mostrar progreso general
    st.subheader("Progreso General")
    progreso = 0.35
    st.progress(progreso)
    st.write(f"Progreso total: {int(progreso * 100)}%")
    
    # Mostrar logros
    st.subheader("Logros Desbloqueados")
    col1, col2 = st.columns(2)
    
    with col1:
        st.success("✅ Registro básico completado")
        st.success("✅ Primera semana de transacciones registradas")
    
    with col2:
        st.error("❌ Completar registro fiscal")
        st.error("❌ Obtener licencia comercial")

if __name__ == "__main__":
    main()
