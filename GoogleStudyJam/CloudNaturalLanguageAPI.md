# Cloud Natural Language API

### Create an API Key
```
export GOOGLE_CLOUD_PROJECT=$(gcloud config get-value core/project)
```
* gcloud config get-value core/project : Project ID extraction command

```
gcloud iam service-accounts create my-natlang-sa --display-name "my natural language service account"
```
* The command is for creating IAM Service Account to access NL API.

* IAM(Identity and Access Management) : Control authorization that uses cloud resouce when it requires

```
gcloud iam service-accounts keys create ~/key.json --iam-account my-natlang-sa@${GOOGLE_CLOUD_PROJECT}.iam.gserviceaccount.com
```
* Create and save API KEY as JSON file through IAM Service account. 

```
export GOOGLE_APPLICATION_CREDENTIALS="/home/USER/key.json"
```


### Make an Entity Analysis Request 1 with gclouod command

```
$ gcloud ml language analyze-entities --content="Michelangelo Caravaggio, Italian painter, is known for 'The Calling of Saint Matthew'."
{
  "entities": [
    {
      "mentions": [
        {
          "text": {
            "beginOffset": 0,
            "content": "Michelangelo Caravaggio"
          },
          "type": "PROPER"
        },
        {
          "text": {
            "beginOffset": 33,
            "content": "painter"
          },
          "type": "COMMON"
        }
      ],
      "metadata": {
        "mid": "/m/020bg",
        "wikipedia_url": "https://en.wikipedia.org/wiki/Caravaggio"
      },
      "name": "Michelangelo Caravaggio",
      "salience": 0.82904786,
      "type": "PERSON"
    },
    {
      "mentions": [
        {
          "text": {
            "beginOffset": 25,
            "content": "Italian"
          },
          "type": "PROPER"
        }
      ],
      "metadata": {
        "mid": "/m/03rjj",
        "wikipedia_url": "https://en.wikipedia.org/wiki/Italy"
      },
      "name": "Italian",
      "salience": 0.13981608,
      "type": "LOCATION"
    },
    {
      "mentions": [
        {
          "text": {
            "beginOffset": 56,
            "content": "The Calling of Saint Matthew"
          },
          "type": "PROPER"
        }
      ],
      "metadata": {
        "mid": "/m/085_p7",
        "wikipedia_url": "https://en.wikipedia.org/wiki/The_Calling_of_St_Matthew_(Caravaggio)"
      },
      "name": "The Calling of Saint Matthew",
      "salience": 0.031136045,
      "type": "EVENT"
    }
  ],
  "language": "en"
}
```

### Make an Entity Analysis Request 2

```
$ cat request2.json
{
  "document":{
    "type":"PLAIN_TEXT",
    "content":"Joanne Rowling, who writes under the pen names J. K. Rowling and Robert Galbraith, is a British novelist and screenwriter who wrote the Harry Potter fantasy series."
  },
  "encodingType":"UTF8"
}
```

