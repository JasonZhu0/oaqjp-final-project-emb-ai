import requests, json

def emotion_detector(text_to_analyse): 
    # URL of the emotion detection service 
    url = 'https://sn-watson-emotion.labs.skills.network/v1/watson.runtime.nlp.v1/NlpService/EmotionPredict' 
    
    # Set the headers required for the API request 
    header =  {"grpc-metadata-mm-model-id": "emotion_aggregated-workflow_lang_en_stock"} 
    
    # Create a dictionary with the text to be analyzed 
    myobj = { "raw_document": { "text": text_to_analyse } } 
    
    # Send a POST request to the API with the text and headers 
    response = requests.post(url, json=myobj, headers=header)

    # Parse the response from the API into required json format
    formatted_response = json.loads(response.text)
    anger_score = formatted_response['emotionPredictions'][0]['emotion']['anger']
    disgust_score = formatted_response['emotionPredictions'][0]['emotion']['disgust']
    fear_score = formatted_response['emotionPredictions'][0]['emotion']['fear']
    joy_score = formatted_response['emotionPredictions'][0]['emotion']['joy']
    sadness_score = formatted_response['emotionPredictions'][0]['emotion']['sadness']
    
    # put all five emotions into a dict and find the max in order to get dominant emotion
    emotion_scores = {
        'anger': anger_score,
        'disgust': disgust_score,
        'fear': fear_score,
        'joy': joy_score,
        'sadness': sadness_score
    }
    dominant_emotion = max(emotion_scores, key=emotion_scores.get)
    
    # Return the response text from the API 
    return {'anger': anger_score,'disgust': disgust_score,'fear': fear_score,'joy': joy_score,'sadness': sadness_score,'dominant_emotion': dominant_emotion}