# UiPath Connector Tests

This directory contains tests for the UiPath Orchestrator Queue Items connector.

## Test Structure

### Unit Tests (`unit/`)

Automated tests using the pytest framework. These tests use the standard Lakeflow test suite and can be run automatically in CI/CD pipelines.

- **`test_uipath_lakeflow_connect.py`** - Complete connector test suite
  - Tests authentication
  - Tests table listing
  - Tests schema retrieval
  - Tests metadata retrieval
  - Tests data reading

**Run unit tests:**
```bash
# From project root
pytest sources/uipath/test/unit/

# Or run specific test
pytest sources/uipath/test/unit/test_uipath_lakeflow_connect.py
```

### Integration Tests (`integration/`)

Manual integration test scripts for validating the connector against a real UiPath Orchestrator instance. These require actual credentials and are useful for debugging and validation during development.

- **`test_connection.py`** - Comprehensive connection and data retrieval test
  - Loads configuration from `configs/dev_config.json`
  - Tests OAuth authentication
  - Lists available tables and schemas
  - Queries available queues
  - Exports and displays queue items
  - Shows detailed output for each queue item

- **`test_direct.py`** - Quick direct test script
  - Direct API calls with minimal abstraction
  - Saves CSV export for inspection
  - Useful for debugging export/parsing issues

**Run integration tests:**
```bash
# From project root
python3 sources/uipath/test/integration/test_connection.py
python3 sources/uipath/test/integration/test_direct.py

# Or from the test directory
cd sources/uipath/test/integration
python3 test_connection.py
python3 test_direct.py
```

## Configuration

Before running tests, update the configuration files:

- `sources/uipath/configs/dev_config.json` - Connection credentials
- `sources/uipath/configs/dev_table_config.json` - Table-specific options (folder_id, queue_definition_id)

**Never commit real credentials to version control!**

## Test Types Comparison

| Aspect | Unit Tests | Integration Tests |
|--------|-----------|-------------------|
| **Purpose** | Automated testing | Manual validation |
| **Credentials** | Mocked/test data | Real credentials required |
| **Framework** | pytest | Standalone Python scripts |
| **CI/CD** | ✅ Yes | ❌ No (manual only) |
| **Output** | Pass/Fail | Detailed console output |
| **Use Case** | Pre-commit checks, CI | Development, debugging |

## Best Practices

1. **Unit tests** should always pass before committing code
2. **Integration tests** are for manual verification during development
3. Keep credentials in local config files, not in code
4. Use `.gitignore` to prevent accidental credential commits
5. Run unit tests frequently, integration tests as needed
