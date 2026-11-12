import streamlit as st
import pydeck as pdk

# 1. Page Config for Mobile Viewports
st.set_page_config(
    page_title="SELA Sentinel | Mobile View",
    layout="wide",
    initial_sidebar_state="collapsed"
)

# 2. Inject Mobile CSS Fixes (Removes Streamlit header/footer whitespace for true 100dvh map)
st.markdown("""
    <style>
        #MainMenu {visibility: hidden;}
        footer {visibility: hidden;}
        header {visibility: hidden;}
        .block-container {
            padding: 0 !important;
            margin: 0 !important;
            max-width: 100% !important;
        }
        iframe {
            height: 100vh !important;
        }
    </style>
""", unsafe_allow_html=True)

# 3. Top Floating Status & Language Toggle
st.markdown("""
    <div style="position: fixed; top: 12px; left: 12px; right: 12px; z-index: 999; display: flex; justify-content: space-between; align-items: pointer; pointer-events: none;">
        <div style="pointer-events: auto; background: rgba(15, 23, 42, 0.9); backdrop-filter: blur(8px); border: 1px solid rgba(51, 65, 85, 0.8); padding: 8px 14px; border-radius: 9999px; display: flex; align-items: center; gap: 8px; color: white; font-size: 12px; font-weight: 600;">
            <span style="width: 8px; height: 8px; background: #34d399; border-radius: 50%;"></span>
            SELA Sentinel • Live
        </div>
    </div>
""", unsafe_allow_html=True)

# 4. PyDeck Map Layer for SELA Air Quality Sensors
# (Centering around South Gate / SELA corridor)
view_state = pdk.ViewState(
    latitude=33.9543,
    longitude=-118.2120,
    zoom=12,
    pitch=0
)

# Dummy point data representing OpenAQ / PurpleAir feeds
sensor_data = [
    {"name": "South Gate Central", "coordinates": [-118.2120, 33.9543], "pm25": 14.2, "jurisdiction": "South Gate"},
    {"name": "Huntington Park Industrial", "coordinates": [-118.2250, 33.9800], "pm25": 18.5, "jurisdiction": "Huntington Park"},
    {"name": "Cudahy Corridor", "coordinates": [-118.1850, 33.9650], "pm25": 11.0, "jurisdiction": "Cudahy"}
]

layer = pdk.Layer(
    "ScatterplotLayer",
    data=sensor_data,
    get_position="coordinates",
    get_radius=300,
    get_fill_color="[239, 68, 68, 180]",
    pickable=True,
    auto_highlight=True
)

r = pdk.Deck(
    layers=[layer],
    initial_view_state=view_state,
    tooltip={"text": "Station: {name}\nPM2.5: {pm25} µg/m³\nJurisdiction: {jurisdiction}"}
)

# Render map filling the mobile viewport
st.pydeck_chart(r, use_container_width=True, height=650)

# 5. Filter & Export Fragment (Prevents full-page reloading on mobile taps)
@st.fragment
def render_mobile_bottom_drawer():
    with st.expander("📊 SELA Filters & CARB Export", expanded=False):
        selected_jurisdiction = st.selectbox(
            "Select Jurisdiction",
            ["All SELA", "South Gate", "Huntington Park", "Cudahy", "Bell Gardens", "Maywood"]
        )
        selected_pollutant = st.selectbox(
            "Pollutant Layer",
            ["PM2.5", "PM10", "VOCs / Benzene", "NO₂ / Diesel"]
        )
        
        if st.button("📥 Export Filtered View (CARB / AQview CSV)"):
            st.success(f"Generated export for {selected_jurisdiction} ({selected_pollutant}) with regulatory disclaimer attached.")

render_mobile_bottom_drawer()
