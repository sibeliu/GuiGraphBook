# VERSION 2.9

# input pane for inputting long texts 
import pandas as pd
from tkinter import *
import tkinter as tk
from tkinter import ttk
import spacy
import time
from py2neo import Graph
import tkinter.font as font
import json
import concurrent.futures

global sensitivity 
sensitivity = .9  #this is the similarity (dotproduct) minimum value for a SIMILAR relation to be created

#       Authentication for neo4j
uri = "bolt://localhost:7687"
user = "neo4j"
psw = "GuiGraphBook"
graph = Graph(uri, auth=(user, psw))

nlp = spacy.load("en_core_web_md")
    
root = Tk() #initialize GUI
root.geometry('950x650')

def create_agreement_relation(source_id, target_id, username, score):  #save a judgment of agreement with username and agreement score
    
    if source_id != target_id:   
        cypher_query = ""
        cypher_query = """
                    MATCH (t),  (s) \n
                    WHERE ID(t)=""" + str(target_id) + """ AND ID(s)=""" + str(source_id) + """\n
                    MERGE (s)-[:AGREES_WITH {username: '""" + str(username) + """', score: '""" + str(score) + """'}]->(t) \n
                """
        graph.run(cypher_query)
    return     

# this routine calculates similarity df based on user input, then creates SIMILAR relations in the graph db accordingly
def create_similar_relations(user_input, user_input_id, user_input_title):
    df = similarity_table(user_input)     

    for index, row in df.iterrows(): 
        target_id = int(row['id'])
        if user_input_id == target_id:  # make sure we don't make things similar to themselves... duh
            continue
        if user_input_title == row['title']:  #make sure we don't make similarity relations _within_ a given text
            continue
        if sensitivity > row['similarity']: #if we are not over the threshold value for sensitivity, do not create relation
            continue
        else:    
            cypher_query = ""
            cypher_query = """
                        MATCH (t),  (s) \n
                        WHERE ID(t)=""" + str(target_id) + """ AND ID(s)=""" + str(user_input_id) + """\n
                        MERGE (s)-[:SIMILAR {similarity: '""" + str(row['similarity']) + """'}]->(t) \n
                    """
            graph.run(cypher_query)
    return    


def similarity_table (sent1):   #pass a sentence, query all sentences in db, return id and similarity of top 5 phrases
    source = nlp(sent1)
    cypher_query = """
        MATCH(n:Sentence)
        RETURN ID(n) as id, n.title as title, n.author as author, n.content as content 
    """
    sentence_data = graph.run(cypher_query).data()

    a=[]
    b=[]
    c=[]
    
    for s in sentence_data:
        target = nlp(str(s['content']))
        if len (target) > 5:
            similarity_value = source.similarity(target) #using spacy dotproduct function
            a.append(similarity_value)
            b.append(s['id'])
            c.append(s['title'])
    
    similarity_table = {'similarity': a, 'id': b, 'title': c}
    
    l = pd.DataFrame(similarity_table, columns=['similarity', 'id', 'title'])
    l = l.nlargest(5,['similarity']) #return dataframe of five highest similarity scores
    
    return (l)


## query database to find all nodes with label :Sentence, return list of dicts containing full node info, and list of sentence content (not necessarily in order)
def get_sentences():
    cypher_query = """
        MATCH(n:Sentence)
        RETURN ID(n) as id, n.title as title, n.author as author, n.content as content 
    """
    sentence_data = graph.run(cypher_query).data()
        ## return list containing "content" property of all sentences
    content = []
    for s in sentence_data:
        content.append(s['content'])
    return sentence_data, content

def clean_text():
    #get text from input box, eliminate empty lines
    original_text = phrase.get("1.0", 'end-1c')
    text = "\n".join([ll.rstrip() for ll in original_text.splitlines() if ll.strip()])
    text = " ".join(text.split())
    text = text.replace('"', '/')
    return text
    

def save_text():

    #get text from input box and break into sentences
    text = clean_text()
    
    nlp_text = nlp(text)

    cypher_query = ""
    #loop through sentences and create nodes
    for index, value in enumerate(nlp_text.sents):  
        temp_name = "s"+str(index)
        content = str(value)
        author = ln_entry.get()
        title = title_entry.get()

        ## titles need to be unique, since they are used as the document key code
        cypher_query += 'CREATE ('+temp_name+':Sentence{author: "'+author+'", title: "'+title+'", content: "'+content+'"})  \n'

    # now create relations between sentences
    for index, value in enumerate(nlp_text.sents): 
        if index > 0:  #do nothing for the first node
            temp_name = "s" + str(index)
            temp_name2 = "s" + str(index-1)
            
            cypher_query += """
                        MERGE ("""+temp_name2+""")-[:PRECEDES]->("""+temp_name+""") \n
                    """
    graph.run(cypher_query) #now we run the first long query to input nodes and linear relations for a single text
    
    
    # now calculate similarity for each sentence and create up to 5 SIMILAR relations for each sentence
    title = title_entry.get()
    cypher_query=""
    cypher_query="""
                MATCH (n:Sentence{title: '""" + str(title) + """'}) \n
                RETURN ID(n) as id, n.title as title, n.author as author, n.content as content 
    """

    list_of_sentences_in_document = graph.run(cypher_query).data()

    for s in list_of_sentences_in_document:
        create_similar_relations(s['content'], s['id'], s['title']) 
        
    ## END OF STANDARD OPERATIONS FOR EVERY NEW TEXT 
    
    saved = "Saved information about " + ln_entry.get() + "'s text."
    myLabel = Label(root, text=saved)
    myLabel.pack()
    

#start laying out pane
new_phrase_frame = LabelFrame(root, text="New Text")
new_phrase_frame.pack(fill="x", expand="yes", padx=20)

new_label = Label(new_phrase_frame, text="New text to be archived")
new_label.grid(row=0, column=0, padx=10, pady=10)

phrase = Text(new_phrase_frame, height=30, width=80, padx=20, pady=20)
phrase.grid(row=0, column=1, padx=10, pady=10)

#insert information about the phrase
data_frame = LabelFrame(root, text="Author information")
data_frame.pack(fill="x", expand="yes", padx=20)
                
ln_label = Label(data_frame, text="Last Name")
ln_label.grid(row=0, column=0, padx=10, pady=10)
ln_entry = Entry(data_frame)
ln_entry.grid(row=0, column=1, padx=10, pady=10)

title_label = Label(data_frame, text="Title of the work")
title_label.grid(row=0, column=2, padx=10, pady=10)
title_entry = Entry(data_frame)
title_entry.grid(row=0, column=3, padx=10, pady=10)

#add buttons
button_frame = LabelFrame(root, text="Commands")
button_frame.pack(fill="x", expand="yes", padx=20)

save_text_button = Button(button_frame, text="Save", command=save_text)
save_text_button.grid(row=0, column=3, padx=10, pady=10)


root.mainloop()  #this is the GUI loop. Information can be input continuously.
## Once the window is closed by clicking red button, the program execution continues below
