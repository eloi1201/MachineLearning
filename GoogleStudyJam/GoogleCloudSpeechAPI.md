# Google Cloud Speech API

The Google Cloud Speech API allows sending audio and receive a text transcription from the service.


### Create an API Key

export API_KEY=<YOUR_API_KEY>


### Create your Speech API request

```
$ cat request.json
{
  "config": {
      "encoding":"FLAC",
      "sample_rate": 16000,
      "language_code": "en-US"
  },
  "audio": {
      "uri":"gs://cloud-samples-tests/speech/brooklyn.flac"
  }
}
```
* encoding      : Audio filetype.
* sample_rate   : The hertz information of audio file.
* Language_code : Language code
* Supportive language : https://cloud.google.com/speech-to-text/docs/languages


### Call the Speech API

```
$ curl -s -X POST -H "Content-Type: application/json" --data-binary @request.json \
> "https://speech.googleapis.com/v1beta1/speech:syncrecognize?key=${API_KEY}"
{
  "results": [
    {
      "alternatives": [
        {
          "transcript": "how old is the Brooklyn Bridge",
          "confidence": 0.9835046
        }
      ]
    }
  ]
}
```

* transcript : Exported strings from audio file through Google Cloud Speech API
* confidence : Confidence score for the exported results
