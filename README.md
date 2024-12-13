## Integration of a Mathematical Calulations with a Chat Completion System using LLM Function-Calling

### AIM:
To design and implement a Python function for calculating the volume of a cylinder, integrate it with a chat completion system utilizing the function-calling feature of a large language model (LLM).

### PROBLEM STATEMENT:

Design and implement a system where:

1. Users interact with a chat interface to calculate the volume of a cylinder.
2. User inputs such as the radius and height of the cylinder are extracted via the LLM.
3. The extracted inputs are passed to a Python function that calculates the volume.
4. The LLM integrates this function and provides the result to the user.

### DESIGN STEPS:

#### STEP 1: Define the Python Function
Create a function calculate_cylinder_volume to compute the volume of a cylinder using the formula:
Volume = 𝜋 × 𝑟2 × ℎ
where, r is the radius and ℎ is the height.

#### STEP 2: LLM Integration
Integrate the function with the LLM using a function-calling interface, allowing the LLM to invoke the Python function based on user queries.

#### STEP 3: User Query Parsing
Use the LLM's ability to interpret natural language queries to extract values for radius and height.

#### STEP 4: Function Invocation
Pass the extracted values to the Python function, compute the result, and return it to the LLM.

#### STEP 5: Result Formatting
Present the result in a user-friendly format and provide additional guidance if necessary.

### PROGRAM:

```python
import math
import re
from langchain.chains import LLMChain
from langchain.prompts import PromptTemplate
from langchain_google_genai import ChatGoogleGenerativeAI
import os
from dotenv import load_dotenv

# Load environment variables from .env file
load_dotenv()

# Get the Google API key from the environment variable
api_key = os.getenv("GOOGLE_API_KEY") 

# Define the mathematical function
def calculate_cylinder_volume(radius, height):
    """Calculate the volume of a cylinder."""
    return math.pi * radius ** 2 * height

# Define the LLM prompt template
prompt_template = """
You are a helpful assistant capable of performing mathematical calculations. 
When the user asks for the volume of a cylinder, ensure you understand the input values (radius and height) and use the function `calculate_cylinder_volume` to compute the result.

User Question: {user_question}
Function Name: calculate_cylinder_volume
"""

# Initialize the conversational chain using LLMChain
def get_conversational_chain():
    """Set up the conversational chain for processing user inputs."""
    model = ChatGoogleGenerativeAI(model="gemini-pro", temperature=0.3, google_api_key=api_key)
    prompt = PromptTemplate(template=prompt_template, input_variables=["user_question"])
    chain = LLMChain(prompt=prompt, llm=model)
    return chain

# Process user input and generate a response
def process_user_question(user_question):
    """Generate a response for the user question."""
    try:
        # Use regular expressions to extract numbers from the question
        numbers = [float(num) for num in re.findall(r'\d+\.\d+|\d+', user_question)]
        
        if len(numbers) < 2:
            return "Please provide both radius and height for the calculation."
        
        if len(numbers) > 2:
            return "Invalid input. Please provide exactly two values for radius and height."
        
        radius, height = numbers[:2]  # Extract the first two numbers
        volume = calculate_cylinder_volume(radius, height)
        return f"The volume of the cylinder with radius {radius} and height {height} is {volume:.2f}."
    except Exception as e:
        return f"An error occurred: {e}"

# Main function to simulate interaction
def main():
    # Sample user questions with missing or invalid inputs
    user_questions = [
        "What is the volume of a cylinder with radius 10 and height 20?",  # Valid input
        "What is the volume of a cylinder with height 20?",  # Missing radius
        "What is the volume of a cylinder with radius ten and height 20?",  # Invalid radius format
        "Please calculate the cylinder volume.",  # Missing both radius and height
    ]
    
    # Process each question and generate responses
    for user_question in user_questions:
        print(f"User: {user_question}")
        response = process_user_question(user_question)
        print(f"Assistant: {response}")
        print("-" * 40)

# Run the program
if __name__ == "__main__":
    main()


```

### OUTPUT:

![image](https://github.com/user-attachments/assets/32b987bf-4bcb-445e-9c82-bae9b364ea8f)


### RESULT:

Thus, a Python function for calculating the volume of a cylinder integrated with a chat completion system utilizing the function-calling feature of a large language model (LLM) is implemented successfully.
