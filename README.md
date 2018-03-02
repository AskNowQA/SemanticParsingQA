# Semantic Parsing Question Answering pipeline
## Description

This document aims to describe how to install the complete pipeline of our QA system.
The pipeline currently is composed of the following 3 components

* https://github.com/AskNowQA/AskNowUI
* https://github.com/AskNowQA/EARL/
* https://github.com/AskNowQA/SQG

The flow of control between the three components is the following

    User ----> AskNowUI ----> EARL ----> SQG
         <----          <----      <----                        
                                

**AskNowUI** is the web UI component of the project which is mainly comprised of HTML and javascript code.

**EARL** receives the text question from the UI and after a series of steps links phrases in the question to entity and reltion URIs in a knowledge base.

**SQG** receives a list of entities and relations from EARL and generates optimal SPARQL queries and returns them to EARL. EARL executes these SPARQL queries to generate a final answer and returns it to the web UI.


# SETUP INSTRUCTIONS

On a computer with 8 - 10 GB of available RAM

    # mkdir asknow
    # cd asknow/

## AskNowUI
    
    # git clone https://github.com/AskNowQA/AskNowUI.git
    # cd AskNowUI/
    # git checkout debayan
    # python app.py 5001

At this point the web UI can be viewed by opening localhost:5001 on a web browser on the same machine. However for it to be useful we need to setup the next 2 components.

## EARL

In a new shell

    # cd asknow/
    # git clone https://github.com/AskNowQA/EARL.git
    # cd EARL/
    # git checkout dev

Follow instructions at EARL github repository https://github.com/AskNowQA/EARL/ for installation. At the end of that process you will have EARL listening on port 4999. AskNowUI expects EARL to be at port 4999.

## SQG

In a new shell

    # cd asknow/
    # git clone https://github.com/AskNowQA/SQG.git
    # cd SQG/
    # python qg_webserver.py

Follow the instructions on SQG github repository closely. Install any dependencies required via pip. Ensure that on running the bloom file is loaded at run time and that the dbpedia endpoint is setup correctly. For this you might have to make 2 changes. Edit **kb/dbpedia.py** **\_\_init\_\_()** function and change the variable **endpoint** to a locally running dbpedia sparql endpoint. Change the variable **one_hop_bloom_file** to **./data/blooms/spo1.bloom**

This starts SQG at port 5000. We have completed the setup. You may now visit localhost:5001 on your web browser on the same machine and ask a question and see the three components replying.
