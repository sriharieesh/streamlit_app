import streamlit as st
import pandas as pd
import numpy as np
import plotly.express as px

# --------------------------------------------------
# Page Config
# --------------------------------------------------
st.set_page_config(
    page_title="Advanced Streamlit UI",
    page_icon="🚀",
    layout="wide"
)

# --------------------------------------------------
# Custom CSS
# --------------------------------------------------
st.markdown("""
<style>
.kpi-card {
    background: linear-gradient(135deg, #1f1f1f, #2b2b2b);
    padding: 20px;
    border-radius: 16px;
    color: white;
    text-align: center;
}
.kpi-value {
    font-size: 32px;
    font-weight: bold;
}
.kpi-label {
    font-size: 14px;
    opacity: 0.8;
}
</style>
""", unsafe_allow_html=True)

# --------------------------------------------------
# Sidebar
# --------------------------------------------------
st.sidebar.title("⚙️ Control Panel")
page = st.sidebar.radio(
    "Navigation",
    ["Dashboard", "Data Upload", "Model Insights", "Settings"]
)

theme = st.sidebar.selectbox("Theme", ["Dark", "Light"])
refresh = st.sidebar.button("🔄 Refresh Data")

# --------------------------------------------------
# Dummy Data
# --------------------------------------------------
@st.cache_data
def load_data():
    df = pd.DataFrame({
        "Day": range(1, 31),
        "Users": np.random.randint(200, 1000, 30),
        "Revenue": np.random.randint(1000, 10000, 30)
    })
    return df

df = load_data()

# --------------------------------------------------
# Dashboard Page
# --------------------------------------------------
if page == "Dashboard":
    st.title("📊 Analytics Dashboard")

    col1, col2, col3 = st.columns(3)

    col1.markdown("""
    <div class="kpi-card">
        <div class="kpi-value">1,284</div>
        <div class="kpi-label">Active Users</div>
    </div>
    """, unsafe_allow_html=True)

    col2.markdown("""
    <div class="kpi-card">
        <div class="kpi-value">$42,500</div>
        <div class="kpi-label">Monthly Revenue</div>
    </div>
    """, unsafe_allow_html=True)

    col3.markdown("""
    <div class="kpi-card">
        <div class="kpi-value">3.8%</div>
        <div class="kpi-label">Conversion Rate</div>
    </div>
    """, unsafe_allow_html=True)

    st.markdown("### 📈 Trends")

    tab1, tab2 = st.tabs(["Users Growth", "Revenue Trend"])

    with tab1:
        fig = px.line(df, x="Day", y="Users", markers=True)
        st.plotly_chart(fig, use_container_width=True)

    with tab2:
        fig = px.bar(df, x="Day", y="Revenue")
        st.plotly_chart(fig, use_container_width=True)

# --------------------------------------------------
# Data Upload Page
# --------------------------------------------------
elif page == "Data Upload":
    st.title("📂 Upload Your Dataset")

    uploaded_file = st.file_uploader(
        "Upload CSV file",
        type=["csv"]
    )

    if uploaded_file:
        data = pd.read_csv(uploaded_file)
        st.success("File uploaded successfully!")
        st.dataframe(data, use_container_width=True)

        with st.expander("🔍 Data Summary"):
            st.write(data.describe())

# --------------------------------------------------
# Model Insights Page
# --------------------------------------------------
elif page == "Model Insights":
    st.title("🤖 AI Model Insights")

    st.info("This section can be connected to an ML / GenAI model")

    confidence = st.slider("Prediction Confidence Threshold", 0.0, 1.0, 0.7)

    st.metric(
        label="Model Accuracy",
        value="92.4%",
        delta="+1.2%"
    )

    st.markdown("### 🔬 Explainability")
    st.progress(80)

    with st.expander("🧠 Feature Importance"):
        st.bar_chart({
            "Feature A": 0.45,
            "Feature B": 0.32,
            "Feature C": 0.23
        })

# --------------------------------------------------
# Settings Page
# --------------------------------------------------
elif page == "Settings":
    st.title("⚙️ App Settings")

    col1, col2 = st.columns(2)

    with col1:
        st.checkbox("Enable Notifications", True)
        st.checkbox("Auto-refresh Dashboard")

    with col2:
        st.selectbox("Language", ["English", "Tamil", "Hindi"])
        st.selectbox("Model Version", ["v1.0", "v2.0", "Experimental"])

    st.button("💾 Save Settings")

# --------------------------------------------------
# Footer
# --------------------------------------------------
st.markdown("---")
st.caption("Built with ❤️ using Streamlit")