```
curl "https://language.googleapis.com/v1/documents:analyzeEntities?key=${API_KEY}" \
  -s -X POST -H "Content-Type: application/json" --data-binary @request2.json
{
  "entities": [
    {
      "name": "Joanne Rowling",
      "type": "PERSON",
      "metadata": {
        "wikipedia_url": "https://en.wikipedia.org/wiki/J._K._Rowling",
        "mid": "/m/042xh"
      },
      "salience": 0.79828626,
      "mentions": [
        {
          "text": {
            "content": "Joanne Rowling",
            "beginOffset": 0
          },
          "type": "PROPER"
        },
        {
          "text": {
            "content": "Rowling",
            "beginOffset": 53
          },
          "type": "PROPER"
        },
        {
          "text": {
            "content": "novelist",
            "beginOffset": 96
          },
          "type": "COMMON"
        },
        {
          "text": {
            "content": "Robert Galbraith",
            "beginOffset": 65
          },
          "type": "PROPER"
        }
      ]
    },
    {
      "name": "pen names",
      "type": "OTHER",
      "metadata": {},
      "salience": 0.07300248,
      "mentions": [
        {
          "text": {
            "content": "pen names",
            "beginOffset": 37
          },
          "type": "COMMON"
        }
      ]
    },
    {
      "name": "J.K.",
      "type": "PERSON",
      "metadata": {},
      "salience": 0.043804582,
      "mentions": [
        {
          "text": {
            "content": "J. K.",
            "beginOffset": 47
          },
          "type": "PROPER"
        }
      ]
    },
    {
      "name": "British",
      "type": "LOCATION",
      "metadata": {
        "mid": "/m/07ssc",
        "wikipedia_url": "https://en.wikipedia.org/wiki/United_Kingdom"
      },
      "salience": 0.019752095,
      "mentions": [
        {
          "text": {
            "content": "British",
            "beginOffset": 88
          },
          "type": "PROPER"
        }
      ]
    },
    {
      "name": "fantasy series",
      "type": "WORK_OF_ART",
      "metadata": {},
      "salience": 0.01764168,
      "mentions": [
        {
     "metadata": {},
      "salience": 0.01764168,
      "mentions": [
        {
          "text": {
            "content": "fantasy series",
            "beginOffset": 149
          },
          "type": "COMMON"
        }
      ]
    },
    {
      "name": "Harry Potter",
      "type": "WORK_OF_ART",
      "metadata": {
        "wikipedia_url": "https://en.wikipedia.org/wiki/Harry_Potter",
        "mid": "/m/078ffw"
      },
      "salience": 0.014916742,
      "mentions": [
        {
          "text": {
            "content": "Harry Potter",
            "beginOffset": 136
          },
          "type": "PROPER"
        }
      ]
    },
    {
      "name": "screenwriter",
      "type": "PERSON",
      "metadata": {},
      "salience": 0.011085264,
      "mentions": [
        {
          "text": {
            "content": "screenwriter",
            "beginOffset": 109
          },
          "type": "COMMON"
        }
      ]
    }
  ],
  "language": "en"
}
```
* name: Entity name
* metadata: It would be contained if there is relavat information in wiki.
* salience: The importance score of the entity in whole text


### Sentiment analysis with the Natural Language API

```
$ cat request3.json
 {
  "document":{
    "type":"PLAIN_TEXT",
    "content":"Harry Potter is the best book. I think everyone should read it."
  },
  "encodingType": "UTF8"
}
```

```
$ curl "https://language.googleapis.com/v1/documents:analyzeSentiment?key=${API_KEY}" \
>   -s -X POST -H "Content-Type: application/json" --data-binary @request3.json
{
  "documentSentiment": {
    "magnitude": 0.8,
    "score": 0.4
  },
  "language": "en",
  "sentences": [
    {
      "text": {
        "content": "Harry Potter is the best book.",
        "beginOffset": 0
      },
      "sentiment": {
        "magnitude": 0.7,
        "score": 0.7
      }
    },
    {
      "text": {
        "content": "I think everyone should read it.",
        "beginOffset": 31
      },
      "sentiment": {
        "magnitude": 0.1,
        "score": 0.1
      }
    }
  ]
}
```
* score - is a number from -1.0 to 1.0 indicating how positive or negative the statement is.
* magnitude - is a number ranging from 0 to infinity that represents the weight of sentiment expressed in the statement, regardless of being positive or negative.

### Analyzing entity sentiment
```
$ cat request4.json
 {
  "document":{
    "type":"PLAIN_TEXT",
    "content":"I liked the sushi but the service was terrible."
  },
  "encodingType": "UTF8"
}
```

```
$ curl "https://language.googleapis.com/v1/documents:analyzeEntitySentiment?key=${API_KEY}"   -s -X POST -H "Content
-Type: application/json" --data-binary @request4.json
{
  "entities": [
    {
      "name": "sushi",
      "type": "CONSUMER_GOOD",
      "metadata": {},
      "salience": 0.51064336,
      "mentions": [
        {
          "text": {
            "content": "sushi",
            "beginOffset": 12
          },
          "type": "COMMON",
          "sentiment": {
            "magnitude": 0.9,
            "score": 0.9
          }
        }
      ],
      "sentiment": {
        "magnitude": 0.9,
        "score": 0.9
      }
    },
    {
      "name": "service",
      "type": "OTHER",
      "metadata": {},
      "salience": 0.48935664,
      "mentions": [
        {
          "text": {
            "content": "service",
            "beginOffset": 26
          },
          "type": "COMMON",
          "sentiment": {
            "magnitude": 0.9,
            "score": -0.9
          }
        }
      ],
      "sentiment": {
        "magnitude": 0.9,
        "score": -0.9
      }
    }
  ],
  "language": "en"
}          
```



### Analyzing syntax and parts of speech

