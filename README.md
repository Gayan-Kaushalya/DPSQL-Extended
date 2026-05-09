# Differential Privacy over SQL (DPSQL)

DPSQL is a system designed for answering SQL queries while satisfying differential privacy guarantees.

## About The Project

The file and directory structure of the project is organized as follows:

```text
project
├── config/        # Configuration files required for the system
├── docs/          # Reference information and documentation
├── Profile/       # Profile information/licenses (e.g., mosek.lic)
├── src/           # Main source code files
│   └── algorithm/ # Core algorithms integrated into the system (e.g., FastSJA, OptSJA)
├── Test/          # Queries used in system experiments (TPCH, Graph)
└── Sample/        # Scripts for database setup and collecting experiment results
```

## Prerequisites

### Tools
* **[PostgreSQL](https://www.postgresql.org/)**: Database engine.
* **[Python3](https://www.python.org/download/releases/3.0/)**: Ensure version 3.0 or higher.
* **[Mosek](https://www.mosek.com/downloads/)**: License file must be placed in `./Profile`.
* **CPLEX (Full Edition)**: Required for large datasets. Note: Do not rely on `pip install cplex` alone, as it has a 1,000-variable limit.
  * [Detailed CPLEX Installation & Python Linking Guide](docs/cplex_setup.md)

### Python Dependencies

Install the required Python packages using the provided `requirements.txt` file:

```bash
pip install -r requirements.txt
```

### Database Permissions
The user running the system must have read permissions for the target database schema.

## Usage / Demo System

The main entry point for the system is `main.py`.

### Command-Line Arguments
| Parameter | Description |
| :--- | :--- |
| `--d` | Path to the database initialization file |
| `--q` | Path to the query file |
| `--r` | Path to the private relation file |
| `--c` | Path to the configuration file |
| `--o` | Path to the output file |
| `--debug` | Enable debug mode for more detailed logging |
| `--optimal` | Use the optimal algorithm for SJA queries |

*Use `python main.py --h` to view complete help instructions.*

**Documentation Links:**
*   [Input File Configuration](./docs/system-input.md)
*   [Supported SQL Syntax](./docs/query-syntax.md)

**Example Run:**
```bash
python main.py --d ./config/database.ini --q ./test.txt --r ./test_relation.txt --c ./config/parameter.config --o out.txt
```

## Collecting Results

Follow these steps to set up the data and collect experiment results:

1. **Install Dependencies**: Ensure tools and Python requirements are installed as per the [Prerequisites](#prerequisites).
2. **Database Setup**: Create an empty database in PostgreSQL.
3. **Data Generation**: Generate `.tbl` data files using `dbgen` from the [TPC-H website](https://www.tpc.org/tpc_documents_current_versions/current_specifications5.asp), and store them in `./Sample/data/TPCH`.
4. **Database Initialization**: Run the setup script provided in `./Sample/setupDBTPCH.py`:
   ```bash
   python Sample/setupDBTPCH.py --db <databasename>
   ```
5. **Run Collection Script**:
   ```bash
   cd Sample
   python collectResult.py
   ```
6. **View Results**: The output will be available in `./Sample/result/TPCH`.

## Query Rewriting & Subquery Unnesting

DPSQL automatically rewrites and unnests subqueries to standard relational joins to ensure differential privacy mechanisms can be seamlessly applied. Through a custom Abstract Syntax Tree (AST) visitor (`UnnestSubqueries` in `src/parser.py`) built using `pglast`, the system traverses the AST and flattens nested `IN`, `ANY`, and `EXISTS` subqueries found in the `WHERE` clause into standard multi-table joins, while automatically preserving and linking the original filtering conditions.

## Future Plans

* Support for distinct count queries (projection).
* Develop a User Interface (UI).
* Improve overall user experience.
* General performance optimization.
