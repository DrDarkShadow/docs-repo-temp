

<!-- DOC_ID: app.py----get_gemini_response -->
python
# Assume 'gemini_model' is a pre-configured instance of a Gemini model client.

def get_gemini_response(text_prompt: str):
    """Get response from Gemini AI model"""
    print(f"[Gemini] Generating response for prompt: {text_prompt}")
    try:
        response = gemini_model.generate_content(text_prompt)
        print("[Gemini] Response generated successfully")
        return response.text
    except Exception as e:
        print(f"[ERROR] Gemini generation failed: {str(e)}")
        return f"Error: {str(e)}"

# Example usage:
prompt = "What are the main benefits of using Python for web development?"
response = get_gemini_response(prompt)

if not response.startswith("Error:"):
    print("AI Response:")
    print(response)
else:
    print(f"Failed to get response: {response}")
<!-- END_DOC_ID: app.py----get_gemini_response -->



<!-- DOC_ID: app.py----generate_tts_audio_file -->
python
import pyttsx3
import os

# Assume generate_tts_audio is defined as in the snippet
def generate_tts_audio(text_to_speak, output_file_path):
    """Generate TTS audio file using pyttsx3"""
    print(f"[TTS] Generating speech for text: {text_to_speak}")
    if not text_to_speak:
        return False

    try:
        engine = pyttsx3.init()
        engine.setProperty('rate', 150)  # Adjust speech rate
        engine.save_to_file(text_to_speak, output_file_path)
        engine.runAndWait()
        print(f"[TTS] Audio saved at: {output_file_path}")

        if os.path.exists(output_file_path) and os.path.getsize(output_file_path) > 0:
            return True
        return False
    except Exception as e:
        print(f"[TTS ERROR] {str(e)}")
        if os.path.exists(output_file_path):
            try: os.remove(output_file_path)
            except: pass
        return False

# Example usage:
text = "Hello, world! This is a test of the text to speech conversion."
output_file = "hello_world.mp3"

success = generate_tts_audio(text, output_file)

if success:
    print(f"Successfully created audio file at '{output_file}'")
    # Clean up the created file
    os.remove(output_file)
else:
    print("Failed to create the audio file.")
<!-- END_DOC_ID: app.py----generate_tts_audio_file -->



<!-- DOC_ID: app.py----index -->
python
from flask import Flask, render_template
import os

app = Flask(__name__)

# Ensure a 'templates' directory exists for the example
if not os.path.exists('templates'):
    os.makedirs('templates')

# Create a simple index.html file for the example
with open('templates/index.html', 'w') as f:
    f.write('<!DOCTYPE html><html><head><title>Home</title></head><body><h1>Welcome to the Index Page!</h1></body></html>')

@app.route('/')
def index():
    """
    Handles requests for the index page.
    """
    print("[ROUTE] Index page requested")
    return render_template('index.html')

if __name__ == '__main__':
    # When you run this script and navigate to http://127.0.0.1:5000/
    # the console will print "[ROUTE] Index page requested"
    # and the browser will display the content of index.html.
    app.run(debug=True)
<!-- END_DOC_ID: app.py----index -->



<!-- DOC_ID: app.py----process_full_audio_route -->
python
import requests
import urllib.parse

# The URL of the Flask endpoint
url = "http://127.0.0.1:5000/process-full-audio"

# Path to the audio file you want to send
audio_file_path = 'path/to/your/query.webm'

try:
    with open(audio_file_path, 'rb') as f:
        # The key 'audio_blob' must match what the server expects
        files = {'audio_blob': (audio_file_path, f, 'audio/webm')}
        
        response = requests.post(url, files=files)

    if response.status_code == 200:
        # Save the returned audio response
        with open('response.wav', 'wb') as out_f:
            out_f.write(response.content)
        
        print("Successfully received and saved response.wav")
        
        # Access and decode the custom headers
        transcription = urllib.parse.unquote(response.headers.get('X-Transcription', ''))
        gemini_response = urllib.parse.unquote(response.headers.get('X-Gemini-Response', ''))
        
        print(f"\n--- Metadata ---")
        print(f"Transcription: {transcription}")
        print(f"Gemini Response: {gemini_response}")
        
    else:
        print(f"Error: {response.status_code}")
        print(response.json())

except requests.exceptions.RequestException as e:
    print(f"An error occurred: {e}")
<!-- END_DOC_ID: app.py----process_full_audio_route -->



<!-- DOC_ID: app.py----process_text_route -->
bash
# Send a POST request with a JSON payload
# The audio output is saved to response.wav
# The response headers are printed to the console
curl -X POST \
  -H "Content-Type: application/json" \
  -d '{"text": "What is the weather like on Mars?"}' \
  -i \
  -o response.wav \
  http://127.0.0.1:5000/process-text

# Expected Console Output (Headers):
# HTTP/1.1 200 OK
# Server: Werkzeug/2.0.1 Python/3.9.1
# Date: Mon, 01 Jan 2024 12:00:00 GMT
# Content-Type: audio/wav
# Content-Length: 123456
# X-Gemini-Response: The%20weather%20on%20Mars%20is%20typically%20cold%20and%20dry...
# ...

# You can now play the 'response.wav' file.
<!-- END_DOC_ID: app.py----process_text_route -->

