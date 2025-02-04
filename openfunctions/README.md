# Gorilla Openfunctions

💡 Comes with Parallel Function Calling!  
🚀 Try it out on [Colab](https://colab.research.google.com/drive/16M5J2H9F8YQora_W2PDnp120slZH-Mqd?usp=sharing)   
📣 Read more in our [OpenFunctions release blog](https://gorilla.cs.berkeley.edu/blogs/4_open_functions.html)

## Introduction
Gorilla OpenFunctions extends Large Language Model(LLM) Chat Completion feature to formulate 
executable APIs call given natural language instructions and API context.

## Models Available
|Model | Functionality|
|---|---|
|gorilla-openfunctions-v0 | Given a function, and user intent, returns properly formatted json with the right arguments|
|gorilla-openfunctions-v1 | + Parallel functions, and can choose between functions|

All of our models are hosted on our Huggingface UC Berkeley gorilla-llm org: [gorilla-openfunctions-v0](https://huggingface.co/gorilla-llm/gorilla-openfunctions-v0) and [gorilla-openfunctions-v1](https://huggingface.co/gorilla-llm/gorilla-openfunctions-v1).

## Example Usage

1. OpenFunctions is compatible with OpenAI Functions

```bash
!pip install openai==0.28.1
```

2. Point to Gorilla hosted servers

```python
import openai

def get_gorilla_response(prompt="Call me an Uber ride type \"Plus\" in Berkeley at zipcode 94704 in 10 minutes", model="gorilla-openfunctions-v0", functions=[]):
  openai.api_key = "EMPTY"
  openai.api_base = "http://luigi.millennium.berkeley.edu:8000/v1"
  try:
    completion = openai.ChatCompletion.create(
      model="gorilla-openfunctions-v1",
      temperature=0.0,
      messages=[{"role": "user", "content": prompt}],
      functions=functions,
    )
    return completion.choices[0].message.content
  except Exception as e:
    print(e, model, prompt)
```

3. Pass the user argument and set of functions, Gorilla OpenFunctions returns a fully formatted json

```python
query = "Call me an Uber ride type \"Plus\" in Berkeley at zipcode 94704 in 10 minutes"
functions = [
    {
        "name": "Uber Carpool",
        "api_name": "uber.ride",
        "description": "Find suitable ride for customers given the location, type of ride, and the amount of time the customer is willing to wait as parameters",
        "parameters":  [{"name": "loc", "description": "location of the starting place of the uber ride"}, {"name":"type", "enum": ["plus", "comfort", "black"], "description": "types of uber ride user is ordering"}, {"name": "time", "description": "the amount of time in minutes the customer is willing to wait"}]
    }
]
get_gorilla_response(query, functions=functions)
```

4. Expected output

```bash
uber.ride(loc="berkeley", type="plus", time=10)
```

## Evaluation

<img src="https://github.com/ShishirPatil/gorilla/blob/gh-pages/assets/img/blog_post_4_OpenFunctions_Distribution.png" width=50% height=50%>



## Contributing

All the models, and data used to train the models is released under Apache 2.0.
Gorilla is an open source effort from UC Berkeley and we welcome contributors. 
Please email us your comments, criticism, and questions. More information about the project can be found at [https://gorilla.cs.berkeley.edu/](https://gorilla.cs.berkeley.edu/)

