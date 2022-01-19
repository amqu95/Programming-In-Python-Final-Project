# Programming-In-Python-Final-Project
#Create a GUI program that stores a user's preferences for a program

import tkinter as tk
from tkinter import ttk
import FinalProject_db_AmandaQuach as db

class PreferencesFrame(ttk.Frame):
    def __init__(self, parent):
        ttk.Frame.__init__(self,parent, padding = "10 10 10")
        self.parent = parent
        self.pack(fill=tk.BOTH, expand=True)

        self.nameDb = tk.StringVar()
        self.languageDb = tk.StringVar()
        self.autoSaveDb = tk.StringVar()
        self.nameError = tk.StringVar()
        self.languageError = tk.StringVar()
        self.autoSaveError = tk.StringVar()

        ttk.Label(self, text="Name: ").grid(column=0, row=0, sticky=tk.E)
        ttk.Entry(self, width=25, textvariable=self.nameDb).grid(column=1, row=0)
        ttk.Label(self, text=" ",textvariable=self.nameError).grid(column=3, row=0, sticky=tk.E)
        
        ttk.Label(self, text="Language: ").grid(column=0, row=1, sticky=tk.E)
        ttk.Entry(self, width=25, textvariable=self.languageDb).grid(column=1, row=1)
        ttk.Label(self, text=" ",textvariable=self.languageError).grid(column=3, row=1, sticky=tk.E)


        ttk.Label(self, text="Auto Save Evey X Minutes: ").grid(column=0, row=2, sticky=tk.E)
        ttk.Entry(self, width=25, textvariable=self.autoSaveDb).grid(column=1, row=2)
        ttk.Label(self, text=" ",textvariable=self.autoSaveError).grid(column=3, row=2, sticky=tk.E)

        self.makeButtons()
        for child in self.winfo_children():
            child.grid_configure(padx=5, pady=3)
            
        db.get_preferences(self.nameDb, self.languageDb, self.autoSaveDb)

    def makeButtons(self):
        buttonFrame = ttk.Frame(self)
        buttonFrame.grid(column=1, row=3, columnspan=2, sticky=tk.E)

        ttk.Button(buttonFrame, text="Save",command=self.save_txt).grid(column=1, row=3, sticky=tk.E)
        ttk.Button(buttonFrame, text="Cancel",command=self.parent.destroy).grid(column=2, row=3, sticky=tk.E)

    def save_txt(self):
        db.save_file(self.nameDb,self.languageDb, self.autoSaveDb, self.nameError, self.languageError, self.autoSaveError)
        return self.nameDb,self.languageDb, self.autoSaveDb, self.nameError, self.languageError, self.autoSaveError

if __name__ == "__main__":
    root = tk.Tk()
    root.title("Prefernces")
    PreferencesFrame(root)
    root.mainloop()
