## Integration of a Mathematical Calulations with a Chat Completion System using LLM Function-Calling

### AIM:
To design and implement a Python function for calculating the volume of a cylinder, integrate it with a chat completion system utilizing the function-calling feature of a large language model (LLM).

### PROBLEM STATEMENT:

### DESIGN STEPS:

#### STEP 1:
Import the required Python libraries and configure the OpenAI API key to establish a connection with the chat completion system.

#### STEP 2: 
Define a Python function to calculate the volume of a cylinder using the formula:
\[
V = \pi r^2 h
\]

where \(r\) is the radius and \(h\) is the height of the cylinder.

#### STEP 3:
Define the function parameters and pass them to the LLM using function calling. Extract the radius and height from the LLM response, execute the Python function, and display the calculated volume.

### PROGRAM:
~~~
import os
import json
import openai
from dotenv import load_dotenv, find_dotenv

_ = load_dotenv(find_dotenv())
openai.api_key = os.environ["OPENAI_API_KEY"]

def calculate_cylinder_volume(radius, height):
    volume = 3.14159 * radius * radius * height
    return json.dumps({"volume": volume})

functions = [
    {
        "name": "calculate_cylinder_volume",
        "description": "Calculate the volume of a cylinder",
        "parameters": {
            "type": "object",
            "properties": {
                "radius": {
                    "type": "number",
                    "description": "Radius of the cylinder"
                },
                "height": {
                    "type": "number",
                    "description": "Height of the cylinder"
                }
            },
            "required": ["radius", "height"]
        }
    }
]

messages = [
    {
        "role": "user",
        "content": "Calculate the volume of a cylinder with radius 5 and height 10."
    }
]

response = openai.ChatCompletion.create(
    model="gpt-4o-mini",
    messages=messages,
    functions=functions
)

response_message = response["choices"][0]["message"]
args = json.loads(response_message["function_call"]["arguments"])

result = calculate_cylinder_volume(**args)

print("Volume of cylinder:", json.loads(result)["volume"])
~~~

### OUTPUT:

<img width="1917" height="1078" alt="image" src="https://github.com/user-attachments/assets/3dbce1fe-7e31-405f-a55b-026565e8d42d" />



### RESULT:
Thus, the Python function for calculating the volume of a cylinder was designed and integrated with the LLM chat completion system using function calling to obtain the required result.
