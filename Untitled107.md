```python
import tkinter as tk
from tkinter import messagebox, filedialog
import base64
from PIL import Image, ImageTk
import pygame
import os

pygame.init()

t = tk.Tk()
t.geometry('420x650')
t.title("Encryption and Decryption")
t.configure(bg="royal blue")

image_label = None
encoded_image_string = None
encoded_audio_string = None
encoded_video_string = None

def encrypt():
    password = code.get()
    if password == "1111":
        screen1 = tk.Toplevel(t)
        screen1.title('Encryption')
        screen1.geometry("400x250")
        screen1.configure(bg="white")
        
        message = aa.get(1.0, tk.END)
        encode_message = message.encode("ascii")
        base64_bytes = base64.b64encode(encode_message)  
        encrypt = base64_bytes.decode("ascii")
        
        ba = tk.Label(screen1, text="Code is encrypted", font="impack 10 bold")
        ba.place(x=5, y=6)
        bb = tk.Text(screen1, font="10", bd=4, wrap=tk.WORD)
        bb.place(x=2, y=30, width=390, height=180)
        bb.insert(tk.END, encrypt)
    elif password == "":
        messagebox.showerror("Error", "Please enter the secret key")
    else:
        messagebox.showerror("Error", "Invalid password")

def decrypt():
    password = code.get()
    if password == "1111":
        screen2 = tk.Toplevel(t)
        screen2.title('Decryption')
        screen2.geometry("400x250")
        screen2.configure(bg="white")
        
        message = aa.get(1.0, tk.END)
        try:
            decode_message = message.encode("ascii")
            base64_bytes = base64.b64decode(decode_message)  
            decrypt = base64_bytes.decode("ascii")
            
            bc = tk.Label(screen2, text="Code is decrypted", font="impack 10 bold")
            bc.place(x=5, y=6)
            bd = tk.Text(screen2, font="10", bd=4, wrap=tk.WORD)
            bd.place(x=2, y=30, width=390, height=180)
            bd.insert(tk.END, decrypt)
        except base64.binascii.Error:
            messagebox.showerror("Error", "Invalid base64 input")
    elif password == "":
        messagebox.showerror("Error", "Please enter the secret key")
    else:
        messagebox.showerror("Error", "Invalid password")


def encode_file(file_path):
    try:
        with open(file_path, "rb") as file:
            file_bytes = file.read()
            encoded_file = base64.b64encode(file_bytes).decode("ascii")
        return encoded_file
    except FileNotFoundError:
        messagebox.showerror("Error", "File not found!")
        return None

def decode_file(encoded_string, output_folder, output_filename):
    try:
        os.makedirs(output_folder, exist_ok=True)
        output_path = os.path.join(output_folder, output_filename)
        decoded_bytes = base64.b64decode(encoded_string)
        with open(output_path, "wb") as file:
            file.write(decoded_bytes)
        messagebox.showinfo("Success", f"Decoded file saved at: {output_path}")
        return output_path
    except Exception as e:
        messagebox.showerror("Error", f"Error decoding file: {e}")
        return None

def encrypt_image():
    global encoded_image_string
    password = code.get()
    if password == "1111":
        file_path = filedialog.askopenfilename(filetypes=[("Image Files", ".png;.jpg;.jpeg;.bmp;*.gif")])
        if file_path:
            encoded_image_string = encode_file(file_path)
            if encoded_image_string:
                messagebox.showinfo("Success", "Image encrypted successfully!")
    else:
        messagebox.showerror("Error", "Invalid password")

def decrypt_image():
    global encoded_image_string
    password = code.get()
    if password == "1111" and encoded_image_string:
        output_folder = filedialog.askdirectory(title="Select Folder to Save Decoded Image")
        if output_folder:
            decode_file(encoded_image_string, output_folder, "decoded_image.png")
    else:
        messagebox.showerror("Error", "Invalid password or no image selected!")

def encrypt_audio():
    global encoded_audio_string
    password = code.get()
    if password == "1111":
        file_path = filedialog.askopenfilename(filetypes=[("Audio Files", ".mp3;.wav")])
        if file_path:
            encoded_audio_string = encode_file(file_path)
            if encoded_audio_string:
                messagebox.showinfo("Success", "Audio encrypted successfully!")
    else:
        messagebox.showerror("Error", "Invalid password")

def decrypt_audio():
    global encoded_audio_string
    password = code.get()
    if password == "1111" and encoded_audio_string:
        output_folder = filedialog.askdirectory(title="Select Folder to Save Decoded Audio")
        if output_folder:
            audio_path = decode_file(encoded_audio_string, output_folder, "decoded_audio.mp3")
            if audio_path:
                pygame.mixer.music.load(audio_path)
                pygame.mixer.music.play()
    else:
        messagebox.showerror("Error", "Invalid password or no audio selected!")

def encrypt_video():
    global encoded_video_string
    password = code.get()
    if password == "1111":
        file_path = filedialog.askopenfilename(filetypes=[("Video Files", ".mp4;.avi;.mov")])
        if file_path:
            encoded_video_string = encode_file(file_path)
            if encoded_video_string:
                messagebox.showinfo("Success", "Video encrypted successfully!")
    else:
        messagebox.showerror("Error", "Invalid password")

def decrypt_video():
    global encoded_video_string
    password = code.get()
    if password == "1111" and encoded_video_string:
        output_folder = filedialog.askdirectory(title="Select Folder to Save Decoded Video")
        if output_folder:
            video_path = decode_file(encoded_video_string, output_folder, "decoded_video.mp4")
            if video_path:
                os.system(f"start {video_path}")
    else:
        messagebox.showerror("Error", "Invalid password or no video selected!")

def reset():
    aa.delete(1.0, tk.END)
    code.set("")
    global encoded_image_string, encoded_audio_string, encoded_video_string
    encoded_image_string = None
    encoded_audio_string = None
    encoded_video_string = None

a = tk.Label(t, text="Enter text for encryption and decryption", font="impack 16 bold")
a.place(x=5, y=6)
aa = tk.Text(t, font="20")
aa.place(x=5, y=45, width=410, height=120)
ab = tk.Label(t, text="Enter secret key", font="impack 13 bold")
ab.place(x=154, y=186)
code = tk.StringVar()
ac = tk.Entry(t, textvariable=code, bd=4, font="20", show="*")
ac.place(x=110, y=220)

btn = tk.Button(t, text="Text Encrypt", font="arial 15 bold", bg="red", fg="white", command=encrypt)
btn.place(x=15, y=280)
btn2 = tk.Button(t, text="Text Decrypt", font="arial 15 bold", bg="red", fg="white", command=decrypt)
btn2.place(x=260, y=280)
btn3 = tk.Button(t, text="Reset", font="arial 12 bold", bg="red", fg="white", command=reset)
btn3.place(x=150, y=350, width=120)
btn4 = tk.Button(t, text="Encrypt Image", font="arial 12 bold", bg="green", fg="white", command=encrypt_image)
btn4.place(x=50, y=400)
btn5 = tk.Button(t, text="Decrypt Image", font="arial 12 bold", bg="green", fg="white", command=decrypt_image)
btn5.place(x=250, y=400)
btn6 = tk.Button(t, text="Encrypt Audio", font="arial 12 bold", bg="blue", fg="white", command=encrypt_audio)
btn6.place(x=50, y=450)
btn7 = tk.Button(t, text="Decrypt Audio", font="arial 12 bold", bg="blue", fg="white", command=decrypt_audio)
btn7.place(x=250, y=450)
btn8 = tk.Button(t, text="Encrypt Video", font="arial 12 bold", bg="purple", fg="white", command=encrypt_video)
btn8.place(x=50, y=500)
btn9 = tk.Button(t, text="Decrypt Video", font="arial 12 bold", bg="purple", fg="white", command=decrypt_video)
btn9.place(x=250, y=500)

t.mainloop()
```


```python

```
