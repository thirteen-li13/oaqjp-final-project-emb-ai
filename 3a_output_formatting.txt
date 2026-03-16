'''
This module is for emotion detection
It calls IBM Watson NLP model to do emotion detection
'''

import requests
import json

def emotion_detector(text_to_analyse):
    '''
    This function uses the Emotion Predict function of the Watson NLP Library
    '''
    # Define the URL for the sentiment analysis API
    url = 'https://sn-watson-emotion.labs.skills.network/v1/watson.runtime.nlp.v1/NlpService/EmotionPredict'

    # Set the headers with the required model ID for the API
    headers = {"grpc-metadata-mm-model-id": "emotion_aggregated-workflow_lang_en_stock"}

    # Create the payload with the text to be analyzed
    myobj = { "raw_document": { "text": text_to_analyse } }

    # Make a POST request to the API with the payload and headers
    response = requests.post(url, json = myobj, headers = headers)

    # Parse the response from the API
    formatted_response = json.loads(response.text)

    # Get the dictionary stores emotion scores
    emotions = formatted_response['emotionPredictions'][0]['emotion']

    # Loop to find the dominant emotion and add to the emotion dictionary
    for key in emotions.keys():
        if emotions[key] == max(list(emotions.values())):
            emotions['dominant_emotion'] = key
            break

    # Return the response, formatted as a dictionary
    return emotions
