# isma_uom_extractor.py – Extract UoMs only
import requests, streamlit as st

# ───────── helpers ───────────────────────────────────────────────────────────
def call_api(url, payload, err_msg):
    try:
        r = requests.post(url, json=payload, timeout=90)
        r.raise_for_status()
        return r.json()
    except Exception as e:
        st.error(f"{err_msg}: {e}")
        st.stop()

# ───────── API ENDPOINT ──────────────────────────────────────────────────────
API_EXTRACT = st.secrets.get("isma_api", {}).get(
    "extract", "https://isma-extract-uom-854321931145.europe-west1.run.app"
)

# ───────── UI SET-UP ─────────────────────────────────────────────────────────
st.set_page_config(page_title="ISMA UoM Extractor", layout="wide")
st.title("✨ ISMA Talent UoM Extractor")
st.subheader("1️⃣  Extract Units of Meaning")

SAMPLE = "Quentin est un explorateur d'idées permanent…"
if st.button("Load sample letter"):
    st.session_state["letter"] = SAMPLE

with st.form("extract_letter"):
    letter = st.text_area(
        "Paste the letter / description",
        value=st.session_state.get("letter", ""),
        height=180,
        placeholder="Layla is a highly motivated strategist…",
    )
    if st.form_submit_button("🚀 Extract UoMs") and letter.strip():
        with st.spinner("Extracting UoMs…"):
            data = call_api(API_EXTRACT, {"text": letter}, "Cannot extract UoMs")
        uoms = [
            u.strip()
            for u in data.get("units_of_meaning", "").replace("\n", ",").split(",")
            if u.strip()
        ]
        if uoms:
            st.success("✅ Units of Meaning Extracted:")
            st.write(", ".join(uoms))
        else:
            st.warning("No Units of Meaning found.")
