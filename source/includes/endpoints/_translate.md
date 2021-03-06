## Translate

```php
<?php
use Weglot\Client\Api\Enum\BotType;
use Weglot\Client\Api\TranslateEntry;
use Weglot\Client\Api\WordEntry;
use Weglot\Client\Api\Enum\WordType;
use Weglot\Client\Client;
use Weglot\Client\Endpoint\Translate;

// TranslateEntry parameters
$params = [
    'language_from' => 'en',
    'language_to' => 'de',
    'title' => 'Lorem ipsum dolor sit amet',
    'request_url' => 'https://foo.bar/',
    'bot' => BotType::HUMAN
];

$translate = new TranslateEntry($params);
$translate->getInputWords()
    ->addOne(new WordEntry('This is a blue car', WordType::TEXT))
    ->addOne(new WordEntry('This is a black car', WordType::TEXT));

// Client
$client = new Client(getenv('WG_API_KEY'));
$translate = new Translate($translate, $client);
$translated = $translate->handle();
```

```shell
curl -X POST "https://api.weglot.com/translate?api_key=my_api_key" \
-H "Content-Type: application/json" \
-d \
'
{
   "l_from": "en",
    "l_to": "fr",
    "words": [
         {
            "t": "1",
            "w": "This is a blue car"
        },
        {
            "t": "1",
            "w": "This is a black car"
        }
    ]
}'
```

> The above command returns JSON structured like this:

```json
{
    "succeeded": 1,
    "answer": {
	    "l_from": "en",
        "l_to": "fr",
        "title": "",
        "bot": 0,
        "from_words": [
            "This is a blue car",
            "This is a black car"
        ],
        "to_words": [  
            "Il s’agit d’une voiture bleue",
            "C'est une voiture noire"
        ]
	}
}
```

This endpoint retrieves all translations. It takes an array of sentences in an original languages in input and output the same array of sentences but translated in another languages.

### HTTP Request

`POST https://api.weglot.com/translate`

### JSON Body Parameters

Parameter | Required | Description
--------- | -------- | -----------
l_from | true | ISO 639-1 code of the original language
l_to | true | ISO 639-1 code of the destination language
words | true | array of sentence in original language. Each words contains 2 parameters 
words\[t\] | true | the type of the word : check at [WordType resource](#wordtype) for more details
words\[w\] | true | the sentence to translate
bot | true | link to user agent : check at [BotType resource](#bottype) for more details
request_url | true | the URL where the request is from
title | false | the title of the page where these sentences come from

### Returned Content

Parameter  | Description
--------- | -----------
succeeded | true if pass, false if not
answer | content of answer as JSON
answer\[l_from\] | ISO 639-1 code of the original language
answer\[l_to\] | ISO 639-1 code of the destination language
answer\[title\] | the title of the page where these sentences come from
answer\[request_url\] | the URL where the request is from
answer\[bot\] | link to user agent : check at [BotType resource](#bottype) for more details
answer\[from_words\] | array of sentence in original language. Each words contains 2 parameters 
answer\[to_words\] | array of sentence in destination language. This is the important part !