```
$ cat request5.json 
{
  "document":{
    "type":"PLAIN_TEXT",
    "content": "Joanne Rowling is a British novelist, screenwriter and film producer."
  },
  "encodingType": "UTF8"
}
```

```
curl "https://language.googleapis.com/v1/documents:analyzeSyntax?key=${API_KEY}" \
  -s -X POST -H "Content-Type: application/json" --data-binary @request.json
  
{
  "sentences": [
    {
      "text": {
        "content": "Joanne Rowling is a British novelist, screenwriter and film producer.",
        "beginOffset": 0
      }
    }
  ],
  "tokens": [
    {
      "text": {
        "content": "Joanne",
        "beginOffset": 0
      },
      "partOfSpeech": {
        "tag": "NOUN",
        "aspect": "ASPECT_UNKNOWN",
        "case": "CASE_UNKNOWN",
        "form": "FORM_UNKNOWN",
        "gender": "GENDER_UNKNOWN",
        "mood": "MOOD_UNKNOWN",
        "number": "SINGULAR",
        "person": "PERSON_UNKNOWN",
        "proper": "PROPER",
        "reciprocity": "RECIPROCITY_UNKNOWN",
        "tense": "TENSE_UNKNOWN",
        "voice": "VOICE_UNKNOWN"
      },
      "dependencyEdge": {
        "headTokenIndex": 1,
        "label": "NN"
      },
      "lemma": "Joanne"
    },
    {
      "text": {
        "content": "Rowling",
        "beginOffset": 7
      },
      "partOfSpeech": {
        "tag": "NOUN",
        "aspect": "ASPECT_UNKNOWN",
        "case": "CASE_UNKNOWN",
        "form": "FORM_UNKNOWN",
        "gender": "GENDER_UNKNOWN",
        "mood": "MOOD_UNKNOWN",
        "number": "SINGULAR",
         "person": "PERSON_UNKNOWN",
        "proper": "PROPER",
        "reciprocity": "RECIPROCITY_UNKNOWN",
        "tense": "TENSE_UNKNOWN",
        "voice": "VOICE_UNKNOWN"
      },
      "dependencyEdge": {
        "headTokenIndex": 2,
        "label": "NSUBJ"
      },
      "lemma": "Rowling"
    },
    {
      "text": {
        "content": "is",
        "beginOffset": 15
      },
      "partOfSpeech": {
        "tag": "VERB",
        "aspect": "ASPECT_UNKNOWN",
        "case": "CASE_UNKNOWN",
        "form": "FORM_UNKNOWN",
        "gender": "GENDER_UNKNOWN",
        "mood": "INDICATIVE",
        "number": "SINGULAR",
        "person": "THIRD",
        "proper": "PROPER_UNKNOWN",
        "reciprocity": "RECIPROCITY_UNKNOWN",
        "tense": "PRESENT",
        "voice": "VOICE_UNKNOWN"
      },
      "dependencyEdge": {
        "headTokenIndex": 2,
        "label": "ROOT"
      },
      "lemma": "be"
    },
    {
      "text": {
        "content": "a",
        "beginOffset": 18
      },
      "partOfSpeech": {
        "tag": "DET",
        "aspect": "ASPECT_UNKNOWN",
        "case": "CASE_UNKNOWN",
        "form": "FORM_UNKNOWN",
        "gender": "GENDER_UNKNOWN",
        "mood": "MOOD_UNKNOWN",
        "number": "NUMBER_UNKNOWN",
        "person": "PERSON_UNKNOWN",
        "proper": "PROPER_UNKNOWN",
        "reciprocity": "RECIPROCITY_UNKNOWN",
        "tense": "TENSE_UNKNOWN",
        "voice": "VOICE_UNKNOWN"
      },
      "dependencyEdge": {
        "headTokenIndex": 5,
        "label": "DET"
      },
      "lemma": "a"
    },
    {
      "text": {
        "content": "British",
        "beginOffset": 20
      },
      "partOfSpeech": {
        "tag": "ADJ",
        "aspect": "ASPECT_UNKNOWN",
        "case": "CASE_UNKNOWN",
        "form": "FORM_UNKNOWN",
        "gender": "GENDER_UNKNOWN",
        "mood": "MOOD_UNKNOWN",
        "number": "NUMBER_UNKNOWN",
        "person": "PERSON_UNKNOWN",
        "proper": "PROPER_UNKNOWN",
        "reciprocity": "RECIPROCITY_UNKNOWN",
        "tense": "TENSE_UNKNOWN",
        "voice": "VOICE_UNKNOWN"
      },
      "dependencyEdge": {
        "headTokenIndex": 5,
        "label": "AMOD"
      },
      "lemma": "British"
    },
    {
      "text": {
        "content": "novelist",
        "beginOffset": 28
      },
      "partOfSpeech": {
        "tag": "NOUN",
        "aspect": "ASPECT_UNKNOWN",
        "case": "CASE_UNKNOWN",
        "form": "FORM_UNKNOWN",
        "gender": "GENDER_UNKNOWN",
        "mood": "MOOD_UNKNOWN",
        "number": "SINGULAR",
        "person": "PERSON_UNKNOWN",
        "proper": "PROPER_UNKNOWN",
        "reciprocity": "RECIPROCITY_UNKNOWN",
        "tense": "TENSE_UNKNOWN",
        "voice": "VOICE_UNKNOWN"
      },
      "dependencyEdge": {
        "headTokenIndex": 2,
        "label": "ATTR"
      },
      "lemma": "novelist"
    },
    {
      "text": {
        "content": ",",
        "beginOffset": 36
      },
      "partOfSpeech": {
        "tag": "PUNCT",
        "aspect": "ASPECT_UNKNOWN",
        "case": "CASE_UNKNOWN",
        "form": "FORM_UNKNOWN",
        "gender": "GENDER_UNKNOWN",
        "mood": "MOOD_UNKNOWN",
        "number": "NUMBER_UNKNOWN",
        "person": "PERSON_UNKNOWN",
        "proper": "PROPER_UNKNOWN",
        "reciprocity": "RECIPROCITY_UNKNOWN",
        "tense": "TENSE_UNKNOWN",
        "voice": "VOICE_UNKNOWN"
      },
      "dependencyEdge": {
        "headTokenIndex": 5,
        "label": "P"
      },
      "lemma": ","
    },
    {
      "text": {
        "content": "screenwriter",
        "beginOffset": 38
      },
      "partOfSpeech": {
        "tag": "NOUN",
        "aspect": "ASPECT_UNKNOWN",
        "case": "CASE_UNKNOWN",
        "form": "FORM_UNKNOWN",
        "gender": "GENDER_UNKNOWN",
        "mood": "MOOD_UNKNOWN",
        "number": "SINGULAR",
        "person": "PERSON_UNKNOWN",
        "proper": "PROPER_UNKNOWN",
        "reciprocity": "RECIPROCITY_UNKNOWN",
        "tense": "TENSE_UNKNOWN",
        "voice": "VOICE_UNKNOWN"
      },
      "dependencyEdge": {
        "headTokenIndex": 5,
        "label": "CONJ"
      },
      "lemma": "screenwriter"
    },
    {
      "text": {
        "content": "and",
        "beginOffset": 51
      },
      "partOfSpeech": {
        "tag": "CONJ",
        "aspect": "ASPECT_UNKNOWN",
        "case": "CASE_UNKNOWN",
        "form": "FORM_UNKNOWN",
        "gender": "GENDER_UNKNOWN",
        "mood": "MOOD_UNKNOWN",
        "number": "NUMBER_UNKNOWN",
        "person": "PERSON_UNKNOWN",
        "proper": "PROPER_UNKNOWN",
        "reciprocity": "RECIPROCITY_UNKNOWN",
        "tense": "TENSE_UNKNOWN",
        "voice": "VOICE_UNKNOWN"
      },
      "dependencyEdge": {
        "headTokenIndex": 5,
        "label": "CC"
      },
      "lemma": "and"
    },
    {
      "text": {
        "content": "film",
        "beginOffset": 55
      },
      "partOfSpeech": {
        "tag": "NOUN",
        "aspect": "ASPECT_UNKNOWN",
        "case": "CASE_UNKNOWN",
        "form": "FORM_UNKNOWN",
        "gender": "GENDER_UNKNOWN",
        "mood": "MOOD_UNKNOWN",
        "number": "SINGULAR",
        "person": "PERSON_UNKNOWN",
        "proper": "PROPER_UNKNOWN",
        "reciprocity": "RECIPROCITY_UNKNOWN",
        "tense": "TENSE_UNKNOWN",
        "voice": "VOICE_UNKNOWN"
      },
      "dependencyEdge": {
        "headTokenIndex": 10,
        "label": "NN"
      },
      "lemma": "film"
    },
    {
      "text": {
        "content": "producer",
        "beginOffset": 60
      },
      "partOfSpeech": {
        "tag": "NOUN",
        "aspect": "ASPECT_UNKNOWN",
        "case": "CASE_UNKNOWN",
        "form": "FORM_UNKNOWN",
        "gender": "GENDER_UNKNOWN",
        "mood": "MOOD_UNKNOWN",
        "number": "SINGULAR",
        "person": "PERSON_UNKNOWN",
        "proper": "PROPER_UNKNOWN",
        "reciprocity": "RECIPROCITY_UNKNOWN",
        "tense": "TENSE_UNKNOWN",
        "voice": "VOICE_UNKNOWN"
      },
      "dependencyEdge": {
        "headTokenIndex": 5,
        "label": "CONJ"
      },
      "lemma": "producer"
    },
    {
      "text": {
        "content": ".",
        "beginOffset": 68
      },
      "partOfSpeech": {
        "tag": "PUNCT",
        "aspect": "ASPECT_UNKNOWN",
        "case": "CASE_UNKNOWN",
        "form": "FORM_UNKNOWN",
        "gender": "GENDER_UNKNOWN",
        "mood": "MOOD_UNKNOWN",
        "number": "NUMBER_UNKNOWN",
        "person": "PERSON_UNKNOWN",
        "proper": "PROPER_UNKNOWN",
        "reciprocity": "RECIPROCITY_UNKNOWN",
        "tense": "TENSE_UNKNOWN",
        "voice": "VOICE_UNKNOWN"
      },
      "dependencyEdge": {
        "headTokenIndex": 2,
        "label": "P"
      },
      "lemma": "."
    }
  ],
  "language": "en"
}
```


