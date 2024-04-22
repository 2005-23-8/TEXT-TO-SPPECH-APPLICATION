# TEXT-TO-SPPECH-APPLICATION
SPEECH APPLICATION
class TextToSpeechApp:
    def _init_(self, root):
        self.root = root
        self.root.title("Text to Speech Converter")
        
        self.text_label = ttk.Label(root, text="Enter text:")
        self.text_label.grid(row=0, column=0, padx=5, pady=5, sticky="w")
        
        self.text_entry = ttk.Entry(root, width=50)
        self.text_entry.grid(row=0, column=1, columnspan=3, padx=5, pady=5)
        
        self.language_label = ttk.Label(root, text="Select language:")
        self.language_label.grid(row=1, column=0, padx=5, pady=5, sticky="w")
        
        self.language_var = tk.StringVar()
        self.language_combobox = ttk.Combobox(root, width=20, textvariable=self.language_var)
        self.language_combobox['values'] = ('en', 'es', 'fr')  # Add more languages as needed
        self.language_combobox.grid(row=1, column=1, padx=5, pady=5)
        self.language_combobox.current(0)  # Default language
        
        self.play_button = ttk.Button(root, text="Play", command=self.play)
        self.play_button.grid(row=1, column=2, padx=5, pady=5)
        
        self.save_button = ttk.Button(root, text="Save", command=self.save)
        self.save_button.grid(row=1, column=3, padx=5, pady=5)
        
        self.feedback_label = ttk.Label(root, text="")
        self.feedback_label.grid(row=2, column=0, columnspan=4, padx=5, pady=5)
        
    def play(self):
        text = self.text_entry.get()
        language = self.language_var.get()
        try:
            tts = gTTS(text=text, lang=language)
            tts.save("output.mp3")
            os.system("start output.mp3")
            self.feedback_label.config(text="Speech generated successfully!")
        except Exception as e:
            self.feedback_label.config(text=f"Error: {str(e)}")
            
    def save(self):
        text = self.text_entry.get()
        language = self.language_var.get()
        try:
            tts = gTTS(text=text, lang=language)
            save_path = tk.filedialog.asksaveasfilename(defaultextension=".mp3", filetypes=[("MP3 files", ".mp3"), ("All files", ".*")])
            if save_path:
                tts.save(save_path)
                self.feedback_label.config(text="Speech saved successfully!")
            else:
                self.feedback_label.config(text="Save operation cancelled.")
        except Exception as e:
            self.feedback_label.config(text=f"Error: {str(e)}")

if _name_ == "_main_":
    root = tk.Tk()
    app = TextToSpeechApp(root)
    root.mainloop()
