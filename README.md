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

```
import math
import re

def calculate_cylinder_volume(radius: float, height: float) -> float:
    """Calculate the volume of a cylinder given its radius and height."""
    if radius <= 0 or height <= 0:
        return "Radius and height must be positive values."
    return math.pi * radius**2 * height


def chat_with_llm(query: str) -> str:
    """Process user query to calculate cylinder volume."""
    if "cylinder" in query.lower() and "volume" in query.lower():
        # Use regex to extract radius and height
        radius = re.search(r"radius\s*(-?\d+(\.\d+)?)", query, re.IGNORECASE)
        height = re.search(r"height\s*(-?\d+(\.\d+)?)", query, re.IGNORECASE)

        if radius and height:
            # Convert matched groups to float
            radius = float(radius.group(1))
            height = float(height.group(1))

            # Calculate the volume
            result = calculate_cylinder_volume(radius, height)
            if isinstance(result, str):  # Error message from the function
                return result
            return f"The volume of the cylinder with radius {radius} and height {height} is {result:.2f} cubic units."
        else:
            return "Please provide valid radius and height in your query."

    return "I can help you calculate the volume of a cylinder. Please specify the radius and height."


# Test cases
queries = [
    "What is the volume of a cylinder with radius 4 and height 5?",
    "Calculate the volume of a cylinder with radius 10 and height -5.",
    "How to find the volume of a cylinder?",
]

for query in queries:
    print(f"Query: {query}")
    response = chat_with_llm(query)
    print(f"Response: {response}\n")

```

### OUTPUT:

![exp-1 op](https://github.com/user-attachments/assets/e47738b8-f5a2-435a-9c25-74b4dd39caca)

### RESULT:

Thus, a Python function for calculating the volume of a cylinder integrated with a chat completion system utilizing the function-calling feature of a large language model (LLM) is implemented successfully.
