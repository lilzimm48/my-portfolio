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

import requests 
import pandas as pd
import tkinter
from tkinter import ttk
from tkinter import \*

class EntryV:
&nbsp;   def \_\_init\_\_(self, master, row, column, name):
&nbsp;       self.master = master
&nbsp;       self.name = name
&nbsp;       self.v = StringVar()
&nbsp;       self.e = ttk.Entry(self.master, textvariable=self.v, width=50)
&nbsp;       self.e.grid(row=row, column=column, sticky='w', columnspan=2, padx=(15,0), pady=(0,10))

&nbsp;   def \_\_str\_\_(self):
&nbsp;       return self.name


root = Tk()

urls = {
&nbsp;   "google": "https://suggestqueries.google.com/complete/search?client=chrome\&q=",
&nbsp;   "youtube": "http://suggestqueries.google.com/complete/search?client=firefox\&ds=yt\&q=",
}


l3 = Label(root, text="Query:")
l3.grid(row=0, column=1, sticky='w', padx=(15,0), pady=(15,5))
e1 = EntryV(root, 1, 1, "e1")


def submit():
&nbsp;   lb1.delete(0, END)
&nbsp;   lb2.delete(0, END)
&nbsp;   queries = \[e1.v.get()]

&nbsp;   to\_be\_saved\_queries = {}
&nbsp;   for query in queries:
&nbsp;       for (domain, url) in urls.items():
&nbsp;           # add the query to the url
&nbsp;           remote\_url = url + query
&nbsp;           response = requests.get(remote\_url).json()
&nbsp;           auto\_suggest = response\[1]
&nbsp;           for suggestion in auto\_suggest:
&nbsp;               to\_be\_saved\_queries\[suggestion] = domain


&nbsp;   for key, value in to\_be\_saved\_queries.items():
&nbsp;       if value == "google":
&nbsp;           lb1.insert(END, key)
&nbsp;       else:
&nbsp;           lb2.insert(END, key)


def submitE(var, index, mode):
&nbsp;   lb1.delete(0, END)
&nbsp;   lb2.delete(0, END)
&nbsp;   queries = \[e1.v.get()]


&nbsp;   to\_be\_saved\_queries = {}
&nbsp;   for query in queries:
&nbsp;       for (domain, url) in urls.items():
&nbsp;           # add the query to the url
&nbsp;           remote\_url = url + query
&nbsp;           response = requests.get(remote\_url).json()
&nbsp;           auto\_suggest = response\[1]
&nbsp;           for suggestion in auto\_suggest:
&nbsp;               to\_be\_saved\_queries\[suggestion] = domain


&nbsp;   for key, value in to\_be\_saved\_queries.items():
&nbsp;       if value == "google":
&nbsp;           lb1.insert(END, key)
&nbsp;       else:
&nbsp;           lb2.insert(END, key)


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


e1.v.trace\_add("write", submitE)
root.bind("<Enter>", submitE)
e1.e.focus\_set()


root.mainloop()
