# gpt-beta-pdf-in-desktop-chatgpt-app
this is basically code that can integrate pdf in chunks to feed in chatgpt desktop app for use.basically an add-on,but this is terribly done so don't trust this lol.not for use as its still in progress. COURTESY OF CHATGPT ITSELF.IT ASSISTED ME WITH THIS,SO TAKE IT WITH A GRAIN OF SALT,THIS IS MOSTLY AN EXPERIMENT OF SORTS.ALSO ONCE I PROGRESS,AND IF I PROGRESS,THEN I WILL UPDATE,BUT ALSO EMPLOY A DIFFERENT MORE CATCHY NAME.FINGERS CROSSED. UPDATE SOON AFTER TESTING,MODIFYIN,WORKING AROUND IT. in the meanwhile this is how one does the function of making gpt application with gpt app to read and reference pdf file ain chunks all in one,then more steps after.CHATGPT SENDS ITS REGARDS. TATA :)

a script that combines everything together into a single file:-


# Import required packages

import os

from dotenv import load_dotenv

from PyPDF2 import PdfReader

from langchain.embeddings.openai import OpenAIEmbeddings

from langchain.text_splitter import CharacterTextSplitter

from langchain.vectorstores import FAISS

from langchain.llms import OpenAI

from langchain.chains.question_answering import load_qa_chain

# Load environment variables
load_dotenv()


# Download and extract text from PDF

pdf_url = os.getenv('PDF_URL')

os.system(f'wget -O ipc.pdf {pdf_url}')

with open('ipc.pdf', 'rb') as f:

    reader = PdfReader(f)
    
    raw_text = ''
    
    for i, page in enumerate(reader.pages):
    
        text = page.extract_text()
        
        if text:
        
            raw_text += text
            

# Split text into smaller chunks

text_splitter = CharacterTextSplitter(

    separator="\n",
    
    chunk_size=1000,
    
    chunk_overlap=200,
    
    length_function=len,
  
)
texts = text_splitter.split_text(raw_text)


# Generate embeddings for chunks

embeddings = OpenAIEmbeddings()

docsearch = FAISS.from_texts(texts, embeddings)

# Load language model and question answering chain

model = OpenAI(temperature=0.5)

chain = load_qa_chain(model, chain_type='stuff')

# Answer user's questions

while True:

    question = input('Ask a question (or type "exit" to quit): ')
    
    if question.lower() == 'exit':
    
        break
        
    docs = docsearch.similarity_search(question)
    
    chain.run(input_documents=docs, question=question)


This script downloads the IPC PDF specified by the PDF_URL environment variable, extracts text from the PDF, splits the text into smaller chunks, generates embeddings for the chunks, loads a language model and a question answering chain, and prompts the user to ask a question. The script then searches the chunks for the answer to the user's question and prints the answer. The user can exit the script by typing "exit".

you can make a repository with this script alone. Once you have created a repository, you can clone it to your local machine using Git. You can then run the script by navigating to the directory where it is located and running the following command:


python main.py

This will prompt you to enter a question, and the script will use the IPC PDF to try and find an answer to your question. Make sure you have the necessary packages installed and have created the .env file with the PDF_URL variable set to the URL of the IPC PDF.

The .env file is a configuration file that holds your environment variables. In the case of the code we wrote earlier, we used it to store the PDF URL that the script needs to access.

When you create a .env file in the same directory as the script, the variables stored in it can be accessed by the script. In our case, we used the os module to read the PDF_URL variable from the .env file, like this:

import os

PDF_URL = os.getenv('PDF_URL')


This allows you to easily change the PDF URL without modifying the script itself. Instead, you can simply update the .env file with the new URL.
