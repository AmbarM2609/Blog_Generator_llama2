# Blog Generation with LLama 2
Welcome to the Blog Generation app powered by LLama 2. This Streamlit application helps you generate blog posts tailored to specific audiences using the advanced LLama 2 language model. 

## Features
- **User Input**: Easily enter your desired blog topic and customize the word count.
- **Audience Selection**: Choose from different audience profiles, such as Researchers, Data Scientists, and Common People.
- **Real-time Blog Generation**: Get a blog generated on the fly based on your input and preferences.

## Requirements
To run this application, ensure you have the following dependencies installed:
- Streamlit
- Langchain
- CTransformers

You can install these dependencies using the following commands:
```bash
pip install streamlit langchain ctransformers
```

## Usage
1. **Setup**: Clone this repository and navigate to the project directory.
2. **Run the Application**: Execute the following command to start the Streamlit app:
   ```bash
   streamlit run app.py
   ```
3. **Interact with the App**: Open the provided URL in your web browser. Enter your blog topic, specify the word count, select the target audience, and click on the "Generate" button to get your blog post.

## Code Overview
The core functionality is defined in the `app.py` file:

### Importing Libraries
```python
import streamlit as st
from langchain.prompts import PromptTemplate
from langchain.llms import CTransformers
```

### Blog Generation Function
The `getLLamaresponse` function takes user inputs and generates a blog post using the LLama 2 model.
```python
def getLLamaresponse(input_text, no_words, blog_style):
    llm = CTransformers(model='models/llama-2-7b-chat.ggmlv3.q8_0.bin',
                        model_type='llama',
                        config={'max_new_tokens': 256, 'temperature': 0.01})

    template = """
        Write a blog for {blog_style} job profile for a topic {input_text}
        within {no_words} words.
            """
    prompt = PromptTemplate(input_variables=["blog_style", "input_text", 'no_words'], template=template)
    response = llm(prompt.format(blog_style=blog_style, input_text=input_text, no_words=no_words))
    print(response)
    return response
```

### Streamlit App Layout
The Streamlit app is configured to have a simple and user-friendly interface.
```python
st.set_page_config(page_title="Generate Blogs",
                   page_icon='🤖',
                   layout='centered',
                   initial_sidebar_state='collapsed')

st.header("Generate Blogs 🤖")

input_text = st.text_input("Enter the Blog Topic")

col1, col2 = st.columns([5, 5])
with col1:
    no_words = st.text_input('No of Words')
with col2:
    blog_style = st.selectbox('Writing the blog for', ('Researchers', 'Data Scientist', 'Common People'), index=0)

submit = st.button("Generate")

if submit:
    st.write(getLLamaresponse(input_text, no_words, blog_style))
```
