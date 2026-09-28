from dotenv import load_dotenv
from google import genai

load_dotenv()

client = genai.Client()


orders = {
    101: "Shipped",
    102: "Delivered",
    103: "Processing"
}


customer = {
    1001: "Manohar",
    1002: "Uday",
    1003: "Milky"
}


def get_order_details(order_id: int) -> str:
    """Gets the status of a customer's order."""

    status = orders[order_id]

    return f"Checking status: {status}"


def get_customer_name(customer_id: int) -> str:
    """Gets the customer's name."""

    value = customer[customer_id]

    return f"Customer name: {value}"


# Functions must be defined BEFORE adding them to tools
tools = [
    get_order_details,
    get_customer_name
]


chat = client.chats.create(
    model="gemini-3.8-flash",
    config={
        "tools": tools
    }
)


message = input("Customer: ")

response = chat.send_message(message)

print(response.text)
