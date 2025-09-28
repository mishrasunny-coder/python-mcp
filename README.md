# E-commerce Database MCP Server

A Model Context Protocol (MCP) server that provides AI assistants with tools to query an e-commerce database. This server exposes customer, order, and product information through a standardized interface that can be used by AI applications like Cursor or Claude.

## 🏗️ Architecture

This MCP server consists of three main components:

- **`main.py`**: The MCP server implementation using FastMCP
- **`transactional_db.py`**: In-memory database with sample e-commerce data
- **`tests/test_server.py`**: Unit tests to verify server functionality

## 📊 Database Schema

The server provides access to three data tables:

### Customers Table
- **CUST123**: Alice Johnson (alice@example.com, 555-1234)
- **CUST456**: Bob Smith (bob@example.com, 555-5678)

### Orders Table
- **ORD1001**: Alice's order - $89.99 (Wireless Mouse + Keyboard) - Shipped
- **ORD1015**: Alice's order - $45.50 (USB-C Cable) - Processing
- **ORD1022**: Bob's order - $120.00 (2x Wireless Mouse) - Delivered

### Products Table
- **SKU100**: Wireless Mouse ($29.99, Stock: 42)
- **SKU200**: Keyboard ($59.99, Stock: 18)
- **SKU300**: USB-C Cable ($15.50, Stock: 77)

## 🛠️ Available Tools

The MCP server exposes 5 tools that AI assistants can use:

| Tool | Description | Parameters | Returns |
|------|-------------|------------|---------|
| `get_customer_info` | Get customer details by ID | `customer_id: str` | Customer information |
| `get_order_details` | Get detailed order information | `order_id: str` | Order details with items |
| `check_inventory` | Search products by name | `product_name: str` | Matching products with stock |
| `get_customer_ids_by_name` | Find customer IDs by full name | `customer_name: str` | List of customer IDs |
| `get_orders_by_customer_id` | Get all orders for a customer | `customer_id: str` | Dictionary of orders |

## 🚀 Setup Instructions

### Prerequisites

- Python 3.8 or higher
- Cursor IDE (or another MCP-compatible AI assistant)

### 1. Clone and Setup

```bash
# Clone the repository
git clone <repository-url>
cd python-mcp

# Create virtual environment
python3 -m venv venv

# Activate virtual environment
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt
```

### 2. Test the Server

```bash
# Run the test suite
pytest tests/test_server.py -v

# Expected output:
# ✓ test_mcp_server_connection - Verifies all 5 tools are available
```

### 3. Configure Cursor IDE

Add the server to your Cursor MCP configuration file (`~/.cursor/mcp.json`):

```json
{
  "mcpServers": {
    "ecommerce-db-server": {
      "command": "/path/to/your/project/venv/bin/python",
      "args": ["/path/to/your/project/main.py"],
      "description": "A set of tools to query customer, order, and product information in a transactional database"
    }
  }
}
```

**⚠️ Important**: Replace `/path/to/your/project/` with the actual absolute path to your project directory.

### 4. Verify Setup

1. Restart Cursor IDE
2. Look for the "ecommerce-db-server" in your MCP servers list
3. The server indicator should show green (connected)
4. Try asking: "What customer IDs match the name Alice Johnson?"

## 💡 Usage Examples

Once configured, you can ask your AI assistant natural language questions:

### Customer Queries
- "What customer IDs match the name Alice Johnson?"
- "Tell me about customer CUST123"
- "Get me information for customers named Bob Smith"

### Order Queries  
- "What has Alice ordered?"
- "Show me details for order ORD1001"
- "What orders has customer CUST456 placed?"

### Product Queries
- "Check inventory for wireless mouse"
- "What products do we have in stock?"
- "Search for keyboard products"

## 🧪 Development

### Project Structure

```
python-mcp/
├── main.py              # MCP server implementation
├── transactional_db.py  # Sample database
├── tests/
│   └── test_server.py   # Unit tests
├── requirements.txt     # Python dependencies
├── .gitignore          # Git ignore rules
└── README.md           # This file
```

### Running the Server Standalone

```bash
# Activate virtual environment
source venv/bin/activate

# Run the server directly
python main.py

# The server will start and wait for MCP protocol messages via stdio
```

### Adding New Tools

To add a new tool to the server:

1. Define the function in `main.py`
2. Add the `@mcp.tool()` decorator
3. Include type hints and docstring
4. Update the test expectations in `test_server.py`

Example:
```python
@mcp.tool()
async def new_tool_name(param: str) -> str:
    """Description of what the tool does"""
    # Your implementation here
    return result
```

## 🔧 Troubleshooting

### Server Not Connecting
- Verify the Python path in `mcp.json` points to your virtual environment
- Check that the script path in `mcp.json` is correct
- Ensure all dependencies are installed: `pip install -r requirements.txt`

### Tool Execution Errors
- Check the server logs in Cursor's MCP panel
- Verify your database queries against the sample data
- Run the test suite to ensure basic functionality works

### Performance Issues
- Each tool includes a 1-second delay for demonstration
- Remove `await asyncio.sleep(1)` lines for production use
- Consider implementing proper database connections for larger datasets

## 📝 Dependencies

Key dependencies included in `requirements.txt`:

- **mcp**: Model Context Protocol implementation
- **pydantic**: Data validation and serialization
- **pytest**: Testing framework
- **pytest-asyncio**: Async testing support

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Add tests for new functionality
4. Ensure all tests pass: `pytest`
5. Submit a pull request

## 📄 License

This project is licensed under the MIT License.

## 🆘 Support

For issues and questions:
1. Check the troubleshooting section above
2. Review the MCP documentation
3. Open an issue in the repository
