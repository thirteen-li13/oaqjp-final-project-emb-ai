'''
Executing this function initiates the application of emotion detection
to be executed over the Flask channel and deployed on localhost:5000.
'''

# Import Flask, render_template, request from the flask pramework package
from flask import Flask, request, render_template

# Import the emotion_detector function from the package created
from EmotionDetection.emotion_detection import emotion_detector

# Initiate the flask app
app = Flask(__name__)

@app.route("/emotionDetector")
def emo_detector():
    ''' This code receives the text from the HTML interface and 
        runs emotion detecton over it using emotion_detector() function.
        The output returned shows the emotion probability
        and the dominant emotion for the provided text.
    '''
    # Send a GET request to the HTML interface to receive the input text,
    # Store the incoming text to a variable text_to_analyze
    text_to_analyze = request.args.get('textToAnalyze')

    # For invalid input: Check if text is a non-empty string
    if text_to_analyze is None or text_to_analyze.strip() == '' or text_to_analyze.isdigit():
        return 'Invalid text! Please try again!'

    # Call sentiment_analyzer application with text_to_analyze as the argument
    response = emotion_detector(text_to_analyze)

    # Return formatted string
    return f"For the given statement, the system response is "\
           f"'anger': {response['anger']}, "\
           f"'disgust': {response['disgust']}, "\
           f"'fear': {response['fear']}, "\
           f"'joy': {response['joy']} and "\
           f"'sadness': {response['sadness']}. "\
           f"The dominant emotion is {response['dominant_emotion']}."

@app.route("/")
def render_index_page():
    ''' This function initiates the rendering of the main application
        page over the Flask channel
    '''
    # Run the render_template function on the HTML template
    return render_template('index.html')

if __name__ == "__main__":

    # Run the application on host: 0.0.0.0 (or localhost) on port number 5000
    app.run(host = '0.0.0.0', port = 5000)
