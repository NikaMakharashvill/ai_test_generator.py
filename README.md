import openai
import os

# --- CONFIG ---
openai.api_key = os.getenv("OPENAI_API_KEY")  # Make sure to set your OpenAI API key

# --- SAMPLE CODE TO TEST ---
code_to_test = """
def add(a, b):
    return a + b

def divide(a, b):
    return a / b
"""

# --- FUNCTION TO GENERATE TESTS ---
def generate_tests(code: str, language="Python") -> str:
    """
    Uses OpenAI API to generate pytest tests for the given code.
    """
    prompt = f"""
You are an expert software engineer.
Generate complete {language} unit tests for the following code using pytest.
Include edge cases, error handling, and comments explaining each test.

Code:
{code}
"""
    response = openai.ChatCompletion.create(
        model="gpt-4",
        messages=[{"role": "user", "content": prompt}],
        temperature=0.2
    )
    test_code = response.choices[0].message.content.strip()
    return test_code

# --- FUNCTION TO SAVE TESTS ---
def save_tests(test_code: str, filename="test_generated.py"):
    with open(filename, "w") as f:
        f.write(test_code)
    print(f"Tests saved to {filename}")

# --- MAIN EXECUTION ---
if __name__ == "__main__":
    print("Generating tests...")
    tests = generate_tests(code_to_test)
    save_tests(tests)
    print("Done! Run `pytest test_generated.py` to execute the tests.")
