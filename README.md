import streamlit as st
import pandas as pd
from backend import read_campaigns, get_platforms, get_business_insights

def display_campaigns():
    """Displays the list of campaigns with filtering and sorting."""
    st.header("Campaigns List")
    platforms = get_platforms()
    selected_platform = st.selectbox("Filter by Platform", platforms)
    
    sort_options = ["Cost", "Clicks", "Conversions"]
    sort_by = st.radio("Sort by", sort_options)

    campaigns = read_campaigns(platform_filter=selected_platform, sort_by=sort_by)
    if campaigns:
        df = pd.DataFrame(campaigns, columns=['ID', 'Name', 'Platform', 'Start Date', 'Cost', 'Clicks', 'Conversions'])
        st.dataframe(df)
    else:
        st.info("No campaigns to display.")

def display_business_insights():
    """Displays key business metrics."""
    st.header("📈 Business Insights")
    insights = get_business_insights()
    
    col1, col2, col3 = st.columns(3)
    
    with col1:
        st.metric(label="Total Campaigns", value=insights.get("total_campaigns", 0))
    with col2:
        st.metric(label="Total Cost", value=f"${insights.get('total_cost', 0.0):.2f}")
    with col3:
        st.metric(label="Average Clicks", value=f"{insights.get('avg_clicks', 0.0):.2f}")
        
    st.markdown(f"**Cost Per Click (CPC):** ${insights.get('cost_per_click', 0.0):.2f}")

def main():
    """Main function to run the Streamlit app."""
    st.set_page_config(layout="wide", page_title="Digital Ad Campaign Tracker")
    
    st.title("Digital Ad Campaign Tracker")
    
    st.sidebar.title("App Navigation")
    
    display_business_insights()
    
    st.markdown("---")
    
    display_campaigns()

if __name__ == "__main__":
    main()
