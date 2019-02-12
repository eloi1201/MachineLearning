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


### Make an Entity Analysis Request

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
