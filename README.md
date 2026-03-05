# Sentiment Analyzer

A Python-based sentiment analysis tool that leverages IBM Watson's BERT-powered NLP service to analyze the emotional tone of text inputs.

## Overview

This project provides a simple yet powerful interface to perform sentiment analysis on any text. It uses Watson's pre-trained BERT model to classify text as positive, negative, or neutral, along with a confidence score.

## Features

- **Real-time Sentiment Analysis**: Analyze text sentiment instantly
- **Multi-language Support**: Powered by a multilingual BERT model
- **Confidence Scoring**: Get probability scores for sentiment predictions
- **Simple API**: Easy-to-use function interface

## Project Structure

```
Sentiment-analyzer/
├── SentimentAnalysis/      # Main analysis module
├── static/                 # Static assets (CSS, JS, images)
├── templates/              # HTML templates
├── server.py               # Flask server implementation
├── test_sentiment_analysis.py  # Unit tests
├── README.md               # Project documentation
├── LICENSE                 # License file
└── .gitignore             # Git ignore rules
```

## Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/sahilgulunjkar/Sentiment-analyzer.git
   cd Sentiment-analyzer
   ```

2. **Install required dependencies**
   ```bash
   pip install requests
   ```

## Usage

### Basic Function Call

```python
from sentiment_analyzer import sentiment_analyzer

# Analyze text sentiment
text = "I love this product! It's absolutely amazing!"
result = sentiment_analyzer(text)

print(f"Sentiment: {result['label']}")
print(f"Confidence Score: {result['score']}")
```

### Expected Output

```python
{
    'label': 'positive',  # or 'negative', 'neutral'
    'score': 0.95         # confidence score (0-1)
}
```

## Core Logic

The main sentiment analysis function works as follows:

```python
import requests
import json

def sentiment_analyzer(text_to_analyse):
    # Watson NLP sentiment analysis endpoint
    url = 'https://sn-watson-sentiment-bert.labs.skills.network/v1/watson.runtime.nlp.v1/NlpService/SentimentPredict'
    
    # Request payload
    myobj = { "raw_document": { "text": text_to_analyse } }
    
    # Model specification header
    header = {"grpc-metadata-mm-model-id": "sentiment_aggregated-bert-workflow_lang_multi_stock"}
    
    # Send POST request
    response = requests.post(url, json=myobj, headers=header)
    
    # Parse response
    formatted_response = json.loads(response.text)
    
    # Extract results
    label = formatted_response['documentSentiment']['label']
    score = formatted_response['documentSentiment']['score']
    
    return {'label': label, 'score': score}
```

## API Endpoint

The service connects to IBM Watson's NLP sentiment analysis API:

- **Endpoint**: `https://sn-watson-sentiment-bert.labs.skills.network/v1/watson.runtime.nlp.v1/NlpService/SentimentPredict`
- **Method**: POST
- **Model**: BERT-based multilingual sentiment aggregation workflow

## Response Format

The API returns a dictionary with two keys:

| Key | Type | Description |
|-----|------|-------------|
| `label` | string | Sentiment classification: 'positive', 'negative', or 'neutral' |
| `score` | float | Confidence score ranging from 0 to 1 |

## Example Use Cases

- **Customer Feedback Analysis**: Analyze product reviews and customer comments
- **Social Media Monitoring**: Track sentiment of brand mentions
- **Survey Analysis**: Process open-ended survey responses
- **Content Moderation**: Identify negative or toxic content
- **Market Research**: Understand public opinion on topics

## Testing

Run the test suite to verify functionality:

```bash
python test_sentiment_analysis.py
```

## Dependencies

- **requests**: For making HTTP requests to the Watson API
- **json**: For parsing API responses (built-in)

## Technical Details

### Model Information
- **Architecture**: BERT (Bidirectional Encoder Representations from Transformers)
- **Training**: Multilingual stock sentiment aggregation
- **Provider**: IBM Watson NLP

### Performance Considerations
- API calls require internet connectivity
- Response time depends on text length and network latency
- Consider implementing caching for repeated analyses

## Limitations

- Requires active internet connection
- Dependent on external Watson API availability
- Rate limiting may apply based on API service terms
- Text length limitations may apply (check API documentation)

**Note**: This project uses an external API service. Ensure you comply with the service's terms of use and rate limiting policies.
