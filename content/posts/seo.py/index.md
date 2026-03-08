---
title: "seo.py"
date: 2024-09-29
description: "a script i wrote to show trending google searches building on custom queries"
featureImage: "seo-screenshot.png"
categories: 
  - "scripts"
tags: 
  - "python"
  - "automation"
  - "scripts"
draft: false
---

a script i wrote to show trending google searches building on custom queries. it generates the most accurate search based off of what you have put in to the program. definitely not fully fleshed out.



you can download the file here: {{< download src="/scripts/seo.py" label="seo.py" >}}



\### the script

Below is the core logic for the file organizer:



```python
import requests  # Include functionality to make a call to the remote websites
import pandas as pd
import tkinter
from tkinter import ttk
from tkinter import *

class EntryV:
    def __init__(self, master, row, column, name):
        self.master = master
        self.name = name
        self.v = StringVar()
        self.e = ttk.Entry(self.master, textvariable=self.v, width=50)
        self.e.grid(row=row, column=column, sticky='w', columnspan=2, padx=(15,0), pady=(0,10))

    def __str__(self):
        return self.name


root = Tk()


urls = {
    "google": "https://suggestqueries.google.com/complete/search?client=chrome&q=",
    "youtube": "http://suggestqueries.google.com/complete/search?client=firefox&ds=yt&q=",
}


l3 = Label(root, text="Query:")
l3.grid(row=0, column=1, sticky='w', padx=(15,0), pady=(15,5))
e1 = EntryV(root, 1, 1, "e1")


def submit():
    lb1.delete(0, END)
    lb2.delete(0, END)
    queries = [e1.v.get()]

    to_be_saved_queries = {}
    for query in queries:
        for (domain, url) in urls.items():
            # add the query to the url
            remote_url = url + query
            response = requests.get(remote_url).json()
            auto_suggest = response[1]
            for suggestion in auto_suggest:
                to_be_saved_queries[suggestion] = domain


    for key, value in to_be_saved_queries.items():
        if value == "google":
            lb1.insert(END, key)
        else:
            lb2.insert(END, key)


def submitE(var, index, mode):
    lb1.delete(0, END)
    lb2.delete(0, END)
    queries = [e1.v.get()]

    to_be_saved_queries = {}
    for query in queries:
        for (domain, url) in urls.items():
            # add the query to the url
            remote_url = url + query
            response = requests.get(remote_url).json()
            auto_suggest = response[1]
            for suggestion in auto_suggest:
                to_be_saved_queries[suggestion] = domain


    for key, value in to_be_saved_queries.items():
        if value == "google":
            lb1.insert(END, key)
        else:
            lb2.insert(END, key)


b1 = ttk.Button(root, text="Submit", command=submit)
b1.grid(row=1, column=2, sticky='w', pady=(0,10))

l1 = Label(root, text="Google Autocomplete:")
l1.grid(row=3, column=1, sticky="w", padx=(15,0), pady=(0,5))
lb1 = Listbox(root, relief=FLAT, width=50, height=15, highlightbackground="grey80", highlightcolor="grey80", highlightthickness=1)
lb1.grid(row=4, column=1, sticky='we', padx=(15,5), pady=(0,15))

l2 = Label(root, text="Youtube Autocomplete:")
l2.grid(row=3, column=2, sticky="w", pady=(0,5))
lb2 = Listbox(root, relief=FLAT, width=50, height=15, highlightbackground="grey80", highlightcolor="grey80", highlightthickness=1)
lb2.grid(row=4, column=2, sticky='we', padx=(0,15), pady=(0,15))

e1.v.trace_add("write", submitE)
root.bind("<Enter>", submitE)
e1.e.focus_set()

root.mainloop()