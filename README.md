# SemanticForms

A library for collecting structured information from users via an LLM-based chat interface. 

This project is in the making. Please open an issue if you're interested in assisting.

# Example

Let's say you're building a chatbot for a car repair shop. You might want to know what is the make, model and year of the vehicle, and whether the customer had a malfunction or an accedient. The chatbot is then expected to ask different questions depending on the answers:
1. If it's a malfunction, is the car driving? If it's driving, is it making noises? Are there any dashboard warning lights on? 
2. If it's an accident, was the front, back or side of the car affected? Is the car driving or does in need to be towed in?

The form can be arbitrarely deep and rules can address different combinations of fields. At the end, you want to make sure that all fields were field according to the rules, regardless of how the customer chose to answer them (in which order, language, did they use a picture, did they answer the questions one by one or shared a long story or a document, and so on).

## Code example

```python
# TBD
```

# How it works

The main class is the `SemanticFormsEngine` which:

1. Is initialized with a form and rules.
2. Keeps a state of the answers.
3. Provides an iterator based interface for generating the next questions for unfilled fields, and errors if the rules are contradicted (your LLM can reformulate these to your style)
4. Provides an parser to extract field values and update the state from the user responses.
5. Exposes `is_minimally_complete()` to tell whether the mandatory part of the fields are filled.
6. Exposes `is_maximally_complete()` to tell when all fields (mandatory and optional) were filled.

Internally, each form is parsed into a Pydantic schema and an internal state object.
The questions iterator takes the internal state object, schema and rules and outputs a prompt to the user. The prompt focuses on the next layer in the subtree which the user is filling. The prompt can be rewritten by adding additional instructions. 
Response parser feeds the previous internal state and the user response to an LLM, extracting the update to the internal state.
Completeness validation feeds the internal state, the schema and the rules to determine if the form is complete. 

# Usage

## General flow 

1. Define forms and rules (in one of several ways).
2. Pass the forms and rules to initialize SemanticFormsEngine.
3. Iteratively query the engine to get the next questions / errors for the user.
   1. Pass the user's response to the `SemanticFormsEngine.parse_user_response()`.
   2. Check `is_minimally_complete()` or `is_maximally_complete()` and if set break the loop
4. Get the resulting object using `get_filled_form()`