### Multilingual natural language processing

```
$ cat request6.json
{
  "document":{
    "type":"PLAIN_TEXT",
    "content":"日本のグーグルのオフィスは、東京の六本木ヒルズにあります"
  }
}
```

```
$ curl "https://language.googleapis.com/v1/documents:analyzeEntities?key=${API_KEY}"   -s -X POST -H "Content-Type: 
application/json" --data-binary @request6.json | head
{
  "entities": [
    {
      "name": "日本",
      "type": "LOCATION",
      "metadata": {
        "mid": "/m/03_3d",
        "wikipedia_url": "https://en.wikipedia.org/wiki/Japan",
        "mid": "/m/03_3d"
      },
      "salience": 0.23804513,
      "mentions": [
        {
          "text": {
            "content": "日本",
            "beginOffset": -1
          },
          "type": "PROPER"
        }
      ]
    },
    {
      "name": "グーグル",
      "type": "ORGANIZATION",
      "metadata": {
        "mid": "/m/045c7b",
        "wikipedia_url": "https://en.wikipedia.org/wiki/Google"
      },
      "salience": 0.21214141,
      "mentions": [
        {
          "text": {
            "content": "グーグル",
            "beginOffset": -1
          },
          "type": "PROPER"
        }
      ]
    },
    {
      "name": "六本木ヒルズ",
      "type": "PERSON",
      "metadata": {
        "wikipedia_url": "https://en.wikipedia.org/wiki/Roppongi_Hills",
        "mid": "/m/01r2_k"
      },
      "salience": 0.19418614,
      
      "metadata": {
        "wikipedia_url": "https://en.wikipedia.org/wiki/Roppongi_Hills",
        "mid": "/m/01r2_k"
      },
      "salience": 0.19418614,
      "mentions": [
        {
          "text": {
            "content": "六本木ヒルズ",
            "beginOffset": -1
          },
          "type": "PROPER"
        }
      ]
    },
    {
      "name": "東京",
      "type": "LOCATION",
      "metadata": {
        "mid": "/g/12lnhn10f",
        "wikipedia_url": "https://de.wikipedia.org/wiki/Tokio"
      },
      "salience": 0.18159479,
      "mentions": [
        {
          "text": {
            "content": "東京",
            "beginOffset": -1
          },
          "type": "PROPER"
        }
      ]
    },
    {
      "name": "オフィス",
      "type": "OTHER",
      "metadata": {},
      "salience": 0.17403255,
      "mentions": [
        {
          "text": {
            "content": "オフィス",
            "beginOffset": -1
          },
          "type": "COMMON"
        }
      ]
    }
  ],
  "language": "ja"
}
```
