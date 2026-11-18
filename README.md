QueryPilot
Ask questions about your data in plain English. Get SQL queries and results.

QueryPilot is a natural-language database querying project built around an agentic Text-to-SQL workflow. Instead of relying on a fixed schema-to-query pipeline, the agent can inspect a connected database, identify relevant tables and columns, generate SQL, execute it, and refine its query using database feedback.
Overview
Working with relational databases often requires knowledge of SQL, table structures, joins, and database-specific syntax. QueryPilot provides a conversational interface to this process: users ask questions in natural language, while an LLM-powered agent explores the schema and works toward an executable query.
Key capabilities
- Natural-language querying — ask questions about database contents in everyday language.
- Autonomous schema exploration — inspect tables and columns to identify relevant data.
- SQL generation and execution — generate and run queries against a connected database.
- Iterative query correction — use execution feedback and database errors to revise SQL.
- Multiple database dialects — connect through SQLAlchemy-supported database drivers.
- Execution tracing — record agent activity for review and analysis.
- Scenario-based guidance — provide domain-specific business rules and query hints through a Markdown scenarios file.
- MCP integration — use the included MCP tooling for trace analysis and scenario improvement.
- Optional LangChain integration — connect the Text-to-SQL workflow to LangChain agents.
How it works
1. The user submits a question in natural language.
2. The agent explores the connected database schema.
3. It identifies relevant tables and columns.
4. It generates and executes a SQL query.
5. It inspects the result or any database error.
6. It can refine and re-execute the query before returning the SQL and resulting data.
Technology stack
- Python 3.10+
- SQLAlchemy
- Pydantic
- OpenAI API or Anthropic API
- SQLite and other SQLAlchemy-supported databases
- Optional LangChain integration
- MCP tooling
Getting started
Prerequisites
- Python 3.10 or newer
- An API key for a supported LLM provider
- A database connection (the bundled demo creates a sample SQLite database)
1. Clone the repository
git clone https://github.com/JAYANTSHARMA3559/querypilot.git
cd querypilot
2. Create a virtual environment
Windows PowerShell
python -m venv venv
.\venv\Scripts\Activate.ps1
Windows Command Prompt
python -m venv venv
venv\Scripts\activate
3. Install dependencies
For OpenAI:
python -m pip install --upgrade pip
pip install -e ".[openai]"
For Anthropic:
pip install -e ".[anthropic]"
4. Configure your API key
PowerShell
$env:OPENAI_API_KEY="your-api-key"
Command Prompt
set OPENAI_API_KEY=your-api-key
Keep API keys private. Never commit API keys, credentials, or secret .env files to the repository.
5. Run the example
python examples/demo_with_dummy_db.py
The bundled demo uses a sample SQLite database. A valid model API key is needed for the agent to answer questions.
Usage
The Python interface exposes the TextSQL class:
from text2sql import TextSQL

engine = TextSQL(
    "sqlite:///company.db",
    model="openai:gpt-4o",
    trace_file="traces.jsonl",
)

result = engine.ask("Which customers have spent more than $10K this year?")

print(result.sql)
print(result.data)
To connect to PostgreSQL, use a SQLAlchemy-compatible connection string:
from text2sql import TextSQL

engine = TextSQL(
    "postgresql://username:password@localhost/database_name",
    model="openai:gpt-4o",
)

result = engine.ask("What are the top 5 products by revenue?")
print(result.sql)
print(result.data)
Install the relevant database driver for your chosen database. Avoid hard-coding database credentials in source files.
Database connectivity
The framework uses SQLAlchemy database URLs. The upstream implementation documents connection patterns for:
- SQLite
- PostgreSQL
- MySQL
- Microsoft SQL Server
- Snowflake
Database support depends on the relevant SQLAlchemy dialect and driver being installed and configured.
Scenario-based context
A scenarios.md file can provide business context that cannot be inferred from database schemas alone, such as:
- Business definitions and metric calculations
- Column-name translations
- Preferred join paths
- Domain-specific query guidance
- Corrections for recurring query mistakes
The agent can retrieve relevant scenario guidance during its query workflow.
Tracing and MCP
Query execution traces can capture the agent's schema exploration, SQL attempts, and execution feedback. The included MCP integration provides tools to inspect traces and support improvements to scenario guidance.

Refer to the mcp/ directory and the project documentation for configuration details.
Repository structure
querypilot/
├── docs/
├── examples/
├── mcp/
├── tests/
├── text2sql/
├── LICENSE
├── README.md
└── pyproject.toml
Configuration notes
- The package currently uses the text2sql Python import.
- The command-line entry point is text2sql.
- The installed distribution name is text2sql-framework.
- Optional provider and integration dependencies are defined in pyproject.toml.

QueryPilot — natural-language access to relational data through an agentic SQL workflow.
