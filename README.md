# AIML-INTERNSHIP-PROJECT
AIML Internship AI Counsellor project
import streamlit as st
import pandas as pd
import nltk
from nltk.tokenize import word_tokenize
from nltk.corpus import stopwords

nltk.download('punkt')
nltk.download('punkt_tab')
nltk.download('stopwords')

# Load Dataset
data = pd.read_csv(r"C:\Users\OMSAI\OneDrive\Desktop\AIML\archive (5)\career_recommender.csv")

st.set_page_config(page_title="AI Virtual Career Counsellor")

st.title("AI Virtual Career Counsellor")

st.write("Enter your interests and get career recommendations.")

user_input = st.text_input("Enter Your Interests")

def preprocess(text):
    text = text.lower()

    words = word_tokenize(text)

    stop_words = set(stopwords.words('english'))

    filtered_words = []

    for word in words:
        if word not in stop_words:
            filtered_words.append(word)

    return filtered_words

if st.button("Get Recommendation"):

    if user_input == "":
        st.warning("Please enter your interests")
    else:

        tokens = preprocess(user_input)

        found = False

        for token in tokens:

            matched = data[data["Interest"].str.lower().str.contains(user_input.lower(), na=False)]

            if not matched.empty:

                found = True

                for _, row in matched.iterrows():

                    st.success(
                        f"Recommended Career: {row['Career']}"
                    )

                    st.write(
                        f"Skills Required: {row['Skills']}"
                    )

                    st.write(
                        f"Salary Range: {row['Salary']}"
                    )

        if not found:
            st.error(
                "No suitable career found. Try different interests."
            )
